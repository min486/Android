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
## 🔥 Photo picker

### Photo picker

> Android 시스템에서 제공하는 안전하고 표준화된 미디어 선택 도구

- 별도의 미디어 권한 요청 없이 사용자가 선택한 파일에만 접근할 수 있다

- 앱이 전체 갤러리에 접근할 필요가 없어 사용자 개인정보를 보호한다
- API 30 이상 프로젝트에서 photo picker만으로 모든 기기에 대응할 수 있다

<br>

### Contract 비교 (단일 vs 다중)

- PickVisualMedia
  - 선택 가능 개수 : 1개
  - 반환 타입 : `Uri?`
  - UI 동작 : 사진을 클릭 시 반환 및 닫힘
- PickMultipleVisualMedia
  - 선택 가능 개수 : 여러개
  - 반환 타입 : `List<Uri>`
  - UI 동작 : 사진마다 체크박스가 생기며, 추가 버튼 클릭 시 반환

<br>

### 미디어 유형 필터링

런처 실행(`launch`) 시 요청 객체를 통해 표시할 미디어의 종류를 결정한다

- `PickVisualMedia.ImageOnly` : 이미지만 표시
- `PickVisualMedia.VideoOnly` : 동영상만 표시
- `PickVisualMedia.ImageAndVideo` : 이미지 및 동영상 모두 표시

<br>

### 다중 이미지 선택 예시

무엇을 보여줄지는 `PickVisualMedia`로 결정

몇 개를 뽑을지는 `PickMultipleVisualMedia(maxItems)`로 결정

```kotlin
// Launcher 등록 (최대 선택 개수 제한)
val multiplePhotoPickerLauncher = rememberLauncherForActivityResult(
    contract = ActivityResultContracts.PickMultipleVisualMedia(5)
) { uris ->
    if (uris.isNotEmpty()) {
        boardWriteViewModel.onPhotoSelected(uris)
    }
}

// 버튼 클릭 시 호출
multiplePhotoPickerLauncher.launch(
    PickVisualMediaRequest(ActivityResultContracts.PickVisualMedia.ImageOnly)
)
```
