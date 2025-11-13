# LittleJS Cheatsheet

## Overview
LittleJS is a fast, lightweight, and fully open source HTML5 game engine designed for simplicity and performance. Its small footprint is packed with a comprehensive feature set including hybrid rendering, physics, particles, sound, and input handling.

### Key Features
- ✨ Super fast WebGL2 + Canvas2D hybrid rendering (100,000+ sprites at 60fps)
- 🔊 Sound and music with mp3, ogg, wave, or ZzFXM
- 🎮 Full input support: keyboard, mouse, gamepad, and touch
- ⚙️ 2D physics and collision system
- 🌟 Robust particle effect system
- 🎨 Post-processing effects with Shadertoy style shaders
- 📦 No dependencies, extremely small footprint

## Quick Start

### Installation
```bash
npm install littlejsengine
```

### Basic Setup
```javascript
// Import LittleJS
import * as LittleJS from 'littlejsengine';

// Core engine callbacks
function gameInit() {
  // Called once after engine is ready
}

function gameUpdate() {
  // Called every frame at 60 FPS
}

function gameUpdatePost() {
  // Called after physics and objects are updated
}

function gameRender() {
  // Called before objects are rendered
}

function gameRenderPost() {
  // Called after objects are rendered
}

// Start the engine
engineInit(gameInit, gameUpdate, gameUpdatePost, 
           gameRender, gameRenderPost, imageSources=[]);
```

## Core Utilities

### Vector2 - 2D Vector Math
```javascript
// Create vectors
const v = vec2(10, 20);           // Create Vector2
const v2 = vec2(5);                // Create Vector2 with x=y=5

// Vector operations
v.add(v2);                         // Add vectors
v.subtract(v2);                    // Subtract vectors
v.multiply(v2);                    // Multiply by vector
v.scale(2);                        // Scale by float
v.length();                        // Get length
v.normalize();                     // Normalize to length 1
v.angle();                         // Get angle (up is 0)
v.rotate(PI/4);                    // Rotate by angle
v.lerp(v2, 0.5);                   // Interpolate 50% to v2
```

### Color - RGBA Color Object
```javascript
// Create colors
const red = rgb(1, 0, 0);          // RGB color
const blue = hsl(0.6, 1, 0.5);     // HSL color
const transparent = rgb(1,1,1,0.5); // With alpha

// Color operations
red.lerp(blue, 0.5);               // Blend colors
red.mutate(0.1);                   // Randomly mutate
red.setHex('#FF0000');             // Set from hex
red.toString();                    // Get hex string
```

### Random Functions
```javascript
rand(10, 20);                      // Random float 10-20
randInt(1, 6);                     // Random int 1-6
randBool(0.7);                     // 70% chance true
randSign();                        // Random -1 or 1
randVec2(10);                      // Random vector length 10
randInCircle(5);                   // Random point in circle
randColor(red, blue);              // Random color between
```

### Math Helper Functions
```javascript
clamp(value, 0, 100);              // Clamp between 0-100
lerp(10, 20, 0.5);                 // Interpolate to 15
percent(15, 10, 20);               // Get percent (0.5)
smoothStep(0.5);                   // Smooth interpolation
wave(2, 10);                       // Oscillating wave
```

### Timer System
```javascript
const timer = new Timer(3);        // 3 second timer
timer.set(5);                      // Set to 5 seconds
timer.active();                    // Is set and not elapsed?
timer.elapsed();                   // Has it elapsed?
timer.getPercent();                // Get percent complete
```

## Drawing System

### Drawing Shapes
```javascript
// Basic shapes
drawRect(pos, size, WHITE);
drawCircle(pos, radius, WHITE);
drawLine(posA, posB, width, WHITE);
drawPoly(points, WHITE);

// With outline
drawCircle(pos, radius, WHITE, lineWidth=.1, BLACK);

// Gradients
drawRectGradient(pos, size, colorTop, colorBottom);
```

