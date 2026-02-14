<div align="center">
  <p>
    <img src="../README.assets/android.png">
  </p>
  <br>
  <h2>Basic</h2>
  <p>용어 관련 내용 정리</p>
  <br>
  <br>
</div>


## 🔥 Android 용어

### 1. Android 기본

#### Context

- 앱 환경 정보에 접근하기 위한 객체
- 리소스, 시스템 서비스, Activity 시작 등에 사용
- Activity, Application, Service 모두 Context를 가짐

#### Intent

- 컴포넌트 간 작업 요청 메시지
- Activity 전환, 데이터 전달, 시스템 기능 호출
- Explicit / Implicit Intent로 구분

#### Intent Extra

- Intent에 포함되는 부가 데이터

- key-value 형태로 전달
- 알림 클릭, 로그인 콜백, 딥링크 처리에 사용

#### Application

- 앱 프로세스 전체 생명주기를 관리하는 클래스
- 전역 상태, 초기화 로직에 사용
- Context 중 가장 긴 생명주기

#### ANR (Application Not Responding)

- 메인 스레드가 일정 시간 응답하지 않으면 발생
- 원인 : 네트워크, 파일 IO, 무한 루프 등
- 해결 : 코루틴, 비동기 처리, 백그라운드 스레드 사용

#### 권한 (Permission)

- 민감한 기능 접근을 위한 사용자 승인 체계
- 일반 / 위험 권한으로 구분

<br>

### 2. 앱 런타임 / 시스템

#### Notification Channel

- Android 8.0 이상 알림 필수 구성 요소
- 알림 중요도, 진동, 소리 설정 단위
- 사용자 설정으로 알림 제어 가능

#### SplashScreen API

- 앱 시작 시 기본 스플래시 화면 제공
- 앱 초기화 완료 시점까지 유지 가능
- Android 12 이상 표준화된 스플래시 처리 방식

#### enableEdgeToEdge

- 시스템바 영역까지 UI 확장
- 몰입형 UI 구성
- Insets 처리를 직접 제어 가능

<br>

### 3. Gradle / Build 시스템

#### Gradle

- Android 빌드 자동화 도구
- 의존성 관리, 빌드 설정 담당
- Groovy / Kotlin DSL 사용

#### AGP (Android Gradle Plugin)

- Android 빌드를 지원하는 Gradle 플러그인
- compileSdk, buildTypes, flavor 등 제공
- Android Studio와 강하게 결합됨

#### plugins

- 빌드 기능 확장 도구
- Compose, Hilt, KSP, Firebase 등 설정
- 프로젝트 빌드 동작 방식 결정

#### dependencies

- 프로젝트에서 사용하는 외부 라이브러리
- implementation / testImplementation 등 스코프 존재
- 컴파일/런타임 영향 범위 제어

#### buildTypes

- 빌드 결과물의 종류 정의
- debug / release
- 로깅, 난독화, 서버 환경 분리

#### debug

- 개발용 빌드
- 디버깅, 로그, 테스트 기능 활성
- 앱 서명 키 분리 가능

#### release

- 배포용 빌드
- Proguard/R8 적용
- 성능, 보안 최적화 대상

#### minSdk

- 앱이 지원하는 최소 Android 버전
- 해당 API 이상 기기에서만 설치 가능

#### targetSdk

- 앱이 테스트된 Android 버전
- 최신 시스템 동작 방식 적용 기준
- 런타임 권한, 백그라운드 제한 등에 영향

#### compileSdk

- 컴파일 시 사용하는 Android API 버전
- 최신 API 사용 가능 여부 결정
- 실행 기기 버전과는 무관

<br>

### 4. 설정 / 보안

#### Properties (local.properties)

- 로컬 환경 전용 설정 파일
- API Key, 민감 정보 저장
- Git에 커밋되지 않음

#### buildConfigField

- BuildConfig 클래스에 상수 생성
- 환경별 값 분기 가능
- 런타임에서 안전하게 접근

#### manifestPlaceholders

- AndroidManifest.xml에 값 주입
- 앱 이름, 스킴, 키 설정
- buildType별 분기 가능

<br>

### 5. 중앙 의존성 관리

#### libs.versions.toml

- 의존성과 버전을 중앙에서 관리
- 버전 충돌 방지
- 멀티 모듈에서 일관성 유지

#### BOM (Bill of Materials)

- 라이브러리 묶음 버전 관리 방식
- Firebase, Compose에서 사용
- 개별 버전 명시 불필요
