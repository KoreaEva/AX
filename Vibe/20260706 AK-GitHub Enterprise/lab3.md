# 3교시 실습 — GitHub Enterprise Cloud: 하나의 테넌트에서 50명이 함께 일하기

__실습 목표__

약 50명 전원이 하나의 Enterprise 테넌트\(실습 조직\) 안에서 각자 Internal 저장소를 만들고, 팀·보호 규칙·보안 기능을 설정한 뒤 파트너와 서로 이슈·PR·리뷰를 주고받으며 Enterprise 협업을 체험한다\.

__소요 시간__

약 60분 \(Step 3\-1~3\-9\)

__사전 조건__

참가자 전원이 실습용 Enterprise 조직에 Member로 등록 완료 \+ 저장소 생성 권한 보유 \(초대 절차 없음\)\. 강사는 Enterprise/Organization Owner\(테넌트 전체 관리자\)\.

__최종 결과물__

조직 내 본인 Internal 저장소\(Ruleset \+ Code/Secret Scanning 적용\) \+ 파트너 PR 상호 리뷰·승인·머지 기록

__🎯 왜 하나요?  __이번 교시의 시나리오 — 지금까지\(1~2교시\)는 각자 자신의 저장소에서 혼자 작업했습니다\. 3교시부터는 참가자 약 50명 전원이 하나의 GitHub Enterprise 테넌트\(실습 조직\) 안에서 함께 일합니다\. 강사는 테넌트 전체 관리자\(Enterprise/Organization Owner\)이고, 여러분은 이미 조직에 Member로 등록되어 있으며 누구나 저장소를 만들 수 있습니다\. 이것이 실제 회사에서 GitHub Enterprise를 쓰는 모습과 동일한 구조입니다\. 모든 실습은 옆자리 파트너와 짝을 이뤄 서로의 저장소에 상호작용하며 진행합니다\.

__   Step 3\-1   실습 조직 접속 확인 & 파트너 정하기__

__🎯 왜 하나요?  __Enterprise는 회사 전체를 감싸는 계정 단위\(테넌트\)이고, 그 안의 Organization이 실제 협업 공간입니다\. 협업의 출발점은 "내가 어느 조직에 어떤 권한으로 들어와 있는가"를 확인하는 것입니다\. 또한 이번 교시의 모든 실습은 파트너와 서로의 저장소를 오가며 진행되므로 짝을 먼저 정합니다\.

1. github\.com에 로그인하고 우측 상단 프로필 아이콘 → Your organizations를 클릭한다\.
2. 강사가 안내한 실습용 조직을 클릭해 이동한다\.
3. People 탭을 클릭해 본인 계정이 Member 역할로 등록되어 있는지 확인한다\. \(참가자 약 50명이 모두 보인다\)
4. 옆자리 참가자와 2인 1조를 만들고, 서로의 GitHub 계정명\(ID\)을 교환해 메모한다\.

__💡 참고  __초대 메일을 기다릴 필요가 없습니다\. 실제 기업 환경에서도 SSO/SCIM 연동으로 입사와 동시에 조직에 자동 등록되며, 오늘 실습 환경이 바로 그 상태를 미리 만들어 둔 것입니다\.

__   Step 3\-2   조직 소유\(Internal\) 저장소 생성__

__🎯 왜 하나요?  __개인 계정의 저장소는 그 사람이 퇴사하면 접근과 관리가 끊기는 개인 자산입니다\. 조직 소유 저장소는 회사 자산으로 남아 조직의 보안 정책과 권한 관리가 일괄 적용됩니다\. 가시성 Internal은 "조직 구성원 모두에게 공개, 외부에는 비공개"라는 뜻으로, 50명이 서로의 작업을 보고 배울 수 있는 사내 오픈소스\(InnerSource\)의 기반이 됩니다\.

1. 조직 페이지에서 Repositories 탭 → New repository 버튼을 클릭한다\.
2. __Owner 드롭다운이 본인 개인 계정이 아니라 실습 조직으로 선택되어 있는지 반드시 확인한다\. __\(가장 많이 틀리는 부분\!\)
3. Repository name에 본인 ID를 포함한 이름을 입력한다\. \(예: ghec\-training\-본인ID — 50명의 저장소 이름이 겹치지 않게 하기 위함\)
4. 가시성에서 Internal을 선택한다\. \(Private: 나와 권한 받은 사람만 / Internal: 조직 구성원 전체 / Public: 인터넷 전체 공개\)
5. Add a README file을 체크하고 \.gitignore는 Python을 선택한 뒤 Create repository를 클릭한다\.
6. 저장소에서 Add file → Create new file을 클릭해 파일명 app\.py를 입력하고, 아래 코드를 붙여넣은 뒤 Commit changes로 저장한다\. \(뒤의 보안 스캐닝 실습에서 분석할 대상 코드\)

