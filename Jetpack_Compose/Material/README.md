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




## 🔥 Material

### MaterialTheme

- 앱의 전반적인 디자인 시스템(color, typography 등)을 전역적으로 설정하고 사용하는 구조
- Compose의 많은 컴포넌트(Text, Button, Surface 등)는 MaterialTheme의 스타일을 자동으로 따름
- 구성 요소
  - colorScheme : primary, surface, background 등 색상 정의
  - typography : 텍스트 스타일 정의
  - shapes : 요소의 모양 (RoundedCornerShape 등)


<br>

### Compose에서 사용하는 Material

- Jetpack Compose에서는 기본적으로 Material3 사용함

- Compose 프로젝트를 새로 생성하면 `androidx.compose.material3` 라이브러리를 사용함

<br>

### Surface

- Material 디자인 시스템에서 시각적 속성을 가지는 Compose 컴포저블
- 색상, 테마 스타일 등을 적용할 수 있는 컨테이너 역할을 한다
- 주요 속성 : color, shape, onClick 처리 등 

- 예제

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

### Surface를 화면 바깥에 사용하는 이유

Compose에서는 `Surface`를 각 화면의 가장 바깥에 사용한다

1. MaterialTheme의 시각적 속성을 적용할 수 있음
   - `Surface`는 기본적으로 `MaterialTheme.colorScheme.surface`를 배경색으로 갖고 있음
   - 따라서 다크모드/라이트모드 전환 시 자동으로 적절한 배경색이 적용됨
2. 클릭 처리나 배경 효과 등 확장에 유리
   - 예 : `onClick`으로 전체 화면 터치 대응, ripple 효과 등
3. Material3 권장사항
   - Material3 시스템은 모든 UI 요소가 `surface` 위에 있어야 한다고함
   - 즉 모든 시각적 요소는 `Surface` 위에서 렌더링되어야 일관된 UX를 제공함

<br>

### Dark Theme, Dynamic Color

- 다크 모드 (Dark Theme)
  - 밝기 기반의 테마 전환
  - MaterialTheme에서 두가지 컬러셋을 정의해 사용함
  - 대부분의 앱은 이 구조를 기본으로함
- 다이나믹 컬러 (Dynamic Color)
  - 사용자가 설정한 배경화면의 색상을 분석해 앱의 색상 팔레트를 자동 생성함
  - Android 12 이상부터 도입된 기능
  - 예 : 사용자가 초록색 계열의 배경화면을 설정하면, `primary`, `secondary`, `surface` 등 모든 컬러가 그 계열로 자동 매핑됨

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
