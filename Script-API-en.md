# Script API

English | [한국어](Script-API-ko)

Load the bridge at the top of your script.

```javascript
var TACZ = Java.type("net.dochi.tacznpcfire.TaczNpcFireBridge")
```

## Firing Control

| Method | Returns | Description |
|---|---:|---|
| `TACZ.start(npc, target)` | `boolean` | Starts automatic firing at `target`. |
| `TACZ.start(npc)` | `boolean` | Starts firing using the current or auto-resolved target. |
| `TACZ.startEvent(event)` | `boolean` | Safely reads `event.npc` and `event.entity` from a CustomNPCs event. |
| `TACZ.stop(npc)` | `boolean` | Stops automatic firing for this NPC. |
| `TACZ.stopEvent(event)` | `boolean` | Stops firing by reading the NPC from an event. |
| `TACZ.shootOnce(npc, target)` | `String` | Attempts one shot and returns a result string. |

For CustomNPCs `TargetEvent`, `target` is usually `e.entity`.

## Reload

| Method | Returns | Description |
|---|---:|---|
| `TACZ.reload(npc)` | `boolean` | Starts the NPC reload flow. Firing is locked until the reload duration finishes. |
| `TACZ.setReload(npc, enabled)` | `boolean` | Enables or disables automatic reload. |
| `TACZ.setReloadDurationMs(npc, durationMs)` | `boolean` | Sets the reload completion delay in milliseconds. |
| `TACZ.setReloadWalkSpeedMultiplier(npc, multiplier)` | `boolean` | Sets movement speed multiplier while reloading. |

## Fire Rate

| Method | Returns | Description |
|---|---:|---|
| `TACZ.setRpm(npc, rpm)` | `boolean` | Sets a fixed target RPM. `0` uses the held gun's native RPM. |
| `TACZ.setRpmRange(npc, minRpm, maxRpm)` | `boolean` | Picks a random target RPM in the range after each shot. |
| `TACZ.clearRpm(npc)` | `boolean` | Clears fixed/random RPM and returns to the held gun's native RPM. |
| `TACZ.setRpmMultiplier(npc, multiplier)` | `boolean` | Legacy compatibility. The multiplier is no longer applied and is forced to `1.0`. |

Use `setRpm()` or `setRpmRange()` for new scripts. RPM multiplier is deprecated because fixed RPM covers the same use case more clearly.

## Ammo

| Method | Returns | Description |
|---|---:|---|
| `TACZ.setSupplyAmmo(npc, enabled)` | `boolean` | Controls whether reload completion inserts ammo from the NPC stock. |
| `TACZ.setAmmoInfinite(npc, infinite)` | `boolean` | Sets ammo stock to unlimited when `true`. |
| `TACZ.setAmmoStock(npc, rounds)` | `boolean` | Sets NPC reserve ammo. `-1` means unlimited. |
| `TACZ.addAmmoStock(npc, rounds)` | `boolean` | Adds or removes reserve ammo. |
| `TACZ.getAmmoStock(npc)` | `int` | Returns current reserve ammo. |
| `TACZ.setAmmoRegen(npc, amount, intervalMs, maxStock)` | `boolean` | Regenerates ammo stock over time. |

## Policy

| Method | Returns | Description |
|---|---:|---|
| `TACZ.setEnabled(npc, enabled)` | `boolean` | Enables or disables the bridge for this NPC. |
| `TACZ.setMaxDistance(npc, blocks)` | `boolean` | Sets maximum firing distance in blocks. |
| `TACZ.setDebug(npc, debug)` | `boolean` | Enables or disables detailed diagnostics. |
| `TACZ.clearPolicy(npc)` | `boolean` | Clears script runtime overrides. |

## Gun Info

| Method | Returns | Description |
|---|---:|---|
| `TACZ.getGunInfo(npc)` | `Map` | Returns current held TACZ gun data and policy state. |

Common keys:

| Key | Meaning |
|---|---|
| `ok` | Whether lookup succeeded |
| `reason` | Failure reason, such as `NO_TACZ_GUN` |
| `gunId` | TACZ gun ID |
| `rpm` | Held gun native RPM |
| `targetRpm` | Target RPM resolved from current policy |
| `ammo` | Current ammo in the gun |
| `bulletAmount` | TACZ bullet amount value |
| `magazineSize` | Magazine size |
| `fireMode` | Gun fire mode |
| `bulletInBarrel` | Whether the chamber has a round |
| `nativeShootIntervalMs` | TACZ native shoot interval |
| `lastAttemptIntervalMs` | Last bridge-scheduled attempt interval |
| `rpmOverride`, `rpmMin`, `rpmMax` | Current fire-rate policy |

## Legacy Gun Methods

| Method | Returns | Current Status |
|---|---:|---|
| `TACZ.setGun(npc, gunId)` | `boolean` | Legacy compatibility. The NPC main-hand TACZ gun is now authoritative. |
| `TACZ.clearGun(npc)` | `boolean` | Clears legacy state. The main-hand item remains the real source. |
| `TACZ.setVirtualGunHoldMs(npc, holdMs)` | `boolean` | Legacy compatibility. Virtual guns are no longer the normal path. |

