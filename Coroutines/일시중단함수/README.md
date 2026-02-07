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


## 🔥 일시 중단 함수와 코루틴

### 1. 일시 중단 함수

> suspend fun 키워드로 선언되는 함수
>
> 함수 내부에 일시 중단 지점을 포함할 수 있는 함수

- 일시 중단 함수는 코루틴의 실행을 중단할 수 있는 코드 블록을 재사용하기 위해 사용된다

- `delay` 같은 일시 중단 지점을 포함할 수 있기 때문에 코루틴 내부에서만 호출 가능하다

<br>

### 2. suspend 키워드가 필요한 이유

아래 코드에서는 오류가 생긴다

- `delay`는 일시 중단 함수
- 하지만 `delayAndPrint`는 일반 함수이기 때문에 일시 중단 지점을 포함할 수 없다

```kotlin
fun main() = runBlocking<Unit> {
    delayAndPrint()
    delayAndPrint()
}

fun delayAndPrint() {
    delay(1000L)  // ❌
    println("hi")
}
```

따라서 아래처럼 `suspend` 키워드를 붙여야 한다

```kotlin
suspend fun delayAndPrint() {
    delay(1000L)  // ✅
    println("hi")
}
```

<br>

### 3. 일시 중단 함수는 코루틴 밖에서 호출할 수 없다

- 일반 함수는 일시 중단이 허용되지 않는 실행 환경
- 따라서 일시 중단 함수는 코루틴 또는 일시 중단 함수 내부에서 호출되어야 한다

```kotlin
fun main() {
    delayAndPrint()  // ❌  
    delayAndPrint()  // ❌
}

suspend fun delayAndPrint() {
    delay(1000L)
    println("hi")
}
```

<br>

### 4. 일시 중단 함수에 대한 오해

- 일시 중단 함수 = 코루틴 ❌
- 일시 중단 함수는 코루틴이 아니다 ✅

일시 중단 함수는

- 코루틴을 생성하지 않는다
- 스스로 비동기로 실행되지 않는다
- 코루틴 안에서 실행될 수 있는 함수일 뿐이다

➡️ 실제로 실행 단위를 만드는 것은 `launch`, `async` 같은 코루틴 빌더다

<br>

### 5. 일시 중단 함수를 병렬로 실행하는 방법

일시 중단 함수는 순차적으로 실행된다

병렬 실행을 원한다면 별도의 코루틴으로 감싸야 한다

```kotlin
fun main() = runBlocking<Unit> {
    val job1 = launch {
        delayAndPrint()
    }
    val job2 = launch {
        delayAndPrint()
    }
    joinAll(job1, job2)
}

suspend fun delayAndPrint() {
    delay(1000L)
    println("hi")
}
```

➡️ 각 `launch`는 별도의 코루틴을 생성

- 두 코루틴이 동시에 실행되므로 전체 실행 시간은 약 1000ms
- 만약 `launch`로 감싸지 않는다면 두 함수는 순차 실행되어 약 2000ms가 걸린다

<br>

### 6. 일시 중단 함수의 호출 가능 지점

일시 중단 함수는 일시 중단을 할 수 있는 곳에서만 호출할 수 있다

#### 호출 가능한 위치

- 코루틴 내부 (`launch`, `async`, `runBlocking` 등)
- 다른 일시 중단 함수 내부
- `coroutineScope` / `supervisorScope` 내부

#### coroutineScope

- 일시 중단 함수 내부에서 새로운 코루틴 스코프를 생성
- 구조화된 동시성을 유지
- 자식 코루틴이 모두 완료될 때까지 일시 중단

```kotlin
suspend fun loadData() = coroutineScope {
    launch { /* 작업 1 */ }
    launch { /* 작업 2 */ }
}
```

#### supervisorScope

- coroutineScope와 구조는 같지만
- 자식 코루틴에서 발생한 예외가 부모로 전파되지 않음
