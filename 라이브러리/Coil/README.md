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

> 안드로이드 앱에서 image를 불러오기 위해 사용하는 이미지 로딩 라이브러리

- **CO**routine **I**mage **L**oader의 약자다

- Kotlin Coroutine으로 만들어진 이미지 로딩 라이브러리

- 코루틴 자체가 내장되어 있기 때문에 코루틴을 별도로 생성하지 않아도 된다

<br>

### Coil 사용

module 수준의 build.gradle에 의존성 추가

```kotlin
implementation("io.coil-kt:coil-compose:2.1.0")  // Jetpack Compose
```

인터넷 권한 추가

```kotlin
// AndroidManifest.xml
<uses-permission android:name="android.permission.INTERNET" />
```

AsyncImage 컴포저블을 사용하여 이미지를 로드하고 표시

```kotlin
AsyncImage(
    model = "https://example.com/image.jpg",
    contentDescription = null,
)
```

<br>

### AsyncImagePainter

내부적으로 AsyncImage와 SubcomposeAsyncImage는 model을 로드하는데 AsyncImagePainter를 사용한다

Painter가 필요하고 AsyncImage를 사용할 수 없으면 rememberAsyncImagePainter를 사용해서 이미지를 로드할 수 있다

```kotlin
Image(
  painter = rememberAsyncImagePainter(
    ImageRequest.Builder(LocalContext.current)
    	.data(data = accountInfo?.profileImageUrl)  // 로드할 이미지의 URL을 지정
      .apply(block = fun ImageRequest.Builder.() {
      	crossfade(true)  // 로딩시간이 소요되면 저화질의 이미지를 먼저 로딩해서 보여준다
      }).build()
  ),
  ...
)
```
