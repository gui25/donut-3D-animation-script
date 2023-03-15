# Donut 3D animation script
Features:

Responsive design: Automatically adapts to the user's screen size.

Lightweight: No external libraries or dependencies required.

Smooth animation: High-quality rotation effect using HTML5 Canvas. 

Usage:

Copy and paste the provided script into your Devtools console in any page.
Enjoy the 3D donut animation on your webpage!

Script : 

```
(function() {
  // Create and style the canvas element
  const canvas = document.createElement('canvas');
  canvas.id = 'canvasDonut';
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
  canvas.style.position = 'fixed';
  canvas.style.zIndex = 1000;
  canvas.style.top = '0';
  canvas.style.left = '0';

  document.body.innerHTML = '';

  // Append the canvas element to the body
  document.body.appendChild(canvas);

  // Update canvas size on window resize
  window.addEventListener('resize', function() {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
  });

  // Animation variables
  let angleA = 1;
  let angleB = 1;
  const outerRadius = 1;
  const innerRadius = 2;
  const distanceFactor1 = 150;
  const distanceFactor2 = 5;
  let animationInterval;

  // Render a single frame of the animation
  function renderFrame() {
    const ctx = canvas.getContext('2d');
    ctx.fillStyle = '#000';
    ctx.fillRect(0, 0, ctx.canvas.width, ctx.canvas.height);

    angleA += 0.07;
    angleB += 0.03;

    const cosA = Math.cos(angleA);
    const sinA = Math.sin(angleA);
    const cosB = Math.cos(angleB);
    const sinB = Math.sin(angleB);

    for (let j = 0; j < 6.28; j += 0.3) {
      const cosJ = Math.cos(j);
      const sinJ = Math.sin(j);

      for (let i = 0; i < 6.28; i += 0.1) {
        const sinI = Math.sin(i);
        const cosI = Math.cos(i);
        const offsetX = innerRadius + outerRadius * cosJ;
        const offsetY = outerRadius * sinJ;
        const x = offsetX * (cosB * cosI + sinA * sinB * sinI) - offsetY * cosA * sinB;
        const y = offsetX * (sinB * cosI - sinA * cosB * sinI) + offsetY * cosA * cosB;
        const depthFactor = 1 / (distanceFactor2 + cosA * offsetX * sinI + sinA * offsetY);
        const screenX = (canvas.width / 2 + distanceFactor1 * depthFactor * x);
        const screenY = (canvas.height / 2 - distanceFactor1 * depthFactor * y);
        const lightIntensity = 0.7 * (cosI * cosJ * sinB - cosA * cosJ * sinI - sinA * sinJ + cosB * (cosA * sinJ - cosJ * sinA * sinI));

        if (lightIntensity > 0) {
          ctx.fillStyle = `rgba(255,255,255,${lightIntensity})`;
          ctx.fillRect(screenX, screenY, 1.5, 1.5);
        }
      }
    }
  }

  // Toggle the animation on and off
  function toggleAnimation() {
    if (animationInterval === undefined) {
      animationInterval = setInterval(renderFrame, 50);
    } else {
      clearInterval(animationInterval);
      animationInterval = undefined;
    }
  }

  // Start the animation on the canvas
  toggleAnimation();
})();
```
