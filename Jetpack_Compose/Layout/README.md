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

### Compose 레이아웃 기초

- Column

  - 자식 컴포저블을 세로 방향으로 순서대로 배치하는 레이아웃

  - 속성

    - verticalArrangement : 자식 간 세로 간격 조절
    - horizontalAlignment : 자식의 수평 정렬 설정

  - 예제

    ```kotlin
    Column(
        verticalArrangement = Arrangement.spacedBy(8.dp),
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        Text("Item 1")
        Text("Item 2")
    }
    ```

- Row

  - 자식 컴포저블을 가로 방향으로 순서대로 배치하는 레이아웃

  - 속성

    - horizontalArrangement : 자식 간 가로 간격 조절
    - verticalAlignment : 자식의 세로 정렬 설정

  - 예제

    ```kotlin
    Row(
        horizontalArrangement = Arrangement.SpaceBetween,
        verticalAlignment = Alignment.CenterVertically
    ) {
        Text("Item 1")
        Text("Item 2")
    }
    ```

- Box

  - 자식들을 겹치거나 특정 위치에 정렬할 때 사용

  - 기본적으로 첫 번째 자식이 가장 아래에 위치함

  - 속성

    - contentAlignment : Box 내부에서 자식의 정렬 위치 지정

  - 예제

    ```kotlin
    Box(
        contentAlignment = Alignment.Center
    ) {
        Image(painter, contentDescription = null)
        Text("Overlay Text")
    }
    ```

<br>

<img src="../README.assets/layout.png" alt="layout" align="center" width="60%" />
