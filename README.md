# 음성폰비서 (Voice Phone Assistant)

폴더 A1 Pro처럼 구글 어시스턴트를 지원하지 않는 안드로이드 기기에서,
버튼 하나로 음성 명령을 받아 **전화 걸기 / 최근 문자 읽어주기**를 처리하는
초경량 커스텀 앱입니다.

## 동작 방식
1. 앱을 열고 화면의 마이크(🎤) 버튼을 누른다.
2. "OOO에게 전화" 라고 말하면 → 연락처에서 이름을 찾아 자동으로 전화를 건다.
3. "문자 확인" / "메시지 확인" 이라고 말하면 → 최근 문자 3건을 음성(TTS)으로 읽어준다.

버튼을 누르는 동작만 필요하고, 그 이후는 전부 음성으로 진행되므로
폴더폰처럼 화면이 작고 조작이 불편한 기기에서도 쓰기 쉽습니다.

## 포함된 파일
- `app/src/main/AndroidManifest.xml` — 필요한 권한 정의
- `app/src/main/java/com/example/voicephone/MainActivity.kt` — 핵심 로직
- `app/src/main/res/layout/activity_main.xml` — 화면 레이아웃

## 가장 쉬운 방법: GitHub Actions로 APK 자동 빌드
Android Studio를 설치하지 않고도 APK 파일을 받을 수 있습니다.

1. github.com에서 새 저장소(레포지토리)를 하나 만듭니다 (Public/Private 상관없음).
2. 이 폴더(`voicephone-app`) 전체 내용을 그 저장소에 업로드/푸시합니다.
   (GitHub 웹사이트의 "Add file → Upload files" 기능으로 폴더째 드래그해도 됩니다.)
3. 저장소의 **Actions** 탭으로 들어가면 `Build APK` 워크플로가 자동으로
   실행되는 걸 볼 수 있습니다. (`.github/workflows/build-apk.yml`에 이미
   설정되어 있어요.)
4. 빌드가 끝나면 해당 실행 결과 페이지 하단 **Artifacts** 항목에
   `voicephone-debug-apk`가 생깁니다. 클릭하면 zip으로 APK를 내려받을 수
   있어요.
5. 내려받은 zip을 풀면 `app-debug.apk`가 나옵니다. 이 파일을 폴더 A1 Pro로
   전송(카카오톡 나에게 보내기, USB 등)한 뒤 설치하면 됩니다.
   - 설치 시 "출처를 알 수 없는 앱" 허용을 물어보면 허용해주세요.

## Android Studio에서 프로젝트로 만드는 법
1. Android Studio → New Project → **Empty Views Activity** (Kotlin, minSdk 24 이상 추천)
   로 새 프로젝트를 만듭니다.
2. 생성된 프로젝트의 `AndroidManifest.xml`, `MainActivity.kt`, `activity_main.xml`
   내용을 여기 파일 내용으로 각각 덮어씁니다. (패키지명이 다르면
   `com.example.voicephone` 부분을 본인 패키지명에 맞게 바꿔주세요.)
3. `app/build.gradle`의 `dependencies`에 아래가 포함되어 있는지 확인:
   ```
   implementation 'androidx.appcompat:appcompat:1.6.1'
   ```
4. USB 디버깅으로 폴더 A1 Pro를 연결하거나, APK를 빌드해 기기에
   설치(사이드로드)합니다.
5. 최초 실행 시 마이크/전화/연락처/문자 권한을 모두 허용해야 정상 동작합니다.

## 알아둘 점
- 기본 `SpeechRecognizer`는 대부분 기기에서 **인터넷 연결(Wi-Fi/데이터)**이
  있어야 인식이 잘 됩니다. 폴더 A1 Pro는 LTE가 되니 데이터가 켜져 있으면
  문제없습니다.
- "전화" / "문자" 두 키워드만 우선 처리하도록 만들었습니다. 필요한 명령어
  (예: "카톡 읽어줘", "문자 보내줘")는 `handleCommand()` 함수에 조건을
  추가하는 방식으로 계속 확장할 수 있습니다.
- 화면을 안 보고 완전히 손대지 않고 쓰려면, 나중에 볼륨 버튼 길게 누르기 등
  하드웨어 키에 앱 실행을 매핑하는 것도 가능합니다(제조사 API 필요, 기종별로
  다름) — 필요하시면 이 부분도 이어서 도와드릴 수 있어요.
