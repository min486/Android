<div align="center">
  <p>
    <img src="../README.assets/kotlin-hero.png">
  </p>
  <br>
  <h2>Kotlin</h2>
  <p>코틀린 관련 내용 정리</p>
  <br>
  <br>
</div>

## 🔥 Sealed Class

### Sealed Class

> 특정 클래스들이 제한된 범위 내에서만 상속되도록 제어하는 추상 클래스

- 주로 ViewModel에서 UI 상태 또는 단일 이벤트를 표현할 때 사용하며,
- when 문과 함께 사용 시 컴파일 타임 타입 안정성을 제공한다

<br>

### 주요 특징

- 제한된 상속
  - sealed class의 서브 클래스는 같은 모듈 내에서만 정의 가능하다
- 컴파일 타임 타입 안정성
  - 모든 서브 타입이 미리 정의되어 있어, when 문에서 모든 상태를 처리하면 else를 생략할 수 있다
- UI 상태 및 이벤트 모델링에 최적화
  - 화면 상태, 네트워크 요청 결과, 단일 처리 이벤트를 표현할 때 유용하다
- 직접 인스턴스화 불가
  - sealed class는 추상적 개념이며, 반드시 서브 클래스를 통해 사용해야 한다

<br>

### 사용 예시

1. UI 상태 표현

   네트워크 요청 또는 비동기 작업 상태를 표현할 때 사용

   ```kotlin
   sealed class HomeUiState {
       // 데이터가 로딩 중인 상태
       data object Loading: HomeUiState()
       
       // 데이터 로딩에 성공한 상태. 성공한 데이터를 포함
       data class Success(val burgers: List<HomeBurger>) : HomeUiState()
       
       // 데이터 로딩에 실패한 상태. 에러 메시지를 포함
       data class Error(val message: String) : HomeUiState()
   }
   
   // Compose UI
   when (uiState) {
       is HomeUiState.Loading -> /* 로딩 표시 */
       is HomeUiState.Success -> /* 데이터 표시 */
       is HomeUiState.Error -> /* 에러 메시지 표시 */
   }
   ```

2. 단일 이벤트 표현 (Navigation, SnackBar 등)

   ```kotlin
   // 로그인 완료 후 홈 화면으로 이동해야 하는 일회성 이벤트
   sealed class LoginEvent {
       data object NavigateToHome : LoginEvent()
   }
   ```
