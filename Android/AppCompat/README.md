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


## 🔥 AppCompat

### AppCompat 라이브러리

> 구 버전의 안드로이드 운영체제에서도 최신 안드로이드 기능을 사용할 수 있도록 지원하는 호환성 라이브러리

👉 안드로이드의 다양한 버전에 걸쳐 사용자에게 앱의 외관과 동작을 일관되게 제공하고,

최신 스타일과 기능을 오래된 안드로이드 버전에서도 사용할 수 있게 한다

<br>

### AppCompat 사용 예시

XML 레이아웃 파일에서 아래와 같이 태그를 추가한다

```xml
<androidx.appcompat.widget.AppCompatTextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>

<androidx.appcompat.widget.AppCompatImageView
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"/>
```

👉 `androidx.appcompat.widget.AppCompat~` 이런 식으로 뷰 컴포넌트를 사용할 수 있다

각 뷰 컴포넌트의 확장 버전으로, 안드로이드X (AppCompat)의 라이브러리

*뷰 컴포넌트 (ViewComponent) : 화면을 그리는 요소. ex) 글, 사진, 버튼 등

👉 안드로이드X는 더 나은 라이브러리 관리, 새로운 기능의 통합, 전반적인 앱 개발 경험을 개선한다
