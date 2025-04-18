<div align="center">
  <p>
    <img src="../README.assets/jetpack-hero.png">
  </p>
  <br>
  <h2>Jetpack Compose</h2>
  <p>Jetpack Compose 관련 내용 정리</p>
  <br>
  <br>
</div>




## 🔥 Jetpack Compose에서 Flow 활용

### Flow 기본 개념

- Flow

  - 코틀린의 비동기 데이터 스트림 처리 도구

  - 코루틴 기반으로 작동하며, 순차적/비동기적으로 데이터를 발행

  - 일반적으로 suspend 함수가 1개의 값을 반환하는 반면, Flow는 여러 값을 순차적으로 제공

- Cold Stream / Hot Stream

  - Cold Stream
    - 구독(collect)할 때마다 새로 시작함
    - 각 구독자는 독립적으로 데이터를 받음
    - 예시 : flow { emit(...) }
  - Hot Stream
    - 이미 실행중인 스트림에 구독자가 붙음
    - 여러 구독자가 동일한 데이터 소스를 공유
    - 예시 : StateFlow, SharedFlow

- suspend fun과 Flow 차이

  | 항목      | suspend fun               | Flow                         |
  | --------- | ------------------------- | ---------------------------- |
  | 반환 값   | 단일 값                   | 여러 값 (연속적으로)         |
  | 처리 방식 | 동기적 / 1회성            | 비동기적 / 스트리밍          |
  | 예시      | `suspend fun fetchUser()` | `fun getUsers(): Flow<User>` |

​	👉 Flow는 suspend fun 으로는 처리하기 어려운 변화하는 데이터 흐름에 적합함

- Compose에서 Flow를 사용하는 이유

  - Flow는 계속 변하는 데이터를 다루고, 

    Compose는 그런 변화에 자동으로 반응해서 화면을 다시 그려주기 때문에 함께 쓰기 좋다

  - `collectAsState()`를 통해 UI에서 자동으로 최신 상태를 감지하고 리컴포지션 수행
  - Flow는 UI뿐만 아니라 Repository ➡️ ViewModel ➡️ UI 구조에서 데이터 흐름을 명확하게 해줌

<br>

### StateFlow & MutableStateFlow

- 일반 Flow와의 차이

  | 항목         | Flow      | StateFlow                 |
  | ------------ | --------- | ------------------------- |
  | 초기 값      | 없음      | 반드시 필요               |
  | 최신 값 저장 | ❌         | `value`로 접근 가능       |
  | Compose 연동 | 다소 불편 | `collectAsState()`로 가능 |

- Compose와 사용하는 이유

  - StateFlow는 항상 최신 값을 가지고 있어서, Compose가 화면을 다시 그릴 때 필요한 정보를 바로 받아올 수 있음

    👉 Compose가 자동으로 화면을 업데이트하기에 좋다

  - collectAsState() 를 통해 UI는 자동으로 상태를 구독하고 리컴포지션함
  - LiveData 없이도 ViewModel ➡️ Compose 상태 전달이 자연스럽게 이뤄짐

- ViewModel에서 주로 StateFlow를 쓰는 이유

  - `val _state = MutableStateFlow(...)` ➡️ 내부에서만 수정 가능
  - `val state = StateFlow<...> = _state` ➡️ 외부에서 수정 불가
  - 즉 ViewModel 에서 상태를 안전하게 읽기 전용으로 노출 가능

<br>

### collectAsState(), collectAsStateWithLifecycle()

- collectAsState가 리컴포지션 되는 시점

  - Flow(StateFlow)의 값이 변경되면, `collectAsState()`는 이를 감지하고 자동으로 최신 값을 수집해 리컴포지션함

  - 즉 `collectAsState()`를 사용하면, `StateFlow`가  `remember`와 `mutableStateOf` 처럼 작동하여,

    값을 기억하고 값이 변경될 때 UI를 자동으로 다시 그려준다

  ```kotlin
  val uiState by viewModel.stateFlow.collectAsState()
  ```


- collectAsStateWithLifecycle 사용 이유

  -  `collectAsState()`는 생명주기를 고려하지 않기 때문에, 생명주기 관리가 어려움

    👉 onStart 이전에도 collect가 이뤄질 수 있음 (불필요한 리소스 낭비 발생 가능)

  - `collectAsStateWithLifecycle()`은 화면이 활성화된 시점에만 collect 한다

    👉 생명주기 안정성을 확보함 (불필요한 리소스 낭비 방지)

    👉 생명주기 상태에 따라 collect를 자동으로 관리해줘서 안전함

  ```kotlin
  // 생명주기를 고려한 안전한 수신
  val uiState by viewModel.stateFlow.collectAsStateWithLifecycle()
  ```

<br>

### StateFlow와 SharedFlow 차이

둘은 Flow의 하위 타입이면서, 실제 사용하는 목적이 구분된다

| 항목         | StateFlow                    | SharedFlow                          |
| ------------ | ---------------------------- | ----------------------------------- |
| 상태 저장    | ⭕️ (초기값 필요)              | ❌                                   |
| 최신 값 유지 | ⭕️                            | ❌ (재발행 없음 기본값)              |
| 기본 용도    | UI 상태 전달                 | 이벤트 처리                         |
| Compose 사용 | `collectAsState()`로 값 관찰 | `LaunchedEffect` + `collect`로 처리 |

👉 지속적으로 변화하는 UI 상태에서 ➡️  collectAsState() 사용해서 UI 처리

👉 일회성 이벤트(Toast, Navigation 등)에서 ➡️ LaunchedEffect + collect 사용해서 UI 처리

👉 즉 UI 상태는 StateFlow로 관리하고, 이벤트는 SharedFlow로 처리한다

<br>

- SharedFlow 사용 예시

  - 토스트 메시지, 네비게이션 등의 이벤트처럼 일회성 이벤트 처리에 적합
  - Compose에서는 `LaunchedEffect(key)`와 함께 사용

  ```kotlin
  LaunchedEffect(Unit) {
      viewModel.eventFlow.collect { event ->
          when (event) {
              is UiEvent.ShowToast -> {
                  Toast.makeText(context, event.message, Toast.LENGTH_SHORT).show()
              }
          }
      }
  }
  ```

  
