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




## 🔥 Jetpack Compose 상태 관리

### 기본 개념

- 상태 (State)
  - UI에서 데이터의 현재 값을 의미함
  - 선언형 UI에서는 상태가 UI를 결정하고, 상태가 바뀌면 UI가 자동으로 다시 그려짐

- mutableStateOf
  - Compose의 상태를 선언할 때 사용하는 기본 도구
  - Compose에서 감지 가능한 상태를 만듬

<br>

### remember와 rememberSaveable 차이

- remember

  - 컴포저블 함수에서 UI 상태를 저장하고 재호출 시에도 해당 값을 유지할 때 사용

  - 값 유지 범위 ➡️ 컴포저블이 메모리에 살아 있는 동안

    👉 컴포지션이 끝났다고 상태가 사라지지 않고, 컴포지션이 끝난 뒤에도 컴포저블이 메모리에 살아있으면 유지됨

  - 사라지는 시점 ➡️ 해당 컴포저블이 컴포지션 트리에서 제거될 때
  - 예시

  ```kotlin
  @Composable
  fun Counter() {
      var count by remember { mutableStateOf(0) }
  
      Button(onClick = { count++ }) {
          Text("Count: $count")
      }
  }
  ```


​	👉 같은 화면에 있으면 count 값은 계속 유지됨

​	👉 다른 화면으로 이동하면 Counter() 자체가 컴포지션 트리에서 사라지므로, count도 사라짐

- rememberSaveable

  - remember와 동일하게 상태를 기억하지만,

    화면 회전이나 프로세스 재시작 시에도 값을 복원할 수 있도록 Bundle에 저장됨

  - 값 유지 범위 ➡️ 컴포저블이 살아 있는 동안 + 화면 회전, 백그라운드 복귀 등에서도 유지됨

  - 사라지는 시점 ➡️ 해당 컴포저블이 컴포지션 트리에서 제거될 때

  - 예시

  ```kotlin
  @Composable
  fun NameInput() {
      var name by rememberSaveable { mutableStateOf("") }
  
      TextField(value = name, onValueChange = { name = it })
  }
  ```

  👉 회전하거나 백그라운드 갔다 와도 name 값 유지됨

  👉 다른 화면으로 이동하면 마찬가지로 값은 사라짐

- remember와 rememberSaveable 비교

  | 항목                  | remember                    | rememberSaveable            |
  | --------------------- | --------------------------- | --------------------------- |
  | 같은 화면 유지        | 유지됨                      | 유지됨                      |
  | 화면 회전             | 초기화됨                    | 유지됨                      |
  | 앱 백그라운드 후 복귀 | 초기화됨                    | 유지됨                      |
  | 다른 화면 이동        | 초기화됨                    | 초기화됨                    |
  | 다시 돌아왔을 때      | 새 컴포지션이라 값 초기화됨 | 새 컴포지션이라 값 초기화됨 |

<br>

### Compose에서 LiveData 잘 안 쓰이는 이유

- Compose는 자체적으로 생명주기를 인식하고, 상태 감지 기능을 갖고 있음

  👉 mutableStateOf, remember, StateFlow 등을 사용해 UI 상태를 효율적으로 관리할 수 있음

- 반면 LiveData는 생명주기 인식이 필요한 구조로, 기존 View 시스템(XML + ViewBinding)에서

  Activity/Fragment의 생명주기와 안전하게 UI를 연결하기 위한 방법으로 사용되었음

- Compose에서는 컴포지션 자체가 상태와 생명주기에 맞춰 반응적으로 동작하므로,

  LiveData 없이도 UI와 상태의 연결이 자연스럽게 처리됨

  👉 LiveData는 Compose에서 비추천, StateFlow 사용 권장
