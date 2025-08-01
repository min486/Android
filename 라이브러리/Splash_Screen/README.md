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

> 이 문서는 스플래시 화면 기능을 구현하기 위해
>
> `androidx.core:core-splashscreen` 라이브러리를 사용하는 방법과, 로고(아이콘) 제작 가이드라인을 정리함

<br>

### 개요

Android 12 (API 31)부터 시스템 스플래시 화면이 기본적으로 도입되었다

`androidx.core:core-splashscreen` 라이브러리를 사용하면, 모든 버전에서 일관된 스플래시 화면 경험을 제공할 수 있다

<br>

### 1. 의존성 추가

`app/build.gradle`

```kotlin
dependencies {
    // Splash Screen
    implementation 'androidx.core:core-splashscreen:1.0.1'
}
```

<br>

### 2. 로고(아이콘) 준비 가이드

스플래시 화면에 표시되는 로고는 앱 아이콘 또는 전용 로고를 사용할 수 있다

구글의 권장 가이드라인은 다음과 같다

| 유형                         | 크기        | 원 안의 크기                |
| ---------------------------- | ----------- | --------------------------- |
| 아이콘 배경이 있는 앱 아이콘 | 240 x 240dp | 지름 160dp 원 안에 맞춰야함 |
| 아이콘 배경이 없는 앱 아이콘 | 288 x 288dp | 지름 192dp 원 안에 맞춰야함 |

*출처 :

https://developer.android.com/develop/ui/views/launch/splash-screen?hl=ko#dimensions

<br>

### 3. 로고 변환 (SVG → Vector Drawable)

스플래시 화면에 사용될 정적 로고(SVG)를 벡터 드로어블로 변환한다

- `res/drawable` 폴더 우클릭 → `New` → `Vector Asset` 선택

- `Asset Type` 을 `Local file (SVG, PSD)` 로 선택

- SVG 파일을 선택하여 자동 변환한다

<br>

### 4. 테마 설정

스플래시 화면에 적용될 테마를 설정한다

#### 4-1. Android 12 (API 31+) 전용 테마

`res/values-v31/themes.xml`

```xml
<resources>
    <style name="Theme.YourApp.SplashScreen" parent="Theme.SplashScreen">
        <!-- 배경색 -->
        <item name="android:windowSplashScreenBackground">@color/splash_background</item>

        <!-- 중앙 로고 -->
        <item name="android:windowSplashScreenAnimatedIcon">@drawable/logo</item>
        
        <!-- 아이콘 배경색 -->
        <item name="android:windowSplashScreenIconBackgroundColor">@color/splash_icon_background</item>

        <!-- 스플래시 화면 이후 적용될 테마 -->
        <item name="postSplashScreenTheme">@style/Theme.YourApp</item>
    </style>
</resources>
```

#### 4-2. Android 11 (API 30) 이하 호환 테마

`res/values/themes.xml`

```xml
<resources>
    <style name="Theme.YourApp" parent="android:Theme.Material.Light.NoActionBar" />

    <style name="Theme.YourApp.SplashScreen" parent="android:Theme.Material.Light.NoActionBar" >
        <!-- 배경 + 로고 -->
        <item name="android:windowBackground">@drawable/splash_layer_list</item>

        <item name="postSplashScreenTheme">@style/Theme.Sptest</item>
    </style>
</resources>
```

`@drawable/splash_layer_list`

```xml
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <!-- 배경색 -->
    <item android:drawable="@color/white" />
    <!-- 중앙 로고 -->
    <item android:drawable="@drawable/logo_bug2" android:gravity="center" />
</layer-list>
```

❗️bitmap 태그 대신 android:drawable 속성을 사용해야 API 30 이하에서도 로고가 표시된다

<br>

### 5. AndroidManifest.xml 설정

MainActivity에 스플래시 테마를 적용한다

```xml
<application
    android:theme="@style/Theme.YourApp">
  
    <activity
        android:name=".MainActivity"
        android:exported="true"
        android:label="@string/app_name"
        android:theme="@style/Theme.YourApp.SplashScreen"> 
      
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />
            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
    </activity>
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

<br>

## 스플래시 라이브러리의 필요성

`androidx.core:core-splashscreen` 라이브러리는

안드로이드 앱에서 버전에 상관없이 일관된 스플래시 화면 경험을 제공하기 위해 필수적이다

- Android 12 (API 레벨 31) 이상
  - 이 버전부터는 시스템 기본 스플래시 화면이 자동으로 적용된다
  - 라이브러리를 사용하면 로고, 배경색, 아이콘 배경색, 표시 시간 등을 커스터마이징할 수 있다
- Android 11 (API 레벨 30) 이하
  - 기본적으로 시스템 스플래시 화면 기능이 없다
  - 라이브러리가 동일한 방식으로 동작하도록 구현하여, 최시 버전과 유사한 스플래시 화면을 제공한다

👉 정리

minSdk가 31 이상이든 30 이하이든, 스플래시 화면의 디자인과 종료 시점을 제어하려면

`androidx.core:core-splashscreen` 라이브러리는 필수적으로 사용하는 것이 좋다
