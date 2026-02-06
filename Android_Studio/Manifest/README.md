<div align="center">
  <p>
    <img src="../README.assets/studio.png">
  </p>
  <br>
  <h2>Android Studio</h2>
  <p>안드로이드 스튜디오 관련 내용 정리</p>
  <br>
  <br>
</div>


## 🔥 AndroidManifest

### 1. AndroidManifest.xml

> 안드로이드 시스템이 앱을 실행하기 전 반드시 알아야 하는 핵심 정보를 선언하는 파일

<br>

### 2. Manifest 역할

- 앱 구성 요소 선언 : Activity, Service, Receiver, Provider를 시스템에 등록한다
- 권한 관리 : 앱이 실행되는데 필요한 시스템 권한(인터넷, 알림 등)을 명시한다
- 연결 고리 : `intent-filter`를 통해 앱이 어떤 데이터를 처리할 수 있는지 시스템에 알린다

<br>

### 3. 핵심 구성 요소

- intent-filter
  - 시스템이 특정 액션(클릭, 웹 링크 등)을 감지했을 때, 앱의 어떤 컴포넌트가 응답할지 결정하는 필터
  - 예시 : `MAIN/LAUNCHER` (앱 실행 아이콘), `VIEW` (콜백 링크 처리)
- service
  - 화면 없이 백그라운드에서 오래 실행되는 작업
  - 예시 : `MyFirebaseMessagingService` (FCM 알림 수신 대기)
- meta-data
  - 앱 구성 요소에 추가적인 설정 값을 전달하는 태그
  - 주로 외부 라이브러리(FCM 등) 설정에 쓰인다
  - 예시 : 알림 아이콘, 알림 색상 등

<br>

### 4. 주요 속성

#### android:exported (보안)

- 다른 앱이나 시스템이 해당 컴포넌트(Activity, Service 등)를 실행할 수 있는지 여부

- `intent-filter`가 있으면 기본적으로 `true`가 되어야 외부(카카오톡, 브라우저 등)에서 해당 앱을 열 수 있다

  반면, 내부용 서비스는 `false`로 설정해 보안을 강화해야 한다

#### android:launchMode (UX)

- 액티비티가 실행되는 방식

- 예시 : 로그인 처리 액티비티에 쓰인 `singleTask`는 이미 열려있는 창이 있다면 새로 만들지 않고 재사용한다는 뜻

  (로그인 중복 실행 방지)

#### android:windowSoftInputMode (UI)

- 키보드가 올라올 때 화면 레이아웃을 어떻게 조정할지 결정
- 예시 : `adjustResize`는 키보드 공간만큼 화면 크기를 줄여서, 입력창이 키보드에 가려지지 않게 한다
