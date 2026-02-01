# Starfield Quest - Particle System Specification

## Project Overview
Create an interactive webpage featuring a particle-based starfield visualization with visible trails and user-controllable animation parameters via sliders. This project demonstrates particle system fundamentals—where each particle is a bundle of data (position, velocity, age) governed by simple rules that produce complex visual effects.

## Source
Assignment from: https://www.frontiersof.tech/starfield-quest/

## Requirements

### Core Requirements
1. **Particle System**: Build a particle-based starfield visualization
   - Each star is an individual particle with properties (position, velocity, age)
   - Stars should appear to fly toward the viewer (classic starfield/warp effect)
   - Use 3D-to-2D perspective projection for depth illusion

2. **Trail Effects**: Stars must leave visible trails as they move
   - Trails should fade over time
   - Trail length should be controllable

3. **Interactive Sliders**: Provide user controls to adjust animation attributes
   - Controls should update the animation in real-time
   - Include visual feedback showing current values

### Stretch Goal
4. **Non-Obstructive Controls**: Position sliders outside the display area
   - Controls should not cover or obscure the starfield animation
   - Consider a sidebar or collapsible panel design

## Technical Requirements

### HTML Structure
- Single-page application with embedded CSS and JavaScript
- Canvas element for rendering the particle system
- Semantic HTML5 structure

### Particle Properties
Each star particle should track:
- **Position**: x, y, z coordinates in 3D space
- **Previous Position**: For drawing trails
- **Age**: For color/brightness variation
- **Velocity**: Controlled by speed parameter

### Animation Controls (Sliders)
| Control | Range | Purpose |
|---------|-------|---------|
| Star Count | 50-500 | Number of particles in the system |
| Speed | 1-10 | How fast stars travel toward viewer |
| Trail Length | 0.5-0.98 | Persistence of trails (fade rate) |
| Star Size | 0.5-5 | Base size of particles |
| Color Hue | 0-360 | Shift the color spectrum |
| Spread | 0.5-2 | How wide the starfield spreads |

### Visual Features
- Perspective projection (stars grow larger as they approach)
- Brightness increases as stars get closer
- Color variation based on particle age and hue setting
- Glow effect on bright, close stars
- FPS and particle count display

## Implementation Plan

### Step 1: Set Up Project Structure
- Create project directory `starfield-quest`
- Create `index.html` with HTML5 boilerplate
- Set up flexbox layout with sidebar for controls

### Step 2: Build the Controls Panel
- Create slider inputs for each animation parameter
- Add labels and value displays
- Style with dark theme to match space aesthetic
- Position as sidebar (outside canvas area)

### Step 3: Implement Canvas and Particle System
- Set up canvas to fill remaining viewport
- Create Star class with position, velocity, and age properties
- Implement 3D-to-2D perspective projection
- Handle canvas resize events

### Step 4: Add Trail Effects
- Store previous position for each particle
- Draw lines from previous to current position
- Use semi-transparent overlay for fade effect (controlled by trail length slider)

### Step 5: Wire Up Controls
- Add event listeners to all sliders
- Update animation parameters in real-time
- Dynamically add/remove particles when count changes

### Step 6: Add Polish
- Implement glow effects for close stars
- Add FPS counter and particle count display
- Add hide/show toggle for controls panel
- Test and optimize performance

### Step 7: Deploy
- Initialize Git repository
- Push to GitHub
- Enable GitHub Pages

## Design Decisions

### Why Canvas over CSS/SVG?
- Better performance for hundreds of moving particles
- Direct pixel manipulation for trail effects
- Easier to implement 3D projection math

### Why Sidebar Layout?
- Satisfies stretch goal of non-obstructive controls
- Clean separation of UI and visualization
- Better UX on desktop browsers

### Trail Implementation
- Using semi-transparent black overlay each frame
- Higher trail length value = more transparent overlay = longer trails
- Simple and performant approach

## Expected Outcome
A publicly accessible webpage featuring:
- Smooth starfield animation with customizable speed and density
- Visible trails creating a "warp speed" effect
- Intuitive slider controls in a collapsible sidebar
- Responsive canvas that fills available space

## Success Criteria
- [ ] Starfield renders with perspective depth effect
- [ ] Trails are visible and adjustable
- [ ] All 6 sliders function correctly
- [ ] Controls don't obscure the animation
- [ ] Animation runs smoothly (30+ FPS)
- [ ] Page is accessible via GitHub Pages
