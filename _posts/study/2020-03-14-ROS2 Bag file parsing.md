---
title: ROS2 Bag file parsing
categories: study
modified: 
tags: [ROS]
ads: true
excerpt:
image:
  feature:
  teaser: teaser/400x250.jpg
  thumb:
sitemap :
  changefreq : daily
  priority : 1.0
toc: true
toc_label: "Table of Contents"
toc_icon: "cog" 
---

## ROS2 Bag File Parsing 하기
ROS의 강력한 도구중 하나가 rosbag 패키지였다. ROS1의 rosbag 패키지는 Logging 과 Replay 기능 뿐만 아니라 rqt 플러그인을 통해서 시스템 개발에 필요한 여러가지 디버깅 기능을 제공하였다. 하지만 아직 ROS2에서는 rosbag(rosbag2) 의 record(logging) play, 그리고 info 기능밖에 구현이 안되어 있다. 게다가 bag파일이 sqlite 등 다른 형태로 저장되어 ROS1의 rosbag 기능을 사용할 수 없는 상태이다.

그도 그럴것이 rosbag2의 개발중인 API들이 정리되어 있지 않은 상태여서 로깅된 데이터를 분석하는데 많은 어려움이 있었다. 하지만 C++ 로 구현되어있는 패키지와 개발중인 [Python API](https://github.com/ros2/rosbag2/issues/232#issuecomment-572818291)를 토대로 rosbag2 의 데이터 베이스 파싱 방법을 찾아냈고, 이러한 과정에서 rosbag2 의 몇가지 개념들을 함께 이해하게 되었다.

## Development environment

* OS: Ubuntu18.04

* ROS:
  1. Install ros1 melodic
  2. Install ros2 dashing
  3. Install rosbag2 packages
    ```bash
    sudo apt install ros-dashing-rosbag2* ros-dashing-ros2bag*
    ```


## Getting Started
```bash
git clone https://github.com/Kyungpyo-Kim/ROS2BagFileParsing.git
cd ROS2BagFileParsing/dev_ws
colcon build
. install/setup.bash
ros2 run dev_cpp_pkg dev_cpp_node
```
```results
[INFO] [rosbag2_storage]: Opened database '/home/kyungpyo/git/ROS2BagFileParsing/rosbag2_test_data'.
meta name: /rosout
meta type: rcl_interfaces/msg/Log
meta serialization_format: cdr
meta name: /parameter_events
meta type: rcl_interfaces/msg/ParameterEvent
meta serialization_format: cdr
meta name: /r2dpac/drv/sp/gps
meta type: r2dpac_msgs/msg/GpsRx
meta serialization_format: cdr

37.4999
127.043
46.0831
```

## How?

### 1. Python API
ROS2 bag 파일 parsing 방법을 알아 보던 중, 다음의 코드를 발견하였다.

Reference: [Python API](https://github.com/ros2/rosbag2/issues/232#issuecomment-572818291)
```python
from rosidl_runtime_py.utilities import get_message

import rosbag2_py._rosbag2_py as rosbag2_py
from rclpy.serialization import deserialize_message


reader = rosbag2_py.SequentialReader()
reader.open('rosbag2_2020_01_06-14_58_37')
i = 0 
type_map = reader.get_all_topics_and_types()
while reader.has_next():
    print(f'{i}')
    i += 1
    topic, data = reader.read_next()
    msg_type = get_message(type_map[topic])
    msg = deserialize_message(data, msg_type)
    print(msg)
```

현재 개발중이어서 dashing 버전에서는 사용할 수 없지만, 두가지 포인트를 확인할 수 있었다.

1. bag 파일을 열고 읽을 수 있는 **SeqeuntialReader**
2. 토픽 데이터를 parsing 해주는 **deserializae_message**

그리고 위의 함수들은 c/c++로 구현되었고 이를 python으로 wrap-up한 코드라는 것을 알 수 있었다.
따라서 c++ API만 동일하게 사용하면 된다!

### 2. C++ API

먼저 bag 파일을 열고 읽는 API를 찾아 보았다.
먼저 파일이 필요하고, 파일 형태가 필요하고, serialization 포맷 형태가 필요하다.

ROS2 에서는 여러 임베디드 시스템에서 상호호환 가능한 인터페이스 설계에 중점을 두었다. 그래서 이를 위한 통신의 serialziation에 많은 신경을 쓰는 듯 하다. 이를 위해 **rosidl_typesupport**, **rmw(ROS MiddleWare)** 인터페이스와 같은 기능들이 구현되어 있다. 
{: .notice}

1. Open

  ```c++
  rosbag2::SequentialReader reader;
  rosbag2::StorageOptions storage_options{};

  storage_options.uri = "bag_data";
  storage_options.storage_id = "sqlite3";

  rosbag2::ConverterOptions converter_options{};
  converter_options.input_serialization_format = "cdr";
  converter_options.output_serialization_format = "cdr";
  reader.open(storage_options, converter_options);
  ```

  파일을 열기 위해서는 bag data(```bag_data```)와 ros2에서 제공하는 bag data format 정보 ```sqlite3``` 그리고 serialization farmat ```cdr``` 방법이 필요한다.

2. Read

  ```c++
    // read and deserialize "serialized data"
  if (reader.has_next()){

    // serialized data
    auto serialized_message = reader.read_next();
  ```

  **SequentialReader** 라는 클래스 이름과 같이 ```read_next``` 함수를 통해 serialization된 메세지를 얻을 수 있다.
  이 메세지 스트럭쳐에는 토픽, 타입, 그리고 serialization된 내용의 data가 포함되어 있다. 이제 남은 단계는 이 serialization 된 메세지를 **deserialization**하는 것이다.

3. Deserialize

  ```c++
  // deserialization and conversion to ros message
  dev_cpp_pkg::msg::GpsRx msg;
  auto ros_message = std::make_shared<rosbag2_introspection_message_t>();
  ros_message->time_stamp = 0;
  ros_message->message = nullptr;
  ros_message->allocator = rcutils_get_default_allocator();
  ros_message->message = &msg;
  auto type_support = rosbag2::get_typesupport("dev_cpp_pkg/msg/GpsRx", "rosidl_typesupport_cpp");

  rosbag2::SerializationFormatConverterFactory factory;
  std::unique_ptr<rosbag2::converter_interfaces::SerializationFormatDeserializer> cdr_deserializer_;
  cdr_deserializer_ = factory.load_deserializer("cdr");
  
  cdr_deserializer_->deserialize(serialized_message, type_support, ros_message);
  ```

  Deserialization 과정은 약간 까다로웠는데 그 이유가 ROS2에서 인터페이스 되는 데이터들의 serialization/deserialization 에 신경을 쓰다 보니, 이 부분이 custom 가능한데서 신경써야 할 부분이 늘어난 것 같다. 그래도 기본적인 ```cdr``` 형태의 serialization 데이터는 parsing 할 수 있도록 defualt로 제공한다. 위 코드는 ```cdr``` 포맷으로 serialization된 데이터를 deserication하여 원래의 ROS message 로 변환하는 과정이다.
  ```cdr_deserializer_->deserialize(serialized_message, type_support, ros_message);``` 이후에 ```msg```에 저장된 정보를 확인해보면 데이터가 정확히 parsing 된 것을 확인할 수 있다!

## Summary
* ROS2의 데이터를 parsing 하기 위한 방법을 알아보았다.
* Parsing 하는 과정에서 다종의 임베디드 시스템에 rmw 인터페이스가 어떻게 적용되는지 알수 있었다.
* 그리고 아직 ros2 개발이 안된 부분이 많다...