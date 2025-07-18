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




## 🔥 Hilt

### 의존성 주입 (Dependency Injection)

> 클래스가 직접 의존 객체를 생성하지 않고, 외부에서 주입받도록 설계하는 패턴

```kotlin
// Without DI
class Car {
  private val engine: Engine = Engine()  // Car가 Engine에 강하게 의존
}

// With DI
class Car(private val engine: Engine)  // 외부에서 Engine을 주입받음
```

<br>

### DI가 필요한 이유

- `Car`가 직접 `Engine`을 생성하면, 테스트나 다른 Engine으로 교체가 어려워짐

  <img src="../README.assets/di.png" alt="di" align="center" width="30%" />

- `Car` 외부에서 `Engine`을 주입하면, 유연하고 테스트가 편해짐

  <img src="../README.assets/di2.png" alt="di2" align="center" width="30%" />

- 각 클래스는 자신의 책임에만 집중할 수 있음

<br>

### DI의 장점

- 결함도 감소 : 클래스 간 의존성이 줄어든다
- 테스트 용이성 : Mock 객체 주입으로 단위 테스트 가능
- 모듈화 : 각 모듈이 독립적으로 개발/유지보수 가능

- 재사용성 : 동일한 의존성을 여러 클래스에서 재사용
- 확장성 : 새로운 구현체로 쉽게 교체 가능

<img src="../README.assets/di3.png" alt="di3" align="center" width="40%" />

👉 거대한 클래스를 가지는 대신 여러 부품으로 나눠서 자동차를 작게 만든다

<br>

### Hilt

> Jetpack 공식 DI 프레임워크로, 내부적으로 Dagger2 기반이며 안드로이드에 최적화된 DI 환경을 제공

<br>

### Hilt 특징

- Android 컴포넌트 생명주기 자동 관리

- 컴포넌트별 스코프 제공 (Activity, ViewModel 등)

- 설정과 테스트가 쉬움

- 보일러플레이트 코드 감소

  *Boilerplate code : 여러곳에서 재사용되며, 반복적으로 비슷한 형태를 띄는 코드

<br>

### Hilt 기본 구성 요소

1. `@HiltAndroidApp`

   Application 클래스에 붙여 hilt가 DI를 시작할 수 있는 루트를 제공

   ```kotlin
   @HiltAndroidApp
   class MyApp : Application()
   ```

   이후 `AndroidManifest.xml`에서 아래 내용 추가

    ```xml
    <application
        android:name=".MyApp"
        ...
    </application>
    ```

2. `@AndroidEntryPoint`

   의존성 주입을 받을 Android 컴포넌트에 사용

   ```kotlin
   @AndroidEntryPoint
   class MainActivity : ComponentActivity() {
       // hilt가 자동으로 의존성을 주입해줌
   }
   ```

3. `@Inject`

   생성자 또는 필드에 사용해 hilt가 의존성을 제공하도록 함

   ```kotlin
   // 생성자 주입
   class UserRepository @Inject constructor(
       private val api: UserApi
   )
   
   // 필드 주입
   @AndroidEntryPoint
   class MainActivity : ComponentActivity() {
       @Inject
       lateinit var userRepository: UserRepository
   }
   ```

4. `@Module` + `@Provides`

   인터페이스나 외부 라이브러리 클래스 등 hilt가 생성할 수 없는 타입을 제공할 때 사용

   ```kotlin
   @Module
   @InstallIn(SingletonComponent::class)
   object NetworkModule {
   
       @Provides
       @Singleton
       fun provideUserApi(retrofit: Retrofit): UserApi {
           return retrofit.create(UserApi::class.java)
       }
   }
   ```

5. `@Binds` (인터페이스 바인딩)

   인터페이스와 구현체를 연결할 때 `@Provides`보다 효율적

   ```kotlin
   @Module
   @InstallIn(SingletonComponent::class)
   abstract class RepositoryModule {
   
       @Binds
       abstract fun bindUserRepository(
           userRepositoryImpl: UserRepositoryImpl
       ): UserRepository
   }
   ```

6. Scope 어노테이션 (생명주기 관리)

   의존성의 생명주기를 설정하는 역할

   | Scope                   | 생명주기                  | 설명                                      |
   | ----------------------- | ------------------------- | ----------------------------------------- |
   | @Singleton              | 전체 앱                   | 앱이 실행되는 동안 하나의 인스턴스만 생성 |
   | @ActivityScoped         | Activity                  | Activity가 생성~소멸까지 동일한 인스턴스  |
   | @ActivityRetainedScoped | Activity (화면 회전 유지) | Configuration change 시에도 유지됨        |
   | @ViewModelScoped        | ViewModel                 | ViewModel 생명주기와 동일                 |
   | @ServiceScoped          | Service                   | Service 생명주기와 동일                   |

   ```kotlin
   // 앱 전체에서 하나의 인스턴스만 생성
   @Singleton
   class UserRepository @Inject constructor(
       private val api: UserApi
   )
   
   // Activity 범위 내에서 하나의 인스턴스
   @ActivityScoped
   class LocationTracker @Inject constructor(
       private val context: Context
   )
   ```

<br>

### 의존성 추가

- libs.versions.toml 파일

  ```toml
  [versions]
  hilt = "2.56.2"
  ksp = "2.0.21-1.0.28"
  hiltNavigationCompose = "1.2.0"
  
  [libraries]
  hilt-android = { module = "com.google.dagger:hilt-android", version.ref = "hilt" }
  hilt-compiler = { module = "com.google.dagger:hilt-android-compiler", version.ref = "hilt" }
  hilt-navigation-compose = { module = "androidx.hilt:hilt-navigation-compose", version.ref = "hiltNavigationCompose" }
  
  
  [plugins]
  hilt = { id = "com.google.dagger.hilt.android", version.ref = "hilt" }
  ksp = { id = "com.google.devtools.ksp", version.ref = "ksp" }
  ```

- 프로젝트 수준 build.gradle 파일

  ```kotlin
  plugins {
      alias(libs.plugins.ksp) apply false
      alias(libs.plugins.hilt) apply false
  }
  ```

- app/build.gradle 파일

  ```kotlin
  plugins {
      alias(libs.plugins.ksp)
      alias(libs.plugins.hilt)
  }
  
  dependencies {
      // hilt
      implementation(libs.hilt.android)
      ksp(libs.hilt.compiler)
    
      // compose와 함께 사용 (hiltViewModel() 지원)
      implementation(libs.hilt.navigation.compose)
  }
  ```
