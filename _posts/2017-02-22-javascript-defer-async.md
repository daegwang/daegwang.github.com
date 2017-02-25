---
layout: post
title: "Javascript Defer & Async"
description: "자바스크립트의 Defer, Async 속성"
date: 2017-02-22
tags: [javascript]
comments: true
share: true
---

자바스크립트의 defer, async 에 대해 알아보도록 하겠습니다.

자바스크립트 파일을 로딩하는데 있어서 가장 기본적인 방법은 head에 자바스크립트를 선언하여 사용하는 방법입니다.

```html
<head>
<script type="text/javascript" src="js/heavy.js"></script>
</head>
```

하지만 로딩하여야 할 스크립트가 많을 경우 스크립트를 모두 로딩하기 전까지는 DOM 생성이 미루어지게 됩니다.
이를 피하기 위해 사용하는 방법 중 하나로 DOM아래에 스크립트를 위치시키는 방법이 있습니다. 


```html
<body>
	//DOM Loading..
</body>
<script type="text/javascript" src="js/heavy.js"></script>
```


# defer와 async
위와 같은 문제점을 해결하기 위해 스크립트의 속성으로 defer와 async 키워드를 사용할 수 있습니다.

## defer

```html
<script defer type="text/javascript" src="js/heavy.js"></script>
```

HTML파싱과 스크립트 로딩이 동시에 실행되며 이후 실행됩니다.

## async

```html
<script defer type="text/javascript" src="js/heavy.js"></script>
```

파싱과 스크립크 로딩이 동시에 이루어지지만 스크립트 로딩 후 실행 시에는
HTML파싱이 중단됩니다.


## References
- [async 속성][link1]
- [defer 속성][link2]

[link1]: http://html5ref.clearboth.org/doku.php?id=html5:attribute:async_script
[link2]: http://html5ref.clearboth.org/doku.php?id=html5:attribute:defer_script