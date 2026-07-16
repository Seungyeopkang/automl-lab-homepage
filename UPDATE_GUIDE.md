# 📝 홈페이지 업데이트 가이드 (연구실 멤버용)

> **여러분이 수정할 곳은 `data/` 폴더의 파일 4개뿐입니다.**
> HTML, CSS, JS는 건드릴 필요 없습니다. 파일을 수정하고 저장(commit)하면 1분 안에 홈페이지에 자동 반영됩니다.

| 파일 | 내용 |
|------|------|
| `data/members.yml` | 연구실 멤버 (신입생 들어오면 여기!) |
| `data/publications.yml` | 논문·특허 (논문 게재되면 여기!) |
| `data/projects.yml` | 연구 과제 |
| `data/news.yml` | 연구실 소식 (수상, 행사 등) |

---

## 1. GitHub 웹에서 수정하기 (git 몰라도 됩니다)

1. 브라우저에서 연구실 GitHub 저장소 접속
2. `data` 폴더 → 수정할 파일 클릭 (예: `members.yml`)
3. 오른쪽 위 **연필 아이콘(✏️ Edit)** 클릭
4. 내용 수정
5. 오른쪽 위 초록색 **Commit changes...** 버튼 클릭
6. 메시지 입력 (예: "신입생 홍길동 추가") → **Commit changes** 확정
7. 1분 정도 후 홈페이지 새로고침 → 반영 확인

---

## 2. 형식별 복붙 예시

### 새 멤버 추가 (`members.yml` 맨 아래에 붙여넣기)

```yaml
- name: 홍길동
  name_en: Gildong Hong
  role: 석사과정          # 교수 | 박사과정 | 석사과정 | 학부연구생 | 졸업생
  interests: [NAS, 모델 경량화]
  email: hong@kyonggi.ac.kr
  github: gildong          # GitHub 아이디 (없으면 이 줄 삭제)
  joined: 2026-03
```

사진을 넣고 싶으면 (선택):

```yaml
  photo: images/members/hong.jpg
```

### 새 논문 추가 (`publications.yml` 맨 위 주석 아래에 붙여넣기)

```yaml
- title: "논문 제목을 여기에"
  authors: "Gildong Hong, Wangduk Seo"    # 랩 멤버 이름은 자동으로 굵게 표시됨
  venue: IEEE Access                       # 저널/학회 이름
  year: 2026
  type: journal            # journal(국제저널) | conference(국제학회) | domestic(국내저널) | patent(특허)
  highlight: true          # 대표 실적이면 이 줄 유지, 아니면 삭제
  link: https://doi.org/...  # 논문 링크 (없으면 이 줄 삭제)
```

### 새 소식 추가 (`news.yml` 맨 위 주석 아래에 붙여넣기)

```yaml
- date: 2026-08
  category: paper          # paper(논문) | award(수상) | member(멤버) | etc(기타)
  text: "홍길동 학생 IEEE Access 논문 게재 🎉"
```

### 새 과제 추가 (`projects.yml` 맨 위 주석 아래에 붙여넣기)

```yaml
- title: "과제 이름"
  period: 2026.03 – 2029.02
  funder: 한국연구재단
  description: 한 줄 설명.
  status: 진행중           # 진행중 | 완료
```

---

## 3. 멤버 사진 올리기

1. GitHub 저장소에서 `images/members` 폴더 열기
2. **Add file → Upload files** 로 사진 업로드
   - 파일명은 영문으로 (예: `hong.jpg`)
   - **정방형(1:1), 400px 이상** 권장
3. `members.yml` 의 본인 항목에 `photo: images/members/hong.jpg` 추가

---

## 4. ⚠️ YAML 작성 규칙 (이것만 지키면 안 깨집니다)

- **들여쓰기는 스페이스 2칸.** 탭(Tab) 절대 금지
- **콜론(`:`) 뒤에는 반드시 스페이스 1칸**: `name: 홍길동` (O) / `name:홍길동` (X)
- 값에 콜론(`:`), 따옴표, `#` 이 들어가면 **전체를 큰따옴표로 감싸기**:
  `title: "ChainImputer: A Neural..."`
- 새 항목은 **`- name:`처럼 하이픈(`-`)으로 시작**

## 5. 🚨 문제가 생겼다면

**홈페이지 섹션에 노란 오류 박스가 떴을 때:**
- 박스에 표시된 **줄 번호 근처**를 확인하세요 (대부분 들여쓰기나 콜론 문제)
- 해결이 안 되면 GitHub에서 해당 파일의 **History** → 직전 버전 확인 후 본인 수정을 되돌리세요
- 그래도 안 되면 교수님 또는 홈페이지 담당 선배에게 연락

**수정했는데 반영이 안 될 때:**
- 1~2분 기다린 후 **강력 새로고침** (Windows: `Ctrl+Shift+R`, Mac: `Cmd+Shift+R`)
- GitHub 저장소 → Actions 탭에서 배포 진행 상태 확인 (초록 체크 = 완료)

## 6. (선택) 내 컴퓨터에서 미리보기

git 을 쓸 줄 안다면:

```bash
git clone <저장소 주소>
cd homepage
python3 -m http.server 8000
# 브라우저에서 http://localhost:8000 열기
```

파일을 수정하고 브라우저를 새로고침하면 바로 확인할 수 있습니다.
