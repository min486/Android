<div align="center">
  <p>
    <img src="../README.assets/studio.png">
  </p>
  <br>
  <h2>Android Studio</h2>
  <p>안드로이드 스튜디오 관련 내용 정리</p>
  <br>
  <br>
</div>


## 🔥 build.gradle 내용

app 수준의 build.gradle 파일에 있는 내용 설명

<br>

### 버전 설정

- versionCode

  - 양의 정수이며, 내부 버전 관리용으로 사용됨
  - 앱 업데이트시 기존 버전보다 높은 값이어야함
  - 구글 플레이스토어애서 버전 비교 및 업데이트 판단에 사용됨

  - 사용자에게 직접 보이진 않음


- versionName

  - 사용자에게 표시되는 버전 문자열

  - 보통 < major >.< minor >.< point > 로 표기 (예 : "1.0.1")


<br>

### SDK 설정

```kotlin
android {
    compileSdk = 35
    minSdk = 30
    targetSdk = 35
}
```

- compileSdk
  - 앱을 컴파일할 때 사용하는 Android SDK 버전
  - 최신 Compose 기능이나 Jetpack 라이브러리를 사용하려면 최신 compileSdk가 필요
  - 예 : `compileSdk = 35`는 Android 15 (API 35) 기준으로 컴파일하겠다는 의미

- minSdk
  - 앱이 지원하는 최소 Android 버전 (API 레벨)
  - 이보다 낮은 버전의 기기에는 앱이 설치되지 않음
  - 예 : `minSdk = 30`는 Android 11 이상에서만 앱이 설치 가능
- targetSdk
  - 앱이 테스트 및 최적화된 Android 버전
  - 최신 버전의 Android에서 동작시, 해당 버전에 맞는 보안 정책과 동작 변경사항을 따름
  - 예 : `targetSdk = 35`는 Android 15에 맞춰 앱이 잘 동작하도록 설정했다는 의미

<br>

### kotlinOptions & compileOptions

kotlinOptions (Kotlin용 설정)

```kotlin
kotlinOptions {
    jvmTarget = "11"
}
```

- jvmTarget : Kotlin이 생성할 JVM 바이트코드 버전을 지정

- "11"이면, Kotlin 코드가 Java 11에서 실행 가능한 JVM 바이트코드로 컴파일됨

- Compose 기반 앱에서는 보통 `jvmTarget = "11"`이 기본값이며, 안정적인 선택

<br>

compileOptions 남아있는 이유

```kotlin
compileOptions {
    sourceCompatibility = JavaVersion.VERSION_11
    targetCompatibility = JavaVersion.VERSION_11
}
```

- Kotlin만 사용하는 프로젝트라도, Gradle 빌드 시스템은 Java 설정도 필요로함

- 이는 Kotlin ➡️ JVM 바이트코드 ➡️ DEX 로 변환되는 과정에서, Java 컴파일러 설정이 내부적으로 활용되기 때문

- 실제 Java 코드를 사용하지 않더라도, 빌드 도구를 위한 필수 설정

<br>

해당 설정이 중요한 이유

- kotlinOptions.jvmTarget과 compileOptions의 버전을 일치시키는 것이 안정적

  (`jvmTarget = "11"` ↔ `JavaVersion.VERSION_11`)

- Kotlin이 생성한 바이트코드는 결국 JVM에서 실행되므로, 대상 JVM 버전 설정은 중요

- 일부 라이브러리는 최소 JVM 타겟 버전을 요구하므로 주의
