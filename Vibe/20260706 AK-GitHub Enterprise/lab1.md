# 0\. 실습 가이드 사용법

이 문서는 4시간 커리큘럼의 각 교시를 실제로 진행할 때 참가자와 강사가 그대로 따라 할 수 있도록 화면 메뉴 경로와 클릭 순서를 Step 단위로 정리한 실습 매뉴얼입니다\. 1교시에서 만든 저장소와 코드를 2~4교시까지 계속 이어서 사용합니다\.

### 1교시

Codespaces 구성 및 Copilot으로 Python\(Flask\) To\-Do 앱 제작

저장소 생성 → Codespace 생성 → Copilot Chat/Agent로 앱 제작 → 실행 확인 → 커밋/푸시 \(Step 1\-1~1\-7\)

### 2교시

이슈 기반 브랜치 → PR → 코드 리뷰 → Merge

이슈 생성 → 브랜치 생성 → 기능 구현 → PR 생성 → 리뷰 → Merge \(Step 2\-1~2\-9\)

### 3교시

하나의 Enterprise 테넌트에서 50명이 함께: 조직 저장소·팀 권한·보호 규칙·보안 기능 \+ 파트너 상호 리뷰

조직 접속·짝 구성 → Internal 저장소 생성 → 팀 권한 연결 → Ruleset → 차단 체험 → 상호 이슈/PR → 상호 리뷰·머지 → GHAS → Push Protection·관리자 시연 \(Step 3\-1~3\-9\)

### 4교시

브라우저만으로 GitHub Actions 최소 체험 — 자동 실행·로그·수동 실행·PR 자동 검사\(CI\)

hello\.yml 작성 → PR 머지로 자동 실행 → 로그 읽기 → 수동 실행 → ci\.yml 최소 CI \(Step 4\-1~4\-5\)

## 실습 표기 규칙

- Step 번호\(예: Step 1\-3\)는 해당 교시 안에서 순서대로 진행하는 세부 절차입니다\.
- 각 Step 안의 번호 목록\(1, 2, 3…\)은 화면에서 클릭·입력하는 순서입니다\.
- 회색 음영 블록은 그대로 입력하거나 붙여넣을 코드/명령어입니다\.

__✓ 체크포인트  __각 교시 끝에는 정상적으로 완료되었는지 확인하는 체크포인트가 있습니다\. 다음 교시로 넘어가기 전에 반드시 확인하세요\.

## 사전 준비물 \(실습 시작 전 확인\)

- GitHub 계정 및 GitHub Copilot 라이선스 활성화 여부
- 최신 버전 브라우저 \(Chrome/Edge 권장\), 안정적인 인터넷 연결
- 3교시는 참가자 전원\(약 50명\)이 사전 등록된 실습용 Enterprise 테넌트\(조직\)에서 진행 — 본인 계정이 조직에 Member로 등록되어 있는지, 조직에서 저장소를 만들 수 있는지 사전 확인 \(별도 초대 절차 없음\)<br><br>


# 1교시 실습 — GitHub Codespaces 환경 구성 & GitHub Copilot

__실습 목표__

Codespace를 생성하고 GitHub Copilot Chat/Agent 모드로 Python\(Flask\) 기반 To\-Do 웹 앱을 만들어 GitHub에 push한다\.

__소요 시간__

약 60분 \(Step 1\-1~1\-7\)

__최종 결과물__

GitHub 저장소에 push된 Flask To\-Do 앱 \(app\.py, templates/index\.html, requirements\.txt\)

__   Step 1\-1   실습용 GitHub 저장소 생성__

1. 브라우저에서 github\.com에 접속해 로그인한다\.
2. 우측 상단의 “\+” 아이콘을 클릭하고 New repository를 선택한다\.
3. Repository name에 저장소 이름을 입력한다\. \(예: git\-training\-todo\-app\)
4. Public/Private 중 Private을 선택한다\. \(사내 실습 권장\)
5. Add a README file 체크박스를 선택한다\.
6. \.gitignore template 항목에서 Python을 선택한다\.
7. Create repository 버튼을 클릭해 저장소를 생성한다\.

__   Step 1\-2   GitHub Codespace 생성__

1. 방금 만든 저장소 메인 화면에서 녹색 Code 버튼을 클릭한다\.
2. 팝업 창에서 Codespaces 탭을 선택한다\.
3. Create codespace on main을 클릭한다\.
4. 추가 권한을 요청하는 화면이 나오면 Continue를 클릭한다\.
5. 머신 사양 선택 화면이 나오면 기본값\(2\-core\)을 유지한다\.
6. 환경이 준비될 때까지 1~2분 기다리면 브라우저 안에 VS Code 화면이 열린다\.

