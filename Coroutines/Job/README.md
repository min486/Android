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


## 🔥 Job & 구조화된 동시성

### 1️⃣ Job을 사용한 코루틴 제어

### 1. 코루틴 순차 처리

#### 순차 처리가 필요한 이유

- '토큰 갱신 후 네트워크 요청' 처럼 작업 간 선후 관계가 있는 경우
- 순차 처리가 보장되지 않으면 잘못된 상태로 작업이 실행 될 수 있다

#### join을 사용한 순차 처리

- `Job.join()` → 대상 코루틴이 완료될 때까지 호출한 코루틴을 일시 중단

```kotlin
fun main() = runBlocking {
    val updateTokenJob = launch(Dispatchers.IO) {
        // 토큰 업데이트
    }
    updateTokenJob.join()
  
    launch(Dispatchers.IO) {
        // 네트워크 요청
    }
}
```

➡️ 토큰 업데이트 완료 후 네트워크 요청 실행

<br>

### 2. 여러 코루틴 완료 대기

- 독립적인 여러 작업을 병렬 실행한 후 모두 완료된 시점에 다음 작업을 실행해야 하는 경우
- `join` 함수를 매번 호출하면 코드가 길어지므로 `joinAll` 함수를 사용하면 좋다
- `joinAll` 함수가 호출되면 인자로 받은 모든 `Job`이 완료될 때까지 호출부의 코루틴이 일시 중단된다

```kotlin
fun main() = runBlocking {
    val job1: Job = launch(Dispatchers.Default) { /* 이미지 변환 1 */ }
    val job2: Job = launch(Dispatchers.Default) { /* 이미지 변환 2 */ }
  
    joinAll(job1, job2)
  
    launch(Dispatchers.IO) {
        // 변환된 이미지 업로드
    }
}
```

<br>

### 3. 지연 코루틴

- 기본적으로 `launch`는 즉시 실행됨
- `CoroutineStart.LAZY`를 사용하면 실행이 지연된 코루틴 생성 가능

- 지연 코루틴은 `start` 함수나 `join` 함수를 호출하면 실행된다

```kotlin
fun main() = runBlocking {
    val lazyJob: Job = launch(start = CoroutineStart.LAZY) {
        println("실행됨")
    }
    delay(1000L)
    lazyJob.start()
}
```

<br>

### 4. 코루틴 취소

#### 코루틴 취소가 필요한 이유

- 코루틴 실행 도중, 더이상 코루틴을 실행할 필요가 없어지면 즉시 취소해야 한다
- 취소하지 않으면 코루틴이 스레드를 계속 사용하기 때문에 애플리케이션의 성능 저하로 이어지기 때문이다

#### cancle 함수의 문제

- `cancel` 함수는 즉시 취소되지 않는다
- 취소 요청 플래그만 설정
- 실제 취소는 취소 확인 시점에서 발생

```kotlin
val job = launch {
    // 작업
}
job.cancel()
```

#### cancelAndJoin 함수 

- 코루틴 취소 이후의 작업이 반드시 취소 완료 후 실행되어야 할 때 사용
- 취소 요청한 후 취소가 완료될 때까지 호출 코루틴 일시 중단 (Cancel + Join)
- 하지만 일시 중단 시점이 없으면 코루틴이 취소되지 않고 계속 실행되는 문제가 있다

```kotlin
fun main() = runBlocking {
    val job = launch(Dispatchers.Default) {
        // 오래 걸리는 작업 
    }
    job.cancelAndJoin()
    // 취소 완료 후 실행돼야 하는 동작
}
```

<br>

### 5. 코루틴 취소 확인 시점

코루틴이 취소를 확인하는 시점은 2가지

- 일시 중단 시점
- 코루틴이 실행을 대기하는 시점

#### 취소 확인 방법 비교

| 방법       | 특징                        | 효율성                |
| ---------- | --------------------------- | --------------------- |
| `delay()`  | 일시 중단 발생              | 비효율                |
| `yield()`  | 일시 중단 후 즉시 재개 요청 | `delay()` 보다 효율적 |
| `isActive` | 일시 중단 없음              | 가장 효율적           |

