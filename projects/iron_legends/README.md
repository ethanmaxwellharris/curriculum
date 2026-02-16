# Iron Legends (Iron RPG / Barbell Legends)

A vibe-coded concept for a strength app that mixes historical powerlifting milestones with RPG progression loops.

## What is in this folder?

- `iron-legends.seed.json`: canonical world + character seed data.
- This README: implementation guide for building an MVP quickly.

## MVP build order

1. **Load seed data**
   - Parse `iron-legends.seed.json` into app state.
   - Expose read-only profile sections (character, stats, equipment, abilities).
2. **Character Sheet screen**
   - Show archetype, class/subclass, quote, passives, and artifacts.
   - Add a “Ritual Ready” indicator when signature ability cooldown is available.
3. **Questline screen**
   - Render arc cards from `questline` keys in order.
   - Mark one active arc + track completion checkboxes per week.
4. **Progression screen**
   - Display ascension title timeline.
   - Compute next title from consistency score (sessions completed / sessions planned).
5. **Lift Log + PR unlock logic**
   - Persist squat / bench / deadlift entries.
   - Trigger unlock toast when a lift beats historical threshold goals.

## Suggested data model (TypeScript)

```ts
export type LiftName = 'squat' | 'bench' | 'deadlift';

export interface LiftEntry {
  dateISO: string;
  lift: LiftName;
  weightLb: number;
  reps: number;
  rpe?: number;
  notes?: string;
}

export interface UnlockEvent {
  id: string;
  unlockedAtISO: string;
  category: 'title' | 'quest' | 'relic' | 'milestone';
  name: string;
  reason: string;
}

export interface IronLegendsSave {
  seedVersion: string;
  playerName: string;
  activeArc: string;
  completedArcs: string[];
  liftLog: LiftEntry[];
  unlocked: UnlockEvent[];
  ritualStreakDays: number;
}
```

## Example unlock rules

- **The Profane Prayer (empowered)**: unlock when ritual streak reaches 7 days.
- **Disciple of the Old Saints**: unlock after completing arc 2 and logging 12 sessions in 30 days.
- **Road to Coan access**: unlock when deadlift reaches 405 lb or training consistency > 85% for 8 weeks.

## Quick UI vibes

- Palette: charcoal / iron gray / parchment white / accent ember.
- Typography: bold condensed headings + readable mono for lift logs.
- Components: relic cards, arc banners, session timeline, PR modal, codex tabs.

## Optional next steps

- Add Notion/Google Sheets sync for external backup.
- Add meet-day “boss fight” mode with attempt selection and confidence meter.
- Add lore codex entries tied to lineage path unlocks (Hepburn → Mythic).
