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



## 🔥 CI (Continuous Integration)

### GitHub Actions 개념

> GitHub 저장소 내에서 소프트웨어 개발 워크플로우를 자동화하는 도구
>
> `.gilthub/workflows/` 경로에 YAML 파일을 작성하는 것만으로 동작한다

Android 프로젝트에서 CI 목적으로 주로 사용하는 시나리오

- 코드 Push 또는 PR 시 → 빌드가 깨지지 않는지 자동 확인
- 유닛 테스트 자동 실행 → 비즈니스 로직 검증
- Lint 검사 → 코드 컨벤션, 잠재적 버그 탐지

<br>

### 워크플로우 구성 요소

GitHub Actions의 YAML 파일은 아래 핵심 개념으로 구성된다

#### Workflow (워크플로우)

- 최상위 개념으로, 자동화된 전체 프로세스
- `.github/workflows/*.yml` 파일 하나가 워크플로우 하나다
- 여러개의 `Job`으로 구성된다

```yaml
name: Android CI Build  # 워크플로우 이름 (GitHub UI에서 표시됨)
```

#### Event (트리거)

- 워크플로우를 실행시키는 조건
- 특정 브랜치에 push하거나 PR이 열렸을 때 등을 설정할 수 있다

```yaml
on:
  push:
    branches:
      - develop  # develop 브랜치에 push될 때 실행
  pull_request:
    - develop  # develop 브랜치로 PR이 열리거나 업데이트될 때 실행
```

| 이벤트 종류       | 설명                             |
| ----------------- | -------------------------------- |
| push              | 특정 브랜치에 커밋이 push될 때   |
| pull_request      | PR이 생성되거나 업데이트될 때    |
| release           | GitHub Release가 생성될 때       |
| Workflow_dispatch | GitHub UI에서 수동으로 실행할 때 |
| Schedule          | cron 표현식으로 주기적 실행      |

#### Job (잡)

- 동일한 Runner에서 실행되는 Step들의 집합
- 기본적으로 여러 Job은 병렬로 실행되며, `needs` 키워드로 의존 관계를 설정하면 순차 실행이 가능하다

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - ...
```

#### Step (스텝)

- Job 안에서 순서대로 실행되는 개별 작업
- `uses`로 미리 만들어진 Action을 사용하거나, `run`으로 직접 셸 명령어를 실행한다

```yaml
steps:
  - name: Checkout Code        # 표시될 이름
    uses: actions/checkout@v4  # 공개 Action 사용
    
  - name: Run Tests
    uses: ./gradlew testDebugUnitTest  # 직접 명령어 실행
```

#### Action (액션)

- 복잡하고 반복적인 작업을 재사용 가능하도록 패키징한 단위
- GitHub Marketplace에서 다양한 공개 Action을 찾아 `uses`로 바로 사용할 수 있다

| Action                     | 역할                            |
| -------------------------- | ------------------------------- |
| actions/checkout@v4        | 저장소 코드를 Runner로 가져오기 |
| actions/setup-java@v4      | 특정 버전의 JDK 설치            |
| actions/upload-artifact@v4 | 빌드 결과물을 GitHub에 저장     |

#### Runner (러너)

- 워크플로우가 실제로 실행되는 가상 서버 환경
- `runs-on: ubuntu-latest`처럼 지정하며, GitHub에서 무료로 제공하는 환경을 사용할 수 있다

| Runner         | OS                            |
| -------------- | ----------------------------- |
| ubuntu-latest  | Ubuntu Linux (가장 많이 사용) |
| windows-latest | Windows                       |
| macos-latest   | macOS                         |

<br>

### 보안 설정 (Secrets)

- Android 프로젝트에는 외부에 노출되면 안되는 민감한 정보가 있다
- 이 값들은 코드에 직접 작성하면 안되고, GitHub Repository Secrets에 등록해서 사용한다

- 등록 경로 :

  - GitHub 저장소 → Settings → Secrets and variables → Actions → New repository secret

- 프로젝트에서 사용했던 Secrets

  | Secret 이름            | 용도                                                         |
  | ---------------------- | ------------------------------------------------------------ |
  | GOOGLE_SERVICES_BASE64 | Firebase 연동을 위한 google-services.json을 Base64로 인코딩한 값 |
  | KAKAO_NATIVE_APP_KEY   | 카카오 Native App Key                                        |
  | NAVER_CLIENT_ID        | 네이버 Client ID                                             |
  | NAVER_CLIENT_SECRET    | 네이버 Client Secret                                         |

- 워크플로우에서는 ${{ secrets.SECRET_NAME }} 문법으로 참조한다

  ```yaml
  env:
    KAKAO_NATIVE_APP_KEY: ${{ secrets.KAKAO_NATIVE_APP_KEY }}
  ```

- Base64로 인코딩하는 이유

  - google-services.json은 파일이기 때문에 Secret에 바로 저장하기 어렵다
  - 파일을 Base64 문자열로 변환하여 Secret에 저장하고,
  - 워크플로우에서 디코딩하여 파일로 복원한다

<br>

## CI 워크플로우 예시 코드

```yaml
name: Android CI Build

