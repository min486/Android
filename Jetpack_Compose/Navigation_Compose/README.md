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




## 🔥 Navigation Compose

### 의존성 추가

Compose BOM을 사용하면 모든 Compose 라이브러리의 호환 가능한 버전이 자동으로 관리된다

- libs.versions.toml 파일

  ```toml
  [versions]
  composeBom = "2025.05.00"
  
  [libraries]
  androidx-compose-bom = { group = "androidx.compose", name = "compose-bom", version.ref = "composeBom" }
  
  # --- BOM에 포함된 Jetpack Compose 관련 라이브러리 ---
  androidx-navigation-compose = { module = "androidx.navigation:navigation-compose" }
  ```

- app/build.gradle 파일

  ```kotlin
  dependencies {
      // Compose BOM을 사용하여 모든 Compose 라이브러리 버전을 일괄 관리
      implementation(platform(libs.androidx.compose.bom))
      
      // Navigation Compose (BOM에 의해 버전 자동 관리)
      implementation(libs.androidx.navigation.compose)
  }
  ```

<br>

## 기본 설정

### 1. NavController 설정

NavController는 내비게이션의 핵심 컴포넌트로, 백스택을 관리하고 화면 간 이동을 담당한다

rememberNavController()를 사용하여 Composable 함수 내에서 NavController 인스턴스를 얻을 수 있다

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
            // NavHost의 람다 블록 안에서 composable 화면들을 정의한다
            composable(route = Screen.Home.route) { HomeScreen(...) }
        }
    }
}
```

<br>

### 2. Navigation Routes 정의

타입 안전성을 확보하고 라우트 관리를 용이하게 하기 위해, sealed class를 사용하여 내비게이션 라우트를 정의하는 것이 좋다

이렇게 하면 컴파일 시점에 라우트 관련 오류를 방지할 수 있다

```kotlin
sealed class Screen(val route: String) {
    object Home : Screen("home")
    object Community : Screen("community")
    object My : Screen("my")
  
