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


## 🔥 스레드 (Thread)

### 스레드 정의

> 작업 공간

<br>

앱을 안드로이드 기기에서 실행하면, 안드로이드 시스템(OS)은 앱을 실행하기 위한 작업공간을 주게 된다

👉 이렇게 생성된 메인 스레드에서 UI 조작을 한다

<br>

### 스레드 종류

- 메인 스레드 (UI 스레드)

  : 앱이 실행되면 안드로이드 시스템이 생성하는 스레드로, UI 관련된 작업을 한다

  *1개만 생김

- 작업자 스레드 (Worker Thread)

  : 메인스레드 이외의 스레드

<br>

### 메인 스레드의 중요성 & 규칙

사용자와 상호작용하는 공간이 UI 이기 때문에, 안드로이드 앱에서 가장 중요한 것은 UI 이다

👉 따라서 안드로이드에서 메인 스레드 관리를 잘 해야한다

<br>

규칙

1. UI 스레드를 차단하면 안된다

   - UI 스레드는 오직 UI만 그리고, UI 스레드에서 다른 작업을 할 수 없다. 시간이 오래 걸리는 작업은 작업자 스레드에서 할 수 있다

   - UI 스레드가 몇 초 이상 차단되면 사용자에서 ANR이 표시된다

     *ANR : Application Not Responding

2. UI 스레드에서만 UI를 조작할 수 있다 (작업자 스레드에서 안드로이드 UI를 작업하면 안된다)
   - UI를 그리는 작업이 매우 중요하므로 오직 UI 스레드에서만 작업해야하기 때문
   - 만약 여기저기서 UI를 조작하게 되면, 어떤 작업을 먼저 해야될지 시스템에서는 알 수가 없다

<br>

### 작업자 스레드에서 UI 작업

실제로 작업을 하다보면 UI 스레드가 아닌 작업자 스레드에서도 UI 조작을 해야되는 경우가 있다

👉 이 경우 해결할 수 있는 방법들

- `Activity.runOnUiThread(Runnable)`
- `View.post(Runnable)`
- `View.postDelayed(Runnable, long)`
- Handler

<br>

### 스레드에서 Handler, Looper

앱을 실행하면 메인 스레드가 생기고, 메인 스레드에서 UI 작업을 한다

<br>

그림 설명

- 메인 스레드 안에 Looper가 있다
  - Looper는 Message Queue를 갖고 있다
- 작업자 스레드에서는 UI에 접근할 수 없다
  - UI를 조작하고 싶은 경우, Handler를 통해서 message 또는 runnable한 객체나 보낼 수 있다
- 메인 스레드 안에 있는 Handler가 다른 스레드들이 보낸 message를 받게 된다
  - Handler는 Message Queue에 내용을 삽입한다 (선입선출)
- Looper는 Message Queue의 내용들을 계속 확인한다
  - message가 들어온 경우, Handler에게 처리하라고 보낸다 (handleMessage() 호출)
  - runnable한 객체가 들어온 경우, 실제 실행하고 싶은 객체이기 때문에 Looper가 실행한다

<img src="../README.assets/thread.png" alt="thread.png" align="center" width="40%" />
