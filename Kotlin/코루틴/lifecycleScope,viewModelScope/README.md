<div align="center">
  <p>
    <img src="../../README.assets/kotlin-hero.png">
  </p>
  <br>
  <h2>Kotlin</h2>
  <p>코틀린 관련 내용 정리</p>
  <br>
  <br>
</div>


## 🔥 lifecycleScope / viewModelScope

### CoroutineScope

> 비동기를 실행하기 위한 작업 범위

비동기 작업을 해당 scope(범위)에서 실행한다

이는 무분별한 비동기 처리로 인한 메모리 누수와 리소스 낭비를 막기 위함

<br>

### CoroutineScope 종류

- lifecycleScope
- viewModelScope

- globalScope

  : 사용 X

<br>

### 생명주기에 안전한 코루틴

lifecycleScope, viewModelScope에서 실행하는 코루틴은 생명주기에 맞춰 안전하게 종료되므로 안전하다

- lifecycleScope, viewModelScope는 모두 각각 coroutineScope를 가지고 있고, 
- lifecycleScope는 activity나 fragment의 lifecycle에 맞게, 
- viewModelScope는 viewModel이 clear되는 시점에 맞게 각각의 couroutine들을 cancel() 해준다

👉 개발자가 신경써서 coroutine을 취소해주지 않아도 된다

<br>

### lifecycleScope

> Lifecycle이 존재하는 Activity or Fragment의 수명주기에 따라 자동으로 코루틴을 시작하고 취소할 수 있는 scope

- 일반적은 코루틴 스코프와 마찬가지로 launch 함수 호출을 통해 suspendable한 작업을 할 수 있다

  ```kotlin
  lifecycleScope.launch {
      // 코루틴 작업
  }
  ```

- 만약 Fragment 또는 Activity와 같은 일반적인 코루틴 스코프를 만들어 작업중이라면

  LifecycleOwner가 Destoryed 될 때,

  실행중인 코루틴을 취소하기 위해 명시적으로 CoroutineContext.cancel()을 호출해줘야 한다

👉 하지만, lifecycleScope에서 실행하는 코루틴은 생명주기에 맞춰 종료되므로 안전하다

<br>

### viewModelScope

> MVVM을 작업할 때 ViewModel에서 코루틴이 필요할 때 사용하는 scope

- ViewModelScope는 앱의 각 ViewModel을 대상으로 한다
- 이 범위에서 시작된 모든 코루틴은 ViewModel이 삭제되면 자동으로 취소된다
- 코루틴은 ViewModel이 활성 상태인 경우에만 실행해야 할 작업이 있을 때 유용하다

원래는 ViewModel이 종료되면 코루틴 scope도 함께 종료하는 것을 따로 명시해줘야 한다

```kotlin
class MainActivityViewModel : ViewModel() {
  	private val myJob = Job()
  	private val myScope = CoroutineScope(Dispatchers.IO + myJob)
  
  	fun getUserData()	{
      	myScope.launch {
          	. . .
        }
    }

    override fun onCleared() {
        super.onCleared()
        myJob.cancel()
    }
}
```

하지만 viewModelScope를 사용하면 이를 간단하게 구현할 수 있다

```kotlin
class MainActivityViewModel : ViewModel() {
  	private var userRepository = UserRepository()
  	var users: MutableLiveData<List<User>> = MutableLiveData()
  
  	fun getUserData() {
      	viewModelScope.launch {
          	. . .
        }
    }
}
```
