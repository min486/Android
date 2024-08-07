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

Composable에는 부수 효과가 없는 것이 좋지만, Composable을 사용하다보면 부수 효과가 발생하는 경우가 생긴다

스낵바를 표시하거나 특정 상태 조건에 따라 다른 화면으로 이동하는 등 일회성 이벤트를 발생시키는 경우엔 필요하다

특정 Composable 내부에서 일어나는 것이 아닌 외부에 영향을 준다

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

<br>

### LaunchedEffect

> Composable에서 컴포지션이 일어날 때 suspend fun을 실행해주는 Composable

LaunchedEffect 함수의 인자인 `key` 값이 변경되면, 현재 실행중인 코루틴은 취소되고 재시작된다

해당 Effect가 컴포지션을 벗어나면 코루틴은 자동 취소된다

키 값의 변화에 따라 중단 함수를 호출하거나,

컴포저블 내부에서 외부의 값을 구독하면서 중단 함수를 호출해야 하는 경우에는 `LaunchedEffect`를 사용한다

👉 별도의 코루틴 스코프에서 부수효과를 실행하며 UI 쓰레드를 차단하지 않고 시간이 오래 걸리는 작업을 실행하는데 유용하다

첫 번째 컴포지션이나 키가 변경 시 트리거 됩니다

<br>

### LaunchedEffect 필요성

리컴포지션은 Composable의 State가 바뀔 때마다 일어나므로,

만약 매번 리컴포지션이 일어날 때마다 이전 LaunchedEffect가 취소되고 다시 수행된다면 매우 비효율적일 것이다

이를 해결하기 위해 LaunchedEffect는 key라 불리는 기준값을 두어 key가 바뀔 때만 LaunchedEffect의 suspend fun을 취소하고 재실행한다

