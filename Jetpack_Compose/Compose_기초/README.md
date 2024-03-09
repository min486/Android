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

### Scaffold

Material Design에서 `Scaffold()`는 복잡한 UI를 위한 표준화된 플랫폼을 제공하는 기본 구조다

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
