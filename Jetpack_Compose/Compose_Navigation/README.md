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




## 🔥 Compose Navigation

### 의존성 추가

Compose BOM을 사용하면 대부분의 Compose 라이브러리 버전을 자동으로 관리할 수 있다

- `libs.versions.toml`

  ```toml
  [versions]
  composeBom = "2025.05.00"
  
  [libraries]
  androidx-compose-bom = { group = "androidx.compose", name = "compose-bom", version.ref = "composeBom" }
  
  # --- BOM에 포함된 Jetpack Compose 관련 라이브러리 ---
  androidx-navigation-compose = { module = "androidx.navigation:navigation-compose" }
  ```

- `app/build.gradle.kts`

  ```kotlin
  dependencies {
      // Compose BOM을 사용하여 모든 Compose 라이브러리 버전을 일괄 관리
      implementation(platform(libs.androidx.compose.bom))
      
      // Navigation Compose (BOM에 의해 버전 자동 관리)
      implementation(libs.androidx.navigation.compose)
  }
  ```

<br>

❗️하지만 Compose Navigation은 버전 호환성 문제가 발생해서, 아래처럼 버전을 명시적으로 지정함

- `libs.versions.toml`

  ```toml
  [versions]
  nav = "2.9.3"
  
  [libraries]
  androidx-navigation-compose = { module = "androidx.navigation:navigation-compose", version.ref = "nav" }
  ```

<br>

## 기본 설정

### 1. NavController 설정

NavController는 내비게이션의 핵심 컴포넌트로, 백스택 관리와 화면 간 이동을 담당한다

`rememberNavController()`를 사용하여 Composable 함수 내에서 NavController 인스턴스를 생성할 수 있다

```kotlin
@Composable
fun MainScreen() {
    val navController = rememberNavController()
    
    Scaffold(
        bottomBar = {
            // 하단바에 NavController를 전달하여 화면 이동을 처리
            BottomNavigationBar(navController = navController)
        }
    ) { paddingValues ->
        // NavHost는 NavController와 함께 작동하며 실제 화면을 호스팅한다
        NavHost(
            navController = navController,
            modifier = Modifier.padding(paddingValues)
        ) {
            // NavHost의 람다 블록 안에서 composable 화면들을 정의
            composable(route = Screen.Home.route) { HomeScreen(...) }
        }
    }
}
```

<br>

### 2. Navigation Routes 정의

라우트 문자열을 직접 사용하기보다, `sealed class`를 활용해 타입 안전성을 확보하는 것이 좋다

이를 통해 컴파일 시점에 라우트 오류를 방지할 수 있다

```kotlin
sealed class Screen(val route: String) {
    object Home : Screen("home")
    object Community : Screen("community")
    object My : Screen("my")
  
    // 파라미터가 있는 라우트 정의
    object Detail : Screen("detail/{itemId}") {
        // 실제 라우트를 생성하는 함수 제공
        fun createRoute(itemId: String) = "detail/$itemId"
    }
}
```

<br>

### 3. NavHost 구성

`NavHost`는 NavController와 함께 작동하며, 정의된 라우트에 따라 어떤 Composable 화면을 보여줄지 결정한다

```kotlin
@Composable
fun MainNavHost(
    navController: NavHostController,
    modifier: Modifier = Modifier
) {
    NavHost(
        navController = navController,
        startDestination = Screen.Home.route,  // 앱 시작 시 보여줄 초기화면
        modifier = modifier
    ) {
        composable(route = Screen.Home.route) {
            HomeScreen(
                onNavigateToDetail = { itemId ->
                    // 상세화면으로 이동 시, 파라미터 포함 라우트 생성
                    navController.navigate(Screen.Detail.createRoute(itemId))
                }
            )
        }
        
        composable(
            route = Screen.Detail.route,  // detail/{itemId}
            arguments = listOf(
                // navArgument로 인자의 이름과 타입을 정의
                navArgument("itemId") { type = NavType.StringType }
            )
        ) { backStackEntry ->
            // backStackEntry.arguments에서 인자 추출
            val itemId = backStackEntry.arguments?.getString("itemId") ?: ""
            DetailScreen(
                itemId = itemId,
                onNavigateBack = {
                    navController.popBackStack()
                }
            )
        }
    }
}
```

