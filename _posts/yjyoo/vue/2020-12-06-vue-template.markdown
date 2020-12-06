---
layout: post
title: Vue Template
image: /yjyoo/vue/vue.jpg
date: 2020-12-06 15:04:00 +0900
tags:
categories: yjyoo
---
#### Vue Template

* HTML, CSS 등의 마크업 속성과 뷰 인스턴스에서 정의한 데이터 및 로직들을 연결하여 사용자가 브라우저에서 볼 수 있는 형태의 HTML로 변환해주는 속성

* 사용방법
1) ES5에서 뷰 인스턴스의 template속성을 활용
{% highlight js %}
new Vue({
    template: '<p>TEMPLATE</p>'
});
{% endhighlight %}

> template 속성에서 정의한 "마크업 + 뷰 데이터"를 virtual DOM 기반의 render() 함수로 변환. 변환된 render()함수는 최종적으로 사용자가 볼 수 있게 화면을 그리는 역할을 함. 변환 과정에서 뷰의 반응성(Reactivity)이 화면에 더해짐.

2) ES6 기반에 싱글 파일 컴포넌트 체계의 &lt;template&gt; 코드 활용
{% highlight js %}
    <template>
        <p>템플릿</p>
    </template>
{% endhighlight %}

***
#### Vue Template 속성과 문법

* 데이터 바인딩 : HTML 화면 요소를 뷰 인스턴스의 데이터와 연결하는 것.<br>
1) \{\{\}\} (콧수염 괄호) : 뷰 인스턴스의 데이터를 HTML 태그에 연결하는 가장 기본적인 방식.
{% highlight js %}
<div id="app" v-once> {{ message }} </div>
new Vue({ el: '#app', data: { message: 'binding' } });
{% endhighlight %}

> v-once 속성 사용하면 뷰 데이터가 변경되어도 값을 바꾸지 않음.

&nbsp;&nbsp;2) v-bind : 아이디, 스타일 등의 HTML 속성 값에 뷰 데이터 값을 연결할 때 사용하는 방식.         
{% highlight js %}
// v-bind는 ":"로 간소화 할 수 있음.(가급적 v-bind 쓰기)
<p v-bind:style="styleA">스타일바인딩</p>
new Vue({ el: '#app', data: { styleA: 'color: blue' } });
{% endhighlight %}

* 자바스크립트 표현식 : 뷰 템플릿에 자바스크립트 표현식 사용가능.
{% highlight js %}
<p>\{\{ message.split('').reverse().join('') }}</p>
new Vue({ el: '#app', data: { message: 'binding' } });
{% endhighlight %}
	
※ 주의점 <br>
1) 자바스크립트 선언문과 분기 구문은 사용할 수 없음. 삼항 연산자는 사용 가능.<br>
\{\{ var a = 10; }} (x)<br>						
\{\{ if(true) \{return 100;} }} (x)<br>				
\{\{ true? 100 : 0 }} (o)<br>
2) 가독성을 높히기 위해 복잡한 연산은 인스턴스 안에서 처리하고 화면에는 연산 결과 표시하기. computed 속성으로 데이터 속성을 계산하여 최종 결과만 표시하기. computed 속성을 이용하여 반복적인 연산에 대해서 미리 계산하여 저장하여 캐싱(caching) 효과를 얻을 수 있음

> 캐싱(caching) : 데이터나 값을 임시 장소에 미리 복사해 놓은 동작. 일반적으로 데이터에 접근하는 시간이나 값을 다시 계산하는 시간이 오래 걸릴 때 해당 값을 미리 입시 장소에 저장해 놓고 필요할 때 바로 불러 올 수 있기 때문에 수행 시간이 훨씬 빠름.

