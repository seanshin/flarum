# Flarum 확장 가능 영역 정리

Flarum은 **Extender API**와 **이벤트**를 통해 백엔드·API·프론트엔드를 확장할 수 있습니다.  
`extend.php`에 Extender를 등록해 포럼을 커스터마이징합니다.

---

## 1. 확장 가능한 3개 레이어

| 레이어 | 기술 스택 | 설명 |
|--------|-----------|------|
| **Backend** | PHP, Laravel, Composer | API·비즈니스 로직·저장소·이벤트 |
| **Public API** | JSON:API | 클라이언트가 사용하는 REST API |
| **Frontend** | Mithril.js SPA | 포럼·관리자 UI |

---

## 2. 확장 방식 (2가지)

1. **Extender API** — 선언적으로 라우트·뷰·직렬화·정책 등을 추가/수정 (마이너 버전 호환 보장)
2. **이벤트(Event)** — 특정 시점에 실행할 리스너 등록

---

## 3. Extender 목록 (Flarum\Extend 네임스페이스)

### 3.1 API·컨트롤러·직렬화

| Extender | 용도 |
|----------|------|
| `ApiController` | API 컨트롤러 추가·수정 |
| `ApiSerializer` | JSON:API 리소스 직렬화기 확장 (속성·관계 추가) |
| `Filter` | API 쿼리 필터(검색·정렬 등) 추가 |
| `ThrottleApi` | API 요청 제한(쓰로틀) 설정 |

### 3.2 인증·권한·보안

| Extender | 용도 |
|----------|------|
| `Auth` | 인증 방식 추가 (예: 소셜 로그인) |
| `Policy` | 모델별 권한 정책(보기·생성·수정·삭제) 정의 |
| `Csrf` | CSRF 검사 동작 확장 |
| `Session` | 세션 설정·동작 확장 |

### 3.3 모델·가시성·URL

| Extender | 용도 |
|----------|------|
| `Model` | Eloquent 모델 수정 (관계·속성 등) |
| `ModelPrivate` | Discussion·CommentPost 등 “비공개” 모드 지원 |
| `ModelUrl` | 모델 ↔ URL 매핑 (라우팅) |
| `ModelVisibility` | 현재 사용자 기준 쿼리 스코핑(가시성) |

### 3.4 콘텐츠·포스트

| Extender | 용도 |
|----------|------|
| `Post` | 포스트 타입·컴포저·표시 방식 확장 |
| `Formatter` | 텍스트 포맷터(마크다운·BBCode 등) 확장 |
| `Validator` | 요청 검증 규칙 추가·수정 |

### 3.5 프론트엔드·UI

| Extender | 용도 |
|----------|------|
| `Frontend` | 포럼/관리자 프론트엔드에 JS·CSS·앱 데이터 주입 |
| `Theme` | 테마 색상·변수 등 확장 |
| `View` | Blade 뷰(서버 렌더 HTML) 등록 |
| `Locales` | 프론트엔드 다국어 리소스 추가 |
| `LanguagePack` | 백엔드/프론트엔드 언어팩 등록 |
| `Link` | 헤더 등에 링크 추가 |

### 3.6 인프라·미들웨어·시스템

| Extender | 용도 |
|----------|------|
| `Routes` | 웹·API 라우트 등록 |
| `Middleware` | HTTP 미들웨어 추가 |
| `Console` | CLI 명령어 등록 |
| `ServiceProvider` | Laravel 서비스 프로바이더 등록 |
| `Event` | 이벤트 리스너 등록 |
| `ErrorHandling` | 예외·에러 처리 방식 확장 |
| `Filesystem` | 파일시스템 디스크 설정 |
| `Mail` | 메일 드라이버·설정 확장 |
| `Settings` | 설정 스키마·기본값 확장 |
| `User` | 사용자 기본값·필드 등 확장 |
| `Notification` | 알림 채널·드라이버 확장 |
| `SimpleFlarumSearch` | 검색 엔진/동작 확장 |

### 3.7 조건부·커스터마이징

| Extender | 용도 |
|----------|------|
| `Conditional` | 특정 조건에서만 다른 Extender 적용 (예: 환경·설정에 따라) |

---

## 4. 확장 시 참고 사항

- **Extender 우선**: 가능하면 직접 코어 파일 수정 대신 Extender 사용 → 마이너 버전 간 호환성 유지.
- **커스텀 Extender**: `Flarum\Extend\ExtenderInterface` 구현 + 필요 시 `LifecycleInterface`로 활성화/비활성화 시 정리.
- **이벤트**: 백엔드 동작을 “특정 시점에 훅”으로 넣을 때는 [Backend Events](https://docs.flarum.org/extend/backend-events) 문서 참고.
- **프론트엔드 재사용**: 다른 확장에서 쓸 클래스/함수는 확장의 `index.js`에서 `export` (확장 간 확장·오버라이드 시 [Frontend 문서](https://docs.flarum.org/extend/frontend) 참고).

---

## 5. 사용 예시 (extend.php)

```php
<?php

use Flarum\Extend;

return [
    // 프론트엔드에 JS/CSS 추가
    (new Extend\Frontend('forum'))
        ->js(__DIR__.'/js/forum.js')
        ->css(__DIR__.'/less/forum.less'),

    // API 라우트 추가
    (new Extend\Routes('api'))
        ->get('/custom', 'custom.endpoint', Controllers\CustomController::class),

    // 이벤트 리스너
    (new Extend\Event())
        ->listen(SomeEvent::class, Listener\DoSomething::class),
];
```

---

## 6. 참고 링크

| 자료 | URL |
|------|-----|
| 확장 시작하기 | https://docs.flarum.org/extend/start |
| 확장성(Extensibility) | https://docs.flarum.org/extend/extensibility |
| API·데이터 흐름 | https://docs.flarum.org/extend/api |
| PHP Extend API 레퍼런스 | https://api.docs.flarum.org/php/master/flarum/extend |
| Extender 소스 (framework) | https://github.com/flarum/framework/tree/main/framework/core/src/Extend |
