---
layout: post
title: Vue Component
image: /yjyoo/vue/vue.jpg
date: 2020-12-06 15:02:00 +0900
tags:
categories: yjyoo
---
#### Vue Component

* 컴포넌트 : 조합하여 화면을 구성할 수 있는 블록. 화면을 구조화할 수 있고, 일괄적인 패턴으로 개발 가능. 재활용 가능.
* 컴포넌트 마다 자체적으로 고유한 유효 범위(scope) 가짐.
* 각 컴포넌트 유효 범위가 독립적. 다른 컴포넌트의 값 직접 참조 불가.
* 컴포넌트를 등록하는 방법 (전역, 지역 컴포넌트) <br>
 1) 전역 컴포넌트 : 뷰로 접근 가능한 모든 범위에서 사용 가능.
{% highlight js %}
Vue.component('컴포넌트 이름', { /*컴포넌트 내용*/ });
{% endhighlight %}
&nbsp;&nbsp; 2) 지역 컴포넌트 : 인스턴스에 component 속성을 추가. 새 인스턴스 생성할 때마다 생성해줘야함.
{% highlight js %}
 	new Vue({ 
	     components: {
		       '컴포넌트 이름': /*컴포넌트 내용*/
		   }
	});
{% endhighlight %}

> - 컴포넌트 이름 : template 속성에서 사용할 HTML 사용자 정의 태그 이름을 의미.
HTML 속성은 대소 문자를 구분하지 않기 때문에 템플릿을 사용할 때 kebab-case(하이픈 구분)를 사용 해야함 (camel-case (x))
 - 컴포넌트 내용 : 컴포넌트 태그가 실제 화면의 HTML 요소로 변환될 때 표시될 속성들을 작성. template, data, methods 등 인스턴스 옵션 속성을 정의 가능.

<br>
{% highlight js %}
new Vue({ 
    data: function() {
        return {
            key : value
        }
    }
});
{% endhighlight %}
> data 옵션에 함수를 지정하는 이유는 동일한 컴포넌트가 여러 번 사용되더라도 동일한 객체를 가리키는 것이 아니라 함수가 호출될 때마다 만들어진 객체가 리턴되기 때문임. 매번 만들어진 객체가 리턴되기 때문에 서로 다른 객체를 참조할 수 있음.

***
예제 - 지역 vs 전역 컴포넌트
{% highlight js %}
<html>
    <head>
        <title>Component</title>
    </head>
    <body>
        <div id="app1"> APP1
            <g-component></g-component>
            <l-component></l-component>
        </div>
        <hr>
        <div id="app2"> APP2
            <g-component></g-component>
            <l-component></l-component>
        </div>
        <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
        <script>
            Vue.component('g-component', {
                template: '<div>전역입니다.</div>'
            });

            var cmp = {
                template: '<div>지역입니다.</div>'
            };

            new Vue({
                el: '#app1',
                components: {
                    'l-component': cmp
                }
            });

            new Vue({
                el: '#app2'
            });
        </script>
    </body>
</html>
{% endhighlight %}

예제 - 컴포넌트 범위(Scope)
{% highlight js %}
<html>
    <head>
        <title>Component Scope</title>
    </head>
    <body>
        <div id="app">
            <component1></component1>
            <component2></component2>
        </div>
        <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
        <script>
            var cmp1 = {
                template: '<div>컴포넌트 1 {{ data1 }}</div>',
                data: function() {
                    return {
                        data1: 'CMP1'
                    }
                }
            };

            var cmp2 = {
                template: '<div>컴포넌트 2 {{ data2 }}</div>',
                data: function() {
                    return {
                        data2: cmp1.data.data1  // cmp1.data().data1
                    }
                }
            };

            new Vue({
                el: '#app',
                components: {
                    'component1': cmp1,
                    'component2': cmp2,
                }
            });
        </script>
    </body>
</html>
{% endhighlight %}
***

##### [참고]
* 가이드 : https://kr.vuejs.org/v2/guide/
* Vue Component: https://luji.tistory.com/80
