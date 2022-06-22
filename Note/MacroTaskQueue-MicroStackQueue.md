<img src="https://github.com/moeyg/Front-end-Knowledge/blob/4bfd33877b5a876fffaf5d995ac0d724d997af60/Images/MacroTaskQueue-MicroStackQueue/MacroTaskQueue-MicroStackQueue-1.gif" width="650px" />
 자바스크립트에서 비동기 작업들은 백그라운드를 통해 태스크 큐에 추가됩니다. 태스크 큐는 발생한 순서대로 쌓이고 이벤트 루프에 의해 처리됩니다.

<img src="https://github.com/moeyg/Front-end-Knowledge/blob/4bfd33877b5a876fffaf5d995ac0d724d997af60/Images/MacroTaskQueue-MicroStackQueue/MacroTaskQueue-MicroStackQueue-2.gif" width="650px" />
 태스크 큐는 매크로 태스크 큐와 마이크로 태스크 큐로 구분할 수 있는데, 이 둘의 차이는 처리할 작업의 우선순위 입니다.

 예를 들어 태스크가 콜백 함수라면 그 종류에 따라 다른 큐에 담기는 겁니다.

 **매크로 태스크 큐에 담기는 콜백 함수의 예시로** setTimeout() , setInterval() 등이 있고,

 **마이크로 태스크 큐에 담기는 콜백 함수의 예시로는** Promise가 있습니다.

 이벤트 루프는 이 2개의 큐를 감시하고 있다가 콜 스택이 비면 콜백 함수를 꺼내와서 실행합니다. 이 때 마이크로 태스크 큐가 매크로 태스크 큐보다 우선순위를 가지기 때문에 마이크로 태스크 큐의 콜백 함수를 전부 실행하고 나서 매크로 태스크 큐의 콜백 함수를 실행합니다.
