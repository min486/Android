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


## 🔥 Room

### Room 정의

> 상당한 양의 구조화된 데이터를 처리할 때 사용할 수 있는 데이터베이스 라이브러리
>
> (Android 내장 DB에 데이터를 저장하기 위해 사용)

<br>

### Room의 구성요소간의 의존관계

<img src="../README.assets/room.png" alt="room" align="center" width="40%" />

Room을 사용하려면

👉 Room Database에 해당하는 class

👉 Entity에 해당하는 class, 

👉 DAO에 해당하는 interface가 필요하다

<br>

### 3가지 구성요소 상세내용

- Room Database

  - Dao를 사용하여 데이터베이스에 쿼리를 실행한다

  - `@Database`로 어노테이션을 달아야 한다 (데이터베이스라는걸 알려주기 위해)

  - Room Database를 상속받는 클래스는 추상 클래스여야 한다

  - parameter가 0개인 추상 메서드를 포함하고, Dao 클래스를 반환한다

- Entity

  - 데이터베이스에서 테이블을 나타낸다
  - `@Entity`로 어노테이션을 달아야 한다
  - 각 Entity에 대해 연결 된 데이터베이스 객체 내에 테이블이 생성된다

- DAO
  - 데이터에 접근하려면 Dao를 사용해야 한다

  - `@Dao`로 어노테이션을 달아야 한다

  - Dao는 interface 혹은 abstract class일 수 있다

  - 각각의 Dao에는 앱의 데이터베이스에 대한 추상적 엑세스를 제공하는 방법이 포함되어 있다.

    이 Dao 객체들은 Room의 주요 구성요소를 형성한다

  - Dao를 이용하여 데이터베이스에 접근해서, 구조의 다양한 요소를 분리할 수 있다

<br>

### 예제

- Room Database

```kotlin
@Database(entities = [Word::class], version = 1)  // 테이블, 버전명
abstract class AppDatabase : RoomDatabase() {
  abstract fun wordDao(): WordDao
  
}
```

- Entity

```kotlin
@Entity(tableName = "word")  // 테이블 이름 (생략 가능)
data class Word(
  val text : String,
  val mean : String,
  val type : String,
  
  @PrimaryKey(autoGenerate = true)  // id 자동 증가
  val id: Int = 0
)
```

- DAO

```kotlin
@Dao
interface WordDao {
  @Query("SELECT * from word ORDER BY id DESC")  // word 테이블에 있는 모든 내용 가져오기, id 내림차순 정렬
  fun getAll(): List<Word>
  
  @Query("SELECT * from word ORDER BY id DESC LIMIT 1")  // DB 조회, 최신꺼 1개 받기
  fun getLatestWord() : Word
  
  @Insert  // 데이터 추가
  fun insert(word: Word)
  
  @Delete  // 데이터 삭제
  fun delete(word: Word)
  
  @Update  // 데이터 수정
  fun update(word: Word)
}
```
