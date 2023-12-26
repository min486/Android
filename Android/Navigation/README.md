<div align="center">
  <p>
    <img src="../README.assets/android.png">
  </p>
  <br>
  <h2>Android</h2>
  <p>안드로이드 관련 내용 정리</p>
  <br>
  <br>
</div>


## 🔥 Navigation

### Navigation

> 화면 이동을 쉽고 안정적이게 도와주는 Jetpack 라이브러리 중 하나

<img src="../README.assets/navigation.png" alt="navigation" align="center" width="70%" />

<br>

### Navigation 구성요소

- Navigation Graph
  - Navigation 관련 정보를 한눈에 확인할 수 있는 XML 리소스
  - 앱 내의 모든 개별적 콘텐츠 영역을 확인할 수 있다
  - 이동 중 어떤 액션을 취할 것인지, 어떤 데이터를 넘겨줄 것인지에 대한 정보가 있다
- NavHost
  - Navigation Graph에 담겨져 있는 목적지,
  - 즉, 화면을 표현하는 빈 컨테이너 공간
- NavController
  - NavHost에서 앱 탐색을 관리하는 객체
  - NavHost에 어떤 화면을 띄울 것인지 컨트롤하는 역할을 수행한다

<br>

### Navigation 장점

- Fragment 트랜잭션 처리

- 화면 이동 쉽게 처리

- 쉬운 애니메이션 구현 지원

- 딥 링크 구현 및 처리

- Bottom Navigation, Navigation Drawer 등을 쉽게 구현할 수 있도록 지원

- Safe Args를 이용한 안전한 데이터 전달 지원

- ViewModel 지원

  : Navigation 그래프에 대한 ViewModel을 확인해 그래프 대상 사이에 UI 관련 데이터를 공유할 수 있도록 지원

<br>

### Navigation 사용

app 레벨의 build.gradle에 의존성 추가

```kotlin
implementation("androidx.navigation:navigation-fragment-ktx:2.5.3")
implementation("androidx.navigation:navigation-ui-ktx:2.5.3")
```
