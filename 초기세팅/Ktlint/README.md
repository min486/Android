<div align="center">
  <p>
    <img src="../README.assets/studio.png">
  </p>
  <br>
  <h2>초기 세팅</h2>
  <p>앱 초기 세팅 관련 내용 정리</p>
  <br>
  <br>
</div>



## 🔥 Kotlin 코딩 컨벤션 (with. Ktlint)

> 프로젝트의 코드 품질과 스타일 일관성을 유지하기 위해 Ktlint를 도입할 수 있다
>
> Ktlint는 Kotlin 공식 스타일 가이드를 기반으로 코드 스타일 검사 및 자동 포맷팅을 지원하는 도구다

<br>

### Ktlint

- Kotlin 전용 정적 코드 분석 도구
- 코드 스타일을 강제해서 가독성 향상 및 팀 내 컨벤션 통일에 도움을 준다
- 불필요한 코드 스타일 논쟁을 줄여 코드리뷰 시간을 절약할 수 있다

<br>

## Ktlint 적용 방법

### 1. Gradle 플러그인 설정

`libs.versions.toml` 파일을 사용하는 경우 아래와 같이 설정한다

- `libs.versions.toml`

  ```toml
  [versions]
  ktlint = "12.3.0"
  
  [plugins]
  ktlint = { id = "org.jlleitschuh.gradle.ktlint", version.ref = "ktlint" }
  ```

- `settings.gradle.kts`

  ```kotlin
  pluginManagement {
      repositories {
          gradlePluginPortal()
          // ...
      }
  }
  ```

- `build.gradle.kts` (프로젝트 수준)

  ```kotlin
  plugins {
      alias(libs.plugins.ktlint) apply false
  }
  ```

- `build.gradle.kts` (앱 수준)

  ```kotlin
  plugins {
      alias(libs.plugins.ktlint)
  }
  
  ktlint {
      android.set(true)
      ignoreFailures.set(false)
  }
  ```

<br>

### 2. 주요 명령어

Ktlint를 사용하기 위해 2가지 Gradle 명령어를 활용할 수 있다

- 코드 검사

  ```bash
  ./gradlew ktlintCheck
  ```

  👉 코드 스타일 가이드 위반 사항을 검사한다

- 자동 포맷팅

  ```bash
  ./gradlew ktlintFormat
  ```

  👉 코드 스타일 가이드에 맞지 않는 부분을 자동으로 수정한다

<br>

### 3. editorconfig 파일을 통한 규칙 설정

`.editorconfig` 파일을 통해 규칙을 세부 조정할 수 있다

IDE 플러그인과 Gradle 플러그인 모두 이 규칙을 참고한다

```ini
root = true

[*]
# 기본적인 텍스트 편집기 설정
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true
charset = utf-8
indent_style = space
indent_size = 4

[*.kt]
# Kotlin 파일 전용 설정
max_line_length = 120
ktlint_standard_max-line-length = 120
ktlint_standard_no-semi = enabled
ktlint_standard_trailing-comma-on-call-site = disabled
ktlint_standard_trailing-comma-on-declaration-site = disabled
ktlint_standard_parameter-list-spacing = enabled
ktlint_standard_function-naming = disabled
ktlint_standard_no-wildcard-imports = disabled
ktlint_standard_multiline-expression-wrapping = disabled
ktlint_standard_string-template-indent = disabled
```

<br>

### 4. 협업 및 개발 가이드

- 코드 작성 중 : IDE 플러그인을 통해 실시간으로 스타일 검사
- 커밋 전 : 터미널에서 `./gradlew ktlintFormat` 명령어를 실행하여 자동 포맷팅
- CI/CD : CI/CD 환경에서 `./gradlew ktlintCheck` 명령어를 실행하여 전체 코드 스타일 검증
