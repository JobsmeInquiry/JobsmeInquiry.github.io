---
layout: post
title: Vue 전역 변수
image: /yjyoo/vue/vue.jpg
date: 2020-12-06 15:06:00 +0900
tags:
categories: yjyoo
---
***
#### Vue 전역변수 (2가지)

* 전역 mixin >> mixin을 전역으로 적용하면 이후에 생성된 모든 Vue 인스턴스 에 영향 줄 수 있음. 
{% highlight js %}
Vue.mixin({
  data: function() {
      return {
        GlobalVar:'Global Variable!'
      }
  },
});
    
new Vue({
	el: '#app',
	data: {
		 result: ''
	 },
	methods:{
		clickMe: function () {   
			this.result= this.GlobalVar;
	  }
  }
});
{% endhighlight %}

* Vuex 사용

***
##### [참고]
* mixin : https://kr.vuejs.org/v2/guide/mixins.html