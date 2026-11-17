# cop-deploy

AI COP 제4회(2026.11.17) 배포 실습 프로젝트입니다.
외부 API, DB, 로그인, 외부 라이브러리 없이 정적 페이지 하나만 있습니다.

## 기능

- 제목 `AI COP 제4회 · 2026.11.17` 표시
- `배포 버전 v1` 표시
- 배경색을 바꾸는 버튼

## 구조

```
docs/index.html   # CSS/JS를 포함한 반응형 정적 페이지
server.mjs        # Node 표준 모듈만 쓰는 서버 (GET /, GET /index.html만 제공)
package.json      # scripts.start = node server.mjs
.gitignore
README.md
```

`server.mjs`는 `docs/index.html` 하나만 제공합니다. 그 외 경로(`/.env`, `/server.mjs` 등)는 모두 404입니다.

## 로컬 실행

Node.js 18 이상이 필요합니다. 설치할 의존성은 없습니다.

```bash
npm start                # http://localhost:3000
PORT=8080 npm start      # 포트 변경
```

`PORT` 환경변수가 없으면 3000번, 호스트는 `0.0.0.0`을 사용합니다.

## 검증

```bash
node --check server.mjs                                             # 문법 검사
npm start &                                                         # 서버 실행
curl -s -o /dev/null -w "%{http_code}\n" http://localhost:3000/            # 200
curl -s -o /dev/null -w "%{http_code}\n" http://localhost:3000/index.html  # 200
curl -s -o /dev/null -w "%{http_code}\n" http://localhost:3000/.env        # 404
curl -s -o /dev/null -w "%{http_code}\n" http://localhost:3000/server.mjs  # 404
```

확인이 끝나면 테스트 서버를 종료합니다(`kill %1` 또는 Ctrl+C).

## 배포 설정

### GitHub Pages

- Settings → Pages → Source: **Deploy from a branch**
- Branch: **main**, 폴더: **/docs**

### Vercel

- Root Directory: **docs**
- Framework Preset: **Other**
- Build Command: 없음 (비워 두거나 Override 후 빈 값)
- Output Directory: 기본값 (docs 폴더 그대로 제공)

### Railway

- Root Directory: **저장소 루트** (기본값)
- Start Command: **npm start**
- 포트는 Railway가 주입하는 `PORT` 환경변수를 자동으로 사용합니다.
