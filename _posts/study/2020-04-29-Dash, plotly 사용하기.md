---
title: Dash plotly 설치부터 튜토리얼까지
categories: study
modified: 
tags: [python, big data]
ads: true
excerpt:
image:
  feature:
  teaser: teaser/bigdata.jpg
  thumb:
sitemap :
  changefreq : daily
  priority : 1.0
toc: true
toc_label: "Table of Contents"
toc_icon: "cog" 
---

## 데이터 시각화 및 분석 라이브러리

Python 관련 커뮤니티를 살펴보던 중 기가막힌 예제를 보게 되었다. 

<figure>
  <a href="https://www.reddit.com/r/Python/comments/g2njfz/hexapod_robot_simulator_with_only_numpy_and/?utm_source=share&utm_medium=web2x"><img src="/assets/images/raddit_plotly_dash.png"></a>
  <figcaption>dash와 plotly를 이용하여 만든 시뮬레이터</figcaption>
</figure>


Numpy, **dash**와 **plotly**라는 라이브러리만을 이용하여 구현하였다고 설명이 쓰여 있었다. 짧은 GIF에서 확인되는 인터랙티브한 인터페이스가 인상적이었다. 이 프로그램을 구현하기 위해 사용된 라이브러리가 궁금해져서 찾아보았고, 짧은 튜토리얼까지 진행해보았다.

튜토리얼만 진행해보았는데 강력한 인터랙티브 함수와 예쁜 그래프를 통해 **데이터를 시각화하고 분석**하기에 좋은 라이브러리라는 것을 알게되었다.
또한 대세인 Web 기반의 시각화 라이브러리여서 앞으로 더욱 많이 이용되지 않을까 싶다.

>강력한 인터랙티브 함수 + 예쁜 그래프 + Web -> 데이터 시각화, 분석

## 설치부터 튜토리얼까지
* OS: Linux Ubuntu 18.04
* Python3

### 설치

