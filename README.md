<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Heart Animation</title>
    <style>
        html, body {
            height: 100%;
            margin: 0;
            overflow: hidden;
            background-color: #000;
        }
        canvas {
            position: absolute;
            width: 100%;
            height: 100%;
        }
    </style>
</head>
<body>
    <canvas id="heartCanvas"></canvas>
    <script>
        (function () {
            var canvas = document.getElementById('heartCanvas');
            var ctx = canvas.getContext('2d');
            var width = canvas.width = window.innerWidth;
            var height = canvas.height = window.innerHeight;
            var particles = [];

            function Particle(x, y, color) {
                this.x = x;
                this.y = y;
                this.color = color;
                this.vx = Math.random() * 10 - 5;
                this.vy = Math.random() * 10 - 5;
            }

            Particle.prototype.update = function() {
                this.x += this.vx;
                this.y += this.vy;
                if (this.x < 0 || this.x > width) {
                    this.vx *= -1;
                }
                if (this.y < 0 || this.y > height) {
                    this.vy *= -1;
                }
            };

            Particle.prototype.draw = function() {
                ctx.beginPath();
                ctx.arc(this.x, this.y, 2, 0, Math.PI * 2);
                ctx.closePath();
                ctx.fillStyle = this.color;
                ctx.fill();
            };

            function createParticles(x, y, num, color) {
                for (var i = 0; i < num; i++) {
                    var p = new Particle(x, y, color);
                    particles.push(p);
                }
            }

            function render() {
                ctx.clearRect(0, 0, width, height);
                particles.forEach(function(p) {
                    p.update();
                    p.draw();
                });
                requestAnimationFrame(render);
            }

            function init() {
                createParticles(width / 2, height / 2, 1000, '#ff85a2');
                render();
            }

            window.addEventListener('resize', function() {
                width = canvas.width = window.innerWidth;
                height = canvas.height = window.innerHeight;
                particles = [];
                init();
            });

            init();
        })();
    </script>
</body>
</html>
