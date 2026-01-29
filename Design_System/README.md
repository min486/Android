<div align="center">
  <p>
    <img src="README.assets/android.png">
  </p>
  <br>
  <h2>Design System</h2>
  <p>디자인 시스템 관련 내용 정리</p>
  <br>
  <br>
</div>




## 🔥 Design System

### 개요

해당 디자인 시스템은 Jetpack Compose 기반 Android 앱에서 일관된 UI 표현과 확장 가능한 구조를 목표로 설계됨.

<br>

### 핵심 원칙

- 일관성 : 앱 전반에 걸쳐 통이된 디자인 언어 사용
- 확장성 : 디자인 변경 시 최소한의 코드 수정으로 전체 반영
- 협업 친화성 : 디자이너-개발자 간 명확한 스펙 공유

<br>

### Material 3 한계

Material 3의 기본 ColorScheme과 Typography는 아래와 같은 한계가 있다

- 의미 불일치 : primary, onSurface 등 Material의 네이밍이 실제 디자인 스펙과 매칭되지 않음
- 확장성 부족 : 새로운 컬러/타이포 추가 시 Material Theme 전체를 수정해야함
- 협업 어려움 : 디자이너가 전달한 스펙을 개발자가 Material 용어로 번역하는 과정에서 오류 발생

<br>

### 해결 방법 : CompositionLocal 기반 커스텀 시스템

Material Theme은 최소한의 브릿지 역할만 수행하고, 실제 디자인 시스템은 독립적으로 관리한다

```kotlin
@Composable
fun HambugTheme(
    ...
    content: @Composable () -> Unit
) {
    // CompositionLocal로 커스텀 시스템 주입
    CompositionLocalProvider(
        LocalHambugColors provides colors,
        LocalAppTypography provides typography
    ) {
        // Material Theme은 브릿지 역할만
        MaterialTheme(
            colorScheme = hambugLightColorScheme(colors),
            content = content
        )
    }
}
```

<br>

### CompositionLocal 장점

- 디자인 스펙과 1:1 매칭
- 전역 변경 용이
- 타입 안정성
- 다크모드 대응
