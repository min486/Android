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


## 🔥 CoroutineContext & 예외 처리

### 1️⃣ CoroutineContext

### 1. CoroutineContext의 구성 요소

`CoroutineContext`는 여러 구성 요소의 집합이다

| 구성 요소                 | 역할                        |
| ------------------------- | --------------------------- |
| CoroutineName             | 디버깅을 위한 코루틴 이름   |
| CoroutineDispatcher       | 코루틴을 실행할 스레드 결정 |
| Job                       | 코루틴의 생명주기 제어      |
| CoroutineExceptionHandler | 처리되지 않은 예외 처리     |

<br>

### 2. CoroutineContext 구성 방식

- `CoroutineContext` 객체는 Key-Value 구조
- 각 구성 요소는 고유한 Key를 가진다
- 같은 Key가 중복되면 나중에 들어온 구성 요소가 덮어 씌운다
- 더하기 연산자(+)로 조합한다
- 2개의 `CoroutineContext`를 합칠 때도 동일한 규칙 적용

```kotlin
val context = CoroutineName("MyCoroutine") + Dispatchers.IO + Job()
```

<br>

### 3. CoroutineContext 요소 접근

`CoroutineContext` 구성요소에 접근하기 위해서는 구성요소가 가진 고유한 키가 필요하다

#### Key를 사용해 접근

```kotlin
fun main() = runBlocking {
    val coroutineContext = CoroutineName("MyCoroutine") + Dispatchers.IO 
    val name = coroutineContext[CoroutineName.Key]
    println(name)  // CoroutineName(MyCoroutine)
}
```

#### 구성 요소 인스턴스의 key 사용

```kotlin
fun main() = runBlocking {
    val name = CoroutineName("MyCoroutine")
    val dispatcher = Dispatchers.IO 
    val coroutineContext = name + dispatcher
  
    println(coroutineContext[name.key])
    println(coroutineContext[dispatcher.key])
}
```

➡️ 안정된 API, 경고 없음

<br>

### 4. CoroutineContext 요소 제거

- `minusKey()` 사용
- `minusKey` 함수를 호출한 `CoroutineContext` 객체는 유지되고, 구성 요소가 제거된 새로운 `CoroutineContext` 객체가 반환된다

```kotlin
fun main() = runBlocking {
    val coroutineName = CoroutineName("MyCoroutine")
    val dispatcher = Dispatchers.IO
    val job = Job()
    val coroutineContext = coroutineName + dispatcher + job
  
    val deletedCoroutineContext = coroutineContext.minusKey(coroutineName)
 
    println("coroutineContext": $coroutineContext)
    println("deletedCoroutineContext": $deletedCoroutineContext)
}
```

#### <br>

## 2️⃣ 예외 처리

### 5. 예외 전파

- 코루틴에서 예외 발생 → 해당 코루틴이 취소된다
- 예외가 부모 코루틴으로 전파된다
- 부모 코루틴이 예외를 처리하지 않으면 상위로 계속 전파된다
- 결국 전체 코루틴 계층이 취소될 수 있다

<br>

### 6. 예외 전파 제한 방법

#### 구조화를 깨는 방법 (비권장)

- 코루틴의 구조화를 깨면 예외 전파를 제한할 수 있다
-  `Job` 객체를 새로 만들어 구조화를 깨고 싶은 코루틴에 연결하면 구조화가 깨진다
- 하지만 코루틴의 구조화가 깨지면, 예외 전파 뿐만 아니라 취소 전파도 제한되는 문제가 있다

#### SupervisorJob 사용

- `SupervisorJob` 객체는 자식 코루틴으로부터 예외를 전파 받지 않는 특수한 `Job` 객체다

- `SupervisorJob` 객체는 자식 코루틴에서 발생한 예외가 다른 자식 코루틴에게 영향을 미치지 못하게 만드는데 사용된다

```kotlin
fun main() = runBlocking {
    val supervisorJob = SupervisorJob(parent = this.coroutineContext[Job])
    ...
    supervisorJob.complete()  // supervisorJob 완료 처리
}
```

➡️ 주의할 점 : `SupervisorJob()`은 자동 완료되지 않기 때문에 실행 코드의 마지막에 완료시켜줘야 한다

#### supervisorScope 사용 (권장)

- `supervisorScope` 함수는 `SupervisorJob` 객체를 가진 `CoroutineScope` 객체를 생성한다
- `supervisorScope` 함수를 통해 생성된 `SupervisorJob` 객체는 `supervisorScope` 함수를 호출한 코루틴을 부모로 가진다
- `supervisorScope` 함수를 통해 생성된 `SupervisorJob` 객체는 코드가 모두 실행되고 자식 코루틴의 실행도 완료되면 자동으로 완료된다