### Drawing Tiles/Sprites
```javascript
// Create tile info
const tileInfo = tile(vec2(0,0), vec2(16,16));

// Draw tile
drawTile(pos, size, tileInfo, WHITE, angle=0, mirror=false);

// Animated tiles
const frame = Math.floor(time * 10) % 4;  // 4 frame animation
const animTile = tileInfo.frame(frame);
drawTile(pos, size, animTile, WHITE);
```

### Text Rendering
```javascript
// World space text
drawText('Score: 100', pos, size=1, WHITE);

// Screen space text
drawTextScreen('Lives: 3', screenPos, size=40, WHITE);

// With outline
drawText('Player', pos, 1, WHITE, lineWidth=.1, BLACK);
```

### Camera Control
```javascript
cameraPos = vec2(100, 50);         // Set camera position
cameraScale = 32;                  // Zoom level (pixels per unit)

screenToWorld(mousePos);           // Convert coordinates
worldToScreen(playerPos);          // Convert coordinates
```

## Audio System

### Sound Effects
```javascript
// Load sound from file
const jumpSound = new Sound('jump.mp3');

// Play sound
jumpSound.play();                  // Simple playback
jumpSound.play(pos, volume=1, pitch=1);  // With params

// ZzFX generated sound (no file needed!)
const beep = new Sound([,,100,.01,.01,.05,1,1.5,,,,,,.5]);
beep.play();
```

### Music
```javascript
// Load and play music
const music = new Sound('music.mp3');
music.playMusic(volume=0.5, loop=true);

// ZzFXM music
const zzfxMusic = new Music(...musicData);
zzfxMusic.playMusic(volume=0.8);
```

### Audio Settings
```javascript
soundEnable = true;                // Enable/disable sound
soundVolume = 0.5;                 // Master volume (0-1)
```

## Input System

### Keyboard
```javascript
// Key states
keyIsDown('Space');                // Is key currently down?
keyWasPressed('Enter');            // Was pressed this frame?
keyWasReleased('Escape');          // Was released this frame?

// Direction input
const dir = keyDirection('ArrowUp', 'ArrowDown', 
                         'ArrowLeft', 'ArrowRight');
// Also works with WASD automatically
```

### Mouse
```javascript
mousePos;                          // World space position
mousePosScreen;                    // Screen space position
mouseWheel;                        // Wheel delta this frame

mouseIsDown(0);                    // Left button down?
mouseWasPressed(0);                // Left button pressed?
mouseWasReleased(0);               // Left button released?
// 0=left, 1=middle, 2=right
```

### Gamepad
```javascript
isUsingGamepad;                    // Is gamepad active?
gamepadIsDown(0);                  // Button 0 down?
gamepadWasPressed(0);              // Button 0 pressed?
const stick = gamepadStick(0);     // Left stick value
```

### Touch/Mobile
```javascript
touchGamepadEnable = true;         // Show virtual gamepad
touchGamepadAnalog = true;         // Analog or 8-way dpad
touchGamepadSize = 99;             // Size of gamepad
touchGamepadAlpha = 0.3;           // Transparency
```

## Object System

### Creating Objects
```javascript
class Player extends EngineObject {
  constructor(pos) {
    super(pos, vec2(1,1));         // Call parent constructor
    this.color = RED;              // Set color
  }
  
  update() {
    super.update();                // Call parent update
    
    // Custom update logic
    const moveInput = keyDirection(
      'ArrowUp', 'ArrowDown', 'ArrowLeft', 'ArrowRight'
    );
    this.velocity = moveInput.scale(0.1);
  }
  
  collideWithObject(object) {
    // Handle collision with another object
    return true;  // Return true to resolve collision
  }
}

// Create instance
const player = new Player(vec2(5, 5));
```

