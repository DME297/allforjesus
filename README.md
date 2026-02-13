<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Valentine's Postcard</title>
  <style>
    body {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
      background-color: #ffe6e6;
      font-family: "Times New Roman", serif;
    }

    .postcard-container {
      position: relative;
      width: 400px;
      height: 600px;
    }

    canvas {
      width: 400px;
      height: 600px;
      border-radius: 15px;
      display: block;
    }

    .overlay-text {
      position: absolute;
      top: 70px;           /* title near top */
      left: 0;
      right: 0;
      bottom: 60px;        /* leaves space for socials */
      display: flex;
      flex-direction: column;
      align-items: center;
      text-align: center;
      z-index: 2;
      color: #b30047;
    }

    .overlay-text .message {
      font-size: 28px;
      font-weight: bold;
      text-decoration: underline;
    }

    .overlay-text .quote {
      font-size: 26px;
      margin-top: 80px;     /* space below title */
      padding: 0 20px;      /* prevents touching edges */
      line-height: 1.4;
      word-wrap: break-word;
      color: #660022;
    }

    a.socials {
      position: absolute;
      bottom: 20px;
      width: 100%;
      text-align: center;
      font-size: 16px;
      font-weight: bold;
      color: #800000;
      z-index: 2;
      text-decoration: none;
    }

    a.socials:hover {
      text-decoration: underline;
      color: #b30047;
    }

    .button-container {
      position: fixed;
      bottom: 20px;
      right: 20px;
      display: flex;
      gap: 10px;
    }

    .btn {
      background-color: #ff1a75;
      color: white;
      border: none;
      padding: 12px 18px;
      font-size: 16px;
      font-weight: bold;
      border-radius: 8px;
      cursor: pointer;
      box-shadow: 2px 2px 6px rgba(0,0,0,0.3);
    }

    .btn:hover {
      background-color: #e60066;
    }
  </style>
</head>
<body>

<div class="postcard-container">
  <canvas id="postcardCanvas" width="400" height="600"></canvas>

  <div class="overlay-text">
    <div class="message">Happy Valentines Day From the All For Jesus Team ❤️</div>
    <div class="quote" id="quoteBox"></div>
  </div>

  <!-- Clickable socials -->
  <a href="https://www.instagram.com/all.forjesus33" target="_blank" class="socials">
    @all.forjesus33
  </a>
</div>

<div class="button-container">
  <button class="btn" onclick="downloadPostcard()">Download</button>
  <button class="btn" onclick="sharePostcard()">Share</button>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<script>
const quotes = [
  `Song of Solomon 8:6
Place me like a seal over your heart, like a seal on your arm; for love is as strong as death, its jealousy unyielding as the grave. It burns like blazing fire, like a mighty flame.`,

  `1 John 4:7-8
Dear friends, let us love one another, for love comes from God. Everyone who loves has been born of God and knows God. Whoever does not love does not know God, because God is love.`,

  `1 Corinthians 13:4-5 
Love is patient, love is kind. It does not envy, it does not boast, it is not proud. It does not dishonor others, it is not self-seeking, it is not easily angered, it keeps no record of wrongs.`,

  `Romans 12:9-10
Love must be sincere. Hate what is evil; cling to what is good. Be devoted to one another in love. Honor one another above yourselves.`
];

const randomQuote = quotes[Math.floor(Math.random() * quotes.length)];
document.getElementById("quoteBox").innerText = randomQuote;
const canvas = document.getElementById('postcardCanvas');
const ctx = canvas.getContext('2d');

// Diagonal stripes background
const stripeWidth = 30;
for (let x = -canvas.height; x < canvas.width; x += stripeWidth*2) {
  ctx.fillStyle = '#ff4d6d';
  ctx.beginPath();
  ctx.moveTo(x, 0);
  ctx.lineTo(x + stripeWidth, 0);
  ctx.lineTo(x + canvas.height + stripeWidth, canvas.height);
  ctx.lineTo(x + canvas.height, canvas.height);
  ctx.closePath();
  ctx.fill();

  ctx.fillStyle = '#ffffff';
  ctx.beginPath();
  ctx.moveTo(x + stripeWidth, 0);
  ctx.lineTo(x + stripeWidth*2, 0);
  ctx.lineTo(x + canvas.height + stripeWidth*2, canvas.height);
  ctx.lineTo(x + canvas.height + stripeWidth, canvas.height);
  ctx.closePath();
  ctx.fill();
}

// Hearts
function drawHeart(x, y, size, color){
  ctx.save();
  ctx.fillStyle = color;
  ctx.beginPath();
  const topCurveHeight = size * 0.3;
  ctx.moveTo(x, y + topCurveHeight);
  ctx.bezierCurveTo(
    x, y,
    x - size/2, y,
    x - size/2, y + topCurveHeight
  );
  ctx.bezierCurveTo(
    x - size/2, y + (size + topCurveHeight)/2,
    x, y + (size + topCurveHeight)/2,
    x, y + size
  );
  ctx.bezierCurveTo(
    x, y + (size + topCurveHeight)/2,
    x + size/2, y + (size + topCurveHeight)/2,
    x + size/2, y + topCurveHeight
  );
  ctx.bezierCurveTo(
    x + size/2, y,
    x, y,
    x, y + topCurveHeight
  );
  ctx.closePath();
  ctx.fill();
  ctx.restore();
}

// Multiple hearts
for(let i=0;i<35;i++){
  const x = Math.random() * canvas.width;
  const y = Math.random() * canvas.height;
  const size = 15 + Math.random()*20;
  drawHeart(x,y,size,'rgba(255,255,255,0.3)');
}

// Rose-like circles
for(let i=0;i<20;i++){
  const x = Math.random() * canvas.width;
  const y = Math.random() * canvas.height;
  const radius = 6 + Math.random()*10;
  ctx.strokeStyle = 'rgba(255,192,203,0.35)';
  ctx.lineWidth = 2;
  ctx.beginPath();
  ctx.arc(x, y, radius, 0, 2*Math.PI);
  ctx.stroke();
}

// Postmarks
for(let i=0;i<5;i++){
  const x = Math.random() * canvas.width;
  const y = Math.random() * (canvas.height-150)+150;
  ctx.strokeStyle = 'rgba(255,255,255,0.2)';
  ctx.lineWidth = 2;
  ctx.beginPath();
  ctx.arc(x, y, 15, 0, 2 * Math.PI);
  ctx.stroke();
}

// Double border
ctx.lineWidth = 10;
ctx.strokeStyle = '#ff1a75';
ctx.strokeRect(0,0,canvas.width,canvas.height);
ctx.lineWidth = 5;
ctx.strokeStyle = '#ff80b3';
ctx.strokeRect(10,10,canvas.width-20,canvas.height-20);

// Download
function downloadPostcard() {
  html2canvas(document.querySelector('.postcard-container')).then(canvas => {
    const link = document.createElement('a');
    link.download = 'valentines_postcard.png';
    link.href = canvas.toDataURL();
    link.click();
  });
}

// Share
function sharePostcard() {
  if (navigator.share) {
    navigator.share({
      title: 'Valentine\'s Postcard',
      text: 'Check out this Valentine\'s postcard!',
      url: window.location.href
    }).catch(err => { alert('Share failed: ' + err); });
  } else {
    prompt("Copy this link to share your postcard:", window.location.href);
  }
}
</script>

</body>
</html>
