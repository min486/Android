<div align="center">
  <p>
    <img src="../README.assets/Git-Icon.png">
  </p>
  <br>
  <h2>Git</h2>
  <p>Git 관련 내용 정리</p>
  <br>
  <br>
</div>


## 🔥 Git 작업 전환 및 충돌 해결

해당 문서는 특정 브랜치에서 작업 중 문제가 발생해 작업을 중단하고 다른 작업을 먼저 진행해야 할 때,

기존 작업을 보관(`stash`)하고 이후 다시 복구 및 합치는 과정을 설명한다

<br>

### 상황 설명

- `feature/old-task` 브랜치에서 작업 중 이슈가 발생하여 작업을 중단

- 현재 상태는 커밋하기엔 미완성이며, 다른 기능(`feature/new-task`)을 먼저 개발해야 하는 상황

- 기존 작업 내용이 새 브랜치에 섞이지 않도록 격리하고,

  이후 최신 `develop` 브랜치 기준으로 다시 이어서 작업하는 것이 목표

<br>

### 작업 전환 프로세스

#### 1. 현재 작업 보관

아직 커밋하지 않은 변경사항을 임시로 보관한다

새로 생성된 파일(Untracked) 까지 함께 보관하기 위해 `-u` 옵션을 사용한다

```bash
git stash -u
```

<br>

#### 2. 기준 브랜치(develop) 최신화 및 새 브랜치 생성

`develop` 브랜치를 최신 상태로 맞춘 후, 새로운 작업을 위한 브랜치를 생성한다

```bash
git checkout develop
git pull origin develop
git checkout -b feature/new-task
```

<br>

#### 3. 새 작업 완료 후 develop 최신화

새 작업을 완료하고 원격 저장소에 반영한 후, 다시 `develop` 브랜치를 최신 상태로 유지한다

```bash
git push origin feature/new-task
git checkout develop
git pull origin develop
```

<br>

#### 4. 이전 작업 브랜치 최신화 및 작업 복구

- 최신 `develop`의 변경사항을 이전 작업 브랜치에 반영

  ```bash
  git checkout feature/old-task
  git merge develop
  ```

- 임시로 보관했던 작업 내용 복구

  ```bash
  git stash pop
  ```

<br>

### Android Studio에서 충돌 해결

`git merge` 또는 `git stash pop` 실행 시 같은 파일의 동일한 코드 영역을 수정했다면 충돌이 발생한다

Android Studio에서는 3-way merge UI를 통해 충돌을 해결할 수 있다

#### 충돌 해결 화면 구성

- 왼쪽 (Local Changes)

  →  현재 브랜치(`feature/old-task`)의 코드

- 오른쪽 (Incoming Changes)

  → 머지 대상 브랜치(`develop`) 또는 stash의 코드

- 가운데 (Result)

  → 최종적으로 반영될 코드

#### 충돌 해결 방법

- 화살표 버튼을 사용해 원하는 변경 사항을 Result 영역으로 가져온다
  - 왼쪽 화살표 : 현재 브랜치 코드 반영
  - 오른쪽 화살표 : 머지 대상 코드 반영
- 두 변경 사항이 모두 필요한 경우
  - 양쪽 화살표를 모두 선택한 후
  - 가운데 영역에서 코드 정리 및 줄 간격을 직접 수정
- 모든 충돌 파일을 해결한 후
  - `Apply` → `Commit` 또는 `Continue Merge`를 선택하여 머지 완료
