<div align="center">
  <p>
    <img src="../README.assets/kotlin-hero.png">
  </p>
  <br>
  <h2>Kotlin</h2>
  <p>코틀린 관련 내용 정리</p>
  <br>
  <br>
</div>

## 🔥 Coroutine (코루틴)

### 코루틴 정의

> 비동기적으로 실행되는 코드를 간소화하기 위해 사용할 수 있는 동시 실행 설계 패턴

<br>

### 코루틴 특징

- 경량화
- 메모리 누수 감소
- 계층적 구조를 통한 취소 전달
- Jatpack과의 통합

<br>

### 코루틴 관련 개념

- AsyncTask
  - 손쉬운 비동기 프로그래밍
  - 메모리 누수 등 여러가지 문제
  - deprecated @API 30
  - 대체재로 코루틴을 권장
-  코루틴
  - 여러 프로그래밍 언어에 구현이 되어 있다
  - 코틀린만의 고유한 개념은 아니다
- 루틴
  - 메인루틴
  - 서브루틴
  - 코루틴

<br>

### 코루틴과 스레드

*Process : 보조기억장치의 '프로그램'이 메모리 상으로 적재되어 실행되면 '프로세스'가 된다

*Thread : 같은 process 내에서 실행되는 여러 작업 (흐름)의 단위

*비동기 작업을 수행한다는 점에서 비슷한 일을 하지만 아래와 같은 차이점들이 있다

- 메모리 구조의 차이

  - 공유 (코루틴) 
  - 할당 (스레드)

- 코루틴의 장점 (메모리 구조의 차이에서 오는)

  - 사용되는 메모리가 적어진다

    : 분리된 메모리를 사용하는 것이 아닌, Process가 가진 Heap 메모리 안에서 코루틴들이 돌아간다

    👉 코루틴들이 메모리를 공유할 수 있다

- 수행방식의 차이

  - 비선점형 (코루틴)

    : 동시에 돌아가는 코루틴 2개가 있을 때, 동일한 시간에 2개의 코루틴이 같이 실행되지 않는다

    👉 코루틴이 전환되면서 수행되는 속도가 매우 빠르기 때문에, 외부에서 볼때는 코루틴 2개가 동시에 수행되는 것처럼 보인다 

    (동시성 ⭕️, 병행성 ❌)

  - 선점형 (스레드)

    : 동시에 돌아가는 스레드가 2개가 있을 때, 동일한 시간에 동시에 2가지 작업을 할 수 있다 (병행성 ⭕️)

<img src="../README.assets/process.png" alt="process" align="center" width="40%" />

👉 Process는 독립된 메모리 영역인 힙을 할당 받는다

👉 Thread는 process 하위에 종속되는 보다 작은 단위다

👉 Thread는 독립된 메모리 영역인 스택 (Stack)을 갖는다

👉 Thread를 하나 생성하면, 하나의 스택 메모리가 생기는 것이다

👉 각 Thread는 다른 Thread에게 스택 메모리를 공유할 수 없다

<br>

### 코루틴 구조

- Coroutine Scope
- Coroutine Context
- Coroutine Builder

<br>

### 각 구조 상세내용

- Coroutine Scope

  > 코루틴이 동작하는 범위를 규정

```kotlin
// CoroutineContext 하나만 멤버 변수로 정의하고 있는 인터페이스
public interface CoroutineScope {
  public val coroutincContext: CoroutineContext
}
```

- Coroutine Context

  - Dispatchers

    > 코루틴이 실행되는 스레드를 지정, 용도마다 사용되는게 다르다

    - Default

      : CPU 연산 작업이 많으면 사용

    - IO

      : 파일이나 네트워크 I/O 작업이 많으면 사용

    - Main

      : UI 스레드 (main 스레드)에서 UI 관련된 변경을 해야할 때 사용

    - Unconfined

      : 일반적인 용도에서는 사용 ❌

  - Job & Deferred

    > 코틀린에서는 코루틴이라는 추상적인 흐름 (한 덩어리)을 Job이라고 하는 Object로 만들어서 사용
    >
    > 👉 object에 대해서 취소 or 예외처리를 함으로써 코루틴 흐름 제어를 쉽게 할 수 있다

    *Deferred : 결과 값을 가지는 Job

    - States

    - methods

      : cancel, join, start

  <img src="../README.assets/job.png" alt="job" align="center" width="50%" />

  <img src="../README.assets/job2.png" alt="job2" align="center" width="50%" />

- Coroutine Builder

  > 일반적으로 launch로 코루틴 시작

  - launch

    : Job 객체 반환, 코루틴 작업을 하고 끝 

  - async

    : Deferred 객체 반환, 코루틴 작업 + 마지막에 나온 값 (Deferred 형태의 object) 반환 

  - runBlocking

    : 사용 ❌ (main 스레드를 block 하기 때문에)

  - withContext

    : Dispatcher를 전환하는 기능

<br>

### 코루틴 정리

> 코틀린에서 코루틴을 사용하기 위해서
>
> Coroutine Scope ➡️ Coroutine Context ➡️ Coroutine Builder를 사용해서 코루틴 객체를 만든다

✅ Scope

: Coroutine Scope 사용

✅ Context

: Default (CPU 작업) or IO (IO 작업) 사용

✅ Builder

: launch (코루틴 처리만) or async (코루틴 처리 + 값 반환)
