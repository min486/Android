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


## 🔥 LiveData

### LiveData

> 안드로이드 Jetpack 구성요소 중 하나
>
> Data의 변경을 관찰할 수 있는 Data Holder 클래스

<br>

### Observer

- LiveData의 Data 관찰은 Observer가 한다

- 안드로이드에서 Observer는 데이터가 변경되는지 감시하고 있다가 데이터가 변경되면 UI 컨트롤러에게 알려주고,

  소식을 들은 UI 컨트롤러는 변경된 데이터를 가지고 UI를 업데이트한다

<br>

### LiveData 장점

- UI와 데이터 상태의 일치 보장

  - LiveData는 기본 데이터가 변경될 때 Observer 객체에게 알린다
  - 앱 데이터가 변경될 때마다 Observer가 대신 UI를 업데이트하므로 개발자가 업데이트할 필요가 없다

- 메모리 누수 없음

  - 관찰자는 Lifecycle 객체에 결합되어 있으며 연결된 수명 주기가 끝나면 자동으로 삭제된다

- 중지된 Activity로 인한 비정상 종료 없음

  - Activity가 BackStack에 있을 때를 비롯하여 Observer의 Lifecycle이 비활성화 상태에 있으면,

    Observer는 어떤 LiveData 이벤트도 받지 않는다

- 수명 주기를 더 이상 수동으로 처리하지 않음

  - UI 구성요소는 관련 데이터를 관찰하기만 할 뿐 관찰을 중지하거나 다시 시작하지 않습니다
  - LiveData는 관찰하는 동안 관련 수명 주기 상태의 변경을 인식하므로 이 모든 것을 자동으로 관리한다

- 최신 데이터 유지

  - 수명 주기가 비활성화되면 다시 활성화될 때 최신 데이터를 수신한다

    예를 들어 백그라운드에 있었던 활동은 포그라운드로 돌아온 직후 최신 데이터를 받는다

<br>

### LiveData, MutableLiveData 차이

- LiveData

  : LiveData에 할당된 value를 수정 불가능

  (setValue or postValue를 protected로 보호했기 때문에 외부에서 데이터 읽기만 가능)

- MutableLiveData

  : LiveData에 할당된 value를 수정 가능

  (외부에서 데이터 읽기 / 쓰기가 가능)

```kotlin
private val _doneEvent = MutableLiveData<Unit>()
val doneEvent : LiveData<Unit> = _doneEvent
```

<br>

### setValue, postValue 차이

LiveData의 값을 변경하게 해주는 함수가 setValue()와 postValue()

- setValue()

  : Main Thread에서 LiveData의 값을 변경

  👉 Main Thread에서 바로 값을 변경해주기 때문에,

  setValue() 함수를 호출한 뒤 바로 밑에서 getValue() 함수로 값을 읽어오면 변경된 값을 가져올 수 있다

- postValue()

  : 백그라운드에서 LiveData의 값을 변경

  👉 백그라운드 Thread에서 동작하다가 LiveData 값을 set 하고 싶을 때, 

  Main Thread에 값을 post 하는 방식으로 사용 (데이터를 Main Thread로 보낸다)

  ```kotlin
  fun insertData() {
    viewModelScope.launch(Dispatchers.IO) {
      contentRepository.insert(
      	...
      )
      _doneEvent.postValue(Unit)  // IO scope이기 때문에 Main Thread로 보내기 위해 postValue 사용
    }
  }
  ```
