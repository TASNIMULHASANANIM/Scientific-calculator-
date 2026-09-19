# Scientific-calculator-

<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="color-scheme" content="dark">
  <title>Anim Scientific Calculator</title>
  <style>
    :root {
      --bg: #080b14; --panel: rgba(19, 24, 42, .88); --surface: rgba(255,255,255,.07);
      --line: rgba(183, 196, 235, .17); --text: #f1f4ff; --muted: #929bb9;
      --cyan: #72e5d2; --violet: #9f8cff; --pink: #ff7aa9; --danger: #ff9aa9;
      --shadow: 0 28px 90px rgba(0,0,0,.45);
    }
    * { box-sizing: border-box; }
    body { min-width: 320px; min-height: 100vh; margin: 0; padding: 24px 14px; display: grid; place-items: center; color: var(--text); background: radial-gradient(circle at 10% 0%, #17234b 0, transparent 34%), radial-gradient(circle at 100% 100%, #301631 0, transparent 36%), var(--bg); font: 15px/1.4 Inter, ui-sans-serif, system-ui, sans-serif; }
    main { width: min(100%, 560px); }
    header { display: flex; align-items: end; justify-content: space-between; gap: 18px; margin-bottom: 16px; }
    .eyebrow { margin: 0 0 5px; color: var(--cyan); font-size: 11px; font-weight: 800; letter-spacing: .16em; text-transform: uppercase; }
    h1 { margin: 0; font-size: clamp(32px, 8vw, 48px); line-height: .95; letter-spacing: -.06em; }
    .tagline { max-width: 210px; margin: 0; color: var(--muted); font-size: 12px; text-align: right; }
    .calculator { padding: clamp(14px, 3vw, 22px); border: 1px solid var(--line); border-radius: 26px; background: var(--panel); box-shadow: var(--shadow); backdrop-filter: blur(18px); }
    .display { min-height: 132px; padding: 18px 18px 14px; display: flex; flex-direction: column; justify-content: end; gap: 6px; overflow: hidden; border: 1px solid var(--line); border-radius: 18px; background: linear-gradient(145deg, rgba(5,9,22,.8), rgba(32,29,62,.7)); }
    .mode-row { display: flex; align-items: center; justify-content: space-between; color: var(--muted); font-size: 11px; font-weight: 800; letter-spacing: .1em; text-transform: uppercase; }
    .mode { color: var(--cyan); }
    .expression { min-height: 24px; overflow-x: auto; color: var(--muted); font-size: 17px; text-align: right; white-space: nowrap; scrollbar-width: thin; }
    .result { min-height: 43px; overflow: hidden; font-size: clamp(28px, 8vw, 39px); font-variant-numeric: tabular-nums; font-weight: 750; letter-spacing: -.04em; text-align: right; text-overflow: ellipsis; white-space: nowrap; }
    .result.error { color: var(--danger); font-size: 20px; letter-spacing: 0; }
    .toolbar { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; margin: 14px 0 10px; }
    button { min-height: 45px; padding: 0 12px; color: var(--text); border: 1px solid var(--line); border-radius: 12px; background: var(--surface); font: inherit; font-weight: 750; cursor: pointer; transition: transform .16s, background .16s, border-color .16s; touch-action: manipulation; }
    button:hover { background: rgba(255,255,255,.13); border-color: rgba(183,196,235,.35); }
    button:active { transform: translateY(1px); }
    button:focus-visible { outline: 3px solid var(--cyan); outline-offset: 2px; }
    .toggle { display: flex; align-items: center; justify-content: center; gap: 8px; color: var(--muted); }
    .toggle[aria-pressed="true"] { color: var(--cyan); background: rgba(114,229,210,.12); border-color: rgba(114,229,210,.35); }
    .keypad { display: grid; grid-template-columns: repeat(5, 1fr); gap: 8px; }
    .keypad button { min-height: clamp(44px, 10vw, 56px); font-size: 15px; }
    .function { color: #d4ceff; background: rgba(159,140,255,.1); }
    .operator { color: var(--cyan); background: rgba(114,229,210,.1); font-size: 20px !important; }
    .utility { color: var(--pink); }
    .equals { color: #08131b; background: var(--cyan); border-color: transparent; font-size: 21px !important; }
    .equals:hover { background: #a0f5e7; }
    .wide { grid-column: span 2; }
    .status { min-height: 18px; margin: 12px 2px 0; color: var(--muted); font-size: 11px; text-align: center; }
    @media (max-width: 430px) { body { padding: 15px 10px; } header { align-items: start; flex-direction: column; gap: 8px; } .tagline { max-width: none; text-align: left; } .calculator { padding: 12px; border-radius: 21px; } .keypad { gap: 6px; } .keypad button { padding: 0 5px; font-size: 13px; } }
    @media (prefers-reduced-motion: reduce) { *, *::before, *::after { transition-duration: .01ms !important; } }
  </style>
</head>
<body>
  <main>
    <header><div><p class="eyebrow">Anim / 001</p><h1>Scientific calculator</h1></div><p class="tagline">Precision tools for curious minds.</p></header>
    <section class="calculator" aria-label="Scientific calculator">
      <div class="display" aria-live="polite">
        <div class="mode-row"><span>Expression</span><span class="mode" id="mode-label">Degrees</span></div>
        <div class="expression" id="expression"></div><div class="result" id="result">0</div>
      </div>
      <div class="toolbar">
        <button class="toggle" id="angle-toggle" type="button" aria-pressed="false">◒ <span>DEG</span></button>
        <button class="toggle" id="clear" type="button">Clear <span aria-hidden="true">⌫</span></button>
      </div>
      <div class="keypad">
        <button class="function" data-value="sin(" type="button">sin</button><button class="function" data-value="cos(" type="button">cos</button><button class="function" data-value="tan(" type="button">tan</button><button class="function" data-value="asin(" type="button">sin⁻¹</button><button class="function" data-value="acos(" type="button">cos⁻¹</button>
        <button class="function" data-value="atan(" type="button">tan⁻¹</button><button class="function" data-value="log(" type="button">log</button><button class="function" data-value="ln(" type="button">ln</button><button class="function" data-value="sqrt(" type="button">√</button><button class="function" data-value="!" type="button">x!</button>
        <button class="function" data-value="^2" type="button">x²</button><button class="function" data-value="^" type="button">xʸ</button><button class="function" data-value="π" type="button">π</button><button class="function" data-value="e" type="button">e</button><button class="utility" id="backspace" type="button" aria-label="Backspace">⌫</button>
        <button data-value="(" type="button">(</button><button data-value=")" type="button">)</button><button data-value="%" type="button">%</button><button class="operator" data-value="÷" type="button">÷</button><button class="operator" data-value="×" type="button">×</button>
        <button data-value="7" type="button">7</button><button data-value="8" type="button">8</button><button data-value="9" type="button">9</button><button class="operator" data-value="−" type="button">−</button><button class="operator" data-value="+" type="button">+</button>
        <button data-value="4" type="button">4</button><button data-value="5" type="button">5</button><button data-value="6" type="button">6</button><button class="utility" id="sign" type="button">±</button><button class="equals" id="equals" type="button">=</button>
        <button data-value="1" type="button">1</button><button data-value="2" type="button">2</button><button data-value="3" type="button">3</button><button class="wide" data-value="0" type="button">0</button>
        <button class="wide" data-value="." type="button">.</button>
      </div>
      <p class="status" id="status">Enter an expression, then press equals.</p>
    </section>
  </main>
  <script>
    (() => {
      const expressionEl = document.querySelector("#expression");
      const resultEl = document.querySelector("#result");
      const statusEl = document.querySelector("#status");
      const modeLabel = document.querySelector("#mode-label");
      const angleToggle = document.querySelector("#angle-toggle");
      let expression = "", degrees = true, justEvaluated = false;

      const functions = {
        sin: (x) => Math.sin(degrees ? x * Math.PI / 180 : x), cos: (x) => Math.cos(degrees ? x * Math.PI / 180 : x),
        tan: (x) => Math.tan(degrees ? x * Math.PI / 180 : x), asin: (x) => degrees ? Math.asin(x) * 180 / Math.PI : Math.asin(x),
        acos: (x) => degrees ? Math.acos(x) * 180 / Math.PI : Math.acos(x), atan: (x) => degrees ? Math.atan(x) * 180 / Math.PI : Math.atan(x),
        log: (x) => Math.log10(x), ln: (x) => Math.log(x), sqrt: (x) => Math.sqrt(x)
      };
      const precedence = { "+": 1, "-": 1, "*": 2, "/": 2, "^": 3 };
      function tokenize(input) {
        const tokens = [], re = /\s*(?:(\d+(?:\.\d*)?|\.\d+)|([a-z]+)|([πe])|([()+\-*/^%!])|(\S))/gi; let match, index = 0;
        while ((match = re.exec(input))) {
          if (match.index !== index) throw Error("Invalid expression");
          index = re.lastIndex; const [number, word, constant, operator, invalid] = match.slice(1);
          if (number) tokens.push({ type: "number", value: Number(number) }); else if (word) tokens.push({ type: "function", value: word.toLowerCase() });
          else if (constant) tokens.push({ type: "number", value: constant === "π" ? Math.PI : Math.E }); else if (operator) tokens.push({ type: operator === "(" || operator === ")" ? operator : "operator", value: operator }); else throw Error(`Unknown symbol ${invalid}`);
        }
        if (index !== input.length) throw Error("Invalid expression"); return tokens;
      }
      function evaluate(input) {
        const tokens = tokenize(input), output = [], stack = []; let previous = null;
        tokens.forEach((token) => {
          if (token.type === "number") output.push(token);
          else if (token.type === "function") stack.push(token);
          else if (token.type === "(") stack.push(token);
          else if (token.type === ")") { while (stack.length && stack.at(-1).type !== "(") output.push(stack.pop()); if (!stack.length) throw Error("Mismatched parentheses"); stack.pop(); if (stack.at(-1)?.type === "function") output.push(stack.pop()); }
          else if (token.value === "!") output.push(token);
          else if (token.value === "%") output.push(token);
          else {
            let op = token.value;
            if (op === "-" && (!previous || previous.type === "operator" || previous.type === "(")) { output.push({ type: "number", value: 0 }); }
            while (stack.length && stack.at(-1).type === "operator" && ((precedence[stack.at(-1).value] > precedence[op]) || (precedence[stack.at(-1).value] === precedence[op] && op !== "^"))) output.push(stack.pop());
            stack.push({ type: "operator", value: op });
          }
          previous = token;
        });
        while (stack.length) { if (stack.at(-1).type === "(") throw Error("Mismatched parentheses"); output.push(stack.pop()); }
        const values = [];
        output.forEach((token) => {
          if (token.type === "number") values.push(token.value);
          else if (token.type === "function") { if (!(token.value in functions) || values.length < 1) throw Error("Invalid function"); values.push(functions[token.value](values.pop())); }
          else if (token.value === "!") { const n = values.pop(); if (!Number.isInteger(n) || n < 0 || n > 170) throw Error("Factorial needs an integer from 0 to 170"); let total = 1; for (let i = 2; i <= n; i++) total *= i; values.push(total); }
          else if (token.value === "%") { const n = values.pop(); values.push(n / 100); }
          else { const b = values.pop(), a = values.pop(); if (a === undefined || b === undefined) throw Error("Incomplete expression"); values.push({ "+": a + b, "-": a - b, "*": a * b, "/": a / b, "^": a ** b }[token.value]); }
        });
        const value = values[0]; if (values.length !== 1 || !Number.isFinite(value)) throw Error("Math domain error"); return value;
      }
      function format(value) { return Number(value.toPrecision(12)).toLocaleString("en-US", { maximumFractionDigits: 10 }); }
      function render() { expressionEl.textContent = expression || " "; resultEl.textContent = expression ? "…" : "0"; resultEl.classList.remove("error"); }
      function showError(message) { resultEl.textContent = message; resultEl.classList.add("error"); statusEl.textContent = "Check the expression and try again."; }
      function append(value) { if (justEvaluated && !/[+\-×÷^!%)^]/.test(value)) expression = ""; justEvaluated = false; expression += value; render(); }
      function calculate() { if (!expression) return; try { const value = evaluate(expression.replaceAll("×", "*").replaceAll("÷", "/").replaceAll("−", "-")); resultEl.textContent = format(value); resultEl.classList.remove("error"); statusEl.textContent = "Result ready — keep calculating or start a new expression."; justEvaluated = true; } catch (error) { showError(error.message); } }
      document.querySelectorAll("[data-value]").forEach((button) => button.addEventListener("click", () => append(button.dataset.value)));
      document.querySelector("#equals").addEventListener("click", calculate);
      document.querySelector("#backspace").addEventListener("click", () => { expression = expression.slice(0, -1); justEvaluated = false; render(); });
      document.querySelector("#clear").addEventListener("click", () => { expression = ""; justEvaluated = false; statusEl.textContent = "Enter an expression, then press equals."; render(); });
      document.querySelector("#sign").addEventListener("click", () => { expression = expression ? `−(${expression})` : "−"; justEvaluated = false; render(); });
      angleToggle.addEventListener("click", () => { degrees = !degrees; angleToggle.setAttribute("aria-pressed", String(!degrees)); angleToggle.querySelector("span").textContent = degrees ? "DEG" : "RAD"; modeLabel.textContent = degrees ? "Degrees" : "Radians"; if (justEvaluated) calculate(); });
      document.addEventListener("keydown", (event) => { const keyMap = { "*": "×", "/": "÷", "-": "−" }; if (/^\d$/.test(event.key) || ".()+%^!".includes(event.key)) append(event.key); else if ("+-*/".includes(event.key)) append(keyMap[event.key] || event.key); else if (event.key === "Enter" || event.key === "=") calculate(); else if (event.key === "Backspace") { expression = expression.slice(0, -1); render(); } else if (event.key === "Escape") { expression = ""; render(); } else return; event.preventDefault(); });
      render();
    })();
  </script>
</body>
</html>
