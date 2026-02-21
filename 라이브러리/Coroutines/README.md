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



## 🔥 Coroutines

### 코루틴

> 비동기 작업을 동기 코드처럼 순차적으로 작성할 수 있게 해주는 경량 실행 단위

<br>

### 의존성 추가

- `libs.versions.toml` 파일

  ```toml
  [versions]
  coroutines = "1.10.2"
  
  [libraries]
  coroutines-core = { 
      module = "org.jetbrains.kotlinx:kotlinx-coroutines-core",
      version.ref = "coroutines"
  }
  
  coroutines-android = { 
      module = "org.jetbrains.kotlinx:kotlinx-coroutines-android", 
      version.ref = "coroutines" 
  }
  ```
  
- 버전 참고

  https://mvnrepository.com/artifact/org.jetbrains.kotlinx/kotlinx-coroutines-core
  
  https://mvnrepository.com/artifact/org.jetbrains.kotlinx/kotlinx-coroutines-android
