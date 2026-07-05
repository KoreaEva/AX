# 4교시 실습 — GitHub Actions 첫걸음: 최소 과정으로 자동화 체험

__실습 목표__

Azure 등 외부 클라우드 없이 브라우저만으로 GitHub Actions의 핵심\(이벤트 → 워크플로우 → 잡 → 스텝\)을 체험한다 — 첫 워크플로우 자동 실행, 로그 읽기, 버튼 수동 실행, PR 자동 검사\(최소 CI\)까지\.

__소요 시간__

약 60분 \(Step 4\-1~4\-5\)

__사전 조건__

3교시에서 만든 조직 저장소\(main 보호 Ruleset 적용 상태\)를 그대로 사용, 파트너와 짝 유지\. 별도 준비물 없음\.

__최종 결과물__

hello\.yml\(push 자동 실행 \+ 수동 실행\)과 ci\.yml\(PR 자동 검사\)이 동작하는 저장소, 성공한 워크플로우 실행 기록

__🎯 왜 하나요?  __이번 교시의 시나리오 — 4교시는 3교시에 만든 조직 저장소를 그대로 사용합니다\. Azure 같은 외부 클라우드나 별도 설치 없이, 브라우저만으로 GitHub Actions의 핵심인 "저장소에 일이 생기면 GitHub가 자동으로 명령을 실행한다"를 가장 짧은 경로로 체험합니다\. 순서는 세 가지: ① 가장 작은 워크플로우를 만들어 자동 실행을 보고 → ② 버튼으로 수동 실행해 보고 → ③ PR마다 코드를 자동 검사하는 최소 CI로 확장합니다\.

__   Step 4\-1   첫 워크플로우 파일 만들기 \(hello\.yml\)__

__🎯 왜 하나요?  __GitHub Actions는 "저장소에 어떤 일\(이벤트\)이 생기면 정해진 작업을 자동으로 실행"하는 GitHub 내장 자동화 도구입니다\. 왜 필요한가 — 테스트·빌드·배포 같은 반복 작업을 사람이 손으로 하면 느리고 실수가 생기기 때문입니다\. 반복 작업을 기계에 맡기는 것이 CI/CD의 출발점이고, 시작에 필요한 것은 서버가 아니라 \.github/workflows/ 폴더의 YAML 파일 하나입니다\.

