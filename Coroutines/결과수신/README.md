<div align="center">
  <p>
    <img src="../README.assets/android.png">
  </p>
  <br>
  <h2>Coroutines</h2>
  <p>코루틴 관련 내용 정리</p>
  <br>
  <br>
</div>




## 🔥 코루틴으로부터 결과 수신 받기

### 1. async-await

- `async` 코루틴 빌더는 코루틴을 생성하고 결과를 반환하기 위해 사용된다
- `async`를 호출하면 `Deferred<T>` 타입의 객체가 반환된다

- `Deferred`는
  - 코루틴의 실행 상태를 관리하는 `Job`의 역할을 포함하면서
  - 코루틴의 결과값을 감싸는 기능을 추가로 가진다

```kotlin
val deferred: Deferred<String> = async {
    "Response"
}
```

<br>

### 2. async와 launch의 차이

- `async`와 `launch` 모두
  - `context` 인자로 `CoroutineName` 이나 `CoroutineDispatcher` 설정 가능
  - `start` 인자로 `CoroutineStart.LAZY`를 설정해 지연 코루틴 생성 가능
- `async` 차이점
  - `async`의 블록은 값을 반환
  - 반환 객체가 `Deferred<T>`

|           | async         | launch    |
| --------- | ------------- | --------- |
| 반환 타입 | `Deferred<T>` | `Job`     |
| 결과값    | 있음          | 없음      |
| 목적      | 결과 수신     | 작업 실행 |

<br>

### 3. Deferred와 await 동작 방식

- `Deferred`는 미래의 어느 시점에 결과값이 반환될 수 있음을 표현하는 코루틴 객체
- 결과가 언제 수신될지 알 수 없기 때문에, 결과가 필요하다면 `await()`를 호출해야 한다
- `await`는
  - 결과가 수신될 때까지 호출한 코루틴을 일시 중단한다
  - 결과가 수신되면 결과값이 반환되고 호출부의 코루틴이 재개된다

```kotlin
fun main() = runBlocking<Unit> {
    val networkDeferred: Deferred<String> = async(Dispatchers.IO) {
        delay(1000L)   
        return@async "Response"
    }
    val result = networkDeferred.await()
    println(result)  // Response 출력
}
```

➡️ `await()` 호출 시점에서 결과값이 반환될 때까지 코루틴이 일시 중단됨

<br>

### 4. Deferred는 Job의 확장 인터페이스

- `launch` 함수를 호출해 생성한 코루틴은 `Job`을 반환하고
- `async` 함수를 통해 생성한 코루틴은 `Deferred`를 반환한다
- `Deferred`는 `Job`의 서브타입으로, 코루틴으로부터의 결과값 수신을 위해 몇가지 함수만 추가된 인터페이스다
- `Deferred` 객체는 `Job` 객체의 모든 함수와 프로퍼티를 사용할 수 있다

<br>

### 5. 복수의 코루틴으로부터 결과 수신

여러 비동기 작업의 결과를 병렬로 실행 후 취합해야 하는 경우가 자주 생긴다

#### 병렬 실행

- `async`를 모두 호출
- 이후 `await()`로 결과 수신

```kotlin
fun main() = runBlocking<Unit> {
    val Deferred1: Deferred<Array<String>> = async(Dispatchers.IO) {
        delay(1000L)   
        return@async arrayOf("사람1","사람2")
    }
     val Deferred2: Deferred<Array<String>> = async(Dispatchers.IO) {
        delay(1000L)   
        return@async arrayOf("사람3")
    }
  
    val result1 = Deferred1.await()
    val result2 = Deferred1.await()
  
    println("참여자 목록 : ${listOf(*result1, *result2)}")  // 참여자 목록 : [사람1, 사람2, 사람3]
}
```

#### awaitAll 사용

- `awaitAll`을 사용하면 여러 `Deferred`의 결과를 한번에 수신할 수 있다
- 모든 결과가 수신될 때까지 호출부의 코루틴을 일시 중단한다

```kotlin
val result: List<Array<String>> = awaitAll(Deferred1, Deferred2)
```

<br>

### 6. withContext를 사용한 결과 수신

#### withContext 함수

- 인자로 받은 `CoroutineDispatcher`에서 코드를 실행한 후 결과값을 반환하는 함수
- `withContext`는 새로운 코루틴을 생성하지 않는다
- 현재 코루틴을 유지한채 실행 컨텍스트만 전환
- 람다식을 실행한 후에는 스레드가 다시 이전의 `Dispatcher`를 사용하도록 전환된다

```kotlin
val result = withContext(Dispatchers.IO) {
    networkCall()
}
```

#### withContext와 async-await

- `withContext`를 2번 이상 사용하면 코루틴이 유지된채로 스레드만 바뀌는 것이기 때문에 순차 처리가 된다
- 따라서 여러 작업을 병렬로 실행해야 한다면 `async` 사용이 필요하다



















