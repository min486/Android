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




## 🔥 Compose 기초

- Compose
- Composable
- Composition
- Recomposition
- State

<br>

### Jetpack Compose (Compose)

> 안드로이드 앱의 UI를 쉽게 디자인하고 빌드하기 위한 라이브러리

<br>

### Composable

Composable은 컴포즈로 화면을 만들기 위한 함수로, `@Composable`로 지정할 수 있다

하나의 화면을 만들기 위해 여러개의 컴포저블 함수를 만들어 사용할 수 있고, 

컴포저블 함수 내에서 필요한 구성 요소들(텍스트, 버튼, 이미지 등)을 원하는 형태로 배치 및 구성할 수 있다

```kotlin
@Composable
fun Greeting(name: String) {
    Text("Hello $name")
}
```

<br>

### Composition (컴포지션)

컴포저블 함수를 실행하면 생성되는 것으로, UI를 그리는 역할을 한다

<br>

### Scaffold

> Material Component들을 편하게 사용할 수 있도록 하기 위해 미리 디자인된 레이아웃이다

- Snackbar도 Material Component이므로

  Compose에서 Snackbar를 기존 Snackbar의 동작대로 이용하기 위해서는 Scaffold State로 감싸야 한다

  만약 Scaffold로 감싸지 않으면 보통의 Composable과 똑같이 동작한다

- Scaffold는 앱을 위한 최상위 수준의 컴포저블이다
- topBar, bottomBar, floatingActionButton 같은 일반적인 최상위 머테리얼 컴포넌트에 대한 Slot을 제공한다
- Scaffold를 사용하면 이러한 컴포넌트들을 배치하고 올바르게 동작하도록 한다

- UI의 여러 부분을 결합하여 앱에 일관된 디자인과 분위기를 준다

```kotlin
@Composable
fun ScaffoldExample() {
    var presses by remember { mutableIntStateOf(0) }

    Scaffold(
        topBar = {
          ...
        },
        bottomBar = {
          ...
        },
        floatingActionButton = {
          ...
        }
    ) { 
      ...
    }
}
```

Snackbar composable이 존재한다

하지만 이를 띄우고, 애니메이션 효과를 주고 하는 등의 작업이 개발자의 몫이 된다

또한, snackbar는 suspend 하게 동작한다

그렇기 때문에 이렇게 하지 않고 `snackbarHostState`를 사용하여 구현한다

```kotlin
@Composable
fun SnackbarEx() {
    val snackbarHostState = remember { SnackbarHostState() }
  
    Scaffold(
        snackbarHost = { SnackbarHost(snackbarHostState) },
        ...
    ) {
        ...
    }
}
```

<br>
