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

## 🔥 Coroutines (코루틴)

### 동기 vs 비동기 및 스레드

- 동기 (Synchronous)
  - 작업이 순차적으로 진행
  - 한 작업이 끝날 때까지 다음 작업은 대기
- 비동기 (Asynchronous)
  - 작업을 시작하고 결과를 기다리지 않고 바로 다음 작업을 시작
  - 결과는 나중에 콜백 등을 통해 받음
  - Android에서는 UI 스레드의 블로킹 방지를 위해 필수적
- 스레드 (Thread)
  - 코드가 실제로 실행되는 경로
  - OS에서 관리하는 최소 실행 단위
  - 코루틴은 이 스레드 위에서 실행됨

<br>

### 코루틴

> 비동기 작업을 간결하고 효율적으로 처리하기 위한 Kotlin 기반 경량 스레드 기술

- OS 스레드보다 가볍고 중단/재개가 가능하며,
- 구조적 동시성을 통해 생명주기에 맞게 관리된다

<br>

### 코루틴 주요 특징

- 경량성
  - OS 스레드가 아닌 Kotlin 수준의 가벼운 실행 단위
- 중단 가능 (suspend)
  - 스레드를 블로킹하지 않고 작업을 일시 중단했다가 조건이 충족되면 재개한다
- 구조적 동시성 (structured concurrency)
  -  CoroutineScope 기반으로 부모 코루틴이 취소되면 자식 코루틴도 자동으로 취소되어 메모리 누수를 방지한다
- 콜백 지옥 제거
  - suspend 함수 등을 사용해 비동기 코드를 동기 코드처럼 순차적으로 작성할 수 있다
- Lifecycle 연계
  - viewModelScope, lifecycleScope 등 컴포넌트 생명주기에 맞게 코루틴을 안전하게 관리할 수 있는 scope를 제공한다

<br>

### 코루틴 핵심 개념

- suspend 함수
  - 코루틴 내에서만 호출 가능한 함수
  - 블로킹 없이 작업을 일시중지 및 재개할 수 있도록함
- CorountineScope
  - 코루틴의 생명주기를 결정하고 관리하는 역할
  - 이 scope가 취소되면 내부의 모든 코루틴이 취소된다 (예: `viewModelScope`)
- Job
  - 실행 중인 코루틴 단위
  - 코루틴의 생명주기를 제어하고 상태를 추적할 수 있다
- Dispatcher
  - 코루틴이 실행될 스레드를 결정한다
  - `Dispatcher.Main` (UI 상호작용 및 빠른 작업)
  - `Dispatcher.IO` (네트워크, 파일 입출력 등 I/O 작업)
  - `Dispatcher.Default` (CPU 집약적 작업)

<br>

### 라이브러리 구성

프로젝트 전체에서 코루틴 버전을 통일하여 관리하고 유지보수성을 높인다

<br>

의존성 추가 (`libs.versions.toml` 파일)

```toml
[versions]
coroutines = "1.10.2"

[libraries]
coroutines-core = { module = "org.jetbrains.kotlinx:kotlinx-coroutines-core", version.ref = "coroutines" }
coroutines-android = { module = "org.jetbrains.kotlinx:kotlinx-coroutines-android", version.ref = "coroutines" }
```

<br>

라이브러리 역할

1. `kotlinx-coroutines-core` (코어 라이브러리)
   - 역할 : 코루틴 기본 기능 제공
   - 주요 기능
     - 코루틴 빌더 : launch, async, runBlocking 등 코루틴을 시작하는 함수
     - suspend 함수 : delay, withContext 등 코루틴의 흐름을 제어하는 함수
     - Dispatcher
2. `kotlinx-coroutines-android` (Android 특화 라이브러리)
   - 역할 : Android UI 처리용 Dispatcher 제공
   - 주요 기능
     - Dispatcher.Main을 제공하여 UI 스레드와의 상호작용을 가능하게 한다

<br>

버전 참고

1. GitHub

   https://github.com/Kotlin/kotlinx.coroutines

2. Maven Repository

   https://mvnrepository.com/artifact/org.jetbrains.kotlinx/kotlinx-coroutines-core

   https://mvnrepository.com/artifact/org.jetbrains.kotlinx/kotlinx-coroutines-android

<br>

### 사용 예시

- ViewModel에서 UI 상태 처리

  `viewModelScope.launch`를 사용하여 비동기 작업을 안전하게 시작하며,

  구조적 동시성을 통해 `ViewModel`이 소멸(`onCleared()`)될 때 코루틴을 자동으로 취소한다 → 메모리 누수 방지

  ```kotlin
  @HiltViewModel
  class HomeViewModel @Inject constructor(
      // suspend 함수 호출 가정
      private val getHomeBurgersUseCase: GetHomeBurgersUseCase 
  ) : ViewModel() {
  
      // MutableStateFlow를 사용하여 Compose에서 관찰 가능한 UI 상태 관리
      private val _uiState = MutableStateFlow<HomeUiState>(HomeUiState.Loading)
      val uiState: StateFlow<HomeUiState> = _uiState.asStateFlow()
  
      init { loadHomeData() }
  
      private fun loadHomeData() = viewModelScope.launch {
          // Dispatcher.Main에서 시작 (로딩 상태 업데이트)
          _uiState.value = HomeUiState.Loading
  
          try {
              // 작업 수행
              val burgers = getHomeBurgersUseCase()
              
              // 작업 완료 후 Dispatcher.Main에서 UI 상태를 Success로 업데이트
              _uiState.value = HomeUiState.Success(burgers)
          } catch (e: Exception) {
              // 예외 발생 시 Dispatcher.Main에서 에러 처리
              _uiState.value = HomeUiState.Error(e.message ?: "데이터 로드 실패")
          }
      }
  }
  ```
