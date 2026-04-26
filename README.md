# Namhyoung Kim — Personal Website

손으로 작성한 minimal Jekyll 사이트.
GitHub Pages에 그대로 배포됩니다 (`https://x7jeon8gi.github.io`).

---

## 디렉터리 구조

```
홈페이지/
├── _config.yml            # 사이트 메타·플러그인·내비게이션·소셜 링크
├── Gemfile                # Ruby gem 의존성 (github-pages 묶음)
├── .gitignore
├── .nojekyll              # 빌드된 폴더를 Jekyll이 다시 처리하지 않도록
│
├── _layouts/              # 페이지 뼈대
│   ├── default.html       #  └ 모든 페이지 공통 (head + header + content + footer)
│   └── page.html          #  └ default 위에 <h1> 한 줄을 더 얹는 일반 문서용
│
├── _includes/             # 부분(partial)
│   ├── head.html          #  └ <head> 태그
│   ├── header.html        #  └ 상단 내비
│   ├── footer.html        #  └ 하단 소셜 + 카피라이트
│   └── publication.html   #  └ 논문 한 건 렌더러
│
├── _data/                 # ← 실제 내용은 거의 다 여기 있음
│   ├── publications.yml   #  └ 모든 논문/발표 (가장 자주 손볼 파일)
│   ├── news.yml           #  └ 홈 News 섹션 5줄
│   └── cv.yml             #  └ CV 페이지 모든 항목
│
├── index.html             # 홈 (about + news + selected papers)
├── publications.html      # 전체 publications 페이지
├── cv.html                # CV 페이지
│
└── assets/
    ├── css/style.css      # 단일 스타일시트
    ├── img/prof_pic.jpg   # 프로필 사진 (없어도 동작)
    └── pdf/CV.pdf         # 다운로드용 CV
```

> 무언가 고치고 싶을 때 가장 먼저 여는 파일은 `_data/*.yml` 세 개입니다.
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

저장 → `git commit -am "Add NeurIPS '26 paper" && git push` → 1~2분 뒤 자동 반영.

### 새 소식 추가

`_data/news.yml` 맨 위에 한 줄 추가:

```yaml
- date: "2026-05"
  html: "Talk at **CMU finance seminar** on vector-quantized factors."
```

### CV 항목 갱신

`_data/cv.yml` 의 해당 섹션을 직접 수정.
PDF 도 갱신했다면 `assets/pdf/CV.pdf` 도 새 파일로 덮어쓰기.

### 프로필 사진 추가

정사각형 사진 (예: 800×800) 을 `assets/img/prof_pic.jpg` 로 저장 → push.
파일이 없으면 홈에 회색 동그라미 placeholder 가 표시돼요.

### 자기소개 본문 수정

`index.html` 안의 `<div class="bio">…</div>` 부분을 직접 편집.
(짧은 본문이라 일반 HTML로 쓰는 게 더 통제하기 쉬워서 markdown 대신 HTML로 짰어요.)

### 색·폰트 변경

`assets/css/style.css` 상단 `:root { … }` 의 CSS 변수만 바꾸면 사이트 전체 톤이 변함.

---

## 4. 막히면 점검할 것

- **로컬 빌드 에러**: `Gemfile.lock` 지우고 `bundle install` 다시.
- **사이트가 한참 빈 페이지**: GitHub Pages 빌드 실패 가능. Actions 탭에서 로그 확인 → 보통 YAML 문법 오류 (들여쓰기·따옴표).
- **CSS가 안 적용됨**: 브라우저 강력 새로고침 (⌘⇧R / Ctrl+Shift+R).
- **새 글이 안 보임**: 푸시 후 Actions 빌드가 성공했는지 확인. 캐시 때문이면 시크릿 창에서 열어보기.

---

문제 생기면 막힌 단계와 에러 메시지만 알려주세요.
