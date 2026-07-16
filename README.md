# AutoML Lab 홈페이지

경기대학교 AI컴퓨터공학부 AutoML Lab (서왕덕 교수) 공식 홈페이지.

- **콘텐츠 수정** (멤버·논문·과제·소식): [`UPDATE_GUIDE.md`](UPDATE_GUIDE.md) — 연구실 멤버는 이것만 보면 됩니다
- **배포·운영**: [`DEPLOY.md`](DEPLOY.md)

## 구조

```
├── index.html          # 페이지 뼈대
├── css/style.css       # 스타일
├── js/main.js          # 데이터 로드 + 렌더링 + 애니메이션
├── data/               # ★ 콘텐츠 (여기만 수정하면 됨)
│   ├── members.yml     #   멤버
│   ├── publications.yml#   논문·특허
│   ├── projects.yml    #   연구과제
│   └── news.yml        #   소식
└── images/members/     # 멤버 사진
```

빌드 과정 없음 — `data/*.yml` 수정 → commit → 즉시 반영.

## 로컬 실행

```bash
python3 -m http.server 8000
# http://localhost:8000
```
