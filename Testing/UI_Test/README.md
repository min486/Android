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

<br>

### UI Test 기본 구조

```kotlin
@HiltAndroidTest
@UninstallModules(RepositoryModule::class)
class KakaoLoginFlowTest {

    @get:Rule(order = 0)
    val hiltRule = HiltAndroidRule(this)

    @get:Rule(order = 1)
    val composeTestRule = createAndroidComposeRule<MainActivity>()

    @Inject
    lateinit var authRepository: AuthRepository

    @Before
    fun setup() {
        hiltRule.inject()
        (authRepository as FakeAuthRepository).shouldSuccess = true
    }

    @Test
    fun `카카오로그인 후 홈 진입`() {
        waitForNode("카카오 로그인")

        composeTestRule
            .onNodeWithText("카카오 로그인")
            .assertIsDisplayed()
            .performClick()

        waitForNode("햄버그")

        composeTestRule
            .onNodeWithText("햄버그")
            .assertIsDisplayed()
    }

    // 지정한 시간 동안 반복해서 해당 텍스트를 가진 노드 탐색
    private fun waitForNode(text: String, timeoutMillis: Long = 5_000) {
        composeTestRule.waitUntil(timeoutMillis) {
            try {
                composeTestRule
                    .onAllNodesWithText(text = text, useUnmergedTree = true)
                    .fetchSemanticsNodes(atLeastOneRootRequired = false)
                    .isNotEmpty()
            } catch (e: IllegalStateException) {
                false
            }
        }
    }
}
```

<br>

### 주요 구성 요소

#### 1. @HiltAndroidTest + @UninstallModules

- `@HiltAndroidTest` : Hilt DI 환경에서 테스트 실행
- `@UninstallModules(RepositoryModule::class)` : 실제 DI 모듈 제거 후 테스트 모듈로 교체

#### 2. Rule 순서

- `HiltAndroidRule`이 먼저(`order = 0`) 실행되어야 Hilt가 초기화된다
- 이후 `ComposeTestRule`(`order = 1`)이 Activity를 실행한다
- 순서를 잘못 지정하면 의존성 주입 전에 Activity가 실행되어 오류가 발생한다

#### 3. hiltRule.inject()

- `@Before`에서 호출하여 `@Inject`로 선언한 필드에 의존성을 주입받는다
- 이후 `(authRepository as FakeAuthRepository).shouldSuccess` 처럼 테스트 시나리오를 직접 제어할 수 있다

#### 4. waitForNode()

- `waitUntil`을 감싸는 유틸 함수
- `useUnmergedTree = true` : 전체 노드 트리를 탐색하여 더 정확하게 노드를 찾는다
- `atLeastOneRootRequired = false` : Compose가 아직 초기화되지 않은 상태에서도 예외 없이 탐색을 시도한다
- `IllegalStateException` catch : 초기화 이전 타이밍에 발생하는 예외를 무시하고 재시도한다

<br>

### 의존성 추가

- libs.versions.toml 파일

  ```toml
  [versions]
  hilt = "2.56.2"
  
  [libraries]
  hilt-android-testing = { module = "com.google.dagger:hilt-android-testing", version.ref = "hilt" }
  hilt-compiler = { module = "com.google.dagger:hilt-compiler", version.ref = "hilt" }
  androidx-compose-ui-test-junit4 = { group = "androidx.compose.ui", name = "ui-test-junit4" }
  androidx-compose-ui-test-manifest = { group = "androidx.compose.ui", name = "ui-test-manifest" }
  ```
  
- 버전 참고

  https://mvnrepository.com/artifact/com.google.dagger/hilt-android-testing

  https://mvnrepository.com/artifact/androidx.compose.ui/ui-test-junit4
