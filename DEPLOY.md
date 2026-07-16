# 🚀 배포 가이드 (관리자용)

빌드 과정이 없는 순수 정적 사이트입니다. 파일을 그대로 올리면 어디서든 동작합니다.

## 1. GitHub Pages (주력 배포)

### 최초 1회 설정

```bash
# 1) GitHub에서 새 저장소 생성 (예: automl-lab-homepage, Public)

# 2) 이 폴더를 push
cd /path/to/homepage
git remote add origin https://github.com/<계정>/<저장소>.git
git push -u origin main
```

3. GitHub 저장소 → **Settings → Pages**
4. **Source: Deploy from a branch**, Branch: `main`, 폴더: `/ (root)` → **Save**
5. 1~2분 후 `https://<계정>.github.io/<저장소>/` 에서 접속 확인

이후에는 **누구든 commit/push 하면 자동으로 반영**됩니다 (별도 작업 없음).

### 학생 권한 부여

- 저장소 → **Settings → Collaborators → Add people** 로 학생 GitHub 계정 초대
- 학생은 `UPDATE_GUIDE.md` 만 보면 됩니다

### (선택) 승인 게이트 — 학생 수정을 교수가 검토 후 반영하고 싶다면

- **Settings → Branches → Add branch protection rule**
  - Branch name pattern: `main`
  - ✅ Require a pull request before merging
- 이후 학생은 수정 시 자동으로 PR 생성 → 교수가 Merge 버튼으로 승인

## 2. 서버컴퓨터 배포 (병행)

같은 파일을 복사만 하면 됩니다. 두 가지 방법:

### 방법 A — git pull 자동 동기화 (권장)

```bash
# 서버에서 최초 1회
cd /var/www
git clone https://github.com/<계정>/<저장소>.git automl-lab

# nginx 예시 설정 (/etc/nginx/sites-available/automl-lab)
server {
    listen 80;
    server_name lab.example.ac.kr;      # 학교에서 받은 도메인
    root /var/www/automl-lab;
    index index.html;
}

# 5분마다 자동 동기화 (crontab -e)
*/5 * * * * cd /var/www/automl-lab && git pull --quiet
```

이러면 GitHub Pages와 서버컴이 항상 같은 내용을 보여줍니다.

### 방법 B — 수동 복사

```bash
rsync -av --delete --exclude='.git' /path/to/homepage/ user@server:/var/www/automl-lab/
```

## 3. 커스텀 도메인 (선택)

- GitHub Pages: Settings → Pages → Custom domain 에 도메인 입력, DNS에서 CNAME을 `<계정>.github.io` 로 설정
- 학교 도메인(`*.kyonggi.ac.kr`)은 학교 전산팀에 DNS 레코드 요청 필요

## 4. 로컬 테스트

```bash
cd homepage
python3 -m http.server 8000
# http://localhost:8000
```

> `file://` 로 index.html 을 직접 열면 데이터 로드(fetch)가 차단되어 빈 화면이 됩니다. 반드시 http.server 로 여세요.
