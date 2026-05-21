# 저장 데이터

[English](Stored-Data-en) | 한국어

`TACZ NPC Fire`는 CustomNPCs NPC 래퍼가 전달될 때 `npc.getStoreddata()`에서 아래 키를 읽을 수 있습니다. GUI로 저장한 설정도 같은 정책으로 반영됩니다.

## 사용 예

```javascript
/**
 * @param {NpcEvent.InitEvent} e
 */
function init(e) {
    var data = e.npc.getStoreddata()
    data.put("tacznpcfire.enabled", "1")
    data.put("tacznpcfire.reload", "1")
    data.put("tacznpcfire.rpm", "600")
    data.put("tacznpcfire.maxDistance", "64")
    data.put("tacznpcfire.supplyAmmo", "1")
    data.put("tacznpcfire.ammoStock", "-1")
}
```

## 키 목록

| 키 | 값 | 설명 |
|---|---|---|
| `tacznpcfire.enabled` | `1` / `0` | 브릿지 기능을 켜거나 끕니다. |
| `tacznpcfire.reload` | `1` / `0` | 자동 재장전 허용 여부입니다. |
| `tacznpcfire.rpm` | 숫자 | 고정 RPM입니다. `0`이면 총기 기본 RPM을 사용합니다. |
| `tacznpcfire.rpmMin` | 숫자 | 무작위 RPM 최소값입니다. |
| `tacznpcfire.rpmMax` | 숫자 | 무작위 RPM 최대값입니다. |
| `tacznpcfire.maxDistance` | 숫자 | 최대 사격 거리입니다. 단위는 블록입니다. |
| `tacznpcfire.debug` | `1` / `0` | 상세 로그를 켭니다. 임시 디버그용입니다. |
| `tacznpcfire.supplyAmmo` | `1` / `0` | 재장전 완료 시 탄약 재고에서 총에 탄약을 넣습니다. |
| `tacznpcfire.ammoStock` | 숫자 | 예비 탄약입니다. `-1`은 무제한, `0`은 비어 있음입니다. |
| `tacznpcfire.ammoStockMax` | 숫자 | 회복 가능한 탄약 재고 상한입니다. `-1`은 상한 없음입니다. |
| `tacznpcfire.ammoRegenAmount` | 숫자 | 회복 간격마다 추가할 탄약 수입니다. |
| `tacznpcfire.ammoRegenIntervalMs` | 숫자 | 탄약 회복 간격입니다. 단위는 밀리초입니다. `0`이면 비활성입니다. |
| `tacznpcfire.reloadWalkSpeedMultiplier` | 숫자 | 재장전 중 이동속도 배율입니다. |
| `tacznpcfire.reloadDurationMs` | 숫자 | 재장전 완료까지 걸리는 시간입니다. 단위는 밀리초입니다. |

## 레거시 키

| 키 | 현재 상태 |
|---|---|
| `tacznpcfire.gunId` | 무시됩니다. NPC 메인핸드 TACZ 총기가 기준입니다. |
| `tacznpcfire.rpmMultiplier` | 무시됩니다. 배율은 `1.0`으로 고정됩니다. |
| `tacznpcfire.virtualGunHoldMs` | 무시됩니다. 가상 총기 방식은 일반 경로가 아닙니다. |

## 우선순위

1. 스크립트에서 호출한 `TACZ.set...()` 런타임 오버라이드
2. `npc.getStoreddata()` 또는 GUI로 저장된 정책
3. 모드 기본 설정

`TACZ.clearPolicy(npc)`를 호출하면 현재 런타임 오버라이드를 지웁니다. 저장 데이터 자체를 지우는 것은 아닙니다.

