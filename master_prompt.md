# TASK

Write a single, standalone HTML file for a Spirit Island opening strategy logger featuring dynamic, JSON-driven spirit configurations.

# NEVER

These rules are absolute. Violating any of them is a critical failure regardless of whether the output otherwise works.

1. NEVER use inline HTML event attributes. `onclick="fn()"`, `onchange="fn()"`, and all similar attributes are forbidden on every element without exception — including static layout buttons like “+ Play Card”, “+ Add Modifier”, “Commit Turn”, and “Clear Turn”. Every event listener must be attached in JavaScript using addEventListener or direct .onclick property assignment AFTER the element is created or selected.
   
   CORRECT:
   
   ```
   const btn = document.createElement('button');
   btn.onclick = () => handleManualCard();
   ```
   
   or:
   
   ```
   document.getElementById('playCardBtn').addEventListener('click', () => handleManualCard());
   ```
   
   FORBIDDEN:
   
   ```
   <button onclick="handleManualCard()">+ Play Card</button>
   ```
1. NEVER write directly to #livePreview from any function other than updatePreview(). updatePreview() is the single source of truth for preview DOM updates. All other functions call updatePreview() — they never touch #livePreview themselves.
1. NEVER use addShortcutToLibrary() for growth or track buttons. Growth and track buttons are created directly in renderProfile() using their own dedicated loops. addShortcutToLibrary() is only for card and modifier shortcuts.
1. NEVER hardcode game buttons into the HTML body. All interactive game buttons (growth, track, card, modifier shortcuts) must be injected dynamically via JavaScript.
1. NEVER use a rows attribute on the history textarea. Use CSS height: 150px and resize: none exclusively.

-----

# DATA LAYER

Initialize this exact constant schema at the very top of your script block:

const SPIRIT_DATABASE = {
“Generic”: {
“growth”: [“G1”, “G2”, “G3”, “G4”, “G5”],
“tracks”: [“T”, “B”, “m”, “M”, “r”, “.”],
“cards”: [],
“modifiers”: []
},
“Lightning”: {
“growth”: [“G1”, “G2”, “G3”, “G4”],
“tracks”: [“T”, “B”, “Air”, “Fire”],
“cards”: [“Shatter Homesteads”, “Raging Storm”, “Harbinger of Lightning”, “Lightning’s Boon”],
“modifiers”: [“Fast”, “Slow”]
},
“Blank”: {
“growth”: [],
“tracks”: [],
“cards”: [],
“modifiers”: []
}
};

-----

# HTML LAYOUT

The HTML body contains only empty layout anchors. No game logic, no hardcoded buttons.

Inside a single div with class “charcoal-container”, include exactly:

1. `<select id="spiritSelector">` — empty, populated by JS
1. `<div id="livePreview">` — empty, written only by updatePreview()
1. `<div id="growthContainer" class="container-box">` — empty, populated by JS
1. `<div id="trackContainer" class="container-box">` — empty, populated by JS
1. A block containing: `<input id="cardInput">`, a `<button id="playCardBtn">+ Play Card</button>`, and `<div id="cardLibrary" class="container-box">`
1. A block containing: `<input id="modifierInput">`, a `<button id="addModifierBtn">+ Add Modifier</button>`, and `<div id="modifierLibrary" class="container-box">`
1. A block containing: `<button id="commitBtn" class="commit-action">Commit Turn</button>` and `<button id="clearBtn" class="clear-action">Clear Turn</button>`
1. `<textarea id="history" readonly>`

All event listeners for #playCardBtn, #addModifierBtn, #commitBtn, and #clearBtn must be attached in JavaScript, not inline.

-----

# STYLING

Dark mode. All measurements explicit.

Body: background #1e1e24, color white, font-size 16px, width 100%, box-sizing border-box, margin 0, padding 0.

.charcoal-container: background #2a2a35, padding 20px, border-radius 8px.

.base-button: background #3e3e47, color white, border none, padding 10px 20px, font-size 16px, border-radius 4px, cursor pointer. No focus color tint — focus styles apply only to inputs, selects, and textareas.

.commit-action: background #0f8c4a, color white, same padding and sizing as .base-button.

