<div align="center">
  <p>
    <img src="../README.assets/android.png">
  </p>
  <br>
  <h2>Architecture</h2>
  <p>아키텍처 관련 내용 정리</p>
  <br>
  <br>
</div>


## 🔥 MVVM & 클린 아키텍처

### 개요

> MVVM(Model-View-ViewModel)과 클린 아키텍처를 결합하면,
>
> 관심사 분리와 테스트 용이성을 극대화해 유지보수하기 쉬운 앱을 만들 수 있다

<br>

### 클린 아키텍처 핵심 원칙

- 의존성 규칙 : 내부 계층은 외부 계층에 의존하지 않는다
- 관심사 분리 : 각 계층은 고유한 책임을 가진다
- 테스트 용이성 : 각 계층을 독립적으로 테스트할 수 있다

<br>

### 계층 구조

#### 1) Presentation Layer

- 역할 : UI 렌더링, 사용자 상호작용 처리, 상태 관리
- 구성 :
  - View (Composable) : 사용자 이벤트를 처리, ViewModel의 상태 관찰
  - ViewModel : UI 상태를 관리, 비즈니스 로직을 담고 있는 UseCase를 호출
- 의존성 : Domain 계층에 의존
- 의존 대상 : Domain 계층의 UseCase와 Repository 인터페이스

```kotlin
presentation/
├── ui/
│   ├── home/
│   │   ├── HomeScreen.kt
│   │   └── HomeViewModel.kt  // 홈 화면의 상태(State)와 로직 관리
│   └── article/
│       ├── ArticleListScreen.kt
│       ├── ArticleDetailScreen.kt
│       └── ArticleViewModel.kt  // 아티클 관련 상태와 UseCase 호출
└── common/
    └── CommonUiUtils.kt  // 공통 UI 유틸리티 함수 모음
```

#### 2) Domain Layer

- 역할 : 앱의 핵심 비즈니스 로직 정의 (순수 Kotlin 코드)
- 구성 :
  - Entity : 비즈니스 모델을 정의하는 데이터 클래스
  - Repository Interface : 데이터 계층이 제공해야 할 기능을 추상화
  - UseCase : 특정 비즈니스 로직을 수행하는 실행 단위
- 의존성 : 다른 계층에 전혀 의존하지 않음

```kotlin
domain/
├── model/
│   └── Article.kt // 앱에서 사용하는 핵심 비즈니스 모델 (Entity)
├── repository/
│   └── ArticleRepository.kt // Repository 인터페이스 (Data 계층이 구현)
└── usecase/
    ├── GetArticleListUseCase.kt  // 아티클 목록 가져오기 유스케이스
    ├── GetArticleDetailUseCase.kt
    └── SearchArticleUseCase.kt
```

#### 3) Data Layer

- 역할 : 실제 데이터 소스(네트워크, DB 등) 관리
- 구성 :
  - Repository 구현체 : Domain 계층의 Repository 인터페이스 구현
  - API/Service : Retrofit2와 같은 라이브러리를 사용해 네트워크 통신
  - DTO (Data Transfer Object) : API 응답 객체 정의, Entity로 변환 담당
- 의존성 : Domain 계층에 의존
- 의존 대상 : Domain 계층의 Repository 인터페이스

```kotlin
data/
├── api/
│   ├── ArticleApi.kt  // Retrofit2 인터페이스, 네트워크 통신 정의
│   └── dto/
│       └── ArticleDto.kt  // 서버와 주고받는 데이터 전송 객체 (네트워크 전용)
├── di/
│   └── DataModule.kt  // Repository 구현체와 ApiService를 제공하는 Hilt 모듈
└── repository/
    └── ArticleRepositoryImpl.kt  // Domain의 Repository 인터페이스 구현체
```

<br>

### 데이터 흐름

MVVM과 클린 아키텍처가 결합된 앱에서 데이터는 아래와 같은 단방향 흐름을 따른다

1. `User 입력 → View (Composable)`
   - 사용자 이벤트(클릭, 스크롤 등)가 Composable UI를 통해 감지된다
2. `View → ViewModel`
   - View는 이벤트를 ViewModel의 함수 호출로 전달한다
3. `ViewModel → UseCase`
   - ViewModel은 비즈니스 로직을 실행하기 위해 Domain 계층의 UseCase를 호출한다
4. `UseCase → Repository`
   - UseCase는 데이터 접근을 위해 Domain 계층에 정의된 Repository 인터페이스를 호출한다
   - 실제 동작은 Data 계층의 Repository 구현체에서 처리된다
5. `Data → Domain → ViewModel`
   - Data 계층에서 처리된 결과가 Domain을 거쳐 ViewModel로 역방향 전달된다
6. `ViewModel → State 업데이트 → View`
   - ViewModel은 전달받은 데이터를 통해 UI 상태를 업데이트한다
   - Compose는 state 변화를 자동으로 감지하여 화면을 재구성한다

<br>

### MVVM & 클린 아키텍처 장점/단점

- 장점
  - 확장성 : 새로운 기능 추가 시 기존 코드에 미치는 영향이 적어 유연하게 확장 가능
  - 테스트 용이성 : 각 계층이 독립적이므로 단위 테스트 작성이 쉽다
  - 유지보수성 : 책임이 명확하게 분리되어 있어 코드 이해 및 수정이 쉽다
  - 재사용성 : 비즈니스 로직을 담은 UseCase를 여러 ViewModel에서 재사용 가능
- 단점
  - 복잡성 : 소규모 앱에는 과도한 구조가 될 수 있음
  - 보일러플레이트 증가 : 인터페이스와 구현체 등 반복되는 코드가 많아 초기 개발 비용이 높음
  - 러닝 커브 : 아키텍처에 대한 이해가 필요하므로 초기 학습 비용이 높음
