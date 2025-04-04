<div align="center">
  <p>
    <img src="../README.assets/jetpack-hero.png">
  </p>
  <br>
  <h2>Jetpack Compose</h2>
  <p>Jetpack Compose 관련 내용 정리</p>
  <br>
  <br>
</div>




## 🔥 HiltViewModel

### HiltViewModel 개념

> Android의 의존성 주입 라이브러리인 Hilt와 Jetpack ViewModel을 연결해주는 어노테이션

<br>

### HiltViewModel 특징

- ViewModel 인스턴스를 Hilt를 사용해 의존성 주입받을 수 있다
- Activity나 Composable의 생명주기에 맞춰 ViewModel을 자동으로 관리한다
- 동일한 Activity 범위 내에서 ViewModel 인스턴스를 일관되게 공유할 수 있다
- ViewModel에 필요한 의존성을 생성자를 통해 주입받을 수 있다

<br>

### 사용 순서

1. 프로젝트 설정
   - 프로젝트 수준 build.gradle에 Hilt 플러그인 추가
   - 앱 수준 build.gradle에 의존성 추가
   - Hilt Application 클래스 생성
2. ViewModel 구현
   - ViewModel 클래스 생성
   - @HiltViewModel 어노테이션 추가
   - 필요한 의존성을 생성자에 @Inject 어노테이션과 함께 선언
3. Compose에서 사용
   - Activity에 @AndroidEntryPoint 어노테이션 추가
   - Composable 함수에서 hiltViewModel() 함수를 통해 ViewModel 인스턴스 사용