```kotlin
fun main() = runBlocking {
    supervisorScope {
        launch(CoroutineName("Coroutine1")) {
            throw Exception("예외 발생")
        }
        launch(CoroutineName("Coroutine2")) {
            ...
        }
    }
}
```

➡️ 복잡한 설정 없이 구조화를 깨지 않고 예외 전파를 제한할 수 있다 (실패한 코루틴만 취소됨)

<br>

### 7. CoroutineExceptionHandler

- `CoroutineExceptionHandler`는 `CoroutineContext`의 구성 요소 중 하나다
- `CoroutineExceptionHandler`는 처리되지 않은 예외만 처리한다
- `CoroutineExceptionHandler`는 `launch` 코루틴으로 시작되는 코루틴 계층의 공통 예외 처리기로 동작하는 구성요소다
- 만약 `launch` 코루틴이 다른 `launch` 코루틴으로 예외를 전파하면, 예외가 처리된 것으로 보기 때문에 자식 코루틴에 설정된 `CoroutineExceptionHandler`는 동작하지 않는다

#### CoroutineExceptionHandler가 사용되는 경우

- 예외를 로깅하거나, 오류 메시지를 표시하기 위해 구조화된 코루틴들에 공통으로 동작하는 예외 처리기를 설정해야 하는 경우 사용된다

```kotlin
fun main() = runBlocking {
    val exceptionHandler = CoroutineExceptionHandler { coroutineContext, throwable ->
        println("[예외 로깅] #{throwable}")
    }
  
    CoroutineScope(Dispatchers.IO)
        .launch(CoroutineName("Coroutine1") + exceptionHandler) {
            launch(CoroutineName("Coroutine2")) {
                throw Exception("Coroutine2에 예외가 발생했습니다")
            }
            launch(CoroutineName("Coroutine3")) {
                // 다른 작업
            }
        }                                     
}
```

<br>

### 8. try catch 사용한 예외 처리

- try catch 문을 사용하면 코루틴에서 발생한 예외를 처리할 수 있다
- try catch 문을 launch 함수의 람다식 내부에서 사용해야 한다

```kotlin
fun main() = runBlocking {
    launch(CoroutineName("Coroutine1")) {
        try {
            throw Exception("Coroutine1에 예외가 발생했습니다")
        } catch (e: Exception) {
            println(e.message)
        }
    }
    launch(CoroutineName("Coroutine2")) {
        delay(100L)
        println("Coroutine2 실행 완료")
    }                             
}
```

#### try catch 문을 잘못 사용하는 경우

- 코루틴 빌더 함수에 try catch 문을 사용하면 예외를 잡지 못한다
- launch 함수를 try catch 문으로 감싸면 try catch 문은 코루틴이 잘 생성되는지만 확인한다

```kotlin
try {
    launch { ... }
} catch (e: Exception) { }
```

<br>

### 9. async 예외 처리

#### async의 예외 노출

- `async`는 예외를 즉시 던지지 않는다
- `await` 호출 시 예외가 노출된다

```kotlin
fun main() = runBlocking {
    supervisorScope {
        val deferred: Deferred<String> = async(CoroutineNmae("Coroutine1")) {
            throw Exception("Coroutine1에 예외가 발생했습니다")
        }
        try {
            deferred.await()
        } catch (e: Exception) {
            println("[노출된 예외] ${e.message}")
        }
    }                        
}
```

#### async의 예외 전파

- `async`도 부모 코루틴으로 예외를 전파한다
- `await`만 감싸면 전파된 예외는 놓칠 수 있다

- `supervisorScope`와 함께 사용하는 것이 안전하다
- 즉 `async` 코루틴 빌더를 사용할 때는 전파된 예외와 노출된 예외를 모두 처리해줘야 한다

```kotlin
fun main() = runBlocking {
    supervisorScope {
        async(CoroutineNmae("Coroutine1")) {
            throw Exception("Coroutine1에 예외가 발생했습니다")
        }
        launch(CoroutineNmae("Coroutine2")) {
            delay(100L)
            println("[${Thread.currentThread().name}] 코루틴 실행")
        }
    }                        
}
```

➡️ Coroutine1에서 발생한 예외가 부모로 전파되지 않기 때문에 프로세스가 강제 종료되지 않고, Coroutine2의 출력이 정상 실행된다

<br>

### 10. 전파되지 않는 예외

- `CancellationException`은 부모 코루틴으로 전파되지 않는다
- `CancellationException`은 코루틴의 취소에 사용되는 특별한 예외이기 때문에 전파되지 않는다