<br>

## 내비게이션 동작

### 1. 기본 이동

```kotlin
// 단순 화면 이동
navController.navigate("home")

// 파라미터와 함께 이동
navController.navigate("detail/123")
```

### 2. 백스택 관리

내비게이션 백스택(Back Stack)은 사용자가 방문한 화면들을 스택(Stack) 형태로 관리한다

NavController를 사용하면 백스택을 유연하게 제어할 수 있다

- 이전 화면으로 돌아가기

  `popBackStack()`은 현재 화면을 스택에서 제거하고 이전 화면으로 이동한다

  ```kotlin
  navController.popBackStack()
  ```

- 특정 화면까지 돌아가기

  지정한 목적지(destination)가 나올 때까지 중간 화면들을 스택에서 제거하고 이동한다

  ```kotlin
  navController.popBackStack(Scree.Home.route, inclusive = false)
  ```

- 특정 화면을 포함하여 제거

  `inclusive = true`를 `popUpTo`와 함께 사용하면, `popUpTo`에 지정된 목적지 화면까지 포함하여 스택에서 제거된다

  예 : 로그인 화면에서 홈 화면으로 이동 후 뒤로가기를 눌렀을 때, 로그인 화면으로 돌아가지 않고 앱이 종료되도록 만들 수 있다

  ```kotlin
  navController.navigate(Screen.Home.route) {
      popUpTo(Screen.Login.route) {
          inclusive = true
      }
  }
  ```

- 단일 인스턴스 보장 (중복화면 방지)

  `launchSingleTop = true` 옵션은 스택의 맨 위에 동일한 화면이 다시 쌓이는 것을 방지한다

  주로 하단바를 구현할 때 많이 사용된다

  ```kotlin
  navController.navigate(Screen.My.route) {
      launchSingleTop = true
  }
  ```

<br>

## SavedStateHandle을 활용한 데이터 전달 및 상태 보존

- `SavedStateHandle`은 UI 상태를 보존하고 화면 간 데이터를 전달하는데 사용되는 도구다
- 안드로이드 시스템이 앱 프로세스를 종료했다가 다시 시작할 때, 기존의 UI 데이터를 복원하여 사용자 경험의 끊김을 최소화한다

<br>

### 1. SavedStateHandle의 목적

- UI 상태 보존
  - 프로세스 재생성 시에도 UI 데이터를 복원
  
  - 예 : 스크롤 위치, 입력 값 등
  
- Navigation 인자 전달
  - `navArgument`로 전달된 인자를 ViewModel에서 직접 접근 가능


<br>

### 2. SavedStateHandle의 작동 방식

- `SavedStateHandle`은 내부적으로 `Map<String, Any?>` 형태로 데이터를 저장하고 관리한다

- `set(key, value)` 으로 데이터를 저장하고, `get(key)` 으로 저장된 데이터 조회 가능

- Navigation을 통해 전달되는 인자는 자동으로 `SavedStateHandle`에 저장된다

  이를 통해 ViewModel에서 화면 전환 시, 함께 전달된 데이터를 쉽게 가져올 수 있다

<br>

### 3. ViewModel 활용 예시

- `SavedStateHandle`은 주로 ViewModel의 생성자 인자로 주입받아 사용된다
- Hilt와 같은 DI 라이브러리를 사용하면 `SavedStateHandle` 인스턴스를 자동으로 주입받을 수 있다

```kotlin
@HiltViewModel
class SnackDetailViewModel @Inject constructor(
    private val savedStateHandle: SavedStateHandle,
    private val snackRepository: SnackRepository
) : ViewModel() {

    // Navigation을 통해 전달된 "snackId" 인자에 안전하게 접근
    private val snackId: Long = checkNotNull(savedStateHandle.get<Long>("snackId"))

    private val _snack = MutableStateFlow<Snack?>(null)
    val snack: StateFlow<Snack?> = _snack

    init {
        // ViewModel 초기화 시점에 전달받은 snackId를 사용해 데이터 로드
        loadSnack(snackId)
    }

    private fun loadSnack(id: Long) {
        viewModelScope.launch {
            _snack.value = snackRepository.getSnack(id)
        }
    }
}
```



