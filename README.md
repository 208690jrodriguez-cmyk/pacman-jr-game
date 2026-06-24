# Pac-Man Jr 🎮

A classic Pac-Man Jr arcade game built with vanilla JavaScript, HTML5, and CSS3.

## Features

✨ **Game Features:**
- 🟡 **Pac-Man Character** - Animated mouth and smooth movement
- 👻 **4 Ghosts** - Red, Pink, Cyan, and Orange with independent AI
- 🍪 **Pellets** - Collect all pellets to advance to the next level
- 🎯 **Score System** - Earn 10 points per pellet, 100 bonus points per level
- 📊 **Level Progression** - Levels increase as you complete mazes
- ⌨️ **Smooth Controls** - Arrow keys or WASD to move

## How to Play

1. **Open the game** - Open `index.html` in your web browser
2. **Use Arrow Keys or WASD** to move Pac-Man around the maze
3. **Eat pellets** to increase your score
4. **Avoid ghosts** - Get caught and the game resets!
5. **Complete the level** - Eat all pellets to advance
6. **Press SPACE** to pause/resume

## Game Controls

| Action | Key |
|--------|-----|
| Move Up | ↑ Arrow / W |
| Move Down | ↓ Arrow / S |
| Move Left | ← Arrow / A |
| Move Right | → Arrow / D |
| Pause/Resume | SPACE |

## Project Structure

```
pacman-jr-game/
├── index.html      # Game HTML structure
├── style.css       # Game styling and layout
├── game.js         # Core game logic
└── README.md       # This file
```

## Game Mechanics

### Pac-Man
- Starts at the top-left area of the maze
- Can move in four directions (up, down, left, right)
- Collects pellets to increase score
- Game resets if caught by a ghost

### Ghosts
- 4 ghosts with different colors spawn in the center
- Move randomly through the maze
- Change direction when hitting a wall
- Game ends if they catch Pac-Man

### Maze
- 20x15 tile grid
- Blue walls block movement
- Open spaces for movement and pellet collection

### Scoring
- **+10 points** - For each pellet eaten
- **+100 points** - Bonus for completing a level
- **Level Counter** - Increases with each completed level

## Future Enhancements 🚀

- [ ] Power-ups (temporary ghost invulnerability)
- [ ] Sound effects and background music
- [ ] High score persistence (localStorage)
- [ ] Difficulty levels
- [ ] Improved ghost AI (chase patterns)
- [ ] Mobile touch controls
- [ ] Multiple lives system
- [ ] Bonus fruits

## Technologies Used

- **HTML5** - Game structure and Canvas API
- **CSS3** - Styling and animations
- **Vanilla JavaScript** - Game logic and rendering

## Browser Compatibility

Works in all modern browsers that support:
- HTML5 Canvas
- ES6 JavaScript
- CSS3

## License

Feel free to use this project for learning and personal projects!

## Credits

Inspired by the classic Pac-Man arcade game series.

---

**Enjoy the game! 🎮👾**
