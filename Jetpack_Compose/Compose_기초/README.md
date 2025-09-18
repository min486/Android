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

> 안드로이드 앱을 위한 선언형 UI 툴킷

- XML 레이아웃 파일 없어 순수 kotlin 코드로 UI를 구성하는 현대적인 접근 방식을 제공한다
- 코드가 간결해지고 유지보수가 쉬워진다

<br>

### Jetpack Compose 장점

- 코드 감소 : 기존 뷰 시스템보다 훨씬 적은 코드로 UI 구현 가능
- 직관적 상태 관리 : UI가 상태(State)에 따라 자동으로 갱신
- 빠른 개발 속도 : 미리보기(`@Preview`) 기능과 재사용 가능한 composable을 통해 개발 주기 단축 가능
- 우수한 성능 : 필요한 부분만 다시 그리는 recomposition 덕분에 효율적

<br>

### 선언형 UI vs 명령형 UI

- 명령형 UI (Imperative UI)
  - HOW 중심 : UI를 어떻게 구성하고 변경할지 직접 명령하는 방식
  - `findViewById()`로 특정 뷰를 찾고, `setText()`, `setVisibility()` 같은 메서드를 호출해 상태를 변경
  - 상태 관리가 복잡하고, 상용구(boilerplate) 코드가 많다

- 선언형 UI (Declarative UI)

  - WHAT 중심 : 무엇을 보여줄지 선언하는 방식

  - Jetpack Compose는 선언형 UI 패러다임을 따른다

  - 단방향 데이터 흐름 (Unidirectional Data Flow, UDF)

    - 데이터 흐름 : 상위 → 하위 (데이터 내려줌)

      👉 UI는 데이터를 기반으로 그려지며, 데이터는 항상 상위 컴포저블에서 하위 컴포저블로 전달된다

      함수는 필요한 데이터를 계층 구조 아래로 내려보내며 UI를 구성한다

      <img src="../README.assets/compose.png" alt="navigation" align="center" width="50%" />

    - 이벤트 처리 : 하위 → 상위 (이벤트 올려줌)

      👉 사용자의 상호작용으로 이벤트가 발생하면, 해당 이벤트는 하위 컴포저블에서 상위 컴포저블로 전달된다

      상위 컴포저블은 이벤트를 처리하고 상태를 갱신한다

      <img src="../README.assets/compose2.png" alt="navigation" align="center" width="50%" />

✅ 이러한 단방향 데이터 흐름 구조 덕분에 앱의 상태 관리는 예측 가능하고 안정적이다

데이터가 변경되면 compose가 자동으로 관련 UI만 다시 그리기 때문에, 개발자는 UI를 직접 업데이트하는 복잡한 과정을 신경쓸 필요가 없다

<br>

### Composable (컴포저블)

> `@Composable` 어노테이션이 붙인 함수, UI를 선언하는 최소 단위

- 여러 컴포저블을 조합하여 하나의 화면을 구성할 수 있다
- 텍스트, 버튼, 이미지 등 다양한 UI 요소를 배치할 수 있다
- 예시 코드

  ```kotlin
  @Composable
  fun Greeting(name: String) {
      Text("Hello $name")
  }
  ```

<br>

### Composition (컴포지션)

> 컴포저블 함수 실행 결과로 UI 트리가 구성되는 과정
>
> Compose 런타임은 이 UI 트리를 관리하며, 상태 변화에 따라 필요한 부분만 업데이트한다

<br>

### Recomposition (리컴포지션)

> 데이터가 변경되었을 때, 변경 사항에 영향을 받는 일부 컴포저블만 다시 실행되는 과정

- 전체 화면이 아닌 필요한 부분만 갱신 → 성능 최적화
- 개발자가 UI 업데이트를 직접 명령할 필요 없음

<br>

### Modifier

> 컴포저블의 UI 속성(레이아웃, 크기, 색상 등)과 동작을 설정하는 불변(immutable) 객체

- 체이닝 (Chaining) : 여러 Modifier를 메서드 체이닝을 통해 연속적으로 적용 가능

- 순서의 중요성 : Modifier는 앞에서부터 순서대로 적용되므로, 순서에 따라 UI 결과가 달라질 수 있음

- 불변성 : Modifier는 새로운 인스턴스를 반환하므로 안전하게 조합 가능

- 예시 코드

  ```kotlin
  // 배경색 적용 후 패딩
  Modifier
      .background(Color.Yellow)  // 전체 영역에 노란색 배경 적용
      .padding(16.dp)            // 그 위에 16dp 패딩 적용
   
  // 패딩 적용 후 배경색
  Modifier
      .padding(16.dp)            // 16dp 패딩 영역 생성
      .background(Color.Yellow)  // 패딩을 제외한 콘텐츠 영역에 노란색 배경 적용
  ```


<br>

### 커스텀 Modifier 만들기

여러 Modifier를 묶어서 재사용하고 싶을 때, 확장 함수로 정의해 사용할 수 있다

- 예시 코드

  ```kotlin
  fun Modifier.myCardStyle(): Modifier = this
      .padding(8.dp)
      .background(Color.White, shape = RoundedCornerShape(12.dp))
      .border(1.dp, Color.Gray, shape = RoundedCornerShape(12.dp))
  
  Box(modifier = Modifier.myCardStyle())
  ```
