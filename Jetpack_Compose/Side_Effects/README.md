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




## 🔥 Side Effects (부수효과)

### 부수효과

> Composable 함수의 범위 밖에서 발생하는 앱 상태에 관한 변경사항

<br>

### side-effect APIs

- LaunchedEffect

- DisposableEffect

- SideEffect

<br>

### DisposableEffect

컴포저블이 컴포지션을 종료한 후 정리 해야 하는 부수 효과가 있는 경우 사용한다

DisposableEffect에 `Key` 값을 설정하면, 해당 키 값이 변경되면 내부의 `onDispose` 절이 실행되고, 해당 Effect를 다시 호출하여 재설정한다

👉 Composable의 Lifecycle에 맞춰 정리되어야 하는 리스너나 작업이 있는 경우에 리스너나 작업을 제거하기 위해 사용되는 Effect
