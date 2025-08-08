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




## 🔥 Android 시스템 UI 커스터마이징

> 이 문서는 Android 앱 상단의 `상태 표시줄(Status Bar)`과 하단의 `시스템 내비게이션 바(System Bar)`의
> 색상 및 아이콘 스타일을 커스터마이징하는 방법을 설명한다
>
> Kotlin, Jetpack Compose를 사용하며 `minSdk` 30 이상인 프로젝트를 기준으로 한다

<br>

### 핵심 개념

- 시스템 UI 확장 : 

  `WindowCompat.setDecorFitsSystemWindows(window, false)` 를 사용하여 앱 콘텐츠를 화면 전체에 확장한다

  이를 통해 앱의 배경색이 상태 표시줄과 시스템 바까지 덮게 되어, 시스템 UI의 색상을 앱 테마와 일치시킬 수 있다

- insets 와 padding :

  insets는 시스템 UI가 차지하는 영역의 크기 정보를 의미한다

  시스템 UI 확장에 따라 콘텐츠가 이 영역에 가려지지 않도록 insets 정보를 이용해 적절한 패딩을 추가한다

- 시스템 UI 아이콘 색상 제어 :

  시스템 바의 배경색이 밝을 경우, 아이콘이 잘 보이도록 어둡게(회색) 변경한다

<br>

## 프로젝트 설정

### 1. MainActivity.kt 설정

`ComponentActivity`의 `onCreate` 메서드에서

`WindowCompat.setDecorFitsSystemWindows(window, false)` 를 호출한다

```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        // 앱 콘텐츠가 시스템 UI 뒤에 표시되도록 설정
        WindowCompat.setDecorFitsSystemWindows(window, false)

        setContent {
            // YourAppTheme 내에서 시스템 UI 색살 설정
            YourAppTheme {
                YourApp()
            }
        }
    }
}
```

<br>

### 2. 테마(Theme) 파일 설정

`res/values/themes.xml` 파일에서 앱의 기본 테마를 수정하여 시스템 UI의 배경색을 설정한다

여기서는 흰색으로 통일한다

```xml
<resources>
    <style name="Theme.YourApp" parent="android:Theme.Material.Light.NoActionBar">
        <item name="android:statusBarColor">@android:color/white</item>
        <item name="android:navigationBarColor">@android:color/white</item>
        <item name="android:windowLightStatusBar">true</item>
    </style>
</resources>
```

👉 `themes.xml`에 있는 속성들

- `android:statusBarColor` : 앱의 상태 표시줄 배경색을 변경
- `android:navigationBarColor` : 앱의 시스템 내비게이션 바 배경색을 변경
- `android:windowLightStatusBar` : 상태 표시줄의 아이콘(시간, 배터리 등) 색상을 변경
  - true로 설정하면 아이콘이 어둡게(회색에 가까운) 표시
  - false 또는 이 속성이 없으면 아이콘이 밝게(흰색) 표시

<br>

### 3. 시스템 UI 아이콘 색상 제어

시스템 내비게이션 바의 아이콘(뒤로가기, 홈 등) 색상을 변경하기 위해, 앱이 실행되고 있는 시스템 창(Window)의 설정을 제어해야 한다

#### 3-1. Window 객체 가져오기 :

컴포저블 내부에서는 직접 Window 객체에 접근할 수 없다

따라서 `LocalView.current`를 통해 현재 화면의 View 객체를 가져오고, 그 View가 속한 Activity의 window 객체를 가져와야 한다

#### 3-2. WindowInsetsController 가져오기 :

window 객체를 통해 WindowInsetsController를 얻는다

이 컨트롤러는 창의 insets와 관련된 다양한 설정을 제어하는 역할을 한다

#### 3-3. 아이콘 색상 설정 :

WindowInsetsController가 가진 `isAppearanceLightNavigationBars` 속성을 true로 설정

- true는 내비게이션 바의 배경이 밝을 때 아이콘을 어둡게(회색으로) 표시
- 반대로 false로 설정하면 배경이 어두울 때 아이콘을 밝게(흰색으로) 표시

```kotlin
@Composable
fun TestApp() {
  
    // 뷰를 통해 Window 객체와 InsetsController에 접근
    val view = LocalView.current
    if (!view.isInEditMode) {
        val window = (view.context as? Activity)?.window ?: return
        val insetsController = WindowCompat.getInsetsController(window, view)

        LaunchedEffect(true) {
            // 내비게이션 바의 아이콘 색상을 어둡게 (흰색 배경에 잘 보이도록)
            insetsController.isAppearanceLightNavigationBars = true
        }
    }

    Scaffold(
        bottomBar = {
            // 하단 바 컴포저블에 padding 적용
            YourBottomNav(
                modifier = Modifier.windowInsetsPadding(WindowInsets.navigationBars)
                // ...
            )
        }
    ) { paddingValues ->
        NavHost(
            // NavHost에 Scaffold가 제공하는 패딩을 적용
            modifier = Modifier.padding(paddingValues)
            // ...
        ) {
            // ...
        }
    }
}
```
