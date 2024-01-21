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



## 🔥 Gson

### Json

> JavaScript Object Notation의 약자로, Name과 Value로 이루어진 하나의 텍스트 형식이다
>
> 데이터를 구조적인 문자열로 표현한 데이터 형식이다

<br>

JSON에서는 기본적으로 key : value 형태로 데이터를 표현한다

데이터들을 묶어 Object(중괄호 { }로 표기)와 Array(대괄호 [ ]로 표기) 형태로 표현한다

속성 이름과 속성 값은 ':' 로, 여러각각의 속성은 ',' 로 구분되어 작성이 된다

```json
{
    "속성 이름" : "속성 값",
    "속성 이름" : "속성 값",
    "속성 이름" : "속성 값"
}
```

<br>

### Gson

Java 객체를 Json으로 변환 또는 Json을 Java 객체로 변환하는데 사용한다

<br>

Java 객체 ➡️ Json : `toJson()`

Json ➡️ Java 객체 : `fromJson()`

```kotlin
class BasketConverter {
  private val gson = GsonBuilder().create()  // Gson 객체 선언
  
  @TypeConverter
  fun fromPrice(value: Price) : String {
    return gson.toJson(value)
  }
  
  @TypeConverter
  fun toPrice(value: String) : Price {
    return gson.fromJson(value, Price::class.java)
  }
}
```

<br>

assets 파일에서 값을 가져와 보여주는 예제


```kotlin
fun getProductList(): List<Product> {
  val inputStream = context.assets.open("product_list.json")
  val inputStreamReader = InputStreamReader(inputStream)
  val jsonString = inputStreamReader.readText()
  val type = object : TypeToken<List<Product>>() { }.type  // List 형태의 타입은 따로 구현해야함
  
  return GsonBuilder().create().fromJson(jsonString, type)
}
```
