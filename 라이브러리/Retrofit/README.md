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




## 🔥 Retrofit

### Retrofit 개념

> 서버와 JSON 데이터를 주고받는 네트워크 코드를 최소한의 코드로 작성할 수 있게 도와주는 라이브러리

<br>

### Retrofit 특징

- 간단한 선언형 인터페이스 기반 설계

  👉 함수에 어노테이션(@GET, @POST 등)을 붙여 간단하게 API 호출 정의 가능

- 자동 JSON 직렬화/역직렬화 지원

  👉 Gson, Moshi 등의 컨버터를 통해 JSON을 객체로 쉽게 변환

- OkHttp 기반

  👉 성능 좋고 확장성 뛰어난 OkHttp를 내부적으로 사용

- Coroutine 및 Flow 연동 지원

  👉 suspend 함수로 비동기 API 호출을 간결하게 처리 가능

- 높은 가독성과 유지보수성

  👉 반복적인 코드 없이 명확하게 API 구조를 표현할 수 있음

<br>

### Retrofit 사용 용도

- 서버로부터 데이터를 가져오기(GET) 또는 전송하기(POST, PUT 등)
- RESTful API와의 통신을 간결한 방식으로 처리
- JSON 응답 데이터를 모델 클래스 객체로 자동 변환
- 다양한 HTTP 요청 방식, 쿼리 파라미터, 헤더 설정 등 유연한 요청 구성 가능

<br>

### 의존성 추가

app 수준의 build.gradle 파일에 의존성 추가

```kotlin
// Retrofit
implementation("com.squareup.retrofit2:retrofit:2.11.0")
// Gson Converter (JSON → 객체 자동 변환)
implementation("com.squareup.retrofit2:converter-gson:2.11.0")
```

<br>

### 사용 예시

1. AndroidManifest.xml 파일에서 인터넷 권한 설정

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">
    
  	<!-- 인터넷 권한 -->
    <uses-permission android:name="android.permission.INTERNET"/>

    <application
        ...
   	</application>
</manifest>
```
