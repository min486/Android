<div align="center">
  <p>
    <img src="../README.assets/android.png">
  </p>
  <br>
  <h2>Etc</h2>
  <p>기타 참고 내용 정리</p>
  <br>
  <br>
</div>



## 🔥 GitHub을 활용한 이미지 링크 생성

GitHub Repository를 활용해서 이미지 링크 생성이 가능하다

<br>

### 1. Repository 생성

- GitHub에서 New 버튼을 클릭하여 새 저장소 생성
- Repository name : certificates (또는 원하는 이름)
- Public 설정 필수
- Create repository 클릭

<br>

### 2. 이미지 업로드

웹에서 직접 업로드하는 방법 사용함

```
certificates/
  ├── image1.png
  ├── image2.jpg
  └── image3.png
```

- 방법 : Add file → Upload files → 이미지 드래그 & 드롭
- 브랜치 : main 혹은 master 브랜치에 업로드

<br>

### 3. Raw 링크 추출

이미지를 업로드한 후, 외부에서 사용하려면 Raw URL이 필요하다

- 방법 : 주소창의 `/blob/` 부분을 `/raw/`로 변경
  - 기존 URL : https://github.com/min486/certificates/blob/master/test.png
- 변경하면 아래 링크로 이동되고, 검은 배경에 이미지만 깔끔하게 보인다
  - Raw URL : https://raw.githubusercontent.com/min486/certificates/master/test.png

<br>

### 주의사항

- Repository가 Public 이어야 링크가 작동한다
- Private으로 바꾸면 링크 접근 불가
