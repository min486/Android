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

<br>

### 데이터 전달

- 대상 간 이동을 위해 Safe Args Gradle 플러그인을 사용하면 안정성을 보장한다

- 이 플러그인은 대상 간 유형 안전 탐색 및 인수 전달을 사용 설정하는 간단한 객체 및 빌더 클래스를 생성한다

최상위(프로젝트) 수준의 build.gradle 파일에 아래 추가

```kotlin
plugins {
    id ("androidx.navigation.safeargs.kotlin") version "2.5.3" apply false
}
```

app 수준의 build.gradle 파일에 아래 추가

```kotlin
plugins {
    id("androidx.navigation.safeargs.kotlin")
}
```

<br>

### 대상으로 이동

대상으로 이동하는 것은 NavController 객체를 사용하여 실행되며 이 객체는 NavHost 내에서 앱 탐색을 관리한다

각 NavHost에는 해당하는 자체 NavController가 있다

다음 메서드 중 하나를 사용하여 NavController를 검색할 수 있다

- Fragment.findNavController()
- View.findNavController()
- Activity.findNavController(viewId: Int)

```kotlin
// exampleFragment.kt

val action = AuthFragmentDirections.actionAuthFragmentToHomeFragment()
findNavController().navigate(action)
```

👉 safe args 플러그인을 사용하면 직접 찾을 필요 없이 미리 만들어준것 사용 가능

<br>

FragmentContainerView를 사용하여 NavHostFragment를 만들 때 

또는 

FragmentTransaction을 통해 NavHostFragment를 activity에 수동으로 추가할 경우

👉 NavHostFragment에서 직접 NavController를 검색해야 한다

```kotlin
// activity - FragmentContainerView랑 BottomNavigationView 연결하는 예제

val navHostFragment = supportFragmentManager.findFragmentById(R.id.nav_host_fragment) as NavHostFragment
    binding.bottomNavigationView.setupWithNavController(navHostFragment.navController)
```

