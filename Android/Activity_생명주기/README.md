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

앱의 완성도와 안정성을 높이기 위해 activity의 생명주기를 알아야 한다

<br>

### 생명주기 콜백

activity가 시작되면 그림과 같은 순서로 콜백이 호출된다

- onCreate()

  - 필수적으로 구현해야함
  - activity의 생명주기 중 한 번만 발생해야하는 로직을 실행
  - 멈춰있는 상태 없이 다음 콜백(onStart)으로 넘어감

- onStart()

  - activity가 사용자에게 표시되기 위해 준비하고 있는 상태
  - 멈춰있는 상태 없이 다음 콜백(onResume)으로 넘어감

- onResume()

  - activity가 포그라운드에 표시되어, 사용자와 상호작용 할 수 있는 상태
  - 앱에서 포커스가 떠날때까지 onResume 상태에 머무름

- onPause()

  - 사용자가 activity를 떠나는 첫 번째 신호

  - activity가 포그라운드에 있지 않지만, 다시 재시작 할 수 있는 상태

  - 멈춰있는 상태 없이 다음 콜백(onResume)으로 넘어감

  - 이 상태에서, 실행중이지 않을 때 필요하지 않은 리소스를 해지할 수 있음

  - 이 상태에서, 데이터를 저장하거나 네트워크 호출, DB의 IO 작업을 하면 안됨

    👉 매우 짧은 시간이라 메서드가 끝나기 전에 activity가 종료될 수 있기 때문

- onStop()

  - activity가 사용자에게 더이상 표시되지 않은 상태

  - CPU를 비교적 많이 소모하는 종료 작업을 실행해야함 (onPause가 아닌 onStop 상태에서)

  - activity가 중단되어 있는 상태여서, android OS가 리소스 관리를 위해, 해당 activity가 포함된 프로세스를 종료시킬 수 있음

    👉 오래동안 activity를 실행하지 않거나, RAM 사용량이 많아 프로세스를 종료시킬만큼 리소스가 부족한 상황인 경우,

    프로세스가 종료되면 다시 실행할때 onCreate가 호출됨 

- onDestroy()

  - activity가 완전히 종료되기 전에 실행

<img src="../README.assets/activity_lifecycle.png" alt="activity_lifecycle.png" align="center" width="50%" />

<br>

### 상황별 생명주기 호출되는 시점

- activity가 실행, 종료시에 어떤 생명주기를 따르는지?

  - activity 실행시 생명주기

    onCreate > onStart > onResume

  - activity 종료시 생명주기 (activity 1개여서 앱 종료된 경우)

    onPause > onStop > onDestroy

- 실행중인 앱을 홈버튼 눌러서 백스택에 둔 경우

  👉 화면은 사용자에게 보이지 않지만, activity가 백에 살아있음

  onPause > onStop

- onStop까지 호출된 후 다시 activity가 실행된 경우

  onRestart > onStart > onResume

- 다른 activity 실행시 생명주기

  - 기존 activity 생명주기

    onPause > onStop

  - 다른 activity 생명주기

    onCreate > onStart > onResume
