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


## 🔥 Serializable / Parcelable

### 직렬화 & 역직렬화

- 직렬화 (serialization)

  : 객체를 바이트 단위의 연속적인 데이터(바이트 스트림)로 변경하는 작업

- 역직렬화 (deserialization)

  : 바이트 스트림을 원래 객체로 변환하는 작업

<img src="../README.assets/sd.png" alt="sd" align="center" width="60%" />

<br>

### 직렬화의 필요성

Serializable과 Parcelable은 모두 직렬화와 관련이 있다. 객체를 주고 받으려면 객체를 직렬화 해야 한다

👉 직렬화는 서로 다른 메모리 영역을 갖는 컴포넌트 간 객체를 주고 받을 때 사용한다

객체는 대부분 다른 객체를 가리키는 참조 필드를 갖고 있는데, 이 주소 값을 다른 메모리에서는 사용할 수가 없다

따라서, 이 참조 변수를 그것이 가리키는 실제 값으로 변환하는 작업이 필요하다

<br>

### Serializable과 Parcelable 

- Serializable
  - Serializable은 Java에서 제공하는 인터페이스로, 객체를 직렬화하여 전달하기 위해 사용된다
  - Serializable을 사용하면 객체를 바이트 스트림으로 변환하여 전달할 수 있다
  - Serializable 인터페이스를 구현한 객체는 Java에서 제공하는 ObjectOutputStream을 사용하여 전달할 수 있다
- Parcelable
  - Parcelable은 안드로이드에서 제공하는 인터페이스로, 객체를 전달하기 위해 사용된다
  - Parcelable을 사용하면 객체를 직렬화하여 안드로이드 OS에서 처리할 수 있는 바이트 배열로 변환하여 전달할 수 있다
  - Parcelable 인터페이스를 구현한 객체는 안드로이드 OS에서 Intent나 Bundle에 담아 전달할 수 있다

<br>

### Serializable과 Parcelable의 차이점

Serializabler과 Parcelable은 객체를 직렬화하여 전달하기 위한 방법이지만, 내부적으로 다음과 같은 차이점이 있다

- 속도
  - Parcelable은 Java의 Serializable보다 빠르다
  - Parcelable은 안드로이드 OS에서 직접 처리하기 때문에 직렬화와 역직렬화 시간이 빠르다
  - Serializable은 Java의 Reflection을 사용하기 때문에 직렬화와 역직렬화 시간이 상대적으로 느리다
- 크기
  - Parcelable은 Java의 Serializable보다 객체를 직렬화할 때 생성되는 데이터 크기가 작다
  - Parcelable은 객체의 멤버 변수들을 Parcel에 쓰기 때문에 필요한 데이터만 쓰게 된다
  - Serializable은 객체의 모든 데이터를 직렬화하기 때문에 불필요한 데이터까지 모두 쓰게 된다
- 안정성
  - Parcelable은 Java의 Serializable보다 안정성이 높다
  - Parcelable은 직렬화와 역직렬화에 대한 오류 검사를 수행하기 때문이다
