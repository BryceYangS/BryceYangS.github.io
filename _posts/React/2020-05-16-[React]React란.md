---
layout: post
title: "[React]React란?"
subtitle: "React란?"
categories: study
tags: react
---

![react.png](/assets/img/react/react.png)

 - **React** : A library for creating user interfaces.  

## 리액트는 왜쓰는가?
 - Jquery 이후 SPA(Single Page Application)기반의 Backbone과 AngularJS 등장  
 - 이 후 React 등장 : React는 Angular와 같은 프레임워크가 아닌 `라이브러리`  
 	- Router와 같은 웹 개발을 위한 도구들이 기본적으로 포함되어 있지 않음.
	- But, 그 대신 가볍고 빠르게 기술 습득 가능.

### 리액트 특징
#### 1. Component
 - Component : UI를 구성하는 개별적인 뷰 단위  
 - 전체 application은 component들의 결합  
	- 각 component들은 다른 앱에서 재사용 가능  
 - React의 목표는 성능보다 유지가능한 앱을 만드는 것 --> Component 덕분에 가능  


#### 2. JSX
```javascript
class Square extends React.Component {
	render() {
		return (
			<button className="square"></button>
		);
	}
}

class Game extends React.Component {
	render() {
		return(
			<div className="game">
				<Square />
			</div>
		);
	}
}

// =====================
ReactDOM.render(
	<Game />,
	document.getElementById('root')
);

```  
 - React 코드는 컴파일 과정 필수. JSX의 경우 Babel을 이용한 컴파일 과정을 거쳐 일반 javascript 코드로 변형.
 	- Babel 트랜스파일러을 통한 컴파일이 주로 사용됨.   
	 `Babel` : ES6를 사용할 때, ES5 문법으로 변환해주는 변환장치. 구 버전 브라우저 혹은 IE를 위한 변환 작업 수행
	- React는 Babel 과 같은 트랜스파일러가 필수이기 때문에 Webpack과 같은 모듈 번들러 등의 초기 세팅이 번거로울 수 있음
 - 그렇다면 왜 이런 번거로운 세팅 과정을 거쳐야 하나?
	- 선언형(Declarative)의 장점
		- html의 형태와 비슷한 JSX는 개발자가 만들고자 하는 view에 직관적으로 접근하는 것을 가능하게 해줌.  


#### 3. Virtual DOM
 - DOM?
	- Document Object Model : 웹페이지에 대한 인터페이스. HTML요소들의 구조화된 표현.
	- 단순히 브라우저에서 보이는 것이 DOM은 아니다. DOM은 display:none 속성을 가질 수도 있고, javascript 소스를 통해 새로운 DOM 객체가 생성, 수정, 삭제될 수 있기 때문이다.

```javascript
class HelloMessage extends React.Component {
	render() {
		return (
			<div>
				<div> Hello {this.props.name}</div>
				<div>I am {this.state.chatName}</div>
			</div>
		);
	}
}
```
 - this.props.name 또는 this.state.chatName 중 하나의 값이 바뀔때마다 브라우저에서 DOM을 변형한다면 성능 상 비효율이 발생할 수 있음
 - React는 이러한 작업에 대한 최적화를 위해 virtual DOM을 사용.
	- React 컴포넌트는 render를 다시 호출해 새로운 결과값을 return. 
	- return 값이 바로 DOM에 반영되지 않음.(브라우저에 렌더링 되지 않음)
	- 새로운 결과값은 virtual DOM으로 만들어지고 현재 브라우저에서 표출되고 있는 DOM과 비교해 차이점을 탐색.  
	- 차이점만 DOM에 적용.  
	- 결과적으로 <u>브라우저가 처리하는 연산의 양 감소</u>  
- 그렇다면 React가 DOM보다 빠르다? 
	- React의 목적은 유지보수가 가능한 어플리케이션을 만드는 것을 위한 라이브러리. React의 속도는 enough fast, 즉 유지보수가 가능한 앱 개발을 위한 충분한 속도를 제공하다는 것.  

**※유의 사항 : React가 무조건 효율적인 것은 아니다. 최적화 작업이 수반되지 않는다면 오히려 퍼포먼스가 떨어질 수 도 있음**
  
  
## 1. Hello World
```javascript
ReactDOM.render(
	<h1>Hello world!</h1>,
	document.getElementById('root')
);
```
  
 Reference:  
  - [What,exactly, is the DOM?](https://bitsofco.de/what-exactly-is-the-dom/?utm_source=CSS-Weekly&utm_campaign=Issue-341&utm_medium=email)  
  - [Mungue Lee님의 "React: #1 React의 탄생배경과 특징"
](https://medium.com/@RianCommunity/react%EC%9D%98-%ED%83%84%EC%83%9D%EB%B0%B0%EA%B2%BD%EA%B3%BC-%ED%8A%B9%EC%A7%95-4190d47a28f)
 - [velpert님의 "[번역] 리액트에 대해서 그 누구도 제대로 설명하기 어려운 것 – 왜 Virtual DOM 인가?"](https://velopert.com/3236)