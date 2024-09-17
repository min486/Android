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

### Column

수직 배치를 지원하는 레이아웃

<br>

### Row

수평 배치를 지원하는 레이아웃

<br>

### LazyColumn, LazyRow 장점

기존의 RecyclerView와 동일하게 리스트에 속한 모든 View를 한번에 그리지 않고

스크롤하면서 화면에 보여지게 될 때만 그리게 함으로써 리소스 사용을 최적화하기 위한 용도로 만들어졌다

<br>

### LazyColumn

화면에 보여지는 컴포저블만을 표시하는 scrollable한 Column이다

LazyColumn은 화면에 보여지는 컴포저블만을 표시하기 때문에 화면 로딩 시간을 최적화 시킬 수 있어,

많은 아이템이 표시되어야 할 경우 필수적으로 사용되어야 하는 레이아웃이다

<br>

### LazyColumn 사용

LazyColumn은 item을 기반으로 동작한다

item 하나가 자식 컴포저블에 대응되며 LazyColumnScope내에서 다음의 item들을 사용할 수 있다

- item

  LazyColumn 내부에 컴포저블을 직접 넣고 싶을 때 사용하는 메서드이다

- items

  items는 컴포저블을 반복해서 나타내고자 할 때 사용한다

- itemsIndexed

  index와 item을 사용할 수 있다

<br>

### LazyRow

화면에 보여지는 컴포저블만을 표시하는 scrollable한 Row이다





