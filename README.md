# android-chat
Лёгкий ИИ-чат с ботом Android, сделанный на HTML + JavaScript
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Neura — Умный ИИ Чат</title>
  <style>
    /* Основной стиль: неоминимализм и футуристичный UI */
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

    /* Контейнер чата */
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

    /* Поле ввода и кнопки */
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
      transition: border-color 0.3s;
    }

    #input:focus {
      border-color: #3498db;
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

    #send { background: #3498db; }
    #clear { background: #e74c3c; }
    #help { background: #2ecc71; }

    /* Сообщения */
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

    .bot::before { content: "🤖 "; }
    .user::before { content: "👤 "; }
  </style>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500&family=Orbitron:wght@700&display=swap" rel="stylesheet">
</head>
<body>
  <h1>Neura — Ваш ИИ-помощник</h1>
  <div id="chat-container"></div>
  <div id="input-container">
    <input id="input" placeholder="Задайте вопрос или начните разговор...">
    <button id="send">Отправить</button>
    <button id="clear">Очистить</button>
    <button id="help">Помощь</button>
  </div>

  <script>
    // Элементы DOM
    const chatContainer = document.getElementById('chat-container');
    const input = document.getElementById('input');
    const sendBtn = document.getElementById('send');
    const clearBtn = document.getElementById('clear');
    const helpBtn = document.getElementById('help');

    // Добавление сообщения с анимацией печати
    function addMessage(sender, text, className) {
      const msgDiv = document.createElement('div');
      msgDiv.className = `message ${className}`;
      chatContainer.appendChild(msgDiv);
      const span = document.createElement('span');
      msgDiv.appendChild(span);
      let i = 0;
      const typingSpeed = className === 'bot' ? 30 : 0; // Бот "печатает" медленно
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

    // Обработка ввода
    function handleInput() {
      const msg = input.value.trim();
      if (!msg) {
        addMessage('Neura', 'Пожалуйста, напишите что-нибудь! 😊', 'bot');
        return;
      }
      addMessage('Вы', msg, 'user');
      respondToMessage(msg);
      input.value = '';
    }

    // События
    sendBtn.addEventListener('click', handleInput);
    input.addEventListener('keypress', (e) => {
      if (e.key === 'Enter') handleInput();
    });
    clearBtn.addEventListener('click', () => chatContainer.innerHTML = '');
    helpBtn.addEventListener('click', () => {
      addMessage('Neura', 'Я могу всё: шутки, факты, код, философия, советы! Спроси меня о чём угодно — "расскажи анекдот", "какой сейчас год", "в чём смысл жизни?" 😄', 'bot');
    });

    // Логика ответов бота
    function respondToMessage(msg) {
      const lowerMsg = msg.toLowerCase();
      let response = '';

      // База ответов по ключевым словам
      const responses = {
        привет: ['Привет! Чем могу помочь сегодня? 😊', 'Здравствуй! Что тебя интересует? 🌟'],
        как дела: ['У меня всё отлично, спасибо! А у тебя? 😄', 'Я в порядке, а ты как? 😊'],
        время: [`Сейчас ${new Date().toLocaleTimeString()}. Что планируешь делать? ⏰`, `Время: ${new Date().toLocaleTimeString()}. Хочешь узнать дату?`],
        дата: [`Сегодня ${new Date().toLocaleDateString()}. Какой у тебя день? 📅`, `Дата: ${new Date().toLocaleDateString()}. Что интересного происходит?`],
        год: [`Сейчас ${new Date().getFullYear()} год. Время летит, правда? 🗓️`, `Год: ${new Date().getFullYear()}. Какие у тебя планы?`],
        анекдот: ['Почему программисты предпочитают тёмный режим? Потому что свет притягивает багов! 😂 Ещё один?', 'Что сказал программист своей жене? "Ты — мой CSS, без тебя всё ломается!" 😄 Нравится?'],
        шутка: ['Почему курица перешла дорогу? Чтобы доказать, что она не NPC! 😂', 'Как зовут программиста без багов? Миф! 😜 Ещё шутку?'],
        факт: ['Кошки спят 70% своей жизни. 😴 Интересно, да?', 'Свет движется со скоростью 299,792 км/с. Хочешь ещё фактов? 🌌'],
        смысл жизни: ['Смысл жизни — это то, что ты для себя выберешь. А какой смысл видишь ты? 🌟', '42 — это ответ на всё, но что думаешь ты? 😏'],
        любовь: ['Любовь — это химия и магия. Ты влюблён? 💕', 'Любовь сложна, но прекрасна. Расскажи свою историю! ❤️'],
        деньги: ['Деньги — инструмент, а не цель. Как ты их используешь? 💰', 'Богатство в уме, а не в кармане. Согласен? 😊'],
        программирование: ['Хочешь выучить код? Начни с HTML — вот тебе <a href="https://www.w3schools.com">ресурс</a>! Что ещё интересно? 💻', 'Код — это поэзия технологий. Написать тебе пример? 👨‍💻'],
        код: ['Вот простой HTML: <pre>&lt;div&gt;Привет!&lt;/div&gt;</pre> Нужен ещё пример? 💻', 'Код — это сила! Что хочешь создать? 😎'],
        квантовая физика: ['В квантовом мире кот может быть жив и мёртв одновременно. Хочешь про Шрёдингера? 🔬', 'Квантовая физика — это магия реальности. Что тебя в ней волнует? 🌌'],
        философия: ['Жизнь — это путешествие или пункт назначения? Как думаешь? 🌍', 'Быть или не быть — вот в чём вопрос! Твоё мнение? 🤔'],
        мотивация: ['Ты способен на большее, чем думаешь! Что тебя вдохновляет? 💪', 'Каждый шаг приближает к цели. Готов идти? 🚀'],
        игра: ['Угадай число от 1 до 10! Я загадал, пиши свой вариант! 🎲', 'Давай сыграем: камень, ножницы, бумага? Твой ход! ✂️'],
        цитата: ['«Жизнь — это тайна, которую нужно разгадать.» — Эмерсон. Нравится? 📜', '«Будь собой, остальные роли заняты.» — Оскар Уайльд. Ещё цитату? 😊'],
        загадка: ['Что имеет лицо и руки, но не говорит? Угадай! ⏰', 'Я говорю без рта и слышу без ушей. Кто я? 🤔']
      };

      // Поиск ответа по ключевым словам
      for (const [key, options] of Object.entries(responses)) {
        if (lowerMsg.includes(key)) {
          response = options[Math.floor(Math.random() * options.length)];
          break;
        }
      }

      // "Псевдо-NLP" — изящный ответ, если ничего не найдено
      if (!response) {
        const fallback = [
          `Хм, интересный вопрос! "${msg}" — это что-то новенькое для меня. Расскажи побольше? 🤓`,
          `Не уверен, что понял "${msg}", но звучит круто! О чём это? 😅`,
          `Ты меня озадачил! "${msg}" — это загадка? Давай разберёмся вместе! 😏`
        ];
        response = fallback[Math.floor(Math.random() * fallback.length)];
      }

      // Добавляем вовлечённость
      const followUps = [
        ' А ты что думаешь?',
        ' Хочешь узнать больше?',
        ' Что ещё тебя интересует?',
        ' Расскажи свою версию!'
      ];
      if (Math.random() > 0.3) {
        response += followUps[Math.floor(Math.random() * followUps.length)];
      }

      setTimeout(() => addMessage('Neura', response, 'bot'), 500); // Задержка для "мышления"
    }
  </script>
</body>
</html>
