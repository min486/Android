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



## 🔥 Coil

### Coil

> Coil은 Coroutine 기반의 경량 이미지 로딩 라이브러리로, Kotlin과 Jetpack Compose에 최적화되어 있다

<br>

### Coil 특징

- **CO**routine **I**mage **L**oader의 약자다
- Kotlin Coroutines & Flow 기반
- Jetpack Compose, View 모두 지원
- OkHttp3, SVG, GIF 등 확장 가능
- 빠르고 가벼움 (Glide보다 메모리 사용량 적음)
- Lifecycle 자동 인식

<br>

### 기본 사용 예시

인터넷 권한 추가

```kotlin
// AndroidManifest.xml
<uses-permission android:name="android.permission.INTERNET" />
```

AsyncImage를 사용하여 이미지를 로드하고 표시

```kotlin
@Composable
fun SampleImage() {
    AsyncImage()(
        model = "https://picsum.photos/200/300",
        contentDescription = "random image",
        contentScale = ContentScale.Crop,
        modifier = Modifier.fillMaxWidth()
    )
}
```

<br>

*예시 이미지 URL (무료 샘플)

- 랜덤 이미지 : https://picsum.photos/200/300
- 정사각형 랜덤 이미지 : https://picsum.photos/200
- 고정 ID 이미지 : https://picsum.photos/id/5/200/300 (노트북 이미지)

<br>

### 의존성 추가

- libs.versions.toml 파일

  ```toml
  [versions]
  coil = "3.1.0"
  
  [libraries]
  coil = { module = "io.coil-kt.coil3:coil-compose", version.ref = "coil" }
  coil-okhttp = { module = "io.coil-kt.coil3:coil-network-okhttp", version.ref = "coil" }
  ```

- app/build.gradle 파일

  ```kotlin
  dependencies {
      implementation(libs.coil)
      implementation(libs.coil.okhttp)
  }
  ```
