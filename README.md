# frequency-generator
<!DOCTYPE html>
<html>
<head>
    <title>Web Frequency Generator</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; margin-top: 50px; background: #f4f4f9; }
        .card { background: white; padding: 30px; border-radius: 10px; display: inline-block; box-shadow: 0px 4px 10px rgba(0,0,0,0.1); }
        input[type=range] { width: 300px; margin: 20px 0; }
        button { padding: 10px 20px; font-size: 16px; cursor: pointer; background: #ff4d4d; color: white; border: none; border-radius: 5px; }
    </style>
</head>
<body>
    <div class="card">
        <h2>Web Frequency Generator</h2>
        <p id="freqText">Frequency: 440 Hz</p>
        <input type="range" id="freqSlider" min="100" max="2000" value="440" oninput="updateFrequency(this.value)">
        <br>
        <button onclick="stopSound()">Stop Sound</button>
    </div>

    <script>
        let audioCtx = null;
        let oscillator = null;

        function startAudio(freq) {
            if (!audioCtx) {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            }
            if (oscillator) {
                oscillator.stop();
            }
            oscillator = audioCtx.createOscillator();
            oscillator.type = 'sine'; // Can be 'sine', 'square', 'triangle', 'sawtooth'
            oscillator.frequency.setValueAtTime(freq, audioCtx.currentTime);
            oscillator.connect(audioCtx.destination);
            oscillator.start();
        }

        function updateFrequency(val) {
            document.getElementById("freqText").innerText = "Frequency: " + val + " Hz";
            if (audioCtx && oscillator) {
                oscillator.frequency.setValueAtTime(val, audioCtx.currentTime);
            } else {
                startAudio(val);
            }
        }

        function stopSound() {
            if (oscillator) {
                oscillator.stop();
                oscillator = null;
            }
        }
    </script>
</body>
</html>
