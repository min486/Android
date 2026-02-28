<div align="center">
  <p>
    <img src="../README.assets/android.png">
  </p>
  <br>
  <h2>CI/CD</h2>
  <p>CI/CD 관련 내용 정리</p>
  <br>
  <br>
</div>



## 🔥 CD (Continuous Deployment)

### CD 흐름

해당 프로젝트의 CD 파이프라인은 master 브랜치에 push될 때 자동으로 실행된다

```
master 브랜치 Push (또는 Tag Push)
         ↓
   코드 체크아웃
         ↓
   JDK 17 설정
         ↓
   Keystore 파일 복원 (Base64 디코딩)
         ↓
   signing.properties 생성
         ↓
   bundleRelease (서명된 AAB 빌드)
         ↓
   GitHub Release 생성 + AAB 업로드
```

#### <br>

### 보안 설정 (Secrets)

CD 파이프라인에서는 앱 서명을 위한 민감한 정보를 다룬다

모두 GitHub Repository Secrets에 등록해야 한다

- 등록 경로 :

  - GitHub 저장소 → Settings → Secrets and variables → Actions → New repository secret

| Secret 이름       | 용도                                     |
| ----------------- | ---------------------------------------- |
| KEYSTORE_BASE64   | Keystore 파일을 Base64로 인코딩한 문자열 |
| KEYSTORE_PASSWORD | Keystore 파일 비밀번호                   |
| KEY_ALIAS         | 키 별칭                                  |
| KEY_PASSWORD      | 키 비밀번호                              |

<br>

## CD 워크플로우 예시 코드

```yaml
name: Android CD Release

on:
  push:
    branches:
      - master

jobs:
  release:
    runs-on: ubuntu-latest

    env:
      KEYSTORE_BASE64: ${{ secrets.KEYSTORE_BASE64 }}
      KEYSTORE_PASSWORD: ${{ secrets.KEYSTORE_PASSWORD }}
      KEY_ALIAS: ${{ secrets.KEY_ALIAS }}
      KEY_PASSWORD: ${{ secrets.KEY_PASSWORD }}

    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Set up JDK 17
        uses: actions/setup-java@v4
        with:
          distribution: 'temurin'
          java-version: '17'

      - name: Make gradlew executable
        run: chmod +x ./gradlew

      - name: Decode Keystore File
        run: echo "$KEYSTORE_BASE64" | base64 -d > app/keystore.jks

      - name: Set up Signing Properties
        run: |
          echo "storeFile=keystore.jks" > ./signing.properties
          echo "storePassword=$KEYSTORE_PASSWORD" >> ./signing.properties
          echo "keyAlias=$KEY_ALIAS" >> ./signing.properties
          echo "keyPassword=$KEY_PASSWORD" >> ./signing.properties

      - name: Build Release AAB
        run: ./gradlew bundleRelease

      - name: Create GitHub Release
        uses: softprops/action-gh-release@v2
        if: startsWith(github.ref, 'refs/tags/')
        with:
          files: app/build/outputs/bundle/release/app-release.aab
          name: ${{ github.ref_name }}
```

<br>

### 각 단계 설명

#### 1~3. Checkout / JDK 설정 / gradlew 권한 부여

CI 워크플로우와 동일하다

#### 4. Decode Keystore File

```yaml
- name: Decode Keystore File
  run: echo "$KEYSTORE_BASE64" | base64 -d > app/keystore.jks
```

- Secret에 저장된 Base64 문자열을 디코딩하여 `app/keystore.jks` 파일로 복원한다

- 이 파일은 이후 `bundleRelease` 빌드 시 서명에 사용된다

- 동작 흐름 :

  Secret(Base64 문자열) → base64 -d 디코딩 → app/keystore.jks 파일 생성

#### 5. Set up Signing Properties

```yaml
- name: Set up Signing Properties
  run: |
    echo "storeFile=keystore.jks" > ./signing.properties
    echo "storePassword=$KEYSTORE_PASSWORD" >> ./signing.properties
    echo "keyAlias=$KEY_ALIAS" >> ./signing.properties
    echo "keyPassword=$KEY_PASSWORD" >> ./signing.properties
```

- Gradle 서명 설정에 필요한 `signing.properties` 파일을 생성한다
- Secret 값들을 파일로 기록하고, `build.gradle`에서 이 파일을 읽어 서명 설정을 적용한다

#### 6. Build Release AAB

```yaml
- name: Build Release AAB
  run: ./gradlew bundleRelease
```

- `bundleRelease` 태스크는 `signing.properties`의 서명 설정을 읽어 서명된 AAB 파일을 생성한다

#### 7. Create GitHub Release

```yaml
- name: Create GitHub Release
  uses: softprops/action-gh-release@v2
  if: startsWith(github.ref, 'refs/tags/')
  with:
    files: app/build/outputs/bundle/release/app-release.aab
    name: ${{ github.ref_name }}
```

- 빌드된 AAB를 GitHub Release에 업로드한다
- `if: startsWith(github.ref, 'refs/tags/')`의 역할
  - 이 조건이 없으면 master에 커밋할때마다 Release가 생성된다
  - `refs/tags/`로 시작하는 경우, 즉 Git Tag가 push될 때만 Release를 생성하도록 제한한다
