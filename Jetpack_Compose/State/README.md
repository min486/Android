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




## 🔥 State

### State 정의

Jetpack Compose에서는 UI의 상태가 변했음을 인식하기 위해 `State`를 사용한다

Composable UI 가 특정 변수를 State로 인식하기 위해서는 `mutableStateOf` 같은 State Object로 감싸주면된다

State Object로 활용하는 방법에는 `mutableStateOf` 말고도 여러가지 방법이 있다

<br>

### MutableStateOf

`mutableStateOf`는 Observable한 `MutableState<T>`를 생성해준다

이 `MutableState<T>` 값이 변경될 때마다 컴포저블 함수가 Recompose 된다

특정한 값 그 자체를 `State`로 쓰고 싶을때 사용할 수 있다

<br>

컴포저블에서 `MutableState` 객체를 선언하는 데는 세 가지 방법이 있다

- val mutableState = remember { mutableStateOf(default) }
- var value by remember { mutableStateOf(default) }
- val (value, setValue) = remember { mutableStateOf(default) }

<br>

### MutableStateFlow

StateFlow, MutableStateFlow는 위의 MutableStateOf와는 조금 다르게 Kotlin Coroutine의 Flow를 State로 사용하기 위한 API이다

<br>

StateFlow는 state-holder 로 observable flow 이다. 현재 상태 또는 새로운 상태 업데이트를 collector 에게 방출(emit)한다

value property 로 현재 상태를 read 할 수 있다

<br>

새로운 상태로 변경하기 위해서는 MutableStateFlow 의 value property 로 변경해야 한다

<br>

### 핵심 용어

- Composition

  : Jetpack Compose가 컴포저블을 실행할 때 빌드한 UI에 관한 설명이다

- Recomposition

  : 데이터가 변경될 때 컴포지션을 업데이트하기 위해 컴포저블을 다시 실행하는 것을 말한다
