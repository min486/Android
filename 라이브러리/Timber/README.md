<div align="center">
  <p>
    <img src="../README.assets/android.png">
  </p>
  <br>
  <h2>Library</h2>
  <p>라이브러리 관련 내용 정리</p>
  <br>
  <br>
</div>



## 🔥 Timber

### Timber

> Android 기본 Log 클래스를 대체하기 위해 설계된 간결하고 확장 가능한 로깅 라이브러리

<br>

### 주요 특징

- 자동 TAG 생성
  - 호출된 클래스명을 기반으로 TAG를 자동 생성
  - 별도의 TAG 상수 정의가 필요 없음
- 릴리즈 로그 제어
  - Debug / Release 환경에 따라 로그 출력 여부를 한 곳에서 관리 가능
  
- 코드 가독성
  - `Log.d(TAG, "message")` → `Timber.d("message")`
  - 코드 가독성과 유지보수성 향상
- 확장 가능한 Tree 구조
  - `Tree`를 통해 로그 저장, 서버 전송, 크래시 리포팅 등 커스텀 처리 가능
  - Crashlytics 등 외부 도구 연동에 유리

<br>

### 기존 Log vs Timber 비교

|             | Log 클래스                               | Timber 라이브러리              |
| ----------- | ---------------------------------------- | ------------------------------ |
| TAG 관리    | 매번 수동 지정                           | 호출 클래스 기준 자동 생성     |
| 릴리즈 제어 | 조건문(`if(DEBUG)`)이 코드 전반에 분산됨 | `Tree` 설정으로 중앙 집중 관리 |
| 사용성      | 호출 코드가 길고 반복적                  | 짧고 직관적인 API              |
| 확장성      | 커스터마이징 어려움                      | `Tree` 기반 확장 용이          |

<br>

### 사용 예시

1. 라이브러리 초기화

   Timber는 Application 레벨에서 한 번만 초기화하면 된다

   ```kotlin
   class MyApplication : Application() {
       override fun onCreate() {
           super.onCreate()
         
           // 디버그 모드일 때만 로그 출력
           if (BuildConfig.DEBUG) {
               Timber.plant(Timber.DebugTree())
           }
       }
   }
   ```


2. 로그 출력

   어느 곳에서나 간단하게 호출할 수 있다

   ```kotlin
   // 일반적인 사용 (TAG는 클래스명으로 자동 지정)
   Timber.d("hi timber")
   
   // 문자열 템플릿
   val user = "min"
   Timber.d("user name is $user")
   
   // 예외 로깅
   try {
       throw Exception("test error")
   } catch (e: Exception) {
       Timber.e(e, "에러가 발생했습니다.")
   }
   
   // 특정 TAG 사용
   Timber.tag("customTag").d("특정 태그로 로그 남기기")
   ```

<br>

### 의존성 추가

- `libs.versions.toml` 파일

  ```toml
  [versions]
  timber = "5.0.1"
  
  [libraries]
  timber = { module = "com.jakewharton.timber:timber", version.ref = "timber" }
  ```
  
- 버전 참고

  https://github.com/JakeWharton/timber
  
  https://mvnrepository.com/artifact/com.jakewharton.timber/timber
