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


## 🔥 ViewModel

### ViewModel

- Android Jetpack의 아키텍처 컴포넌트 중 하나
- UI 관련 데이터를 저장하고 관리하는 역할
- 화면 회전과 같은 생명주기 변화에도 데이터를 안전하게 유지할 수 있음
- MVVM 패턴에서 View와 Model 사이의 중간 계층 역할

<br>

### ViewModel의 필요성

- Activity나 Fragment가 모든 로직을 처리하면 클래스가 비대해지고, 유지보수가 어려움

- ViewModel을 통해 UI 컨트롤러의 책임을 분리하고 데이터 로직을 독립시킴
- 재사용성, 테스트 용이성, UI 생명주기 문제 해결 가능

<br>

### ViewModel()

- `ViewModel()` 은 ViewModel 클래스를 상속해서 생명주기를 인식하고 상태를 안전하게 관리할 수 있게 만드는 기본 클래스

- 뷰모델로서 동작하게 만들기 위한 필수적인 상속이고,

  UI 데이터를 화면회전 등 생명주기 변화에도 안전하게 보관하는 용도로 설계되었다

```kotlin
class MyViewModel : ViewModel() {
    ...
}
```

<br>

### by viewModels()

- `by viewModels()` 함수를 통해 간단하게 viewmodel 인스턴스를 생성할 수 있다
- 보통 Compose를 사용하지 않거나, Hilt 없이 MVVM 구조만 적용한 경우 사용

```kotlin
private val viewModel: MainViewModel by viewModels()
```