### Object Properties
```javascript
object.pos;                        // Position (Vector2)
object.size;                       // Size (Vector2)
object.velocity;                   // Velocity (Vector2)
object.angle;                      // Rotation angle
object.angleVelocity;              // Angular velocity
object.color;                      // Tint color
object.mass;                       // Mass (0 = static)
object.damping;                    // Velocity damping (0-1)
object.friction;                   // Friction (0-1)
object.restitution;                // Bounciness (0-1)
object.gravityScale;               // Gravity multiplier
object.renderOrder;                // Draw order
```

### Object Methods
```javascript
object.destroy();                  // Remove object
object.applyForce(force);          // Apply physics force
object.applyAcceleration(accel);   // Apply acceleration
object.addChild(child, localPos);  // Attach child object
object.removeChild(child);         // Detach child
object.setCollision();             // Enable collision
```

## Tile Layer System

### Creating Tile Layers
```javascript
// Create visible tile layer
const tileInfo = tile(vec2(0,0), vec2(16,16));
const layer = new TileLayer(vec2(0,0), vec2(32,32), tileInfo);

// Set tile data
layer.setData(vec2(10,10), new TileLayerData(1));
layer.getData(vec2(10,10));        // Get tile data
layer.redraw();                    // Redraw canvas
```

### Collision Layer
```javascript
// Create collision layer
const collisionLayer = new TileCollisionLayer(vec2(0,0), vec2(32,32));

// Set collision data (1 = solid, 0 = empty)
collisionLayer.setCollisionData(vec2(5,5), 1);

// Check collision
const hit = tileCollisionTest(pos, size);
```

## Particle System

### Creating Particle Effects
```javascript
// Create particle emitter
const explosion = new ParticleEmitter(
  pos,                             // Position
  0,                               // Angle
  1,                               // Emit size
  0.1,                             // Emit time
  500,                             // Emit rate
  PI,                              // Emit cone angle
  0,                               // Tile info
  RED,                             // Color
  WHITE,                           // Color 2
  1,                               // Particle time
  0.5,                             // Size start
  0,                               // Size end
  0.1,                             // Speed
  0.05,                            // Angular velocity
  0.99,                            // Damping
  1,                               // Angle start
  0,                               // Angle end
  0,                               // Gravity scale
  0.1,                             // Particle cone angle
  0.5,                             // Fade rate
  0.1,                             // Randomness
  false                            // Collision
);

// Emit particles manually
explosion.emitParticle();
```

## Physics & Collision

### Gravity
```javascript
gravity = vec2(0, -0.01);          // Set gravity (default 0,0)
```

### Collision Detection
```javascript
// Check overlap between two boxes
isOverlapping(posA, sizeA, posB, sizeB);

// Raycast
const hit = tileCollisionRaycast(start, end);

// Object raycast
const object = engineObjectsRaycast(start, end);
```

## Debugging

### Debug Drawing
```javascript
debugRect(pos, size, '#0f0', time=0);     // Draw debug rect
debugCircle(pos, radius, '#f00', time=0); // Draw debug circle
debugPoint(pos, '#00f');                  // Draw debug point
debugLine(posA, posB, '#fff');            // Draw debug line
debugText('Health: 100', pos);            // Draw debug text
```

### Debug Settings
```javascript
debug = true;                      // Enable debug mode
debugOverlay = true;               // Show debug overlay
debugWatermark = true;             // Show FPS counter
```

### Debug Controls
- **Escape** - Toggle debug overlay
- **1-9** - Toggle debug functions
- **+/-** - Adjust time scale

## Settings & Configuration

### Display Settings
```javascript
canvasMaxSize = vec2(1920, 1200); // Max canvas size
canvasFixedSize = vec2(0, 0);     // Fixed size (0,0 = auto)
canvasClearColor = BLACK;         // Background color
canvasPixelated = true;           // Pixelated scaling
glEnable = true;                  // Enable WebGL
```

### Tile Settings
```javascript
tileDefaultSize = vec2(16, 16);   // Default tile size
tileDefaultBleed = 0.3;           // Prevent tile bleeding
tilesPixelated = true;            // Crisp pixel art
```

