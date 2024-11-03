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




## 🔥 레이아웃

### Compose 레이아웃 기초

Column

항목들을 화면에 세로로 배치한다

Row

항목들을 화면에 가로로 배치한다

Box

여러 요소를 겹쳐서 놓을 수 있는 레이아웃

<br>

<img src="../README.assets/layout.png" alt="layout" align="center" width="60%" />

<br>

### Surface / Box

- Surface

  - Material Design 시스템에 기반한 Compose의 컨테이너

  - light/dark 테마에 따라 적용 가능

- Box

  - 단순한 컨테이너로, Compose의 레이아웃을 구성하는데 사용
  - Box는 자식 요소를 수직 또는 수평으로 정렬하고, 간격, 패딩 등의 스타일을 적용하는데 유용하다

<br>

### Modifier

Modifier를 통해 다음과 같은 종류의 작업을 실행할 수 있다

- 컴포저블의 크기, 레이아웃, 동작 및 모양 변경
- 접근성 라벨과 같은 정보 추가
- 사용자 입력 처리
- 요소를 클릭 가능, 스크롤 가능, 드래그 가능 또는 확대/축소 가능하게 만드는 높은 수준의 상호작용 추가
