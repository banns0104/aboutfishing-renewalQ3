# GitHub Pages 배포 가이드

## 배포할 파일 (이 2개만 있으면 됨)

```
index.html      ← 프로토타입 (메인 페이지)
layout.html     ← 전체 18 프레임 그리드 뷰
```

`프로토타입_v2.html`, `프로토타입_v2_layout.html`은 작업용 원본이라 배포 폴더에 안 올려도 됨. 필요하면 같이 올려도 무방 — 한글 파일명도 GitHub Pages에서 동작은 하지만 ASCII가 안전.

## 외부 의존성 (CDN — 추가 파일 업로드 불필요)

- Pretendard Variable: `cdn.jsdelivr.net`
- JetBrains Mono: `fonts.googleapis.com`
- Leaflet 1.9.4: `unpkg.com`
- Leaflet.markercluster 1.5.3: `unpkg.com`
- 지도 타일: `basemaps.cartocdn.com`

전부 CDN이라 별도 업로드 없음.

## 배포 절차

### 1) 새 레포 만들기
```bash
gh repo create aboutfishing-prototype --public
```
또는 GitHub 웹에서 새 레포 생성.

### 2) 파일 업로드 (둘 중 하나)

**A. CLI:**
```bash
git init
git add index.html layout.html
git commit -m "Initial prototype"
git branch -M main
git remote add origin https://github.com/<username>/aboutfishing-prototype.git
git push -u origin main
```

**B. 웹 드래그앤드롭:** 레포 페이지에서 `Add file → Upload files` → `index.html`과 `layout.html` 드롭 → Commit.

### 3) GitHub Pages 활성화

1. 레포 → `Settings` → `Pages`
2. `Source`: **Deploy from a branch**
3. `Branch`: **main**, **/ (root)**
4. `Save`

1-2분 후 `https://<username>.github.io/aboutfishing-prototype/` 에서 접근 가능.

## 404 방지 체크리스트

| 항목 | 처리됨 | 비고 |
|---|---|---|
| 파일명 ASCII | ✅ | `index.html` / `layout.html` |
| 내부 링크 ASCII | ✅ | `window.open('layout.html')`, `href="./"` |
| 대소문자 | ✅ | 모두 소문자 (GitHub Pages는 case-sensitive) |
| 절대경로 사용 안 함 | ✅ | 상대경로만 사용 |
| 외부 자산 HTTPS | ✅ | 모든 CDN https |
| 라우팅(Router) 사용 안 함 | ✅ | 단일 SPA-like, 새로고침해도 안전 |
| custom 404.html | 선택 | 필요시 추가 |

## (선택) 404 페이지

GitHub Pages는 `404.html`이 있으면 자동으로 404 응답에 사용. 안 만들어도 기본 404 페이지가 뜨지만, 깔끔하게 하려면:

```html
<!-- 404.html — 잘못된 경로 진입 시 메인으로 자동 이동 -->
<!DOCTYPE html>
<html lang="ko"><head>
<meta charset="UTF-8">
<title>어바웃피싱 — 페이지를 찾을 수 없음</title>
<meta http-equiv="refresh" content="0;url=./">
</head><body>
프로토타입으로 이동 중… <a href="./">바로 가기</a>
</body></html>
```

## 커스텀 도메인 (선택)

`Settings → Pages → Custom domain` 에서 도메인 설정 가능. CNAME 레코드를 `<username>.github.io` 로 가리키면 됨.

## 배포 후 확인 URL

- 메인: `https://<username>.github.io/<repo>/`
- 레이아웃: `https://<username>.github.io/<repo>/layout.html`
- "전체 화면 배치 보기" 버튼 클릭 → 새 탭에서 layout.html 열림
- 레이아웃에서 "← 프로토타입으로 돌아가기" → 메인으로 복귀

## 트러블슈팅

**Q. 한글 파일명을 굳이 쓰고 싶다면?**  
A. 동작은 하지만 일부 브라우저/공유링크에서 URL 인코딩(`%ED%94%84...`) 처리가 깨질 수 있음. ASCII 권장.

**Q. 지도가 안 뜬다면?**  
A. 브라우저 콘솔 확인 — Leaflet CDN 차단 (회사 방화벽 등) 또는 https 혼합 컨텐츠 이슈.

**Q. 폰트가 기본 sans-serif로 보인다면?**  
A. Pretendard CDN(`cdn.jsdelivr.net`)이 차단됐을 가능성. 폐쇄망이면 폰트 파일 직접 호스팅 필요.
