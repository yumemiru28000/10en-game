# Yellow Area Launch Mechanism - Visual Features

## Overview
This document describes the visual features added to the 10-yen coin game for the yellow area (spring) launcher mechanism.

## Features Implemented

### 1. Power Gauge Display (During Drag)
```
When dragging from a yellow spring area:

    ┌────────────────────────────────┐
    │  [====>     ] 50%              │  ← Power gauge bar (80px x 15px)
    │    ↑                           │     - Green (0-50%)
    │    │                           │     - Yellow (50-90%)
    │    │   🟡 ← Spring area        │     - Orange/Red (90-100%)
    │    │   /                       │  
    │    └──┘  ← Dashed line        │  
    │        \                       │  
    │         (cursor position)      │  
    └────────────────────────────────┘
    
    At maximum power (≥90%):
    ┌────────────────────────────────┐
    │  [==========|] 100%  MAX!      │  ← Red limit line at 90%
    │              ↑                 │     "MAX!" warning text
    └────────────────────────────────┘
```

### 2. Spring Area Indicator (When Coin is in Range)
```
When coin is inside a yellow spring area (not dragging):

    ╔═══════════════════╗  ← Dashed yellow highlight
    ║                   ║
    ║       🟡          ║
    ║        ↑          ║  ← Direction arrow
    ║        →          ║     (→ for left spring)
    ║    (10円)         ║     (← for right spring)
    ║                   ║
    ╚═══════════════════╝
```

### 3. Direction Logic
```
Screen Layout:
    Left Side               Center              Right Side
  ┌────────────┬──────────────────────┬────────────┐
  │            │                      │            │
  │   🟡 (L)   │                      │   🟡 (R)   │
  │    dir=+1  │                      │    dir=-1  │
  │  pushes → │      (10円)          │  ← pushes  │
  │            │                      │            │
  └────────────┴──────────────────────┴────────────┘

Left Yellow:  x < centerX → dir = +1 → pushes RIGHT (→)
Right Yellow: x ≥ centerX → dir = -1 → pushes LEFT  (←)
```

## Configuration Constants

All visual parameters are configurable:

| Constant | Value | Description |
|----------|-------|-------------|
| MAX_PULL_DISTANCE | 140px | Maximum drag distance |
| GAUGE_WARNING_THRESHOLD | 0.9 | Power threshold for warning (90%) |
| GAUGE_COLOR_MID_THRESHOLD | 0.5 | Color transition point (50%) |
| GAUGE_WIDTH | 80px | Gauge bar width |
| GAUGE_HEIGHT | 15px | Gauge bar height |
| GAUGE_OFFSET_X | 60px | Horizontal offset from spring |
| GAUGE_OFFSET_Y | 20px | Vertical offset from spring |
| ARROW_OFFSET_Y | 30px | Vertical offset for direction arrow |

## Color Scheme

### Power Gauge Gradient
- **0-50% power**: Green (#00ff00) → Yellow (#ffff00)
- **50-90% power**: Yellow (#ffff00) → Orange (#ff8800)
- **90-100% power**: Orange (#ff8800) → Red (#ff0000)

### UI Elements
- **Drag line**: Yellow (rgba(255, 200, 0, 0.8)) with dashed pattern
- **Gauge background**: Black (rgba(0, 0, 0, 0.7))
- **Gauge border**: White (rgba(255, 255, 255, 0.8))
- **Limit marker**: Red (rgba(255, 0, 0, 0.9))
- **Spring highlight**: Yellow (rgba(255, 255, 0, 0.6)) with dashed pattern
- **Direction arrow**: Yellow (rgba(255, 255, 0, 0.8))

## User Interaction Flow

1. **Coin enters yellow area**
   - Yellow dashed highlight appears around spring area
   - Direction arrow shows which way coin will be launched

2. **User clicks/touches**
   - Drag starts from click position
   - Active spring is locked in

3. **User drags**
   - Dashed line connects spring center to cursor
   - Power gauge appears next to spring
   - Gauge fills based on drag distance
   - Color changes from green → yellow → orange → red
   - Percentage shown above gauge
   - "MAX!" warning appears at ≥90% power

4. **User releases**
   - Coin launches with calculated power
   - Direction determined by spring position (left→right, right→left)
   - Visual feedback disappears
   - Spring enters cooldown (0.3s)

## Visual Feedback Summary

✅ Clear power indication with color coding
✅ Maximum limit warning prevents over-pulling
✅ Direction arrows eliminate confusion
✅ Real-time visual feedback during drag
✅ Spring area highlights when interactive
✅ Consistent visual language throughout
