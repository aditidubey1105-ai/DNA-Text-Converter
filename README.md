<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Text and DNA Converter</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f9fafc;
      color: #222;
      margin: 40px;
      line-height: 1.6;
    }
    h1 {
      color: #1d5fa7;
    }
    input[type="text"], textarea {
      width: 100%;
      padding: 10px;
      margin-top: 6px;
      margin-bottom: 12px;
      border: 1px solid #ccc;
      border-radius: 4px;
      font-size: 15px;
    }
    button {
      background-color: #1d5fa7;
      color: white;
      padding: 10px 16px;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      font-size: 15px;
    }
    button:hover {
      background-color: #164a80;
    }
    .mapping-box {
      display: flex;
      justify-content: space-between;
      max-width: 350px;
      margin-bottom: 20px;
    }
    .mapping-box div {
      display: flex;
      align-items: center;
    }
    .mapping-box label {
      width: 30px;
    }
  </style>
</head>
<body>
  <h1>Text ↔ DNA Converter</h1>
  <p>This tool converts English text to a DNA sequence and back. You can also change how each DNA base (A, T, G, C) is represented.</p>

  <div class="mapping-box">
    <div><label>A =</label><input id="A" type="text" value="00"></div>
    <div><label>T =</label><input id="T" type="text" value="01"></div>
    <div><label>G =</label><input id="G" type="text" value="10"></div>
    <div><label>C =</label><input id="C" type="text" value="11"></div>
  </div>

  <h3>Convert Text to DNA</h3>
  <input id="textInput" type="text" placeholder="Enter English text here">
  <button onclick="convertToDNA()">Convert to DNA</button>
  <textarea id="dnaOutput" placeholder="DNA output" readonly></textarea>

  <h3>Convert DNA to Text</h3>
  <input id="dnaInput" type="text" placeholder="Enter DNA sequence here (A, T, G, C)">
  <button onclick="convertToText()">Convert to Text</button>
  <textarea id="textOutput" placeholder="Text output" readonly></textarea>

  <script>
    function getMapping() {
      return {
        'A': document.getElementById('A').value || '00',
        'T': document.getElementById('T').value || '01',
        'G': document.getElementById('G').value || '10',
        'C': document.getElementById('C').value || '11'
      };
    }

    function invertMapping(map) {
      let inv = {};
      for (let key in map) inv[map[key]] = key;
      return inv;
    }

    function textToDNA(text) {
      const map = getMapping();
      let dna = '';
      for (let char of text) {
        let binary = char.charCodeAt(0).toString(2).padStart(8, '0');
        for (let i = 0; i < 8; i += 2) {
          let pair = binary.substring(i, i + 2);
          for (let base in map) {
            if (map[base] === pair) {
              dna += base;
              break;
            }
          }
        }
      }
      return dna;
    }

    function dnaToText(dna) {
      const map = getMapping();
      let bits = '';
      for (let base of dna.toUpperCase()) {
        bits += map[base] || '';
      }
      let text = '';
      for (let i = 0; i < bits.length; i += 8) {
        let byte = bits.substring(i, i + 8);
        if (byte.length === 8) text += String.fromCharCode(parseInt(byte, 2));
      }
      return text;
    }

    function convertToDNA() {
      const text = document.getElementById('textInput').value;
      document.getElementById('dnaOutput').value = textToDNA(text);
    }

    function convertToText() {
      const dna = document.getElementById('dnaInput').value;
      document.getElementById('textOutput').value = dnaToText(dna);
    }
  </script>
</body>
</html>
