<!DOCTYPE html>
<html lang="uk">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NSAT Bingo Тренажер Pro</title>
    <style>
        body { background-color: #1a1a24; color: #f0f0f5; font-family: 'Segoe UI', sans-serif; display: flex; flex-direction: column; align-items: center; padding: 20px; margin: 0; min-height: 100vh; }
        h1 { color: #4CAF50; margin-bottom: 5px; }
        .subtitle { color: #aaa; margin-bottom: 25px; text-align: center; max-width: 600px; }
        .game-container { display: flex; flex-direction: column; gap: 25px; max-width: 900px; width: 100%; }
        @media (min-width: 768px) { .game-container { flex-direction: row; align-items: flex-start; } }
        .bingo-card-container { flex: 1; background: #23232f; padding: 20px; border-radius: 12px; box-shadow: 0 8px 24px rgba(0,0,0,0.3); border: 2px solid #333; }
        .bingo-grid { display: flex; flex-direction: column; gap: 10px; }
        .bingo-cell { background: #2d2d3d; padding: 15px; border-radius: 8px; text-align: center; font-weight: bold; border: 1px solid #444; transition: all 0.4s ease; color: #ccc; }
        .bingo-cell.active { background: linear-gradient(135deg, #4CAF50, #45a049); color: white; border-color: #66bb6a; transform: scale(1.02); }
        .simulator-container { flex: 1.5; background: #23232f; padding: 20px; border-radius: 12px; border: 2px solid #333; }
        .nsat-stars { font-size: 1.5rem; color: #ffca28; margin-bottom: 15px; text-align: center; font-weight: bold; }
        .chat-box { background: #111116; padding: 15px; border-radius: 8px; margin-bottom: 20px; border-left: 4px solid #00bcd4; }
        .client-name { font-weight: bold; color: #00bcd4; margin-bottom: 5px; }
        .option-btn { background: #2d2d3d; color: #fff; border: 1px solid #555; padding: 12px; border-radius: 8px; cursor: pointer; text-align: left; width: 100%; margin-bottom: 8px; transition: 0.2s; }
        .option-btn:hover { background: #3d3d52; }
    </style>
</head>
<body>

    <h1>Гейміфікація: NSAT Bingo Pro 🎯</h1>
    <div class="subtitle">Твій показник NSAT залежить від атмосфери, простоти пояснень та швидкості. Збери всі 5 елементів ідеального сервісу, щоб закрити картку Бінго на 5 зірок!</div>

    <div class="game-container">
        <div class="bingo-card-container">
            <h3>Твоя Картка Бінго</h3>
            <div class="bingo-grid">
                <div class="bingo-cell" id="cell-empathy">❤️ Емпатія</div>
                <div class="bingo-cell" id="cell-simple">💡 Просте пояснення</div>
                <div class="bingo-cell" id="cell-name">👤 Ім'я клієнта</div>
                <div class="bingo-cell" id="cell-wow">✨ WOW-фраза</div>
                <div class="bingo-cell" id="cell-needs">🔍 Уточнення потреби</div>
            </div>
        </div>

        <div class="simulator-container">
            <div class="nsat-stars" id="nsat-rating">Поточний NSAT: ⭐ 3.0</div>
            <div class="chat-box">
                <div class="client-name" id="client-title">Завантаження...</div>
                <div class="client-text" id="client-text"></div>
            </div>
            <div id="options-block"></div>
        </div>
    </div>

    <script>
        const scenarios = [
            { client: "Олена", text: "«Я випадково видалила eSIM і тепер дуже хвилююсь, що залишусь без номера 😟»", targetSkill: "empathy", options: [ { text: "«Олено, не хвилюйтеся 🌿 Зараз усе перевіримо та спробуємо швидко відновити ваш номер.»", correct: true }, { text: "«Видаляти eSIM не потрібно було.»", correct: false }, { text: "«Такі ситуації трапляються часто.»", correct: false } ] },
            { client: "Віктор", text: "«А що означає “безлімітний інтернет”?»", targetSkill: "simple", options: [ { text: "«Це означає, що ви можете користуватися інтернетом без додаткової оплати 😊 Але після певного обсягу ГБ швидкість може зменшуватись.»", correct: true }, { text: "«У тарифі діє політика fair usage.»", correct: false }, { text: "«Усі умови є на сайті.»", correct: false } ] },
            { client: "Сергій", text: "«Я не можу зайти в My Vodafone.»", targetSkill: "name", options: [ { text: "«Сергію, зараз разом перевіримо, чому не вдається увійти в додаток 😊»", correct: true }, { text: "«Перевстановіть додаток.»", correct: false }, { text: "«Спробуйте пізніше.»", correct: false } ] },
            { client: "Ірина", text: "«Дякую за допомогу 🙏»", targetSkill: "wow", options: [ { text: "«Ірино, щиро радий був допомогти 🌸 Нехай у вас сьогодні буде тільки гарний настрій та стабільний зв’язок.»", correct: true }, { text: "«Будь ласка. До побачення.»", correct: false }, { text: "«Оцініть консультацію в SMS.»", correct: false } ] },
            { client: "Максим", text: "«Мені потрібен новий тариф.»", targetSkill: "needs", options: [ { text: "«Максиме, підкажіть, будь ласка, що для вас зараз найважливіше: інтернет, дзвінки чи вигідна абонплата?»", correct: true }, { text: "«Можу одразу запропонувати найдорожчий тариф.»", correct: false }, { text: "«Усі тарифи приблизно однакові.»", correct: false } ] },
            { client: "Дмитро", text: "«Я вже другий день не можу нормально користуватись інтернетом!»", targetSkill: "empathy", options: [ { text: "«Дмитре, розумію ваше невдоволення 😔 Давайте зараз усе перевіримо та спробуємо вирішити ситуацію.»", correct: true }, { text: "«У нас зараз велике навантаження на мережу.»", correct: false }, { text: "«Таке інколи буває.»", correct: false } ] },
            { client: "Марина", text: "«Чому в роумінгу інтернет працює повільніше?»", targetSkill: "simple", options: [ { text: "«У роумінгу швидкість може залежати від мережі закордонного оператора 😊 Але зараз давайте перевіримо, чи можна її покращити.»", correct: true }, { text: "«Це залежить від роумінг-партнера.»", correct: false }, { text: "«Такі умови роумінгу.»", correct: false } ] },
            { client: "Оксана", text: "«Мені вже набридло дзвонити з цього питання!»", targetSkill: "name", options: [ { text: "«Оксано, розумію ваше роздратування. Давайте зараз уважно все перевіримо.»", correct: true }, { text: "«Я вас почув.»", correct: false }, { text: "«Попередній оператор мав це вирішити.»", correct: false } ] },
            { client: "Андрій", text: "«Ну добре, дякую за пояснення.»", targetSkill: "wow", options: [ { text: "«Дякую, що звернулися 😊 Нехай користування Vodafone буде для вас максимально комфортним.»", correct: true }, { text: "«До побачення.»", correct: false }, { text: "«Очікуйте SMS.»", correct: false } ] },
            { client: "Юлія", text: "«У мене дуже поганий інтернет.»", targetSkill: "needs", options: [ { text: "«Юліє, підкажіть, будь ласка, інтернет повільний постійно чи лише в певному місці?»", correct: true }, { text: "«Перезавантажте телефон.»", correct: false }, { text: "«У нас зараз усе працює стабільно.»", correct: false } ] },
            { client: "Владислав", text: "«Через ваш інтернет у мене зірвалася важлива робоча зустріч!»", targetSkill: "empathy", options: [ { text: "«Владиславе, розумію, наскільки це могло бути важливо для вас 😔 Давайте зараз перевіримо, що можемо зробити для стабілізації зв’язку.»", correct: true }, { text: "«Технічні складнощі можуть траплятися.»", correct: false }, { text: "«Мережа не може працювати ідеально постійно.»", correct: false } ] },
            { client: "Тетяна", text: "«Чому eSIM встановилась, але мережі немає?»", targetSkill: "simple", options: [ { text: "«Схоже, eSIM ще не активувалась 😊 Це не шкодить телефону, а зараз я допоможу все перевірити.»", correct: true }, { text: "«Можливо, це пуста eSIM.»", correct: false }, { text: "«Таке буває.»", correct: false } ] },
            { client: "Сергій", text: "«Я вже хочу перейти до іншого оператора!»", targetSkill: "name", options: [ { text: "«Сергію, розумію ваше розчарування 😔 Давайте спробуємо знайти рішення, яке буде для вас комфортним.»", correct: true }, { text: "«Це ваше право.»", correct: false }, { text: "«Усі оператори мають однакові проблеми.»", correct: false } ] },
            { client: "Олена", text: "«Сподіваюсь, тепер усе працюватиме нормально.»", targetSkill: "wow", options: [ { text: "«Щиро сподіваюсь, що тепер користування буде для вас лише комфортним 😊 А якщо щось знадобиться — ми завжди поруч.»", correct: true }, { text: "«Має працювати.»", correct: false }, { text: "«Проблему вже вирішено.»", correct: false } ] },
            { client: "Максим", text: "«Мені потрібен нормальний тариф.»", targetSkill: "needs", options: [ { text: "«Максиме, підкажіть, будь ласка, що саме для вас означає “нормальний тариф”: більше інтернету, вигідна ціна чи роумінг?»", correct: true }, { text: "«Можу запропонувати стандартний тариф.»", correct: false }, { text: "«Усі тарифи зараз однакові.»", correct: false } ] }
        ];

        let activeSkills = { empathy: false, simple: false, name: false, wow: false, needs: false };
        let nsatScore = 3.0;
        let pool = [...scenarios].sort(() => Math.random() - 0.5);

        function updateUI() {
            document.getElementById('nsat-rating').innerText = `Поточний NSAT: ⭐ ${nsatScore.toFixed(1)}`;
            if (Object.values(activeSkills).every(v => v === true)) { alert("БІНГО! Ти майстер сервісу!"); location.reload(); }
            
            if (pool.length === 0) pool = scenarios.filter(s => !activeSkills[s.targetSkill]);
            const current = pool[0];
            document.getElementById('client-title').innerText = current.client + ":";
            document.getElementById('client-text').innerText = current.text;
            
            const container = document.getElementById('options-block');
            container.innerHTML = '';
            current.options.forEach(opt => {
                const btn = document.createElement('button');
                btn.className = 'option-btn';
                btn.innerText = opt.text;
                btn.onclick = () => {
                    if (opt.correct) {
                        activeSkills[current.targetSkill] = true;
                        document.getElementById(`cell-${current.targetSkill}`).classList.add('active');
                        nsatScore = Math.min(5.0, nsatScore + 0.2);
                    } else {
                        nsatScore = Math.max(1.0, nsatScore - 0.2);
                    }
                    pool.shift();
                    updateUI();
                };
                container.appendChild(btn);
            });
        }
        window.onload = updateUI;
    </script>
</body>
</html>
