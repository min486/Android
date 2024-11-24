<div align="center">
  <p>
    <img src="../README.assets/android.png">
  </p>
  <br>
  <h2>기능</h2>
  <p>기능 관련 내용 정리</p>
  <br>
  <br>
</div>






## 🔥 알림

### OneSignal

무료 푸시 알림 서비스

안드로이드 플랫폼 지원

<br>

### 안드로이드 등록

안드로이드 플랫폼을 등록하기 위해서는 Firebase에 해당 프로젝트가 등록이 되어있어야 한다

➡️ Firebase에서 프로젝트 설정 ➡️ 서비스 계정 ➡️ [ 새 비공개 키 생성 ] 버튼 클릭해서 이용한다

<br>

### 앱 알림 설정으로 이동

사용자를 앱 알림 설정으로 이동시키기 위해 아래 코드 사용

👉 앱에서 설정 창이 나타나는게 아닌, 앱과 분리된 설정 창이 나타남

```kotlin
@Composable
fun PushButton() {
    val context = LocalContext.current
    
    Button(
        onClick = {
            val intent = Intent(Settings.ACTION_APPLICATION_DETAILS_SETTINGS).apply {
                data = Uri.parse("package:${context.packageName}")
                addFlags(Intent.FLAG_ACTIVITY_NEW_TASK)
            }
            context.startActivity(intent)
        }
    ) {
        Text("기기 알림 켜기")
    }
}
```
