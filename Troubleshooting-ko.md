# 문제 해결

[English](Troubleshooting-en) | 한국어

## NPC가 사격하지 않음

확인할 것:

- NPC 메인핸드에 TACZ 총기가 있는지 확인합니다.
- `TACZ.getGunInfo(e.npc)`의 `ok`가 `true`인지 확인합니다.
- `TACZ.setEnabled(e.npc, true)` 또는 저장 데이터 `tacznpcfire.enabled = 1`인지 확인합니다.
- 타겟 이벤트에서 `TACZ.start(e.npc, e.entity)`를 호출하는지 확인합니다.
- 거리가 `maxDistance`보다 멀지 않은지 확인합니다.

진단 예:

```javascript
var TACZ = Java.type("net.dochi.tacznpcfire.TaczNpcFireBridge")

/**
 * @param {NpcEvent.InteractEvent} e
 */
function interact(e) {
    var info = TACZ.getGunInfo(e.npc)
    e.npc.say("ok=" + info.get("ok") + " reason=" + info.get("reason"))
    e.npc.say("gun=" + info.get("gunId") + " rpm=" + info.get("rpm"))
}
```

## 총기 RPM이 원하는 대로 안 나옴

현재 배율 방식은 사용하지 않습니다.

- 총기 기본 RPM 사용: `TACZ.setRpm(e.npc, 0)`
- 고정 RPM 사용: `TACZ.setRpm(e.npc, 600)`
- 무작위 범위 사용: `TACZ.setRpmRange(e.npc, 400, 700)`

GUI의 사격 탭은 NPC가 현재 들고 있는 총기의 기본 RPM을 참고값으로 보여줍니다.

## 재장전 중인데 사격함

최신 빌드에서는 재장전 시작과 종료 시간이 보장되며, 재장전 잠금 중에는 사격하지 않습니다.

확인할 것:

- 새 JAR가 `mods/tacznpcfire-0.1.0.jar`에 덮어씌워졌는지 확인합니다.
- `TACZ.setReload(e.npc, true)` 또는 저장 데이터 `tacznpcfire.reload = 1`인지 확인합니다.
- 탄약 보급형이면 `TACZ.setSupplyAmmo(e.npc, true)`와 충분한 `ammoStock`이 있는지 확인합니다.
- `reloadDurationMs`가 너무 짧지 않은지 확인합니다.

## 재장전 애니메이션이 안 보임

모드는 재장전 시작 시 클라이언트에 재장전 신호를 보냅니다. 그래도 애니메이션이 안 보이면 아래를 확인합니다.

- 클라이언트와 서버 양쪽에 같은 JAR가 있는지 확인합니다.
- NPC가 실제로 TACZ 총기를 들고 있는지 확인합니다.
- 로그에서 `npcReload` 또는 재장전 관련 디버그 출력이 있는지 확인합니다.
- `player-animation-lib`가 로드되어 있는지 확인합니다.
- 다른 렌더/애니메이션 모드가 NPC 팔 애니메이션을 덮어쓰지 않는지 확인합니다.

## 탄약이 채워지지 않음

확인할 것:

- `TACZ.setSupplyAmmo(e.npc, true)`인지 확인합니다.
- `TACZ.getAmmoStock(e.npc)`가 `0`보다 크거나 `-1`인지 확인합니다.
- `TACZ.setReloadDurationMs(e.npc, 값)`이 0보다 큰 값인지 확인합니다.
- 총기의 탄창 크기와 호환되는 탄약인지 확인합니다.

## 디버그 로그 켜기

```javascript
var TACZ = Java.type("net.dochi.tacznpcfire.TaczNpcFireBridge")

/**
 * @param {NpcEvent.InitEvent} e
 */
function init(e) {
    TACZ.setDebug(e.npc, true)
}
```

디버그 로그는 사격 결과, 탄약, 총기 RPM, 재장전 상태를 확인하는 임시 진단 기능입니다.

