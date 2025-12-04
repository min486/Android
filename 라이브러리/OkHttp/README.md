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




## 🔥 OkHttp 기반 네트워킹 및 JWT 인증 관리

### 1. OkHttp

> OkHttp는 Retrofit의 기반이 되는 고성능 HTTP 클라이언트 라이브러리

- Retrofit이 REST API 요청을 인터페이스 기반으로 추상화해준다면,
- 실제 네트워크 통신은 OkHttp가 담당한다 (Socket 연결, 요청/응답 처리)

<br>

#### 주요 구성 요소

- OkHttpClient
  - 요청을 처리하는 중심 객체
  - 모든 요청의 설정을 관리한다 (타임아웃, 인터셉터, 인증자)
  - 예시 : `OkHttpClient.Builder().build()`
- Request
  - 전송할 HTTP 요청 정보를 담는 객체 (URL, 헤더, 메서드, 바디)
  - 예시 : `Request.Builder().url(...).build)`
- Response
  - 서버로부터 받은 HTTP 응답 정보를 담는 객체
  - 예시 : `chain.proceed(request)`의 결과

<br>

### 2. 인터셉터를 활용한 요청 가공

`okhttp3.Interceptor`는 HTTP 요청/응답의 흐름을 가로채서 원하는 작업을 수행하도록 한다

요청이 서버로 가기 전, 또는 응답이 앱으로 돌아오기 전에 공통 로직을 주입할 때 사용된다

#### 2-1. HttpLoggingInterceptor

요청과 응답의 상세 정보를 Logcat에 출력하여 디버깅을 돕는 인터셉터

```kotlin
@Provides
@Singleton
fun provideLoggingInterceptor(): HttpLoggingInterceptor {
    return HttpLoggingInterceptor().apply {
        // Logcat에 요청/응답의 바디까지 출력하도록 설정
        level = HttpLoggingInterceptor.Level.BODY
    }
}
```

*로그 레벨 종류

- HEADERS : 헤더까지 출력
- BODY : 본문까지 출력
- BASIC : 요청/응답 라인만 출력
- NONE : 로그 출력 없음

<br>

#### 2-2. AuthInterceptor

모든 API 요청 헤더에 저장된 JWT Access Token을 자동으로 주입하는 역할 담당

```kotlin
class AuthInterceptor @Inject constructor(
    private val tokenManager: HambugTokenManager
) : Interceptor {

    override fun intercept(chain: Interceptor.Chain): Response {
        val originalRequest = chain.request()
      
        // 토큰이 필요없는 경로는 바로 요청 진행 (로그인 등)
        val requestPath = originalRequest.url.encodedPath        
        if (NO_AUTH_PATH.any { path -> requestPath.contains(path) }) {
            return chain.proceed(originalRequest)
        }

        // Access Token 가져와서
        val accessToken = runBlocking { tokenManager.getAccessToken() }

        // Authorization 헤더에 Bearer 토큰 형식으로 추가
        val newRequest = if (accessToken != null) {
            originalRequest.newBuilder()
                .header("Authorization", "Bearer $accessToken")
                .build()
        } else {
            originalRequest
        }

        return chain.proceed(newRequest)
    }
}
```

<br>

### 3. 인증자를 활용한 토큰 자동 갱신

`okhttp3.Authenticator`는 서버로부터 401 Unauthorized 응답을 받았을 때,

자동으로 인증 정보를 갱신하고 실패한 요청을 재시도하도록 처리한다

