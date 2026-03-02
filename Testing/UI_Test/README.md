<div align="center">
  <p>
    <img src="../README.assets/android.png">
  </p>
  <br>
  <h2>테스트 코드</h2>
  <p>테스트 코드 관련 내용 정리</p>
  <br>
  <br>
</div>





## 🔥 UI Test

### UI Test 개요

> 실제 디바이스 또는 에뮬레이터에서 앱을 실행하며 UI 흐름을 검증하는 테스트

- Unit Test와 달리 실제 Activity, Composable, Navigation이 동작하는 환경에서 진행된다
- 해당 문서는 `Hilt + Compose UI Test + Fake Repository` 구조를 기준으로, 카카오로그인 흐름 테스트를 예시로 구성함

<br>

### UI Test 라이브러리

- Hilt Testing

  > Hilt DI 환경에서 테스트용 의존성을 주입하기 위한 라이브러리

  - `hilt-android-testing` : 테스트 전용 DI 환경 구성


  - `@HiltAndroidTest`, `HiltAndroidRule`로 Hilt 초기화
    

  - `@UninstallModules`로 실제 모듈을 제거하고 테스트 모듈로 교체

- Compose UI Test

  > Jetpack Compose 화면을 직접 조작하고 검증하는 테스트 라이브러리

  - `createAndroidComposeRule<Activity>()`로 실제 액티비티 기반 테스트 환경 구성

  - `onNodeWithText`, `performClick`, `assertIsDisplayed` 등으로 UI 요소 조작 및 검증

  - `waitUntil`로 비동기 UI 상태 변환 대기 가능

<br>

### 주요 구성 요소

#### 1. HiltTestRunner

Hilt 기반 UI 테스트를 실행하려면 기본 `AndroidJUnitRunner` 대신 `HiltTestApplication`을 사용하는 커스텀 러너가 필요하다

```kotlin
class HiltTestRunner : AndroidJUnitRunner() {
    override fun newApplication(cl: ClassLoader?, className: String?, context: Context?): Application {
        return super.newApplication(cl, HiltTestApplication::class.java.name, context)
    }
}
```

`app/build.gradle`에 러너를 등록한다

```kotlin
android {
    defaultConfig {
        testInstrumentationRunner = "desktop.hambug.HiltTestRunner"
    }
}
```

<br>

#### 2. FakeRepository

실제 네트워크나 카카오 SDK 호출 없이 테스트하기 위해, 실제 `AuthRepository`를 대체하는 가짜 구현체를 만든다

```kotlin
class FakeAuthRepository : AuthRepository {

    // 플래그로 성공/실패 시나리오를 간단히 전환할 수 있다
    var shouldSuccess = true
    // 특정 예외 상황도 재현 가능하다
    var shouldThrowSpecificError: Exception? = null

    override suspend fun loginWithKakao(context: Context): LoginResponse {
        shouldThrowSpecificError?.let { throw it }

        if (!shouldSuccess) throw Exception("mock kakao login failed")

        return LoginResponse(
            token = LoginToken(
                accessToken = "fake_access_token",
                refreshToken = "fake_refresh_token"
            ),
            user = LoginUser(
                isRegister = true,
                kakao = true,
                loginType = "KAKAO",
                nickname = "HAMBUG_0123456789",
                profileImageUrl = null,
                role = "ROLE_USER",
                userId = 1
            )
        )
    }

    // ...
}
```

<br>

#### 3. TestModule (DI 모듈 교체)

`@UninstallModules`로 실제 `RepositoryModule`을 제거하고, `FakeAuthRepository`를 제공하는 테스트 전용 모듈로 교체한다

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object TestAuthModule {

    @Provides
    @Singleton
    fun provideAuthRepository(): AuthRepository {
        return FakeAuthRepository()
    }

    // 나머지 Repository는 실제 구현체를 그대로 사용
    @Provides
    @Singleton
    fun provideHomeRepository(
        api: HambugApi
    ): HomeRepository {
        return HomeRepositoryImpl(api)
    }

    // ...
}
```

<br>

#### 4. TestEnvironment (권한 요청 우회)

UI 테스트 중 알림 권한 요청 다이얼로그가 뜨면 테스트 흐름이 막히는 이슈가 있다

이를 해결하기 위해 `TestEnvironment` 인터페이스를 도입하여 테스트 환경 여부를 런타임에 전달한다

```kotlin
interface TestEnvironment {
    fun isFakeTest(): Boolean
}

class ProductionTestEnvironment : TestEnvironment {
    override fun isFakeTest(): Boolean = false
}

class FakeTestEnvironment : TestEnvironment {
    override fun isFakeTest(): Boolean = true
}
```

`LoginScreen`에서는 `isFakeTest()`가 `false`일 때만 권한 요청을 수행한다

```kotlin
LaunchedEffect(Unit) {
    // 테스트 환경에서는 권한 요청 생략
    if (!testEnvironment.isFakeTest() &&
        !NotificationPermissionHelper.hasNotificationPermission(context)) {
        notificationPermissionLauncher.launch(Manifest.permission.POST_NOTIFICATIONS)
    }
}
```

`TestUtilModule`에서 `UtilModule`을 `@TestInstallIn`으로 교체하여 테스트 환경에 `FakeTestEnvironment`를 주입한다

```kotlin
@Module
@TestInstallIn(
    components = [SingletonComponent::class],
    replaces = [UtilModule::class]
)
object TestUtilModule {

    @Provides
    @Singleton
    fun provideTestEnvironment(): TestEnvironment {
        return FakeTestEnvironment()
    }
}
```
