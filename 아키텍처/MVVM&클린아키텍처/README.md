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

> MVVM(Model-View-ViewModel)과 클린 아키텍처를 결합한 패턴으로,
>
> 관심사 분리와 테스트 용이성을 극대화하기 위한 아키텍처

<br>

### 클린 아키텍처 핵심 원칙

- 의존성 규칙 : 내부 계층은 외부 계층에 대해 알지 못함
- 관심사 분리 : 각 계층은 고유한 책임을 가짐
- 테스트 용이성 : 각 계층을 독립적으로 테스트 가능

### 계층 구조

#### 1) Presentation Layer

```kotlin
presentation/
├── di/
│   └── PresentationModule.kt
├── ui/
│   ├── home/
│   │   ├── HomeScreen.kt
│   │   └── HomeViewModel.kt
│   └── article/
│       ├── ArticleListScreen.kt
│       ├── ArticleDetailScreen.kt
│       └── ArticleViewModel.kt
└── common/
    └── CommonUiUtils.kt
```

#### 2) Domain Layer

```kotlin
domain/
├── model/
│   └── Article.kt
├── repository/
│   └── ArticleRepository.kt
└── usecase/
    ├── GetArticleListUseCase.kt
    ├── GetArticleDetailUseCase.kt
    └── SearchArticleUseCase.kt
```

#### 3) Data Layer

```kotlin
data/
├── api/
│   ├── ArticleApi.kt
│   └── dto/
│       └── ArticleDto.kt
├── di/
│   └── DataModule.kt
└── repository/
    └── ArticleRepositoryImpl.kt
```

#### 



