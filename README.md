# Namhyoung Kim — Personal Website

손으로 작성한 minimal Jekyll 사이트.
GitHub Pages에 그대로 배포됩니다 (`https://x7jeon8gi.github.io`).

---

## 디렉터리 구조

```
git-homepage/
├── _config.yml            # 사이트 메타·플러그인·내비게이션·소셜 링크
├── Gemfile                # Ruby gem 의존성 (github-pages 묶음)
├── .gitignore
│
├── _layouts/
│   ├── default.html       #  └ 모든 페이지 공통 (head + 상단 내비 + content + footer)
│   └── page.html          #  └ 하위 페이지용: 남색 배너(eyebrow·제목) + 본문
│
├── _includes/
│   ├── head.html          #  └ <head>, 글꼴(Source Serif 4 / Source Sans 3, 한글은 Noto Sans KR)
│   ├── header.html        #  └ 상단 내비 바 + EN/한국어 전환
│   ├── footer.html        #  └ 하단 연락처·링크
│   ├── lang-urls.html     #  └ 현재 언어와 영문·국문 짝 페이지 주소 계산
│   ├── publication.html   #  └ 논문·발표 한 건 렌더러 (번호형 / 홈 요약형)
│   ├── cv-entries.html    #  └ CV 항목 렌더러
│   └── pages/             #  └ 각 페이지 본문 (영문·국문 페이지가 같은 본문을 공유)
│
├── _data/                 # ← 실제 내용은 거의 다 여기 있음
│   ├── publications.yml   #  └ 논문·워킹페이퍼·국내외 발표 (가장 자주 손볼 파일)
│   ├── projects.yml       #  └ 진행 중 과제·연구비·산학과제 (Projects, CV 페이지가 같이 읽음)
│   ├── research.yml       #  └ 연구 분야 설명과 분야별 관련 논문·과제 id
│   ├── news.yml           #  └ 홈 News
│   ├── cv.yml             #  └ 학력·경력·특허·수상·강의·기술
│   └── i18n.yml           #  └ 메뉴와 화면 문구 (영문·국문)
│
├── index.html             # 홈 (소개 + News + Selected Publications)
├── research.html          # /research/      연구 분야
├── publications.html      # /publications/  학술지·학회 논문, 국내 학술지, 워킹페이퍼
├── presentations.html     # /presentations/ 국제·국내 학회 발표
├── projects.html          # /projects/      진행 중 과제, 연구비, 산학과제
├── cv.html                # /cv/            CV
├── ko/                    # 국문 페이지 (/ko/, /ko/research/ …), 파일마다 lang: ko
│
├── _cv/                   # 영문 PDF CV 소스 (XeLaTeX)
└── assets/
    ├── css/main.css       # 단일 스타일시트 (상단 :root 변수에 색·글꼴)
    ├── img/prof_pic.jpg   # 프로필 사진
    └── pdf/              # 다운로드용 영문 CV.pdf 및 국문 CV_ko.pdf
```

> 무언가 고치고 싶을 때 가장 먼저 여는 파일은 `_data/*.yml` 입니다.
> HTML/CSS는 거의 손 댈 일이 없어요.

---

## 1. 로컬에서 미리보기

### 1-1. Ruby 설치 (한 번만)

macOS:
```bash
# Homebrew가 없으면 먼저 https://brew.sh 에서 설치
brew install rbenv ruby-build
rbenv init                           # 출력대로 셸 설정에 한 줄 추가
rbenv install 3.2.4
rbenv global 3.2.4
ruby -v                              # → ruby 3.2.4 (...)
```

### 1-2. 의존성 설치 (한 번만, 이 폴더에서)

```bash
cd ~/홈페이지
gem install bundler
bundle install
```

### 1-3. 서버 실행

```bash
bundle exec jekyll serve --livereload
```

브라우저에서 <http://localhost:4000> 으로 접속.
파일을 저장하면 자동으로 새로고침됩니다.

---

## 2. GitHub Pages에 배포

### 2-1. 저장소 만들기 (한 번만)

1. <https://github.com/new> 접속
2. **Repository name**: `x7jeon8gi.github.io` (정확히 이 이름)
3. **Public** 으로 생성 (README/Gitignore 추가는 ❌)

### 2-2. 로컬 폴더를 그 저장소에 연결

이 폴더에서:

```bash
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/x7jeon8gi/x7jeon8gi.github.io.git
git push -u origin main
```

### 2-3. GitHub Pages 설정 확인

저장소 **Settings → Pages**:
- **Source**: `Deploy from a branch`
- **Branch**: `main` / `/ (root)`
- **Save**

1~2분 뒤 <https://x7jeon8gi.github.io> 에서 사이트가 떠요.
(Actions 탭에서 빌드 진행 상황을 확인할 수 있습니다.)

---

## 3. 자주 하게 될 작업

### 새 논문 추가

`_data/publications.yml` 의 알맞은 섹션 맨 위에 다음 형식으로 한 블록을 추가하면 끝:

