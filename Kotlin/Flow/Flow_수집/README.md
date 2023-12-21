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


## 🔥 repeatOnLifecycle / flowWithLifecycle

### Android에서의 Flow 수집

Background에서 collect가 유지 된다면,

collect에서 어떠한 데이터를 받을시 Dialog를 띄운다고 하는 구문이 있다면 ➡️ 앱 크래쉬가 생긴다

(*collect 함수를 통해서 Flow에서 흐르는 데이터를 수집하게 된다)

👉 그래서 Lifecycle에 따라 효율적으로 수집을 유지하거나 닫을지 결정하는 메서드를 Android에서 제공한다

- repeatOnLifecycle
- flowWithLifecycle

<br>

### repeatOnLifecycle

coroutine Scope 내에서 repeatOnLifecycle 함수를 사용할 수 있습니다. 인자로 Lifecycle을 받아서 처리한다

repeatOnLifecycle은 여러 launch를 만들어 셋팅할 수 있도록 하는 용도

```kotlin
lifecycleScope.launch {
  	repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.dataList.collect {
          // 마실 물을 받음 
        }

        launch {
          // 샤워할 물을 받음
        }
        launch {
          // 흙탕물을 받음
        }      
   	}
}
```

<br>

### flowWithLifecycle

주로 collect를 하나만 사용할 때 사용

repeatOnLifecycle의 경우 다른 launch를 내부에서 만들수 있었지만,

flowWithLifecycle은 하나의 collect를 보일러플레이트 없이 작성할 수 있도록 돕는다

```kotlin
lifecycleScope.launch {
    exampleProvider.exampleFlow()
        .flowWithLifecycle(lifecycle, Lifecycle.State.STARTED)
        .collect {
            // Process the value.
        }
}
```

👉 Lifecycle이 최소 STARTED 상태일 때마다 실행되고 (흐름을 수집), 

Lifecycle이 STOPPED일 때 취소된다 (수집을 취소)

✅ UI가 화면에 표시될 때만 흐름 내보내기를 처리하여 리소스를 절약하고 앱 비정상 종료를 방지할 수 있다