```kotlin
class TokenAuthenticator(
    private val tokenManager: HambugTokenManager,
    private val refreshRetrofit: Retrofit
) : Authenticator {

    private val tokenRefreshMutex = Mutex()

    override fun authenticate(route: Route?, response: Response): Request? {
        // 401 응답이 아닌 경우 null 반환
        if (response.code != 401) return null
      
        // (Mutex 락을 사용하여 동시 요청 방지 로직)

        return runBlocking {
            // refresh token이 있는지 확인, 없으면 로그아웃 처리
            val oldRefreshToken = tokenManager.getRefreshToken()
            if (oldRefreshToken == null) {
                tokenManager.logout()
                return@runBlocking null
            }

            // Mutex 락을 사용하여 토큰 갱신 요청을 한번만 실행하도록 보장
            val newAccessToken = tokenRefreshMutex.withLock {
                // (2차 체크)

                // Refresh API 호출 및 새 Access Token 저장
                val refreshApi = refreshRetrofit.create(RefreshApi::class.java)
                val refreshResponse = refreshApi.refreshToken("Bearer $oldRefreshToken")

                if (refreshResponse.success) {
                    val newToken = refreshResponse.data
                    tokenManager.saveAccessToken(newToken)
                    newToken
                } else {
                    tokenManager.logout()
                    null
                }
            }

            // 갱신 성공 시, 원래 요청을 새 토큰으로 재시도
            return@runBlocking if (newAccessToken != null) {
                response.request.newBuilder()
                    .header("Authorization", "Bearer $newAccessToken")
                    .build()
            } else {
                null
            }
        }
    }
}
```

👉 로직 설명

- Mutex 사용

  - 여러 API 요청이 401을 받아 `authenticate`가 호출될 때,

    Refresh Token 갱신 요청이 여러 번 발생하는 것을 막기 위해 `kotlinx.coroutines.sync.Mutex` 사용

  - 토큰 갱신 로직은 Lock 내부에서 한 번만 실행됨

- 이중 체크

  - 락 진입 전 : 이미 토큰이 갱신된 경우를 확인하여 불필요한 갱신 방지
  - 락 내부 : 갱신이 완료되는 동안 대기했던 요청이 갱신 후에도 중복 갱신을 시도하지 않도록 다시 확인

- Refresh 전용 Client

  Authenticator 내부에서 Refresh API를 호출할 때, 일반 API 클라이언트를 사용하면 순환 참조가 발생한다

  - 일반 Client → Authenticator 실행 → Authenticator 내부에서 다시 Client 호출 → 무한 루프

  - 이를 방지하기 위해 `@RefreshOkHttpClient` 와 `@RefreshRetrofit` Hilt Qualifier를 사용하여

    `AuthInterceptor` 와 `Authenticator` 가 빠진 별도의 클라이언트를 구성하고, 이를 Refresh API 로출에 사용해야 한다

<br>

### 4. Hilt를 활용한 OkHttpClient 구성

```kotlin
@Provides
@Singleton
fun provideOkHttpClient(
    loggingInterceptor: HttpLoggingInterceptor,
    authInterceptor: AuthInterceptor,
    tokenManager: HambugTokenManager,
    @RefreshRetrofit refreshRetrofit: Retrofit
): OkHttpClient {
    // 인증자 생성 (Refresh 전용 Retrofit 주입)
    val tokenAuthenticator = TokenAuthenticator(tokenManager, refreshRetrofit)

    return OkHttpClient.Builder()
        // ... 타임아웃 설정
        .addInterceptor(loggingInterceptor)  // 로깅 인터셉터
        .addInterceptor(authInterceptor)     // 인증 인터셉터 (토큰 주입)
        .authenticator(tokenAuthenticator)   // 인증자 (401 에러 처리)
        .build()
}
```

<br>

### OkHttp 의존성 추가

- `libs.versions.toml` 파일

  ```toml
  [versions]
  okhttp = "5.0.0"
  
  [libraries]
  okhttp = { module = "com.squareup.okhttp3:okhttp", version.ref = "okhttp" }
  okhttp-logging = { module = "com.squareup.okhttp3:logging-interceptor", version.ref = "okhttp" }
  ```

- 버전 참고

  https://square.github.io/okhttp/

  https://mvnrepository.com/artifact/com.squareup.okhttp3/okhttp
