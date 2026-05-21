# Stored Data

English | [한국어](Stored-Data-ko)

When a CustomNPCs NPC wrapper is passed to the bridge, `TACZ NPC Fire` can read the following keys from `npc.getStoreddata()`. GUI-saved settings use the same policy layer.

## Example

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

## Keys

| Key | Value | Description |
|---|---|---|
| `tacznpcfire.enabled` | `1` / `0` | Enables or disables the bridge. |
| `tacznpcfire.reload` | `1` / `0` | Enables automatic reload. |
| `tacznpcfire.rpm` | number | Fixed RPM. `0` uses the held gun's native RPM. |
| `tacznpcfire.rpmMin` | number | Random RPM minimum. |
| `tacznpcfire.rpmMax` | number | Random RPM maximum. |
| `tacznpcfire.maxDistance` | number | Maximum firing distance in blocks. |
| `tacznpcfire.debug` | `1` / `0` | Enables detailed logging. Temporary debug feature. |
| `tacznpcfire.supplyAmmo` | `1` / `0` | Inserts ammo from reserve stock when reload completes. |
| `tacznpcfire.ammoStock` | number | Reserve ammo. `-1` is unlimited, `0` is empty. |
| `tacznpcfire.ammoStockMax` | number | Maximum regenerated reserve ammo. `-1` means uncapped. |
| `tacznpcfire.ammoRegenAmount` | number | Ammo added every regen interval. |
| `tacznpcfire.ammoRegenIntervalMs` | number | Ammo regen interval in milliseconds. `0` disables regen. |
| `tacznpcfire.reloadWalkSpeedMultiplier` | number | Movement speed multiplier while reloading. |
| `tacznpcfire.reloadDurationMs` | number | Reload completion delay in milliseconds. |

## Legacy Keys

| Key | Current Status |
|---|---|
| `tacznpcfire.gunId` | Ignored. The NPC main-hand TACZ gun is authoritative. |
| `tacznpcfire.rpmMultiplier` | Ignored. Multiplier is forced to `1.0`. |
| `tacznpcfire.virtualGunHoldMs` | Ignored. Virtual guns are not the normal path. |

## Priority

1. Runtime overrides from `TACZ.set...()` script calls
2. `npc.getStoreddata()` or GUI-saved policy
3. Mod defaults

`TACZ.clearPolicy(npc)` clears runtime overrides. It does not erase stored data.

