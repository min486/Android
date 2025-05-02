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

## 🔥 코루틴 (Coroutines)

### 코루틴

- 경량 스레드 기반의 비동기 프로그래밍 도구
- `suspend` 키워드로 중단 가능한 함수 작성 가능
- 복잡한 콜백 없이도 비동기 처리 가능

<br>

### 코루틴이 필요한 이유

- 메인 스레드 블로킹 방지
- UI 반응성 유지 ➡️ ANR 방지
- 네트워크/DB 등 I/O 작업을 별도 스레드에서 수행

<br>

### 동기와 비동기

동기와 비동기는 어떤 작업이 수행되는 방식을 나타낸다

| 개념                  | 설명                                                         |
| --------------------- | ------------------------------------------------------------ |
| 동기 (Synchronous)    | 요청과 응답이 순차적으로 일어남                              |
| 비동기 (Asynchronous) | 요청과 응답이 동시에 일어나지 않음, 응답 기다리지 않고 다음 작업 수행 가능 |

<br>

### 코루틴과 스레드

*Process : 보조기억장치의 '프로그램'이 메모리 상으로 적재되어 실행되면 '프로세스'가 된다

*Thread : 같은 process 내에서 실행되는 여러 작업 (흐름)의 단위

*비동기 작업을 수행한다는 점에서 비슷한 일을 하지만 아래와 같은 차이점들이 있다

| 항목          | 코루틴                    | 스레드             |
| ------------- | ------------------------- | ------------------ |
| 실행 단위     | 함수 수준                 | 운영체제 수준      |
| 메모리 구조   | 공유                      | 할당               |
| 메모리 사용   | 작음 (경량)               | 큼                 |
| 스케줄링 방식 | 비선점형 (직접 양보 필요) | 선점형 (OS가 전환) |
| 동시에 실행   | ❌ (동시성)                | ⭕ (병행성 가능)    |

<br>

<img src="../README.assets/coroutine.png" alt="coroutine" align="center" width="40%" />

👉 Process는 독립된 메모리 영역인 힙을 할당 받는다

👉 Thread는 process 하위에 종속되는 보다 작은 단위다

👉 Thread는 독립된 메모리 영역인 스택 (Stack)을 갖는다

👉 Thread를 하나 생성하면, 하나의 스택 메모리가 생기는 것이다

👉 각 Thread는 다른 Thread에게 스택 메모리를 공유할 수 없다

<br>

### 코루틴의 스레드 작업 최적화

코루틴은 Thread 하나를 일시중단 가능한 다중 경량 Thread 처럼 활용한다

<img src="../README.assets/coroutine2.png" alt="coroutine2" align="center" width="80%" />

👉 코루틴은 Thread 안에서 실행되는 일시 중단 가능한 작업의 단위

👉 하나의 Thread에 여러 코루틴이 존재할 수 있다

<br>

### 코루틴 구성요소

- Coroutine Scope

  - 코루틴의 생명주기를 관리
  - 예 :  `viewModelScope`, `lifecycleScope`

- Coroutine Context

  - 코루틴의 실행환경 (스레드, Job 등 포함)

  - 주요 Dispatcher

    - 코루틴이 실행되는 스레드를 지정, 용도마다 사용되는게 다르다

    | Dispatcher | 설명                  | 사용 시점                  |
    | ---------- | --------------------- | -------------------------- |
    | Main       | UI 작업               | UI 변경                    |
    | IO         | 네트워크, 파일        | 비동기 I/O                 |
    | Default    | CPU 연산              | 데이터 처리, 정렬          |
    | Unconfined | 현재 호출 스레드 사용 | 일반적인 용도에서는 사용 ❌ |

  - Job, Deferred : 코루틴 작업의 상태 관리 도구

    - Job

      코루틴의 생명주기를 관리하는 객체로, 취소(cancel), 완료 상태 확인(isActive, isCompleted, isCancelled) 등의 기능을 제공

      👉 `launch`가 반환

    - Deferred

      Job의 하위 클래스이며, 결과 값을 반환할 수 있는 Job

      👉 `async`가 반환

      👉 `await()`을 통해 결과를 비동기적으로 얻을 수 있음

- Coroutine Builder

  - 코루틴 시작 도구 (일반적으로 launch로 코루틴 시작)

  -  launch (코루틴 처리만) / async (코루틴 처리 + 값 반환)

    | Builder     | 반환 타입                            | 용도                          |
    | ----------- | ------------------------------------ | ----------------------------- |
    | launch      | Job                                  | 결과 필요 없을 때             |
    | async       | Deferred                             | 결과 필요할 때 (await)        |
    | runBlocking | 사용 ❌ (main 스레드 block 하기 때문) | 테스트/메인 함수 외 사용 금지 |
    | withContext | context 변경                         | Dispatcher 전환시 사용        |

<br>

### Scope 종류

| Scope                  | 대상               | 자동 취소 시점             |
| ---------------------- | ------------------ | -------------------------- |
| viewModelScope         | ViewModel          | ViewModel cleared 시점     |
| lifecycleScope         | Activity/Fragment  | Lifecycle destroyed 시점   |
| rememberCoroutineScope | Compose Composable | Composition destroyed 시점 |

<br>

### 코루틴 예시

```kotlin
// ViewModel
viewModelScope.launch(Dispatchers.IO) {
    val data = repository.loadData()
    withContext(Dispatchers.Main) {
        _uiState.value = data
    }
}
```

<br>

### Compose 관련 코루틴

[Side Effect (부수 효과)](https://github.com/min486/Android/tree/master/Jetpack_Compose/Side_Effect)

👉 LaunchedEffect, DisposableEffect, rememberCoroutineScope

