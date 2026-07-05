# 2교시 실습 — GitHub 핵심 기능: 이슈 · 브랜치 · Pull Request

__실습 목표__

1교시 코드에 이슈 기반으로 기능을 추가하고, 브랜치·PR·코드 리뷰·Merge 흐름을 경험한다\.

__소요 시간__

약 60분 \(Step 2\-1~2\-9\)

__최종 결과물__

이슈에 연결된 PR이 리뷰를 거쳐 main 브랜치에 병합된 저장소

__   Step 2\-1   저장소 상태 확인__

1. 저장소 메인 페이지에서 Commits 링크를 클릭해 1교시 커밋 히스토리를 확인한다\.
2. Code 탭에서 app\.py 등 파일이 정상적으로 올라가 있는지 확인한다\.

__   Step 2\-2   개선 요청 이슈 생성__

1. 저장소 상단 탭에서 Issues를 클릭한다\.
2. New issue 버튼을 클릭한다\.
3. 제목을 입력한다\. \(예: 완료된 항목만 보는 필터 기능 추가\)
4. 본문에 요구 사항을 작성한다\. \(예: 완료 처리된 할 일만 모아 보여주는 버튼이 필요합니다\.\)
5. 우측 사이드바 Labels에서 enhancement를 선택한다\.
6. 우측 사이드바 Assignees에서 본인 계정을 지정한다\.
7. Create를 클릭한다\.

__   Step 2\-3   이슈에서 작업 브랜치 생성__

1. 방금 생성한 이슈의 상세 페이지를 연다\.
2. 우측 사이드바 Development 섹션에서 Create a branch를 클릭한다\.
3. 브랜치 이름은 기본 제안값을 그대로 사용한다\. \(이슈 번호가 포함된 이름\)
4. Create branch 버튼을 클릭한다\.

__   Step 2\-4   Codespace에서 새 브랜치로 전환__

1. 1교시에 사용하던 Codespace로 돌아간다\. \(github\.com 저장소 → Code → Codespaces 탭에서 기존 Codespace 클릭\)
2. 터미널에서 아래 명령으로 원격 브랜치 정보를 가져온다\.

git fetch origin

1. 아래 명령으로 2\-3에서 만든 브랜치로 전환한다\. \(브랜치명은 실제 생성된 이름으로 대체\)

git checkout <이슈\-브랜치\-이름>

__💡 참고  __하단 상태 표시줄의 브랜치명을 클릭해 목록에서 선택하는 방법도 동일하게 동작합니다\.

__   Step 2\-5   기능 구현__

1. Copilot Chat을 열고 Agent 모드에서 아래와 같이 요청한다\.

완료된 항목만 보여주는 필터 버튼을 index\.html과 app\.py에 추가해줘\.

필터를 끄면 전체 목록이 다시 보여야 해\.

1. 제안된 변경 사항을 검토하고 Keep으로 적용한다\.
2. 터미널에서 python app\.py로 실행해 필터 기능이 정상 동작하는지 확인한다\.

__   Step 2\-6   커밋 및 브랜치 push__

1. Source Control 패널에서 변경 파일을 스테이징한다\.
2. 커밋 메시지를 입력한다\. \(예: Add completed\-only filter\)
3. Commit 버튼을 클릭한다\.
4. Sync Changes\(또는 Publish Branch\) 버튼을 눌러 현재 브랜치를 GitHub로 push한다\.

__   Step 2\-7   Pull Request 생성__

1. GitHub 저장소 페이지에 나타나는 Compare & pull request 배너를 클릭한다\. \(배너가 없으면 Pull requests 탭 → New pull request\)
2. base: main ← compare: 방금 push한 브랜치인지 확인한다\.
3. 제목을 입력한다\.
4. 본문에 Closes \#이슈번호를 입력해 이슈와 자동 연결한다\. \(예: Closes \#3\)
5. 우측 Reviewers에서 리뷰어\(동료 또는 강사\)를 지정한다\.
6. Create pull request 버튼을 클릭한다\.

__   Step 2\-8   코드 리뷰 진행__

1. 리뷰어 계정으로 로그인해 해당 PR의 Files changed 탭으로 이동한다\.
2. 코멘트를 남길 코드 라인에 마우스를 올리고 파란 “\+” 아이콘을 클릭해 코멘트를 작성한다\.
3. 우측 상단 Review changes 버튼을 클릭한다\.
4. Comment, Approve, Request changes 중 하나를 선택하고 Submit review를 클릭한다\.
5. \(선택\) PR 우측 Reviewers 목록에 Copilot을 추가해 자동 코드 리뷰 코멘트를 받아본다\.
6. 요청받은 수정 사항이 있으면 작성자가 코드를 고쳐 추가 커밋을 push한다\. \(PR에 자동 반영됨\)

__   Step 2\-9   Merge 및 정리__

1. 필요한 승인\(Approve\)이 모두 완료된 것을 확인한다\.
2. Merge pull request 옆 드롭다운 화살표를 클릭한다\.
3. Squash and merge를 선택한다\.
4. 커밋 메시지를 확인하고 Confirm squash and merge를 클릭한다\.
5. 병합 완료 후 나타나는 Delete branch 버튼을 클릭해 브랜치를 정리한다\.
6. 연결된 이슈 페이지로 돌아가 상태가 Closed로 자동 변경되었는지 확인한다\.

__✓ 체크포인트  __이슈가 Closed 상태이고, main 브랜치에 필터 기능 커밋이 병합되어 있으며, 로컬 Codespace를 main으로 전환\(git checkout main; git pull\)했을 때 최신 코드가 반영되어야 합니다\.