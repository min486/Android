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

> 코루틴 기반의 경량 이미지 로딩 라이브러리로, Kotlin과 Jetpack Compose 환경에 최적화되어 있다

<br>

### 주요 특징

- **CO**routine **I**mage **L**oader의 약자
- Kotlin Coroutines & Flow 기반으로 비동기 작업을 처리하여 성능을 최적화한다
- 경쟁 라이브러리(예: Glide)에 비해 일반적으로 메모리 사용량이 적다
- Composable의 생명주기를 자동으로 감지하여 이미지 로딩 요청을 관리한다 (일시중지 / 취소)
- 확장성
  - OkHttp3 기반의 네트워킹을 지원한다 (`coil-network-okhttp` 사용) 
  - SVG, GIF 같은 다양한 이미지 형식을 지원한다


<br>

### 사용 예시

1. 인터넷 권한 추가

   `AndroidManifest.xml` 파일에 인터넷 권한을 추가한다

   ```xml
   <manifest>
       <uses-permission android:name="android.permission.INTERNET" />
       ...
   </manifest>
   ```


2. AsyncImage 사용 예시

   `AsyncImage`는 이미지를 비동기적으로 로드하고 화면에 표시한다

   ```kotlin
   @Composable
   fun SampleImage() {
       AsyncImage()(
           modifier = Modifier.fillMaxWidth()
           model = "https://picsum.photos/200/300",  // 로드할 이미지의 URL
           contentDescription = "random image",      // 설명
           contentScale = ContentScale.Crop,         // 크기 조절
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

- `libs.versions.toml` 파일

  ```toml
  [versions]
  coil = "3.1.0"
  
  [libraries]
  # Compose 환경에서 AsyncImage 사용
  coil = { module = "io.coil-kt.coil3:coil-compose", version.ref = "coil" }
  # 네트워킹에 OkHttp3 사용
  coil-okhttp = { module = "io.coil-kt.coil3:coil-network-okhttp", version.ref = "coil" }
  ```

- 버전 참고

  https://github.com/coil-kt/coil
  
  https://coil-kt.github.io/coil/getting_started/
  