on:
  push:
    branches:
      - develop
  pull_request:
    branches:
      - develop


jobs:
  build:
    runs-on: ubuntu-latest

    env:
      GOOGLE_SERVICES_BASE64: ${{ secrets.GOOGLE_SERVICES_BASE64 }}
      KAKAO_NATIVE_APP_KEY: ${{ secrets.KAKAO_NATIVE_APP_KEY }}
      NAVER_CLIENT_ID: ${{ secrets.NAVER_CLIENT_ID }}
      NAVER_CLIENT_SECRET: ${{ secrets.NAVER_CLIENT_SECRET }}

    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Set up JDK 17
        uses: actions/setup-java@v4
        with:
          distribution: 'temurin'
          java-version: '17'

      - name: Make gradlew executable
        run: chmod +x ./gradlew

      - name: Decode Google Services File
        run: echo "$GOOGLE_SERVICES_BASE64" | base64 -d > app/google-services.json

      - name: Create local.properties for CI
        run: |
          echo "kakao.native.app.key=$KAKAO_NATIVE_APP_KEY" > local.properties
          echo "naver.client.id=$NAVER_CLIENT_ID" >> local.properties
          echo "naver.client.secret=$NAVER_CLIENT_SECRET" >> local.properties

      - name: Run Unit Tests
        run: ./gradlew testDebugUnitTest

      - name: Upload Test Results
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: test-results
          path: app/build/reports/tests/testDebugUnitTest
          retention-days: 7

      - name: Build Debug APK
        run: ./gradlew assembleDebug

      - name: Upload APK Artifact
        uses: actions/upload-artifact@v4
        with:
          name: dngo-app-debug
          path: app/build/outputs/apk/debug/app-debug.apk
          retention-days: 90
```

<br>

### 각 단계 설명

#### 1. Checkout Code

```yaml
- name: Checkout Code
  uses: actions/checkout@v4
```

- Runner(가상 서버)는 처음에 아무 코드도 없는 빈 환경이다
- `actions/checkout`은 현재 저장소의 코드를 Runner 작업 디렉토리로 가져오는 역할을 한다
- 이 Step이 없으면 이후 모든 빌드 명령어가 동작하지 않는다

#### 2. Set up JDK 17

```yaml
- name: Set up JDK 17
  uses: actions/setup-java@v4
  with:
    distribution: 'temurin'  # Eclipse Temurin 배포판 사용
    java-version: '17'       # JDK 17 설치
```

- Android 프로젝트 빌드에는 JDK가 필요하다
- Runner의 기본 JDK 버전이 프로젝트와 맞지 않을 수 있으므로, 명시적으로 설정한다

#### 3. Make gradlew executable

```yaml
- name: Make gradlew executable
  run: chmod +x ./gradlew
