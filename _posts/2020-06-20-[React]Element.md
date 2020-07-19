---
layout: post
title: "[React] Element"
subtitle: "Element"
categories: study
tags: react
---


**블로그 글은 React에서 제공하는 문서를 거의 그대로 참조하고 가져온 것 입니다.**   

### 1. Element
```javascript
const element = <h1>Hello world!</h1>
```
 - 엘리먼트
 	- React 앱의 가장 작은 단위
	- 엘리먼트는 컴포넌트의 "구성 요소", 컴포넌트가 더 큰 개념

### 2. DOM에 엘리먼트 렌더링하기

```javascript  
<div id="root"></div>  
```  

 - 루트 DOM 노드
	- 모든 엘리먼트를 React DOM에서 관리함
	- React로 구현된 app은 일반적으로 하나의 루트 DOM 노드가 있음
	- React 엘리먼트를 루트 DOM 노드에 렌더링하려면 `ReactDOM.render()`로 전달  
```javascript  
const element = <h1>Hello, World</h1>
ReactDOM.render(
	element,
	document.getElementById('root')
)
```
### 3. 렌더링 된 엘리먼트 업데이트하기
 - React 엘리먼트는 `불변객체`
	- 엘리먼트 생성 후 해당 엘리먼트의 자식 또는 속성 변경 불가  
	(엘리먼트는 영화에서 하나의 프레임과 같이 특정 시점의 UI를 보여줌)  
	





Reference:
 - [React docs : 3.엘리먼트 렌더링](https://ko.reactjs.org/docs/rendering-elements.html)