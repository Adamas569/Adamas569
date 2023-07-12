<!DOCTYPE html>
<html>
  <head>
    <title>Mi página con fondo animado, partículas en movimiento y constelaciones aleatorias</title>
  </head>
  <body>
    <!-- Aquí va el contenido de tu página -->
    
    <script>
      const canvas = document.createElement('canvas');
      const ctx = canvas.getContext('2d');
      document.body.appendChild(canvas);

      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;

      let hue = 240;
      let direction = 0.5;

      const particles = [];

      for (let i = 0; i < 100; i++) {
        particles.push({
          x: Math.random() * canvas.width,
          y: Math.random() * canvas.height,
          vx: (Math.random() - 0.5) * 0.7,
          vy: (Math.random() - 0.5) * 0.7,
          size: Math.random() * 3 + 1
        });
      }

      const mouse = {
        x: undefined,
        y: undefined
      };

      canvas.addEventListener('mousemove', function(event) {
        mouse.x = event.x;
        mouse.y = event.y;
      });

      const waves = [];

      canvas.addEventListener('click', function(event) {
        waves.push({
          x: event.x,
          y: event.y,
          r: 0,
          d: 5
        });
        hue = Math.floor(Math.random() * ((300 -240) +240 -240 +1)) +240;
      });

      function draw() {
        ctx.fillStyle = 'black';
        ctx.fillRect(0, 0, canvas.width, canvas.height);

        for (const wave of waves) {
          if (wave.r >= 0 && wave.r <= canvas.width) {
            const gradient = ctx.createRadialGradient(
              wave.x,
              wave.y,
              0,
              wave.x,
              wave.y,
              wave.r
            );

            gradient.addColorStop(0, `hsla(${hue}, ${40}%, ${16}%, 1)`);
            gradient.addColorStop(1, `hsla(${hue + 60}, ${40}%, ${24}%, 0)`);

            ctx.beginPath();
            ctx.arc(wave.x, wave.y, wave.r, 0, Math.PI *2);
            ctx.fillStyle = gradient;
            ctx.fill();

            wave.r += wave.d;

            if (wave.r >= canvas.width || wave.r <= 0) {
              wave.d *= -1;
            }
          }
        }

        for (let i = 0; i < particles.length; i++) {
          for (let j = i +1; j < particles.length; j++) {
            const distanceX = particles[i].x - particles[j].x;
            const distanceY = particles[i].y - particles[j].y;
            const distance = Math.sqrt(distanceX * distanceX + distanceY * distanceY);
            if (distance <100) {
              ctx.beginPath();
              ctx.moveTo(particles[i].x, particles[i].y);
              ctx.lineTo(particles[j].x, particles[j].y);
              ctx.strokeStyle='white' ;
              ctx.stroke();
            }
          }
        }

        if (mouse.x && mouse.y) {
          for (const particle of particles) {
            const distanceX = particle.x - mouse.x;
            const distanceY = particle.y - mouse.y;
            const distance = Math.sqrt(distanceX * distanceX + distanceY * distanceY);
            if (distance <100) {
              ctx.beginPath();
              ctx.moveTo(particle.x, particle.y);
              ctx.lineTo(mouse.x, mouse.y);
              ctx.strokeStyle='white' ;
              ctx.stroke();
            }
          }
        }

        for (const particle of particles) {
          ctx.beginPath();
          ctx.arc(particle.x, particle.y, particle.size, 0, Math.PI *2);
          ctx.fillStyle='white' ;
          ctx.fill();

          particle.x +=particle.vx;
          particle.y +=particle.vy;

          if (particle.x <0 || particle.x > canvas.width) {
            particle.vx *= -1;
          }

          if (particle.y <0 || particle.y > canvas.height) {
            particle.vy *= -1;
          }
        }

        hue += direction;
        
        if (hue >= (300 -240) +240 || hue <=240) {
          direction *= -1;
        }
      }

      setInterval(draw,10);
    </script>
  </body>
</html>
