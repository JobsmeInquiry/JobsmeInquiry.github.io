---
layout: post
title: Vue Instance
image: /yjyoo/vue/vue.jpg
date: 2020-12-06 15:01:00 +0900
tags:
categories: yjyoo
---
#### Vue Instance

* Instance 생성자 : 뷰로 화면을 개발할 때 필수적으로 생성해야하는 기본 단위. 뷰로 개발시 필요한 기능들을 생성자에 미리 정의하여 사용자가 재정의/확장하여 사용. new Vue({...})
* 유효 범위 : 뷰 인스턴스를 생성하면 HTML의 특정 범위 안에서만 옵션 속성들이 적용됨.
* 인스턴스가 화면에 적용되는 과정 :<br>
  뷰 라이브러리 파일 로딩 -> 인스턴스 객체 생성(옵션 속성 포함) -> 특정 화면 요소에 인스턴스 붙임<br>
   -> 인스턴스 내용이 화면 요소로 변환 -> 변화된 화면 요소를 사용자가 최종 확인

***
#### Vue LifeCycle
* LifeCycle : 인스턴스의 상태에 따라 호출할 수 있는 속성을 '라이프 사이클 속성' 이라하고, 속성에서 실행되는 커스텀 로직을 '라이프 사이클 훅(hook)'이라함. <br>
(생성 > 부착 > 갱신(변화 있는 경우만) > 소멸)

![]({{site.baseurl}}/images/yjyoo/vue/lifecycle.jpg)
![]({{site.baseurl}}/images/yjyoo/vue/lifecycle2.jpg)

***
##### Vue 인스턴스 속성
1. el 속성 : vue 인스턴스가 그려질 지점 지정.
2. data 속성 : 값을 정의하여 HTML에서 \{\{\}\}값과 바인딩. 
3. template : 화면에 표시할 HTML, CSS등의 마크업 요소를 정의. 뷰의 데이터 및 기타 속성등도 
                 화면에 그릴 수 있음.
4. methods : 화면 로직 제어와 관계된 메서드를 정의하는 속성. 마우스 클릭 이벤트 처리와 같이 
                 화면의 전반적인 이벤트와 화면 동작과 관련된 로직 추가 가능.
5. created : 뷰 인스턴스가 생성되자마자 실행할 로직 정의.

***
예제 - 인스턴스 생성
{% highlight js %}
<html>
  <head>
      <title>Vue Sample 1</title>
  </head>
  <body>
      <div id="app">ID APP : {{ message }}</div>
      <div id="app2">ID APP2 : {{ message }}</div>
      <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
      <script>
          new Vue({
              el: '#app',
              data: {
                  message : 'Hello World!'
              }
          })
      </script>
  </body>
</html>
{% endhighlight %}

예제 - Lifecycle hook : 콘솔창 로그 확인!
{% highlight js %}
<html>
    <head>
        <title>Life Cycle</title>
    </head>
    <body>
        <div id="app">{{ message }}</div>
        <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
        <script>
            new Vue({
                el: '#app',
                data: {
                    message : 'One!'
                },
                beforeCreate : function() {
                    console.log('beforeCreate');
                },
                created : function() {
                    console.log('created');
                },
                mounted : function() {
                    console.log('mounted');
                    //this.message = 'Two!'
                },
                beforeUpdate : function() {
                    console.log('beforeUpdate');
                    //this.message = 'Three!'
                },
                updated : function() {
                    console.log('updated');
                },
                destroyed : function() {
                    console.log('destroyed');
                }
            })
        </script>
    </body>
</html>
{% endhighlight %}
***

##### [참고]
* 가이드 : https://kr.vuejs.org/v2/guide/