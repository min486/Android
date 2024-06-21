<div align="center">
  <p>
    <img src="../../README.assets/jetpack-hero.png">
  </p>
  <br>
  <h2>Jetpack Compose</h2>
  <p>Jetpack Compose 관련 내용 정리</p>
  <br>
  <br>
</div>





## 🔥 Compose UI - Image

### Image 함수

<br>

### 함수 파라미터

- contentScale

<br>

### contentScale

- painter - 이미지 정보 (필수)
- contentDescription - 이 이미지가 나타내는 것을 설명하기 위해 접근성 서비스에서 사용하는 텍스트 (필수)
- modifier - 레이아웃 알고리즘을 조정하거나 데코레이션 콘텐츠를 그리는 데 사용되는 수정자
- alignment - 너비(width)와 높이(height)로 정의된 지정된 범위에 Painter를 배치하는 데 사용되는 선택적 정렬 매개변수
- contentScale - 경계가 Painter의 고유 크기와 다른 경우 사용할 가로, 세로스케일링을 결정하는 데 사용되는 선택적 스케일 매개변수
- alpha - 화면에 렌더링될 때 Painter에 적용되는 선택적 불투명도는 기본적으로 Painter를 완전히 불투명하게 렌더링
- colorFilter - 화면에 렌더링될 때 Painter에 적용할 선택적 colorFilter