### Physics Settings
```javascript
enablePhysicsSolver = true;       // Enable object collisions
objectDefaultMass = 1;            // Default mass
objectDefaultDamping = 1;         // Default damping
objectDefaultFriction = 0.8;      // Default friction
objectMaxSpeed = 1;               // Max speed limit
```

## Common Patterns

### Game Loop Structure
```javascript
let player;
let score = 0;

function gameInit() {
  // Initialize game
  player = new Player(vec2(0, 0));
  score = 0;
}

function gameUpdate() {
  // Update game logic
  if (player.pos.y < -10) {
    player.destroy();
    player = new Player(vec2(0, 0));
    score = 0;
  }
}

function gameRender() {
  // Custom rendering before objects
  drawText(`Score: ${score}`, cameraPos.add(vec2(0, 10)));
}
```

### Simple Enemy
```javascript
class Enemy extends EngineObject {
  constructor(pos) {
    super(pos, vec2(1, 1));
    this.color = RED;
    this.setCollision();
  }
  
  update() {
    super.update();
    
    // Simple AI: move toward player
    const direction = player.pos.subtract(this.pos).normalize();
    this.velocity = direction.scale(0.05);
  }
  
  collideWithObject(object) {
    if (object instanceof Player) {
      // Damage player
      object.health--;
    }
    return true;
  }
}
```

### Collectible Item
```javascript
class Coin extends EngineObject {
  constructor(pos) {
    super(pos, vec2(0.5, 0.5));
    this.color = YELLOW;
    this.renderOrder = 1;
  }
  
  update() {
    super.update();
    
    // Rotate
    this.angle += 0.05;
    
    // Check player collision
    if (isOverlapping(this.pos, this.size, 
                      player.pos, player.size)) {
      score += 10;
      this.destroy();
    }
  }
}
```

## Useful Snippets

### Screen Shake
```javascript
function screenShake(amount = 0.1) {
  cameraPos = cameraPos.add(randVec2(amount));
}
```

### Health Bar
```javascript
function drawHealthBar(pos, health, maxHealth) {
  const barSize = vec2(2, 0.2);
  const percent = health / maxHealth;
  
  // Background
  drawRect(pos, barSize, BLACK);
  
  // Health
  const healthSize = vec2(barSize.x * percent, barSize.y);
  drawRect(pos.add(vec2((1-percent) * barSize.x/2, 0)), 
           healthSize, RED);
}
```

### Simple Animation
```javascript
class AnimatedSprite extends EngineObject {
  update() {
    super.update();
    
    // 4 frame animation at 10 fps
    const frame = Math.floor(time * 10) % 4;
    this.tileInfo = tile(vec2(frame * 16, 0), vec2(16, 16));
  }
}
```

## References

- [LittleJS GitHub](https://github.com/KilledByAPixel/LittleJS) - Official repository
- [LittleJS Documentation](https://killedbyapixel.github.io/LittleJS/docs) - API documentation
- [LittleJS Demos](https://killedbyapixel.github.io/LittleJS/examples) - Example games
- [Particle System Designer](https://killedbyapixel.github.io/LittleJS/examples/particles) - Visual particle editor
- [ZzFX Sound Designer](https://killedbyapixel.github.io/ZzFX) - Create sound effects
- [Breakout Tutorial](https://github.com/KilledByAPixel/LittleJS/blob/main/examples/breakoutTutorial/README.md) - Step-by-step tutorial
- [LittleJS Discord](https://discord.gg/zb7hcGkyZe) - Community support

## Quick Reference Constants

```javascript
// Common colors
WHITE, BLACK, RED, GREEN, BLUE, YELLOW, CYAN, MAGENTA, GRAY

// Math constants
PI                                 // 3.14159...
```

---

*LittleJS Engine Copyright 2021 Frank Force*
