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




## 🔥 Retrofit 2

### Retrofit 2

- HTTP 통신을 간단하고 안전하게 처리할 수 있도록 도와주는 라이브러리

- 복잡한 네트워크 코드를 직접 작성하지 않고, 인터페이스와 어노테이션을 이용해 REST API 요청을 선언적으로 정의할 수 있다

<br>

### Retrofit 2 특징

- 선언적 API 정의

  `@GET`, `@POST` 등의 어노테이션을 사용하여 HTTP 요청을 명확하게 표현한다

- 다양한 컨버터 지원

  JSON, XML 등 서버 응답을 kotlin 객체로 자동 변환하는 컨버터(converter)를 제공한다

- 비동기/동기 통신 지원

  Coroutines, RxJava 등과 연동하여 비동기 요청을 쉽게 처리할 수 있으며, 동기도 가능하다

- OkHttp 기반

  OkHttp를 내부적으로 사용하며, OkHttp의 다양한 기능(인터셉터, 캐싱 등)을 활용할 수 있다

<br>

### Retrofit 2 활용

Retrofit은 주로 클라이언트-서버 구조의 앱에서 서버와 데이터를 주고받을 때 사용된다

예시 :

- 날씨 앱 → 외부 API에서 날씨 데이터를 가져와 화면에 표시
- 소셜 미디어 앱 → 게시글 목록 조회, 게시글 작성/업로드
- 쇼핑 앱 → 상품 목록, 주문 내역 등 서버 데이터와 연동

<br>

### Kotlin Serialization 

- JSON 직렬화/역질렬화

  Kotlinx Serialization은 kotlin 객체를 JSON 문자열로 변환(직렬화)하거나,

  반대로 JSON 문자열을 kotlin 객체로 변환(역직렬화)하는데 사용된다

- 타입 안전성

  컴파일 시점에 데이터 모델 타입을 검증해 런타임 오류를 줄인다

- `@Serializable` 어노테이션

  데이터 클래스에 이 어노테이션을 추가하면 직렬화/역직렬화 대상임을 명시할 수 있다

<br>

### Retrofit 2와 Serialization 통합

- Retrofit → 네트워크 통신 (HTTP 요청/응답 처리)
- Kotlinx Serialization → 데이터 변환 (JSON ↔ Kotlin 객체)

👉 두 라이브러리를 함께 사용하면, REST API 통신 결과를 쉽게 kotlin 객체로 변환해 안전하게 다룰 수 있다

<br>

### 의존성 추가

Retrofit 2와 Kotlinx Serialization을 함께 사용하기 위해서 아래와 같이 설정한다

- libs.versions.toml

  ```toml
  [versions]
  kotlin = "2.2.0"
  retrofit = "3.0.0"
  kotlinSerialization = "1.9.0"
  
  [libraries]
  retrofit = { module = "com.squareup.retrofit2:retrofit", version.ref = "retrofit" }
  retrofit-converter-kotlinx-serialization = { module = "com.squareup.retrofit2:converter-kotlinx-serialization", version.ref = "retrofit" }
  kotlinx-serialization-json = { module = "org.jetbrains.kotlinx:kotlinx-serialization-json", version.ref = "kotlinSerialization" }
  
  [plugins]
  kotlin-serialization = { id = "org.jetbrains.kotlin.plugin.serialization", version.ref = "kotlin" }
  ```

- build.gradle.kts (앱 수준)

  ```kotlin
  plugins {
      alias(libs.plugins.kotlin.serialization)
  }
  
  dependencies {
      // retrofit2 & kotlin serialization
      implementation(libs.retrofit)
      implementation(libs.retrofit.converter.kotlinx.serialization)
      implementation(libs.kotlinx.serialization.json)
  }
  ```
