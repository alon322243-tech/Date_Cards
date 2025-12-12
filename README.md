# Date_Cards
<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <title>חפיסת קלפי דייטים</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    }

    body {
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      background: radial-gradient(circle at top, #ffdee9 0, #b5fffc 40%, #222 100%);
      color: #fff;
    }

    .app {
      max-width: 480px;
      width: 100%;
      padding: 24px 16px 32px;
    }

    .title {
      text-align: center;
      margin-bottom: 12px;
      font-size: 1.6rem;
      font-weight: 700;
    }

    .subtitle {
      text-align: center;
      font-size: 0.9rem;
      opacity: 0.85;
      margin-bottom: 24px;
    }

    .card-wrapper {
      perspective: 1200px;
      margin-bottom: 20px;
    }

    .card {
      background: rgba(5, 5, 10, 0.92);
      border-radius: 18px;
      padding: 32px 24px;
      text-align: center;
      min-height: 180px;
      display: flex;
      align-items: center;
      justify-content: center;
      box-shadow:
        0 18px 40px rgba(0, 0, 0, 0.45),
        0 0 0 1px rgba(255, 255, 255, 0.05);
      position: relative;
      transform-style: preserve-3d;
      transition:
        transform 0.4s ease,
        box-shadow 0.3s ease,
        background 0.3s ease;
    }

    .card.flipped {
      transform: rotateY(360deg) scale(1.02);
      box-shadow:
        0 22px 50px rgba(0, 0, 0, 0.6),
        0 0 0 1px rgba(255, 255, 255, 0.09);
      background: radial-gradient(circle at top, #2c2c3f, #050509);
    }

    .card::before {
      content: "";
      position: absolute;
      inset: 10px;
      border-radius: 14px;
      border: 1px dashed rgba(255, 255, 255, 0.14);
      pointer-events: none;
    }

    .card-content {
      position: relative;
      z-index: 1;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 8px;
      padding-inline: 8px;
    }

    .card-emoji {
      font-size: 2.6rem;
      margin-bottom: 4px;
      filter: drop-shadow(0 4px 10px rgba(0, 0, 0, 0.5));
    }

    .card-text {
      font-size: 1.7rem;
      font-weight: 600;
      letter-spacing: 0.03em;
      word-wrap: break-word;
    }

    .controls {
      display: flex;
      flex-direction: column;
      gap: 8px;
      align-items: center;
    }

    button {
      border: none;
      border-radius: 999px;
      padding: 12px 28px;
      font-size: 1rem;
      font-weight: 600;
      cursor: pointer;
      background: linear-gradient(135deg, #ff7a9b, #ffb86c);
      color: #1b1b1f;
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.35);
      transition:
        transform 0.12s ease,
        box-shadow 0.12s ease,
        opacity 0.25s ease;
    }

    button:hover {
      transform: translateY(-1px);
      box-shadow: 0 14px 30px rgba(0, 0, 0, 0.45);
    }

    button:active {
      transform: translateY(1px) scale(0.98);
      box-shadow: 0 6px 16px rgba(0, 0, 0, 0.4);
    }

    button.secondary {
      background: transparent;
      color: #fff;
      box-shadow: none;
      font-size: 0.85rem;
      opacity: 0.85;
    }

    .counter {
      font-size: 0.85rem;
      opacity: 0.85;
      margin-top: 4px;
    }

    @media (max-width: 480px) {
      .card {
        padding: 28px 18px;
        min-height: 160px;
      }
      .card-text {
        font-size: 1.5rem;
      }
      .card-emoji {
        font-size: 2.3rem;
      }
    }
  </style>
</head>
<body>
  <div class="app">
    <h1 class="title">חפיסת קלפי דייטים</h1>
    <p class="subtitle">כל קלף – נושא אחד. אתם מחליטים מה עושים איתו 💫</p>

    <div class="card-wrapper">
      <div id="card" class="card">
        <div class="card-content">
          <div id="cardEmoji" class="card-emoji">🎴</div>
          <div id="cardText" class="card-text">לחצו כדי לשלוף קלף</div>
        </div>
      </div>
    </div>

    <div class="controls">
      <button id="drawButton">שלוף קלף</button>
      <span id="counter" class="counter"></span>
      <button id="resetButton" class="secondary" style="display:none;">
        אפס את החפיסה
      </button>
    </div>
  </div>

  <script>
    const allCards = [
      "סגול",
      "כחול",
      "אדום",
      "שחור",
      "זהב",
      "רטוב",
      "יבש",
      "חושך",
      "אור",
      "הפוך",
      "30 שקלים",
      "ההפתעה",
      "שקט",
      "בלי טלפונים",
      "בחוץ",
      "בבית",
      "גשם",
      "שמש",
      "נוח",
      "כוכבים",
      "קפה",
      "יין",
      "מתוק",
      "חריף",
      "לילה",
      "בוקר",
      "מסע",
      "נוח",
      "מוזיקה",
      "צחוק",
      "ריח",
      "מגע",
      "תמונות",
      "כתיבה",
      "בישול",
      "טעימות",
      "סרט",
      "פופקורן",
      "יצירה",
      "ספונטני",
      "לבן",
      "ירוק",
      "מסיכה",
      "זיכרונות",
      "אני בוחר",
      "את בוחרת",
      "עם קורה",
      "דורון",
      "אלון",
      "לילה ארוך"
    ];

    const emojiMap = {
      "סגול": "💜",
      "כחול": "🔵",
      "אדום": "❤️",
      "שחור": "⚫",
      "זהב": "🏆",
      "רטוב": "💦",
      "יבש": "🌵",
      "חושך": "🌑",
      "אור": "💡",
      "הפוך": "🔁",
      "30 שקלים": "💸",
      "ההפתעה": "🎁",
      "שקט": "🤫",
      "בלי טלפונים": "📵",
      "בחוץ": "🌳",
      "בבית": "🏠",
      "גשם": "☔",
      "שמש": "☀️",
      "נוח": "🛋️",
      "כוכבים": "⭐",
      "קפה": "☕",
      "יין": "🍷",
      "מתוק": "🍰",
      "חריף": "🌶️",
      "לילה": "🌙",
      "בוקר": "🌅",
      "מסע": "🧳",
      "מוזיקה": "🎵",
      "צחוק": "😂",
      "ריח": "🌸",
      "מגע": "🤲",
      "תמונות": "📸",
      "כתיבה": "✍️",
      "בישול": "🍳",
      "טעימות": "🍽️",
      "סרט": "🎬",
      "פופקורן": "🍿",
      "יצירה": "🎨",
      "ספונטני": "🎲",
      "לבן": "⚪",
      "ירוק": "🌿",
      "מסיכה": "🎭",
      "זיכרונות": "🧠",
      "אני בוחר": "👉",
      "את בוחרת": "👈",
      "עם קורה": "🐶",
      "דורון": "💖",
      "אלון": "💙",
      "לילה ארוך": "🌌",
      "נגמרו הקלפים 💖": "🃏",
      "לחצו כדי לשלוף קלף": "🎴"
    };

    const STORAGE_KEY = "dateCardsDeck_v2";

    let remaining = [];

    const cardEl = document.getElementById("card");
    const cardTextEl = document.getElementById("cardText");
    const cardEmojiEl = document.getElementById("cardEmoji");
    const drawButton = document.getElementById("drawButton");
    const resetButton = document.getElementById("resetButton");
    const counterEl = document.getElementById("counter");

    function shuffle(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
      return array;
    }

    function canUseLocalStorage() {
      try {
        const testKey = "__test_ls__";
        localStorage.setItem(testKey, "1");
        localStorage.removeItem(testKey);
        return true;
      } catch (e) {
        console.warn("localStorage לא זמין (אולי גלישה בסתר / מגבלות דפדפן)");
        return false;
      }
    }

    const LS_ENABLED = canUseLocalStorage();

    function saveState(currentText) {
      if (!LS_ENABLED) return;
      try {
        const state = {
          remaining,
          current: currentText
        };
        localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
      } catch (e) {
        console.warn("לא הצלחנו לשמור ל-localStorage", e);
      }
    }

    function loadState() {
      if (!LS_ENABLED) return null;
      try {
        const raw = localStorage.getItem(STORAGE_KEY);
        if (!raw) return null;
        const parsed = JSON.parse(raw);
        if (!parsed || !Array.isArray(parsed.remaining)) return null;
        // נוודא שכל הקלפים תקינים (מחרוזות שקיימות בחפיסה)
        const cleaned = parsed.remaining.filter(
          (c) => typeof c === "string" && allCards.includes(c)
        );
        return {
          remaining: cleaned,
          current: typeof parsed.current === "string"
            ? parsed.current
            : "לחצו כדי לשלוף קלף"
        };
      } catch (e) {
        console.warn("בעיה בקריאת localStorage", e);
        return null;
      }
    }

    function updateCardDisplay(text) {
      cardTextEl.textContent = text;
      const emoji = emojiMap[text] || "✨";
      cardEmojiEl.textContent = emoji;
    }

    function updateUIAfterLoad() {
      if (remaining.length === 0) {
        updateCardDisplay("נגמרו הקלפים 💖");
        drawButton.textContent = "אין עוד קלפים";
        drawButton.disabled = true;
        resetButton.style.display = "inline-block";
        counterEl.textContent = "";
      } else {
        drawButton.textContent = "שלוף קלף";
        drawButton.disabled = false;
        resetButton.style.display = "none";
        counterEl.textContent = `נשארו ${remaining.length} קלפים בחפיסה`;
      }
    }

    function resetDeck() {
      remaining = shuffle([...allCards]);
      updateCardDisplay("לחצו כדי לשלוף קלף");
      drawButton.textContent = "שלוף קלף";
      drawButton.disabled = false;
      resetButton.style.display = "none";
      counterEl.textContent = `נשארו ${remaining.length} קלפים בחפיסה`;
      saveState(cardTextEl.textContent);
    }

    function drawCard() {
      if (remaining.length === 0) {
        updateCardDisplay("נגמרו הקלפים 💖");
        drawButton.textContent = "אין עוד קלפים";
        drawButton.disabled = true;
        resetButton.style.display = "inline-block";
        counterEl.textContent = "";
        saveState(cardTextEl.textContent);
        return;
      }

      cardEl.classList.remove("flipped");
      setTimeout(() => {
        const next = remaining.pop();
        updateCardDisplay(next);
        cardEl.classList.add("flipped");
        if (remaining.length > 0) {
          counterEl.textContent = `נשארו ${remaining.length} קלפים בחפיסה`;
        } else {
          counterEl.textContent = "";
          drawButton.textContent = "אין עוד קלפים";
          drawButton.disabled = true;
          resetButton.style.display = "inline-block";
        }
        saveState(cardTextEl.textContent);
      }, 80);
    }

    drawButton.addEventListener("click", drawCard);
    resetButton.addEventListener("click", resetDeck);

    (function init() {
      const saved = loadState();
      if (saved) {
        remaining = saved.remaining;
        updateCardDisplay(saved.current);
        updateUIAfterLoad();
      } else {
        resetDeck();
      }
    })();
  </script>
</body>
</html>