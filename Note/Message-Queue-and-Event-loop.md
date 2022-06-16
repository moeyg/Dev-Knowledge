# 메시지 큐와 이벤트 루프

 JS는 실제로 어떻게 작동하는 것일까? 

 JS는 **싱글 스레드 기반 언어**로, JS 엔진은 단일 콜 스택을 갖는다. 이 말은 **the call stack == one thread == one call stack == one thing at a time** 으로 싱글 스레드 런타임을 가지고 있다는 뜻인데, 한 번에 하나의 싱글 콜 스택을 갖는다는 뜻이다.

 즉, 특정 코드를 수행 완료한 이후 아래줄의 코드를 수행하는 동기적 처리 방식을 갖는다는 것을 의미한다. (참고로 비동기적은 특정 코드를 수행하는 도중에도 아래로 계속 내려가며 수행하는 방식을 갖는다.)

<img src="https://github.com/moeyg/Front-end-Knowledge/blob/1433707dea3ddf3eb54437c8d9eab17ab9c9d04f/Images/Message-Queue-and-Event-loop/Message-Queue-and-Event-loop-01.png" width="600px" />

 우선 콜 스택은 실행되는 순서를 기억한다. 함수를 실행하려면, stack에 해당하는 함수를 집어 넣게 되는데 함수에서 리턴이 일어나면, 스택의 가장 위쪽에서 해당하는 함수를 꺼내준다. 이것이 콜 스택이 하는 일의 전부이다.

<img src="https://github.com/moeyg/Front-end-Knowledge/blob/1433707dea3ddf3eb54437c8d9eab17ab9c9d04f/Images/Message-Queue-and-Event-loop/Message-Queue-and-Event-loop-02.png" width="600px" />

  무엇인가를 리턴할 때마다 위와 같이 스택의 맨 위에 있는 것을 하나씩 꺼내게 된다. 예시에서는  `multifly` 에서  `square` 로 리턴 되고  `printSquare` 까지 돌아와  `console.log` 를 실행한다. 

 이렇게 순차적으로 하나씩 실행이 된다면, 무거운 함수가 있을 경우에는 속도가 저하되는 상황이 발생할 것이다. 그렇기 때문에 콜백을 받아 비동기적 처리 방식을 사용하게 된다.

 비동기 요청 처리는 JS를 실행하는 환경인 브라우저나 Node.js가 담당하게 된다. **여기서 JS 엔진과 그 실행 환경을 상호 연동시켜주는 장치가 바로 이벤트 루프이다.** 따라서, 이벤트 루프는 자바스크립트 엔진에 있지 않고 그 환경에 속한다.

  메시지 큐는 프로세스 또는 프로그램 인스턴스가 데이터를 서로 교환할 때 사용하는 통신 방법이다. 즉, 비동기 메시지를 사용하는 응용 프로그램 간의 데이터 송수신을 의미한다. 비동기에서 큐를 사용하기 때문에 나중에 처리할 수 있다.

 자바스크립트의 실행 환경은 2가지 큐를 가지고 있으며 각각 스크립트 실행, 이벤트 핸들러, 콜백함수 등의 태스크(Task) 담기는 공간이다. 

 태스크가 콜백 함수라면 그 종류에 따라 다른 큐에 담기며 대표적인 예로는 다음과 같은 것들이 있다.

- **매크로 태스크 큐 :** `setTimeout()` , `setInterval()` , UI 렌더링, `requestAnimationFrame()`
- **마이크로 태스크 큐 :** `Promise`,`MutationObserver`
    
    

 이벤트 루프는 2개의 큐를 감시하고 있다가 콜 스택이 비면, 콜백 함수를 꺼내와서 실행한다. 

 이 때 마이크로 태스크 큐의 콜백 함수가 우선순위를 가지기 때문에 마이크로 태스크 큐의 콜백 함수를 전부 실행하고 나서 매크로 태스크 큐의 콜백 함수들을 실행한다.

```jsx
console.log('call-stack');
setTimeout(() => console.log('Macro-task-queue'), 0);
Promise.resolve().then(() => console.log('Micro-task-queue'));

// 결과

// call-stack
// Micro-task-queue
// Macro-task-queue
```

 다음과 같은 코드가 처음 실행될 때,

<img src="https://github.com/moeyg/Front-end-Knowledge/blob/1433707dea3ddf3eb54437c8d9eab17ab9c9d04f/Images/Message-Queue-and-Event-loop/Message-Queue-and-Event-loop-03.png" width="600px" />

가장 우선적으로 “스크립트 실행” 이 태스크 큐에 들어가게 된다. 

<img src="https://github.com/moeyg/Front-end-Knowledge/blob/1433707dea3ddf3eb54437c8d9eab17ab9c9d04f/Images/Message-Queue-and-Event-loop/Message-Queue-and-Event-loop-04.png" width="600px" />

 이후, 이벤트 루프가 그 태스크를 로드해 스크립트를 실행시킨다.

 따라서 맨 처음에 `console.log('call-stack')` 이 실행된다.

