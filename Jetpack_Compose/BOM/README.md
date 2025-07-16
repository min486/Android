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




## 🔥 Jetpack Compose BOM (Bill of Materials)

### Compose BOM 이란?

- Compose BOM은 특정 Compose 릴리스와 호환되는 모든 Compose 라이브러리의 권장 버전 집합을 정의한 Gradle 기능이다

- BOM을 사용하면 개발자가 각 Compose 라이브러리의 버전을 수동으로 관리할 필요 없이,

  Gradle이 BOM에 명시된 버전을 기반으로 자동으로 올바른 라이브러리를 가져온다

<br>

*Jetpack Compose는 여러개의 개별 라이브러리로 구성되어 있으며, 이 라이브러리들은 특정 버전에서 서로 호환되도록 설계되어 있다

<br>

### BOM 사용 이유

- 버전 불일치 방지

  Compose 라이브러리 간의 호환성 문제를 방지하고, 런타임 오류를 줄여준다

- 관리의 용이성

  새로운 Compose 버전이 나올 때마다 각 라이브러리 버전을 수동으로 관리할 필요 없이, BOM 버전만 업데이트하면 된다

- 의존성 충돌 감소

  Compose 관련 모든 라이브러리의 일관된 버전 관리를 통해 의존성 충돌 가능성을 줄여준다

<br>

### BOM 적용 방법

app 수준의 `build.gradle` 파일에서 BOM을 추가하고, Compose 관련 라이브러리의 개별 버전 명시는 생략한다

```kotlin
dependencies {
    // 1. Compose BOM을 추가
    // 'platform' 키워드를 사용하여 BOM이 포함된 라이브러리들의 버전을 통합 관리
    implementation(platform("androidx.compose:compose-bom:2025.05.00"))  // 예시 버전

    // 2. Compose 라이브러리의 버전는 개별 버전 없이 선언
    implementation("androidx.compose.ui:ui")
    implementation("androidx.compose.ui:ui-graphics")
    implementation("androidx.compose.material3:material3")

    // 3. Compose BOM에 포함되지 않은 라이브러리는 여전히 버전 명시 필요
    implementation("com.google.dagger:hilt-android:2.56.2")
    ksp("com.google.dagger:hilt-compiler:2.56.2")
}
```
