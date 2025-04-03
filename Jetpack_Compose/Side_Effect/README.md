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




## 🔥 Side Effect (부수 효과)

### Side Effect

> Composable 범위를 벗어나 앱 상태를 변경하는 것

<br>

- Composable은 각각의 Lifecycle을 가진다
- Composable은 단방향으로만 상태(State)를 전달한다

<br>

👉 기본적으로 바깥쪽 Composable이 안쪽 Composable로 상태를 내려주며, 이는 단방향 데이터 흐름을 따른다.

그러나 안쪽 Composable이 바깥쪽 Composable의 상태를 변경하거나,
Composable에서 앱의 전역 상태를 수정하면, 양방향 의존성이 발생하여 예측할 수 없는 부수 효과가 생길 수 있다.

<br>

### LaunchedEffect

Composable이 Composition에 들어갈 때 한 번만 실행되며, key가 변경되면 이전 코루틴이 취소되고 다시 실행된다.

비동기 작업을 안전하게 처리할 때 유용하다.

- 사용처

  - 화면 진입시 데이터 로딩
  - 이벤트 기반 로직 처리

- 예제

  ```kotlin
  @Composable
  fun UserProfile(userId: String, viewModel: UserViewModel) {
      LaunchedEffect(userId) {
          viewModel.loadUserData(userId)
      }
  }
  ```


<br>

### DisposableEffect

Composable이 Composition에 들어갈 때 효과를 설정하고, 컴포지션에서 나갈 때(또는 키가 변경될 때) onDispose 블록의 정리 로직을 실행한다.

리소스 관리가 필요한 작업에 적합하다.

- 사용처

  - 이벤트 리스너 등록/해제
  - 시스템 리소스 할당/해제

- 예제

  ```kotlin
  @Composable
  fun NetworkObserver() {
      val context = LocalContext.current
      DisposableEffect(Unit) {
          val connectivityManager = context.getSystemService(Context.CONNECTIVITY_SERVICE) as ConnectivityManager
          val networkCallback = object : ConnectivityManager.NetworkCallback() {
              // 네트워크 콜백 구현
          }
          connectivityManager.registerDefaultNetworkCallback(networkCallback)
          
          onDispose {
              connectivityManager.unregisterNetworkCallback(networkCallback)
          }
      }
  }
  ```

<br>

### rememberCoroutineScope

Composable 함수 내에서 Composition 범위 밖에서 코루틴을 시작할 수 있게 해주는 코루틴 스코프.

Composable의 Lifecycle에 맞춰 자동으로 취소된다.

- 사용처

  - 사용자 이벤트(클릭 등)에 응답하여 코루틴 실행

  - ViewModel 함수 호출
  - SnackBar 표시와 같은 일회성 작업

- 예제

  ```kotlin
  @Composable
  fun LoginScreen(viewModel: LoginViewModel) {
      val coroutineScope = rememberCoroutineScope()
      var username by remember { mutableStateOf("") }
      var password by remember { mutableStateOf("") }
      
      Column {
          Button(onClick = {
              coroutineScope.launch {
                  viewModel.login(username, password)
              }
          }) {
              Text("로그인")
          }
      }
  }
  ```

  

