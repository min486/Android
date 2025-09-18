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




## 🔥 레이아웃 (Layout)

- 레이아웃은 화면에 UI 요소를 배치하고 정렬하는 과정이다

- Jetpack Compose 에서는 컴포저블(`Composable`)을 통해 UI를 선언하며,

  `Column`, `Row`, `Box`와 같은 레이아웃 컴포저블을 이용해 자식 컴포저블을 체계적으로 배치할 수 있다

- 뷰 시스템과 달리 중첩 레이아웃 비용이 낮고, 함수 단위로 UI를 재사용할 수 있다는 장점이 있다

## 1. 기본 레이아웃

<img src="../README.assets/layout.png" alt="layout" align="center" width="50%" />

### Column

> 자식 컴포저블을 세로 방향으로 순서대로 배치하는 레이아웃

- 주요 속성

  - verticalArrangement : 자식 간의 수직 간격 조절 및 정렬

  - horizontalAlignment : 자식 간의 수평 정렬 방식

- 예시 코드

  ```kotlin
  Column(
      verticalArrangement = Arrangement.spacedBy(8.dp),
      horizontalAlignment = Alignment.CenterHorizontally
  ) {
      Text("Item 1")
      Text("Item 2")
  }
  ```

<br>

### Row

> 자식 컴포저블을 가로 방향으로 순서대로 배치하는 레이아웃

- 주요 속성

  - horizontalArrangement : 자식 간의 수평 간격 조절 및 정렬

  - verticalAlignment : 자식 간의 수직 정렬 방식

- 예시 코드

  ```kotlin
  Row(
      horizontalArrangement = Arrangement.SpaceBetween,
      verticalAlignment = Alignment.CenterVertically
  ) {
      Text("Item 1")
      Text("Item 2")
  }
  ```

<br>

### Box

> 자식 컴포저블을 겹치거나 특정 위치에 정렬할 때 사용
>
> 첫 번째 자식이 가장 아래에 위치하고, 뒤에 오는 자식들이 그 위에 쌓인다

- 주요 속성

  - `contentAlignment` : Box 내부에서 자식들의 정렬 위치를 지정

- 예시 코드

  ```kotlin
  Box(
      contentAlignment = Alignment.Center
  ) {
      Image(painter, contentDescription = null)
      Text("Overlay Text")
  }
  ```

## 2. 심화 레이아웃

### Scaffold

> Material Design 표준 화면 구조를 제공하는 컴포저블
>
> 상단바, 하단바, 플로팅액션버튼 등 다양한 UI 요소를 체계적으로 배치하는 틀을 제공한다

- 주요 슬롯

  - `topBar` : 상단바
  - `bottomBar` : 하단바
  - `floatingActionButton` : 플로팅액션버튼

  - `content` : 메인 콘텐츠 영역

    → `innerPadding`을 적용해야 `topBar`/`bottomBar`에 가려지지 않음

    *`Scaffold`는 `innerPadding` 이라는 매개변수를 제공하여 `topBar`, `bottomBar`의 크기만큼 자동으로 패딩 정보를 전달한다

    콘텐츠에 이 `innerPadding`을 적용해야 레이아웃이 올바르게 표시된다

- 예시 코드

  ```kotlin
  @OptIn(ExperimentalMaterial3Api::class)
  @Composable
  fun MainScreenWithScaffold() {
      Scaffold(
          topBar = {
              TopAppBar(title = { Text("Scaffold 예제") })
          },
          bottomBar = {
              NavigationBar { /* Navigation items */ }
          },
          floatingActionButton = {
              FloatingActionButton(onClick = { /* ... */ }) { Text("FAB") }
          }
      ) { innerPadding ->
          // innerPadding을 content에 적용
          Column(
              modifier = Modifier
                  .fillMaxSize()
                  .padding(innerPadding),
              verticalArrangement = Arrangement.Center,
              horizontalAlignment = Alignment.CenterHorizontally
          ) {
              Text(text = "메인 콘텐츠 영역입니다.")
          }
      }
  }
  ```
