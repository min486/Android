<div align="center">
  <p>
    <img src="../README.assets/android.png">
  </p>
  <br>
  <h2>기능</h2>
  <p>기능 관련 내용 정리</p>
  <br>
  <br>
</div>






## 🔥 스플래시 화면 설정 방법

안드로이드 12 (API 레벨 31)부터 시스템 스플래시 화면이 기본적으로 도입되었다

이 가이드에서는 `androidx.core:core-splashscreen` 라이브러리를 사용하여, 

모든 API 레벨에서 일관된 스플래시 화면 경험을 제공하는 방법을 다룬다

<br>

### 1. 의존성 추가

`app/build.gradle` 파일에 Splash Screen API 라이브러리를 추가한다

```kotlin
dependencies {
    // Splash Screen
    implementation 'androidx.core:core-splashscreen:1.0.1'
}
```

<br>

### 2. SVG 벡터 드로어블 생성 (스플래시 로고)

스플래시 화면에 사용될 정적 SVG 로고를 벡터 드로어블로 변환한다

- `res/drawable` 폴더를 우클릭 → `New` → `Vector Asset`을 선택

- `Asset Type`을 `Local file (SVG, PSD)`로 선택
- SVG 파일을 선택하여 자동 변환한다

<br>

### 3. 테마 설정

스플래시 화면에 적용될 테마를 설정한다

- 3-1. API 31+ 전용 테마
- 3-2. 하위 API 레벨 호환성 테마

