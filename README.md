# Formwheel AQS

Achievement · Quest · Secret를 통합하는 Formwheel 성장 허브입니다.

## 포함된 기능
- 모바일/PC 대시보드, XP/레벨 진행률
- 출석·탐험·시크릿 일일 퀘스트, 보상 수령
- 업적 자동 해금, 희귀도 표시
- 첫 Secret 퍼즐
- 브라우저 localStorage 오프라인 저장 및 Firebase Auth 로그인 상태에서 RTDB 동기화 시도

## 실행
GitHub Pages에서 루트 `index.html`을 배포합니다. 배포 여부는 저장소 Pages 설정에서 확인하세요.

## 프로젝트 연동
Formwheel의 다른 프로젝트가 같은 origin(예: semicolonxss.github.io)에서 실행된다면 방문 시 다음 이벤트를 기록할 수 있습니다.

```js
const key = 'formwheel.aqs.events.v1';
let events = [];
try { events = JSON.parse(localStorage.getItem(key) || '[]'); } catch {}
if (!Array.isArray(events)) events = [];
events.push({ type: 'project_visit', project: 'cook', time: Date.now() });
localStorage.setItem(key, JSON.stringify(events.slice(-100)));
```

AQS 화면을 열 때 큐를 읽고 기록을 반영합니다. 이 동작은 클라이언트 기반이며 **검증된 플레이 증빙은 아닙니다**.

## Firebase
AQS는 Firebase Authentication 세션이 존재하면 `aqs/v1/users/{uid}`에 동기화를 시도합니다.
보안 규칙은 반드시 읽기/쓰기 대상의 `auth.uid` 일치를 검사해야 합니다.
부모 노드에 공개 `.read: true` 또는 `.write: true`가 있다면 하위 규칙으로 취소할 수 없으므로 먼저 상위 규칙을 보호하세요.

## 다음 단계 (중요)
- 신뢰할 수 있는 서버 검증 도입 후 XP·업적을 서버에서만 지급
- 자정마다 프로젝트 탐험 카운트를 새로 세도록 퀘스트 정책 정리
- 여러 기기에서 병합 중복 XP 방지
- Hub 계정의 프로필 대표 배지와 연동
- Secret 정답이 공개 클라이언트 코드에 포함되지 않도록 서버에서 확인

**주의:** 현재 v1.0은 기능 체험용 MVP입니다. 로컬 기록과 브라우저에서 계산한 XP는 조작될 수 있으므로 경쟁 랭킹이나 교환 보상에 사용하지 마세요.
