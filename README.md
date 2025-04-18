<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>FrenzyMusic AI Composer</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/tone/14.8.35/Tone.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
  <style>
    body {
      margin: 0;
      font-family: 'Inter', sans-serif;
      background: linear-gradient(135deg, #1e3c72, #2a5298);
      color: #fff;
      display: flex;
      flex-direction: column;
      align-items: center;
      min-height: 100vh;
      padding: 40px 20px;
      overflow-x: hidden;
    }

    h1 {
      font-size: 3.2rem;
      margin-bottom: 40px;
      font-weight: 700;
      text-align: center;
    }

    .composer-card {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 16px;
      padding: 30px;
      max-width: 400px;
      width: 100%;
      box-shadow: 0 8px 30px rgba(0,0,0,0.3);
      text-align: center;
    }

    label {
      font-size: 1.1rem;
      margin: 12px 0 6px;
      display: block;
      font-weight: 600;
    }

    select, input[type="number"], input[type="checkbox"] {
      padding: 12px;
      border: none;
      border-radius: 8px;
      font-size: 1rem;
      margin: 8px 0 18px;
      width: 100%;
      background: rgba(255, 255, 255, 0.1);
      color: #fff;
    }

    select:focus, input[type="number"]:focus {
      outline: none;
      box-shadow: 0 0 0 3px rgba(255, 255, 255, 0.3);
    }

    button {
      padding: 15px 30px;
      font-size: 1rem;
      border: none;
      border-radius: 50px;
      background: #ff8c00;
      color: #fff;
      cursor: pointer;
      transition: all 0.4s ease;
      margin: 10px;
      font-weight: 600;
    }

    button:hover {
      background: #ffa733;
      transform: translateY(-2px);
      box-shadow: 0 5px 15px rgba(0,0,0,0.3);
    }

    button:disabled {
      background: #888;
      cursor: not-allowed;
    }

    .logo {
      width: 300px;
      margin-bottom: 20px;
    }

    footer {
      margin-top: 40px;
      font-size: 0.9rem;
      opacity: 0.6;
    }

  </style>
</head>
<body>

  <img src="https://i.postimg.cc/9Q5CmRpR/IMG-20250416-WA0005-01-Picsart-Ai-Image-Enhancer-jpeg.png" alt="FrenzyMusic Logo" class="logo" />
  
  <h1>🎶 FrenzyMusic AI Composer</h1>

  <div class="composer-card">
    <label>Genre</label>
    <select id="genre">
      <option value="classical">Classical</option>
      <option value="jazz">Jazz</option>
      <option value="electronic">Electronic</option>
    </select>

    <label>Tempo (BPM)</label>
    <input type="number" id="tempo" value="120" />

    <label>Mood</label>
    <select id="mood">
      <option value="happy">Happy</option>
      <option value="sad">Sad</option>
      <option value="dark">Dark</option>
    </select>

    <label>
      <input type="checkbox" id="loop" checked /> Loop
    </label>

    <button id="startBtn" onclick="startMusic()">▶ Start Music</button>
    <button id="stopBtn" onclick="stopMusic()" disabled>■ Stop Music</button>
  </div>

  <footer>
    Thank you 
  </footer>

  <script>
    let synth, part;

    function startMusic() {
      const startBtn = document.getElementById("startBtn");
      const stopBtn = document.getElementById("stopBtn");

      startBtn.textContent = "🎵 Playing...";
      startBtn.disabled = true;
      stopBtn.disabled = false;

      const genre = document.getElementById("genre").value;
      const tempo = parseInt(document.getElementById("tempo").value);
      const mood = document.getElementById("mood").value;
      const loop = document.getElementById("loop").checked;

      Tone.Transport.bpm.value = tempo;
      Tone.Transport.stop();
      Tone.Transport.cancel();

      if (part) part.dispose();
      if (synth) synth.dispose();

      // Select instrument by genre
      if (genre === "classical") {
        synth = new Tone.Synth().toDestination();
      } else if (genre === "jazz") {
        synth = new Tone.AMSynth().toDestination();
      } else if (genre === "electronic") {
        synth = new Tone.FMSynth().toDestination();
      }

      const notes = generateMelody(mood);

      part = new Tone.Part((time, note) => {
        synth.triggerAttackRelease(note, "8n", time);
      }, notes).start(0);

      part.loop = loop;
      part.loopEnd = `${notes.length * 0.5}s`;

      Tone.Transport.start();
    }

    function stopMusic() {
      const startBtn = document.getElementById("startBtn");
      const stopBtn = document.getElementById("stopBtn");

      Tone.Transport.stop();

      startBtn.textContent = "▶ Start Music";
      startBtn.disabled = false;
      stopBtn.disabled = true;
    }

    function generateMelody(mood) {
      let scale;
      if (mood === "happy") {
        scale = ["C4", "D4", "E4", "G4", "A4"];
      } else if (mood === "sad") {
        scale = ["C4", "D4", "Eb4", "G4", "Ab4"];
      } else {
        scale = ["C4", "Db4", "E4", "F4", "G4"];
      }

      const melody = [];
      for (let i = 0; i < 16; i++) {
        const note = scale[Math.floor(Math.random() * scale.length)];
        melody.push([i * 0.5, note]);
      }
      return melody;
    }
  </script>

</body>
</html>