<img src="https://github.com/moeyg/Front-end-Knowledge/blob/1433707dea3ddf3eb54437c8d9eab17ab9c9d04f/Images/Message-Queue-and-Event-loop/Message-Queue-and-Event-loop-05.png" width="600px" />

그 다음, `setTimeout()` 이 콜 스택으로 가고 브라우저가 이를 받아서 타이머를 동작시킨다.

<img src="https://github.com/moeyg/Front-end-Knowledge/blob/1433707dea3ddf3eb54437c8d9eab17ab9c9d04f/Images/Message-Queue-and-Event-loop/Message-Queue-and-Event-loop-06.png" width="600px" />

타이머가 끝나면 `setTimeout()` 의 콜백 함수를 태스크 큐에 넣고 대기 시킨다.

<img src="https://github.com/moeyg/Front-end-Knowledge/blob/1433707dea3ddf3eb54437c8d9eab17ab9c9d04f/Images/Message-Queue-and-Event-loop/Message-Queue-and-Event-loop-07.png" width="600px" />

`Promise()` 가 콜 스택으로 가고 콜백 함수를 마이크로 태스크 큐에 넣는다.

<img src="https://github.com/moeyg/Front-end-Knowledge/blob/1433707dea3ddf3eb54437c8d9eab17ab9c9d04f/Images/Message-Queue-and-Event-loop/Message-Queue-and-Event-loop-08.png" width="600px" />

이벤트 루프는 마이크로 태스크 큐에서 `Promise()` 의 콜백함수를 가져와 콜 스택에 넣는다.

`console.log('Micro-task-queue')` 가 실행된다.

<img src="https://github.com/moeyg/Front-end-Knowledge/blob/7d89e49ffa888509ce9c6b1bb3750a2c0b6db8b9/Images/Message-Queue-and-Event-loop/Message-Queue-and-Event-loop-09.png" width="600px" />

 `Promise` 의 콜백 함수가 끝나고 태스크 큐에서 제일 오래된 태스크인 `setTimeout()` 의 콜백함수를 가져와 콜 스택에 넣는다.

 그리고 마지막으로 `console.log('Macro-task-queue')` 가 실행된다.

<img src="https://github.com/moeyg/Front-end-Knowledge/blob/1433707dea3ddf3eb54437c8d9eab17ab9c9d04f/Images/Message-Queue-and-Event-loop/Message-Queue-and-Event-loop-10.png" width="600px" />

모든 과정이 끝나면 콜 스택이 비게 되고 프로그램이 종료된다.

<br>

## 정리

 JS는 **싱글 스레드 기반 언어**로, 콜 스택에서 코드를 순차적으로 수행하는 동기적 처리 방식을 갖습니다. 
 하지만 이렇게 순차적으로 하나씩 실행이 된다면, 무거운 함수가 있을 경우에는 스택을 블락하고 있기 때문에 다음 코드가 실행 되기까지 기다려야 할 것입니다.
 이를 해결하기 위해 이벤트 루프를 통해 비동기 콜백 방식을 사용합니다.
 
  콜 스택의 역할은 실행되는 순서를 기억합니다. 함수를 실행하려면, 스택에 해당하는 함수를 집어 넣고, 리턴이 일어나면, 스택의 가장 위쪽에서 해당하는 함수를 꺼내줍니다. 이것이 콜 스택이 하는 일의 전부입니다.

 비동기 요청 처리는 JS를 실행하는 환경인 브라우저나 Node.js가 담당하게 됩니다. **여기서 JS 엔진과 그 실행 환경을 상호 연동시켜주는 장치가 바로 이벤트 루프입니다.**

 비동기적으로 처리해야 하는 함수들은 대기실과 같은 역할을 하는 Web APIs로 전달되고, 실행될 준비가 되면 태스크 큐에 순서대로 대기하게 됩니다.

 태스크 큐의 종류로는 매크로 태스크 큐와 마이크로 태스크 큐, 두 가지가 있습니다. 태스크가 콜백 함수라면 그 종류에 따라 다른 큐에 담깁니다.
 
 대표적인 예로는 매크로 태스크 큐에 setTimeout() setInterval() 등이 있고, 마이크로태스크 큐에는 Promise 등이 있습니다.

 이벤트 루프는 2개의 큐를 감시하고 있다가 콜 스택이 비면, 콜백 함수를 꺼내와서 실행합니다. 

 이 때 마이크로 태스크 큐의 콜백 함수가 우선순위를 가지기 때문에 마이크로 태스크 큐의 콜백 함수를 전부 실행하고 나서 태스크 큐의 콜백 함수들을 실행합니다.
 
 여기서 중요한 점은 스택이 비어있을 때만 올려 보낸다는 것입니다.

 이렇게 자바스크립트는 본디 한 코드가 끝나야 그 다음 코드가 시작되는 동기적인 언어지만, 앞서 말한 일련의 과정들을 거치면서 마치 여러 코드가 동시에 실행되는 것처럼 비동기적인 처리가 가능하게 되는 것 입니다. 
