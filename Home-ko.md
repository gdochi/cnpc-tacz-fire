# TACZ NPC Fire 위키

[English](Home-en) | 한국어

`TACZ NPC Fire`는 CustomNPCs 스크립트에서 NPC에게 TACZ 총기 사격을 시키기 위한 브릿지입니다. 스크립트는 언제 사격을 시작하고 멈출지 결정하고, 모드는 TACZ 내부 사격/재장전/탄약/애니메이션 신호를 처리합니다.

## 빠른 시작

NPC가 실제로 들고 있는 메인핸드 TACZ 총기를 사용합니다. 예전 `setGun()` 방식은 레거시 호환용이며, 현재는 NPC 손에 총을 들려두는 방식이 기준입니다.

```javascript
var TACZ = Java.type("net.dochi.tacznpcfire.TaczNpcFireBridge")

/**
 * @param {NpcEvent.TargetEvent} e
 */
function target(e) {
    TACZ.start(e.npc, e.entity)
}

/**
 * @param {NpcEvent.TargetLostEvent} e
 */
function targetLost(e) {
    TACZ.stop(e.npc)
}
```

## 문서

| 문서 | 내용 |
|---|---|
| [스크립트 API](Script-API-ko) | 스크립트에서 호출할 수 있는 모든 `TACZ` 메서드 |
| [예제](Examples-ko) | 타겟 자동 사격, 상호작용 토글, 탄약, 디버그 예제 |
| [저장 데이터](Stored-Data-ko) | `npc.getStoreddata()`로 설정 가능한 키 |
| [문제 해결](Troubleshooting-ko) | 사격/재장전/애니메이션이 안 될 때 확인할 것 |

## 스크립트 작성 규칙

CustomNPCs 1.20.1 스크립트는 Nashorn 기반이므로 ES5 문법으로 작성하는 것을 권장합니다.

- `var` 사용
- `function` 선언 사용
- 화살표 함수, `let`, `const`, 템플릿 리터럴 사용 금지
- 이벤트 핸들러에는 JSDoc 타입 주석을 붙이면 편집기 자동완성이 좋아집니다

