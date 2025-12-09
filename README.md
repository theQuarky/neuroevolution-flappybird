# 🐦 Neuroevolution Flappy Bird

A demonstration of the **Neuroevolution** algorithm implemented from scratch, where AI learns to play Flappy Bird through genetic algorithms and neural networks.

[**🎮 Live Demo**](https://thequarky.github.io/neuroevolution-flappybird/)

![Flappy Bird AI](graphics/train.png)

## 📋 Table of Contents

- [Overview](#overview)
- [What is Neuroevolution?](#what-is-neuroevolution)
- [How It Works](#how-it-works)
  - [Neural Network Architecture](#neural-network-architecture)
  - [Genetic Algorithm](#genetic-algorithm)
  - [Training Process](#training-process)
- [Project Structure](#project-structure)
- [Algorithm Flow](#algorithm-flow)
- [Getting Started](#getting-started)
- [Technologies Used](#technologies-used)
- [Code Explanation](#code-explanation)
- [Credits](#credits)

## 🎯 Overview

This project demonstrates **neuroevolution** - a form of artificial intelligence that uses evolutionary algorithms to train neural networks. Instead of traditional backpropagation, the AI learns through natural selection: birds with better performance pass their "genes" (neural network weights) to the next generation.

**Key Features:**
- 🧬 Genetic Algorithm implementation from scratch
- 🧠 Custom Neural Network (no ML libraries)
- 🎮 Real-time visualization of 300 birds learning simultaneously
- 📊 Fitness-based selection and mutation
- 🚀 Pure JavaScript implementation using p5.js

## 🧬 What is Neuroevolution?

**Neuroevolution** combines two powerful concepts:

1. **Neural Networks** - Computational models inspired by biological brains
2. **Genetic Algorithms** - Optimization techniques inspired by natural selection

Instead of training a neural network with labeled data (supervised learning), neuroevolution evolves the network weights through generations, keeping the best performers and mutating them to explore new solutions.

### Why Neuroevolution for Flappy Bird?

- ✅ No training data required
- ✅ Works well for game AI and control problems
- ✅ Naturally explores different strategies
- ✅ Easy to understand and visualize
- ✅ Demonstrates evolutionary principles

## 🔧 How It Works

### Neural Network Architecture

Each bird has its own neural network "brain" with the following structure:

```
Input Layer (5 neurons)          Hidden Layer (7 neurons)         Output Layer (1 neuron)
┌─────────────────┐             ┌──────────────────┐             ┌─────────────┐
│ Pipe X Position │────┐        │                  │             │             │
├─────────────────┤    │        │                  │             │  Jump/Don't │
│ Pipe Top Y      │────┼───────▶│   Hidden Layer   │────────────▶│    Jump     │
├─────────────────┤    │        │    (7 nodes)     │             │  (0.0-1.0)  │
│ Pipe Bottom Y   │────┤        │                  │             │             │
├─────────────────┤    │        │                  │             └─────────────┘
│ Bird Y Position │────┤        └──────────────────┘
├─────────────────┤    │
│ Bird Y Velocity │────┘
└─────────────────┘

Activation Function: Sigmoid
Decision Threshold: > 0.5 = Jump
```

**Input Neurons (5):**
1. `Pipe X Position` - Horizontal distance to nearest pipe
2. `Pipe Top Y` - Y coordinate of top pipe opening
3. `Pipe Bottom Y` - Y coordinate of bottom pipe opening  
4. `Bird Y Position` - Current vertical position of the bird
5. `Bird Y Velocity` - Current vertical velocity (gravity effect)

**Hidden Layer (7 neurons):**
- Processes the inputs through weighted connections
- Uses sigmoid activation function
- Learns complex patterns through evolution

**Output Layer (1 neuron):**
- Outputs a value between 0 and 1
- If output > 0.5, the bird jumps
- If output ≤ 0.5, the bird falls

### Genetic Algorithm

The genetic algorithm follows these steps:

```
Generation N                 Selection                 Mutation              Generation N+1
┌──────────┐                                                                ┌──────────┐
│ Bird 1   │──┐                                                             │ Bird 1   │
│ Bird 2   │  │           ┌──────────────┐         ┌──────────────┐       │ Bird 2   │
│ Bird 3   │  ├──────────▶│   Calculate  │────────▶│    Fitness   │       │ Bird 3   │
│   ...    │  │           │   Fitness    │         │   Selection  │       │   ...    │
│ Bird 300 │──┘           │   Scores     │         │   (Roulette) │       │ Bird 300 │
└──────────┘              └──────────────┘         └──────┬───────┘       └──────────┘
All birds die                                              │                New generation
when hitting                                               │                with mutated
obstacles                                                  ▼                networks
                                                   ┌──────────────┐
                                                   │   Mutation   │
                                                   │  (10% chance)│
                                                   │  Gaussian    │
                                                   │  Noise ±0.5  │
                                                   └──────────────┘
```

**1. Initialization (Generation 0):**
- Create 300 birds with random neural network weights
- Each bird starts at the same position

**2. Evaluation:**
- All birds play simultaneously
- Each bird uses its neural network to decide when to jump
- Birds gain score for each frame they survive
- Birds die when they hit pipes or boundaries

**3. Fitness Calculation:**
```javascript
fitness = bird.score / total_score_of_all_birds
```
- Birds that survive longer get higher fitness
- Fitness is normalized (sum = 1.0)

**4. Selection (Roulette Wheel):**
- Birds with higher fitness have higher chance of being selected
- Selection is probabilistic, not deterministic
- Even low-fitness birds have a small chance

**5. Mutation:**
```javascript
if (random(1) < 0.1) {  // 10% chance
    weight = weight + gaussian_noise * 0.5
}
```
- 10% chance each weight gets mutated
- Uses Gaussian (normal) distribution
- Allows exploration of new strategies

**6. Repeat:**
- New generation starts with mutated copies of selected birds
- Process repeats until convergence

### Training Process

**Generation 1-5:**
- Random flapping
- Most birds die immediately
- A few survive by chance

**Generation 5-20:**
- Birds learn basic timing
- Start recognizing pipe patterns
- Survival time increases

**Generation 20-50:**
- Refined strategies emerge
- Better pipe navigation
- High scores achieved

**Generation 50+:**
- Optimized behavior
- Consistent high performance
- Mastery of the game

## 📁 Project Structure

```
neuroevolution-flappybird/
│
├── index.html          # Main HTML file, loads all scripts
├── sketch.js           # Main game loop, p5.js setup and draw
├── bird.js             # Bird class with neural network integration
├── pipe.js             # Pipe (obstacle) class
├── nn.js               # Neural Network implementation
├── ga.js               # Genetic Algorithm (selection, mutation)
├── matrix.js           # Matrix operations for neural network
├── graphics/           # Image assets
│   ├── background.png
│   ├── train.png       # Bird sprite
│   └── pipe_*.png      # Pipe sprites
└── README.md          # This file
```

### File Descriptions

**`sketch.js`** - Main game controller
- Initializes 300 birds
- Manages game loop
- Updates pipes and birds
- Handles generation transitions
- Displays score

**`bird.js`** - Bird entity with AI
- Bird physics (gravity, jump)
- Neural network "brain"
- `think()` - Makes decisions using NN
- `mutate()` - Applies genetic mutations

**`nn.js`** - Neural Network implementation
- Forward propagation
- Sigmoid activation
- Matrix operations
- Weight mutation
- Network copying

**`ga.js`** - Genetic Algorithm
- Fitness calculation
- Roulette wheel selection
- Generation management

**`matrix.js`** - Matrix math utilities
- Matrix multiplication
- Element-wise operations
- Used by neural network

**`pipe.js`** - Obstacle management
- Pipe generation
- Collision detection
- Movement and rendering

## 🔄 Algorithm Flow

Here's how everything works together:

```
┌─────────────────────────────────────────────────────────────────┐
│                        GAME STARTS                               │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ▼
                   ┌─────────────────┐
                   │  Initialize 300 │
                   │  Birds with     │
                   │  Random NN      │
                   └────────┬────────┘
                            │
                            ▼
        ┌───────────────────────────────────────┐
        │         GAME LOOP (60 FPS)            │
        │  ┌─────────────────────────────────┐  │
        │  │  For each bird still alive:     │  │
        │  │  1. Get nearest pipe            │  │
        │  │  2. Create input array          │  │
        │  │  3. Run through neural network  │  │
        │  │  4. Decide: Jump or not?        │  │
        │  │  5. Update physics              │  │
        │  │  6. Check collisions            │  │
        │  │  7. Increment score             │  │
        │  └─────────────────────────────────┘  │
        │                                       │
        │  Update pipes (move left, add new)   │
        │  Render everything                    │
        └────────────┬──────────────────────────┘
                     │
                     ▼
              ┌──────────────┐      No
              │ All birds    │────────┐
              │ dead?        │        │
              └──────┬───────┘        │
                     │ Yes            │
                     ▼                │
         ┌────────────────────┐       │
         │  GENETIC ALGORITHM │       │
         │  ┌──────────────┐  │       │
         │  │1. Calculate  │  │       │
         │  │   Fitness    │  │       │
         │  └──────┬───────┘  │       │
         │         │          │       │
         │  ┌──────▼───────┐  │       │
         │  │2. Select     │  │       │
         │  │   Parents    │  │       │
         │  │   (Roulette) │  │       │
         │  └──────┬───────┘  │       │
         │         │          │       │
         │  ┌──────▼───────┐  │       │
         │  │3. Create 300 │  │       │
         │  │   Children   │  │       │
         │  └──────┬───────┘  │       │
         │         │          │       │
         │  ┌──────▼───────┐  │       │
         │  │4. Mutate     │  │       │
         │  │   Weights    │  │       │
         │  └──────┬───────┘  │       │
         │         │          │       │
         └─────────┼──────────┘       │
                   │                  │
                   └──────────────────┘
                   Back to Game Loop
```

## 🚀 Getting Started

### Prerequisites

- A modern web browser (Chrome, Firefox, Safari, Edge)
- No installation required!

### Running Locally

1. **Clone the repository:**
   ```bash
   git clone https://github.com/theQuarky/neuroevolution-flappybird.git
   cd neuroevolution-flappybird
   ```

2. **Open in browser:**
   ```bash
   # Option 1: Simple HTTP server with Python
   python -m http.server 8000
   
   # Option 2: Using Node.js
   npx http-server
   
   # Option 3: Just open index.html directly in browser
   open index.html  # macOS
   start index.html # Windows
   ```

3. **Watch the AI learn:**
   - Birds start with random behavior
   - Each generation gets better
   - Watch the score increase over generations

### Tweaking Parameters

Edit `sketch.js` to experiment:

```javascript
const TOTAL = 300;          // Number of birds per generation
let parallax = 0.8;         // Background scroll speed

// In bird.js:
this.gravity = 0.6;         // Gravity strength
this.lift = -10;            // Jump force

// Neural network size in bird.js:
new NeuralNetwork(5, 7, 1)  // Inputs, Hidden, Output

// Mutation rate in bird.js:
if (random(1) < 0.1) {      // 10% mutation chance
```

## 🛠️ Technologies Used

- **[p5.js](https://p5js.org/)** - Creative coding library for visualization
- **Vanilla JavaScript** - No frameworks, pure implementation
- **HTML5 Canvas** - Rendering engine
- **GitHub Pages** - Hosting

## 💻 Code Explanation

### Neural Network Forward Propagation

```javascript
predict(input_array) {
  // Convert input to matrix
  let inputs = Matrix.fromArray(input_array);
  
  // Hidden layer calculation
  let hidden = Matrix.multiply(this.weights_ih, inputs);
  hidden.add(this.bias_h);
  hidden.map(sigmoid);  // Activation
  
  // Output layer calculation
  let output = Matrix.multiply(this.weights_ho, hidden);
  output.add(this.bias_o);
  output.map(sigmoid);  // Activation
  
  return output.toArray();
}
```

### Bird's Decision Making

```javascript
think(pipes) {
  // Find closest pipe
  let closest = null;
  let record = Infinity;
  for (let i = 0; i < pipes.length; i++) {
    let diff = pipes[i].x - this.x;
    if (diff > 0 && diff < record) {
      record = diff;
      closest = pipes[i];
    }
  }
  
  if (closest != null) {
    // Create inputs (normalized 0-1)
    let inputs = [];
    inputs[0] = map(closest.x, this.x, width, 0, 1);
    inputs[1] = map(closest.top, 0, height, 0, 1);
    inputs[2] = map(closest.bottom, 0, height, 0, 1);
    inputs[3] = map(this.y, 0, height, 0, 1);
    inputs[4] = map(this.velocity, -5, 5, 0, 1);
    
    // Get decision from neural network
    let action = this.brain.predict(inputs);
    
    // Jump if output > 0.5
    if (action[0] > 0.5) {
      this.up();
    }
  }
}
```

### Genetic Algorithm Selection

```javascript
function pickOne() {
  var index = 0;
  var r = random(1);  // Random number 0-1
  
  // Roulette wheel selection
  while (r > 0) {
    r = r - savedBirds[index].fitness;
    index++;
  }
  index--;
  
  // Clone selected bird's brain
  let bird = savedBirds[index];
  let child = new Bird(bird.brain);
  
  // Apply mutation
  child.mutate();
  return child;
}
```

### Mutation Function

```javascript
function MyMutate(x) {
  if (random(1) < 0.1) {  // 10% chance
    let offset = randomGaussian() * 0.5;  // Gaussian noise
    let newx = x + offset;
    return newx;
  } else {
    return x;  // No change
  }
}
```

## 🎓 Learning Resources

To understand the concepts better:

- **Neural Networks**: [3Blue1Brown - Neural Networks](https://www.youtube.com/watch?v=aircAruvnKk)
- **Genetic Algorithms**: [Introduction to Genetic Algorithms](https://www.youtube.com/watch?v=9zfeTw-uFCw)
- **Neuroevolution**: [Coding Train - Neuroevolution](https://www.youtube.com/watch?v=c6y21FkaUqw)
- **p5.js**: [p5.js Documentation](https://p5js.org/reference/)

## 📊 Performance Notes

- **Population Size**: 300 birds per generation
- **Convergence**: Usually achieves good performance within 50 generations
- **Frame Rate**: 60 FPS with all 300 birds
- **Input Normalization**: All inputs scaled to 0-1 range
- **Mutation Rate**: 10% per weight
- **Mutation Magnitude**: Gaussian distribution with σ=0.5

## 🤝 Contributing

Contributions are welcome! Ideas for improvement:

- [ ] Add generation counter display
- [ ] Save/load best neural network
- [ ] Adjustable difficulty
- [ ] Different bird colors based on fitness
- [ ] Graph of fitness over generations
- [ ] Replay best performer

## 📝 Credits

- **Original Concept**: Based on Coding Train's Neuroevolution tutorial
- **Game Assets**: Custom sprites
- **Implementation**: Built from scratch to demonstrate neuroevolution
- **Inspiration**: Daniel Shiffman's educational content

## 📄 License

This project is open source and available for educational purposes.

---

**Made with 🧠 and 🧬 by [theQuarky](https://github.com/theQuarky)**

**Learn. Code. Evolve.** 🚀
