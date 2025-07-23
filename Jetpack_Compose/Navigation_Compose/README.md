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

(Navigation Compose 포함)

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

NavController는 네비게이션의 핵심 컴포넌트로, 백스택을 관리하고 화면 간 이동을 담당한다

```kotlin
@Composable
fun MainScreen() {
    val navController = rememberNavController()
    
    Scaffold(
        bottomBar = {
            BottomNavigationBar(navController = navController)
        }
    ) { paddingValues ->
        NavHost(
            navController = navController,
            modifier = Modifier.padding(paddingValues)
        )
    }
}
```

<br>

### 2. Navigation Routes 정의

타입 안전성을 위해 sealed class를 사용하여 라우트를 정의한다

```kotlin
sealed class Screen(val route: String) {
    object Home : Screen("home")
    object Community : Screen("community")
    object My : Screen("my")
  
    // 파라미터가 있는 라우트
    object Detail : Screen("detail/{itemId}") {
        fun createRoute(itemId: String) = "detail/$itemId"
    }
}
```

<br>

### 3. NavHost 구성

```kotlin
@Composable
fun MainNavHost(
    navController: NavHostController,
    modifier: Modifier = Modifier
) {
    NavHost(
        navController = navController,
        startDestination = Screen.Home.route,
        modifier = modifier
    ) {
        composable(route = Screen.Home.route) {
            HomeScreen(
                onNavigateToDetail = { itemId ->
                    navController.navigate(Screen.Detail.createRoute(itemId))
                }
            )
        }
        
        composable(
            route = Screen.Detail.route,
            arguments = listOf(
                navArgument("itemId") { type = NavType.StringType }
            )
        ) { backStackEntry ->
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

### 네비게이션 동작

- 기본 이동

  ```kotlin
  // 단순 이동
  navController.navigate("home")
  
  // 파라미터와 함께 이동
  navController.navigate("detail/123")
  ```

- 백스택 관리

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

- 조건부 네비게이션

  ```kotlin
  @Composable
  fun HomeScreen(onNavigateToDetail: (String) -> Unit) {
      // 로그인 상태에 따른 조건부 네비게이션
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
