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



## 🔥 Browser (브라우저)

### Browser

> `androidx.browser`는 앱 내에서 외부 웹 콘텐츠를 안전하게 보여주기 위한 라이브러리
>
> 시스템 브라우저(Chrome 등)의 엔진을 사용하되, 앱 위에 오버레이 형태로 띄워진다

<br>

### 주요 특징

- 사용자 경험 유지 
  - 앱을 완전히 벗어나지 않고 웹 페이지를 탐색할 수 있다

- 쿠키 및 세션 공유
  - 시스템 브라우저에 로그인된 세션을 공유할 수 있어 OAuth 로그인 등에 유리하다

- 성능
  - 브라우저 전체를 새로 실행하는 것보다 빠르며, 사전 로딩 기능을 지원한다


<br>

### Browser 활용

- OAuth 로그인 : 외부 인증 페이지 실행 (예: Apple 로그인)
- 외부 링크 이동 : 서비스 이용약관, 개인정보 처리방침 등
- 인앱 결제 : 외부 결제 모듈 호출

<br>

### 사용 예시

```kotlin
val customTabsIntent = CustomTabsIntent.Builder()
    .setShowTitle(true)
    .build()

customTabsIntent.launchUrl(context, authUrl.toUri())
```

<br>

### 의존성 추가

- `libs.versions.toml` 파일

  ```toml
  [versions]
  browser = "1.9.0"
  
  [libraries]
  browser = { module = "androidx.browser:browser", version.ref = "browser" }
  ```
  
- 버전 참고

  https://developer.android.com/jetpack/androidx/releases/browser?hl=ko
  
  https://mvnrepository.com/artifact/androidx.browser/browser
