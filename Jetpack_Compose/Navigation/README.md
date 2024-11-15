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

app(module) 수준의 build.gradle에 의존성 추가

```kotlin
dependencies {
    implementation("androidx.navigation:navigation-compose:2.5.3")
    // Hilt는 Navigation Compose 라이브러리와도 통합된다
    implementation("androidx.hilt:hilt-navigation-compose:1.0.0")  
}
```

<br>

### NavController 설정

컴포즈에서 네비게이션을 사용할 때 NavController는 중심이 되는 컴포넌트다

백 스택 항목을 추적하고, 스택을 앞으로 이동하고, 백 스택 조작을 활성화하고, 화면 상태(Screen states) 간 네비게이팅 한다

NavController는 네비게이션의 중심이기 때문에, 목적지로 이동하려면 NavController를 먼저 생성해야 한다

<br>

컴포즈내에서 NavController의 하위 클래스인 NavHostController로 작업하고 있다

rememberNavController() 함수를 사용하여 NavController 얻을 수 있다

이는 NavController를 생성하고 컴포지션내에 저장한다

NavController는 단일 NavHost 컴포저블과 연관되어있다

NavHost는 NavController를 컴포저블 목적지들이 명시되어있는 네비게이션 그래프와 연결한다

```kotlin
@Composable
fun MainScreen() {
    val navController = rememberNavController()
    
    Scaffold(
        ...
    ) {
      	MainNavScreen(navController)
    }
}
```

👉 MainScreen내에서 NavController를 얻고, 저장한다. NavController는 애플리케이션 전체에 대한 최상위(root) 컴포저블이다

<br>

### NavHost 만들기

NavHost 를 생성하고 NavController를 연결한다면

Navigation Graph와 Destination간의 이동이 가능한 컴포저블 Destination을 연동하게 된다

만약 컴포저블끼리 탐색이 된다면 NavHost가 자동적으로 리컴포지션을 수행한다

```kotlin
@Composable
fun MainNavScreen(navController: NavHostController) {
    NavHost(
        navController = navController,
        startDestination = MainNav.Home.route
    ) {
        composable(
            route = MainNav.Home.route
        ) {
            HomeScreen()
        }
        composable(
            route = MainNav.My.route
        ) {
            MyScreen()
        }
    }
```

<br>

### 컴포저블로 이동

NavGraph에서 Destination으로 이동하려면

NavController의 `navigate()` 함수를 사용하여 Destination의 경로를 나타내는 String 값을 이용하여 컴포저블에서 이동한다

```kotlin
NavController.navigate("$routeName$argument")
```

<br>

❗️NavController는 내부에서 사용하기 위한 Navigation의 대부분 동작을 명시해놓은 class이며,

개발자가 이 코드에 직접적으로 접근하는 것을 방지하기 위해 NavHostController를 통해 접근하는 것이 좋다

```kotlin
NavHostController.navigate("$routeName$argument")
```

<br>

### Compose Navigation backstack

popBackStack()

<br>

### Bottom Navigation

- Bottom Navigation item 리스트 만들기

```kotlin
@Composable
fun MainBottomBar(navController: NavHostController, currentRoute: String?) {
    val bottomNavigationItems = listOf(
        MainNav.Home,
        MainNav.My
    )
    ...
}
```

<br>

- Bottom Navigation 틀 만들기
  - material3의 NavigationBar 사용 (material - BottomNavigation)
  - material3의 NavigationBarItem 사용 (material - BottomNavigationItem)
  - bottom navigation item에 관한 정보, 즉 각 item의 속성값을 지정해준다

```kotlin
@Composable
fun MainBottomBar(navController: NavHostController, currentRoute: String?) {
    ...
    NavigationBar(
        ...
    ) {
        bottomNavigationItems.forEach { item ->
            NavigationBarItem(
                selected = currentRoute == item.route,  // 경로 비교 (맨 아래 참고)
                onClick = {
                    NavigationUtils.navigate(
                        navController, item.route,
                        navController.graph.startDestinationRoute
                    )
                },
                icon = { /*TODO*/ })
        }
    }
}
```

```kotlin
object NavigationUtils {
    fun navigate(
        controller: NavHostController,
        routeName: String,
        args: Any? = null,
        backStackRouteName: String? = null,
        isLaunchSingleTop: Boolean = true,
        needToRestoreState: Boolean = true
    ) {
        var argument = ""
        if(args != null) {

        }
        controller.navigate("$routeName$argument") {
            if(backStackRouteName != null) {
                popUpTo(backStackRouteName) { saveState = true }
            }
            launchSingleTop = isLaunchSingleTop
            restoreState = needToRestoreState
        }
    }
}
```

👉 `popUpTo()`를 통해 backStackRouteName만 스택에 쌓일 수 있게 했다

👉 `launchSingleTop = true`를 통해 화면 인스턴스 하나만 만들어지게 했다

👉 `restoreState = true`를 통해 버튼을 재클릭했을 때 이전 상태가 남아있게 했다

<br>

- NavDestination 접근하여 선택된 위치 확인

  - 어떤 BottomNavigationItem이 선택됐는지 알기 위해

    item 경로와 현재 대상 및 상위 대상 경로를 비교하여 선택된 상태를 알 수 있게 한다

```kotlin
val navBackStackEntry by navController.currentBackStackEntryAsState()
val currentRoute = navBackStackEntry?.destination?.route
```

👉 NavController의 `currentBackStackEntryAsState()`를 통해

navBackStackEntry를 가져와 목적지의 route(위치 string)를 가져온다