```

- Linux/macOS 환경에서는 `gradlew` 파일에 실행 권한이 없으면 Permission denied 오류가 발생한다
- `chmod +x` 명령어로 실행 권한을 부여한다

#### 4. Decode Google Services File

```yaml
- name: Decode Google Services File
  run: echo "$GOOGLE_SERVICES_BASE64" | base64 -d > app/google-services.json
```

- `google-services.json`은 `.gitignore`에 등록되어 저장소에 포함되지 않는다

- Secret에 저장된 Base64 문자열을 디코딩하여 Runner에 파일로 복원한다

- 동작 흐름 :

  Secret(Base64 문자열) → base64 -d 디코딩 → app/google-services.json 파일 생성

#### 5. Create local.properties for CI

```yaml
- name: Create local.properties for CI
  run: |
    echo "kakao.native.app.key=$KAKAO_NATIVE_APP_KEY" > local.properties
    echo "naver.client.id=$NAVER_CLIENT_ID" >> local.properties
    echo "naver.client.secret=$NAVER_CLIENT_SECRET" >> local.properties
```

- `local.properties`도 `.gitignore`에 등록되어 저장소에 없다
- Gradle 빌드 시 이 파일에서 API Key를 읽어야해서 Secrets 값으로 파일을 직접 생성한다
- `>` : 파일 새로 생성 (기존 내용 덮어쓰기)
- `>>` : 기존 파일에 내용 추가

#### 6. Run Unit Tests

```yaml
- name: Run Unit Tests
  run: ./gradlew testDebugUnitTest
```

- Debug 빌드 타입 기준으로 유닛 테스트를 실행한다
- 테스트가 하나라도 실패하면 이 단계는 실패 상태가 되고, 이후 단계는 실행되지 않는다

#### 7. Upload Test Results

```yaml
- name: Upload Test Results  # Artifact 이름 (GitHub Actions 탭에서 표시)
  if: always()
  uses: actions/upload-artifact@v4
  with:
    name: test-results
    path: app/build/reports/tests/testDebugUnitTest  # 저장할 파일/폴더 경로
    retention-days: 7                                # 보존 기간 (이후 자동 삭제)
```

- 테스트 결과 HTML 리포트를 GitHub Artifact로 저장한다
- `if: always()` 역할
  - 기본적으로 이전 단계가 실패하면 이후 단계는 실행되지 않는다
  - `if: always()`를 사용하면 테스트가 실패하더라도 이 단계는 항상 실행되어, 실패 원인을 담은 리포트를 확인할 수 있다

#### 8. Build Debug APK

```yaml
- name: Build Debug APK
  run: ./gradlew assembleDebug
```

- 테스트가 모두 통과되면 Debug APK를 빌드한다
- 이 단계의 성공 여부로 코드가 컴파일 가능한 상태인지 최종 확인한다

#### 9. Upload APK Artifact

```yaml
- name: Upload APK Artifact
  uses: actions/upload-artifact@v4
  with:
    name: dngo-app-debug
    path: app/build/outputs/apk/debug/app-debug.apk
    retention-days: 90
```

- 빌드된 Debug APK를 GitHub Artifact로 저장한다
- 90일간 보관되며, GitHub Actions 탭 → 해당 워크플로우 실행 → Artifacts 섹션에서 다운로드 가능하다

<br>

### 도입 효과

- 빠른 피드백 : PR을 올린 후 빌드/테스트 결과를 자동으로 확인할 수 있어, 코드리뷰 전에 기본적인 품질 검증이 완료된다
- 팀 전체의 코드 안정성 보장 : 로컬 활경에 상관없이 동일한 환경에서 빌드/테스트가 수행되므로 안정성이 보장된다
- 테스트 결과 기록 보존 : Artifact로 저장된 테스트 리포트를 통해 어떤 테스트가, 왜 실패했는지 나중에도 확인할 수 있다
