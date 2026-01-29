<div align="center">
  <p>
    <img src="../README.assets/android.png">
  </p>
  <br>
  <h2>Design System</h2>
  <p>디자인 시스템 관련 내용 정리</p>
  <br>
  <br>
</div>





## 🔥 Color

### 색상 시스템 구조

색상은 Raw → Semantic → Material Bridge 3단계로 분리하여 관리한다


<br>

### 1. Raw Color (원시 색상)

실제 Hex 값을 정의하며, UI에서 직접 사용하지 않는다

```kotlin
val HambugRed = Color(0xFFFF7155)
val Gray900 = Color(0xFF1C1917)
```

<br>

### 2. Semantic Color (의미 기반 색상)

UI의 역할에 따라 네이밍하며, 컴포넌트에서는 이 색상한 사용한다

```kotlin
@Immutable
data class HambugColors(
    val primRed: Color,
    val textHeadline: Color,
    ...
)

val LightColorPalette = HambugColors(
    primRed = HambugRed,
    textHeadline = Gray900,
    ...
)
```

<br>

### 3. Material Bridge (Material 3 호환 레이어)

일부 Material 컴포넌트가 `MaterialTheme.colorScheme`을 직접 참조하므로, 최소한의 매핑만 제공한다

- Material3 기본 ColorScheme에 커스텀 색상 매핑
- 전체 색상 시스템을 대체하지 않음

```kotlin
private fun hambugLightColorScheme(colors: HambugColors) = lightColorScheme(
    primary = colors.primRed,
    onPrimary = colors.bgWhite,
    background = colors.bgNormal,
    ...
)
```