* 디렉티브(Directive) : HTML 태그안에 'v-'접두사를 갖는 모든 속성. 화면의 요소를 더 쉽게 조작하기 위해 사용하는 기능. 뷰의 데이터 값이 변경되었을 때 화면의 요소들이 리액티브(Reactive)하게 반응하여 변경된 데이터 값에 따라 갱신됨.
{% highlight js %}
<p v-if:"flag">VueJS</p> // flag 값에 따라 VueJS가 보이고 안 보이고가 결정됨.
{% endhighlight %}

    1) v-if : 지정한 뷰 데이터 값의 참/거짓 여부에 따라 해당 HTML 태그 표시 여부 결정.
    2) v-for : 지정한 뷰의 개수 만큼 태그 반복하여 출력
	3) v-show : 데이터의 진위여부에 따라 화면에 표시 여부 결정. 거짓인 경우 css효과를 display:none;로 하여 실제 태그는 남기고 화면 상에 보이지 않음. v-if와 비슷하지만 v-if는 해당 태그를 완전히 삭제.
	4) v-bind : HTML 기본 속성과 뷰 데이터 속성 연결. (간소화 : ':')
	5) v-on : 화면 요소의 이벤트를 감지하여 처리할 때 사용. (간소화 : '@')
	6) v-model : 폼(form)에서 주로 사용되는 속성. 폼에 입력한 값을 뷰 인스턴스 데이터와 즉시 동기화. 입력값을 서버에 보내거나 watch 등의 속성으로 추가 로직 수행 가능. v-model은 <input>, <select>, <textarea>태그에서만 사용가능.

* computed 속성 : 데이터 연산들을 정의하는 영역. data 속성 값의 변화에 따라 자동으로 다시 연산. 캐싱됨.
    
* watch 속성 : 데이터 변화를 감지하여 자동으로 로직 수행. 데이터 호출과 같이 시작이 상대적으로 더 많이 소모되는 비동기 처리에 적합.

> - methods 속성과 computed 속성의 차이 : methods 속성은 호출할 때만 해당 로직 수행(수동적). computed 속성은 대상 테이터의 값이 변경되면 자동적으로 수행(능동적).
 - computed 속성과 유사하지만, computed 속성은 내장 API를 활용한 간단한 연산 정도로 적합.

***
#### Single File Component

* SPA: 미리 해당 페이지들을 받아 놓고 페이지 이동 시에 클라이언트의 라우팅을 이용하여 화면을 갱신하는 패턴을 적용한 어플리케이션. (페이지 이동시 서버에 요청하여 갱신하지 않음)
* Vue는 단일 파일 컴포넌트 (Single File Component) 기반의 프레임워크.
* 웹의 뷰(view)를 구성하는 요소인 HTML, CSS, JavaScript 코드를 하나의 .vue 파일로 작성.
* .vue 확장자 파일로 프로젝트 구조를 구성하는 방식. 
* .vue 파일 1개는 뷰 어플리케이션을 구성하는 1개의 컴포넌트와 동일.
* 이러한 관리 방식은 적당한 규모의 프로젝트에서 관리의 생산성을 높이고, 협업을 수월하게 함.

* .vue 파일 구성 
1. <template></template> : HTML 태그 내용. 화면에 표시할 요소들을 정의하는 영역.                예) HTML + 뷰 데이터 바인딩
2. <script>export default{}</script> : 자바스크립트 내용. 뷰 컴포넌트의 내용을 정의하는 영역.     예) template, data, methods 등
3. <style></style> : Css 스타일 내용. 템플릿에 추가한 HTML 태그의 CSS 스타일을 정의하는 영역.

***
예제 - data binding
{% highlight js %}
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Data Binding</title>
    </head>
    <body>
        <div id="app">
            <!-- <h1>{{ message }}</h1> -->
        </div>
        <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
        <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
        <script>
            new Vue({
                el: '#app',
                data: {
                    message: '<h1>을 먼저 화면에 부착하고 message값 치환', // {{ message }}가 출력되고 치환 되어서 깜빡거림.
                    message2: '<h2>템플릿</h2>'
                },
                template: '<h2>{{ message2 }}</h2>'
            });
        </script>
    </body>
</html>
{% endhighlight %}

