# Promise

`Promise`는 JavaScript 안에 내장되어 있는 오브젝트로 비동기를 간편하게 처리할 수 있도록 도와주는 역할을 한다. 따라서 `Promise`는 클래스이기 때문에 `new` 키워드를 사용하여 오브젝트를 생성할 수 있다.

 `Promise`는 기능이 실행되고 나서 정상적으로 수행 되었다면 성공 메시지와 함께 처리된 결과값을 리턴하고, 만약 수행 중 문제가 발생하면 에러 메시지를 호출할 수 있다.

<img src="https://github.com/moeyg/Front-end-Knowledge/blob/7865562c07b10183112ec5ac2cdb1bc2eb5c2b4f/Images/Promise/Promise-1.png" width="500px" />

 `Promise`는 생성되고 오퍼레이션을 수행 중일 때는 pending 상태가 되고, 성공적으로 끝내게 되면 fullfilled 상태가 된다. 하지만, 파일을 찾을 수 없거나 네트워크 오류가 생긴다면 rejected 상태가 됩니다. 

 `Promise`에는 원하는 기능을 수행해서 데이터를 만들어 내는 Producer와 테이터를 소비하는 Consumer object로 나눠진다.

<img src="https://github.com/moeyg/Front-end-Knowledge/blob/7865562c07b10183112ec5ac2cdb1bc2eb5c2b4f/Images/Promise/Promise-2.png" width="500px" />

 우선 Producer에 대해서 알아보면, 

 `Promise` 생성자에는 `executor`라는 콜백 함수를 전달해 주어야 하는데, 여기에는 또 다른 두 가지의 콜백함수를 받는다. 바로 기능을 정상적으로 수행해서 최종 값을 리턴하는 `resolve`와 기능을 수행하다가 문제가 발생하면 호출하게 될 `reject`가 들어있다.

```jsx
const promise = new Promise((resolve, reject) => {
	// 동기로 시작하기 어렵고 무거운 일 수행 중..
  // 딜레이 
	setTimeout(() => {
		// 성공적으로 수행
		resolve('completion');
		// 수행 실패
    // Error 핸들링
		reject(new Error('No network'));
	}, 2000);
});
```

 `Promise`는 생성되는 순간 바로 `executor` 콜백 함수가 자동으로 실행되기 때문에 이 점을 유의해서 코드를 작성해야 한다. 이를 간과하면 불필요한 네트워크 통신을 야기할 수 있습니다. 그렇기 때문에 `setTimeout()`을 사용하여 딜레이를 줄 수 있다.

  Consumers는 `then`, `catch`, `finally`를 사용하여 값을 받아올 수 있습니다. 

 즉, 위에서 작성된 코드는 나중에 `.then`과 `.catch`를 이용해서 성공한 값이나 에러를 받아와서 원하는 방식으로 처리할 수 있다. 또는 성공, 실패의 여부와 상관없이 출력되게 하고 싶은 코드가 있다면 `.finally()`를 사용하여 호출할 수 있다.

```jsx
promise.then((value) => {
	// 작성한 기능 수행 (value에는 resolve 콜백 함수에서 전달된 값이 들어온다.)
	console.log(value); // completion
})
```

 `Promise` 값이 정상적으로 수행이 된다면, `.then`을 사용하여 `resolve` 값을 받아와서 원하는 기능을 수행하는 콜백 함수를 전달하면 된다. `.then`에 들어오는 값은 `resolve` 콜백 함수에서 전달된 값이 들어오고 `console.log(value);` 로 출력하면 `resolve`에 적힌 내용이 출력된다.

```jsx
promise.catch((error) => {
	// 에러가 발생할 경우 어떻게 할 것인지 작성
	console.log(error); // No network
})
```

 `reject`에는 `Error` 오브젝트를 통해서 값을 전달하는데 `.catch`를 통해서  error가 발생했을 때 어떻게 처리할 것인지 콜백 함수를 등록할 수 있다. 만약 `.catch`를 작성하지 않으면 Uncatch SyntaxError가 발생한다.

 `.then`을 호출하게 되면 `Promise`를 리턴하기 때문에 리턴된 `Promise`에서 `catch`를 등록할 수 있는 것이다.

 보통 `Promise` 안에는 무거운 일을 수행하는 함수가 들어있다. 예를 들면, 네트워크에서 데이터를 받아오거나 파일에서 큰 데이터를 불러오는 과정은 시간이 꽤 걸린다. 이러한 일들을 동기로 시작하면 그 다음 라인의 코드가 실행되지 않기 때문에 시간이 걸리는 일들은 `Promise`로 처리하는 것이 좋다.

## 정리

 `Promise`는 비동기를 간편하게 처리할 수 있는 역할을 합니다.

 `Promise`의 `executor`에는 기능을 정상적으로 수행해서 최종 값을 리턴하는 `resolve`와 기능을 수행하다가 문제가 발생하면 호출하게 될 `reject`가 들어있습니다.

 이를 난잡하게 사용할 경우 콜백 지옥에 빠지게 될 수 있습니다. 이를 보완하기 위해 비동기 처리 시점을 명확하게 하여 동기적으로 순서를 설정할 수 있습니다.

 그 방법으로는 `.then`과 `.catch`를 이용해서 성공한 값이나 에러를 받아와서 원하는 방식으로 처리할 수 있습니다. 성공, 실패의 여부와 상관없이 출력되게 하고 싶은 코드가 있다면 `.finally()`를 사용하여 호출할 수 있습니다.

 보통 `Promise` 안에는 무거운 일을 수행하는 함수가 들어있습니다. 예를 들면, 네트워크에서 데이터를 받아오거나 파일에서 큰 데이터를 불러오는 과정은 시간이 꽤 걸립니다. 이러한 일들을 동기로 시작하면 그 다음 라인의 코드가 실행되지 않기 때문에 시간이 걸리는 일들은 `Promise`로 처리하는 것이 좋습니다.