*app\.py — 보안 스캐닝 실습용 예시 코드*

from flask import Flask

app = Flask\(\_\_name\_\_\)

@app\.route\("/"\)

def index\(\):

    return "GitHub Enterprise Training\!"

if \_\_name\_\_ == "\_\_main\_\_":

    app\.run\(debug=True\)  \# 실습용: 보안 스캐닝 확인을 위한 debug 모드

__💡 참고  __app\.run\(debug=True\)는 운영 환경에서 원격 코드 실행 취약점이 될 수 있어 CodeQL이 탐지하는 대표 패턴입니다\. Step 3\-8에서 이 코드가 실제 보안 경고로 잡히는지 확인합니다\.

__   Step 3\-3   팀\(Team\) 생성과 권한 연결 — 파트너에게 내 저장소 권한 주기__

__🎯 왜 하나요?  __50명 규모에서 저장소마다 사람을 한 명씩 초대하면 권한 관리가 불가능해집니다\. Enterprise의 방식은 "팀에 권한을 주고, 사람을 팀에 넣는" 그룹 단위 권한 관리입니다\. 팀은 @조직명/팀명 형태의 멘션 대상, 코드 리뷰 지정 단위로도 활용됩니다\. 이 Step이 끝나면 파트너가 내 저장소에 push할 수 있는 상태가 됩니다 — 다음 Step에서 이 상태를 "통제"하는 방법을 배웁니다\.

1. 조직 페이지의 Teams 탭 → New team을 클릭한다\. \(한 조에 한 명만 생성\)
2. Team name에 두 사람의 ID를 조합한 이름을 입력한다\. \(예: team\-ID1\-ID2\)
3. Create team을 클릭한다\.
4. 팀 페이지의 Members 탭 → Add a member로 파트너를 검색해 추가한다\.
5. 같은 팀 페이지의 Repositories 탭 → Add repository로 본인 저장소와 파트너 저장소를 모두 연결한다\.
6. 두 저장소 모두 권한 수준을 Write로 설정한다\.

__💡 참고  __권한 수준의 의미: Read\(코드 열람\) < Triage\(이슈 관리\) < Write\(push 가능\) < Maintain\(설정 일부\) < Admin\(모든 설정\)\. 실무에서는 개발팀 Write, 테크리드 Maintain, 플랫폼팀 Admin처럼 계층적으로 부여하는 것이 표준입니다\.

__   Step 3\-4   Ruleset으로 main 브랜치 보호__

__🎯 왜 하나요?  __방금 파트너에게 Write 권한을 줬기 때문에, 지금은 파트너가 실수로든 고의로든 내 main 브랜치를 직접 바꿀 수 있는 상태입니다\. Ruleset은 "main의 변경은 반드시 PR \+ 승인 1건을 거쳐야 한다"는 규칙을 시스템으로 강제합니다\(4\-eyes 원칙\)\. 권한은 넓게 열어 협업을 빠르게 하되, 중요한 지점만 통제하는 것 — 이것이 Enterprise 협업의 핵심 원리입니다\.

1. 본인 저장소에서 Settings 탭을 클릭한다\.
2. 좌측 Code and automation 섹션의 Rules → Rulesets를 클릭한다\.
3. New ruleset → New branch ruleset을 클릭한다\.
4. Ruleset Name에 main\-protection을 입력한다\.
5. Enforcement status를 Active로 설정한다\. \(Active가 아니면 규칙이 만들어져도 동작하지 않음\)
6. Target branches에서 Add target → Include default branch를 선택해 main을 지정한다\.
7. Branch protections 섹션에서 Require a pull request before merging을 체크하고 Required approvals를 1로 설정한다\.
8. Block force pushes가 체크되어 있는지 확인한다\. \(커밋 이력을 강제로 덮어쓰는 것을 차단\)
9. 화면 하단 Create 버튼을 클릭한다\.

__💡 참고  __지금은 저장소 1개에 적용했지만, 관리자는 조직 수준 Ruleset으로 조직 안 모든 저장소에 같은 규칙을 한 번에 강제할 수 있습니다\. 저장소가 1,000개여도 정책은 한 곳에서 관리됩니다 — 교시 마지막에 강사가 시연합니다\.

