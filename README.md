<吃飽太閒做的網站，能幫到你我覺得很開心>
<html lang="zh-Hant">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>147測驗｜單一檔案版</title>

    <!-- 預先套用主題避免閃爍 -->
    <script>
      (function () {
        try {
          var saved = localStorage.getItem('theme');
          if (saved === 'dark' || (!saved && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
            document.documentElement.classList.add('preload-dark');
            document.documentElement.setAttribute('data-theme','dark');
          }
        } catch(e){}
      })();
    </script>

    <style>
      :root{
        --bg:#f0f4f8; --txt:#000; --card:#fff; --rule:#e9ecef; --muted:#666;
        --primary:#007bff; --primary-800:#0056b3; --danger:#dc3545;
        --green:#28a745; --green-800:#218838; --teal:#17a2b8; --teal-800:#138496;
        --table-bd:#ccc;

        /* 正解強調（淺色） */
        --ans-chip-bg:#e7f6ec; --ans-chip-fg:#136d3a;
        --ans-row-bg:#f5fbf7; --ans-row-bd:#cce8d5;
      }
      body.dark {
        --bg:#121212; --txt:#fff; --card:#1e1e1e; --rule:#333; --table-bd:#444; --muted:#aaa;

        /* 正解強調（深色） */
        --ans-chip-bg:#1f3a29; --ans-chip-fg:#b9f6ca;
        --ans-row-bg:#12301f; --ans-row-bd:#2c5a3f;
      }

      html, body { height: 100%; }
      body {
        font-family: Arial, sans-serif;
        margin: 0;
        padding: 40px;
        /* 基礎自適應字級（桌機普遍上限 24px） */
        font-size: clamp(16px, 1.1vw + 12px, 24px);
        background: var(--bg);
        color: var(--txt);
        transition: background-color .4s, color .4s;
      }

      #container {
        max-width: 1320px; margin: auto; background: var(--card); padding: 40px;
        border-radius: 20px; box-shadow: 0 0 10px rgba(0,0,0,.1);
        transition: background-color .4s, color .4s;
      }

      .hidden { display: none; }
      h1, h2 { text-align: center; font-size: 1.6em; margin: .2em 0 .6em; }
      #rules { background: var(--rule); padding: 16px; margin-bottom: 28px; border-radius: 10px; font-size: .95em; }

      .btn {
        background: var(--primary); color: #fff; border: none; padding: 14px 24px;
        margin: 8px; border-radius: 10px; cursor: pointer; font-size: .95em;
        transition: background-color .2s, transform .1s;
        white-space: nowrap;
      }
      .btn:hover { background: var(--primary-800); }
      #leaveBtn { background: var(--danger); }
      #openBankBtn { background: var(--teal); }
      #openBankBtn:hover { background: var(--teal-800); }

      /* 進度條 */
      #progress, #timer { font-weight: bold; font-size: 1em; }
      .progress-container { background: #ddd; height: 10px; border-radius: 5px; margin-top: 14px; overflow: hidden; }
      body.dark .progress-container { background: #555; }
      .progress-bar { height: 100%; width: 0; background: var(--primary); transition: width .6s ease; }

      .question { margin: 24px 0 12px; font-size: 1.2em; }
      .options label { display: block; margin-bottom: 12px; font-size: 1em; }

      /* 表格與響應式容器 */
      .table-responsive { width: 100%; overflow-x: auto; -webkit-overflow-scrolling: touch; }
      table { width: 100%; border-collapse: collapse; margin-top: 16px; font-size: 1em; min-width: 460px; }
      th, td {
        border: 1px solid var(--table-bd); padding: 12px 14px; text-align: left; vertical-align: top;
        color: var(--txt); word-break: break-word;
      }

      /* 錯題列色（深色模式改亮字） */
      tr.wrong { background-color: #ffe6e6; }
      body.dark tr.wrong { background-color: #5a1a1a; }
      body.dark tr.wrong td { color: #fff; }

      /* 正解強調（提高優先度，避免被錯題底色吃掉） */
      .ans-chip { display:inline-block; padding:2px 8px; border-radius:999px; background: var(--ans-chip-bg); color: var(--ans-chip-fg); font-size:.9em; margin-right:6px; }
      .ans-cell  { background: var(--ans-row-bg) !important; border-left: 3px solid var(--ans-row-bd) !important; color: var(--txt) !important; }
      .opt-row   { display:block; padding:4px 6px; margin:2px 0; border-radius:6px; }
      .opt-row.is-correct { background: var(--ans-row-bg) !important; border-left: 3px solid var(--ans-row-bd) !important; color: var(--txt) !important; }
      tr.wrong td.ans-cell { background: var(--ans-row-bg) !important; color: var(--txt) !important; }

      /* 右上角按鈕固定 */
      #darkModeToggle, #homeBtn {
        position: fixed; right: 20px; padding: 10px 16px; border: none; border-radius: 8px;
        cursor: pointer; font-size: .95em; z-index: 2000; box-shadow: 0 4px 12px rgba(0,0,0,.15);
      }
      #darkModeToggle { top: 20px; background: #333; color: #fff; }
      #darkModeToggle:hover { background: #555; }
      #homeBtn { top: 68px; background: var(--green); color: #fff; text-decoration: none; }
      #homeBtn:hover { background: var(--green-800); }

      .mono { font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Courier New", monospace; }
      .muted { opacity: .8; color: var(--muted); }

      /* 小尺寸優化 */
      @media (max-width: 768px) {
        body { padding: 20px; font-size: clamp(14px, 2.2vw + 8px, 17px); }
        #container { padding: 20px; border-radius: 16px; }
        .btn { padding: 12px 16px; margin: 6px; font-size: .95em; }
        #darkModeToggle, #homeBtn { right: 12px; }
        #darkModeToggle { top: 12px; }
        #homeBtn { top: 56px; }
      }
      @media (max-width: 540px) {
        /* 手機上把「您的答案」隱藏，保留題目/正解/OX */
        #results table th:nth-child(2), #results table td:nth-child(2) { display:none; }
      }
      @media (max-width: 420px) {
        #bank .controls { display: grid; grid-template-columns: 1fr; gap: 8px; }
      }

      /* —— 大螢幕加碼放大（到 4K 前） —— */
      @media (min-width: 1200px) {
        body       { font-size: clamp(18px, 0.9vw + 12px, 26px); }
        #container { max-width: 1440px; }
        h1, h2     { font-size: 2rem; }
      }
      @media (min-width: 1800px) {
        body       { font-size: clamp(20px, 0.7vw + 12px, 28px); }
        #container { max-width: 1680px; }
        h1, h2     { font-size: 2.2rem; }
      }
      @media (min-width: 2400px) {
        body       { font-size: clamp(22px, 0.6vw + 14px, 32px); }
        #container { max-width: 1920px; }
        h1, h2     { font-size: 2.4rem; }
      }

      /* —— 2560×1440（含以上）專用上限 —— */
      @media (min-width: 2560px) and (min-height: 1400px) {
        body       { font-size: clamp(22px, 0.55vw + 16px, 34px); } /* 上限 34px */
        #container { max-width: 2000px; }
        h1, h2     { font-size: 2.6rem; }
      }

      /* 預載深色（head 腳本使用），load 後會移除 */
      .preload-dark body, .preload-dark { background:#121212; color:#fff; }
    </style>
  </head>
  <body>
    <button id="darkModeToggle" aria-pressed="false">深色模式 / Dark Mode</button>
    <a id="homeBtn" href="https://hisausage7.github.io/147test/" title="回首頁">🏠 回首頁</a>

    <div id="container">
      <!-- 歡迎頁 -->
      <div id="welcome">
        <h1>147測驗M15-期中考</h1>
        <div id="rules">
          <p><strong>考試注意事項 / Exam Rules:</strong></p>
          <p>1. 請輸入姓名後才能開始作答。 / You must enter your name to start the quiz.</p>
          <!-- 這行改成可自訂 -->
          <p>2. 考試限時可自訂（預設 80 分鐘,可設定至999分鐘），開始後自動倒數。 / You can set the time limit (default 80 minutes); countdown starts on begin.</p>
          <p>3. 作答途中可隨時點擊「離開考試」提前結束。 / You can click "Leave Quiz" anytime to finish early.</p>
          <p>4. 完成後會自動顯示所有答題結果與成績。 / Results and scores will be displayed after completion.</p>
          <p>5. 答對題目顯示O，答錯題目顯示X。 / Correct answers will show O, incorrect answers will show X.</p>
          <p>6. 測驗過程為亂序出題。 / The test process was chaotic and disordered in setting questions.</p>
          <p>!版權及源代碼所有-航機系008沈崑宸!</p>
          <p>!僅作為自我測驗使用!</p>
          <p>!此章節不含圖片題</p>
        </div>
        <input type="text" id="nameInput" placeholder="輸入姓名 / Enter your name"
               style="width:100%;padding:8px;margin-bottom:10px;font-size:1em;" />
        <input type="number" id="questionLimit" placeholder="輸入題數,至多282題 / Enter number of questions"
               style="width:100%;padding:8px;margin-bottom:10px;font-size:1em;" />
        <!-- 新增：作答時間（分鐘），預設 80，限制 1~300 -->
        <input type="number" id="durationInput" value="80" min="1" max="300"
               placeholder="作答時間（分鐘，預設 80,可設定置999分鐘） / Duration in minutes"
               style="width:100%;padding:8px;margin-bottom:10px;font-size:1em;" />
        <div style="text-align:center; display:flex; flex-wrap:wrap; justify-content:center;">
          <button id="startBtn" class="btn">開始測驗 / Start Quiz</button>
          <button id="openBankBtn" class="btn">題庫瀏覽 / Browse Bank</button>
        </div>
      </div>

      <!-- 測驗頁 -->
      <div id="quiz" class="hidden">
        <div style="display:flex; align-items:center; gap:8px;">
          <span id="welcomeName"></span>
          <span id="timer" style="margin-left:auto">80:00</span>
        </div>
        <div class="mono" id="progress">題數: <span id="current">0</span> / <span id="total">0</span></div>
        <div class="progress-container"><div id="progressBar" class="progress-bar"></div></div>
        <div class="question" id="questionText"></div>
        <div class="options" id="options"></div>
        <div style="display:flex;gap:10px;flex-wrap:wrap">
          <button id="prevBtn" class="btn" style="background:#6c757d">上一題 / Previous</button>
          <button id="leaveBtn" class="btn">離開考試 / Leave Quiz</button>
        </div>
      </div>

      <!-- 結果頁 -->
      <div id="results" class="hidden">
        <h2>測驗結果 / Results</h2>
        <div style="display:flex; flex-wrap:wrap; justify-content:center;">
          <button id="retryBtn" class="btn">重新開始 / Retry</button>
          <button id="showWrongBtn" class="btn" style="background:#ffc107">只看錯題 / Wrong Only</button>
          <button id="showAllBtn" class="btn">看全部結果 / Show All</button>
          <button id="toBankFromResults" class="btn" style="background:#17a2b8">題庫瀏覽 / Browse Bank</button>
        </div>
        <div class="table-responsive">
          <table>
            <thead>
              <tr><th>題目</th><th>您的答案</th><th>正確答案</th><th>結果</th></tr>
            </thead>
            <tbody id="resultsBody"></tbody>
          </table>
        </div>
        <div id="scoreSummary" style="text-align:center;margin-top:12px;font-size:1em"></div>
      </div>

      <!-- 題庫瀏覽頁 -->
      <div id="bank" class="hidden">
        <h2>題庫瀏覽 / Question Bank</h2>
        <div class="controls">
          <input id="bankSearch" type="text" placeholder="關鍵字搜尋（會搜尋題目與選項）" />
          <label>每頁顯示
            <select id="perPage">
              <option value="5">5</option>
              <option value="10" selected>10</option>
              <option value="20">20</option>
              <option value="50">50</option>
              <option value="all">全部</option>
            </select>
            題
          </label>
          <button id="backToWelcome" class="btn" style="padding:12px 18px">返回首頁 / Home</button>
        </div>
        <div class="table-responsive">
          <table>
            <thead>
              <tr>
                <th style="width:70px">#</th>
                <th>題目</th>
                <th style="width:34%">選項</th>
                <th style="width:160px">正確答案</th>
              </tr>
            </thead>
            <tbody id="bankBody"></tbody>
          </table>
        </div>
        <div class="pagination" style="display:flex;justify-content:center;align-items:center;gap:10px;margin-top:10px;">
          <button id="prevPage" class="btn" style="padding:10px 16px">上一頁</button>
          <span id="pageInfo" class="mono"></span>
          <button id="nextPage" class="btn" style="padding:10px 16px">下一頁</button>
        </div>
        <div style="text-align:center;margin-top:8px">
          <small class="muted">提示：可用右上角「深色模式」切換外觀；搜尋支援中英文與數字。</small>
        </div>
      </div>
    </div>
    

    <script>
            document.addEventListener("DOMContentLoaded", function () {
              const questions = [
  {
    question: "Newton's first law of motion states:",
    options: [
      "A. For every action there is an equal and opposite reaction.",
      "B. A body will continue to remain at rest or in uniform motion in a straight line unless acted upon by an external force.",
      "C. The rate of change of momentum of a body is proportional to the force applied to it."
    ],
    answer: "B"
  },
  {
    question: "The energy required to accelerate a large mass of air to a low final velocity is, in comparison to that required to accelerate a small mass of air to a high velocity is:",
    options: [
      "A. higher",
      "B. the same",
      "C. lower"
    ],
    answer: "C"
  },
  {
    question: "A turbo-jet engine gives:",
    options: [
      "A. A small acceleration to a large mass of air",
      "B. A large acceleration to a large mass of air",
      "C. A large acceleration to a small mass of air"
    ],
    answer: "C"
  },
  {
    question: "The Brayton Cycle is a:",
    options: [
      "A. A constant pressure cycle",
      "B. A constant volume cycle",
      "C. A constant temperature cycle"
    ],
    answer: "A"
  },
  {
    question: "Kinetic energy is attributed to:",
    options: [
      "A. Pressure",
      "B. Motion",
      "C. Altitude"
    ],
    answer: "B"
  },
  {
    question: "Power is defined as the rate of:",
    options: [
      "A. Application of a force",
      "B. Change in velocity",
      "C. Doing work"
    ],
    answer: "C"
  },
  {
    question: "Force is the product of mass and:",
    options: [
      "A. Velocity",
      "B. Acceleration",
      "C. Distance"
    ],
    answer: "B"
  },
  {
    question: "A turbo-prop engine gives:",
    options: [
      "A. A small acceleration to a large mass of air",
      "B. A large acceleration to a small mass of air",
      "C. A small acceleration to small mass of air"
    ],
    answer: "A"
  },
  {
    question: "A turbo-fan engine is:",
    options: [
      "A. An engine with a propeller",
      "B. A single spool engine",
      "C. A by-pass engine"
    ],
    answer: "C"
  },
  {
    question: "A turbo-shaft engine is:",
    options: [
      "A. An engine that drives something other than a propeller",
      "B. An engine that drives a propeller",
      "C. A free turbine engine"
    ],
    answer: "A"
  },
  {
    question: "The density of a gas may be expressed as:",
    options: ["A. Pressure/volume", "B. Volume/mass", "C. Mass/volume"],
    answer: "C"
  },
  {
    question: "Engine efficiencies may be compared using:",
    options: ["A. Thrust : weight ratio", "B. Specific fuel consumption", "C. Overall fuel consumption"],
    answer: "B"
  },
  {
    question: "A by-pass ratio of 3:1 means the by-pass airflow is:",
    options: ["A. Three times higher than the core flow", "B. A third of the core flow", "C. Twice the core flow"],
    answer: "A"
  },
  {
    question: "If the volume of a mass of air is 819 cubic feet at 273K, at 275K it is:",
    options: ["A. 2 cubic feet greater", "B. 4 cubic feet greater", "C. 6 cubic feet greater"],
    answer: "C"
  },
  {
    question: "Bernoulli's Theorem states that the total amount of energy in a gas flow:",
    options: ["A. Remains constant", "B. Increases", "C. Decreases"],
    answer: "A"
  },
  {
    question: "As the speed of an aircraft increases the momentum thrust of an engine will:",
    options: ["A. Increase", "B. Decrease", "C. Remain constant"],
    answer: "B"
  },
  {
    question: "Pressure thrust is developed at:",
    options: ["A. The combustion chamber inlet", "B. The exhaust unit outlet", "C. The propelling nozzle outlet"],
    answer: "C"
  },
  {
    question: "Net thrust is:",
    options: ["A. Momentum thrust plus pressure thrust", "B. Gross thrust minus momentum drag", "C. Gross thrust minus pressure thrust and momentum drag"],
    answer: "B"
  },
  {
    question: "As aircraft forward speed increases the propulsive efficiency of an engine:",
    options: ["A. Increases", "B. Decreases", "C. Does not change"],
    answer: "A"
  },
  {
    question: "As aircraft forward speed increases the specific fuel consumption of an engine:",
    options: ["A. Increases", "B. Decreases", "C. Does not change"],
    answer: "A"
  },
  {
    question: "Air passing through a pitot intake:",
    options: [
      "A. Decreases in velocity and increases in pressure",
      "B. Increases in velocity and decreases in pressure",
      "C. Decreases in velocity and pressure"
    ],
    answer: "A"
  },
  {
    question: "When air intake thermal anti-icing protection is switched ON:",
    options: [
      "A. The engine thrust and EGT are unaffected",
      "B. The engine thrust reduces and the EGT rises",
      "C. The engine thrust increases and the EGT rises"
    ],
    answer: "B"
  },
  {
    question: "Ram recovery is said to have occurred when the thrust increase it produces:",
    options: [
      "A. Compensates for the thrust lost to momentum drag",
      "B. Reaches the rated sea level thrust value",
      "C. Matches the net thrust"
    ],
    answer: "A"
  },
  {
    question: "The duct pressure efficiency of a turbo jet pitot intake is:",
    options: ["A. 80%", "B. 75%", "C. 98%"],
    answer: "C"
  },
  {
    question: "The ideal sub-sonic air intake is a:",
    options: [
      "A. Convergent/divergent intake",
      "B. Pitot intake",
      "C. Bell mouth intake"
    ],
    answer: "B"
  },
  {
    question: "During icing conditions an electrical intake anti-icing system would be selected to fast cycle when the air temperature was:",
    options: [
      "A. Between plus 10°C and minus 6°C",
      "B. Below minus 6°C",
      "C. Above 10°C"
    ],
    answer: "A"
  },
  {
    question: "The maximum permissible air velocity entering a gas turbine compressor is:",
    options: ["A. Mach 1", "B. Mach 0.4", "C. Mach 1.2"],
    answer: "B"
  },
  {
    question: "The effect of a normal shock wave forming at the lip of an intake is to:",
    options: [
      "A. Increase air velocity and drop air pressure and temperature",
      "B. Decrease air velocity and increase air pressure and temperature",
      "C. Decrease air velocity and drop air pressure and temperature"
    ],
    answer: "B"
  },
  {
    question: "The electrical heating elements on an intake heater mat operate:",
    options: ["A. Continuously", "B. Intermittently", "C. Part continuous, part intermittent"],
    answer: "C"
  },
  {
    question: "The intake 'ram ratio' is the ratio between:",
    options: [
      "A. Ambient temperature and inlet temperature",
      "B. Aircraft speed and compressor inlet air velocity",
      "C. Ambient pressure and inlet pressure"
    ],
    answer: "C"
  },
  {
    question: "The diffuser vanes in a centrifugal compressor:",
    options: [
      "A. Convert kinetic energy into pressure energy",
      "B. Convert pressure energy into kinetic energy",
      "C. Turn the airflow smoothly in the outlet elbows"
    ],
    answer: "A"
  },
  {
    question: "The rotating guide vanes in a centrifugal compressor:",
    options: [
      "A. Diffuse the inlet air before entry into the impeller",
      "B. Turn the airflow smoothly in the outlet elbows",
      "C. Guide the inlet air smoothly into the impeller"
    ],
    answer: "C"
  },
  {
    question: "The ducts formed between adjacent rotor blades and adjacent stator vanes are:",
    options: [
      "A. Both divergent",
      "B. Both convergent",
      "C. Divergent for the rotors and parallel for the stators"
    ],
    answer: "A"
  },
  {
    question: "The diffuser section in an axial flow engine:",
    options: [
      "A. Increases the air pressure and velocity",
      "B. Increases the air pressure and reduces its velocity",
      "C. Decreases the air pressure and increases its velocity"
    ],
    answer: "B"
  },
  {
    question: "If an axial flow compressor is running at constant RPM an increase in inlet air velocity will:",
    options: [
      "A. Increase the angle of attack of the first stage rotors",
      "B. Have no effect on the angle of attack of the rotors",
      "C. Decrease the angle of attack of the first stage rotors"
    ],
    answer: "C"
  },
  {
    question: "If the power lever is advanced too rapidly it may:",
    options: [
      "A. Produce a 'flame-out'",
      "B. Cause a stall and surge",
      "C. Overstress the engine"
    ],
    answer: "B"
  },
  {
    question: "As the outside air temperature raises the LP compressor RPM will:",
    options: [
      "A. Rise",
      "B. Reduce",
      "C. Not be affected"
    ],
    answer: "A"
  },
  {
    question: "The purpose of an axial compressor bleed valve is to:",
    options: [
      "A. Prevent 'choking' of the front stages by bleeding air from the forward stages",
      "B. Prevent choking of the intermediate stages by bleeding air from the rear stages",
      "C. Prevent choking of the rear stages by bleeding air from the intermediate stages"
    ],
    answer: "C"
  },
  {
    question: "In an axial flow compressor, compression takes place in:",
    options: [
      "A. The rotor stages only",
      "B. The rotor and the stator stages",
      "C. The stator stages only"
    ],
    answer: "B"
  },
  {
    question: "The cross sectional area of an axial flow compressor casing gradually reduces to:",
    options: [
      "A. Increase the air pressure against the decreasing air velocity",
      "B. Increase the air velocity against the rising air pressure",
      "C. Keep the air velocity constant"
        ],
        answer: "C"
  },
  {
    "question": "On an axial flow, dual compressor forward fan engine, the fan turns the same speed as the.",
    "options": ["A. low pressure turbine.", "B. high pressure compressor.", "C. forward turbine wheel."],
    "answer": "A"
  },
  {
    "question": "A turbo jet engine gives.",
    "options": ["A. large acceleration to a small mass of air.", "B. large acceleration to a large weight of air.", "C. small acceleration to a large mass of air."],
    "answer": "A"
  },
  {
    "question": "The basic gas turbine engine is divided into two main sections: the cold section and the hot section.",
    "options": ["A. The cold section includes the engine inlet, compressor, and turbine sections.", "B. The hot section includes the combustor, diffuser, and exhaust.", "C. The hot section includes the combustor, turbine, and exhaust."],
    "answer": "C"
  },
  {
    "question": "A jet engine derives its thrust by.",
    "options": ["A. drawing air into the compressor.", "B. impingement of the propelling gases on the outside air.", "C. reaction of the propelling gases."],
    "answer": "C"
  },
  {
    "question": "Which of the following might be used to identify turbine discharge pressure?",
    "options": ["A. Pt7.", "B. Pt2.", "C. Tt7."],
    "answer": "A"
  },
  {
    "question": "In a free turbine.",
    "options": ["A. there is a clutch between compressor and power output shaft.", "B. there is a direct drive with a free-wheel unit.", "C. there is no mechanical connection with the compressor."],
    "answer": "C"
  },
  {
    "question": "Bernoulli's Theorem states that at any point in a flow of gas.",
    "options": ["A. the static pressure and dynamic pressure are equal.", "B. the static pressure is less than the dynamic pressure.", "C. the total energy remains constant."],
    "answer": "C"
  },
  {
    "question": "The working fluid of a gas turbine engine is.",
    "options": ["A. gasoline.", "B. kerosene.", "C. air."],
    "answer": "C"
  },
  {
    "question": "Which statements are true regarding aircraft engine propulsion?",
    "options": ["A. Turbojet and turbofan engines impart a relatively large amount of acceleration to a smaller mass of air.", "B. In modern turboprop engines, nearly 50 percent of the exhaust gas energy is extracted by turbines to drive the propeller and compressor with the rest providing exhaust thrust.", "C. An engine driven propeller imparts a relatively small amount of acceleration to a large mass of air."],
    "answer": "C"
  },
  {
    "question": "As subsonic air flows through a convergent nozzle the velocity.",
    "options": ["A. decreases.", "B. increases.", "C. remains constant."],
    "answer": "B"
  },
  {
    "question": "In a twin spool compressor system, the first stage turbine drives the.",
    "options": ["A. N2 compressor.", "B. N1 and N2 compressors.", "C. N1 compressor."],
    "answer": "A"
  },
  {
    "question": "At what point in an axial flow turbojet engine will the highest gas pressures occur?",
    "options": ["A. At the compressor outlet.", "B. At the turbine entrance.", "C. Within the burner section."],
    "answer": "A"
  },
  {
    "question": "Which of the following units are generally used to measure aircraft noise?",
    "options": ["A. Effective perceived noise decibels (E P N dB).", "B. Decibels (dB).", "C. Sound pressure."],
    "answer": "A"
  },
  {
    "question": "The diffuser section is located between.",
    "options": ["A. the burner section and the turbine section.", "B. station No. 7 and station No. 8.", "C. the compressor section and the burner section."],
    "answer": "C"
  },
  {
    "question": "If the LP shaft shears.",
    "options": ["A. turbine runaway occurs.", "B. compressor overspeed occurs.", "C. compressor underspeed occurs."],
    "answer": "A"
  },
  {
    "question": "The term Pt7 means.",
    "options": ["A. pressure and temperature at station No. 7.", "B. the total pressure at station No. 7.", "C. the total inlet pressure."],
    "answer": "B"
  },
  {
    "question": "What section provides proper mixing of the fuel and efficient burning of the gases?",
    "options": ["A. Diffuser section and combustion section.", "B. Combustion section and compressor section.", "C. Combustion section only."],
    "answer": "C"
  },
  {
    "question": "Of the following, which engine type would most likely have a noise suppression unit installed?",
    "options": ["A. Turboprop.", "B. Turbojet.", "C. Turboshaft."],
    "answer": "B"
  },
  {
    "question": "The pressure of supersonic air as it flows through a divergent nozzle.",
    "options": ["A. decreases.", "B. increases.", "C. is inversely proportional to the temperature."],
    "answer": "A"
  },
  {
    "question": "The symbol for designating the speed of a LP compressor in a twin spool engine is.",
    "options": ["A. N.", "B. NG.", "C. N1."],
    "answer": "C"
  },
  {
    "question": "A turbojet engine is smoother running than a piston engine because.",
    "options": ["A. the lubrication is better.", "B. it runs at a lower temperature.", "C. it has no reciprocating parts."],
    "answer": "C"
  },
  {
    "question": "A gas turbine engine comprises which three main sections?",
    "options": ["A. Compressor, diffuser, and stator.", "B. Turbine, compressor, and combustion.", "C. Turbine, combustion, and stator."],
    "answer": "B"
  },
  {
    "question": "When a volume of air is compressed.",
    "options": ["A. heat is gained.", "B. no heat is lost or gained.", "C. heat is lost."],
    "answer": "B"
  },
  {
    "question": "The pressure of subsonic air as it flows through a convergent nozzle.",
    "options": ["A. increases.", "B. remains constant.", "C. decreases."],
    "answer": "C"
  },
  {
    "question": "If a volume of a mass of air is 546 cubic feet at 273K, at 274K it will be.",
    "options": ["A. 2 cubic feet greater.", "B. 1/273 less by weight.", "C. 2 cubic feet smaller."],
    "answer": "A"
  },
  {
    "question": "In what section of a turbojet engine is the jet nozzle located?",
    "options": ["A. Exhaust.", "B. Turbine.", "C. Combustion."],
    "answer": "A"
  },
  {
    "question": "Newton's First Law of Motion, generally termed the Law of Inertia, states:",
    "options": ["A. To every action there is an equal and opposite reaction.", "B. Force is proportional to the product of mass and acceleration.", "C. Every body persists in its state of rest, or of motion in a straight line, unless acted upon by some outside force."],
    "answer": "C"
  },
  {
    "question": "A high bypass engine results in.",
    "options": ["A. overall slower airflow and greater propulsive efficiency.", "B. overall faster airflow.", "C. greater propulsive efficiency."],
    "answer": "A"
  },
  {
    "question": "Bernoulli's Theorem states that at any point in a flow of gas.",
    "options": ["A. the static pressure and dynamic pressure are equal.", "B. the static pressure is less than the dynamic pressure.", "C. the total energy remains constant."],
    "answer": "C"
  },
  {
    "question": "The Brayton cycle is known as the constant.",
    "options": ["A. temperature cycle.", "B. mass cycle.", "C. pressure cycle."],
    "answer": "C"
  },
  {
    "question": "In a choked nozzle, velocity increases, and.",
    "options": ["A. density decreases.", "B. pressure decreases.", "C. pressure increases."],
    "answer": "C"
  },
  {
    "question": "Using standard atmospheric conditions, the standard sea level temperature is.",
    "options": ["A. 29°C.", "B. 59°F.", "C. 59°C."],
    "answer": "B"
  },
  {
    "question": "Standard sea level pressure is.",
    "options": ["A. 29.92 inches Hg.", "B. 29.29 inches Hg.", "C. 29.00 inches Hg."],
    "answer": "A"
  },
  {
    "question": "The highest pressure in a gas turbine is.",
    "options": ["A. at the nozzle exit.", "B. at the burner exit.", "C. just after the last compressor stage but before the burner."],
    "answer": "C"
  },
  {
    "question": "The velocity of subsonic air as it flows through a convergent nozzle.",
    "options": ["A. remains constant.", "B. increases.", "C. decreases."],
    "answer": "B"
  },
  {
    "question": "A turboprop engine derives its thrust by.",
    "options": ["A. impingement of the prop-wash on the outside air.", "B. reaction of the prop-wash.", "C. reaction of the propulsion gases."],
    "answer": "B"
  },
  {
    "question": "Adiabatic compression is.",
    "options": ["A. an isothermal process.", "B. one where there is an increase in kinetic energy.", "C. one where there is no loss or gain of heat."],
    "answer": "C"
  },
  {
    "question": "In a ducted fan engine, the fan is driven by the.",
    "options": ["A. turbine.", "B. air passing over the compressor.", "C. accessory gearbox."],
    "answer": "A"
  },
  {
    "question": "A modular constructed gas turbine engine means that.",
    "options": ["A. all engines have a specific component layout.", "B. the engine is constructed by the vertical assembly technique.", "C. its major components can be removed and replaced without disturbing the rest of the engine."],
    "answer": "C"
  },
  {
    "question": "The accessory gearbox of a high bypass engine is.",
    "options": ["A. on the HP compressor housing.", "B. in the forward bearing housing.", "C. attached to the turbine casing."],
    "answer": "A"
  },
  {
    "question": "On a gas turbine engine, what is the fan driven by?",
    "options": ["A. I P turbine.", "B. LP turbine.", "C. H P turbine."],
    "answer": "B"
  },
  {
    "question": "Which law relates to the kinetic, pressure, and potential energy in a fluid flow?",
    "options": ["A. Bernoulli's theorem.", "B. Newton's laws.", "C. Charles's law."],
    "answer": "A"
  },
  {
    "question": "The density of gas may be expressed as.",
    "options": ["A. volume/weight.", "B. weight/volume.", "C. pressure/volume."],
    "answer": "B"
  },
  {
    "question": "E S HP is.",
    "options": ["A. Horsepower/efficiency.", "B. Shaft horse power + exhaust efflux.", "C. Power available at the turbine less the power required to drive the."],
    "answer": "B"
  },
  {
    "question": "A divergent duct will cause subsonic flow to decrease in.",
    "options": ["A. velocity, increase pressure.", "B. velocity, pressure remains constant.", "C. pressure, increase velocity."],
    "answer": "A"
  },
  {
    "question": "The Brayton cycle is.",
    "options": ["A. the name given to the intermittent cycling of an electrical de-icing system.", "B. the continuous combustion cycle taking place in a gas turbine engine.", "C. the constant velocity cycle taking place in a gas turbine engine."],
    "answer": "B"
  },
  {
    "question": "The purpose of a diffuser is to.",
    "options": ["A. increase the kinetic energy of the air.", "B. induce a swirl to the air prior to combustion.", "C. increase the static pressure of the air."],
    "answer": "C"
  },
  {
    "question": "On a triple spool engine, the first stage of turbines drive.",
    "options": ["A. the LP compressor.", "B. the HP compressor.", "C. the I P compressor."],
    "answer": "B"
  },
  {
    "question": "Ram effect is.",
    "options": ["A. the increase of dynamic pressure at the face of the compressor.", "B. conversion of static pressure to kinetic pressure at the face of the compressor.", "C. conversion of kinetic energy to pressure energy at the face of the compressor."],
    "answer": "C"
  },
  {
    "question": "Which of the following statements is true on a high bypass ratio turbofan?",
    "options": ["A. Both the compressor and combustion system are larger than their turbojet equivalent.", "B. The compressor assembly is larger and combustion chamber smaller than their turbojet equivalent.", "C. Both the compressor and combustion chamber are smaller than the turbojet equivalent."],
    "answer": "C"
  },
  {
    "question": "In the dual axial flow or twin spool compressor system with a free power turbine, Nf would be an indication of.",
    "options": ["A. turbine thrust indication.", "B. first stage compressor speed.", "C. free power turbine speed."],
    "answer": "C"
  },
  {
    "question": "A waisted drive shaft is primarily to.",
    "options": ["A. achieve dynamic balance.", "B. reduce weight.", "C. provide a fuse if the driven component is overloaded."],
    "answer": "C"
  },
  {
    "question": "The 'core engine' or 'gas generator' is made up of the following components:",
    "options": ["A. Inlet, compressor, combustion chamber, turbine, exhaust.", "B. Turbine, combustion chamber, compressor.", "C. Compressor, turbine, exhaust, propelling nozzle."],
    "answer": "B"
  },
  {
    "question": "The principle of jet propulsion is.",
    "options": ["A. the calorific value of fuel burnt is equal to aircraft.", "B. the interaction of fluids and gases.", "C. every action has a equal and opposite reaction."],
    "answer": "C"
  },
  {
    "question": "Boyle's law states that, at constant temperature, if a gas is compressed.",
    "options": ["A. its absolute pressure is proportional to its volume.", "B. its absolute temperature is proportional to it's volume.", "C. its absolute pressure is inversely proportional to its volume."],
    "answer": "C"
  },
  {
    "question": "What part of a jet engine has the most potential energy?",
    "options": ["A. Immediately after the combustion chamber.", "B. Just before the combustion chamber.", "C. Immediately after the HP compressor."],
    "answer": "B"
  },
  {
    question: "On an axial flow, dual compressor forward fan engine, the fan turns the same speed as the:",
    options: [
      "A. low pressure turbine.",
      "B. high pressure compressor.",
      "C. forward turbine wheel."
    ],
    answer: "A"
  },
  {
    question: "A turbo jet engine gives:",
    options: [
      "A. large acceleration to a small mass of air.",
      "B. large acceleration to a large weight of air.",
      "C. small acceleration to a large mass of air."
    ],
    answer: "A"
  },
  {
    question: "The basic gas turbine engine is divided into two main sections: the cold section and the hot section. The hot section includes:",
    options: [
      "A. the engine inlet, compressor, and turbine sections.",
      "B. the combustor, diffuser, and exhaust.",
      "C. the combustor, turbine, and exhaust."
    ],
    answer: "C"
  },
  {
    question: "A jet engine derives its thrust by:",
    options: [
      "A. drawing air into the compressor.",
      "B. impingement of the propelling gases on the outside air.",
      "C. reaction of the propelling gases."
    ],
    answer: "C"
  },
  {
    question: "Which of the following might be used to identify turbine discharge pressure?",
    options: [
      "A. Pt7.",
      "B. Pt2.",
      "C. Tt7."
    ],
    answer: "A"
  },
  {
    question: "In a free turbine:",
    options: [
      "A. there is a clutch between compressor and power output shaft.",
      "B. there is a direct drive with a free-wheel unit.",
      "C. there is no mechanical connection with the compressor."
    ],
    answer: "C"
  },
  {
    question: "Bernoulli's Theorem states that at any point in a flow of gas:",
    options: [
      "A. the static pressure and dynamic pressure are equal.",
      "B. the static pressure is less than the dynamic pressure.",
      "C. the total energy remains constant."
    ],
    answer: "C"
  },
  {
    question: "The working fluid of a gas turbine engine is:",
    options: [
      "A. gasoline.",
      "B. kerosene.",
      "C. air."
    ],
    answer: "C"
  },
  {
    question: "Which statements are true regarding aircraft engine propulsion?",
    options: [
      "A. Turbojet and turbofan engines impart a relatively large amount of acceleration to a smaller mass of air.",
      "B. In modern turboprop engines, nearly 50 percent of the exhaust gas energy is extracted by turbines to drive the propeller and compressor with the rest providing exhaust thrust.",
      "C. An engine driven propeller imparts a relatively small amount of acceleration to a large mass of air."
    ],
    answer: "C"
  },
  {
    question: "As subsonic air flows through a convergent nozzle the velocity:",
    options: [
      "A. decreases.",
      "B. increases.",
      "C. remains constant."
    ],
    answer: "B"
  },
  {
    question: "In a twin spool compressor system, the first stage turbine drives the:",
    options: [
      "A. N2 compressor.",
      "B. N1 and N2 compressors.",
      "C. N1 compressor."
    ],
    answer: "A"
  },
  {
    question: "At what point in an axial flow turbojet engine will the highest gas pressures occur?",
    options: [
      "A. At the compressor outlet.",
      "B. At the turbine entrance.",
      "C. Within the burner section."
    ],
    answer: "A"
  },
  {
    question: "Which of the following units are generally used to measure aircraft noise?",
    options: [
      "A. Effective perceived noise decibels (E P N d B).",
      "B. Decibels (dB).",
      "C. Sound pressure."
    ],
    answer: "A"
  },
  {
    question: "The diffuser section is located between:",
    options: [
      "A. the burner section and the turbine section.",
      "B. station No. 7 and station No. 8.",
      "C. the compressor section and the burner section."
    ],
    answer: "C"
  },
  {
    question: "If the LP shaft shears:",
    options: [
      "A. turbine runaway occurs.",
      "B. compressor overspeed occurs.",
      "C. compressor underspeed occurs."
    ],
    answer: "A"
  },
  {
    question: "The term Pt7 means:",
    options: [
      "A. pressure and temperature at station No. 7.",
      "B. the total pressure at station No. 7.",
      "C. the total inlet pressure."
    ],
    answer: "B"
  },
  {
    question: "What section provides proper mixing of the fuel and efficient burning of the gases?",
    options: [
      "A. Diffuser section and combustion section.",
      "B. Combustion section and compressor section.",
      "C. Combustion section only."
    ],
    answer: "C"
  },
  {
    question: "Of the following, which engine type would most likely have a noise suppression unit installed?",
    options: [
      "A. Turboprop.",
      "B. Turbojet.",
      "C. Turboshaft."
    ],
    answer: "B"
  },
  {
    question: "The pressure of supersonic air as it flows through a divergent nozzle:",
    options: [
      "A. decreases.",
      "B. increases.",
      "C. is inversely proportional to the temperature."
    ],
    answer: "A"
  },
  {
    question: "The symbol for designating the speed of a LP compressor in a twin spool engine is:",
    options: [
      "A. N.",
      "B. NG.",
      "C. N1."
    ],
    answer: "C"
  },
  {
    question: "A turbojet engine is smoother running than a piston engine because:",
    options: [
      "A. the lubrication is better.",
      "B. it runs at a lower temperature.",
      "C. it has no reciprocating parts."
    ],
    answer: "C"
  },
  {
    question: "A gas turbine engine comprises which three main sections?",
    options: [
      "A. Compressor, diffuser, and stator.",
      "B. Turbine, compressor, and combustion.",
      "C. Turbine, combustion, and stator."
    ],
    answer: "B"
  },
  {
    question: "When a volume of air is compressed:",
    options: [
      "A. heat is gained.",
      "B. no heat is lost or gained.",
      "C. heat is lost."
    ],
    answer: "B"
  },
  {
    question: "The pressure of subsonic air as it flows through a convergent nozzle:",
    options: [
      "A. increases.",
      "B. remains constant.",
      "C. decreases."
    ],
    answer: "C"
  },
  {
    question: "If a volume of a mass of air is 546 cubic feet at 273K, at 274K it will be:",
    options: [
      "A. 2 cubic feet greater.",
      "B. 1/273 less by weight.",
      "C. 2 cubic feet smaller."
    ],
    answer: "A"
  },
  {
    question: "In what section of a turbojet engine is the jet nozzle located?",
    options: [
      "A. Exhaust.",
      "B. Turbine.",
      "C. Combustion."
    ],
    answer: "A"
  },
  {
    question: "Newton's First Law of Motion, generally termed the Law of Inertia, states:",
    options: [
      "A. To every action there is an equal and opposite reaction.",
      "B. Force is proportional to the product of mass and acceleration.",
      "C. Every body persists in its state of rest, or of motion in a straight line, unless acted upon by some outside force."
    ],
    answer: "C"
  },
  {
    question: "A high bypass engine results in:",
    options: [
      "A. overall slower airflow and greater propulsive efficiency.",
      "B. overall faster airflow.",
      "C. greater propulsive efficiency."
    ],
    answer: "A"
  },
  {
    question: "Bernoulli's Theorem states that at any point in a flow of gas:",
    options: [
      "A. the static pressure and dynamic pressure are equal.",
      "B. the static pressure is less than the dynamic pressure.",
      "C. the total energy remains constant."
    ],
    answer: "C"
  },
  {
    question: "The Brayton cycle is known as the constant:",
    options: [
      "A. temperature cycle.",
      "B. mass cycle.",
      "C. pressure cycle."
    ],
    answer: "C"
  },
  {
    question: "In a choked nozzle, velocity increases, and:",
    options: [
      "A. density decreases.",
      "B. pressure decreases.",
      "C. pressure increases."
    ],
    answer: "C"
  },
  {
    question: "Using standard atmospheric conditions, the standard sea level temperature is:",
    options: [
      "A. 29°C.",
      "B. 59°F.",
      "C. 59°C."
    ],
    answer: "B"
  },
  {
    question: "Standard sea level pressure is:",
    options: [
      "A. 29.92 inches Hg.",
      "B. 29.29 inches Hg.",
      "C. 29.00 inches Hg."
    ],
    answer: "A"
  },
  {
    question: "The highest pressure in a gas turbine is:",
    options: [
      "A. at the nozzle exit.",
      "B. at the burner exit.",
      "C. just after the last compressor stage but before the burner."
    ],
    answer: "C"
  },
  {
    question: "The velocity of subsonic air as it flows through a convergent nozzle:",
    options: [
      "A. remains constant.",
      "B. increases.",
      "C. decreases."
    ],
    answer: "B"
  },
  {
    question: "A turboprop engine derives its thrust by:",
    options: [
      "A. impingement of the prop-wash on the outside air.",
      "B. reaction of the prop-wash.",
      "C. reaction of the propulsion gases."
    ],
    answer: "B"
  },
  {
    question: "Adiabatic compression is:",
    options: [
      "A. an isothermal process.",
      "B. one where there is an increase in kinetic energy.",
      "C. one where there is no loss or gain of heat."
    ],
    answer: "C"
  },
  {
    question: "In a ducted fan engine, the fan is driven by the:",
    options: [
      "A. turbine.",
      "B. air passing over the compressor.",
      "C. accessory gearbox."
    ],
    answer: "A"
  },
  {
    question: "A modular constructed gas turbine engine means that:",
    options: [
      "A. all engines have a specific component layout.",
      "B. the engine is constructed by the vertical assembly technique.",
      "C. its major components can be removed and replaced without disturbing the rest of the engine."
    ],
    answer: "C"
  },
  {
    question: "The accessory gearbox of a high bypass engine is:",
    options: [
      "A. on the HP Compressor housing.",
      "B. in the forward bearing housing.",
      "C. attached to the turbine casing."
    ],
    answer: "A"
  },
  {
    question: "On a gas turbine engine, what is the fan driven by?",
    options: [
      "A. I P turbine.",
      "B. LP turbine.",
      "C. H P turbine."
    ],
    answer: "B"
  },
  {
    question: "Which law relates to the kinetic, pressure, and potential energy in a fluid flow?",
    options: [
      "A. Bernoulli's theorem.",
      "B. Newton's laws.",
      "C. Charles's law."
    ],
    answer: "A"
  },
  {
    question: "The density of gas may be expressed as:",
    options: [
      "A. volume/weight.",
      "B. weight/volume.",
      "C. pressure/volume."
    ],
    answer: "B"
  },
  {
    question: "E S HP is:",
    options: [
      "A. Horsepower/efficiency.",
      "B. Shaft horse power + exhaust efflux.",
      "C. Power available at the turbine less the power required to drive the."
    ],
    answer: "B"
  },
  {
    question: "A divergent duct will cause subsonic flow to decrease in:",
    options: [
      "A. velocity, increase pressure.",
      "B. velocity, pressure remains constant.",
      "C. pressure, increase velocity."
    ],
    answer: "A"
  },
  {
    question: "The Brayton cycle is:",
    options: [
      "A. the name given to the intermittent cycling of an electrical de-icing system.",
      "B. the continuous combustion cycle taking place in a gas turbine engine.",
      "C. the constant velocity cycle taking place in a gas turbine engine."
    ],
    answer: "B"
  },
  {
    question: "The purpose of a diffuser is to:",
    options: [
      "A. increase the kinetic energy of the air.",
      "B. induce a swirl to the air prior to combustion.",
      "C. increase the static pressure of the air."
    ],
    answer: "C"
  },
  {
    question: "On a triple spool engine, the first stage of turbines drive:",
    options: [
      "A. the LP compressor.",
      "B. the HP compressor.",
      "C. the I P compressor."
    ],
    answer: "B"
  },
  {
    question: "Ram effect is:",
    options: [
      "A. the increase of dynamic pressure at the face of the compressor.",
      "B. conversion of static pressure to kinetic pressure at the face of the compressor.",
      "C. conversion of kinetic energy to pressure energy at the face of the compressor."
    ],
    answer: "C"
  },
  {
    question: "Which of the following statements is true on a high bypass ratio turbofan?",
    options: [
      "A. Both the compressor and combustion system are larger than their turbojet equivalent.",
      "B. The compressor assembly is larger and combustion chamber smaller than their turbojet equivalent.",
      "C. Both the compressor and combustion chamber are smaller than their turbojet equivalent."
    ],
    answer: "C"
  },
  {
    question: "In the dual axial flow or twin spool compressor system with a free power turbine, Nf would be an indication of:",
    options: [
      "A. turbine thrust indication.",
      "B. first stage compressor speed.",
      "C. free power turbine speed."
    ],
    answer: "C"
  },
  {
    question: "A waisted drive shaft is primarily to:",
    options: [
      "A. achieve dynamic balance.",
      "B. reduce weight.",
      "C. provide a fuse if the driven component is overloaded."
    ],
    answer: "C"
  },
  {
    question: "The 'core engine' or 'gas generator' is made up of the following components:",
    options: [
      "A. Inlet, compressor, combustion chamber, turbine, exhaust.",
      "B. Turbine, combustion chamber, compressor.",
      "C. Compressor, turbine, exhaust, propelling nozzle."
    ],
    answer: "B"
  },
  {
    question: "The principle of jet propulsion is:",
    options: [
      "A. the calorific value of fuel burnt is equal to aircraft.",
      "B. the interaction of fluids and gases.",
      "C. every action has a equal and opposite reaction."
    ],
    answer: "C"
  },
  {
    question: "Boyle's law states that, at constant temperature, if a gas is compressed:",
    options: [
      "A. its absolute pressure is proportional to its volume.",
      "B. its absolute temperature is proportional to it's volume.",
      "C. its absolute pressure is inversely proportional to its volume."
    ],
    answer: "C"
  },
  {
    question: "What part of a jet engine has the most potential energy?",
    options: [
      "A. Immediately after the combustion chamber.",
      "B. Just before the combustion chamber.",
      "C. Immediately after the HP compressor."
    ],
    answer: "B"
  },
  {
    question: "If an electrical de-icing system is operating, thrust will.",
    options: ["A. decrease.", "B. remain constant.", "C. increase."],
    answer: "B"
  },
  {
    question: "A bellmouth compressor inlet is used on.",
    options: ["A. helicopters.", "B. supersonic aircraft.", "C. aircraft with low ground clearance."],
    answer: "A"
  },
  {
    question: "Electrical de-icing operates.",
    options: ["A. continuously and intermittently.", "B. cyclically independent of ambient air temperature.", "C. cyclically dependent on ambient air temperature."],
    answer: "A"
  },
  {
    question: "The inlet door on a variable geometry intake is open at.",
    options: ["A. idle speed.", "B. supersonic speeds.", "C. subsonic speeds."],
    answer: "C"
  },
  {
    question: "Anti-ice is recommended during.",
    options: ["A. OAT +10°C and visible moisture.", "B. thunderstorms.", "C. OAT below 10°C."],
    answer: "A"
  },
  {
    question: "A pitot intake is divergent from front to rear because it.",
    options: ["A. reduces ram compression.", "B. produces the maximum amount of ram compression.", "C. speeds up the air before it hits the compressor face."],
    answer: "B"
  },
  {
    question: "Anti icing of jet engine air inlets is commonly accomplished by.",
    options: ["A. electrical heating elements located within the engine air inlet cowling.", "B. electrical heating elements inside the inlet guide vanes.", "C. engine bleed air ducted through the critical areas."],
    answer: "C"
  },
  {
    question: "The term 'Ram Ratio' in regard to air intakes is the relationship between.",
    options: ["A. ambient pressure and ambient temperature.", "B. ambient temperature and compressor inlet temperature.", "C. ambient pressure and compressor inlet pressure."],
    answer: "C"
  },
  {
    question: "An increase in the Ram Ratio of an intake will.",
    options: ["A. have no effect upon the temperature of the air.", "B. increase the temperature of the air.", "C. decrease the temperature of the air."],
    answer: "C"
  },
  {
    question: "As an aircraft approaches the transonic range, the aerodynamic efficiency of a Pitot type intake.",
    options: ["A. increases due to the ram effect.", "B. decreases due to the shock wave.", "C. is not effected by forward speed."],
    answer: "B"
  },
  {
    question: "Inlet guide vanes are anti-iced with.",
    options: ["A. rubber boots.", "B. thermal blankets.", "C. engine bleed air."],
    answer: "C"
  },
  {
    question: "Intake air turbulence.",
    options: ["A. decreases the efficiency of the compressor.", "B. increases the efficiency of the compressor.", "C. has little effect on the efficiency of the compressor."],
    answer: "A"
  },
  {
    question: "What will be the effect of operating the intake anti-icing system of a gas turbine engine?",
    options: ["A. A decrease in power.", "B. Increased power at altitude.", "C. Increased power for take off."],
    answer: "A"
  },
  {
    question: "A Pitot intake is divergent from front to rear because it.",
    options: ["A. produces the maximum amount of ram compression.", "B. reduces ram compression and turbulence.", "C. speeds up the air before it hits the compressor face."],
    answer: "A"
  },
  {
    question: "With an electrical ice protection system, the heating elements operate.",
    options: ["A. continuously.", "B. part continuous - part intermittent.", "C. intermittently."],
    answer: "B"
  },
  {
    question: "The purpose of a bellmouth compressor inlet is to.",
    options: ["A. provide an increased ram air effect at low airspeeds.", "B. maximize the aerodynamic efficiency of the inlet.", "C. provide an increased pressure drop in the inlet."],
    answer: "B"
  },
  {
    question: "The vortex dissipators installed on some turbine-powered aircraft to prevent engine FOD utilize.",
    options: ["A. variable geometry inlet ducts.", "B. variable inlet guide vanes (IGV) and/or variable first stage fan blades.", "C. a stream of engine bleed air blown toward the ground ahead of the engine."],
    answer: "C"
  },
  {
    question: "Variable Ramp Intakes restrict airflow by.",
    options: ["A. diverting the airflow around the intake.", "B. reducing the area of the intake.", "C. creating shock-waves in the intake."],
    answer: "C"
  },
  {
    question: "The inlet door of a variable geometry intake at supersonic speeds will be.",
    options: ["A. closed.", "B. open.", "C. mid-Position."],
    answer: "A"
  },
  {
    question: "When operating an engine in icing conditions, care should be taken when the.",
    options: ["A. temperature is below +10°C with visible moisture.", "B. temperature is below 10°C.", "C. temperature is below 0°C."],
    answer: "A"
  },
  {
    question: "Anti-icing for a turboprop is achieved by.",
    options: ["A. bleed air supply from compressor.", "B. electric bonded heater mats.", "C. hot oil supply from lubrication system."],
    answer: "B"
  },
  {
    question: "A divergent intake is.",
    options: ["A. divergent from front to rear.", "B. convergent/divergent from front to rear.", "C. divergent/convergent from front to rear."],
    answer: "A"
  },
  {
    question: "What purpose does the nose cone serve on the (N1) fan on a high bypass engine?",
    options: ["A. Streamlined fairing.", "B. Reduce and straighten any turbulent air.", "C. Assist in diffusing airflow."],
    answer: "A"
  },
  {
    question: "A variable geometry intake at subsonic speeds.",
    options: ["A. jet pipe area is increased.", "B. throat area is decreased.", "C. throat area is increased."],
    answer: "C"
  },
  {
    question: "Electrical anti-ice.",
    options: ["A. heats oil which is distributed around engine.", "B. heats elements, placed under mats around engine.", "C. heats air which is distributed around engine."],
    answer: "B"
  },
  {
    question: "The cycling speed of the electrical de-icing mat.",
    options: ["A. comes in 4 speeds.", "B. is not affected by weather conditions.", "C. is affected by weather conditions."],
    answer: "C"
  },
  {
    question: "The variable inlet guide vanes are operated.",
    options: ["A. by fuel pressure.", "B. electrically from cockpit.", "C. using N1 fan speed."],
    answer: "A"
  },
  {
    question: "The intake of a gas turbine engine is designed to.",
    options: ["A. protect compressor from FOD.", "B. provide turbulent free air.", "C. provide streamlined fairing for aircraft."],
    answer: "B"
  },
  {
    question: "The velocity of air on entry to compressor inlet on an aircraft flying supersonic speed would be controlled at.",
    options: ["A. Mach 2.2.", "B. Mach 1.", "C. Mach 0.4."],
    answer: "C"
  },
  {
    question: "If an inlet is choked then the velocity.",
    options: ["A. increases and pressure decreases.", "B. increases and pressure increases.", "C. decreases and pressure increases."],
    answer: "C"
  },
  {
    question: "In an aircraft flying at supersonic speed, to reduce the air velocity at the compressor, the variable intake.",
    options: ["A. exhaust jet cone area increased.", "B. throat area is decreased.", "C. throat area is increased."],
    answer: "B"
  },
  {
    question: "A well designed intake will take advantage of forward speed by.",
    options: ["A. converting kinetic energy into pressure energy.", "B. converting velocity energy into kinetic energy.", "C. converting pressure energy of the air into kinetic energy."],
    answer: "A"
  },
  {
    question: "In subsonic multi-engine aircraft, a normal inlet duct will.",
    options: ["A. decrease and then increase in size, front to rear, along length of the duct.", "B. increase in size, front to rear, along length of the duct.", "C. increase and then decrease in size, front to rear, along length of the duct."],
    answer: "B"
  },
  {
    question: "What type of intake is one that decreases gradually in area and then increases?",
    options: ["A. Convergent.", "B. Convergent / Divergent.", "C. Divergent."],
    answer: "B"
  },
  {
    question: "In an electrical de-icing system, the main elements will be on.",
    options: ["A. intermittently, 8 times a minute, dependant on OAT.", "B. intermittently, 4 times a minute, dependant on OAT.", "C. continuously and intermittently."],
    answer: "C"
  },
  {
    question: "Intakes are designed to.",
    options: ["A. decrease the intake air pressure.", "B. decelerate the free air stream flow.", "C. accelerate the free air stream flow."],
    answer: "B"
  },
  {
    question: "The air intake for a gas turbine powered subsonic aircraft would be of.",
    options: ["A. convergent form.", "B. divergent form.", "C. convergent/divergent form."],
    answer: "B"
  },
  {
    question: "turboprop engine inlet anti-ice system operates.",
    options: ["A. continuously.", "B. cyclically dependant on weather conditions.", "C. cyclically independent on weather conditions."],
    answer: "A"
  },
  {
    question: "What is true for a bellmouth intake?",
    options: ["A. Pressure increases and velocity decreases.", "B. Velocity increases and pressure decreases.", "C. Pressure and velocity decrease."],
    answer: "B"
  },
  {
    question: "What is the system that breaks up ice formations on a turboprop engine nose cowl called?",
    options: ["A. Nose cowl heating.", "B. De-icing.", "C. Anti-icing."],
    answer: "B"
  },
  {
    question: "In a variable geometry intake, the velocity of the air on the engine compressor face is controlled by.",
    options: ["A. ramp and spill doors.", "B. intake augmentation doors.", "C. shock-wave pattern, ramp and spill doors."],
    answer: "C"
  },
  {
    question: "A bypass engine LP compressor.",
    options: [
      "A. supplies less air than is required for combustion.",
      "B. supplies more air than is required for combustion.",
      "C. supplies only the required quantity for combustion."
    ],
    answer: "B"
  },
  {
    question: "How does a dual axial flow compressor improve the efficiency of a turbojet engine?",
    options: [
      "A. The velocity of the air entering the combustion chamber is increased.",
      "B. More turbine wheels can be used.",
      "C. Higher compression ratios can be obtained."
    ],
    answer: "C"
  },
  {
    question: "In a reverse flow system, the last stage of an axial flow compressor is often centrifugal. This is to.",
    options: [
      "A. provide initial turning of the airflow.",
      "B. prevent compressor surge.",
      "C. increase the temperature rise."
    ],
    answer: "A"
  },
  {
    question: "What are the two main functional components in a centrifugal compressor?",
    options: ["A. Bucket and expander.", "B. Impeller and diffuser.", "C. Turbine and compressor."],
    answer: "B"
  },
  {
    question: "A bypass ratio of 5:1 indicates that the bypass flow is.",
    options: [
      "A. equal to 1/5 of the hot stream.",
      "B. five times the hot stream.",
      "C. five times the cold stream."
    ],
    answer: "B"
  },
  {
    question: "The stator vanes in an axial-flow compressor.",
    options: [
      "A. direct air into the first stage rotor vanes at the proper angle.",
      "B. convert velocity energy into pressure energy.",
      "C. convert pressure energy onto velocity energy."
    ],
    answer: "B"
  },
  {
    question: "What units in a gas turbine engine aid in stabilisation of the compressor during low thrust engine operations?",
    options: ["A. Bleed air valves.", "B. Stator vanes.", "C. Inlet guide vanes."],
    answer: "A"
  },
  {
    question: "What purpose do the diffuser vanes of a centrifugal compressor serve?",
    options: [
      "A. To convert pressure energy into kinetic energy.",
      "B. To increase the air velocity.",
      "C. To convert kinetic energy into pressure energy."
    ],
    answer: "C"
  },
  {
    question: "During the high RPM range on an axial flow gas turbine engine, in what position are the variable intake guide vanes and bleed valves?",
    options: [
      "A. At maximum swirl position, bleed valves open.",
      "B. At minimum swirl position, bleed valves closed.",
      "C. At maximum swirl position, bleed valves closed."
    ],
    answer: "B"
  },
  {
    question: "What is the purpose of the diffuser section in a turbine engine?",
    options: [
      "A. To convert pressure to velocity.",
      "B. To reduce pressure and increase velocity.",
      "C. To increase pressure and reduce velocity."
    ],
    answer: "C"
  },
  {
    question: "The fan speed of a twin spool axial compressor engine is the same as the.",
    options: ["A. low pressure compressor.", "B. forward turbine wheel.", "C. high pressure compressor."],
    answer: "A"
  },
  {
    question: "Bleed valves are normally spring loaded to the.",
    options: ["A. closed position.", "B. open position.", "C. mid-position."],
    answer: "B"
  },
  {
    question: "What is the function of the stator vane assembly at the discharge end of a typical axial flow compressor?",
    options: [
      "A. To increase air swirling motion into the combustion chambers.",
      "B. To direct the flow of gases into the combustion chambers.",
      "C. To straighten airflow to eliminate turbulence."
    ],
    answer: "C"
  },
  {
    question: "In a turbine engine with a dual spool compressor, the low speed compressor.",
    options: [
      "A. always turns at the same speed as the high speed compressor.",
      "B. seeks its own best operating speed.",
      "C. is connected directly to the high speed compressor."
    ],
    answer: "B"
  },
  {
    question: "What units in a gas turbine engine aid in guiding the airflow during low thrust engine operations?",
    options: ["A. Stator vanes.", "B. Bleed air valves.", "C. Inlet guide vanes."],
    answer: "C"
  },
  {
    question: "What is one purpose of the stator blades in the compressor section?",
    options: [
      "A. Increase the velocity of the airflow.",
      "B. Stabilize the pressure of the airflow.",
      "C. Control the direction of the airflow."
    ],
    answer: "C"
  },
  {
    question: "Compressor stall is caused by.",
    options: [
      "A. a low angle of attack airflow through the first stages of compression.",
      "B. rapid engine deceleration.",
      "C. a high angle of attack airflow through the first stages of compression."
    ],
    answer: "C"
  },
  {
    question: "What is used to aid in stabilization of compressor airflow?",
    options: [
      "A. Variable guide vanes and/or compressor bleed valves.",
      "B. Pressurization and dump valves.",
      "C. Stator vanes and rotor vanes."
    ],
    answer: "A"
  },
  {
    question: "What is the primary factor which controls the pressure ratio of an axial flow compressor?",
    options: [
      "A. Compressor inlet temperature.",
      "B. Compressor inlet pressure.",
      "C. Number of stages in compressor."
    ],
    answer: "C"
  },
  {
    question: "The non-rotating axial-flow compressor airfoils in an aircraft gas turbine are known as.",
    options: ["A. stator vanes.", "B. bleed vanes.", "C. pressurization vanes."],
    answer: "A"
  },
  {
    question: "The purpose of a bleed valve, located in the beginning stages of the compressor is to.",
    options: [
      "A. vent some of the air overboard to prevent a compressor stall.",
      "B. control excessively high RPM to prevent a compressor stall.",
      "C. vent high ram air pressure overboard to prevent a compressor stall."
    ],
    answer: "A"
  },
  {
    question: "During the low RPM range on an axial flow gas turbine engine, in what position are the variable intake guide vanes and bleed valves?",
    options: [
      "A. At maximum swirl position, bleed valves open.",
      "B. At maximum swirl position, bleed valves closed.",
      "C. At minimum swirl position, bleed valves closed."
    ],
    answer: "A"
  },
  {
    question: "The energy changes that take place in the impeller of a centrifugal compressor are.",
    options: [
      "A. pressure decrease, velocity decrease, temperature increase.",
      "B. pressure increase, velocity decrease, temperature increase.",
      "C. pressure increase, velocity increase, temperature increase."
    ],
    answer: "C"
  },
  {
    question: "What is the primary advantage of an axial flow compressor over a centrifugal compressor?",
    options: ["A. High frontal area.", "B. Greater pressure ratio.", "C. Less expensive."],
    answer: "B"
  },
  {
    question: "Compression occurs.",
    options: ["A. across stators and rotors.", "B. across rotors.", "C. across stators."],
    answer: "A"
  },
  {
    question: "Which of the following can cause fan blade shingling in a turbofan engine?",
    options: [
      "A. Large, rapid throttle movements and FOD.",
      "B. Engine over temperature, large, rapid throttle movements and FOD.",
      "C. Engine overspeed and large, rapid throttle movements."
    ],
    answer: "A"
  },
  {
    question: "Severe rubbing of turbine engine compressor blades will usually cause.",
    options: ["A. cracking.", "B. bowing.", "C. galling."],
    answer: "C"
  },
  {
    question: "Which two elements make up the axial flow compressor assembly?",
    options: ["A. Rotor and stator.", "B. Stator and diffuser.", "C. Compressor and manifold."],
    answer: "A"
  },
  {
    question: "If the RPM of an axial flow compressor remains constant, the angle of attack of the rotor blades can be changed by.",
    options: [
      "A. changing the compressor diameter.",
      "B. changing the velocity of the airflow.",
      "C. increasing the pressure ratio."
    ],
    answer: "B"
  },
  {
    question: "The gas turbine Compressor Pressure Ratio is.",
    options: [
      "A. Compressor inlet pressure divided by Compressor discharge pressure.",
      "B. Mass of air bypassing the combustion system divided by Mass of air going through the combustion system.",
      "C. Compressor discharge pressure divided by Compressor inlet pressure."
    ],
    answer: "C"
  },
  {
    question: "An advantage of the centrifugal flow compressor is its high.",
    options: ["A. ram efficiency.", "B. pressure rise per stage.", "C. peak efficiency."],
    answer: "C"
  },
  {
    question: "The compression ratio of an axial flow compressor is a function of the.",
    options: ["A. number of compressor stages.", "B. air inlet velocity.", "C. rotor diameter."],
    answer: "A"
  },
  {
    question: "Jet engine turbine blades removed for detailed inspection must be reinstalled in.",
    options: [
      "A. the same slot.",
      "B. a specified slot 180°away.",
      "C. a specified slot 90°away in the direction of rotation."
    ],
    answer: "A"
  },
  {
    question: "The procedure for removing the accumulation of dirt deposits on compressor blades is called.",
    options: ["A. the purging process.", "B. the soak method.", "C. field cleaning."],
    answer: "C"
  },
  {
    question: "The two types of centrifugal compressor impellers are.",
    options: ["A. impeller and diffuser.", "B. single entry and double entry.", "C. rotor and stator."],
    answer: "B"
  },
  {
    question: "Between each row of rotating blades in a compressor, there is a row of stationary blades which act to diffuse the air. These stationary blades are called.",
    options: ["A. stators.", "B. rotors.", "C. buckets."],
    answer: "A"
  },
  {
    question: "Bleed valves are.",
    options: ["A. closed at low RPM.", "B. always slightly open.", "C. closed at high RPM."],
    answer: "C"
  },
  {
    question: "Compressor field cleaning on turbine engines is performed primarily in order to.",
    options: [
      "A. prevent engine oil contamination and subsequent engine bearing wear or damage.",
      "B. prevent engine performance degradation, increased fuel costs, and damage or corrosion to gas path surfaces.",
      "C. facilitate flight line inspection of engine inlet and compressor areas for defects or FOD."
    ],
    answer: "B"
  },
  {
    question: "If the LP compressor shaft severed.",
    options: [
      "A. the LP turbine will speed up and the LP compressor will slow down.",
      "B. the LP compressor of cruise thrust.",
      "C. the HP compressor will slow down."
    ],
    answer: "A"
  },
  {
    question: "An advantage of a centrifugal compressor is.",
    options: [
      "A. it is dynamically balanced.",
      "B. it is unaffected by turbulence.",
      "C. it is robust and can stand some shock from icing-up."
    ],
    answer: "C"
  },
  {
    question: "Variable inlet guide vanes prevent.",
    options: ["A. compressor runaway.", "B. engine flame out at high speed.", "C. compressor stalling."],
    answer: "C"
  },
  {
    question: "An axial flow compressor surges when.",
    options: ["A. later stages are stalled.", "B. all stages are stalled.", "C. early stages are stalled."],
    answer: "B"
  },
  {
    question: "As a consequence of tapping air from the compressor, the TGT will.",
    options: ["A. fall.", "B. remain constant.", "C. rise."],
    answer: "C"
  },
  {
    question: "Compressor air bleeds promote the flow of air through the early stages by.",
    options: ["A. opening to allow air in.", "B. closing.", "C. opening to allow air out."],
    answer: "C"
  },
  {
    question: "Compressor blades have a reduced angle of attack at the tips.",
    options: [
      "A. to prevent turbine stall.",
      "B. to increase the velocity.",
      "C. to allow uniform axial velocity."
    ],
    answer: "C"
  },
  {
    question: "Compressor surge is caused by.",
    options: ["A. over fuelling.", "B. rapid closing of the throttle.", "C. prolonged engine running at high RPM."],
    answer: "A"
  },
  {
    question: "Pressure rise across a single spool axial flow compressor is in the order of.",
    options: ["A. four to one.", "B. two to one.", "C. up to fifteen to one."],
    answer: "C"
  },
  {
    question: "What purpose do the diffuser vanes of a centrifugal compressor serve?",
    options: [
      "A. To convert pressure energy into kinetic energy.",
      "B. To increase the air velocity.",
      "C. To convert kinetic energy into pressure energy."
    ],
    answer: "C"
  },
  {
    question: "The purpose of the rotating guide vanes on a centrifugal compressor is to.",
    options: [
      "A. direct the air smoothly into the impeller.",
      "B. provide initial diffusing of the air.",
      "C. prevent damage by solid objects."
    ],
    answer: "B"
  },
  {
    question: "What is the surge margin of an axial flow compressor?",
    options: [
      "A. The margin between the compressor working line and the surge line.",
      "B. The margin between minimum and maximum pressure ratio obtained at constant RPM.",
      "C. The margin between the stall condition and the surge condition."
    ],
    answer: "A"
  },
  {
    question: "The compression ratio of a jet engine is.",
    options: [
      "A. the compressor outlet pressure divided by the number of compressor stages.",
      "B. the ratio between turbine pressure and compressor outlet pressure.",
      "C. the ratio between compressor outlet pressure and compressor inlet pressure."
    ],
    answer: "C"
  },
  {
    question: "Variable inlet guide vanes help to prevent.",
    options: ["A. compressor runaway.", "B. ice build up on compressor blades.", "C. compressor stalling."],
    answer: "C"
  },
  {
    question: "Air through the compressor, before entering the combustion chamber, passes.",
    options: [
      "A. through divergent passage to increase the pressure.",
      "B. through nozzles to increase the velocity.",
      "C. through divergent passage to decrease the pressure."
    ],
    answer: "A"
  },
  {
    question: "Low mass airflow through a compressor will produce.",
    options: ["A. stalling of rear stages.", "B. stalling of early stages.", "C. no effect."],
    answer: "B"
  },
  {
    question: "A bleed valve.",
    options: [
      "A. relieves compressor choking at low RPM.",
      "B. controls air intake pressure.",
      "C. bleeds air from compressor for intake deicing."
    ],
    answer: "A"
  },
  {
    question: "If a compressor has a compression ratio of 9:1 and an intake compression of 2:1, what is the overall compression ratio?",
    options: [
      "A. 9:1 intake compression does not add to the overall compression ratio of the system.",
      "B. 18:1.",
      "C. 11:1."
    ],
    answer: "B"
  },
  {
    question: "A compressor stage stalls when.",
    options: [
      "A. adiabatic temperature rise is too high.",
      "B. compression ratio is too high.",
      "C. smooth airflow is disrupted."
    ],
    answer: "C"
  },
  {
    question: "Inlet guide vanes are fitted to.",
    options: [
      "A. control the quantity of air entering the intake.",
      "B. guide the airflow onto the turbine rotor first stage.",
      "C. control the angle of airflow into the compressor."
    ],
    answer: "C"
  },
  {
    question: "Why, in an axial flow compressor is the cross sectional area of the compressor air duct reduced at each stage?",
    options: [
      "A. To decrease the velocity of the air rising under pressure.",
      "B. To maintain the velocity of the air under rising pressure.",
      "C. To permit stronger, shorter blades to be used in the later stages."
    ],
    answer: "B"
  },
  {
    question: "An abradable lining around the fan is to.",
    options: [
      "A. provide less leakage for anti-icing.",
      "B. prevent fan blade tip rub.",
      "C. strengthen the EPR value."
    ],
    answer: "C"
  },
  {
    question: "Allowable damage on the first stage compressor blade is restricted to.",
    options: [
      "A. middle third of the blade to the outer edge.",
      "B. outer third of the blade to the outer edge.",
      "C. root end of the blade."
    ],
    answer: "A"
  },
  {
    question: "Tip speed of a centrifugal compressor can reach.",
    options: ["A. Mach 1.3.", "B. Mach 1.0.", "C. Mach 0.8."],
    answer: "A"
  },
  {
    question: "What is the profile of a compressor blade?",
    options: [
      "A. A cutout that reduces blade tip thickness.",
      "B. The leading edge of the blade.",
      "C. The curvature of the blade root."
    ],
    answer: "A"
  },
  {
    question: "The resultant velocity of air exiting an axial compressor stage depends upon.",
    options: ["A. aircraft forward speed.", "B. compressor RPM.", "C. Both of the above."],
    answer: "C"
  },
  {
    question: "What is a compressor stage?",
    options: [
      "A. One compressor rotor and one nozzle guide vane.",
      "B. One rotor plus one stator.",
      "C. One Nozzle Guide Vane and one rotor."
    ],
    answer: "B"
  },
  {
    question: "If the bypass ratio is 0.7:1, the 0.7 pounds of air is.",
    options: [
      "A. fed into H.P compressor compared to 1 pound fed around it.",
      "B. fed around the engine to 1 pound fed into H.P. compressor.",
      "C. bypassed for every 1 pound at the intake."
    ],
    answer: "B"
  },
  {
    question: "Advantage of an axial flow over a centrifugal flow gas turbine engine.",
    options: ["A. power required for starting is less.", "B. low weight.", "C. high peak efficiencies."],
    answer: "C"
  },
  {
    question: "Compressor blades reduce in length.",
    options: [
      "A. from tip to root to maintain uniform velocity in compressor.",
      "B. from L.P to H.P section to maintain uniform velocity in compressor.",
      "C. from root to tip to maintain correct angle of attack."
    ],
   answer: "B"
  },
  {
    question: "Deposit build-up on compressor blades.",
    options: [
      "A. airflow is too fast for deposits to build up.",
      "B. will not decrease efficiency but may cause corrosion.",
      "C. can decrease compressor efficiency and cause corrosion."
    ],
    answer: "C"
  },
  {
    question: "The diffuser after the compressor, before the combustion chamber.",
    options: [
      "A. increases velocity, decreases pressure.",
      "B. decreases velocity, pressure increases.",
      "C. increases velocity, pressure remains constant."
    ],
    answer: "B"
  },
  {
    question: "In a compressor, diffusion action takes place across.",
    options: ["A. rotors.", "B. rotors and stators.", "C. stators."],
    answer: "B"
  },
  {
    question: "The ring of fixed blades at the intake of an axial flow compressor are called.",
    options: ["A. inlet guide vanes.", "B. first stage stator blades.", "C. first stage diffuser blades."],
    answer: "A"
  },
  {
    question: "What is the purpose of pressure balance seal?",
    options: [
      "A. To ensure the location bearing is adequately loaded throughout the engine thrust range.",
      "B. To ensure LP compressor is statically balanced.",
      "C. To ensure HP compressor is dynamically balanced."
    ],
    answer: "A"
  },
  {
    question: "The optimum air speed for entrance into the compressor is approximately.",
    options: ["A. same as aircraft speed.", "B. Mach 0.4.", "C. Mach 1."],
    answer: "B"
  },
  {
    question: "What is the acceptable damage on stator blades that have been blended?",
    options: [
      "A. One third along from root to tip.",
      "B. One third from tip to root.",
      "C. One third chord wise."
    ],
    answer: "B"
  },
  {
    question: "With regard to compressor blades, which of the following is true? No damage is permissible on.",
    options: [
      "A. a shroud fillet area.",
      "B. the lip of a blade.",
      "C. the last third of the outboard leading edge."
    ],
    answer: "A"
  },
  {
    question: "Identify a function of the cascade vanes in a turbojet engine compressor section.",
    options: [
      "A. To remove air swirl before the combustion chamber.",
      "B. To direct the flow of air to strike the turbine blades at a desired angle.",
      "C. To decrease the velocity of air to the combustor."
    ],
    answer: "A"
  },
  {
    question: "The pressure ratio can be influenced by.",
    options: ["A. compressor inlet temperature.", "B. number of stages in compressor.", "C. compressor inlet pressure."],
    answer: "B"
  },
  {
    question: "Air bleed valves are.",
    options: ["A. closed at low RPM.", "B. open at high RPM.", "C. open at low RPM."],
    answer: "C"
  },
  {
    question: "The compressor case annulus is.",
    options: ["A. convergent.", "B. divergent.", "C. parallel."],
    answer: "A"
  },
  {
    question: "If the tip clearance in a centrifugal compressor is too small.",
    options: [
      "A. there would be pressure losses through leakage.",
      "B. there is danger of seizure.",
      "C. aerodynamic buffeting would cause vibration."
    ],
    answer: "C"
  },
  {
    question: "A 1st stage LP compressor blade is able to continue in service if the damage is within limits, and within the.",
    options: ["A. middle third of blade chord-wise.", "B. outer third only.", "C. root section only."],
    answer: "B"
  },
  {
    question: "What is meant by a compressor stage?",
    options: [
      "A. One rotor and one stator assembly.",
      "B. All rotors and stators.",
      "C. One rotor and one guide vane assembly."
    ],
    answer: "A"
  },
  {
    question: "What is the normal pressure rise across each compressor stage of an axial flow compressor?",
    options: ["A. 1.5:1.", "B. 1.2:1.", "C. 5:1."],
    answer: "B"
  },
  {
    question: "Where does compression take place as air passes through an axial flow compressor?",
    options: ["A. Rotor blades.", "B. Stator Blades.", "C. Rotor and Stator blades."],
    answer: "C"
  },
  {
    question: "Nozzle Guide Vane bow is an indication of.",
    options: ["A. engine overspeed.", "B. engine overheat.", "C. engine shock loading."],
    answer: "B"
  },
  {
    question: "A build up of foreign objects and dirt on compressor blades.",
    options: [
      "A. has a large effect on compressor efficiency and may cause corrosion.",
      "B. has no effect on the efficiency of the compressor but may cause corrosion.",
      "C. has no effect on compressor efficiency due to the speed of rotation."
    ],
    answer: "A"
  },
  {
    question: "What is the purpose of the stator vanes in the compressor section of a gas turbine engine?",
    options: [
      "A. Increase the velocity of the airflow.",
      "B. Control direction of the airflow.",
      "C. Prevent compressor surge."
    ],
    answer: "B"
  },
  {
    question: "In a twin spool compressor, the LP section runs at.",
    options: [
      "A. a lower RPM than the HP spool.",
      "B. a higher RPM than the HP spool.",
      "C. the same RPM than the HP spool."
    ],
    answer: "A"
  }
]

               function shuffle(array) {
          for (let i = array.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [array[i], array[j]] = [array[j], array[i]];
          }
          return array;
        }
        function fmtTime(s) {
          const m = Math.floor(s / 60).toString().padStart(2, "0");
          const sec = (s % 60).toString().padStart(2, "0");
          return m + ":" + sec;
        }
        function showView(idToShow) {
          ["welcome","quiz","results","bank"].forEach(id => {
            const el = document.getElementById(id);
            if (!el) return;
            if (id === idToShow) el.classList.remove("hidden");
            else el.classList.add("hidden");
          });
          window.scrollTo({ top: 0, behavior: "smooth" });
        }

        /* ====== 深色模式：持久化 ====== */
        const darkBtn = document.getElementById("darkModeToggle");
        function applyInitialTheme() {
          try {
            const saved = localStorage.getItem('theme');
            const isDark = (saved === 'dark') || (!saved && window.matchMedia('(prefers-color-scheme: dark)').matches);
            document.body.classList.toggle('dark', isDark);
            darkBtn.setAttribute('aria-pressed', String(isDark));
          } catch(e) {}
          document.documentElement.classList.remove('preload-dark');
        }
        applyInitialTheme();
        darkBtn.addEventListener('click', function(){
          const willDark = !document.body.classList.contains('dark');
          document.body.classList.toggle('dark', willDark);
          darkBtn.setAttribute('aria-pressed', String(willDark));
          try { localStorage.setItem('theme', willDark ? 'dark' : 'light'); } catch(e){}
        });

        /* ====== 測驗狀態 ====== */
        let shuffledQuestions;
        let current = 0, total = 0, timer = 80 * 60, interval, answers = [];
        let quizActive = false;
        const progressBar = document.getElementById("progressBar");

        /* ====== 元件 ====== */
        const startBtn = document.getElementById("startBtn");
        const prevBtn = document.getElementById("prevBtn");
        const leaveBtn = document.getElementById("leaveBtn");
        const retryBtn = document.getElementById("retryBtn");
        const showWrongBtn = document.getElementById("showWrongBtn");
        const showAllBtn = document.getElementById("showAllBtn");
        const openBankBtn = document.getElementById("openBankBtn");
        const toBankFromResults = document.getElementById("toBankFromResults");

        /* ====== 題庫瀏覽 ====== */
        const bankBody = document.getElementById("bankBody");
        const bankSearch = document.getElementById("bankSearch");
        const perPageSel = document.getElementById("perPage");
        const pageInfo = document.getElementById("pageInfo");
        const prevPageBtn = document.getElementById("prevPage");
        const nextPageBtn = document.getElementById("nextPage");
        const backToWelcome = document.getElementById("backToWelcome");
        const pagination = document.querySelector('#bank .pagination');

        // 抓取自訂時間輸入框
        const durationInput = document.getElementById("durationInput");

        let bankFiltered = questions.slice();
        let page = 1;
        function totalPages() {
          const sel = perPageSel.value;
          if (sel === 'all') return 1;
          return Math.max(1, Math.ceil(bankFiltered.length / parseInt(sel || "10")));
        }

        /* ====== 測驗流程 ====== */
        startBtn.addEventListener("click", () => {
          const n = document.getElementById("nameInput").value.trim();
          const qLimit = parseInt(document.getElementById("questionLimit").value);

          if (!n) return alert("請輸入姓名 / Enter your name");
          if (!qLimit || qLimit <= 0) return alert("請輸入要作答的題數,最多5題 / Enter number of questions");

          // 讀取自訂時間（分鐘）；預設 80，限制 1~300 分鐘
          let minutes = parseInt((durationInput && durationInput.value) ? durationInput.value : "80");
          if (isNaN(minutes)) minutes = 80;
          minutes = Math.max(1, Math.min(999, minutes));

          shuffledQuestions = shuffle(questions.slice()).slice(0, Math.min(qLimit, questions.length));
          total = shuffledQuestions.length;
          current = 0; answers = [];
          timer = minutes * 60;            // 用自訂分鐘
          quizActive = true;

          document.getElementById("welcomeName").innerText = "歡迎: " + n;
          document.getElementById("total").innerText = total;
          // 進場前先把右上角時計顯示成正確的起始值
          document.getElementById("timer").innerText = fmtTime(timer);
          updateProgress();

          // 確保不會重複計時
          clearInterval(interval);
          interval = setInterval(() => {
            if (timer > 0 && quizActive) {
              timer--;
              document.getElementById("timer").innerText = fmtTime(timer);
            } else if (quizActive && timer <= 0) {
              clearInterval(interval);
              finish();
            }
          }, 1000);

          showQ();
          showView("quiz");
        });

        prevBtn.addEventListener("click", () => {
          if (current > 0) {
            current--;
            answers.pop();
            showQ();
          }
        });

        leaveBtn.addEventListener("click", finish);
        retryBtn.addEventListener("click", () => location.reload());
        showWrongBtn.addEventListener("click", () => renderResults(answers.filter(a => !a.correct)));
        showAllBtn.addEventListener("click", () => renderResults(answers));
        openBankBtn.addEventListener("click", enterBank);
        toBankFromResults.addEventListener("click", enterBank);

        /* ====== 題庫事件 ====== */
        bankSearch.addEventListener("input", applyFilter);
        perPageSel.addEventListener("change", () => { page = 1; renderBank(); });
        prevPageBtn.addEventListener("click", () => { if (page > 1) { page--; renderBank(); } });
        nextPageBtn.addEventListener("click", () => { if (page < totalPages()) { page++; renderBank(); } });
        backToWelcome.addEventListener("click", () => showView("welcome"));

        function enterBank() {
          if (quizActive) { alert("測驗進行中不可瀏覽題庫。請先完成或離開考試。"); return; }
          if (!bankSearch.value) { bankFiltered = questions.slice(); page = 1; }
          renderBank();
          showView("bank");
        }

        function applyFilter() {
          const kw = bankSearch.value.trim().toLowerCase();
          bankFiltered = kw
            ? questions.filter(q => q.question.toLowerCase().includes(kw) ||
                                    q.options.some(o => o.toLowerCase().includes(kw)))
            : questions.slice();
          page = 1; renderBank();
        }

        function renderBank() {
          bankBody.innerHTML = "";
          const sel = perPageSel.value;
          const per = (sel === 'all') ? bankFiltered.length : parseInt(sel || "10");
          const start = (page - 1) * per;
          const items = (sel === 'all') ? bankFiltered.slice() : bankFiltered.slice(start, start + per);

          items.forEach((q, idx) => {
            const tr = document.createElement("tr");
            const num = (sel === 'all') ? (idx + 1) : (start + idx + 1);
            const correctText = q.options.find(o => o.charAt(0) === q.answer) || "";

            const optsHtml = q.options.map(o => {
              const isC = (o.charAt(0) === q.answer);
              return `<span class="opt-row ${isC ? 'is-correct' : ''}">${isC ? '<span class="ans-chip">正解</span>' : ''}${o}</span>`;
            }).join("");

            tr.innerHTML =
              '<td class="mono">#' + num + "</td>" +
              "<td>" + q.question + "</td>" +
              "<td>" + optsHtml + "</td>" +
              '<td class="mono ans-cell">' + (q.answer + "｜" + correctText) + "</td>";
            bankBody.append(tr);
          });

          const tp = totalPages();
          if (sel === 'all') {
            if (pagination) pagination.style.display = 'none';
            pageInfo.textContent = "全部顯示（共 " + bankFiltered.length + " 題）";
            prevPageBtn.disabled = true; nextPageBtn.disabled = true;
          } else {
            if (pagination) pagination.style.display = 'flex';
            pageInfo.textContent = "第 " + page + " / " + tp + " 頁（共 " + bankFiltered.length + " 題）";
            prevPageBtn.disabled = (page <= 1); nextPageBtn.disabled = (page >= tp);
          }
        }

        /* ====== 測驗：出題/進度/結束 ====== */
        function showQ() {
          if (current >= total) return finish();
          document.getElementById("current").innerText = current + 1;
          updateProgress();

          const q = shuffledQuestions[current];
          document.getElementById("questionText").innerText = q.question;

          const optDiv = document.getElementById("options");
          optDiv.innerHTML = "";

          let optionsWithFlag = q.options.map(option => ({ text: option, isAnswer: option.charAt(0) === q.answer }));
          optionsWithFlag = shuffle(optionsWithFlag);

          optionsWithFlag.forEach(opt => {
            const lbl = document.createElement("label");
            const rd = document.createElement("input");
            rd.type = "radio"; rd.name = "opt"; rd.value = opt.text;
            rd.onchange = function () {
              answers.push({
                q, selectedText: opt.text,
                correctText: q.options.find(optItem => optItem.charAt(0) === q.answer),
                correct: opt.isAnswer
              });
              current++; showQ();
            };
            lbl.append(rd, " ", opt.text);
            optDiv.append(lbl);
          });
        }

        function updateProgress() {
          const percent = (current / total) * 100;
          progressBar.style.width = percent + "%";
        }

        function finish() {
          clearInterval(interval);
          quizActive = false; // 結束後可進入題庫
          showView("results");
          renderResults(answers);
          document.getElementById("scoreSummary").innerText =
            "答對 " + answers.filter(a => a.correct).length +
            " 題 / 已作答 " + answers.length + " 題 / 共 " + total + " 題";
        }

        function renderResults(data) {
          const tb = document.getElementById("resultsBody");
          tb.innerHTML = "";
          data.forEach(a => {
            const tr = document.createElement("tr");
            tr.innerHTML =
              "<td>" + a.q.question + "</td>" +
              "<td>" + a.selectedText + "</td>" +
              '<td class="ans-cell">' + a.correctText + "</td>" +
              "<td>" + (a.correct ? "O" : "X") + "</td>";
            if (!a.correct) tr.classList.add("wrong");
            tb.append(tr);
          });
        }
      });
    </script>
  </body>
</html>
