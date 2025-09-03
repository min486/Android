<div align="center">
  <p>
    <img src="../README.assets/android.png">
  </p>
  <br>
  <h2>Library</h2>
  <p>라이브러리 관련 내용 정리</p>
  <br>
  <br>
</div>







## 🔥 스플래시 화면 설정 방법

> 이 문서는 `androidx.core:core-splashscreen` 라이브러리를 사용해서 스플래시 화면을 구현하는 방법을 설명하고,
>
> 스플래시 로고(아이콘) 제작 가이드라인을 제공한다

<br>

### 개요

Android 12 (API 31)부터 시스템 스플래시 화면이 기본적으로 도입되었다

`androidx.core:core-splashscreen` 라이브러리를 사용하면, 모든 Android 버전에서 일관된 스플래시 화면 경험을 제공할 수 있다

<br>

### 1. 의존성 추가

`app/build.gradle`

```kotlin
dependencies {
    // Splash Screen
    implementation("androidx.core:core-splashscreen:1.0.1")
}
```

<br>

### 2. 스플래시 로고 준비

스플래시 화면에 표시될 로고는 SVG 파일 형식으로 준비하는 것이 좋다

구글의 권장 가이드라인은 아래와 같다

| 유형             | 권장 프레임 크기 (정사각형) | 로고 크기 및 위치                                            |
| ---------------- | --------------------------- | ------------------------------------------------------------ |
| 배경이 있는 로고 | 240 x 240 px                | 로고는 프레임 중앙에 배치하되, 약 160 x 160 px 원 내에 들어가도록 배치 |
| 배경이 없는 로고 | 288 x 288 px                | 로고는 프레임 중앙에 배치하되, 약 192 x 192 px 원 내에 들어가도록 배치 |

*출처 : https://developer.android.com/develop/ui/views/launch/splash-screen?hl=ko#dimensions

<br>

### 3. 로고 변환 (SVG → Vector Drawable)

준비한 SVG 로고를 안드로이드 프로젝트에서 사용할 수 있는 Vector Drawable로 변환해야 한다

1. `res/drawable` 폴더 우클릭 → `New` → `Vector Asset` 선택

2. `Asset Type`을 `Local file (SVG, PSD)`로 선택

3. 준비한 SVG 파일을 선택하면 자동으로 변환된다

<br>

### 4. 테마 설정

스플래시 화면에 적용될 테마를 설정한다

라이브러리를 사용하면 API 레벨에 상관없이 동일한 속성을 사용해서 테마를 구성할 수 있다

#### 4-1. Android 12 (API 31) 이상 테마

- `res/values-v31/themes.xml`

  ```xml
  <resources>
    	<style name="Theme.YourApp" parent="android:Theme.Material.Light.NoActionBar">
          <item name="android:statusBarColor">@android:color/white</item>
          <item name="android:navigationBarColor">@android:color/white</item>
          <item name="android:windowLightStatusBar">true</item>
          <item name="android:windowLightNavigationBar">true</item>
      </style>
    
      <style name="Theme.YourApp.SplashScreen" parent="Theme.SplashScreen">
          <!-- 배경색 -->
          <item name="android:windowSplashScreenBackground">@color/splash_bg</item>
          <!-- 중앙 로고 -->
          <item name="android:windowSplashScreenAnimatedIcon">@drawable/ic_hambug</item>
          <!-- 로고 배경색 -->
          <item name="android:windowSplashScreenIconBackgroundColor">@color/ic_bg</item>
          <!-- 스플래시 화면 이후 적용될 테마 -->
          <item name="postSplashScreenTheme">@style/Theme.YourApp</item>
      </style>
  </resources>
  ```

  👉 이런 식으로 테마를 분리하면, `Theme.YourApp.SplashScreen`은 스플래시 화면에만 적용되고,

  `postSplashScreenTheme` 속성을 통해 스플래시 화면이 사라진 후에는 자동으로 `Theme.YourApp` 테마로 전환된다

#### 4-2. Android 11 (API 30) 이하 테마

