# Eikonomiya-data
Game info to update [eikonomiya](https://github.com/eikofee/eikonomiya) without rebuilding docker image every time.
Since this thing is updated "by hand", I use yaml markup.

## Structure of data

### Buff/Debuff
Field name | Available values | Description
---|---|---
`target` | `self`, `enemy`, `team`| Target of the buff.
`stat` | *See stats symbols* | Stat affected by the buff.
`value` | Float | Modifier affected to the stat. Can be a flat value or a multiplier in ratio cases.
`maxvalue` | Float | (Optional) Used when value is a ratio. Maximum value allowed for the buff.

### Effect/Passive
Field name | Available values | Description
---|---|---
`type` | `none`,`buff`, `bool buff`, `elem bool buff`, `stack buff`, `precise-stack buff` | Mechanic of the effect, see below.
`text` | String | Description of how the effect/passive works.
`maxstack` | Int | (Optional) Maximum number of stack allowed when `stack buff` or `precise-stack buff` is used in `type`.
`buff` | Buff Object | Buff provided by the effect/passive. Can be an array.
`elems`| Buff Objects | (Optional) Buff provided depending on the element involved when `elem bool buff` is used in `type`. Use field names `pyro`, `hydro`, `cryo`, `electro`, `geo`, `dendro` to list the buffs. Can **not** be an array.
`stacks` | Buff Objects | (Optional) Buff provided depending on the number of stacks when `precise-stack buff` is used in `type`. Use integers to list effects. Can **not** be an array.

`buff` is the simplest form of effect, it gives bonus without condition. For other types, use the `text` field to explain how it works.
`bool buff` asks for certain conditions to be enabled, such as using an elemental skill. It has to be enabled manually by the user.
`stack buff` is an extension of the boolean version but is multiply the buff by the number of stacks selected, starting at 0.
`precise-stack buff` is a more complicated version of the boolean version, but instead of multiplying the buff, it applies the buff listed under a specific number of stacks.
`elem bool buff` works in a similar way but instead of a number of stacks, it works with specific elements.
If the effect is not important for the computation, use `none`.

### Artefact
Field name | Available values | Description
---|---|---
`2pc` | Effect Object | The passive given when wearing 2 pieces of the artefact set. Can be an array.
`4pc` | Effect Object | The passive given when wearing 4 pieces of the artefact set. Can be an array.

### Stats Symbols
Symbol | Description
---|---
`hp` | HP (flat)
`hp%` | HP%
`atk` | ATK (flat)
`atk%` | ATK%
`def` | DEF (flat)
`def%` | DEF%
`em` | Elemental Mastery
`er%` | Energy Recharge
`cr%` | Crit Rate%
`cdmg%` | Crit DMG%
`dmg` | Bonus DMG (flat)
`dmg%` | Bonus DMG%
`spd%` | Bonus animation speed
`na` | Normal Attack (prefix)
`ca` | Charged Attack (prefix)
`pa` | Plunged Attack (prefix)
`skill` | Elemental Skill (prefix)
`burst` | Elemental Burst (prefix)
`heal in%` | Healing received bonus%
`heal out%` | Healing effectiveness%
`shield%` | Shield resistance%

Elements and Elemental reactions symbols can also be used as prefix.

## DMG Formula reminder
```
(Talent Multiplier * ((Character ATK + Weapon ATK) * (100% + ATK%) + Artefact ATK) + DMG Bonus) * (100% + DMG Bonus%) * Enemy Def (50%) * (100% - Enemy Elemental RES%)

```