__   Step 3\-5   보호 규칙 동작 체험 — main 직접 수정이 막히는지 확인__

__🎯 왜 하나요?  __정책은 "설정했다"로 끝나지 않고 "실제로 동작한다"까지 확인해야 합니다\. 차단을 직접 겪어 보면 왜 모든 변경이 PR을 거쳐야 하는지 몸으로 이해하게 되고, 실제 업무에서 이 화면을 만났을 때 당황하지 않게 됩니다\.

1. 본인 저장소 Code 탭에서 README\.md를 열고 연필\(Edit\) 아이콘을 클릭한다\.
2. 아무 내용이나 한 줄 추가하고 우측 상단 Commit changes\.\.\. 버튼을 클릭한다\.
3. "Commit directly to the main branch" 옵션이 차단\(선택 불가 또는 경고 표시\)되어 있는지 확인한다\. — Step 3\-4의 Ruleset이 동작하는 증거
4. "Create a new branch for this commit and start a pull request"를 선택하고 Propose changes를 클릭한다\.
5. 이 PR은 규칙 동작 확인이 목적이므로 Create pull request 대신 뒤로 가기로 취소하거나, 만들었다면 Close pull request로 닫아 정리한다\.

__   Step 3\-6   파트너 저장소에 이슈 생성 & 개선 PR 보내기 \(상호작용 ①\)__

__🎯 왜 하나요?  __Enterprise 협업의 본질은 "다른 사람의 저장소에 안전하게 기여하는 것"입니다\. 이슈는 "무엇이 필요한가"를 남기는 공식 기록이고, PR은 "이렇게 바꾸자"는 변경 제안입니다\. 같은 테넌트에 있고 팀으로 Write 권한을 받았기 때문에 fork 없이 브랜치만으로 바로 기여할 수 있습니다 — 사내 협업\(InnerSource\)이 외부 오픈소스 방식보다 빠른 이유입니다\.

1. 조직 Repositories 탭에서 파트너의 저장소를 검색해 이동한다\.
2. Issues 탭 → New issue를 클릭한다\.
3. 제목과 본문을 작성한다\. \(예: 제목 "README에 프로젝트 소개와 실행 방법 추가", 본문에 구체적인 요청 내용\)
4. 우측 Assignees에 파트너\(저장소 주인\)를 지정하고 Create를 클릭한 뒤, 이슈 번호를 기억한다\.
5. Code 탭으로 이동해 README\.md의 연필\(Edit\) 아이콘을 클릭하고 프로젝트 소개 문구를 추가한다\.
6. Commit changes\.\.\.를 클릭한다\. — main이 보호되어 있으므로 새 브랜치 생성 옵션이 자동 선택된다\. Propose changes를 클릭한다\.
7. PR 생성 화면 본문에 Closes \#이슈번호 를 입력해 이슈와 연결한다\.
8. 우측 Reviewers에서 파트너\(저장소 주인\)를 지정하고 Create pull request를 클릭한다\.

__💡 참고  __"Closes \#번호"는 PR이 머지되는 순간 연결된 이슈를 자동으로 닫습니다\. 요구\(이슈\)–변경\(PR\)–이력\(커밋\)이 자동으로 연결되어, 몇 달 뒤에도 "이 코드가 왜 바뀌었는지"를 추적할 수 있게 됩니다\.

__   Step 3\-7   서로의 PR 리뷰 → 승인 → 머지 \(상호작용 ②\)__

__🎯 왜 하나요?  __코드 리뷰는 결함을 조기에 잡는 품질 게이트이자 팀의 지식 공유 수단입니다\. Step 3\-4에서 만든 규칙\(승인 1건 필수\)이 실제로 머지를 막고 있다가 승인이 이뤄지는 순간 열리는 과정을 직접 확인합니다 — 정책\(Ruleset\)과 협업\(리뷰\)이 맞물려 돌아가는 지점입니다\.

1. 내 저장소의 Pull requests 탭에서 파트너가 보낸 PR을 연다\.
2. Files changed 탭에서 변경 내용을 확인한다\.
3. 의견을 남기고 싶은 라인에 마우스를 올려 파란 "\+" 아이콘을 클릭하고 코멘트를 작성한다\.
4. Conversation 탭으로 돌아가 Merge 버튼이 비활성\(Review required\) 상태인 것을 먼저 확인한다\. — 승인 없이는 누구도 머지할 수 없다
5. Files changed 탭 우측 상단 Review changes → Approve를 선택하고 Submit review를 클릭한다\.
6. Conversation 탭에서 Merge 버튼이 활성화된 것을 확인하고 Merge pull request → Confirm merge를 클릭한다\.
7. Delete branch로 브랜치를 정리하고, 연결된 이슈가 자동으로 Closed 되었는지 확인한다\.
8. 반대 방향\(내가 파트너 저장소에 보낸 PR\)도 파트너가 같은 방식으로 리뷰·승인·머지한다\.

