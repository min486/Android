<div align="center">
  <p>
    <img src="../README.assets/studio.png">
  </p>
  <br>
  <h2>Android Studio</h2>
  <p>안드로이드 스튜디오 관련 내용 정리</p>
  <br>
  <br>
</div>


## 🔥 안드로이드 앱 아이콘 변경 및 디자인 가이드라인

> 이 문서는 안드로이드 스튜디오를 사용하여 앱 아이콘을 변경하는 과정과,
>
> 로고 디자인시 고려해야 할 안정 영역에 대한 가이드라인을 제공함

<br>

## 안드로이드 앱 아이콘 변경 과정

안드로이드 앱 아이콘을 변경하는 가장 권장되는 방법은

SVG 파일을 활용하여 Vector Asset Studio와 Image Asset Studio를 함께 사용하는 것이다

<br>

### 1. SVG 파일 준비 및 VectorDrawable 변환

- SVG 파일 준비

  앱 아이콘으로 사용할 SVG 파일 준비

- Vector Asset Studio 열기

  프로젝트 창에서 `res` 폴더 우클릭 → New → Vector Asset 선택

- SVG 파일 선택

  - Asset Type을 `Local file (SVG, PSD)`로 설정
  - Path 옆의 폴더 아이콘 클릭하여 준비한 SVG 파일 선택
  - Name은 ic_hambug와 같은 이름으로 지정
  - Size는 기본으로 표시되는 값으로 두기

- 변환 완료

  - Next 클릭하고, 확인 후 Finish 클릭

  - `res/drawable` 폴더 안에 SVG 데이터가 포함된 XML 파일 생성됨

    (`your_icon_name.xml`)

<br>

### 2. Image Asset Studio를 통한 런처 아이콘 생성

- Image Asset Studio 열기

  프로젝트 창에서 `res` 폴더 우클릭 → New → Image Asset 선택

- 아이콘 유형 설정

  Icon Type을 Launcher Icons (Adaptive and Legacy)로 설정

- Foreground 레이어 설정

  - Asset Type을 Image로 선택

<br>

### 앱 아이콘 변경 방법

Android Studio에서  `res` 폴더에서 `mipmap` 디렉토리를 찾는다

오른쪽 클릭 후 `New -> Image Asset` 으로 이동

<img src="../README.assets/icon.png" alt="sdk33" align="center" width="60%" />

<br>

Path 항목에서 다운받은 아이콘 이미지를 선택하고 (이미지 화질이 깨져서 SVG 사용)

Name 변경 후 Resize 통해 크기(위치)를 조정한다

<img src="../README.assets/icon2.png" alt="sdk33" align="center" width="70%" />

<br>

이후 왼쪽 패널에서 Project ➡️ app ➡️ manifest ➡️ AndroidManifest.xml 로 이동
