# Pixel Pals — implementation plan

## Goal and delivery

Build the approved browser-playable, offline 2D version of Pixel Pals in the initialized Godot Web project. Keep seven interactive companions and the six requested mini-games at the center. Browser-local saves hold progress and settings; native Electron, system tray, OS notifications and startup hooks are excluded by the approved browser adaptation. Use the starter's Web export, runtime, save/checkpoint and preview paths rather than replacing its managed infrastructure.

## Gameplay and implementation

- Replace the generic maze screen with an app home area containing the pals, persistent hearts/stars/streak, reminder card, and visible navigation for Arcade, Wardrobe, Focus/Pomodoro and Settings.
- Store editable character definitions, quiz questions and grouped reminder text in project-local JSON. Draw each mascot from 32×32 pixel primitives in GDScript so no character PNG/sprite-sheet dependency is introduced. Each actor owns its animation state, movement, drag capture, and click-to-arcade interaction.
- Keep home state, browser save data, reminder scheduling and settings in `PixelPalsApp`. Keep game-specific state, scoring and feedback in `MiniGameHost`; keep procedural actor drawing/behavior in `PixelPalActor`. Use a small main-scene bridge and leave compatible template autoloads and Web runtime wiring intact.
- Implement the quiz, card-pair Memory Match, solvable 3×3 slide puzzle, timed Math Sprint, keyboard/button Rhythm Tap, and five-letter word guesses. Include difficulty selection, score/timer display where relevant, add-question form, result feedback and return/replay flow.
- Schedule reminders against persisted settings, honor quiet hours and focus mode, provide Done/Snooze interactions and a synthesized low-volume chime. Replace system notifications/tray actions with in-app UI controls and toasts.
- Persist hearts, stars, streak, achievements, wardrobe ownership/equipment, active characters, settings, custom quiz entries and Pomodoro state locally. Use the platform's finite Godot checks, build the Web Preview, prepare a matching release, and checkpoint the verified project.

## Project structure

```text
project.godot                         # Pixel Pals identity, Web canvas settings and native icon
scenes/main.tscn                      # Stable initialized root scene
scripts/main.gd                       # Minimal entry bridge into the app
scripts/game/pixel_pals_app.gd        # Main UI, state, settings, reminders, rewards and navigation
scripts/game/mini_game_host.gd        # Six mini-games and scoring/results
scripts/game/pixel_pal_actor.gd       # Procedural 32×32 sprite, behavior and drag interaction
data/characters.json                  # Seven editable pal definitions
 data/questions.json                  # 40 sample and category-based quiz questions
 data/messages.json                   # 100+ categorized reminder lines
web/loading.html                      # Pixel Pals-branded recoverable Godot Web loader
assets/share/favicon.png              # Small project-specific pixel favicon
assets/share/og.png                   # Game-specific promotional cover
 game-sharing.json                    # Title, description and favicon/OG references
 game-verification.json               # Finite Pixel Pals-specific core checks
```

## Visual design

- **Design movement:** The user's requested pastel pixel-art desktop-companion aesthetic: tiny chibi mascots with a purple K-pop-idol-group vibe, expressed in a browser-friendly 2D play space.
- **Core principles:** Distinct friendly mascots; readable pixel-scale silhouettes; controls that stay obvious while pals animate; gentle feedback that supports productivity rather than competing for attention.
- **Color philosophy:** Purple and lavender establish the group identity; pastel character accents distinguish the seven pals; warm light panels and calm backgrounds keep text readable and the mood gentle.
- **Layout paradigm:** A single-stage companion room with persistent navigation/status at the top, pals along the lower play area, and focused in-app panels for arcade, wardrobe, settings and timer.
- **Signature elements:** 32×32 code-drawn chibi sprites with job-specific accessories; floating sleep z's and heart/stars feedback; compact pixel-framed game cards.
- **Interaction philosophy:** Click a pal for their arcade, drag them to reposition within the room, use visible controls for longer tasks, and let Done/Snooze/focus options stay easy to find.
- **Animation:** Use a restrained 6–8 fps pixel-frame cadence for walk/idle/sleep/cheer/dance; keep movement bounded to the lower play area; honor reduced-motion preference by minimizing bobbing and idle movement.
- **Typography system:** Use the initialized project's bundled display and body fonts for readable headings and controls; preserve live text for all interface labels, quiz content, scores and timers.
- **Brand essence:** A pocket-sized pixel friend group for gentle focus, self-care and playful study breaks. Personality: cozy, encouraging, playful.
- **Brand voice:** Warm, brief and kind, with no guilt. Example lines: “Tiny sip break? Your brain is thirsty too!” and “One page, one problem, one win!”
- **Wordmark & logo:** A live-text PIXEL PALS lockup with a small pixel heart/star badge, rendered in the game UI and echoed in the loader.
- **Signature brand color:** Pastel violet/lavender, paired with seven individually distinguishable pastel accents.

## Browser-specific constraints

- Use an offline single-player Godot Web export. Browser local persistence replaces the Electron JSON/localStorage arrangement.
- Use in-app reminder cards rather than OS notifications; no native tray, OS startup or transparent desktop-wide overlay is part of the browser version.
- Keep code-generated character art; the required favicon and social cover are separate sharing/loader assets, not sprite sources for runtime characters.
- Preserve English as the game's single content language; no locale switcher is introduced.
