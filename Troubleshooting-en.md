# Troubleshooting

English | [한국어](Troubleshooting-ko)

## NPC Does Not Fire

Check:

- The NPC is holding a TACZ gun in its main hand.
- `TACZ.getGunInfo(e.npc)` returns `ok = true`.
- `TACZ.setEnabled(e.npc, true)` is called, or `tacznpcfire.enabled = 1` is stored.
- Your target event calls `TACZ.start(e.npc, e.entity)`.
- The target is not beyond `maxDistance`.

Diagnostic sample:

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

## Fire Rate Is Not What You Expected

RPM multiplier is no longer used.

- Use native gun RPM: `TACZ.setRpm(e.npc, 0)`
- Use fixed RPM: `TACZ.setRpm(e.npc, 600)`
- Use randomized range: `TACZ.setRpmRange(e.npc, 400, 700)`

The GUI fire-rate tab shows the native RPM of the gun currently held by the NPC.

## NPC Fires While Reloading

The current build enforces reload start and end timing. The NPC should not fire while the reload lock is active.

Check:

- The newest JAR is installed as `mods/tacznpcfire-0.1.0.jar`.
- `TACZ.setReload(e.npc, true)` is active, or `tacznpcfire.reload = 1` is stored.
- If ammo supply is enabled, `TACZ.setSupplyAmmo(e.npc, true)` is active and reserve ammo exists.
- `reloadDurationMs` is not too short.

## Reload Animation Does Not Play

The mod sends a reload signal to the client when reload starts. If the animation is still missing:

- Make sure client and server use the same JAR.
- Make sure the NPC is holding a real TACZ gun.
- Look for `npcReload` or reload diagnostics in the log.
- Make sure `player-animation-lib` is loaded.
- Check whether another render/animation mod is overriding NPC arm animations.

## Ammo Is Not Inserted

Check:

- `TACZ.setSupplyAmmo(e.npc, true)` is active.
- `TACZ.getAmmoStock(e.npc)` is greater than `0` or equals `-1`.
- `TACZ.setReloadDurationMs(e.npc, value)` is greater than `0`.
- The gun's magazine and ammo type are compatible.

## Enable Debug Logging

```javascript
var TACZ = Java.type("net.dochi.tacznpcfire.TaczNpcFireBridge")

/**
 * @param {NpcEvent.InitEvent} e
 */
function init(e) {
    TACZ.setDebug(e.npc, true)
}
```

Debug logging is a temporary diagnostic feature for checking shot results, ammo state, gun RPM, and reload state.

