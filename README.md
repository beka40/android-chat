# android-chat
Лёгкий ИИ-чат с ботом Android, сделанный на HTML + JavaScript
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>Android — ИИ Чат</title>
  <script src="https://unpkg.com/compromise@latest/builds/compromise.min.js"></script>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: #101820;
      color: #f1f1f1;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 40px;
    }
    h1 {
      color: #00ffcc;
    }
    #chat {
      background: #1e1e2f;
      border-radius: 10px;
      padding: 20px;
      width: 100%;
      max-width: 500px;
      height: 400px;
      overflow-y: auto;
      margin-bottom: 20px;
    }
    input {
      padding: 10px;
      width: 100%;
      max-width: 500px;
      border-radius: 5px;
      border: none;
      font-size: 16px;
    }
    .msg {
      margin: 10px 0;
    }
    .bot {
      color: #00ffcc;
    }
    .user {
      color: #ffd700;
    }
  </style>
</head>
<body>
  <h1>🤖 Android — твой ИИ помощник</h1>
  <div id="chat"></div>
  <input id="input" placeholder="Напиши что-нибудь..." onkeypress="handleKey(event)" />
  <script>
    const chat = document.getElementById('chat');

    function handleKey(e) {
      if (e.key === 'Enter') {
        const input = document.getElementById('input');
        const msg = input.value.trim();
        if (msg) {
          addMessage('Ты', msg, 'user');
          respond(msg);
          input.value = '';
        }
      }
    }

    function addMessage(from, text, cls) {
      const div = document.createElement('div');
      div.className = 'msg ' + cls;
      div.innerHTML = `<strong>${from}:</strong> ${text}`;
      chat.appendChild(div);
      chat.scrollTop = chat.scrollHeight;
    }

    function respond(msg) {
      const doc = nlp(msg);
      let reply = "Я ещё учусь понимать такие сообщения.";

      if (doc.has('привет') || doc.has('здравствуй')) {
        reply = "Привет! Чем могу помочь?";
      } else if (doc.has('как дела') || doc.has('как ты')) {
        reply = "Отлично! Я готов помочь тебе.";
      } else if (doc.has('сайт') || doc.has('веб')) {
        reply = "Я могу помочь тебе создать сайт! Хочешь пример кода?";
      } else if (doc.has('время') || doc.has('час')) {
        reply = `Сейчас ${new Date().toLocaleTimeString('ru-RU')}`;
      } else if (doc.has('имя') || doc.has('кто ты')) {
        reply = "Меня зовут Android — твой ИИ-помощник, созданный легендарным разработчиком 😉";
      } else if (doc.topics().length > 0) {
        const topic = doc.topics().out('array')[0];
        reply = `О, ты говоришь про ${topic}? Расскажи подробнее!`;
      } else if (doc.questions().length > 0) {
        reply = "Хороший вопрос! Дай мне секунду подумать...";
      } else {
        reply = "Интересно! Расскажи больше, я учусь на лету!";
      }

      setTimeout(() => addMessage('Android', reply, 'bot'), 500);
    }
  </script>
</body>
</html>
