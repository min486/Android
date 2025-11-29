<div align="center">
  <p>
    <img src="../README.assets/kotlin-hero.png">
  </p>
  <br>
  <h2>Kotlin</h2>
  <p>코틀린 관련 내용 정리</p>
  <br>
  <br>
</div>

## 🔥 Enum Class

### Enum Class

> 유한하고 고정된 값의 집합을 정의할 때 사용되는 클래스

- 각 enum 항목은 싱글턴 객체로 존재하며, 프로퍼티와 메서드를 포함할 수 있다
- 주로 고정된 선택지를 표현할 때 사용된다

<br>

### 주요 특징

- 유한한 값 정의
  - 미리 정의된 상수 외 다른 값 불가
- 단일 인스턴스
  - 각 enum 값은 하나의 객체만 생성됨

- 타입 안정성 보장
  - 문자열 하드코딩 대비 안전 (예: "ALL" 대신 FilterType.ALL)
- 로직 포함 가능
  - 생성자, 프로퍼티, 메서드 포함 가능
- when 문과 함께 사용 용이
  - 모든 enum 값 처리하도록 강제됨 (else 없이 가능)

<br>

### 사용 예시

1. 단순 상수 정의 - UI 필터 옵션

   ```kotlin
   // UI에서 필터링에 사용될 고정된 선택지
   enum class FilterType {
       ALL,
       TALK,
       REVIEW,
       RECOMMENDATION
   }
   
   // Compose UI
   when (filter) {
       FilterType.ALL -> showAll()
       FilterType.TALK -> showTalk()
       FilterType.REVIEW -> showReview()
       FilterType.RECOMMENDATION -> showRecommendation()
   }
   ```

2. 프로퍼티 포함 + 메서드 포함 가능한 enum

   enum 상수에 생성자를 정의하고 프로퍼티를 할당하여, 각 상수와 관련 데이터를 포함할 수 있다

   ```kotlin
   // 각 카테고리 타입에 이름 부여
   enum class CategoryType(val displayName: String) {
       FREE_TALK("자유잡담"),
       FRANCHISE("프랜차이즈"),
       HANDMADE("수제버거"),
       RECOMMENDATION("맛집추천");  // 마지막 enum 상수 뒤에 세미콜론(;)을 넣어 메서드 정의를 구분함
       
       // Enum 상수는 메서드도 가질 수 있음
       fun getFormattedName(): String = "[$displayName]"
   }
   
   val category = CategoryType.FRANCHISE
   println(category.displayName)  // "프랜차이즈"
   println(category.getFormattedName())  // "[프랜차이즈]"
   ```

3. Compose Navigation에 enum 사용 가능

   ```kotlin
   enum class Route(val path: String) {
       HOME("home"),
       COMMUNITY("community"),
       MY("my")
   }
   
   navController.navigate(Route.HOME.path)
   ```
