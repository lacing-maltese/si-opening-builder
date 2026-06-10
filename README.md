# Spirit Island Opening Shorthand

A compact notation for recording the first few turns of a Spirit Island opening: growth choices, presence track usage, cards gained/forgotten/reclaimed, and cards played each turn. The goal is *gist of the opening at a glance*, not full game-state reconstruction. Notation explicitly encodes decisions made during the opening (e.g. Gaining a Major vs. a Minor), but not forced actions (e.g. Gain 3 Energy), nor decisions that pertain to the Island Board itself (e.g. Move a Presence requires a decision, i.e. where the presence moves, but the decision pertains only to Board state). 

---

## Format Overview

Each turn is a single chunk. 

`G#[modifiers]:[cards played]`

- Everything **before the colon** describes the growth choice and what entered your hand  
- Everything **after the colon** is the list of cards played that turn  
- If no cards were played, omit the colon entirely

Each turn is separated by ` | `.

`[Turn 1 Chunk] | [Turn 2 Chunk]`

---

## Components

### Growth Option: `G#`

The growth option number, indexed 1-N from left to right on the spirit's panel. Spirit-specific. 

For Spirits that use multiple Growth options in a turn (Sharp Fangs, Keeper, Trickster, Lure, Starlight), each Growth is concatenated (e.g. `G1G3`)

### Track Placement: `T`, `B`, `.1`\-`.6`

When the Growth option has a presence placement, which presence track a presence is taken from is annotated as:

- **Default Spirits:** `T` \= top track, `B` \= bottom track
- **Multiple placements:** repeat the letter — `TT`, `BB`, `TB`
- **>2 Track Spirits:** (i.e. Finder/Starlight): `.1`, `.2`, ... , `.n` as ordered on the panel

For mandatory Growth options, drop the `G` (for presence track or any other choices). Example: `BG2T` \= mandatory Growth, presence chosen from bottom track \+ G2's presence from top track.

### Reclaim: `r`

For passive reclaim abilities unlocked from presence tracks (e.g. River's "Reclaim One" slot), and for growth options that reclaim exactly one chosen card (e.g. Bringer G2). Takes an **Choice Modifier**.

**NB:** Does *not* apply to Reclaim All, because this does not require a player choice.

### Card Gained: `m` or `M`

For Growth options that allow a card to be Gained, indicate the chosen card type as:

- `m` — Gain a Minor power
- `M` — Gain a Major power (takes a **Choice Modifier**)

### Choice Modifier: `(Option)`

When your Turn includes a step that requires the player to make a specific choice, annotate it within parantheses. Some examples are: 

- Forgetting a card when Gaining a Major — `M(CardName)`
- Discarding a card (e.g. Downpour's G2) — `G2(CardName1,CardName2)`
- Making a choice within a Growth option (e.g. elements in Lure's G3) — `G3(moon)`
- Reclaim One Growth or Track options — `r(CardName)`
- Cards played with specific Choices (e.g. choosing to Gain a Major with Root's Boon) — `...:Boon(M(CardName))`

### Board-state Dependent Choices: `[X]`, `[X/Y]`, and `*`

Parentheses always mark a player decision point.

- `[n,X/Y/...]` — choose any n of the listed options (if n=1 it can be dropped)
- `*` — used when card plays are too situational to specify at all  
- `[Card]` — player choice to play this card at all  
- `!` — precede any notation with `!` to indicate the only option to **not** take of available options.

Examples:

- `G3[T/B]` — G3, place presence from top OR bottom track  
- `G2M-[Card1/Card2]` — G2, gain major forgetting Card1 OR Card 2  
- `...:[Card1/m]` — play Card1 OR a gained minor  
- `[2,Card1/Card2/Card3]` — play any of the three 0-cost uniques  
- `...:Card1,[Card2]` — play Card1 and decide whether to play Card2 depending on board-state

---

### Cards Played: After the Colon

A comma-separated list of cards played that turn.

- **Named Unique:** referred to by a single, uniquely identifying word  
- **Gained cards:** `m`, `M`

If no cards are played, omit the colon entirely.

---

## Parsing Cheat Sheet

When reading the opening `  G4Tm(moon)r(Card1):Card2(m),[m] | G2G3M(Card2):M,[2,Card3/Card4/m] | G2:*`:

1. Growth 4, Presence from Top track, Gain a Minor, Gain a Moon element (from the choices available to that Growth option), Play Card 2 and a minor (if you decide the board state requires it), Gain a Minor (from the choices on Card2)
2. Growth 2 and Growth 3, Gain a Major, Forget Card2, Play a Major and 2 of the cards Card3, Card4 or one of the chosen Minors
3. Growth 2, decide which cards to play based on board state

---

## Spirit Specific Considerations

### Vengeance as a Burning Plague

For G3, omit a `T`/`B` to indicate disease placement. All possible options for G3 are therfore begin as `G3T`, `G3B`, `G3`. 

### Fractured Days Split the Sky

When annotating G2 and G3, which require a choice to take presences as Time or a second option, take as many Time as tracks annotated and the remainder as the other option (e.g. `G3TB` reads as "Gain one Time from Top, Gain one Time from Bottom, and (implied) Gain 2 Energy").

**1-board setup:** In a 1-board game, gain 1 Time before T1, annotated as a bare track letter as its own turn: `T | G2B:...`
before T1

### Starlight Seeks Its Form

All choices are indicated by the order they are displayed on the spirit panel (i.e. Tracks are 1, ... , 6 top to bottom; unlocked Growth options are numbered 1 & 2 for left & right option). For example, Growing from Track 3 and for the second time to unlocking the "Reclaim Cards" Growth: `G2.3(2)`.

### Dances Up Earthquakes

When a card is played as Impending, it is annotated as `CardName'` or with a `(i)` modifier.

---

## Generally Out of Scope

- Range, target land requirements, and choices regarding board placement  
- Growth effects that do not result in a choice: gained card plays, element gains, Gather/Push presence, Beasts placement, Energy  
- Specific Gained minor/major card identities  
- Innate usage  
