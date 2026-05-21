# 스크립트 API

[English](Script-API-en) | 한국어

스크립트 상단에 브릿지를 불러옵니다.

```javascript
var TACZ = Java.type("net.dochi.tacznpcfire.TaczNpcFireBridge")
```

## 사격 제어

| 메서드 | 반환 | 설명 |
|---|---:|---|
| `TACZ.start(npc, target)` | `boolean` | NPC가 `target`을 향해 사격을 시작합니다. |
| `TACZ.start(npc)` | `boolean` | 현재 타겟을 자동으로 찾거나, 이미 잡힌 타겟으로 사격을 시작합니다. |
| `TACZ.startEvent(event)` | `boolean` | CustomNPCs 이벤트에서 `event.npc`, `event.entity`를 안전하게 읽어 시작합니다. |
| `TACZ.stop(npc)` | `boolean` | 해당 NPC의 자동 사격을 중지합니다. |
| `TACZ.stopEvent(event)` | `boolean` | 이벤트에서 NPC를 읽어 사격을 중지합니다. |
| `TACZ.shootOnce(npc, target)` | `String` | 한 번만 사격을 시도하고 결과 문자열을 반환합니다. |

`target`은 보통 `e.entity`입니다. CustomNPCs의 `TargetEvent`에서는 `e.target`보다 `e.entity`가 맞는 경우가 많습니다.

## 재장전

| 메서드 | 반환 | 설명 |
|---|---:|---|
| `TACZ.reload(npc)` | `boolean` | NPC의 재장전 흐름을 시작합니다. 재장전 시간이 끝나기 전에는 사격하지 않도록 잠금 처리됩니다. |
| `TACZ.setReload(npc, enabled)` | `boolean` | 자동 재장전 허용 여부를 설정합니다. |
| `TACZ.setReloadDurationMs(npc, durationMs)` | `boolean` | 재장전 완료까지 기다릴 시간을 밀리초로 설정합니다. |
| `TACZ.setReloadWalkSpeedMultiplier(npc, multiplier)` | `boolean` | 재장전 중 이동속도 배율을 설정합니다. |

## 사격 속도

| 메서드 | 반환 | 설명 |
|---|---:|---|
| `TACZ.setRpm(npc, rpm)` | `boolean` | 고정 사격속도를 설정합니다. `0`이면 현재 총기의 기본 RPM을 사용합니다. |
| `TACZ.setRpmRange(npc, minRpm, maxRpm)` | `boolean` | 매 사격 후 범위 안에서 목표 RPM을 다시 고릅니다. |
| `TACZ.clearRpm(npc)` | `boolean` | 고정/무작위 RPM 설정을 지우고 총기 기본 RPM으로 돌아갑니다. |
| `TACZ.setRpmMultiplier(npc, multiplier)` | `boolean` | 레거시 호환용입니다. 현재는 배율을 적용하지 않고 `1.0`으로 고정합니다. |

권장 방식은 `setRpm()` 또는 `setRpmRange()`입니다. 배율 방식은 중복 기능이라 더 이상 사용하지 않습니다.

## 탄약

| 메서드 | 반환 | 설명 |
|---|---:|---|
| `TACZ.setSupplyAmmo(npc, enabled)` | `boolean` | 재장전 완료 시 NPC 탄약 재고에서 총에 탄약을 넣을지 설정합니다. |
| `TACZ.setAmmoInfinite(npc, infinite)` | `boolean` | `true`면 탄약 재고를 무제한으로 설정합니다. |
| `TACZ.setAmmoStock(npc, rounds)` | `boolean` | NPC의 예비 탄약 수를 설정합니다. `-1`은 무제한입니다. |
| `TACZ.addAmmoStock(npc, rounds)` | `boolean` | 예비 탄약을 더하거나 뺍니다. |
| `TACZ.getAmmoStock(npc)` | `int` | 현재 예비 탄약 수를 반환합니다. |
| `TACZ.setAmmoRegen(npc, amount, intervalMs, maxStock)` | `boolean` | 일정 시간마다 탄약 재고를 회복합니다. |

## 기본 정책

| 메서드 | 반환 | 설명 |
|---|---:|---|
| `TACZ.setEnabled(npc, enabled)` | `boolean` | 해당 NPC의 브릿지 기능을 켜거나 끕니다. |
| `TACZ.setMaxDistance(npc, blocks)` | `boolean` | 최대 사격 거리를 블록 단위로 설정합니다. |
| `TACZ.setDebug(npc, debug)` | `boolean` | 상세 로그를 켜거나 끕니다. |
| `TACZ.clearPolicy(npc)` | `boolean` | 스크립트 런타임 오버라이드를 초기화합니다. |

## 총기 정보

| 메서드 | 반환 | 설명 |
|---|---:|---|
| `TACZ.getGunInfo(npc)` | `Map` | 현재 NPC가 든 TACZ 총기의 정보와 정책 상태를 반환합니다. |

주요 키:

| 키 | 의미 |
|---|---|
| `ok` | 총기 정보 조회 성공 여부 |
| `reason` | 실패 이유. 예: `NO_TACZ_GUN` |
| `gunId` | TACZ 총기 ID |
| `rpm` | 총기의 기본 RPM |
| `targetRpm` | 현재 정책으로 계산된 목표 RPM |
| `ammo` | 현재 장탄 수 |
| `bulletAmount` | 탄약 아이템 수량 관련 값 |
| `magazineSize` | 탄창 크기 |
| `fireMode` | 총기 발사 모드 |
| `bulletInBarrel` | 약실 탄 여부 |
| `nativeShootIntervalMs` | TACZ 기본 사격 간격 |
| `lastAttemptIntervalMs` | 브릿지가 마지막으로 예약한 사격 간격 |
| `rpmOverride`, `rpmMin`, `rpmMax` | 현재 사격 속도 정책 |

## 레거시 총기 메서드

| 메서드 | 반환 | 현재 상태 |
|---|---:|---|
| `TACZ.setGun(npc, gunId)` | `boolean` | 레거시 호환용입니다. 현재는 NPC 메인핸드 총기를 기준으로 합니다. |
| `TACZ.clearGun(npc)` | `boolean` | 레거시 상태를 정리합니다. 실제 기준은 NPC 메인핸드입니다. |
| `TACZ.setVirtualGunHoldMs(npc, holdMs)` | `boolean` | 레거시 호환용입니다. 현재 가상 총기 방식은 사용하지 않습니다. |

