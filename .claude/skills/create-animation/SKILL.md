---
name: create-animation
description: Create a custom 8-bit pixel art side-scroller animation for the webinar hero section based on a photo. Use when the user wants to add the hero animation, create the 8-bit character, or runs /create-animation during the demo.
---

## Instructions

1. **Ask for a photo** — Ask the user to paste or drag an image of the person who should be the 8-bit character. Wait for the image before proceeding.

2. **Analyze the image** — Study the photo carefully and note:
   - Hair color, length, and style
   - Skin tone
   - Glasses (yes/no, frame style and color)
   - Clothing color and style (shirt, jacket, etc.)
   - Any distinctive features (beard, accessories, etc.)

3. **Create the pixel art animation** — Write CSS in `webinar_site/static/animation.css` that creates a **Super Mario-style side-scroller scene**:

   ### Scene Structure
   - **Full-width scene** — `pixel-character-scene` spans 100% of the left column, **160px tall**, `overflow: hidden`
   - **Transparent background** — no background color, page cream (#faf9f5) shows through as the "sky"
   - **Scrolling ground** — Anthropic coral (#d97757 / #c84400) 8-bit brick blocks at bottom (~24px tall), with black mortar lines in staggered rows, scrolling left via `background-position` animation
   - **Pixel art hills** — stepped pyramid shapes using `box-shadow` with green shading (#5ab847, #4a9a3a, #3a8a2a), scrolling right-to-left. Use **negative y-values** so hills grow upward from `bottom: 24px`. Scale(7) for big hill, scale(5) for small hill.
   - **Pixel art clouds** — white puffy `box-shadow` sprites (~6x4 pixels at scale 3), floating across at different speeds with staggered `animation-delay`
   - **Speech bubble** — white box with black border, monospace font, positioned with `top: -28px` relative to the runner container (NOT `bottom`) so it floats just above the character's head. CSS triangle tail pointing down. Text: "let claude cook"

   ### Character (Chibi Style — Super Cute!)
   - **Proportions**: Big head (~60% of height), tiny body, stubby legs. ~12 wide x 13 tall pixel grid at `scale(5)`
   - **Style**: Super Mario-inspired — bold flat colors (2 tones max per material), no gradient shading. Black (#000) is NOT used as outline around the whole character — use dark colors that contrast naturally against cream background
   - **Running pose**: Side profile facing RIGHT, holding a laptop (solid gray rectangle, NO screen dot) tucked under arm
   - **Match the person's appearance** from the photo (hair color, skin tone, clothing color)
   - **Cute details**: Big white eye with dark pupil, rosy cheek pixel (#ff9999), smile, hair pixel blown back for motion feel
   - **Running animation**: Two leg frames that alternate via opacity toggle (`steps(1)`), synced with a vertical bounce on the runner container (both ~0.25s)

   ### Animations
   - `runner-bounce` — character bobs up/down ~4px while running. The `.scene-runner` container must be positioned at `bottom: 24px` (same as ground height). Since the character is drawn with **negative y-values** in `box-shadow`, the pixels extend upward from the element — so the element itself sits at the character's feet, right on top of the ground.
   - `leg-toggle` — alternates between two leg frame divs (steps(1), 0.25s)
   - `scroll-ground` — brick pattern scrolls left via background-position
   - `float-cloud` — clouds translate from right to left at different speeds (10s, 15s, 20s)
   - `scroll-hill` — hills scroll right-to-left, slower than ground (parallax)

   ### Key CSS Patterns
   - Use `box-shadow` on 1px elements with `transform: scale(N)` for all pixel art
   - Hills use **negative y-values** in box-shadow (e.g., `6px -5px 0 #5ab847`) so they extend upward from the ground line
   - Ground uses CSS `background-image` with `repeating-linear-gradient` for brick pattern + `::after` pseudo-element for offset bottom row
   - Character body is one layer; legs are two separate layers toggled with opacity animation
   - Speech bubble is a regular HTML div with CSS styling (not pixel art)

4. **Update the HTML template** — In `webinar_site/templates.py`:
   - **ADD** the animation **between the countdown section and the featuring section**
   - **DO NOT** replace or remove the terminal visual (`placeholder-graphic` / `placeholder-terminal` inside `#hero-visual`) — it must stay in its original position after the featuring section
   - **MUST include the `<!-- 8-bit Side Scroller Animation -->` comment marker** — `reset-demo.sh` uses it to find and remove the block
   - Use this HTML structure:
     ```html
     <!-- 8-bit Side Scroller Animation -->
     <div class="pixel-character-scene">
         <div class="scene-cloud cloud-1"></div>
         <div class="scene-cloud cloud-2"></div>
         <div class="scene-cloud cloud-3"></div>
         <div class="scene-hill hill-1"></div>
         <div class="scene-hill hill-2"></div>
         <div class="scene-runner">
             <div class="speech-bubble">let claude cook</div>
             <div class="runner-body"></div>
             <div class="runner-legs leg-frame-1"></div>
             <div class="runner-legs leg-frame-2"></div>
         </div>
         <div class="scene-ground"></div>
     </div>
     ```

5. **Verify** — Tell the user to open http://localhost:8000 to see the animation (hot-reload in `run.py` picks up the changes automatically — no server restart needed). Describe what it looks like.

## Style Reference

Super Mario World meets chibi anime. Chunky pixels, bold saturated colors, black mortar lines on bricks, stepped green hills, puffy white clouds. The character should be unmistakably cute — oversized head, tiny body, rosy cheek, big expressive eye. The running animation and scrolling scene make it feel alive and playful.
