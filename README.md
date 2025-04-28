# Colony Events Expanded – Design Document

## 1. Vision & Scope
Expand Stellaris’ ambient colony flavour by adding **20 lore-friendly, planet-scoped story events** that randomly fire during normal gameplay. Each event is lightweight (text + temporary modifier), so the mod remains save-game-friendly and performance-neutral.

Goals:
* More narrative variety on colonies without overwhelming the player.
* Balanced positive/negative outcomes (roughly 50 / 50).
* Zero new GUI assets or decisions to keep footprint small.

Non-Goals:
* No scripted event chains, pop-ups with choices, or permanent planet modifiers.
* Maintain full multiplayer/achievement safety (ironman-safe – only localisation and data).

## 2. Technical Overview
Directory layout (inside `mod/colony_events_expanded/`):
```
colony_events_expanded.mod     – launcher descriptor
/events/colony_events_expanded.txt  – dispatcher + 20 planet_events
/common/planet_modifiers/cee_modifiers.txt – 20 temporary planet modifiers
/localisation/english/cee_l_english.yml – localisation strings
/DESIGN_DOCUMENT.md            – this file
```

Key elements:
| Element | Type | Purpose |
|---------|------|---------|
| `on_action on_colony_yearly_pulse` | Built-in on-action | 20 % chance yearly per colony to call dispatcher. |
| `planet_event cee.1` | Hidden | Random-lists to one of the 20 visible events (equal weight). |
| `planet_event cee.2 … cee.21` | Visible | Show title, desc, image; apply matching modifier or flavour. |
| `cee_*` planet modifiers | Planet modifier | 3600-day (10-year) temporary effects. |

## 3. Event Design
Each event defines:
* **Title** & **Description** (localised) – storytelling tone.
* **Picture** – chosen vanilla GFX to match theme.
* **Trigger** – limited to `owner.is_ai = no` to avoid AI spam.
* **Effect** – `add_planet_modifier` (or none for pure flavour).

Balance matrix (summary):
| Event | Bias | Main Impact |
|-------|------|-------------|
| Climate Systems Failure | – | Housing -1, Amenities -5, Stability -2 |
| Festival of Prosperity | + | Stability +3, Amenities +5 |
| … | … | … (see modifiers file) |

Duration set to 3600 days so that a given planet rarely stacks multiple identical modifiers.

## 4. Modifiers
Naming prefix `cee_` avoids conflicts. All modifiers have:
* `description` key for localisation.
* `tag = cee` group tag.
* An **icon** reused from vanilla (no new assets).
* Simple additive or percentage modifiers sized for flavour, not heavy balance impact (~±5-10 %).

## 5. Localisation
English only (`cee_l_english.yml`). Each key follows:
* `cee.<id>.title` / `cee.<id>.desc` for events.
* `cee_<modifier>` & `_desc` for modifiers.

Adding new languages = copy file, translate strings.

## 6. Compatibility & Load Order
* Uses **only** new IDs & namespaces (`cee`).
* Replaces *vanilla* `on_colony_yearly_pulse` block. If another mod overwrites the same on-action, last-loaded mod wins. To avoid conflict, users should load CEE later, or merge on-actions manually.
* Ironman & multiplayer safe – no scripted effects impacting checksum.

## 7. Performance Considerations
* Only one RNG roll per colony per year (very low impact).
* No looping or heavy scripted triggers.

## 8. Future Expansion Ideas
* 5–10 additional events with player choices (branching outcomes).
* Chain some events into multi-stage narratives.
* Add planet decisions to attempt mitigation (e.g., cure plague early).
* More localisation languages.

## 9. Changelog
* **v1.0** – initial release with 20 events, modifiers, localisation.

---
*Author: OGENRWOT DANIEL – 2025-04-28*
