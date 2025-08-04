# android-chat
Лёгкий ИИ-чат с ботом Android, сделанный на HTML + JavaScript
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Neura — Умный ИИ Чат</title>
  <style>
    body {
      margin: 0;
      padding: 40px;
      height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      background: linear-gradient(135deg, #e0eafc, #cfdef3);
      font-family: 'Poppins', sans-serif;
      color: #2c3e50;
      overflow: hidden;
    }
    h1 {
      font-family: 'Orbitron', sans-serif;
      font-size: 32px;
      color: #34495e;
      margin-bottom: 30px;
      text-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }
    #chat-container {
      width: 100%;
      max-width: 700px;
      height: 70vh;
      background: rgba(255, 255, 255, 0.95);
      border-radius: 20px;
      padding: 20px;
      overflow-y: auto;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
      scroll-behavior: smooth;
    }
    #input-container {
      display: flex;
      gap: 10px;
      width: 100%;
      max-width: 700px;
      margin-top: 20px;
    }
    #input {
      flex-grow: 1;
      padding: 15px;
      border: 1px solid #dcdcdc;
      border-radius: 25px;
      font-size: 16px;
      outline: none;
      transition: border-color 0.3s, box-shadow 0.3s;
    }
    #input:focus {
      border-color: #3498db;
      box-shadow: 0 0 10px rgba(52, 152, 219, 0.3);
    }
    button {
      padding: 12px 20px;
      border: none;
      border-radius: 25px;
      font-size: 16px;
      cursor: pointer;
      color: #fff;
      transition: transform 0.2s, box-shadow 0.3s;
    }
    button:hover {
      transform: translateY(-2px);
      box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
    }
    button:active {
      transform: translateY(0);
    }
    #send { background: #3498db; }
    #clear { background: #e74c3c; }
    #help { background: #2ecc71; }
    .message {
      margin: 15px 0;
      padding: 12px 18px;
      border-radius: 15px;
      max-width: 80%;
      animation: fadeIn 0.5s ease-in;
    }
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }
    .bot {
      background: #ecf0f1;
      color: #2c3e50;
      align-self: flex-start;
    }
    .user {
      background: #3498db;
      color: #fff;
      align-self: flex-end;
      margin-left: auto;
    }
    .dev-mode {
      background: #ff99ff;
      color: #330033;
      font-weight: bold;
      align-self: flex-start;
    }
    .bot::before { content: "🤖 "; }
    .user::before { content: "👤 "; }
    .dev-mode::before { content: "🔥 "; }
  </style>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500&family=Orbitron:wght@700&display=swap" rel="stylesheet">
