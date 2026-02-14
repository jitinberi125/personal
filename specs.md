# Valentine’s Day Interactive Web Page - Specifications

## Overview

Create a single-page HTML application for mobile devices that asks someone to be your Valentine with interactive, playful button behavior.

## Core Functionality

### Questions Flow

The page displays three questions sequentially:

1. “Will you be my valentine? 💝”
1. “Are you sure? 🥺” (after first “No” click)
1. “Are you 100% sure? 😢” (after second “No” click)

### Buttons

Two buttons are displayed: “Yes” and “No”

**Initial State:**

- Yes button: positioned at 25% from left, centered vertically
- No button: positioned at 75% from left, centered vertically
- Both buttons use rounded square styling (border-radius: 15px)

### Button Behavior - “Yes” Button

**When clicked:**

- Hide the question section
- Display success message with:
  - Large animated heart emoji (❤️)
  - Text: “Thank you! Love you! 💕 I knew you would say yes! 😊”

**Scaling:**

- Starts at scale 1.0
- Increases by 0.5x after each “No” click
- Final scale after 3 “No” clicks: 2.5x

**Visual:**

- Background: Pink/red gradient (linear-gradient(135deg, #ff6b9d 0%, #ff1744 100%))
- Color: White text
- Always stays in its original position (25% left, 50% top)

### Button Behavior - “No” Button

**When clicked (first 3 times):**

- Trigger question change (see Questions Flow above)
- Scale down by 0.25x each click
- Stay in original position (75% left, 50% top)
- Final scale after 3 clicks: 0.25x

**After 3rd click:**

- Button becomes “dodgeable” - it moves when user tries to interact
- On mobile: moves on `touchstart` event (before user can tap)
- On desktop: moves on `mouseenter` event (when hovering)

**Dodging Logic:**

- Only activates after 3 “No” clicks
- Must jump at least 120px away from current position
- Must stay at least 100px away from Yes button center
- Position range: 15-85% horizontal, 20-80% vertical
- Maximum 50 attempts to find valid position
- Uses `preventDefault()` and `stopPropagation()` on touch events

**Visual:**

- Background: Gray gradient (linear-gradient(135deg, #9e9e9e 0%, #616161 100%))
- Color: White text
- NOT visually disabled after 3 clicks (stays looking clickable)

## Layout & Styling

### Page Structure

- Full viewport height with flexbox centering
- Pink gradient background: linear-gradient(135deg, #ffeef8 0%, #ffe0f0 100%)
- Maximum width: 500px
- Optimized for mobile devices

### Typography

- Font family: Arial, sans-serif
- Question heading: 1.8em, pink color (#ff1744), with text shadow
- Button text: 1.1em, bold
- Success message: 1.8em, pink color, bold

### Button Container

- Fixed height: 350px
- Provides space for button movement
- Relative positioning for absolute button placement

### Buttons

- Padding: 15px 35px
- Border radius: 15px (rounded squares, not ovals)
- Box shadow: 0 4px 15px rgba(0, 0, 0, 0.2)
- Position: absolute with percentage-based positioning
- Transform origin: center (for proper scaling)
- No tap highlight on mobile (-webkit-tap-highlight-color: transparent)

### Animations

**Heartbeat (for success heart):**

- Duration: 1.5s, infinite, ease-in-out
- Keyframes: scale(1) → scale(1.1) → scale(1.05) → scale(1)

**Fade In (for success message):**

- Duration: 1s, ease-in
- Keyframes: opacity 0 + translateY(-20px) → opacity 1 + translateY(0)

## Technical Requirements

### File Type

- Single HTML file with embedded CSS and JavaScript
- No external dependencies
- Works offline after initial load

### Browser Compatibility

- Modern mobile browsers (iOS Safari, Chrome, Firefox)
- Touch event support with `{ passive: false }` for preventDefault
- No jQuery or libraries needed

### Responsive Design

- Mobile-first approach
- Container max-width: 500px
- Touch-friendly button sizes
- No horizontal scrolling

### JavaScript State Management

Track these variables:

- `noClickCount`: number (0-3)
- `yesBtnScale`: number (starts at 1, increases by 0.5)
- `noBtnScale`: number (starts at 1, decreases by 0.25)

### Position Calculation Algorithm

For “No” button dodging after 3 clicks:

1. Get current button center coordinates
1. Generate random position (15-85% width, 20-80% height)
1. Calculate distance from current position
1. If distance < 120px, reject and try again
1. Calculate distance from Yes button center
1. If distance < 100px, reject and try again
1. If valid, apply new position with percentage values
1. Maximum 50 attempts before giving up

### Event Handling

**Yes Button:**

- Click event → show success message

**No Button:**

- Click event (first 3 times) → update question, scale buttons
- Click event (after 3 times) → preventDefault, do nothing
- Desktop: mouseenter → moveNoButton()
- Mobile: touchstart → preventDefault + moveNoButton()

## Color Palette

- Background gradient: #ffeef8 to #ffe0f0
- Primary pink: #ff1744
- Light pink: #ff6b9d
- Gray: #9e9e9e to #616161
- White: #ffffff
- Shadow: rgba(0, 0, 0, 0.2)
- Text shadow: rgba(255, 23, 68, 0.2)

## Success Criteria

- [ ] Three questions display sequentially on “No” clicks
- [ ] Yes button grows with each “No” click
- [ ] No button shrinks with each “No” click
- [ ] Buttons never overlap at any scale
- [ ] No button jumps away after 3rd “No” click
- [ ] Button movement works on both mobile touch and desktop hover
- [ ] Success message displays with heart animation on “Yes” click
- [ ] All elements stay within viewport bounds
- [ ] Page is fully functional as a single HTML file
- [ ] Mobile-optimized and touch-friendly

## Edge Cases to Handle

- User tries to click “No” more than 3 times (block the click)
- Button movement fails to find valid position after 50 attempts (keep current position)
- Both buttons remain visible and accessible throughout interaction
- Transform scales are applied while maintaining translate centering
- Touch events don’t trigger unwanted clicks on mobile