1. 본인 저장소\(3교시에 만든 조직 저장소\)에서 Actions 탭을 클릭한다\.
2. set up a workflow yourself 링크를 클릭한다\. \(템플릿 목록 화면 상단에 있음\)
3. 파일명 입력칸의 main\.yml을 hello\.yml로 바꾼다\.
4. 편집기의 기본 내용을 모두 지우고 아래 코드를 붙여넣는다\. \(\# 뒤의 주석이 각 줄의 의미 — 꼭 함께 읽기\)

*\.github/workflows/hello\.yml — 가장 작은 완결 워크플로우*

name: Hello Actions        \# Actions 탭에 표시될 워크플로우 이름

on:                        \# "언제 실행할 것인가" = 이벤트

  push:

    branches: \[main\]       \# main에 커밋이 올라오면 자동 실행

  workflow\_dispatch:       \# 버튼 클릭으로 수동 실행도 허용

jobs:

  hello:                   \# 잡\(Job\) 이름

    runs\-on: ubuntu\-latest \# GitHub가 빌려주는 리눅스 가상머신\(러너\)

    steps:

      \- name: 인사 출력

        run: echo "Hello, GitHub Actions\!"

      \- name: 실행 환경 확인

        run: |

          date

          echo "이 명령은 내 컴퓨터가 아니라 GitHub 러너에서 실행됐습니다\."

__💡 참고  __runs\-on: ubuntu\-latest의 러너는 실행할 때마다 새로 만들어지고 끝나면 사라지는 일회용 가상머신입니다\. 그래서 "내 컴퓨터에서는 되는데…" 문제가 없는, 항상 깨끗한 실행 환경이 보장됩니다\. YAML은 들여쓰기\(스페이스 2칸\)가 문법이므로 붙여넣은 뒤 들여쓰기가 흐트러지지 않았는지 확인하세요\.

__   Step 4\-2   보호된 main에 PR로 반영 → 첫 자동 실행 확인__

__🎯 왜 하나요?  __3교시에서 main을 보호했기 때문에 워크플로우 파일도 PR을 거쳐야 들어갑니다\. 번거로워 보이지만 이것이 핵심 안전장치입니다 — 워크플로우는 저장소 안에서 명령을 실행하는 "권한 있는 코드"이므로, 자동화 설정의 변경조차 동료 리뷰를 거치게 하는 것이 Enterprise의 표준입니다\. 그리고 머지가 완료되는 순간이 곧 main에 push가 일어나는 순간 — 워크플로우가 처음으로 스스로 실행됩니다\.

1. 우측 상단 Commit changes\.\.\. 버튼을 클릭한다\.
2. 새 브랜치 생성 옵션이 자동 선택된 것을 확인하고 Propose changes를 클릭한다\.
3. Create pull request를 클릭하고, 우측 Reviewers에 파트너를 지정한다\.
4. 파트너가 3교시와 같은 방법\(Files changed → Review changes → Approve\)으로 승인하면 Merge pull request → Confirm merge를 클릭한다\.
5. __머지 직후 저장소 Actions 탭으로 이동한다\. __Hello Actions 실행이 자동으로 시작된 것을 확인한다\. \(머지 완료 = main에 push 이벤트 발생 = 워크플로우 자동 실행\)

__   Step 4\-3   실행 로그 읽기 — run → job → step 계층__

__🎯 왜 하나요?  __자동화는 "돌았다"로 끝나지 않습니다\. 무엇이 성공했고 어디서 왜 실패했는지 로그로 확인할 수 있어야 디버깅과 감사가 가능합니다\. Actions는 실행\(run\) 안에 잡\(job\), 잡 안에 스텝\(step\) 계층으로 모든 출력을 남깁니다 — 이 화면을 읽을 줄 아는 것이 Actions 활용의 절반입니다\.

1. Actions 탭에서 방금 실행된 Hello Actions 항목\(run\)을 클릭한다\.
2. 왼쪽의 hello 잡\(job\)을 클릭한다\.
3. "인사 출력" 스텝을 펼쳐 echo 메시지가 출력되었는지 확인한다\.
4. "실행 환경 확인" 스텝을 펼쳐 date 결과\(러너의 현재 시각\)를 확인한다\.
5. 상단의 초록 체크\(성공\)와 총 소요 시간을 확인한다\.

__💡 참고  __스텝이 실패하면 빨간 X가 표시되고 해당 로그가 자동으로 펼쳐집니다\. 우측 상단 Re\-run all jobs로 같은 실행을 다시 돌릴 수 있어, 일시적 오류인지 코드 문제인지 구분할 때 유용합니다\.

__   Step 4\-4   버튼으로 수동 실행 — workflow\_dispatch__

__🎯 왜 하나요?  __모든 자동화가 이벤트에만 반응하는 것은 아닙니다\. "필요할 때 사람이 버튼을 눌러 실행"하는 패턴 — 배포 개시, 데이터 갱신, 실패한 작업의 재시도 — 도 실무에서 매우 흔합니다\. hello\.yml에 넣어 둔 workflow\_dispatch 한 줄이 바로 그 실행 버튼을 만들어 줍니다\.

1. Actions 탭 왼쪽 목록에서 Hello Actions를 클릭한다\.
2. 오른쪽 상단에 나타난 Run workflow 버튼을 클릭한다\.
3. Branch가 main인지 확인하고 초록 Run workflow 버튼을 클릭한다\.
4. __몇 초 뒤 목록에 새 실행이 나타나면 열어서 로그를 확인한다\. __— 같은 워크플로우가 "push 때문"이 아니라 "내가 눌러서" 실행되었다는 점이 핵심

__   Step 4\-5   최소 CI — PR이 열릴 때마다 코드 자동 검사__

__🎯 왜 하나요?  __실무에서 Actions의 가장 흔한 용도는 CI\(지속적 통합\)입니다 — "코드가 들어올 때마다 기계가 먼저 검사한다"\. 3교시의 사람 리뷰 위에 기계 검사를 얹으면 이중 게이트가 완성됩니다\. 여기서는 app\.py가 문법 오류 없이 컴파일되는지 확인하는 가장 작은 CI를 만듭니다 — 실무에서는 이 자리가 단위 테스트·린트·빌드로 확장됩니다\.

1. Actions 탭 → New workflow → set up a workflow yourself를 클릭한다\.
2. 파일명을 ci\.yml로 바꾸고 아래 내용을 붙여넣는다\.

*\.github/workflows/ci\.yml — PR마다 자동 검사하는 최소 CI*

name: Python CI

on:

  pull\_request:            \# PR이 열리거나 갱신될 때마다 자동 검사

  workflow\_dispatch:

jobs:

  check:

    runs\-on: ubuntu\-latest

    steps:

      \- uses: actions/checkout@v4      \# 저장소 코드를 러너로 가져오는 공식 액션

      \- uses: actions/setup\-python@v5  \# 파이썬 설치 \(Marketplace 액션 재사용\)

        with:

          python\-version: "3\.12"

      \- name: 의존성 설치

        run: pip install flask

      \- name: 문법 검사

        run: python \-m py\_compile app\.py

1. Commit changes\.\.\. → Propose changes → Create pull request로 PR을 만든다\.
2. __PR 화면 하단 Checks 영역에 Python CI가 자동으로 나타나 실행되는 것을 확인한다\. __— 이 PR 자체가 pull\_request 이벤트이므로 방금 만든 CI가 즉시 동작
3. 검사가 초록 체크\(성공\)로 끝나면 파트너 Approve를 받아 Merge pull request → Confirm merge로 머지한다\. — 기계 검사 통과 \+ 사람 승인, 이중 게이트 통과

__💡 참고  __여기에 Ruleset의 "Require status checks to pass" 옵션을 결합하면 검사를 통과하지 못한 코드는 아예 머지가 불가능해집니다 — 품질 게이트의 완성형입니다\. \(시간이 되면 강사 시연\) 오늘 만든 hello/ci 워크플로우가 실무에서는 테스트 → 빌드 → 클라우드 배포\(CD\)로 확장됩니다\.

__✓ 체크포인트  __hello\.yml과 ci\.yml이 main 브랜치에 머지되어 있고, ① push로 자동 실행 1회 ② Run workflow 버튼으로 수동 실행 1회 ③ PR에서 Python CI 초록 체크 1회를 모두 확인했어야 합니다\. 이것으로 이벤트 → 워크플로우 → 잡 → 스텝의 전체 흐름을 직접 체험했습니다\.

# 부록

## A\. 실습 종료 후 정리

- 실습 저장소는 조직 자산으로 남겨 해커톤·팀 프로젝트 템플릿으로 재활용하거나, 정리가 필요하면 저장소 Settings 맨 아래 Danger Zone에서 삭제한다\.
- Codespace는 기본적으로 일정 시간 유휴 상태가 지속되면 자동으로 중지되며, github\.com → Your codespaces에서 수동으로 Stop 하거나 완전히 Delete할 수 있다\.
- 실습용 Organization/Team, 초대된 게스트 계정 등은 강사가 교육 종료 후 일괄 정리한다\.

## B\. 자주 발생하는 문제와 해결

__증상__

원인 / 해결 방법

__Codespace 생성이 느림__

머신 사양을 2\-core로 낮추거나, 잠시 후 새로고침하여 재시도한다\.

__Copilot 아이콘이 비활성화__

GitHub 계정에 Copilot 라이선스\(Individual/Business/Enterprise\)가 할당되어 있는지 조직 관리자에게 확인한다\.

__Actions 탭에 워크플로우가 보이지 않음__

YAML 파일이 \.github/workflows/ 폴더에 있는지, 확장자가 \.yml인지, main 브랜치에 머지되었는지 확인한다\. 경로나 확장자가 다르면 인식되지 않는다\.

__워크플로우 실행이 실패\(빨간 X\)로 끝남__

실패한 스텝의 로그를 펼쳐 에러 메시지를 확인한다\. 대부분 YAML 들여쓰기 오류\(스페이스 2칸 규칙\)이거나 파일 경로\(app\.py\) 오타이다\. 수정 커밋 후 자동 재실행되며, Re\-run all jobs로 수동 재실행도 가능하다\.

__Push protection 알림이 뜨지 않음__

저장소 Settings → Advanced Security에서 Secret Protection과 Push protection이 모두 Enable 상태인지 확인한다\.

__PR Merge 버튼이 비활성화__

Ruleset에서 설정한 필수 승인 수\(Required approvals\)를 아직 충족하지 못한 경우이다\. 리뷰어의 Approve를 받은 후 다시 시도한다\.