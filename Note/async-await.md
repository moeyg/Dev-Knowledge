# async / await

비동기를 동기적으로 하기 위해 `.then`을 이용해 Promise chain을 계속 하게 된다면 코드가 난잡할 수 있습니다. 이때 `async`와 `await`을 이용하면 코드를 순서대로 작성하는 것처럼 도움을 줍니다.

 즉, `Promise`를 깔끔하게 사용할 수 있는 방법으로 syntactic sugar 입니다.

 `async`는 함수 앞에 `async` 키워드를 붙여 사용할 수 있습니다. 이렇게 하면 자동적으로 함수 안의 코드들이 `Promise`로 암시됩니다. 그리고 resolved 된 값을 반환해 줍니다.

 `await`은 `async`가 붙은 함수 안에서만 사용이 가능합니다. `await`은 말 그대로 `Promise`가 처리될 때까지 함수 실행을 기다리게 만들고 `Promise`가 settled 상태가 되면 resolved 된 값을 반환합니다. 

 `await`은 `promise.then`보다 좀 더 세련되고 가독성이 좋게 `Promise`의 결과값을 얻을 수 있도록 해주는 문법입니다.
