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





## 🔥 Unit Test

### Unit Test 개요

> Unit Test는 비즈니스 로직을 앱 실행 없이 검증할 수 있는 테스트

ViewModel, UseCase, Repository 등 DI 기반 구조에서 테스트하기 유용하다

해당 문서는 Kotlin 기반 Unit Test 환경(`Kotest + Coroutines Test + MockK`)을 기준으로 구성했다

<br>

### Unit Test 라이브러리

1. Kotest

   > 코틀린 친화적인 테스트 프레임워크

   - kotest-runner-junit5-jvm

     - JUnit5 기반 테스트 러너

     - FunSpec, StringSpec 등 다양한 테스트 스타일 제공
     - `test("테스트 설명") { }` 형태로 한글 테스트명 사용 가능

   - kotest-assertions-core-jvm
     - 직관적인 Assertion 함수 제공
     - 예: shouldBe, shouldContain, shouldBeInstanceOf 등

2. Kotlinx Coroutines Test

   > 코루틴 기반 비동기 코드를 동기적으로 테스트하기 위한 라이브러리

   - runTest 블록을 통해 suspend 함수 검증
   - Main Dispatcher 교체 가능 (`Dispatchers.setMain()`)
   - TestDispatcher로 delay, 순서, 타이밍 테스트 가능

3. MockK

   > 코틀린 전용 모킹(Mocking) 라이브러리

   - UseCase, Repository 등 의존성을 가짜(Mock) 객체로 대체
   - 실제 DB/네크워크 없이 테스트 가능
   - Mockito 대비 코틀린에서 안정적

<br>

### TestDispatcher 비교

> StandardTestDispatcher vs UnconfinedTestDispatcher
>
> StandardTestDispatcher 사용이 권장된다

- StandardTestDispatcher

  - 실제 코루틴 동작과 유사 : 코루틴을 즉시 실행하지 않고 큐에 쌓음
  - 실행 시점 명시적 제어 가능 : `advanceUntilIdle()`
  - delay, 순서 테스트 가능 (예측 가능)
  - 테스트 안정성이 높음

- UnconfinedTestDispatcher

  - 코루틴 즉시 실행
  - 예측 불가능한 타이밍 발생 가능

- 동작 차이 예시

  ```kotlin
  @OptIn(ExperimentalCoroutinesApi::class)
  class DispatcherComparisonTest : FunSpec({
      
      test("StandardTestDispatcher - 명시적 제어") {
          val dispatcher = StandardTestDispatcher()
          val output = mutableListOf<String>()
          
          runTest(dispatcher) {
              launch { output.add("1") }  // 실행되지 않음
              output.add("2")
              
              dispatcher.scheduler.advanceUntilIdle()  // 명시적으로 실행
          }
          
          output shouldBe listOf("2", "1")  // 2가 먼저 출력
      }
      
      test("UnconfinedTestDispatcher - 즉시 실행") {
          val dispatcher = UnconfinedTestDispatcher()
          val output = mutableListOf<String>()
          
          runTest(dispatcher) {
              launch { output.add("1") }  // 즉시 실행
              output.add("2")
          }
          
          output shouldBe listOf("1", "2")  // 1이 먼저 출력
      }
  })
  ```


<br>

### Unit Test 기본 구조 (FunSpec 스타일)

```kotlin
@OptIn(ExperimentalCoroutinesApi::class)
class HomeViewModelTest : FunSpec({

    // 테스트용 Dispatcher 생성
    val testDispatcher = StandardTestDispatcher()

    // Mock 객체 생성
    val getCurrentUserIdUseCase: GetCurrentUserIdUseCase = mockk()
    val getUserDataUseCase: GetUserDataUseCase = mockk()
    val getUserRecordUseCase: GetUserRecordUseCase = mockk()

    // 테스트 대상 변수
    lateinit var viewModel: HomeViewModel

    // 생명주기 훅
    beforeTest {
        Dispatchers.setMain(testDispatcher)  // Main Dispatcher 교체
    }

    afterTest {
        Dispatchers.resetMain()  // 원래대로 복원
    }

    // 테스트 케이스
    test("테스트 설명") {
        // 테스트 코드...
    }
})
```

<br>

### 주요 구성 요소

1. beforeTest / afterTest

   - 각 테스트 실행 전후에 실행되는 훅
   - 테스트 환경에서는 Main dispatcher가 없음
   - `setMain()` 으로 TestDispatcher로 교체 필수

2. coEvery (MockK)

   - suspend 함수의 동작을 모킹
   - `returns` : 정상 반환값 설정
   - `throws` : 예외 던지기
   - `returnsMany` : 여러번 호출 시 다른 값 반환
   - 일반 함수는 `every` 사용

   ```kotlin
   coEvery { getCurrentUserIdUseCase() } returns fakeUid
   coEvery { getUserDataUseCase(fakeUid) } returns fakeUser
   coEvery { getUserRecordUseCase() } throws Exception("DB 연결 오류")
   ```

3. advanceUntillIdle()

   - StandardTestDispatcher 사용 시 필수 (코루틴을 큐에만 쌓고 실행 안하기 때문)
   - 큐에 있는 모든 코루틴 작업을 완료시킴

   ```kotlin
   viewModel = HomeViewModel(...)               // init 블록에서 코루틴 실행
   testDispatcher.scheduler.advanceUntilIdle()  // 모든 코루틴 완료 대기
   ```

4. Kotest Assertions

   - `shouldBe` : 값 동일성 (==)
   - `shouldBeInstanceOf<T>()` : 타입 검증 및 스마트 캐스트
   - `shouldContain` : 문자열/컬렉션 포함 여부

   - `shouldNotBe` : 값 불일치
   - `shouldBeNull()`, `shouldNotBeNull()` : null 검증

<br>

### app/build.gradle 설정

```kotlin
dependencies {
    // ...
    // Unit Test
    testImplementation(libs.kotest.runner)
    testImplementation(libs.kotest.assertion)
    testImplementation(libs.kotlinx.coroutines.test)
    testImplementation(libs.mockk)
}

// Kotest 사용을 위한 JUnit5 설정
tasks.withType<Test>().configureEach {
    useJUnitPlatform()
}
```

👉 설정이 필요한 이유

- 이 설정으로 Kotest는 JUnit5 환경을 사용해서 실행됨
- 이 설정이 없으면 Gradle이 Kotest로 작성된 코드를 일반 코드로 간주하여 테스트 실행 불가

<br>

### 의존성 추가

- libs.versions.toml 파일

  ```toml
  [versions]
  kotest = "5.8.1"
  mockk = "1.14.0"
  coroutines = "1.10.2"
  
  [libraries]
  kotest-runner = { module = "io.kotest:kotest-runner-junit5-jvm", version.ref = "kotest" }
  kotest-assertion = { module = "io.kotest:kotest-assertions-core-jvm", version.ref = "kotest" }
  mockk = { module = "io.mockk:mockk", version.ref = "mockk" }
  kotlinx-coroutines-test = { module = "org.jetbrains.kotlinx:kotlinx-coroutines-test", version.ref = "coroutines" }
  ```

- 버전 참고

  https://mvnrepository.com/artifact/io.kotest/kotest-runner-junit5-jvm

  https://mvnrepository.com/artifact/io.kotest/kotest-assertions-core-jvm

  https://mvnrepository.com/artifact/io.mockk/mockk

  https://mvnrepository.com/artifact/org.jetbrains.kotlinx/kotlinx-coroutines-test
