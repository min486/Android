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


## 🔥 Activity 생명주기

### Activity Lifecycle

- 안드로이드 앱의 안정성과 성능을 위해 Activity의 생명주기(Lifecycle)를 이해하는 것이 중요하다
- 각 콜백 메서드는 특정 시점에 호출되며, 그 목적과 올바른 활용법이 다르다
- Jetpack Compose를 사용하더라도, Activity는 여전히 앱의 진입점으로서 생명주기를 관리한다

<br>

### 생명주기 콜백

- onCreate()
  - activity가 처음 생성될 때 호출
  - 한 번만 호출되는 초기 설정 로직 수행
  - compose 환경에서는 `setContent { ... }` 를 통해 UI 정의
  - Hilt, Navigation, ViewModel 초기화 같은 앱 전역 설정을 주로 배치


- onStart()

  - activity가 사용자에게 보일 준비가 된 상태
  - 아직 상호작용은 불가
  - compose에서는 특별히 구현할 일이 적고, 상태 관리용으로 사용 가능

- onResume()

  - activity가 포그라운드에서 사용자와 상호작용 가능한 상태
  - 카메라, 센서, 애니메이션 시작 등의 로직을 재개하는 시점
  - compose에서는 `LaunchedEffect`, `DisposableEffect` 등을 활용해 UI와 동기화 가능

- onPause()

  - 사용자가 activity를 떠나는 첫번째 신호
  - activity가 포그라운드에 있지 않지만, 다시 재시작할 수 있는 상태

  - 오래 걸리는 작업(DB, 네트워크, 파일 저장 등)을 넣으면 안됨

    👉 onPause는 매우 짧게 실행되며, 도중에 Activity가 종료될 수 있기 때문

  - 대신 애니메이션 일시 정지, 카메라 프리뷰 정지 같은 가벼운 작업만 처리

- onStop()

  - activity가 화면에 완전히 보이지 않는 상태
  - compose에서는 UI가 더이상 표시되지 않으므로, 무거운 리소스 해제 가능
  - OS 리소스 관리로 인해 프로세스가 종료될 수 있음 → 다시 실행 시 onCreate 호출

- onDestroy()

  - activity가 완전히 종료되기 직전에 호출
  - 더이상 UI에 접근할 수 없고, activity가 메모리에서 해제될 준비가 된 상태

<img src="../README.assets/activity_lifecycle.png" alt="activity_lifecycle.png" align="center" width="50%" />

<br>

### 상황별 호출 흐름

- Activity 실행
  - `onCreate → onStart → onResume`

- Activity 종료 (앱 종료 포함)
  - `onPause → onStop → onDestroy`

- 홈 버튼 눌러 백그라운드 이동

  - `onPause → onStop `

    (activity는 메모리에 유지됨)

- onStop 후 다시 실행 (복귀)
  - `onRestart → onStart → onResume`

- 다른 Activity 실행
  - 기존 Activity : `onPause → onStop`
  - 새 Activity : `onCreate → onStart → onResume`
