# TACZ NPC Fire Wiki

English | [한국어](Home-ko)

`TACZ NPC Fire` is a CustomNPCs scripting bridge for making NPCs use TACZ guns. Your scripts decide when an NPC starts or stops firing; the mod handles TACZ internals, reload timing, ammo supply, and animation signals.

## Quick Start

The bridge uses the TACZ gun currently held in the NPC's main hand. The old `setGun()` path is legacy compatibility only.

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

## Pages

| Page | Contents |
|---|---|
| [Script API](Script-API-en) | Every `TACZ` method available to scripts |
| [Examples](Examples-en) | Auto-target fire, interact toggles, ammo, and debug samples |
| [Stored Data](Stored-Data-en) | Keys readable from `npc.getStoreddata()` |
| [Troubleshooting](Troubleshooting-en) | What to check when firing, reload, or animations do not work |

## Script Style

CustomNPCs 1.20.1 scripts run on Nashorn, so ES5-style JavaScript is recommended.

- Use `var`
- Use `function` declarations
- Avoid arrow functions, `let`, `const`, and template literals
- Add JSDoc event annotations for better editor autocomplete

