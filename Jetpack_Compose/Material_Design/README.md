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




## 🔥 Material Design

- Material Design은 Google이 만든 UI/UX 디자인 시스템이다

- Jetpack Compose는 Material Design을 기반으로 하여, 일관되고 현대적인 UI를 쉽게 만들 수 있도록 지원한다

- Jetpack Compose는 기본적으로 Material3 디자인 시스템을 사용한다

  👉 새로운 프로젝트를 생성하면 `androidx.compose.material3` 라이브러리가 자동으로 포함된다

<br>

### MaterialTheme

> 앱의 전반적인 디자인 시스템을 정의하고,
>
> 모든 UI 요소에 일관된 스타일을 적용하는데 사용되는 컴포저블

- MaterialTheme은 아래 3가지 구성 요소를 정의한다
  - colorScheme : 앱의 색상 팔레트(primary, surface, background 등) 정의
  - typography : 앱 내 텍스트 스타일(폰트, 크기, 굵기 등) 정의
  - shapes : 컴포넌트 모양(둥근 모서리, 직사각형 등) 정의

✅ MaterialTheme은 앱의 최상위 컴포저블로 위치하여,

하위의 모든 Material 컴포저블(Button, Text, Surface 등)이 정의된 스타일을 자동으로 상속받는다

<br>

### Surface

> Material Design에서 시각적 속성(색상, 모양, 고도 등)을 가지는 컨테이너 역할을 하는 컴포저블
>
> 배경색, 모서리 모양, 그림자 효과 등을 쉽게 적용할 수 있으며, 클릭 이벤트도 처리할 수 있다

- 예시 코드

  ```kotlin
  Surface(
      color = MaterialTheme.colorScheme.surface,
      shape = RoundedCornerShape(12.dp),
      modifier = Modifier.padding(16.dp)
  ) {
      Text(
          text = "This is inside a Surface"
      )
  }
  ```

<br>

### Surface를 화면 최상위에 두는 이유

Material Design 가이드라인에 따라, `Surface`는 각 화면의 가장 바깥쪽 컴포저블로 두는 것이 권장된다

1. 테마 적용

   `Surface`는 기본적으로 `MaterialTheme.colorScheme.surface` 색상을 사용한다

   이 덕분에 다크모드/라이트모드 전환 시 자동으로 적절한 배경색이 적용되어 일관된 디자인을 유지할 수 있다

2. 클릭 및 효과 처리

   `onClick` 같은 속성을 제공하여 전체화면 또는 특정영역의 터치 이벤트를 쉽게 처리할 수 있다

3. Material Design 원칙

   Material3 시스템은 모든 UI 요소가 `Surface` 위에서 렌더링되도록 설계되었다

   이를 통해 앱 전체가 일관된 시각적 경험을 제공한다

<br>

### Dark Theme & Dynamic Color

Material Design은 다양한 사용자 환경에 맞춰 다크 테마와 다이나믹 컬러를 지원한다

- 다크 테마 (Dark Theme)
  
  - 사용자의 시스템 설정에 따라 UI를 밝거나 어둡게 전환
  - 개발자는 MaterialTheme에 밝은 테마용 / 어두운 테마용 `colorScheme`을 정의하여 적용
  
- 다이나믹 컬러 (Dynamic Color)

  - Android 12 (API 31) 이상에서 지원

  - 사용자의 배경화면 색상을 기반으로 주요 색상 팔레트를 생성하여 앱에 적용

  - 앱이 개인화된 환경과 자연스럽게 어울리도록 해준다


```kotlin
val colorScheme = when {
    dynamicColor && Build.VERSION.SDK_INT >= Build.VERSION_CODES.S -> {
        val context = LocalContext.current
        if (darkTheme) dynamicDarkColorScheme(context) else dynamicLightColorScheme(context)
    }
    darkTheme -> DarkColorScheme
    else -> LightColorScheme
}
```
