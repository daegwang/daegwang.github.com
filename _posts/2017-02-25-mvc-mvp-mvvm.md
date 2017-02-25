---
layout: post
title: "Diferrence between MVC, MVP and MVVM"
description: "MVC, MVP, MVVM 패턴"
date: 2017-02-25
tags: [pattern]
comments: true
share: true
---

자주 사용되는 세 가지 패턴(MVC, MVP, MVVM)에 대해 알아보겠습니다. 

## Description
먼저 패턴의 구성요소에 대해 알아보겠습니다.

### 1. Model (Data Layer)
어플리케이션 데이터들을 가지고 있는 객체입니다.

### 2. View (Presentation Layer)
실제 사용자에게 보여지는 화면을 담당합니다. 모델의 정보가 뷰를 통해 사용자에게 보여집니다.

### 3. Controller, Presentation, View Model
각 패턴별로 일부 차이가 있지만 모델과 뷰 사이의 상호 작용에 관여하게 됩니다.


## Patterns
다음으로는 각 패턴의 특징에 대해 알아보도록 하겠습니다.

![alt text](http://lh3.ggpht.com/_Im7hKl1lzL0/TEA29BTyskI/AAAAAAAABAQ/gW4lvkUkEuo/mvc-mvp-mvvm_thumb%5B8%5D.png?imgmax=800 "MVC vs MVP vs MVVM")



### MVC
일반적으로 많이 알려진 패턴입니다. 사용자 요청은 컨트롤러에게 반영이 되며 컨트롤러에서는 변경이 필요할 경우 모델의 데이터를 변경해 줍니다.
이러한 모델의 변경은 컨트롤러를 통해 뷰가 받게 됩니다. (ex. Spring MVC, Ruby on Rails)

### MVP
컨트롤러 대신 Presenter를 사용하게 됩니다. 이는 모델과 뷰의 의존성을 없애기 위함이며 각각의 상호 작용은 Presenter를 통해 이루어지게 됩니다. 

### MVVM
컨트롤러 대신 ViewModel을 사용한 패턴입니다. Binding을 통해 View와 ViewModel간의 연결이 이루어지며, Model과 ViewModel간의 상호 작용이 이루어 집니다. ViewModel의 값이 변경되면 View의 변경이 이루어지게 됩니다. View와 Model간의 의존성을 분리할 수 있는 장점이 있습니다. (ex. WPF, Windows App(XAML), Adobe Flex)


## References
- [Comparing the MVC, MVP and MVVM patterns][link1]

[link1]: http://www.andyfrench.info/2010/07/comparing-mvc-mvp-and-mvvm-patterns.html