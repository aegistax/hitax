# 하이택스(HI Tax) 설치·운영 안내

## 파일 구성
| 파일 | 역할 |
|---|---|
| index.html | 홈 + Q&A 목록 (검색·유형 필터) |
| ask.html | 질문 4단계 폼 (유형 선택 → 질문 → 약관 동의 → 이메일) |
| qna/001.html, 002.html | 답변 페이지 예시 (세무사 Q&A / AI 답변 검토) |
| about.html, terms.html, privacy.html | 서비스 안내, 이용약관, 개인정보처리방침 |
| style.css | 공통 디자인 |
| apps-script/Code.gs | 구글 시트 접수·알림·답변 회신 스크립트 (GitHub에는 올리지 않아도 됨) |
| posts.json | Q&A 목록 데이터 (시트에서 자동 갱신) |
| CNAME, robots.txt, sitemap.xml | 도메인 연결, 검색엔진용 (sitemap은 자동 갱신) |

## 1. 구글 시트 + Apps Script 연결
1. 구글 드라이브에서 새 스프레드시트 만들기 (이름 예: 하이택스_질문접수)
2. 확장 프로그램 → Apps Script → Code.gs 내용 전체 붙여넣기 → 저장
3. 시트를 새로고침하면 상단에 [하이택스] 메뉴가 생김 → [시트 초기 설정] 1회 실행 (권한 허용)
4. Apps Script 화면에서 배포 → 새 배포 → 유형: 웹 앱
   - 실행 사용자: 나 / 액세스 권한: 모든 사용자
5. 받은 웹앱 URL을 ask.html의 `const APPS_SCRIPT_URL = "";` 따옴표 안에 붙여넣기

## 2. GitHub Pages 배포 (고객 가이드와 같은 방식)
1. GitHub 계정 aegistax에 새 저장소 `hitax` (Public) 생성
2. apps-script 폴더를 뺀 나머지 파일 업로드
3. Settings → Pages → Branch: main / (root) 저장
4. 도메인 구입처 DNS 관리에서 레코드 추가 (hitax.io.kr)
   - A 레코드 4개: 호스트 비움(또는 @) → 185.199.108.153 / 185.199.109.153 / 185.199.110.153 / 185.199.111.153
   - CNAME 1개: 호스트 `www` → `aegistax.github.io`
5. Pages 설정에서 Custom domain이 hitax.io.kr로 잡히면 Enforce HTTPS 체크

## 3. GitHub 토큰 발급 (최초 1회)
1. GitHub 로그인 → 우측 상단 프로필 → Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token
2. Token name: hitax-sheet / Expiration: 1년(만료 전 재발급) / Repository access: Only select repositories → `hitax`
3. Permissions → Repository permissions → Contents: **Read and write**
4. 생성된 토큰(github_pat_...)을 복사 → 시트 메뉴 [하이택스 → GitHub 토큰 설정]에 붙여넣기
   - 토큰은 시트가 아니라 스크립트 속성에 저장되어 시트를 공유해도 보이지 않습니다.
   - 토큰은 비밀번호와 같으니 다른 곳에 붙여넣지 마세요.

## 4. 답변 운영 흐름
1. 새 질문 알림 메일 → 시트에서 질문 확인
2. 금색 헤더 칸을 채웁니다.
   | 칸 | 필수 | 내용 |
   |---|---|---|
   | 세무사 답변 | 필수 | 답변 본문. 빈 줄 = 문단, `## 소제목`, `**굵게**` 사용 가능 |
   | 검토판정 | AI 검토만 필수 | 맞음 / 일부 맞음 / 보완 필요 / 틀림 |
   | 근거 | 선택 | 예) 소득세법 제33조의2 |
   | 요약 | 선택 | 목록에 보일 1~2문장. 비우면 답변 앞부분 자동 사용 |
   | 공개제목·공개질문·공개AI답변 | 선택 | 개인정보를 지우거나 다듬은 공개용 문구. 비우면 원문이 공개됨 |
3. 행 선택 → 메뉴 **[하이택스 → ★ 공개 + 회신 한 번에]**
   - GitHub에 qna/번호.html 생성, posts.json(목록)·sitemap.xml 자동 갱신
   - 공개URL·페이지번호 자동 기록 → 질문자에게 답변 메일(페이지 링크 포함) 발송
   - 사이트 반영까지 1~2분 소요
4. 답변을 고친 뒤 다시 [공개]를 누르면 같은 번호 페이지가 수정됩니다.
5. 내리고 싶으면 [선택한 행 공개 취소] → 페이지 삭제, 목록·사이트맵에서 제거

※ 샘플 페이지(qna/001, 002)는 정식 오픈 전에 GitHub에서 삭제하고 posts.json을 `[]`로 비우세요.

## 5. 검색 등록
- 구글 서치콘솔, 네이버 서치어드바이저에 hitax.io.kr 등록 후 sitemap.xml 제출
