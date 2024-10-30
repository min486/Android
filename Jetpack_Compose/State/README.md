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

`mutableStateOf`는 관찰 가능한 `MutableState<T>`를 생성해준다

이 `MutableState<T>` 값이 변경될 때마다 컴포저블 함수가 Recompose 된다

특정한 값 그 자체를 `State`로 쓰고 싶을때 사용할 수 있다

<br>

mutableStateOf는 변화 가능한 상태를 생성하고, 이 상태가 변경될 때 관련된 컴포저블 함수들을 재구성하도록 합니다

이는 Compose 런타임과 통합된 관찰 가능한 타입(MutableState)을 통해 이루어집니다

<br>

컴포저블에서 `MutableState` 객체를 선언하는 데는 세 가지 방법이 있다

- val mutableState = remember { mutableStateOf(default) }
- var value by remember { mutableStateOf(default) }
- val (value, setValue) = remember { mutableStateOf(default) }

<br>

### MutableState의 재구성 유도

mutableStateOf를 통해 생성된 MutableState 객체가 컴포저블 함수들을 재구성(recomposition)할 수 있는 주된 이유는

이 객체가 관찰 가능한 상태(observable state)를 제공하기 때문이다

<br>

MutableState는 상태의 변화를 자동으로 감지하고 이에 반응할 수 있는 구조로 설계되어 있다

이 객체의 value 속성에 변화가 발생하면,

Compose 런타임은 그 변화를 감지하고 value를 사용하는 모든 컴포저블 함수들을 자동으로 재구성하도록 스케줄링한다

<br>

### MutableStateFlow

StateFlow, MutableStateFlow는 위의 MutableStateOf와는 조금 다르게 Kotlin Coroutine의 Flow를 State로 사용하기 위한 API이다

<br>

StateFlow는 state-holder 로 observable flow 이다. 현재 상태 또는 새로운 상태 업데이트를 collector 에게 방출(emit)한다

value property 로 현재 상태를 read 할 수 있다

<br>

새로운 상태로 변경하기 위해서는 MutableStateFlow 의 value property 로 변경해야 한다

<br>

### Remember

Compose는 State값이 바뀌면 Recomposition(재구성)이 일어난다

이때 이전 State를 기억해야 하는 경우 remember을 이용해 값을 저장할 수 있다

<br>

### Remember의 역할

remember는 컴포저블이 초기 구성(Initial Composition)에서 실행될 때 계산된 값을 기억하고, 

재구성 시에 이 값을 유지하여 반환합니다. 이는 Compose의 효율성을 높여주는 메커니즘으로, 불필요한 계산을 방지하고 성능을 최적화합니다

<br>

따라서, remember를 사용할 때는 그것이 재구성 시에 다시 호출되지 않고, 초기에 저장된 값을 계속해서 반환한다

이는 상태 관리를 효과적으로 돕고, Compose UI의 성능을 유지하는데 도움을 준다

<br>

### Remember와 RememberSaveable 차이

- `remember`는 객체를 컴포지션에 저장하고 `remember`를 호출한 컴포저블이 컴포지션에서 삭제되면 그 객체를 삭제합니다

- `rememberSaveable`은 상태를 `Bundle`에 저장하여 구성 변경 간에 상태를 유지합니다
