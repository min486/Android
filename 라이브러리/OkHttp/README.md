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




## 🔥 OkHttp

### OkHttp 개념

> OkHttp는 HTTP 클라이언트 라이브러리로,
>
> Retrofit의 네트워크 엔진이자, 직접 사용 가능한 고성능 HTTP 클라이언트

<br>

### OkHttp 특징

- 비동기/동기 요청 처리 가능
- 인터셉터를 통한 요청/응답 가공 가능
- 백엔드 API 호출용으로 자주 사용

<br>

### 구성 요소

1. OkHttpClient

요청을 처리하는 중심 객체. Builder 패턴으로 구성됨

```kotlin
val client = OkHttpClient.Builder().build()
```

2. Request

HTTP 요청 정보를 담는 객체

```kotlin
val request = Request.Builder()
    .url("https://api.example.com/data")
    .build()
```

<br>

### Interceptor

인터셉터는 요청/응답 흐름을 가로채고 수정할 수 있다

<br>

### HttpLoggingInterceptor

요청/응답 로그를 출력해주는 인터셉터

```kotlin
val logging = HttpLoggingInterceptor().apply {
    level = HttpLoggingInterceptor.Level.BODY
}

val client = OkHttpClient.Builder()
    .addInterceptor(logging)
    .build()
```

로그 레벨 종류

- NONE : 로그 출력 없음
- BASIC : 요청/응답 라인만 출력
- HEADERS : 헤더까지 출력
- BODY : 본문까지 출력

<br>

### 의존성 추가

libs.versions.toml 파일에 추가

```toml
[versions]
okhttp = "4.12.0"

[libraries]
okhttp = { module = "com.squareup.okhttp3:okhttp", version.ref = "okhttp" }
okhttp-logging = { module = "com.squareup.okhttp3:logging-interceptor", version.ref = "okhttp" }
```
