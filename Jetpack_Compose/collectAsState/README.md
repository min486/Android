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




## 🔥 collectAsState / collectAsStateWithLifecycle 차이

### collectAsState()

`collectAsState()`는 flow에서 값을 수집하고 최신 값을 compose state로 나타내는 composable 함수다

새로운 flow 방출이 발생할 때마다 state object가 업데이트 되어 composition의 모든 state.value 사용이 recomposition된다

(View 쪽에서 사용)

<br>

특징

- Composition의 수명 주기를 따르며, 

  composable 함수가 composition에 들어갈 때 flow 수집을 시작하고 composition을 떠날 때 중지된다

- Android 앱이 백그라운드에 있을 때에도 

  flow 수집을 활성 상태로 유지하므로 불필요한 리소스 낭비가 발생할 수 있다

<br>

### collectAsStateWithLifecycle()

특징

- 수명 주기를 인식하는 방식으로 flow를 수집하여 필요하지 않을 때 앱 리소스를 확보하는데 도움이 된다
- 기본적으로 앱이나 화면이 백그라운드에 있을 때 flow 수집을 취소하여 더 나은 리소스 관리를 가능하게 한다

<br>

### 차이점 비교

```kotlin
class ExampleViewModel : ViewModel() {
    private val _uiStateFlow = MutableStateFlow(0)
    val uiStateFlow: StateFlow<Int> = _uiStateFlow

    init {
        viewModelScope.launch {
            // Simulate data generation
            while (true) {
                delay(1000)
                _uiStateFlow.value += 1
            }
        }
    }
}
```

<br>

`collectAsState()` 사용하면

Flow 수집은 앱이 백그라운드에 있을 때에도 활성 상태를 유지하여 리소스를 낭비한다

```kotlin
@Composable
fun ExampleScreen(viewModel: ExampleViewModel) {
    val uiState by viewModel.uiStateFlow.collectAsState()

    Text("Current value: $uiState")
}
```

<br>

`collectAsStateWithLifecycle()` 사용하면

앱이 백그라운드로 전환되면 flow 수집이 자동으로 중지되어 리소스가 절약된다

```kotlin
@Composable
fun ExampleScreenWithLifecycle(viewModel: ExampleViewModel) {
    val uiState by viewModel.uiStateFlow.collectAsStateWithLifecycle()

    Text("Current value: $uiState")
}
```

<br>

### 결론

Android 앱 개발의 경우 `collectAsStateWithLifecycle()`를 사용하면

Android 수명주기에 맞춰서 불필요한 리소스 낭비를 방지할 수 있다

`collectAsState()`는 다른 플랫폼으로 작업할 때나 Android 수명주기 문제가 관련이 없을 때 사용 가능하다
