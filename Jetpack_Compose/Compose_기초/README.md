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




## 🔥 Jetpack Compose 기초

### Jetpack Compose

안드로이드 앱을 위한 선언형 UI 툴킷으로, UI를 쉽게 작성하고 유지할 수 있도록 돕는다

<br>

### Jetpack Compose 장점

- 코드 감소
- 높은 직관성
- 빠른 개발 속도
- 우수한 성능

<br>

### 선언형 UI

- 선언형 UI는 어떻게(HOW) UI를 구성할지 명령하는 방식이 아니라, 무엇(WHAT)을 보여줄지를 선언하는 방식이다

- UI의 변경은 전체 UI를 다시 그리는 방식으로 처리되며, UI 요소에 직접 변수나 이벤트를 연결할 필요가 없다

<br>

- 함수는 데이터를 기반으로 다른 컴포저블을 호출해 UI를 구성하고,

  필요한 데이터를 계층 구조 아래로 전달한다

<img src="../README.assets/compose.png" alt="navigation" align="center" width="60%" />

<br>

- 사용자의 상호작용으로 이벤트가 발생하면 앱 로직이 이를 처리하고,

  필요시 컴포저블이 새로운 매개변수로 자동 호출되어 UI가 다시 구성된다 

<img src="../README.assets/compose2.png" alt="navigation" align="center" width="60%" />

<br>

### Composable (컴포저블)

- `@Composable` 어노테이션을 붙인 함수로, UI를 선언적으로 작성한다
- 여러 개의 컴포저블 함수를 조합해 하나의 화면을 만들 수 있다
- 텍스트, 버튼, 이미지 등 다양한 UI 요소를 자유롭게 배치할 수 있다

```kotlin
@Composable
fun Greeting(name: String) {
    Text("Hello $name")
}
```

<br>

### Composition (컴포지션)

- 컴포저블 함수가 실행되어 UI 트리를 구성하는 과정이다
- 컴포저블 함수는 실행시 실제 UI 요소들을 생성하고 구성한다

<br>

### Recomposition (리컴포지션)

- 데이터가 변경되면 해당 변경을 감지하여 관련된 컴포저블 함수만 다시 실행되는 것이다
- 이 과정을 통해 UI는 항상 최신 상태를 유지한다

<br>

### Modifier

- 컴포저블의 UI 속성을 설정하거나 동작을 부여하기 위해 사용하는 객체

- 크기, 패딩, 정렬, 배경색 등 레이아웃 관련 속성뿐만 아니라, 클릭, 스크롤, 드래그 같은 동작까지 정의 가능

- 불변(immutable)한 객체로, 메서드 체이닝을 통해 연속적으로 설정함

- 예제

  ```kotlin
  Box(
      modifier = Modifier
          .fillMaxWidth()
          .height(100.dp)
          .padding(16.dp)
          .background(Color.Blue)
  )
  ```

  👉 위 코드에서 `Modifier`는 순차적으로 크기를 지정하고, 패딩을 주고, 배경색을 적용하는 역할을 한다

<br>

### Modifier 체이닝 순서

- Modifier는 앞에서부터 순서대로 적용됨
- 순서가 바뀌면 화면 출력 결과가 달라질 수 있음

- 예제

  ```kotlin
  // 배경 → 패딩
  Modifier
      .background(Color.Yellow)
      .padding(16.dp)
  
  // 패딩 → 배경
  Modifier
      .padding(16.dp)
      .background(Color.Yellow)
  ```

  👉 결과 : 첫 번째는 전체 배경이 노란색, 두 번째는 패딩 제외 영역만 노란색

<br>

### 커스텀 Modifier 만들기

- 여러 Modifier를 묶어서 재사용하고 싶을 때 사용

- 확장 함수 형태로 정의

- 예제

  ```kotlin
  fun Modifier.myCardStyle(): Modifier = this
      .padding(8.dp)
      .background(Color.White, shape = RoundedCornerShape(12.dp))
      .border(1.dp, Color.Gray, shape = RoundedCornerShape(12.dp))
  
  Box(modifier = Modifier.myCardStyle())
  ```

  👉 `Modifier.myCardStyle()`을 사용하면 여러 스타일을 한번에 적용 가능

