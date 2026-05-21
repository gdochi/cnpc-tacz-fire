# Examples

English | [한국어](Examples-ko)

All examples assume the NPC is holding a TACZ gun in its main hand.

## Start Firing When a Target Is Acquired

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

## Toggle Firing by Interacting

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

## Read Native Gun RPM and Set a Fixed RPM

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

Call `TACZ.setRpm(e.npc, 0)` to clear the fixed RPM and return to the gun's native RPM.

## Randomized Fire Rate

```javascript
var TACZ = Java.type("net.dochi.tacznpcfire.TaczNpcFireBridge")

/**
 * @param {NpcEvent.InitEvent} e
 */
function init(e) {
    TACZ.setRpmRange(e.npc, 360, 720)
}
```

The next target RPM is picked again after each shot.

## Ammo Stock and Regeneration

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

This NPC starts with 120 reserve rounds and regenerates 15 rounds every 10 seconds up to 120.

## Reload Timing

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

Reload start and end times are enforced. While the reload lock is active, the NPC will not fire.

## Debug Info

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

Debug logging should be treated as a temporary troubleshooting feature.

