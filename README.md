# ai-meme-forge
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>AI Meme Forge ⚡ 5-Second Meme Maker</title>
  <meta name="description" content="Free meme generator. Upload → text → download. $3 Pro HD."/>
  <link rel="icon" href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 100 100%22><text y=%22.9em%22 font-size=%2290%22>⚡</text></svg>">
  <link href="https://fonts.googleapis.com/css2?family=Impact&family=Inter:wght@400;700&display=swap" rel="stylesheet">
  <style>
    :root { --neon: #0f0; --dark: #111; }
    * { margin:0; padding:0; box-sizing:border-box; }
    body { font-family: 'Inter', sans-serif; background: var(--dark); color: #fff; text-align: center; padding: 20px; }
    h1 { font-family: 'Impact', sans-serif; color: var(--neon); text-shadow: 0 0 10px var(--neon); margin: 20px 0; }
    .container { max-width: 600px; margin: auto; }
    input, button { padding: 14px; margin: 8px; font-size: 1.1em; border: 2px solid var(--neon); border-radius: 8px; }
    input { background: #222; color: #fff; width: 48%; text-transform: uppercase; }
    button { background: transparent; color: var(--neon); cursor: pointer; font-weight: bold; transition: 0.3s; }
    button:hover { background: var(--neon); color: #000; }
    .pro { background: var(--neon); color: #000; }
    canvas { border: 3px solid var(--neon); margin: 20px auto; display: block; max-width: 100%; border-radius: 12px; }
    @media (max-width: 600px) { input { width: 100%; } }
  </style>
</head>
<body>
  <div class="container">
    <h1>AI MEME FORGE ⚡</h1>
    <p>Upload → Text → Download. <strong>Free preview. $3 Pro HD.</strong></p>
    
    <input type="file" id="upload" accept="image/*" />
    <br/>
    <input type="text" id="topText" placeholder="TOP TEXT" />
    <input type="text" id="bottomText" placeholder="BOTTOM TEXT" />
    <br/>
    <button onclick="generateMeme()">GENERATE</button>
    <button class="pro" onclick="proDownload()">PRO HD ($3)</button>
    
    <canvas id="canvas"></canvas>
    <p>Free: 600px + watermark | Pro: 4K + clean</p>
  </div>

  <script>
    const canvas = document.getElementById('canvas');
    const ctx = canvas.getContext('2d');
    let img = new Image();
    img.crossOrigin = "anonymous";

    document.getElementById('upload').onchange = (e) => {
      const file = e.target.files[0];
      if (!file) return;
      const reader = new FileReader();
      reader.onload = (ev) => {
        img.src = ev.target.result;
        img.onload = () => { resizeCanvas(); generateMeme(); };
      };
      reader.readAsDataURL(file);
    };

    function resizeCanvas() {
      const max = 600;
      const ratio = Math.min(max / img.width, max / img.height);
      canvas.width = img.width * ratio;
      canvas.height = img.height * ratio;
    }

    function generateMeme(watermark = true) {
      if (!img.src) return;
      resizeCanvas();
      ctx.drawImage(img, 0, 0, canvas.width, canvas.height);

      const top = document.getElementById('topText').value.toUpperCase();
      const bottom = document.getElementById('bottomText').value.toUpperCase();

      ctx.font = 'bold 48px Impact';
      ctx.fillStyle = 'white';
      ctx.strokeStyle = 'black';
      ctx.lineWidth = 4;
      ctx.textAlign = 'center';

      if (top) drawText(top, canvas.width/2, 60);
      if (bottom) drawText(bottom, canvas.width/2, canvas.height - 30);

      if (watermark) {
        ctx.font = '20px Inter';
        ctx.fillStyle = 'rgba(255,255,255,0.4)';
        ctx.fillText('AI Meme Forge', canvas.width - 90, canvas.height - 10);
      }
    }

    function drawText(text, x, y) {
      ctx.strokeText(text, x, y);
      ctx.fillText(text, x, y);
    }

    function proDownload() {
      generateMeme(false);
      setTimeout(() => {
        const link = canvas.toDataURL('image/png', 1.0);
        const a = document.createElement('a');
        a.href = link;
        a.download = 'meme-pro-4k.png';
        a.click();
        window.open('https://yourname.gumroad.com/l/memepro', '_blank');
        setTimeout(() => generateMeme(true), 1000);
      }, 300);
    }

    ['topText', 'bottomText'].forEach(id => {
      document.getElementById(id).addEventListener('input', () => generateMeme());
    });
  </script>
</body>
</html>