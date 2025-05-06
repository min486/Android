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




## 🔥 Scaffold

### Scaffold

- Compose에서 기본적인 화면 구조를 잡기 위해 사용하는 레이아웃 컨테이너

- 앱 화면의 topBar, bottomBar, floatingActionButton 등을 일관성 있게 배치

- Compose에서 페이지 구조를 간단하고 명확하게 구성할 수 있음

- 예시

  ```kotlin
  Scaffold(
      topBar = {
          ...
      },
      bottomBar = {
          ...
      },
      floatingActionButton = {
          ...
      },
      floatingActionButtonPosition = FabPosition.End,
      content = { innerPadding ->
          Column(modifier = Modifier.padding(innerPadding)) {
              Text("Scaffold의 Content 영역입니다.")
          }
      }
  )
  
  ```

  👉 `innerPadding`을 `content`의 Modifier에 적용해야 Top/BottomBar와 겹치지 않음

<br>

- topBar
  - 상단 앱바로, 제목/탭/뒤로가기 버튼 등을 배치하는 데 사용
- bottomBar
  - 하단에 간단한 버튼이나 정보를 배치
- floatingActionButton
  - 액션 버튼, 기본 위치는 화면 우측 하단

