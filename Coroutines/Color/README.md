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




## 🔥 CoroutineDispatcher

### CoroutineDispatcher

> 코루틴이 어떤 스레드에서 실행될지 결정하는 객체

- 코루틴을 어디에서 실행할지 결정
- 실행 요청된 코루틴을 적절한 스레드에 디스패치 하는 역할

- Coroutine + Dispatcher → 코루틴을 보내는 주체


<br>

### 1. CoroutineDispatcher 동작 방식

코루틴이 실행될 때의 흐름은 아래와 같다

1. 개발자가 특정 CoroutineDispatcher에 코루틴 실행을 요청
2. 디스패처는 해당 코루틴을 작업 대기열(queue)에 등록
3. 사용 가능한 스레드가 있으면 코루틴을 해당 스레드로 보내 실행

<br>

### 2. CoroutineDispatcher 종류

#### Dispatcher.IO

- 입출력(IO) 작업을 위한 디스패처
  - 네트워크 요청
  - DB 읽기 / 쓰기
  - 파일 입출력
- 많은 동시 IO 작업을 처리하기 위해 확장 가능한 스레드풀을 사용
  - 기본적으로 최대 64개 스레드까지 사용 가능

<br>

#### Dispatcher.Default

- CPU 바운드 작업을 위한 디스패처
- 지속적인 연산이 필요한 작업에 적합
  - 이미지 처리
  - 대용량 데이터 변환
  - 복잡한 계산 로직

<br>

#### Dispatcher.Main

- 메인 스레드(UI 스레드) 에서 실행되는 디스패처

<br>

#### Dispatchers.Unconfined

- 특정 스레드에 고정되지 않는 디스패처
- 실행 스레드가 예측하기 어렵기 때문에 일반적은 Android 앱에서는 거의 사용하지 않음

<br>

### 3. 코루틴 라이브러리의 공유 스레드풀

코루틴 라이브러리는 스레드의 생성과 관리를 효율적으로 할 수 있도록 공유 스레드풀을 사용한다

#### Dispatcher.Default의 limitedParallelism

- CPU 바운드 작업에서 동시에 실행할 스레드 수를 제한
- 기존 Default 스레드풀 내에서 병렬성만 제한

```kotlin
val imageDispatcher = Dispatchers.Default.limitedParallelism(2)

viewModelScope.launch(imageDispatcher) {
    processImage()
}
```

➡️ 이미지 처리 요청이 많아도 최대 2개 스레드만 사용

<br>

#### Dispatcher.IO의 limitedParallelism

- `Dispatcher.Default`와 다르게 동작함
- IO 디스패처의 일부를 제한하는 것이 아니라, 별도의 전용 스레드풀을 새로 생성
- Dispatcher.IO의 limitedParallelism 사용이 적합한 경우
  - 다른 IO 작업에 방해받지 않아야 하는 중요한 작업
  - 예시 : 메시저 앱의 메시지 싱크 작업

```kotlin
val syncDispatcher = Dispatchers.IO.limitedParallelism(2)
```

<br>

### 4. Dispatcher.Main과 Dispatcher.Main.immediate 차이

Android의 `lifecycleScope` 또는 `viewModelScope` 내부에서는 `Dispatcher.Main.immediate`가 사용된다

즉, 별도의 Dispatcher를 지정하지 않고 launch를 사용하면 기본적으로 `Main.immediate`에서 실행된다

#### Dispatcher.Main

- 어떤 스레드에서 실행 요청하든
  - 작업 대기열에 먼저 등록
  - 메인 스레드가 비었을 때 실행
- 대기열에 작업이 많으면 UI 반응이 늦어질 수 있음

<br>

#### Dispatcher.Main.immediate

- 이미 메인 스레드에서 실행 중인 경우
  - 대기열에 넣지 않고
  - 즉시 메인 스레드에서 실행
- 불필요한 디스패치 과정 생략
  - UI 업데이트 지연 최소화
  - Android UI 작업에 더 적합

```kotlin
viewModelScope.launch(Dispatchers.Main.immediate) {
    _uiState.value = UiState.Success
}
```
