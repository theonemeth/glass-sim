# glass-sim
You can fill or unfill glasses with water. Soothing and relaxing game.
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>100 Glasses Simulation</title>
  <style>
    /* Basic page styling */
    body {
      font-family: sans-serif;
      background: #f0f0f0;
      margin: 20px;
    }
    h1 {
      text-align: center;
    }
    /* Grid container: 5 columns (and therefore 20 rows for 100 items) */
    .grid-container {
      display: grid;
      grid-template-columns: repeat(5, 1fr);
      gap: 20px;
      max-width: 700px;
      margin: 0 auto;
    }
    /* Each glass container centers its content */
    .glass-container {
      text-align: center;
    }
    /* The glass (cup) is drawn as a bordered rectangle */
    .glass {
      position: relative;
      width: 120px;
      height: 180px;
      border: 4px solid #555;
      border-radius: 10px;
      overflow: hidden;
      background: #fff;
      margin: 0 auto;
      transform-origin: bottom center;
    }
    /* The water element starts at 0% height and will grow upward */
    .water {
      position: absolute;
      bottom: 0;
      width: 100%;
      height: 0%;
      background: rgba(0, 150, 255, 0.7);
    }
    /* Controls (buttons) styling */
    .controls {
      margin-top: 8px;
    }
    button {
      margin: 2px;
      padding: 4px 8px;
      font-size: 14px;
      cursor: pointer;
    }
    /* A simple splash effect */
    .splash {
      position: absolute;
      background: rgba(0, 150, 255, 0.7);
      border-radius: 50%;
      pointer-events: none;
      animation: splash 0.6s ease-out forwards;
    }
    @keyframes splash {
      from {
        transform: scale(1);
        opacity: 1;
      }
      to {
        transform: scale(2);
        opacity: 0;
      }
    }
  </style>
</head>
<body>
  <h1>100 Glasses Simulation</h1>
  <div class="grid-container" id="grid-container"></div>

  <script>
    // Total number of glasses
    const numGlasses = 100;
    const gridContainer = document.getElementById('grid-container');

    // Create each glass with its controls
    for (let i = 0; i < numGlasses; i++) {
      // Create an outer container for each glass + buttons
      const glassContainer = document.createElement('div');
      glassContainer.classList.add('glass-container');

      // Create the glass element
      const glass = document.createElement('div');
      glass.classList.add('glass');

      // Create the water element inside the glass
      const water = document.createElement('div');
      water.classList.add('water');
      glass.appendChild(water);

      // Create a container for the buttons
      const controls = document.createElement('div');
      controls.classList.add('controls');

      // Create the "Fill" button and hook up the event
      const fillBtn = document.createElement('button');
      fillBtn.textContent = 'Fill';
      fillBtn.addEventListener('click', () => fillGlass(glass, water));

      // Create the "Unfill" button and hook up the event
      const unfillBtn = document.createElement('button');
      unfillBtn.textContent = 'Unfill';
      unfillBtn.addEventListener('click', () => unfillGlass(glass, water));

      // Append the buttons to the controls div
      controls.appendChild(fillBtn);
      controls.appendChild(unfillBtn);

      // Assemble the glass container
      glassContainer.appendChild(glass);
      glassContainer.appendChild(controls);
      gridContainer.appendChild(glassContainer);
    }

    // Function to animate the filling process
    function fillGlass(glass, water) {
      // Tilt the glass slightly (simulate pouring motion)
      glass.style.transition = 'transform 0.5s ease';
      glass.style.transform = 'rotate(-10deg)';
      
      // Animate water level rising from current to 100%
      let currentLevel = parseFloat(water.style.height) || 0;
      const targetLevel = 100; // in percent
      const step = 1; // percentage increase per frame
      
      function animateFill() {
        if (currentLevel < targetLevel) {
          currentLevel += step;
          water.style.height = currentLevel + '%';
          // Create a little splash effect
          createSplash(glass);
          requestAnimationFrame(animateFill);
        } else {
          // Once filled, reset the tilt
          glass.style.transform = 'rotate(0deg)';
        }
      }
      animateFill();
    }

    // Function to animate the un-filling process
    function unfillGlass(glass, water) {
      // Tilt the glass the other way (simulate pouring out)
      glass.style.transition = 'transform 0.5s ease';
      glass.style.transform = 'rotate(10deg)';
      
      // Animate water level decreasing from current to 0%
      let currentLevel = parseFloat(water.style.height) || 0;
      const targetLevel = 0;
      const step = 1; // percentage decrease per frame
      
      function animateUnfill() {
        if (currentLevel > targetLevel) {
          currentLevel -= step;
          water.style.height = currentLevel + '%';
          // Create a little splash effect
          createSplash(glass);
          requestAnimationFrame(animateUnfill);
        } else {
          // Once empty, reset the tilt
          glass.style.transform = 'rotate(0deg)';
        }
      }
      animateUnfill();
    }

    // Function to create a random splash animation
    function createSplash(glass) {
      const splash = document.createElement('div');
      splash.classList.add('splash');
      // Randomize the splash size and position within the glass
      const size = Math.random() * 10 + 5;
      splash.style.width = size + 'px';
      splash.style.height = size + 'px';
      splash.style.left = Math.random() * (glass.offsetWidth - size) + 'px';
      splash.style.top = Math.random() * (glass.offsetHeight - size) + 'px';
      glass.appendChild(splash);
      // Remove the splash element after the animation completes
      setTimeout(() => splash.remove(), 600);
    }
  </script>
</body>
</html>
