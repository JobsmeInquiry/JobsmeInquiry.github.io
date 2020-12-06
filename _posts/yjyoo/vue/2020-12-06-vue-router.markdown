---
layout: post
title: Vue Router
image: /yjyoo/vue/vue.jpg
date: 2020-12-06 15:05:00 +0900
tags:
categories: yjyoo
---

***
#### Vue Router

* Routing : 웹 페이지 간의 이동 방법. 화면 간 전환이 매끄럽고, 빠름. .SPA에서 주로 사용함. 
* 라이브러리 추가하기.
* <router-link to="URL 값"> : 페이지 이동 태그, 화면에서는 <a>로 표시되며 to에 지정한 URL로 이동함.
* <router-view> : 페이지 표시 태그. 변경되는 URL에 따라 해당 컴포넌트를 뿌려주는 영역.
* history mode 사용하면 URL의 해시 값 없앨 수 있음.

1. Nested Router : 라우터로 페이지를 이동할 때 최소 2개 이상의 컴포넌트를 화면에 나타낼 수 있음. 상위 컴포넌트 1개에 하위 컴포넌트 1개를 포함하는 구조. 하지만, 화면을 구성하는 컴포넌트의 수가 적을 때는 유용하지만 한 번에 많은 컴포넌트 표시에 한계 있음. > 네임드 뷰
2. Named View : 특정 페이지로 이동했을 때 여러 개의 컴포넌를 동시에 표시하는 라우팅 방식. name 값이 없는 <router-view> 태그는 default값과 매칭됨.

> $mount() API : el 속성과 동일하게 인스턴스를 화면에 붙이는 역할. 인스턴스 생성 후 $mount API를 이용하여 강제로 인스턴스를 화면에 붙일 수 있음.

***
예제 - Router
{% highlight js %}
<html>
    <head>
        <title>Router</title>
    </head>
    <body>
        <div id="app">
            <h1>뷰 라우터 예제</h1>
            <p>
                <router-link to="/main">Main으로 이동</router-link>
                <router-link to="/login">Login으로 이동</router-link>
            </p>
            <router-view>Look At Here!</router-view>
        </div>
        <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
        <script src="https://unpkg.com/vue-router@2.0.0/dist/vue-router.js"></script>
        <script>
            var Main = {
                template: '<div>Main Component</div>'
            }
            var Login = {
                template: '<div>Login Component</div>'
            }
            
            var routes = [
                { path: '/main', component: Main },
                { path: '/login', component: Login },
            ]

            var router = new VueRouter({
                routes
            });

            var app = new Vue({
                router
            }).$mount('#app');
        </script>
    </body>
</html>
{% endhighlight %}

예제 - Nested Router
{% highlight js %}
<html>
    <head>
        <title>Vue Nested Router</title>
    </head>
    <body>
        <div id="app">
            <router-view></router-view>
        </div>
        <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
        <script src="https://unpkg.com/vue-router@3.0.1/dist/vue-router.js"></script>
        <script>
            var User = {
                template: `<div>This is User<router-view></router-view></div>`
            };
            
            var UserPost = {
                template: '<div>This is UserPost.</div>'
            }

            var UserProfile = {
                template: '<div>This is UserProfile.</div>'
            }

            var routes = [
                {
                    path: '/user',
                    component: User,
                    children: [
                        { path: 'post', component: UserPost },
                        { path: 'profile', component: UserProfile },
                    ]
                },
            ];

            var router = new VueRouter({
                routes
            });

            var app = new Vue({
                router
            }).$mount('#app');
        </script>
    </body>
</html>
{% endhighlight %}

예제 - Named View
{% highlight js %}
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Named View</title>
    </head>
    <body>
        <div id="app">
            <router-view name="header"></router-view>
            <router-view></router-view>
            <router-view name="footer"></router-view>
        </div>
        <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
        <script src="https://unpkg.com/vue-router@3.0.1/dist/vue-router.js"></script>
        <script>
            var Header = { template: '<div>This is Header.</div>' };
            var Body = { template: '<div>This is Body.</div>' };
            var Footer = { template: '<div>This is Footer.</div>' };
            
            var router = new VueRouter({
                routes: [
                    { 
                        path: '/',
                        components: {
                            default: Body,
                            header: Header,
                            footer: Footer
                        } 
                    }
                ]
            });

            var app2 = new Vue({
                router
            }).$mount('#app');
        </script>
    </body>
</html>
{% endhighlight %}