__   Step 3\-8   GitHub Advanced Security 활성화 — 취약점·시크릿 자동 탐지__

__🎯 왜 하나요?  __사람의 리뷰만으로는 보안 취약점과 시크릿\(토큰·비밀번호\) 유출을 모두 잡을 수 없습니다\. Code scanning\(CodeQL\)은 코드의 의미를 분석해 취약한 패턴을 자동으로 찾아내고, Secret scanning은 저장소에 들어온 시크릿을 탐지합니다\. 실제 보안 사고의 상당수가 "커밋에 실려 들어간 시크릿"에서 시작되기 때문에, Enterprise에서는 이 기능을 조직 전체에 기본 적용하는 것이 표준입니다\.

1. 본인 저장소 Settings → 좌측 Security 섹션의 Advanced Security를 클릭한다\.
2. Code Security 항목의 CodeQL analysis 옆 Set up 버튼 → Default를 선택한다\.
3. 분석 언어가 Python으로 자동 인식되었는지 확인하고 Enable CodeQL을 클릭한다\. \(분석 워크플로우가 자동 실행되며 몇 분 소요 — 기다리는 동안 다음 항목 진행\)
4. 같은 화면에서 Secret Protection 옆 Enable 버튼을 클릭한다\. \(이미 켜져 있으면 생략\)
5. Push protection 옆 Enable 버튼을 클릭한다\. — Secret scanning이 "이미 들어온 시크릿을 찾아내는 것\(사후 탐지\)"이라면, Push protection은 "push 단계에서 아예 거부하는 것\(사전 차단\)"
6. 몇 분 뒤 저장소 Security 탭 → Code scanning alerts에서 Step 3\-2에 넣었던 debug=True 코드가 "Flask app is run in debug mode" 경고로 탐지되었는지 확인한다\.
7. 경고를 클릭해 취약점 설명을 읽고, Copilot Autofix가 수정안을 제안하는지 확인한다\. — 탐지에서 끝나지 않고 AI가 수정까지 제안하는 흐름

__   Step 3\-9   Push Protection 차단 체험 & 강사의 테넌트 관리자 화면 시연__

__🎯 왜 하나요?  __시크릿 유출 차단은 직접 겪어봐야 실제 상황에서 올바르게 대처할 수 있습니다\. 마지막으로 강사\(테넌트 관리자\) 화면을 통해, 50명이 오늘 만든 저장소·보안 경고·활동 기록이 관리자에게 어떻게 한눈에 보이는지 확인합니다 — 기업이 Enterprise를 도입하는 가장 큰 이유인 "전체 가시성과 통제"를 체감하는 시간입니다\.

1. 본인 저장소에서 README\.md를 편집해 실제 자격 증명처럼 보이는 더미 문자열을 한 줄 추가한다\. \(예: AKIA로 시작하는 20자 대문자·숫자 조합 — 실제 키는 절대 사용 금지\)
2. 새 브랜치로 커밋을 시도한다\. \(Commit changes\.\.\. → Propose changes\)
3. GitHub가 커밋/push를 차단하며 Secret detected 경고를 표시하는지 확인한다\.
4. 취소\(Cancel\)하고 추가했던 문자열을 지워 원래 상태로 되돌린다\.
5. __\(강사 시연\) __조직 Security 탭의 Security Overview — 50개 저장소의 Code/Secret scanning 경고 현황이 한 화면에 집계되는 모습
6. __\(강사 시연\) __조직 People/Teams — 오늘 만들어진 조별 팀과 권한 구조의 전체 현황
7. __\(강사 시연\) __조직 Settings → Audit log — 방금 참가자들이 수행한 저장소 생성·Ruleset 변경·머지 기록이 모두 남아 있는 모습 \(누가 언제 무엇을 했는지 추적 — 감사·컴플라이언스의 기반\)

__✓ 체크포인트  __조직 안에 본인 명의의 Internal 저장소가 있고, main 브랜치 Ruleset이 Active 상태이며, 파트너와 서로 PR을 승인·머지한 기록이 있어야 합니다\. 또한 Code scanning 경고\(Flask debug mode\) 확인과 Push protection 차단 체험을 각각 1회 이상 완료했는지 확인하세요\.