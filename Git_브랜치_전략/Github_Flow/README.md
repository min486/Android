<div align="center">
  <p>
    <img src="../README.assets/Git-Icon.png">
  </p>
  <br>
  <h2>Git Branch 전략</h2>
  <p>Git 브랜치 전략 관련 내용 정리</p>
  <br>
  <br>
</div>





## 🔥 Github Flow

### Git 브랜치 전략

> 여러 개발자가 하나의 저장소에 작업을 할 때, 협업을 좀 더 효과적으로 하기 위해
>
> git branch에 대한 규칙을 정하고 저장소를 잘 활용하기 위한 workflow를 정의하는 것

<br>

### Github Flow

Github Flow 전략은 기능 개발, 버그 픽스 등 어떤 이유로든 새로운 브랜치를 생성하는 것으로 시작된다


브랜치 하나에 의존하게 되기 때문에 브랜치 이름을 통해 의도를 명확하게 드러내는 것이 중요하다

<br>

단순한 개발 과정으로 작은 규모의 팀부터 큰 규모의 팀까지 효과적으로 적용 가능하다

단순하기 때문에 개발 진행 과정이 빠르고 빠르게 수정 배포할 수 있는 전략이다

👉 하나의 base 브랜치 (master) + master 에 기능을 추가하기 위한 브랜치 (feature)

<br>

### 개발 과정의 기본적인 흐름

- master 브랜치는 항상 배포할 수 있는 상태로 둔다

- 새로운 작업을 수행할 때는 master 브랜치에서 새로운 브랜치를 생성한다

  이때, 새로운 브랜치의 이름은 무슨 작업을 할지 알 수 있도록 자세히 적는다

- 새로 만든 브랜치에서 작업을 진행하고 commit을 수행한다

  주기적으로 push 하고, 도움을 주거나 피드백을 원할 때는 Pull Request를 주고 받는다

- 다른 개발자가 리뷰하고 작업 종료를 확인하면 master 브랜치에 통합한다

- master 브랜치에 통합하면 곧바로 배포한다

  테스트가 없는 코드 또는 테스트를 실패한 코드는 절대로 master 브랜치에 넣어서는 안된다

<br>

### Github Flow의 WorkFlow

<img src="../README.assets/github-flow-branching-model.jpeg" alt="github-flow" align="center" width="50%" />