> Reference: [https://dash.plotly.com/installation](https://dash.plotly.com/installation)

설치 방법
  ```bash
  pip3 install dash
  pip3 install ipywidgets
  pip3 install pandas
  ```

설치 확인
  ```python
  import dash_core_components
  print(dash_core_components.__version__)
  ## >> 1.9.0
  ```


### 튜토리얼
진행한 튜토리얼은 plotly에 있는 튜토리얼을 기준으로 진행했고 내용은 Layout, Callback 부분을 진행하였다.
실행 코드는 [Github](https://github.com/Kyungpyo-Kim/PlotlyDashExercise.git)에 업로드 하였다. 

```bash
# 튜토리얼 소스코드
git clone https://github.com/Kyungpyo-Kim/PlotlyDashExercise.git
cd PlotlyDashExercise
```

#### Layout
우선 소스코드를 돌려보고 결과를 먼저 본다. 소스코드를 실행하고 정상적으로 실행이 되면 **results**와 같이 결과가 나오게 된다. 그러면 브라우저를 열어 "http://127.0.0.1:XXXX/" 주소로 들어가보면 기가막힌 그래프들이 나타난다!

```bash
python3 app_layout.py

# results
Running on http://127.0.0.1:XXXX/
Debugger PIN: XXXXXXXXX
 * Serving Flask app "app_layout" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: on
Running on http://127.0.0.1:XXXX/
Debugger PIN: XXXXXXXXX
```

<figure>
  <img src="/assets/images/results_layout.png">
  <figcaption>layout tutorial 실행 결과</figcaption>
</figure>

코드를 하나하나 보기보다는 feature 위주로 코드를 살펴보기로 한다.

* import
  ```python
  import dash
  import dash_core_components as dcc
  import dash_html_components as html
  ```
  import 하는 라이브러리를 살펴보면, 
    - 어플리케이션 레이어인 dash
    - 그래프 등 plotly 의 다양한 인터랙티브 그래프를 지원하는 dash core components
    - html 을 지원하는 dash html components
  를 사용하게 된다.

* html components
  웹기반 라이브러리이다보니 당연히 html 을 지원한다.
  ```python
  """
  HTML
  """
  ## HTML H1
  header = html.H1(children='Hello Dash', 
    style={'textAlign': 'center','color': colors['text']})
  ## HTML Div
  div_description = html.Div(children='''Dash: A web application framework for Python.''', 
    style={'textAlign': 'center','color': colors['text']})
  ## HTML Table
  def generate_table(dataframe, max_rows=10):
      return html.Table([
          html.Thead(
              html.Tr([html.Th(col) for col in dataframe.columns])
          ),
          html.Tbody([
              html.Tr([
                  html.Td(dataframe.iloc[i][col]) for col in dataframe.columns
              ]) for i in range(min(len(dataframe), max_rows))
          ])
      ])
  ```
  H1, div 등의 태그 외에도 html 의 문법을 다양하게 지원한다.

* plotly components
  ```python
  """ 
  Dash Core Components
  """
  ## Dash Graph
  dash_graph_data = dcc.Graph(
          id='example-graph',
          figure={
              'data': [
                  {'x': [1, 2, 3, 4], 
                    'y': [4, 1, 2, 6], 
                    'type': 'bar', 
                    'name': 'SF'},
                  {'x': [1, 2, 3, 4], 
                    'y': [2, 4, 5, 9], 
                    'type': 'bar', 
                    'name': u'Montréal'},
              ],
              'layout': {
                  'title': 'Dash Data Visualization',
                  'plot_bgcolor': colors['background'],
                  'paper_bgcolor': colors['background'],
                  'font': {
                      'color': colors['text']
                  }
              }
          }
      )
  
  # ...생략...

  ## Markdown
  md = dcc.Markdown(children=markdown_text)
  
  ## Dropdown
  dd = dcc.Dropdown(
          options=[
              {'label': 'New York City', 'value': 'NYC'},
              {'label': u'Montréal', 'value': 'MTL'},
              {'label': 'San Francisco', 'value': 'SF'}
          ],
          value='MTL'
      )
  
  # ...생략...

  ## Text Input
  ti = dcc.Input(value='MTL', type='text')
  ## Slider
  sl = dcc.Slider(
          min=0,
          max=9,
          marks={i: 'Label {}'.format(i) if i == 1 else str(i) for i in range(1, 6)},
          value=5,
      )
  ```
  plotly dash 코드가 인상적이었던 부분은 위의 코드에서 처럼 graph 뿐만 아니라 dropdown, radio button 등의 다양한 인터페이스를 쉽게 구현할 수 있다는 것이다.
  그리고 스타일 시트를 활용하면 큰 수고없이 예쁜 그래프를 그릴 수 있게 된다.

* app
  구현한 html, plotly 컴포넌트들은 app에서 쉽게 배치(layout)하여 웹으로 구현이 된다.
  ```python
  """
  Dash App
  """
  ## App
  external_stylesheets = ['https://codepen.io/chriddyp/pen/bWLwgP.css']
  app = dash.Dash(__name__, external_stylesheets=external_stylesheets)

  ## Layout
  app.layout = html.Div(
    style={'backgroundColor': colors['background'], 'columnCount': 1}, 
    children=[
      
      header,

      div_description,

      dash_graph_data,

      html.H4(children='US Agriculture Exports (2011)'),
      generate_table(df_agri),
      html.H4(children='GDP'),
      dash_graph_gdp,

      # ...생략...

  ])

  if __name__ == '__main__':
      app.run_server(debug=True)
  ```

  여기까지만 하면 plotly, dash 를 이용하여 이쁜 그래프를 매우 쉽게 그릴 수 있게 된다. 그것도 웹에다가 말이다. 내가 웹개발 경험이 전무하여 더 신기하게(?) 느껴졌을지도 모르겠다.
  하지만 dash plotly 정말 인상적인 이유는 다음 파트 **Callback**에서 확인할 수 있다.

#### Callback
내가 사용해보았던 거의 모든 GUI의 구현은 대부분 입력에 대한 Callback 형식으로 이루어져있었다. Dash 역시 마찬가 지인데 조금 더 인상적이었던 부분은 callback 처리가 매우 쉽고 직관적이었다는 부분이다. 예제와 코드를 간단히 살펴본다.


```bash
python3 app_callback.py
```

<figure>
  <img src="/assets/images/results_callback.gif">
  <figcaption>callback tutorial 실행 결과</figcaption>
</figure>

* id를 이용한 callback 구현

  Dash에서는 인터랙션 기능 지원을 위해 input, output, state 의 callback 메소드를 지원한다. 그리고 이러한 메소드들은 Dash components 의 id를 이용해 바인딩되어 callback 이 호출되는 아주 간단한 구조로 되어 있다.

  코드를 통해 간단히 확인해보면,

  - id 등록
  ```python
  dcc.Input(id='input-1-state', type='text', value='Montréal'),
  dcc.Input(id='input-2-state', type='text', value='Canada'),
  html.Button(id='submit-button-state', n_clicks=0, children='Submit'),
  html.Div(id='output-state'),
  ```

  위와 같이 input, button, div 컴포넌트들의 id를 지정하고

  - callback 등록
  ```python
  @app.callback(Output('output-state', 'children'),
              [Input('submit-button-state', 'n_clicks')],
              [State('input-1-state', 'value'),
               State('input-2-state', 'value')])
  def update_output(n_clicks, input1, input2):
      return u'''
          The Button has been pressed {} times,
          Input 1 is "{}",
          and Input 2 is "{}"
      '''.format(n_clicks, input1, input2)
  ```
  위와 같이 함수의 데코레이터를 이용하여 Output, Input, State 메소드를 지정한다. 그리고 함수를 통해 해당 액션이나 데이터들을 처리할 수 있게 되어있다. 함수 argument 의 n_clicks, input1, input2 는 각각 input, state 두개의 값이 들어오게 되어있다.
  **너무 쉽다!**

여담으로 개발을 하면서 재사용성을 높이기 위한 고민을 많이 하게 되는데, 구현된 코드가 간단하고 쉬울수록 재사용하기가 좋다는 생각이 많이 들었다. 이런 관점에서 이렇게 쉬운 코드는 앞으로도 많이 사용되지 않을까 싶다.

### 마무리
튜토리얼 내용을 따라해보면 callback 을 바탕으로 확장할 수 있는 다양한 기능들을 보여준다. 눈에 띄는 점은 이러한 간단한 구현에 따른 유연성이었다. 앞으로 여러가지 데이터 분석을 위해서 활용해 보고자 한다. 뒤에 이어지는 튜토리얼은 Web 환경에서의 data sharing 에 따는 여러 어프로치들이 제시되어 있는데 이부분은 web쪽 공부를 같이 진행하면서 보아야 할 것 같다. 앞으로 활용하면서 중요한 기능들이나 특징들은 더 정리해 나가야겠다.

## 요약
* web 기반 데이터 시각화 도구
* 강력한 인터랙티브 기능
  - 구현이 쉽다.
  - 정말 쉽다.
* layout, callback 만으로 쉽게 구현됨