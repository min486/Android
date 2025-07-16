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




## 🔥 HiltViewModel

### HiltViewModel 개념

> Hilt는 Android의 의존성 주입(DI)을 위한 라이브러리이며,
>
> @HiltViewModel은 Hilt를 통해 ViewModel에 필요한 의존성을 주입할 수 있도록 연결해주는 어노테이션

<br>

### HiltViewModel 특징

- ViewModel에 필요한 의존성을 Hilt를 통해 생성자 주입으로 받을 수 있다
- ViewModel의 생명주기를 Composable 또는 Activity에 맞춰 자동으로 관리해준다
- 동일한 Activity 범위 내에서는 동일한 ViewModel 인스턴스를 공유할 수 있다

<br>

### 사용 순서

1. 프로젝트 설정
   - 프로젝트 수준 build.gradle에 Hilt 플러그인 추가
   - 앱 수준 build.gradle에 의존성 추가
   - Hilt Application 클래스 생성
2. ViewModel 구현
   - ViewModel 클래스 생성 후 @HiltViewModel 어노테이션 추가
   - 생성자에 @Inject로 의존성 주입
3. Compose에서 사용
   - Activity에 @AndroidEntryPoint 어노테이션 추가
   - Composable에서 hiltViewModel() 함수를 통해 ViewModel 인스턴스 획득

<br>

### 의존성 추가

- Hilt 의존성 추가

  - libs.versions.toml 파일

    ```toml
    [versions]
    # ...
    hilt = "2.56.2"
    ksp = "2.0.21-1.0.28"
    
    [libraries]
    # ...
    hilt-android = { module = "com.google.dagger:hilt-android", version.ref = "hilt" }
    hilt-compiler = { module = "com.google.dagger:hilt-android-compiler", version.ref = "hilt" }
    
    [plugins]
    # ...
    hilt = { id = "com.google.dagger.hilt.android", version.ref = "hilt" }
    ksp = { id = "com.google.devtools.ksp", version.ref = "ksp" }
    ```

  - 프로젝트 수준 build.gradle 파일

    ```kotlin
    plugins {
        // ...
        alias(libs.plugins.ksp) apply false
        alias(libs.plugins.hilt) apply false
    }
    ```

  - app/build.gradle 파일

    ```kotlin
    plugins {
        // ...
        alias(libs.plugins.ksp)
        alias(libs.plugins.hilt)
    }
    
    android {
        // ...
    }
    
    dependencies {
        // ...
        implementation(libs.hilt.android)
        ksp(libs.hilt.compiler)
    }
    ```

- Hilt with Compose Navigation 의존성 추가

  > `hiltViewModel()` 사용 가능하게함
  >
  > Hilt와 Navigation Compose 연동을 위한 라이브러리

  - libs.versions.toml 파일

    ```toml
    [versions]
    # ...
    hiltNavigationCompose = "1.2.0"
    
    [libraries]
    # ...
    hilt-navigation-compose = { module = "androidx.hilt:hilt-navigation-compose", version.ref = "hiltNavigationCompose" }
    ```

  - app/build.gradle 파일

    ```kotlin
    dependencies {
        // ...
        implementation(libs.hilt.navigation.compose)
    }
    ```

<br>

### 사용 예시

1. Application 설정

```kotlin
@HiltAndroidApp
class MyApplication : Application()  // Hilt 사용을 위한 진입점 등록
```

2. 의존성으로 사용할 Repository 클래스

```kotlin
class TestRepository @Inject constructor() {  // DI 대상 클래스
    ...
}
```

3. HiltViewModel 구현

```kotlin
class TestViewModel @Inject constructor(
    private val testRepository: TestRepository
) : ViewModel() {
    ...
}
```

4. Activity 설정

```kotlin
@AndroidEntryPoint
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            ...
        }
    }
}
```

5. Composable에서 ViewModel 사용

```kotlin
// 상위 Composable에서 hiltViewModel() 으로 가져온 ViewModel 인스턴스를
// 하위 Composable에 매개변수로 전달
@Composable
fun MainScreen() {
    val testViewModel = hiltViewModel<TestViewModel>()
    ...
    TestScreen(testViewModel)
}

// 동일한 ViewModel 인스턴스를 공유해서 사용
@Composable
fun TestScreen(viewModel: TestViewModel) {
    ...
}
```