예제 - v-bind
{% highlight js %}
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>v-bind</title>
        <style>
            .classnm {color: blue;}
        </style>
    </head>
    <body>
        <div id="app">
            <p v-bind:id="myid">ID</p>
            <p v-bind:style="mystyle">STYLE</p>
            <p :class="myclass">CLASS</p>
        </div>
        <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
        <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
        <script>
            new Vue({
                el: '#app',
                data: {
                    myid: 'userid',
                    myclass: 'classnm',
                    mystyle: 'color: red; background: yellow;'
                }
            });
        </script>
    </body>
</html>
{% endhighlight %}

예제 - javascript expression
{% highlight js %}
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Javascript Expression</title>
    </head>
    <body>
        <div id="app">
            <p>{ { message } }</p>
            <p>{ { message + '!!!!' } }</p>
            <p>{ { message.split('').reverse().join('') } }</p>
            <hr>
            <!-- { { if(true) { return 100; } } } -->
            <br>
            { { true? 10 + 1 : 8*7 } }
            <br>
            { { message.split('').reverse().join('') } }
            <br>
            { { reversedMessage } }
        </div>
        <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
        <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
        <script>
            new Vue({
                el: '#app',
                data: {
                    message: 'ABCD'
                },
                computed: {
                    reversedMessage: function() {
                        return this.message.split('').reverse().join('');
                    }
                }
            });
        </script>
    </body>
</html>
{% endhighlight %}

예제 - directive
{% highlight js %}
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Vue Directive</title>
    </head>
    <body>
        <div id="app">
            <a v-if="flag">Flag is true.(v-if)</a>
            
            <ul>
                <li v-for="item in list">{{ item }}</li>
            </ul>

            <p v-show="flag">Flag is true.(v-show)</p>
            <h5 v-bind:id="uid">Check Tag Id Attribute</h5>
            <button v-on:click="ClickEvent">click me!</button>
        </div>
        <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
        <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
        <script>
            var vm = new Vue({
                el: '#app',
                data: {
                    flag: true, // false
                    list: ['A', 'B', 'C'],
                    uid: 1
                },
                methods: {
                    ClickEvent: function() {
                        document.getElementsByTagName('button')[0].style = 'color: red;'
                    }
                }
            });
        </script>
    </body>
</html>
{% endhighlight %}

예제 - event
{% highlight js %}
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Vue Event</title>
    </head>
    <body>
        <div id="app">
            <button v-on:click="GetAlert">GetAlert</button>
            <button v-on:click="GetNum(10)">GetNum</button>
            <button v-on:click="GetEventObj">GetEventObj</button>
        </div>
        <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
        <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
        <script>
            new Vue({
                el: '#app',
                methods: {
                    GetAlert: function() {
                        return alert('alert!');
                    },
                    GetNum: function(num) {
                        return alert('num = '+num);
                    },
                    GetEventObj: function(evnt) {
                        console.log(event)
                    }
                }
            });
        </script>
    </body>
</html>
{% endhighlight %}

예제 - Computed
{% highlight js %}
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Computed</title>
    </head>
    <body>
        <div id="app">
            <p v-once>ORIGN : {{ message }}</p>
            <button v-on:click="ChangeMessage">change message!</button>
            <p>message : {{ message }}</p>
            <p>computed : {{ changedMessage }}</p>
        </div>
        <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
        <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
        <script>
            new Vue({
                el: '#app',
                data: {
                    message: 'apple'
                },
                methods: {
                    ChangeMessage: function() {
                        return this.message = 'banna';
                    }
                },
                computed: {
                    changedMessage: function() {
                        return this.message.toUpperCase();
                    }
                }
            });
        </script>
    </body>
</html>
{% endhighlight %}

예제 - watch
{% highlight js %}
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Watch</title>
    </head>
    <body>
        <div id="app">
            <input v-model="msg">
            <p>Input History</p>
        </div>
        <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
        <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
        <script>
            new Vue({
                el: '#app',
                data: {
                    msg: 'Hello Vue!S'
                },
                watch: {
                    msg: function(data) {
                        document.getElementById('app').append(data + ' ');
                    }
                }
            });
        </script>
    </body>
</html>
{% endhighlight %}

***

##### [참고]
* 가이드 : https://kr.vuejs.org/v2/guide/