➡️ `delay`, `yield` 함수 모두 일시 중단 후 재개 과정을 거치기 때문에 비효율적이다

#### CoroutineScope의 isActive 예시

- `CoroutineScope`의 `isActive` 확장 프로퍼티를 사용하면 코루틴을 일시 중단 시키지 않고 취소를 확인할 수 있다
- 코루틴에 취소가 요청되면 `CoroutineScope.isActive`가 `false`가 된다

```kotlin
fun main() = runBlocking {
    val job = launch(Dispatchers.Default) {
        while(this.isActive) {
            println("작업 중")
        }
    }
    delay(100L)
    job.cancel()
}
```

➡️ 코루틴이 100ms 정도 후에 정상적으로 취소된다

<br>

### 6. Job 객체의 상태 변수

- Job 객체를 프린트 하면 코루틴의 상태를 디버그용 문자열 값으로 출력할 수 있다. 하지만 이 값은 코드에서 사용하기 어렵다
- Job 객체는 코루틴의 상태를 간접적으로 나타내는 3가지 상태 변수를 외부로 공개한다

| 프로퍼티    | 의미                     |
| ----------- | ------------------------ |
| isActive    | 실행 중                  |
| isCancelled | 취소 요청됨              |
| isCompleted | 정상 종료 또는 취소 완료 |

<br>

## 2️⃣ 구조화된 동시성

> 비동기 작업을 계층 구조로 관리하여 취소, 예외, 생명주기를 예측 가능하게 만드는 원칙

### 7. 실행 환경 상속

- 부모 코루틴은 자식 코루틴에게 실행 환경을 상속한다

- 코루틴 빌더 함수의 인자로 CoroutineContext가 전달되면 부모 코루틴의 실행 환경을 덮어 씌울 수 있다

- 코루틴 빌더 함수 호출시 Job은 상속되지 않고 새롭게 생성되며 부모 Job을 참조함

```kotlin
launch {
    launch(Dispatchers.IO) {
        // Dispatcher는 덮어씀
    }
}
```

<br>

### 8. 코루틴의 구조화와 작업 제어

- 부모가 취소되면 자식 코루틴도 취소가 전파된다

- 자식 코루틴이 완료되지 않으면 부모도 완료되지 않는다

- 부모 코루틴이 실행해야 할 코드가 모두 실행됐는데 자식 코루틴이 실행 중이라면, 

  부모 코루틴은 '실행 완료 중' 상태를 가진다

<br>

### 9. CoroutineScope 사용해 코루틴 관리

- CoroutineScope 인터페이스는 CoroutineContext를 가진 인터페이스

- launch 또는 async 함수가 호출될 때 CoroutineScope 으로부터 실행 환경을 상속 받아 코루틴이 실행된다

- CoroutineScope 객체를 사용해 특정 범위의 코루틴을 제어할 수 있다

  - cancel 함수를 호출하면 CoroutineScope의 범위에 속한 모든 코루틴을 취소할 수 있다

  - CoroutineScope의 cancel 함수를 호출하는 것은 

    실제로 CoroutineScope이 가진  Job 객체에 cancel 함수를 호출하는 것이다

```kotlin
val scope = CoroutineScope(Dispatchers.Main)
scope.launch { }
scope.launch { }
scope.cancel()  // 모든 코루틴 취소
```

<br>

### 10. 코루틴의 구조화와 Job

- Job의 구조화가 제어에 핵심 역할을 한다

- CoroutineScope 생성 함수나, Job 생섬 함수를 사용해 코루틴의 구조화를 깰 수 있다

- Job 생성 함수에 부모 Job을 설정할 수 있다. 이를 사용해 구조화를 깨지 않을 수도 있다

- Job 생성 함수로 생성된 Job은 자동으로 실행 완료되지 않는다. complete 함수를 명시적으로 호출해 줘야 한다


<br>

### 11. runBlocking 함수

- runBlocking 함수를 호출한 스레드를 차단하는 특수한 코루틴

- 호출부의 스레드를 사용할 수 있는 코루틴은 자신과 자식 코루틴들 뿐이다
- 호출부의 스레드는 runBlocking 함수가 생성한 코루틴이 실행 완료될 때까지 다른 작업에 사용될 수 없다





