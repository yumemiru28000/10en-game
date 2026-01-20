# Yellow Area Launch Mechanism - Implementation Notes

## Feature Summary
Implemented a visual gauge UI for the yellow area (spring) launcher mechanism in the 10-yen coin game.

## Requirements (Translated from Japanese)
When the coin touches a yellow area:
- Yellow on the right side pushes the coin to the left
- Yellow on the left side pushes the coin to the right
- A UI appears next to the yellow area for dragging
- A gauge fills up as you drag
- There is a maximum power limit
- When released, the coin launches with strength based on drag distance

## Implementation Details

### 1. Direction Logic
Located in `addSpringsFromComponents()` (line 402):
```javascript
const dir = (c.centroid.x < centerX) ? +1 : -1;
```
- Left yellow areas (x < centerX): dir = +1 → pushes RIGHT
- Right yellow areas (x >= centerX): dir = -1 → pushes LEFT

### 2. Launch Velocity
Located in `launchFromSpring()` (line 490):
```javascript
Body.setVelocity(coin, { x: s.dir * impulse, y: -impulse * verticalBias });
```
- Horizontal velocity = direction × power
- Vertical velocity = upward boost (negative y)

### 3. Visual Gauge UI
New rendering code added in `afterRender` event:

#### When Dragging:
1. **Drag Line**: Dashed yellow line from spring center to cursor position
2. **Power Gauge Bar**:
   - Position: Next to the active spring (right side for left springs, left side for right springs)
   - Size: 80px wide × 15px tall
   - Color gradient based on power:
     - 0-50%: Green → Yellow
     - 50-90%: Yellow → Orange
     - 90-100%: Orange → Red
3. **Maximum Limit Indicator**: Red line at 90% position
4. **Power Percentage**: Shows current power (0-100%)
5. **MAX! Warning**: Red text when power ≥ 90%

#### When Not Dragging (Coin in Spring Range):
1. **Spring Area Highlight**: Dashed yellow rectangle around the spring
2. **Direction Arrow**: Shows "→" or "←" indicating push direction

### 4. Drag Mechanics
- Maximum drag distance: 140px
- Power calculation: `power01 = Math.min(1, dragDistance / 140)`
- Active spring is captured at drag start and used throughout the interaction
- Power is applied when mouse/touch is released

## Files Modified
- `game.html`: Added visual gauge rendering and improved drag tracking

## Testing Notes
- The game requires Matter.js to run (loaded from CDN)
- In sandbox environments without CDN access, the logic can be verified by code review
- The implementation follows the existing code patterns and integrates with the current spring mechanism

## Visual Features Added
1. Real-time power gauge display during drag
2. Color-coded power levels for better feedback
3. Maximum power warning
4. Direction indicators for spring areas
5. Visual connection between spring and drag point