```yaml
- id:          neurips26-mywork
  title:       "Some Awesome Paper Title"
  authors:     ["Namhyoung Kim", "Coauthor Name", "Jae Wook Song"]
  venue:       "Advances in Neural Information Processing Systems (NeurIPS)"
  venue_short: "NeurIPS '26"
  year:        2026
  note:        "Spotlight"
  selected:    true                # 홈페이지 'Selected Publications'에도 노출
  url:         "https://arxiv.org/abs/..."
  links:
    - { label: "PDF",  url: "https://..." }
    - { label: "Code", url: "https://github.com/x7jeon8gi/..." }
```

교신저자는 이름 뒤에 `*` 를 붙이고(`"Jae Wook Song*"`), FINX 페이지처럼 키워드를 달려면
`keywords: ["Vector quantization", "Return prediction"]` 를 추가. 번호([4], [3] …)는 자동으로 매겨짐.
학회 발표는 `international_talks` / `domestic_talks` 에 같은 형식으로 넣되 `year` 대신
`date: "2026.07"` 과 `location: "Pohang, Korea"` 를 씀.

저장 → `git commit -am "Add NeurIPS '26 paper" && git push` → 1~2분 뒤 자동 반영.

### 한국어 페이지

영문 페이지는 `/`, 국문 페이지는 `/ko/` 아래에 있고 상단 오른쪽 EN / 한국어 버튼으로 같은 페이지끼리 오감.
두 언어가 같은 데이터 파일을 읽으며, 국문 문구는 같은 항목에 `_ko` 를 붙인 필드로 넣음
(`title_ko`, `desc_ko`, `html_ko`, `authors_ko`, `venue_ko` 등). `_ko` 필드가 없으면 영문이 그대로 보임.
논문 제목은 원제 그대로 두고, 국내 학회 발표만 저자·학회명을 한글로 표기함.
메뉴와 섹션 제목 같은 화면 문구는 `_data/i18n.yml` 에서 고침.
새 소식을 올릴 때는 `html` 아래에 `html_ko` 한 줄을 같이 넣으면 됨.

### 과제 추가·종료

`_data/projects.yml` 의 `ongoing` 맨 위에 추가. 끝난 과제는 기간을 고치고
`grants`(정부 연구비) 또는 `industry`(산학) 로 옮기면 Projects 와 CV 페이지가 함께 바뀜.

### 연구 분야 페이지

`_data/research.yml` 에서 분야 설명(`desc`)과 관련 논문·과제 id 목록(`papers`, `projects`)을 수정.
id 는 publications.yml / projects.yml 의 `id` 와 같아야 링크가 걸림.

### 새 소식 추가

`_data/news.yml` 맨 위에 한 줄 추가:

```yaml
- date: "2026-05"
  html: "Talk at **CMU finance seminar** on vector-quantized factors."
```

### CV 항목 갱신

`_data/cv.yml` 의 해당 섹션을 직접 수정.
PDF 도 갱신했다면 `assets/pdf/CV.pdf` 도 새 파일로 덮어쓰기.
영문 CV 소스는 `_cv/cv_en.tex` (XeLaTeX/Tectonic, Erewhon 글꼴)이며,
필요한 글꼴은 `_cv/fonts/`에 포함되어 별도로 설치할 필요가 없음.
`_cv` 폴더에서 `tectonic cv_en.tex` 또는 `xelatex cv_en.tex`로 빌드한 PDF를
`assets/pdf/CV.pdf`로 복사하면 됨. macOS ChatGPT 앱에 포함된 Tectonic은
`/Applications/ChatGPT.app/Contents/Resources/tectonic/tectonic`에서 실행할 수 있음.
국문 CV 소스는 `_cv/cv_ko.tex`이며, 필요한 나눔명조 글꼴도 `_cv/fonts/`에 포함되어 있음.
같은 방식으로 빌드한 `cv_ko.pdf`를 `assets/pdf/CV_ko.pdf`로 복사하면 국문 페이지에서 다운로드할 수 있음.
각 언어의 CV 페이지에서 다른 언어의 PDF도 함께 다운로드할 수 있음.

### 프로필 사진 추가

정사각형 사진 (예: 800×800) 을 `assets/img/prof_pic.jpg` 로 저장 → push.
파일이 없으면 홈에 빈 상자가 표시됨.

### 자기소개 본문 수정

`index.html` 안의 `<div class="bio">…</div>` 부분을 직접 편집.
(짧은 본문이라 일반 HTML로 쓰는 게 더 통제하기 쉬워서 markdown 대신 HTML로 짰어요.)

### 색·폰트 변경

`assets/css/main.css` 상단 `:root { … }` 의 CSS 변수만 바꾸면 사이트 전체 톤이 변함.

---

## 4. 막히면 점검할 것

- **로컬 빌드 에러**: `Gemfile.lock` 지우고 `bundle install` 다시.
- **사이트가 한참 빈 페이지**: GitHub Pages 빌드 실패 가능. Actions 탭에서 로그 확인 → 보통 YAML 문법 오류 (들여쓰기·따옴표).
- **CSS가 안 적용됨**: 브라우저 강력 새로고침 (⌘⇧R / Ctrl+Shift+R).
- **새 글이 안 보임**: 푸시 후 Actions 빌드가 성공했는지 확인. 캐시 때문이면 시크릿 창에서 열어보기.

---

문제 생기면 막힌 단계와 에러 메시지만 알려주세요.