.clear-action: background #b91c1c, color white, same padding and sizing as .base-button.

Utility buttons (#playCardBtn, #addModifierBtn): must use class “base-button”. They are styled identically to game token buttons.

#livePreview: background #111827, color white, padding 10px, font-family monospace, word-break break-all, border-radius 4px.

.container-box: display flex, flex-wrap wrap, gap 6px, margin 10px 0, width 100%, box-sizing border-box. Buttons inside a container-box render beside each other horizontally and wrap to the next line only when they run out of space. They never stack vertically by default.

input, select, textarea: font-size 16px, width 100%, box-sizing border-box, background #3e3e47, border 1px solid #3e3e47, color white, padding 10px, margin-bottom 10px.

textarea#history: height 150px, resize none. No rows attribute.

input:focus, select:focus, textarea:focus: border-color #0f8c4a, outline none.

-----

# JAVASCRIPT STATE ENGINE

## Global state

```
let sequenceArray = [];
let cardsActive = false;
```

## Boot sequence

Inside a single DOMContentLoaded listener:

1. Loop over Object.keys(SPIRIT_DATABASE) and append an `<option>` for each to #spiritSelector.
1. Attach a change listener to #spiritSelector: `spiritSelector.addEventListener('change', (e) => renderProfile(e.target.value));`
1. Attach click listeners to all four static buttons: #playCardBtn → handleManualCard(), #addModifierBtn → handleManualModifier(), #commitBtn → commitTurn(), #clearBtn → clearCurrentTurn().
1. Call renderProfile(“Generic”) to initialize the workspace.

## renderProfile(spiritName)

1. Reset sequenceArray = [] and cardsActive = false. Call updatePreview().
1. Clear innerHTML of #growthContainer, #trackContainer, #cardLibrary, #modifierLibrary.
1. For each item in spiritData.growth: create a button with class “base-button”, set textContent, assign .onclick = () => logTokenEntry(item), append to #growthContainer.
1. For each item in spiritData.tracks: create a button with class “base-button”, set textContent, assign .onclick = () => logTokenEntry(item), append to #trackContainer.
1. For each card in spiritData.cards: call addShortcutToLibrary(card, ‘cardLibrary’, () => executeCardPlay(card)).
1. For each modifier in spiritData.modifiers: call addShortcutToLibrary(modifier, ‘modifierLibrary’, () => executeModifier(modifier)).

## logTokenEntry(token)

Push token to sequenceArray. Call updatePreview().

## handleManualCard()

Read and trim #cardInput value. If blank, return. Call addShortcutToLibrary(cardName, ‘cardLibrary’, () => executeCardPlay(cardName)). Call executeCardPlay(cardName). Clear #cardInput.

## executeCardPlay(cardName)

If cardsActive is false: push “:” + cardName to sequenceArray, set cardsActive = true. Otherwise push “,” + cardName. Call updatePreview().

## handleManualModifier()

Read and trim #modifierInput value. If blank, return. Call addShortcutToLibrary(modifierText, ‘modifierLibrary’, () => executeModifier(modifierText)). Call executeModifier(modifierText). Clear #modifierInput.

## executeModifier(modifierText)

If sequenceArray.length > 0: sequenceArray[sequenceArray.length - 1] += “(” + modifierText + “)”. Call updatePreview().

## addShortcutToLibrary(text, targetContainerId, executionCallback)

Get container by id. Check existing .base-button elements for duplicate text — if found, return without adding. Create button with class “base-button”, set textContent = text, assign .onclick = executionCallback. Append to container.

## clearCurrentTurn()

Set sequenceArray = [], cardsActive = false. Call updatePreview(). Do not touch #history.

## commitTurn()

If sequenceArray.length === 0, return. Append sequenceArray.join(’’) + “ | “ to #history.value. Set sequenceArray = [], cardsActive = false. Call updatePreview().

## updatePreview()

document.getElementById(‘livePreview’).textContent = sequenceArray.join(’’);
This is the ONLY function that writes to #livePreview.

-----

# OUTPUT RESTRAINT

Output ONLY raw, functional HTML and vanilla JavaScript. Do not wrap the response in markdown backticks. Do not include any explanation or commentary before or after the HTML.
