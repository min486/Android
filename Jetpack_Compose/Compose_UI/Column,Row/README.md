<div align="center">
  <p>
    <img src="../../README.assets/jetpack-hero.png">
  </p>
  <br>
  <h2>Jetpack Compose</h2>
  <p>Jetpack Compose 관련 내용 정리</p>
  <br>
  <br>
</div>





## 🔥 Column, Row

### LazyColumn

화면에 보여지는 컴포저블만을 표시하는 scrollable한 Column이다

LazyColumn은 화면에 보여지는 컴포저블만을 표시하기 때문에 화면 로딩 시간을 최적화 시킬 수 있어,

많은 아이템이 표시되어야 할 경우 필수적으로 사용되어야 하는 레이아웃이다

<br>

### LazyColumn 사용

LazyColumn은 item을 기반으로 동작한다

item 하나가 자식 컴포저블에 대응되며 LazyColumnScope내에서 다음의 item들을 사용할 수 있다

- item
- items
- itemsIndexed



