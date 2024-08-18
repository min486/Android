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





## 🔥 Remember 관련

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

### MutableState

mutableStateOf는 변화 가능한 상태를 생성하고, 이 상태가 변경될 때 관련된 컴포저블 함수들을 재구성하도록 합니다

이는 Compose 런타임과 통합된 관찰 가능한 타입(MutableState)을 통해 이루어집니다

<br>

### MutableState의 재구성 유도

mutableStateOf를 통해 생성된 MutableState 객체가 컴포저블 함수들을 재구성(recomposition)할 수 있는 주된 이유는

이 객체가 관찰 가능한 상태(observable state)를 제공하기 때문이다

<br>

MutableState는 상태의 변화를 자동으로 감지하고 이에 반응할 수 있는 구조로 설계되어 있다

이 객체의 value 속성에 변화가 발생하면,

Compose 런타임은 그 변화를 감지하고 value를 사용하는 모든 컴포저블 함수들을 자동으로 재구성하도록 스케줄링한다

<br>

### Remember와 RememberSaveable 차이

- `remember`는 객체를 컴포지션에 저장하고 `remember`를 호출한 컴포저블이 컴포지션에서 삭제되면 그 객체를 삭제합니다

- `rememberSaveable`은 상태를 `Bundle`에 저장하여 구성 변경 간에 상태를 유지합니다
