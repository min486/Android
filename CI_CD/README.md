<div align="center">
  <p>
    <img src="./README.assets/android.png">
  </p>
  <br>
  <h2>CI/CD</h2>
  <p>CI/CD 관련 내용 정리</p>
  <br>
  <br>
</div>




## 🔥 CI / CD

### CI/CD 개념

> 소프트웨어 개발에서 코드 통합 → 검증 → 배포 과정을 자동화하는 방법론

- 수동으로 반복하던 빌드, 테스트, 배포 작업을 자동화하여
- 생산성을 높이고 실수를 줄이는 것이 목표다

```
개발자 코드 작성
      ↓
   Git Push
      ↓
 [CI] 빌드 + 테스트 + 정적분석  ← 문제 발생 시 즉시 알림
      ↓ 통과
 [CD] 서명 + AAB 생성 + 배포
      ↓
  Google Play / Firebase
```

<br>

### CI (Continuous Integration, 지속적 통합)

> 개발자가 작성한 코드를 메인 저장소에 자주 통합하고,
>
> 통합 시 자동으로 빌드, 테스트, 정적 분석을 수행해 문제를 조기에 발견하는 프로세스

- 목표
  - 코드 충돌 및 버그를 빠르게 발견
  - 모든 팀원이 안정적인 코드 기반에서 개발 가능
  - 반복되는 검사를 자동화하여 생산성 향상
- 자동화 범위
  - 빌드 성공 여부 확인
  - 유닛 테스트 실행
  - Lint (정적 분석, 코드 스타일 검사)

<br>

### CD (Continuous Deployment, 지속적 배포)

> CI 단계가 완료된 코드를 자동으로 배포 환경까지 전달하는 프로세스

- 목표
  - 빠르고 안정적인 배포
  - 배포 과정의 수동 작업 감소
  - QA, 베타, 프로덕션 등 여러 트랙으로 일관된 배포
- 자동화 범위
  - 앱 서명 (Keystore)
  - APK/AAB 생성
  - Google Play Store 업로드 또는 GitHub Release 생성

<br>

### GitHub Actions vs Jenkins 비교

|             | GitHub Actions                             | Jenkins                         |
| ----------- | ------------------------------------------ | ------------------------------- |
| 서버 관리   | 불필요 (GitHub 관리)                       | 직접 서버 구축, 운영 필요       |
| 설정 방법   | YAML 파일                                  | Groovy 기반 Jenkinsfile         |
| GitHub 연동 | 완벽 통합                                  | 플러그인 필요                   |
| 유연성      | 보통                                       | 매우 높음                       |
| 비용        | Public 무료, Private 일정량 무료 후 종량제 | 소프트웨어 무료, 서버 비용 발생 |
| 적합한 환경 | 개인/소규모 팀, GitHub 기반 프로젝트       | 기업 내부망, 복잡한 파이프라인  |

➡️ 개인 프로젝트나 소규모 팀에서 GitHub를 사용 중이라면 GitHub Actions를 빠르게 도입할 수 있다
