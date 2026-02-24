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



## 🔥 GitHub Actions

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
