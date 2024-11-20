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

### 앱 알림 설정으로 이동

사용자를 앱 알림 설정으로 이동시키기 위해 아래 코드 사용

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
