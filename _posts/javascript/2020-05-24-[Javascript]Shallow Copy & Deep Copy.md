---
layout: post
title: "[Javascript] Shallow Copy & Deep Copy"
subtitle: "javascript copy"
categories: study
tags: javascript
---

## Javascript 복사 
 - Javascript에서 기본형을 복사할 때는 기존 값에 영향을 주지 않음  
 - 문제는 **Object 또는 Array를 복사할 때** 발생
	- Deep Copy를 통해서 해결해야함


### Primitive Type(기본형) 복사
```javascript
let a = 0;
let b = a;
b = 1;

console.log(‘b =’+b);//b =1
console.log(‘a =’+a);//a =0
```  
 - 기본형의 복사는 기존 변수에 영향을 주지 않음


### Object / Array 복사
```javascript
let c = [1,2,3];
let d = c;
d[0] = 0;

console.log(c)//[0, 2, 3]
console.log(d)//[0, 2, 3]
```  
 - Object / Array 복사는 기존 변수에 영향을 줌
 - 얕은 복사(Shallow copy) 때문에 기존 변수에 영향을 주게 됨
	> 깊은 복사(Deep copy)를 통해 기존 변수와의 영향을 단절시킬 수 있음
  
### 얕은 복사(Shallow Copy) vs 깊은 복사(Deep Copy)
  
![얕은복사vs깊은복사](https://miro.medium.com/max/780/1*6fjXVjxrpLWB_U3Gkz51MQ.png)  

 - Shallow Copy : Call by reference
    - 새로운 객체를 생성하고 <u>주소값</u>만 복사
	- javascript 는 객체의 주소값을 할당
	- 결과적으로 **기존 변수에 영향을 줌**
	
 - Deep Copy : Call by Value
    - 새로운 객체를 생성하고 값들을 복사
	- **기존 변수에 영향을 주지 않음**
 
### Array.from() / Object.create()
1. Array.from()
	```javascript
	let a = [1,2,3];
	let b = Array.from(a);
	b[0] = 0;
	console.log(a); // [1, 2, 3]
	console.log(b); // [0, 2, 3]
	```  

	- 만약 배열 내부에 Object가 존재할 경우 `Array.from()` 만으로 해결 불가


	```javascript
	let a = [{x:1,y:2,z:3}];
	let b = Array.from(a);
	b[0].x = 0;

	console.log(JSON.stringify(a)); // [{"x":0,"y":2,"z":3}]
	console.log(JSON.stringify(b)); // [{"x":0,"y":2,"z":3}]
	```

2. Object.create()
	```javascript
	let a = [{x: 1,y: 2,z: 3}];
	let b = Array.from(Object.create(a));
	b[0].x = 0;

	console.log(JSON.stringify(a)); // [{"x":0,"y":2,"z":3}]
	console.log(JSON.stringify(b)); // [{"x":0,"y":2,"z":3}]
	```

### Deep Copy 방법
 - `JSON.parse(JSON.stringify(a))`  
```javascript  
// Deep Copy
let a = { x:{z:1} , y: 2};
let b = JSON.parse(JSON.stringify(a));
b.x.z=0

console.log(JSON.stringify(a)); // {"x":{"z":1},"y":2}
console.log(JSON.stringify(b)); // {"x":{"z":0},"y":2}  
```


 - `Array.from(myObject, obj => Object.assign({}, obj))` :  JSON.parse(JSON.stringify(obj)) 보다 더 빠름 
	- 다만, 유의사항으로 IE에서 Array.from() 이 지원되지 않음

 - `var newArray = oldArray.slice();` : 기본형이 담긴 배열에 대해서만 deep copy 가능

Reference:
 - [JavaScript Deep copy for array and object](https://medium.com/@gamshan001/javascript-deep-copy-for-array-and-object-97e3d4bc401a)