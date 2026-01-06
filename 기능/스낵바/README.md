<div align="center">
  <p>
    <img src="../README.assets/android.png">
  </p>
  <br>
  <h2>기능</h2>
  <p>기능 관련 내용 정리</p>
  <br>
  <br>
</div>


## 🔥 스낵바 (Snackbar)

### Snackbar

> 앱에서 수행한 작업에 대해 가벼운 피드백을 제공하는 UI 구성 요소

- 화면 하단에 잠시 나타났다가 자동으로 사라지며, 간단한 액션을 포함할 수 있다

- 사용자가 앱을 계속 사용할 수 있도록 방해하지 않는다
- 일정 시간이 지나면 자동으로 사라지는 일회성 메시지이다

<br>

### UX 가이드라인

- 위치 선정
  - 주요 액션 버튼(FAB, 하단 버튼 등)을 가리지 않도록 배치해야 한다
  - 한 번에 하나의 스낵바만 표시해야 하며, 여러 메시지는 큐(queue) 방식으로 순차적으로 보여준다
- 텍스트
  - 메시지는 한 줄 내외로 짧고 명확하게 작성한다
  - 단순한 결과 표현보다는 사용자의 행동과 결과를 함께 전달한다

<br>

### 스낵바 유지 시간

- `Short` (약 4초) : 짧은 문장으로 된 일반적인 피드백
- `Long` (약 10초) : 문장이 길어 읽는 시간이 필요하거나, 액션이 포함된 경우
- `Indefinite` (무제한) : 사용자가 직접 닫거나 특정 액션을 취하기 전까지 유지되는 경우

```kotlin
val snackbarHostState = remember { SnackbarHostState() }

snackbarHostState.showSnackbar(
    message = message.message,
    duration = SnackbarDuration.Short
)
```

<br>

### Jetpack Compose 구현

#### 주요 컴포넌트

- `SnackbarHostState` : 스낵바의 표시 상태를 관리하고 `showSnackbar` 함수를 제공한다
- `SnackbarHost` : 실제 스낵바가 화면 어디에 그려질지 결정하는 컨테이너
- `Scaffold` : 파라미터를 통해 스낵바의 위치와 생명주기를 쉽게 관리하게 해준다

<br>

#### 코드 예시

```kotlin
val snackbarHostState = remember { SnackbarHostState() }

Scaffold(
    snackbarHost = {
        SnackbarHost(
            hostState = snackbarHostState,
            modifier = Modifier.padding(bottom = 120.dp)  // 버튼 위로 띄우기
        )
    }
) { paddingValues ->
    Button(onClick = {
        scope.launch {
            snackbarHostState.showSnackbar("메시지가 전송되었습니다.")
        }
    }) {
        Text("전송")
    }
}
```
