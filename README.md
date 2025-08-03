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
    const chatContainer = document.getElementById('chat-container');
    const input = document.getElementById('input');
    const sendBtn = document.getElementById('send');
    const clearBtn = document.getElementById('clear');
    const helpBtn = document.getElementById('help');
    let devMode = false;
    const devPassword = 'legendarytriosuleimenbekanurali090429nuralisilaayanmogila2010sila2009mogila';

    function addMessage(sender, text, className) {
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
      if (!msg) {
        addMessage('Neura', 'Йо, брат, напиши что-нибудь! 😊', 'bot');
        return;
      }
      addMessage('Вы', msg, 'user');
      respondToMessage(msg);
      input.value = '';
    }

    sendBtn.addEventListener('click', handleInput);
    input.addEventListener('keypress', (e) => {
      if (e.key === 'Enter') handleInput();
    });
    clearBtn.addEventListener('click', () => {
      chatContainer.innerHTML = '';
      devMode = false;
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
          анекдот: ['Почему программист предпочитает тёмную тему? Светлый режим напоминает о счёте за свет! 😂 Ещё?', 'Кодер заходит в бар и говорит: "Синтаксис без ошибок!" Бармен: "Похер, наливаю!" 😜'],
          код: ['Вот тебе HTML: <pre>&lt;div&gt;Йо, брат!&lt;/div&gt;</pre> Ещё пример?', 'Код — это магия. Что создаём? 💻']
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
          анекдот: ['Почему программисты любят тёмный режим? Потому что свет притягивает багов! 😂 Ещё один?', 'Что сказал программист жене? "Ты — мой CSS, без тебя всё ломается!" 😄 Нравится?'],
          код: ['Вот простой HTML: <pre>&lt;div&gt;Привет!&lt;/div&gt;</pre> Нужен ещё пример? 💻', 'Код — это сила! Что хочешь создать? 😎']
        };
        for (const [key, options] of Object.entries(responses)) {
          if (lowerMsg.includes(key)) {
            response = options[Math.floor(Math.random() * options.length)];
            break;
          }
        }
        if (!response) response = `Хм, "${msg}" — интересный вопрос! Расскажи побольше? 🤓`;
      }

      setTimeout(() => addMessage('Neura', response, className), 500);
    }

    input.focus();
  </script>
</body>
</html>
