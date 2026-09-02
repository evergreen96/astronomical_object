# StarSky remote content

이 디렉터리는 `evergreen96/astronomical_object` 저장소에서 앱 릴리스와 분리해 갱신하는
편집 콘텐츠의 배포 원본이다. 앱은
`manifest.json`을 하루에 한 번 확인하고, 더 높은 `contentVersion`이 있으면 팩을 받은 뒤
SHA-256과 스키마를 검증한다. 실패하면 마지막 정상 캐시 또는 앱 내장 팩을 계속 사용한다.

## 새 버전 배포

1. `packs/content-v1.json`을 새 버전 파일(예: `content-v2.json`)로 복사한다.
2. 팩의 `contentVersion`과 `publishedAt`을 올리고 카드를 편집한다.
3. UTF-8 파일의 SHA-256을 계산한다.
   - PowerShell: `Get-FileHash -Algorithm SHA256 content/packs/content-v2.json`
4. `manifest.json`의 `contentVersion`, `packUrl`, `sha256`, `publishedAt`을 같은 값으로 바꾼다.
5. 두 파일을 함께 검토한 뒤 `main` 브랜치에 반영한다.

현재 규격은 `schema/content-pack.schema.json`과 `schema/manifest.schema.json`에 있다.
이미지는 저장소의 `images/`에 추가한 뒤 `raw.githubusercontent.com` HTTPS 주소를 `image.url`에 넣는다.
이미지에는 원 출처, 라이선스, 크레디트를 반드시 기록하며 비영리 전용 라이선스와 GIF는
앱 검증에서 거부된다.

## 콘텐츠 원칙

- `zodiac_sign`: 열대황도 12궁을 `activeFromMonthDay`/`activeUntilMonthDay`로 정확히 한 개 선택한다. 실제 IAU 별자리 경계와 다른 체계임을 본문에 밝힌다.
- `monthly_sky`: `activeMonths`로 월별 북반구 중위도 추천 묶음을 고르고, 여러 `targets`를 각각 실제 하늘에서 열 수 있다.
- `sky_event`: `previewFrom`부터 `activeFrom` 전날까지는 예정 이벤트로, `activeFrom`/`activeUntil` 안에서는 진행 중 이벤트로 노출한다. 연도별 유성우·식·관측 캠페인은 새 팩에서 교체한다.
- `western_zodiac`: 점성술의 단정이나 운세가 아니라 실제 하늘과 문화사를 연결한다.
- `korean_sky`: 천상열차분야지도, 삼원, 이십팔수처럼 출처를 확인할 수 있는 내용을 쓴다.
- `russian_folk_sky`: 러시아계 민속 자료의 범위를 명시하고, 여러 민족의 전승을 러시아 전체의 단일 전통으로 일반화하지 않는다.
- 현재 위치·시각에 따라 변하는 오늘 밤 별자리의 고도·방위각은 JSON에 넣지 않는다.
  이 값은 Unity 천문 엔진이 자동 계산한다. 달은 식·합·공식 관측일처럼 이벤트와 연결될 때만 Today에 넣는다.
- `objectIds`와 `focusKind`를 함께 넣으면 카드에서 해당 대상을 실제 하늘로 열 수 있다.
- 여러 대상을 열 때는 `targets[]`에 `objectId`, `focusKind`, 현지화 라벨을 함께 넣는다.
- `sources`에는 카드의 사실을 뒷받침하는 HTTPS 원문 페이지를 기록한다.