__💡 참고  __Codespace 생성이 오래 걸리면 우측 하단 알림\(Notification\) 아이콘에서 진행 상태를 확인할 수 있습니다\.

__   Step 1\-3   개발 환경 확인 및 Copilot 로그인__

1. 좌측 탐색기\(Explorer\) 패널에서 README\.md 등 저장소 파일이 보이는지 확인한다\.
2. 하단 상태 표시줄에서 GitHub Copilot 아이콘을 확인한다\.
3. 아이콘이 비활성\(회색\)이거나 로그인 요청 팝업이 뜨면 안내에 따라 Sign in to GitHub Copilot을 진행한다\.
4. 로그인 후 상태 표시줄의 Copilot 아이콘이 활성화\(흰색/파란색\)로 바뀌는지 확인한다\.

__   Step 1\-4   Copilot Chat/Agent 모드로 앱 초안 생성__

1. 단축키 Ctrl\+Alt\+I \(Windows/Linux\) 또는 Ctrl\+Cmd\+I \(Mac\)로 Copilot Chat 패널을 연다\.
2. 채팅 입력창 하단의 모드 드롭다운에서 Agent를 선택한다\.
3. 아래 예시 프롬프트를 입력하고 Enter를 누른다\.

*Copilot Chat 프롬프트 예시*

Flask로 간단한 To\-Do 웹 앱을 만들어줘\.

\- 할 일 추가, 완료 처리\(토글\), 삭제 기능이 있어야 해

\- 데이터는 데이터베이스 없이 메모리\(list\)에 저장해

\- app\.py, templates/index\.html, requirements\.txt 세 파일로 구성해줘

\- requirements\.txt에는 flask와 gunicorn을 포함해줘

1. Copilot이 제안하는 파일 생성/수정 내용을 검토한 뒤 Keep\(적용\) 버튼을 클릭한다\.
2. Agent가 pip install flask 등 터미널 명령 실행을 제안하면 Continue 또는 Run을 눌러 승인한다\.
3. 모든 파일 적용이 끝나면 좌측 탐색기에 app\.py, templates/index\.html, requirements\.txt가 생성되었는지 확인한다\.

__   Step 1\-5   코드 검토 및 자동완성으로 다듬기__

1. app\.py 파일을 열어 라우트\(route\)와 로직을 확인한다\.
2. templates/index\.html을 열어 화면 구성을 확인한다\.
3. 부족한 부분\(예: 완료 항목에 취소선 표시\)이 있으면 원하는 위치에 커서를 두고 주석이나 코드 일부를 입력해 Copilot 자동완성\(Ghost Text\)을 받아본다\.
4. 제안된 코드가 원하는 내용이면 Tab 키를 눌러 수락한다\.
5. requirements\.txt를 열어 flask와 gunicorn이 포함되어 있는지 확인하고, 없으면 직접 추가한다\.

*requirements\.txt 예시*

flask

gunicorn

__   Step 1\-6   로컬 실행 및 동작 확인__

1. 터미널을 연다\. \(단축키 Ctrl\+\`\)
2. 아래 명령으로 패키지를 설치한다\.

pip install \-r requirements\.txt

1. 아래 명령으로 서버를 실행한다\.

python app\.py

1. 하단 PORTS 탭을 클릭해 5000번\(또는 사용 중인\) 포트가 포워딩되었는지 확인한다\.
2. 포트 목록의 지구본 모양 아이콘을 클릭해 브라우저 미리보기를 연다\.
3. 화면에서 할 일 추가 → 완료 처리 → 삭제가 모두 정상 동작하는지 확인한다\.

__   Step 1\-7   변경 사항 커밋 및 GitHub로 push__

1. 터미널에서 실행 중인 서버를 Ctrl\+C로 종료한다\.
2. 좌측 활동 표시줄에서 Source Control\(분기 모양\) 아이콘을 클릭한다\.
3. 변경된 파일 목록 옆 “\+” 아이콘을 눌러 전체 변경 사항을 스테이징한다\.
4. 상단 메시지 입력창에 커밋 메시지를 입력한다\. \(예: Add initial Flask To\-Do app\)
5. 체크\(✓\) 모양의 Commit 버튼을 클릭한다\.
6. Sync Changes\(또는 Push\) 버튼을 클릭해 GitHub 저장소로 반영한다\.
7. github\.com의 저장소 페이지를 새로고침해 커밋이 반영되었는지 확인한다\.

__✓ 체크포인트  __GitHub 저장소\(main 브랜치\)에 app\.py, templates/index\.html, requirements\.txt가 모두 push되어 있고, 커밋 히스토리에 방금 작성한 커밋이 보여야 합니다\.