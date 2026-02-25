<div align="center">
  <p>
    <img src="../README.assets/android.png">
  </p>
  <br>
  <h2>CI/CD</h2>
  <p>CI/CD 관련 내용 정리</p>
  <br>
  <br>
</div>



## 🔥 CI

### GitHub Actions 개념

> GitHub 저장소 내에서 소프트웨어 개발 워크플로우를 자동화할 수 있게 해주는 도구

- Android 프로젝트에서는 주로 코드 push 시 빌드 성공 여부를 확인하거나,
- Google Play Store에 AAB 파일을 자동으로 업로드하는데 사용한다

<br>

### 구성 요소

- Workflow
  - 최상위 개념으로, 하나 이상의 `Job`으로 구성된 자동화된 전체 프로세스
  - `.github/workflows/*.yml` 파일로 정의한다

- Event
  - 워크플로우를 실행시키는 트리거
  - 예시 : `push`, `pull_request`, `release`

- Job
  - 동일한 러너(Runner)에서 실행되는 `Step`들의 집합
  - 기본적으로 여러 Job은 병렬로 실행된다
- Action
  - 복잡하고 반복적인 작업을 재사용할 수 있도록 만든 구성 요소
  - 예시 : `actions/checkout`, `actions/setup-java`

- Runner
  - 워크플로우가 실행되는 서버
  - GitHub 호스팅 러너 또는 자체 서버 사용 가능

<br>

### 프로젝트 적용 시나리오

Android 개발 환경에서 주로 자동화하는 작업들

- CI (지속적 통합)
  - Lint Check : `Static Analysis`를 통해 코드 컨벤션 및 잠재적 버그 검사
  - Unit Test : `Local Unit Test`를 실행하여 비즈니스 로직 검증
  - Build Check : 프로젝트가 성공적으로 컴파일되는지 확인
- CD (지속적 배포)
  - Firebase App Distribution : QA 및 팀원들에게 테스트용 APK 배포
  - Google Play Store : 정식 출시를 위한 AAB 업로드 및 트랙 관리

<br>

### 보안 설정

Android 프로젝트는 서명 키(Keystore), API Key 등 민감한 정보를 포함한다

GitHub Actions에서는 이를 Repository Secrets로 관리한다

| 구분            | 관리 항목          | 설명                                   |
| ------------------- | ---------------------- | ------------------------------------------- |
| Keystore        | `SIGNING_KEY`          | 앱 서명을 위한 키 파일 |
| Password        | `ALIAS_PASSWORD`       | 키스토어 및 별칭 비밀번호                   |
| Service Account | `SERVICE_ACCOUNT_JSON` | Google Play 배포를 위한 JSON 키             |

<br>

### 적용 예시

프로젝트에 적용된 `android_ci.yml` 구조의 예시

```yaml
name: Android CI

on:
  push:
    branches: ["main", "develop"]
  pull_request:
    branches: ["main", "develop"]
    
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v4
        
      - name: set up JDK 17
        uses: actions/setup-java@v4
        with:
          java-version: '17'
          distribution: 'temurin'
          cache: gradle
          
      - name: Grant execute permission for gradlew
        run: chmod +x gradlew
        
      - name: Run Lint & Unit Test
        run: ./gradlew test
        
      - name: Assemble Debug APK
        run: ./gradlew assembleDebug
```

<br>

### 도입 후 기대 효과

- 빠른 피드백 : 코드리뷰 전 빌드 성공 여부를 즉시 파악 가능
- 배포 실수 방지 : 수동으로 빌드하여 업로드할 때 발생하는 실수 차단
- 일관된 환경 : 개발자의 로컬 환경과 상관없이 동일한 환경에서 빌드 및 테스트가 수행됨
