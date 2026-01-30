# Flarum 한글화 가이드

포럼에 표시되는 메뉴, 버튼, 메시지 등을 한글로 보이게 하려면 **한국어 언어팩**을 설치한 뒤 기본 언어를 한국어로 설정하면 됩니다.

**Flarum 2.x 참고:** 현재 공식 한글 언어팩([flarum-lang/korean](https://github.com/flarum-lang/korean))은 `flarum/core: ^1.0`만 지원합니다. 2.x에서 한글화를 쓰려면 해당 패키지가 2.x를 지원할 때까지 대기하거나, [flarum-translations](https://rob006-software.github.io/flarum-translations/) / [Weblate](https://weblate.rob006.net/projects/flarum/)에서 2.x용 한국어 빌드가 나오면 그 안내를 따르세요.

---

## 1. 한글화 방식 요약

| 단계 | 내용 |
|------|------|
| 1 | 한국어 언어팩 확장 프로그램 설치 (Composer) |
| 2 | 관리자 > **확장 프로그램**에서 해당 확장 **활성화** |
| 3 | 관리자 > **기본 설정**에서 **기본 언어**를 **한국어**로 선택 |

---

## 2. Flarum 2.x에서 한글 언어팩 설치

### 2.1 공식 계열: flarum-lang/korean

- **패키지:** [flarum-lang/korean](https://github.com/flarum-lang/korean)
- **설치:** `composer require flarum-lang/korean`
- **호환성:** Packagist 기준으로는 `flarum/core: ^1.0`만 지원합니다.  
  Flarum 2.x를 쓰는 경우, 아래 중 하나가 필요합니다.
  - flarum-lang/korean 쪽에서 `^2.0` 지원이 나오면 그때 `composer require flarum-lang/korean` 사용
  - 또는 [flarum-translations](https://rob006-software.github.io/flarum-translations/) / [Weblate](https://weblate.rob006.net/projects/flarum/)에서 2.x용 한국어 번역이 포함된 버전이 배포되면 해당 안내에 따라 설치

### 2.2 2.x 호환 버전이 있을 때 설치 절차

1. 프로젝트 루트에서 한글 언어팩 추가:
   ```bash
   composer require flarum-lang/korean
   ```
   또는 `composer.json`의 `require`에 `"flarum-lang/korean": "*"` 를 넣은 뒤:
   ```bash
   composer update flarum-lang/korean
   ```
2. 캐시 삭제:
   ```bash
   php flarum cache:clear
   ```
3. 관리자 패널 > **확장 프로그램** → **한국어** 확장 **활성화**
4. 관리자 패널 > **기본 설정** → **기본 언어**를 **한국어**로 선택 후 저장

이후 포럼·관리자 화면에 표시되는 문구는 (번역이 있는 부분부터) 한글로 표시됩니다.

---

## 3. 표시되는 영역 (한글화 대상)

언어팩이 적용되면 아래와 같은 **기능 표시 영역**이 한글로 나옵니다.

### 3.1 포럼(일반 사용자)

- 상단/사이드 메뉴: 홈, 토론, 사용자, 설정, 로그아웃 등
- 토론 목록: 제목, 작성자, 답글 수, 마지막 활동 등
- 글쓰기/댓글: 버튼 문구, 플레이스홀더, 유효성 검사 메시지
- 알림, 프로필, 검색 관련 라벨·메시지

### 3.2 관리자

- **기본 설정:** 사이트 제목, 환영 메시지, 기본 언어 등
- **확장 프로그램:** 확장 이름·설명(번역이 있는 경우)
- **권한:** 그룹·권한 이름·설명
- **외관:** 테마·커스텀 CSS 등 라벨

### 3.3 확장 프로그램별

- flarum/approval, flarum/flags, flarum/likes, flarum/tags 등 **공식 확장**은 flarum-lang/korean에 번역이 있으면 한글로 표시됩니다.
- 서드파티 확장은 해당 확장이 한국어 번역을 포함하거나, Weblate 등에 한국어가 올라와 있어야 한글화됩니다.

---

## 4. 현재 버전(Flarum 2.x)에서의 제한

- **flarum-lang/korean**은 Packagist 기준으로 `flarum/core: ^1.0`만 요구합니다.  
  따라서 **Flarum 2.x**에서는 `composer require flarum-lang/korean` 시 버전 충돌로 설치가 안 될 수 있습니다.
- 2.x용 한글화를 쓰려면:
  - [flarum-lang/korean](https://github.com/flarum-lang/korean) 이슈/PR로 2.x 지원 요청, 또는
  - [flarum-translations](https://github.com/rob006-software/flarum-translations) / [Weblate](https://weblate.rob006.net/projects/flarum/)에서 2.x용 한국어 빌드가 나오면 그 패키지/안내를 따르면 됩니다.

---

## 5. 참고 링크

| 자료 | URL |
|------|-----|
| Flarum 언어 설정 (공식) | https://docs.flarum.org/2.x/languages |
| flarum-lang/korean | https://github.com/flarum-lang/korean |
| Flarum 번역 (Weblate) | https://weblate.rob006.net/projects/flarum/ |
| flarum-translations | https://rob006-software.github.io/flarum-translations/ |

이 문서는 **클론 프로젝트**에서 한글화를 적용할 때 참고용으로 사용하면 됩니다.
