# Phaser Cheatsheet

## Overview
Phaser is a fast, free, and fun 2D game framework for HTML5 games, supporting Canvas and WebGL.

## Quick Start
```bash
npm install phaser
```

```javascript
import Phaser from 'phaser';

const config = {
  type: Phaser.AUTO,
  width: 800,
  height: 600,
  scene: { preload, create }
};
new Phaser.Game(config);
```

## Common Tasks
- Load assets: `scene.load.image('key', 'url')`
- Add Sprite: `scene.add.sprite(x, y, 'key')`
- Input: `scene.input.on('pointerdown', callback)`

## Useful Snippets
```javascript
// Add text to the game
scene.add.text(100, 100, 'Hello Phaser!', { fill: '#0f0' });
```

## References
- [Phaser Documentation](https://phaser.io/docs/)
- [Phaser GitHub](https://github.com/phaserjs/phaser)