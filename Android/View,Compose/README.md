<div align="center">
  <p>
    <img src="../README.assets/android.png">
  </p>
  <br>
  <h2>Android</h2>
  <p>안드로이드 관련 내용 정리</p>
  <br>
  <br>
</div>


## 🔥 View vs Compose

### 명령형 UI vs 선언형 UI

- 명령형 UI (Imperative UI)

  - 개발자가 UI 변경 과정을 직접 명령하는 방식

  - 뷰 객체를 찾아 속성을 변경해야 해서 상태 관리가 복잡해짐

    ```kotlin
    textView.text = "변경된 텍스트"
    ```

- 선언형 UI (Declarative UI)

  - UI를 상태(State)에 따라 선언하는 방식

  - 상태만 바꾸면 compose가 알아서 UI 갱신

    ```kotlin
    var text by remember { mutableStateOf("초기값") }
    Text(text = text)
    ```

<br>

### View System (명령형 UI, XML 기반)

기존 안드로이드 UI 방식은 명령형(Imperative) 방식을 따른다

즉 UI 상태가 바뀔 때마다 개발자가 직접 뷰를 찾아 속성을 수정해야 한다

- 동작 방식

  - 레이아웃 파일(`.xml`) 작성 → `findViewById` 또는 `View Binding`으로 뷰 객체 참조

  - Kotlin 코드에서 직접 속성 변경

- 단점

  - 상태가 복잡해질수록 코드량 증가

  - UI와 로직이 뒤섞여 유지보수가 어려움

<br>

### Jetpack Compose (선언형 UI)

Jetpack Compose는 선언형(Declarative) 방식의 최신 UI 툴킷이다

XML 없이 Kotlin 코드만으로 UI 작성이 가능하다

- 동작 방식

  - 데이터가 바뀌면 해당 데이터를 사용하는 `@Composable` 함수가 자동으로 재구성(Recomposition) 된다

    ```kotlin
    Button(onClick = { /* 클릭 처리 */ }) {
        Text("클릭하세요")
    }
    ```

- 핵심 개념

  - Composable 함수 : `@Composable`로 정의된, 재사용 가능한 UI 블록
  - State : `mutableStateOf`로 생성된 관찰 가능한 데이터 (Compose 상태 관리 도구)
  - Recomposition : State 변경 시 UI 자동 갱신
  - 단방향 데이터 흐름 (Unidirectional Data Flow, UDF)
    - 상태를 상위에서 관리하고 하위로 전달하는 상태 호이스팅(State Hoisting) 패턴 사용
    - 데이터 관리 단순화 + 버그 발생 가능성 감소

<br>

### View System vs Compose 비교

| 구분          | View System (XML)           | Jetpack Compose                       |
| ------------- | --------------------------- | ------------------------------------- |
| UI 패러다임   | 명령형 (Imperative)         | 선언형 (Declarative)                  |
| UI 업데이트   | 뷰 속성을 직접 변경         | 상태 변경 시 자동 재구성              |
| 상태 관리     | 복잡, 오류 발생 쉬움        | State + Hoisting으로 단순화           |
| 데이터 바인딩 | View Binding / Data Binding | @Composable 함수 내 직접 상태 사용    |
| 코드 구조     | XML + Kotlin 분리           | Kotlin 단일 언어로 통합               |
| 장점          | 레거시 코드와 호환          | 개발 속도 빠름, 직관적, 유지보수 용이 |
| 단점          | 보일러플레이트 많음         | 초기 학습 비용                        |