    // 파라미터가 있는 라우트 정의 : 중괄호 안에 인자 이름을 명시한다
    object Detail : Screen("detail/{itemId}") {
        // 실제 라우트를 생성하는 함수 제공
        fun createRoute(itemId: String) = "detail/$itemId"
    }
}
```

<br>

### 3. NavHost 구성

NavHost는 NavController와 함께 작동하며, 정의된 라우트에 따라 어떤 Composable 화면을 보여줄지 결정하는 역할을 한다

각 composable 블록은 특정 라우트에 해당하는 화면을 정의한다

```kotlin
@Composable
fun MainNavHost(
    navController: NavHostController,
    modifier: Modifier = Modifier
) {
    NavHost(
        navController = navController,
        startDestination = Screen.Home.route,  // 앱 시작시 보여줄 초기화면 라우트
        modifier = modifier
    ) {
        composable(route = Screen.Home.route) {
            HomeScreen(
                onNavigateToDetail = { itemId ->
                    // 상세화면으로 이동시, 파라미터와 함께 라우트 생성
                    navController.navigate(Screen.Detail.createRoute(itemId))
                }
            )
        }
        
        // 라우트와 인자 목록을 함께 정의
        composable(
            route = Screen.Detail.route,  // detail/{itemId}
            arguments = listOf(
                // navArgument를 사용하여 인자의 이름과 타입을 명시한다
                navArgument("itemId") { type = NavType.StringType }
            )
        ) { backStackEntry ->
            // backStackEntry.arguments에서 인자 값을 추출한다
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

### 내비게이션 동작

- 기본 이동

  ```kotlin
  // 단순 화면 이동
  navController.navigate("home")
  
  // 파라미터와 함께 이동
  navController.navigate("detail/123")
  ```

- 백스택 관리

  내비게이션 백스택은 사용자가 방문한 화면들의 기록이다

  NavController를 통해 백스택을 유연하게 제어할 수 있다

  ```kotlin
  // 이전 화면으로 돌아가기
  navController.popBackStack()
  
  // 특정 화면까지 돌아가기
  navController.popBackStack(Scree.Home.route, inclusive = false)
  
  // 백스택을 정리하면서 이동 (예 : 로그인 후 홈으로)
  navController.navigate(Screen.Home.route) {
      popUpTo(Screen.Login.route) {
          inclusive = true  // Login 화면도 백스택에서 제거
      }
  }
  
  // 단일 인스턴스 보장 (중복화면 방지)
  navController.navigate(Screen.My.route) {
      launchSingleTop = true
  }
  ```

- 조건부 내비게이션

  앱의 상태에 따라 다른 화면으로 이동하는 로직을 구현할 수 있다

  (예 : 로그인 여부, 데이터 로드 완료)

  ```kotlin
  @Composable
  fun HomeScreen(onNavigateToDetail: (String) -> Unit) {
      // 로그인 상태에 따른 조건부 내비게이션
      val isLoggedIn = remember { mutableStateOf(false) }
      
      Button(
          onClick = {
              if (isLoggedIn.value) {
                  onNavigateToDetail("123")
              } else {
                  // 로그인 화면으로 이동
              }
          }
      ) {
          Text("상세 보기")
      }
  }
  ```

<br>

### SavedStateHandle을 활용한 데이터 전달 및 상태 보존

- SavedStateHandle은 Jetpack ViewModel과 함께 사용되어 UI 상태를 안전하게 보존하고,

  Jetpack Compose Navigation을 통해 화면 간 데이터를 간편하게 전달할 수 있도록 돕는 강력한 도구

- 이는 프로세스 재생성과 같은 상황에서도 데이터 손실 없이 사용자 경험을 유지하는데 핵심적인 역할을 한다

<br>

### 1. SavedStateHandle의 목적

- UI 상태 보존 :

  안드로이드 시스템이 앱의 프로세스를 종료했다가 다시 시작할 때, ViewModel에 저장된 UI 관련 데이터를 복원하여 사용자에게 끊김 없는 경험을 제공한다

  (예 : 스크롤 위치, 텍스트 입력 값 등)

- Navigation 인자 전달 :

  Jetpack Navigation 컴포넌트와 연동되어 navArgument를 통해 전달되는 화면 간 인자(arguments)를 ViewModel에서 직접, 그리고 안전하게 접근할 수 있도록 해준다

<br>

### 2. SavedStateHandle의 작동 방식

SavedStateHandle은 내부적으로 Map<String, Any?>과 유사한 형태로 데이터를 저장하고 검색한다

- 데이터 저장 및 검색 :

  set() 메서드로 키-값 쌍을 저장하고, get() 메서드로 저장된 데이터를 검색한다

  필요시 checkNotNull 등을 사용하여 필수 값임을 명시할 수 있다

- Jetpack Navigation과의 통합 :

  composable 블록의 arguments에 navArgument로 정의된 인자들은 화면 이동시 SavedStateHandle에 자동으로 저장된다

<br>

### 3. SavedStateHandle의 활용 (ViewModel 내에서)

- SavedStateHandle은 주로 ViewModel의 생성자 인자로 주입받아 사용된다
- Hilt와 같은 의존성 주입 라이브러리를 사용하면 SavedStateHandle 인스턴스를 자동으로 주입받을 수 있다

```kotlin
@HiltViewModel
class SnackDetailViewModel @Inject constructor(
    private val savedStateHandle: SavedStateHandle,  // Hilt가 SavedStateHandle을 자동으로 주입
    private val snackRepository: SnackRepository
) : ViewModel() {

    // Navigation을 통해 전달된 인자 접근
    // "snackId"는 NavHost에서 navArgument("snackId")로 정의하고, navigate("detail/$snackId")로 전달된 값
    private val snackId: Long = checkNotNull(savedStateHandle.get<Long>("snackId"))

    private val _snack = MutableStateFlow<Snack?>(null)
    val snack: StateFlow<Snack?> = _snack

    init {
        // ViewModel 초기화 시점에 전달받은 snackId를 사용하여 데이터 로드
        loadSnack(snackId)
    }

    private fun loadSnack(id: Long) {
        viewModelScope.launch {
            _snack.value = snackRepository.getSnack(id)  // Repository에서 실제 데이터 가져옴
        }
    }
}
```











