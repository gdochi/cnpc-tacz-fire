# 예제

[English](Examples-en) | 한국어

모든 예제는 NPC가 메인핸드에 TACZ 총기를 들고 있다는 전제를 둡니다.

## 타겟을 잡으면 자동 사격

```javascript
var TACZ = Java.type("net.dochi.tacznpcfire.TaczNpcFireBridge")

/**
 * @param {NpcEvent.InitEvent} e
 */
function init(e) {
    TACZ.setEnabled(e.npc, true)
    TACZ.setReload(e.npc, true)
    TACZ.setSupplyAmmo(e.npc, true)
    TACZ.setAmmoInfinite(e.npc, true)
    TACZ.setMaxDistance(e.npc, 64)
}

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

/**
 * @param {NpcEvent.DiedEvent} e
 */
function died(e) {
    TACZ.stop(e.npc)
}
```

## 상호작용으로 사격 켜고 끄기

```javascript
var TACZ = Java.type("net.dochi.tacznpcfire.TaczNpcFireBridge")

/**
 * @param {NpcEvent.InitEvent} e
 */
function init(e) {
    e.npc.getStoreddata().put("tacz_demo_enabled", "0")
}

/**
 * @param {NpcEvent.InteractEvent} e
 */
function interact(e) {
    var data = e.npc.getStoreddata()
    var enabled = data.get("tacz_demo_enabled") == "1"

    if (enabled) {
        TACZ.stop(e.npc)
        data.put("tacz_demo_enabled", "0")
        e.npc.say("TACZ fire stopped")
    } else {
        TACZ.start(e.npc, e.player)
        data.put("tacz_demo_enabled", "1")
        e.npc.say("TACZ fire started")
    }
}
```

## 현재 총기 RPM을 보고 고정 RPM 지정

```javascript
var TACZ = Java.type("net.dochi.tacznpcfire.TaczNpcFireBridge")

/**
 * @param {NpcEvent.InteractEvent} e
 */
function interact(e) {
    var info = TACZ.getGunInfo(e.npc)
    if (!info.get("ok")) {
        e.npc.say("No TACZ gun: " + info.get("reason"))
        return
    }

    e.npc.say("Native RPM: " + info.get("rpm"))
    TACZ.setRpm(e.npc, 450)
    e.npc.say("Fixed RPM set to 450")
}
```

`TACZ.setRpm(e.npc, 0)`을 호출하면 고정값을 지우고 총기 기본 RPM으로 돌아갑니다.

## 무작위 사격 속도

```javascript
var TACZ = Java.type("net.dochi.tacznpcfire.TaczNpcFireBridge")

/**
 * @param {NpcEvent.InitEvent} e
 */
function init(e) {
    TACZ.setRpmRange(e.npc, 360, 720)
}
```

이 설정은 매 사격 후 다음 사격 목표 RPM을 다시 뽑습니다.

## 탄약 재고와 회복

```javascript
var TACZ = Java.type("net.dochi.tacznpcfire.TaczNpcFireBridge")

/**
 * @param {NpcEvent.InitEvent} e
 */
function init(e) {
    TACZ.setSupplyAmmo(e.npc, true)
    TACZ.setAmmoStock(e.npc, 120)
    TACZ.setAmmoRegen(e.npc, 15, 10000, 120)
}
```

이 NPC는 120발을 들고 시작하고, 10초마다 15발씩 최대 120발까지 회복합니다.

## 재장전 시간 조정

```javascript
var TACZ = Java.type("net.dochi.tacznpcfire.TaczNpcFireBridge")

/**
 * @param {NpcEvent.InitEvent} e
 */
function init(e) {
    TACZ.setReload(e.npc, true)
    TACZ.setReloadDurationMs(e.npc, 1800)
    TACZ.setReloadWalkSpeedMultiplier(e.npc, 0.45)
}
```

재장전은 시작 시간과 종료 시간이 보장됩니다. 재장전 잠금이 걸린 동안 NPC는 사격하지 않습니다.

## 디버그 정보 출력

```javascript
var TACZ = Java.type("net.dochi.tacznpcfire.TaczNpcFireBridge")

/**
 * @param {NpcEvent.InteractEvent} e
 */
function interact(e) {
    TACZ.setDebug(e.npc, true)
    var info = TACZ.getGunInfo(e.npc)
    e.npc.say("Gun info ok: " + info.get("ok"))
    e.npc.say("Gun: " + info.get("gunId") + " RPM: " + info.get("rpm"))
}
```

디버그 로그는 나중에 제거될 임시 기능으로 보는 것이 좋습니다.

