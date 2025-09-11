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

### 직렬화와 역직렬화

- 직렬화 (Serialization)

  객체를 바이트 단위의 연속적인 데이터(바이트 스트림)로 변환하는 작업

- 역직렬화 (Deserialization)

  바이트 스트림을 다시 원래 객체로 복원하는 작업


<img src="../README.assets/sd.png" alt="sd" align="center" width="60%" />

<br>

### 직렬화가 필요한 이유

- 객체를 네트워크로 전송하거나, 파일에 저장하거나, 서로 다른 프로세스 간 통신(IPC)을 할 때 객체의 상태를 그대로 보존하고 전달하기 위해 필요하다
- 메모리가 다른 컴포넌트(액티비티, 서비스 등) 간에 데이터를 전달할 때도 객체의 참조가 아닌 값을 전달해야 하므로 직렬화가 필수적이다

<br>

### Serializable vs Parcelable 개요

- Serializable (Java 표준)

  - `implements Serializable`만 선언하면 자동으로 직렬화 가능

  - 구현이 매우 간단하지만 Reflection 기반이라 성능이 느리다

  - 안드로이드에서는 성능 문제로 거의 사용하지 않는다

- Parcelable (Android 전용)

  - 안드로이드에 최적화된 직렬화 방식

  - 데이터를 Parcel 객체에 직접 쓰고 읽는 방식으로 동작

  - 성능이 좋아 안드로이드에서 권장되는 방식

<br>

### Serializable vs Parcelable 비교

| 항목        | Serializable                         | Parcelable                       |
| ----------- | ------------------------------------ | -------------------------------- |
| 제공        | Java 표준                            | Android 전용                     |
| 구현 난이도 | 간단 (인터페이스만 구현)             | `@Parcelize` 사용 시 간단        |
| 성능        | 느림 (Reflection 사용)               | 빠름 (안드로이드 OS 최적화)      |
| 데이터 크기 | 불필요한 데이터까지 직렬화될 수 있음 | 필요한 데이터만 기록 → 크기 작음 |
| 사용 빈도   | 거의 사용하지 않음                   | 실무에서 주로 사용됨             |