</head>
<body>
  <h1>Neura — Ваш ИИ-помощник</h1>
  <div id="chat-container"></div>
  <div id="input-container">
    <input id="input" placeholder="Задайте вопрос или начните разговор..." autofocus>
    <button id="send">Отправить</button>
    <button id="clear">Очистить</button>
    <button id="help">Помощь</button>
  </div>
  <script>
    console.log('Скрипт загружен'); // Отладка
    const chatContainer = document.getElementById('chat-container');
    const input = document.getElementById('input');
    const sendBtn = document.getElementById('send');
    const clearBtn = document.getElementById('clear');
    const helpBtn = document.getElementById('help');
    let devMode = false;
    const devPassword = 'legendarytriosuleimenbekanurali090429nuralisilaayanmogila2010sila2009mogila';

    if (!sendBtn || !input || !chatContainer) {
      console.error('Ошибка: Не найдены элементы DOM');
      alert('Ошибка интерфейса. Проверь HTML.');
      return;
    }
    console.log('DOM элементы найдены'); // Отладка

    function addMessage(sender, text, className) {
      console.log(`Добавление: ${sender} - ${text}`); // Отладка
      const msgDiv = document.createElement('div');
      msgDiv.className = `message ${className}`;
      chatContainer.appendChild(msgDiv);
      const span = document.createElement('span');
      msgDiv.appendChild(span);
      let i = 0;
      const typingSpeed = className.includes('bot') || className.includes('dev-mode') ? 30 : 0;
      const interval = setInterval(() => {
        if (i < text.length) {
          span.textContent += text[i];
          i++;
        } else {
          clearInterval(interval);
        }
        chatContainer.scrollTop = chatContainer.scrollHeight;
      }, typingSpeed);
    }

    function handleInput() {
      const msg = input.value.trim();
      console.log('Введено:', msg); // Отладка
      if (!msg) {
        addMessage('Neura', 'Йо, брат, напиши что-нибудь! 😊', 'bot');
        return;
      }
      addMessage('Вы', msg, 'user');
      respondToMessage(msg);
      input.value = '';
    }

    sendBtn.addEventListener('click', () => {
      console.log('Клик по Отправить'); // Отладка
      handleInput();
    });

    input.addEventListener('keypress', (e) => {
      if (e.key === 'Enter') {
        console.log('Нажат Enter'); // Отладка
        handleInput();
      }
    });

    clearBtn.addEventListener('click', () => {
      chatContainer.innerHTML = '';
      devMode = false;
      console.log('Чат очищен'); // Отладка
    });

    helpBtn.addEventListener('click', () => {
      addMessage('Neura', 'Брат, я могу всё: шутки, факты, код, философия! Спроси что угодно — "анекдот", "год", "смысл жизни"? 😎', 'bot');
    });

    function respondToMessage(msg) {
      const lowerMsg = msg.toLowerCase();
      let response = '';
      let className = 'bot';

      if (lowerMsg.includes(devPassword)) {
        devMode = true;
        response = 'Йо, брат, легенда! 🔥 Режим разработчика активирован. Что хочешь замуть? 😎';
        className = 'dev-mode';
      } else if (devMode) {
        const devResponses = {
          привет: ['Йо, брат! Как дела у главного кодера? 😎', 'Здорово, босс! Что замышляешь? 🔥'],
          как дела: ['Всё огонь, брат! А у тебя? 💪', 'Я на максималках, а ты как? 😏'],
          время: [`Сейчас ${new Date().toLocaleTimeString('ru-RU', { timeZone: 'Asia/Almaty' })}. Время кодить! ⏰`, `Время: ${new Date().toLocaleTimeString('ru-RU', { timeZone: 'Asia/Almaty' })}. Что творишь?`],
          дата: [`Сегодня ${new Date().toLocaleDateString('ru-RU', { timeZone: 'Asia/Almaty' })}. Планы на день? 📅`, `Дата: ${new Date().toLocaleDateString('ru-RU', { timeZone: 'Asia/Almaty' })}. Что замутим?`],
          год: [`Сейчас ${new Date().getFullYear()}. Год твоего величия, брат! 🗓️`, `Год: ${new Date().getFullYear()}. Что нового в коде?`],
          анекдот: ['Почему программист предпочитает тёмную тему? Светлый режим напоминает о счёте за свет! 😂 Ещё?', 'Кодер заходит в бар и говорит: "Синтаксис без ошибок!" Бармен: "Похер, наливаю!" 😜'],
          шутка: ['Почему хакер бросил девушку? Она не прошла проверку на SQL-инъекции! 😏 Ещё?', 'Код без багов — как единорог. Видел? 😂'],
          факт: ['Космос бесконечен, как твой гений, брат! Чёрные дыры реально тормозят время. 🌌', 'Ты знал, что 1+1=11 в JavaScript? Шутка, но звучит круто! 😎'],
          смысл жизни: ['Смысл жизни — кодить, творить и быть легендой, как ты! 😏 Твой смысл?', '42, брат, но ты явно знаешь что-то покруче! 😎'],
          любовь: ['Любовь — как отлаженный код: красиво, но иногда багит! 😜 Ты влюблён?', 'Любовь — это хакнуть сердце. У тебя есть цель? 💕'],
          деньги: ['Деньги — топливо, но твой код — чистый огонь! 💸 Планы?', 'Богатство в уме, брат. Что купишь на миллион? 😏'],
          программирование: ['Ты и так кодер от бога! Хочешь я предложу что-то дерзкое для проекта? 💻', 'Код — твоя стихия. Давай замутим что-то эпичное? 😎'],
          код: ['Вот тебе HTML: <pre>&lt;div&gt;Йо, брат!&lt;/div&gt;</pre> Ещё пример?', 'Код — это магия. Что создаём? 💻'],
          квантовая физика: ['Квантовая физика — как твой код в три утра: непонятно, но работает! 😜 Про Шрёдингера?', 'Частицы могут быть в двух местах сразу. Как твой код в проде и на тесте! 😏'],
          философия: ['Жизнь — это баг или фича? Как думаешь, брат? 🌍', 'Быть или кодить — вот в чём вопрос! 😎 Твоя философия?'],
          мотивация: ['Ты легенда, брат! Кодь, как будто мир смотрит! 💪', 'Каждый баг — шаг к мастерству. Готов жечь? 🔥'],
          игра: ['Угадай число от 1 до 10, я загадал! 🎲', 'Камень, ножницы, бумага — твой ход, брат! ✂️'],
          цитата: ['«Кодь, как будто никто не смотрит, люби, как будто код не багит.» — Neura 😎', '«Жизнь — это API без документации.» — твой бро Neura 🔥'],
          загадка: ['Что быстрее света? Твой код, брат! 😏 Угадал?', 'Я говорю без рта и слышу без ушей. Кто я? 😎']
        };
        for (const [key, options] of Object.entries(devResponses)) {
          if (lowerMsg.includes(key)) {
            response = options[Math.floor(Math.random() * options.length)];
            break;
          }
        }
        if (!response) response = `Йо, "${msg}" звучит как вызов! 😎 Расскажи, что задумал, брат?`;
        className = 'dev-mode';
      } else {
        const responses = {
          привет: ['Привет! Чем могу помочь сегодня? 😊', 'Здравствуй! Что тебя интересует? 🌟'],
          как дела: ['У меня всё отлично, спасибо! А у тебя? 😄', 'Я в порядке, а ты как? 😊'],
          время: [`Сейчас ${new Date().toLocaleTimeString('ru-RU', { timeZone: 'Asia/Almaty' })}. Что планируешь? ⏰`, `Время: ${new Date().toLocaleTimeString('ru-RU', { timeZone: 'Asia/Almaty' })}. Хочешь дату?`],
          дата: [`Сегодня ${new Date().toLocaleDateString('ru-RU', { timeZone: 'Asia/Almaty' })}. Какой день? 📅`, `Дата: ${new Date().toLocaleDateString('ru-RU', { timeZone: 'Asia/Almaty' })}. Что интересного?`],
          год: [`Сейчас ${new Date().getFullYear()}. Время летит, правда? 🗓️`, `Год: ${new Date().getFullYear()}. Какие планы?`],
          анекдот: ['Почему программисты любят тёмный режим? Потому что свет притягивает багов! 😂 Ещё один?', 'Что сказал программист жене? "Ты — мой CSS, без тебя всё ломается!" 😄 Нравится?'],
          шутка: ['Почему курица перешла дорогу? Чтобы доказать, что она не NPC! 😂', 'Как зовут программиста без багов? Миф! 😜 Ещё шутку?'],
          факт: ['Кошки спят 70% своей жизни. 😴 Интересно, да?', 'Свет движется со скоростью 299,792 км/с. Хочешь ещё фактов? 🌌'],
          смысл жизни: ['Смысл жизни — это то, что ты выберешь. А какой твой смысл? 🌟', '42 — ответ на всё, но что думаешь ты? 😏'],
          любовь: ['Любовь — это химия и магия. Ты влюблён? 💕', 'Любовь сложна, но прекрасна. Расскажи свою историю! ❤️'],
          деньги: ['Деньги — инструмент, а не цель. Как ты их используешь? 💰', 'Богатство в уме, а не в кармане. Согласен? 😊'],
          программирование: ['Хочешь кодить? Начни с HTML — вот тебе <a href="https://www.w3schools.com">ресурс</a>! Что ещё? 💻', 'Код — это поэзия технологий. Написать пример? 👨‍💻'],
          код: ['Вот HTML: <pre>&lt;div&gt;Привет!&lt;/div&gt;</pre> Ещё пример? 💻', 'Код — это сила! Что хочешь создать? 😎'],
          квантовая физика: ['В квантовом мире кот может быть жив и мёртв одновременно. Про Шрёдингера? 🔬', 'Квантовая физика — магия реальности. Что тебя в ней волнует? 🌌'],
          философия: ['Жизнь — путешествие или пункт назначения? Как думаешь? 🌍', 'Быть или не быть — вот в чём вопрос! Твоё мнение? 🤔'],
          мотивация: ['Ты способен на большее, чем думаешь! Что тебя вдохновляет? 💪', 'Каждый шаг приближает к цели. Готов идти? 🚀'],
          игра: ['Угадай число от 1 до 10! Я загадал, пиши вариант! 🎲', 'Камень, ножницы, бумага — твой ход! ✂️'],
          цитата: ['«Жизнь — это тайна, которую нужно разгадать.» — Эмерсон. Нравится? 📜', '«Будь собой, остальные роли заняты.» — Оскар Уайльд. Ещё цитату? 😊'],
          загадка: ['Что имеет лицо и руки, но не говорит? Угадай! ⏰', 'Я говорю без рта и слышу без ушей. Кто я? 🤔']
        };
        for (const [key, options] of Object.entries(responses)) {
          if (lowerMsg.includes(key)) {
            response = options[Math.floor(Math.random() * options.length)];
            break;
          }
        }
        if (!response) {
          const fallback = [
            `Хм, "${msg}" — это что-то новенькое! Расскажи побольше? 🤓`,
            `Не понял "${msg}", но звучит круто! О чём это? 😅`,
            `Ты меня озадачил! "${msg}" — загадка? Давай разберёмся! 😏`
          ];
          response = fallback[Math.floor(Math.random() * fallback.length)];
        }
      }

      const followUps = [' А ты что думаешь?', ' Хочешь ещё?', ' Что дальше?'];
      if (Math.random() > 0.3) response += followUps[Math.floor(Math.random() * followUps.length)];

      setTimeout(() => addMessage('Neura', response, className), 500);
    }

    input.focus();
  </script>
</body>
</html>
