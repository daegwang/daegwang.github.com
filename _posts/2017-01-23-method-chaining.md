---
layout: post
title: "Method Chaining Pattern for Javascript"
description: "자바스크립트의 메소드 체이닝(Method Chaining) 패턴"
date: 2017-01-23
tags: [javscript, pattern]
comments: true
share: true
---

## 1. Method Chaining이란?
많이들 사용하는 자바스크립트 라이브러리인 jQuery에서 어떻게 사용되는지 알아보면서 메소드 체이닝에 대하여 알아보도록 하겠습니다.

```js
var targetDiv = $('#divId');
targetDiv.html('Hello!');
targetDiv.css('font-size', '12');
targetDiv.show();
```

위의 코드를 보면 특정 div의 html을 변경한 후, css속성 추가, show메소드 실행을 차례대로 하는 것을 볼 수 있습니다. 이런 코드는 다른 언어에서도 일반적으로 자주 보이는 코드인데요. 이를 메소드 체이닝을 사용하면 더 간결하게 표현할 수 있습니다.

```js
$('#divId').html('Hello!').css('font-size', '12').show();
```

이전의 코드에 비해 조금 더 간결해진 것을 보실 수 있죠? 이처럼 한 객체에 여러 메소드들을 호출할 경우 뒤에 메소드들을 연결시켜서 호출하는 방법을 메소드 체이닝 방식이라고 부릅니다. 

## 2. Method Chaining 구현하기
메소드 체이닝 구현을 위해 dom객체를 다루는 간단한 오브젝트를 만들도록 하겠습니다.

```js
var util = {
	var dom;
};

util.prototype.get = function(id){
	this.dom = document.getElementById(id);
}

util.prototype.visible = function(v){
	this.dom.style.display = (v ? "block" : "none");
}

util.prototype.html = function(html){
	this.dom.append(html);
}
```

간단히 설명하자면 document내에서 id를 가지는 컴포넌트를 얻어와서 dom 변수에 할당하는 get함수,  html추가, display옵션을 바꾸는 메소드로 이루어져 있습니다.

위 코드를 메소드 체이닝 기법이 가능하도록 수정하겠습니다.

```js
util.prototype.get = function(id){
	this.dom = document.getElementById(id);
	return this;
}

util.prototype.visible = function(v){
	this.dom.style.display = (v ? "block" : "none");
	return this;
}

util.prototype.html = function(html){
	this.dom.append(html);
	return this;
}
```

위와 같이 각 메소드 내에 객체 인스턴스인 `this`를 리턴하도록 구현하면 메소드 체이닝 패턴을 구현할 수 있습니다. 체이닝을 사용 예시는 다음과 같습니다.

```js
util.get('divId').html('text').visible(true);
```

위와 같은 메소드 체이닝은 자바스크립트에서 자주 보실 수 있는 패턴입니다. 다른 언어에서도 사용이 가능하기는 하지만 언어 특성에 따라서 체이닝을 사용하였더니 코드 복잡도가 증가할 수도 있으니 잘 선택해서 사용하는 것이 중요합니다.