- `res/values/themes.xml`

  ```xml
  <resources>
      <style name="Theme.YourApp" parent="android:Theme.Material.Light.NoActionBar">
          <item name="android:statusBarColor">@android:color/white</item>
          <item name="android:navigationBarColor">@android:color/white</item>
          <item name="android:windowLightStatusBar">true</item>
          <item name="android:windowLightNavigationBar">true</item>
      </style>
  
      <style name="Theme.YourApp.SplashScreen" parent="Theme.YourApp" >
          <!-- 배경 + 로고 -->
          <item name="android:windowBackground">@drawable/splash_layer_list</item>
      </style>
  </resources>
  ```

  👉 `Theme.YourApp.SplashScreen` 테마는 `Theme.YourApp`을 상속받아야 `statusBarColor` 등의 속성을 공유할 수 있다

  👉 `Theme.YourApp.SplashScreen` 테마는 `android:windowBackground`를 포함하므로 `postSplashScreenTheme`는 필요 없다

- `@drawable/splash_layer_list`

  ```xml
  <layer-list xmlns:android="http://schemas.android.com/apk/res/android">
      <!-- 배경색 -->
      <item android:drawable="@color/white" />
      <!-- 중앙 로고 -->
      <item android:drawable="@drawable/logo" android:gravity="center" />
  </layer-list>
  ```

  ❗️bitmap 태그 대신 android:drawable 속성을 사용해야 API 30 이하에서도 로고가 표시된다

#### 4-3. 종합적인 동작 방식

- API 31 이상 : `AndroidManifest`의 테마 이름을 보고, `res/values-v31/themes.xml`의 테마가 적용된다

  이 테마(`Theme.YourApp.SplashScreen`)의 속성들을 통해 스플래시 화면이 표시된다

- API 30 이하 : `AndroidManifest`의 동일한 테마 이름을 보고, `res/values/themes.xml`의 테마가 적용된다

  이 테마의 `android:windowBackground` 속성을 통해 스플래시 화면이 표시된다

<br>

### 5. AndroidManifest.xml 설정

- AndroidManifest 에서 application 태그에만 스플래시 테마를 지정하고,

- activity 태그에는 테마를 지정하지 않는다 (테마 제거)

```xml
<application
    android:theme="@style/Theme.YourApp.SplashScreen">
    ...
</application>
```

<br>

### 6. MainActivity 에서 스플래시 화면 제어

MainActivity 에서 `installSplashScreen()` 을 호출하고,

앱의 초기화 상태에 따라 스플래시 화면이 사라지도록 `setKeepOnScreenCondition` 을 설정한다

```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        // 1. 스플래시 화면 설치
        val splashScreen = installSplashScreen()
      
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
      
        // 2. 스플래시 화면 유지 조건 설정
        // splashScreen.setKeepOnScreenCondition { 앱 초기화 진행 중 여부 }
      
        // 3. 앱 초기화 작업 시작
        // 실제 앱의 필수 초기화 작업 (데이터 로딩, 사용자 인증, 초기 UI 구성 등)을 여기서 시작한다

        setContent {
            // 실제 UI 구성
        }
}
```

- `setKeepOnScreenCondition { false }`
  - false를 반환할 때 : 스플래시 화면을 계속 보여준다
  - true를 반환할 때 : 스플래시 화면을 즉시 종료한다

<br>

## 스플래시 라이브러리의 필요성

`androidx.core:core-splashscreen` 라이브러리는

안드로이드 앱에서 버전에 상관없이 일관된 스플래시 화면 경험을 제공하기 위해 필수적이다

- Android 12 (API 레벨 31) 이상
  - 이 버전부터는 시스템 기본 스플래시 화면이 자동으로 적용된다
  - 라이브러리를 사용하면 로고, 배경색, 로고 배경색 등을 커스터마이징할 수 있다
- Android 11 (API 레벨 30) 이하
  - 기본적으로 시스템 스플래시 화면 기능이 없다
  - 라이브러리가 동일한 방식으로 동작하도록 구현하여, 최신 버전과 유사한 스플래시 화면을 제공한다

- 정리
  - minSdk가 31 이상이든 30 이하이든, 스플래시 화면의 디자인과 종료 시점을 제어하려면
  - `androidx.core:core-splashscreen` 라이브러리는 필수적으로 사용하는 것이 좋다
