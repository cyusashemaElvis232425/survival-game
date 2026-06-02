# PixelSurvivor - L3 SOD Game Prototype Submission

### Student Information
* **Name:** [Your Full Name Here]
* **Student ID:** [Your Student ID Here]

### Deploy Artifact Links
* **Live Netlify URL:** https://pixel-survivor-game-232425.netlify.app
* **GitHub Repository URL:** https://github.com/cyusashemaElvis232425/survival-game

---

## Exploration Challenge & Interaction Model Choice

For PixelSurvivor, I selected a **mouse tracker interaction model**. The player avatar follows the cursor inside the game board so the controls are immediately obvious for anyone who opens the link.

This model suits a non-technical youth audience because it requires no keyboard memorization, no configuration, and the action is visible instantly. Users can simply move the mouse to dodge hazards and survive as long as possible.

### Bonus Elements Added
1. **High-score tracker** stored in Vue state and saved during the session.
2. **SVG-based player character** and hazard rendering directly in the Vue component markup.

### Technical Obstacle & Engineering Solution
* **Challenge:** The cursor coordinates were not matching the game board after resizing, which made the avatar drift away from the user pointer.
* **Solution:** I used `getBoundingClientRect()` on the game board container and mapped the browser coordinates back into the fixed `800 × 600` game coordinate system. This keeps mouse movement aligned with the SVG game area on different screen sizes.
