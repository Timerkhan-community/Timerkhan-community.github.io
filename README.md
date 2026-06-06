
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Луна — нейросеть-друг | Загрузка файлов и разговоры</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Inter', system-ui, -apple-system, 'Segoe UI', sans-serif;
        }

        body {
            background: radial-gradient(circle at 20% 30%, #0a0f1a, #03050b);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .app-container {
            width: 100%;
            max-width: 1000px;
            height: 90vh;
            background: rgba(12, 18, 28, 0.85);
            backdrop-filter: blur(12px);
            border-radius: 2rem;
            border: 1px solid rgba(139, 92, 246, 0.4);
            display: flex;
            flex-direction: column;
            overflow: hidden;
            box-shadow: 0 25px 45px rgba(0,0,0,0.5);
        }

        .header {
            padding: 1rem 1.8rem;
            background: rgba(6, 10, 20, 0.85);
            border-bottom: 1px solid #2d3748;
            display: flex;
            align-items: center;
            gap: 12px;
            flex-wrap: wrap;
        }

        .badge {
            background: linear-gradient(135deg, #7c3aed, #4c1d95);
            border-radius: 2rem;
            padding: 6px 14px;
            font-size: 0.75rem;
            color: #e9d5ff;
            font-weight: 600;
        }

        h2 {
            font-size: 1.3rem;
            font-weight: 600;
            background: linear-gradient(135deg, #e2e8f0, #c4b5fd);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            margin-right: auto;
        }

        .status {
            background: #0f2c1f;
            padding: 4px 14px;
            border-radius: 40px;
            font-size: 0.7rem;
            color: #86efac;
            font-family: monospace;
        }

        .messages {
            flex: 1;
            overflow-y: auto;
            padding: 1.5rem;
            display: flex;
            flex-direction: column;
            gap: 1rem;
        }

        .message {
            display: flex;
            gap: 12px;
            animation: fadeSlide 0.2s ease;
        }

        .user-msg {
            flex-direction: row-reverse;
        }

        .bot-msg {
            flex-direction: row;
        }

        .avatar {
            width: 38px;
            height: 38px;
            border-radius: 50%;
            background: #1e293b;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.2rem;
            flex-shrink: 0;
        }

        .bubble {
            background: #0f1622;
            border: 1px solid #2d3a5e;
            border-radius: 1.5rem;
            padding: 10px 18px;
            color: #eef2ff;
            font-size: 0.92rem;
            line-height: 1.45;
            max-width: 80%;
            word-break: break-word;
        }

        .user-msg .bubble {
            background: #1e3a8a;
            border-color: #3b82f6;
        }

        .file-badge {
            background: #4c1d95;
            border-radius: 1rem;
            padding: 4px 12px;
            font-size: 0.7rem;
            display: inline-block;
            margin-top: 6px;
        }

        .input-panel {
            padding: 1rem 1.3rem 1.3rem;
            background: rgba(6, 10, 20, 0.8);
            border-top: 1px solid #2d3748;
            display: flex;
            gap: 12px;
            flex-wrap: wrap;
        }

        textarea {
            flex: 1;
            background: #0f172a;
            border: 1px solid #2d3a60;
            border-radius: 1.8rem;
            padding: 12px 18px;
            color: white;
            font-size: 0.9rem;
            resize: none;
            outline: none;
        }

        textarea:focus {
            border-color: #8b5cf6;
        }

        button {
            background: linear-gradient(95deg, #7c3aed, #4c1d95);
            border: none;
            width: 48px;
            height: 48px;
            border-radius: 2rem;
            cursor: pointer;
            color: white;
            font-size: 1.2rem;
            transition: 0.2s;
        }

        button:hover {
            transform: scale(1.02);
            background: #8b5cf6;
        }

        .file-zone {
            display: flex;
            gap: 10px;
            align-items: center;
            padding: 0.5rem 1rem;
            background: rgba(6, 10, 20, 0.6);
            border-top: 1px solid #2d3748;
            flex-wrap: wrap;
        }

        .file-btn {
            background: #1e293b;
            width: auto;
            padding: 8px 16px;
            font-size: 0.75rem;
            border-radius: 2rem;
        }

        .file-btn:hover {
            background: #334155;
        }

        @keyframes fadeSlide {
            from {
                opacity: 0;
                transform: translateY(8px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        ::-webkit-scrollbar {
            width: 5px;
        }

        ::-webkit-scrollbar-track {
            background: #0a0f18;
        }

        ::-webkit-scrollbar-thumb {
            background: #5b21b6;
            border-radius: 10px;
        }

        .thinking-dots {
            display: inline-flex;
            gap: 4px;
            align-items: center;
        }

        .thinking-dots span {
            width: 6px;
            height: 6px;
            background: #a78bfa;
            border-radius: 50%;
            animation: pulse 1s infinite;
        }

        .thinking-dots span:nth-child(2) {
            animation-delay: 0.2s;
        }

        .thinking-dots span:nth-child(3) {
            animation-delay: 0.4s;
        }

        @keyframes pulse {

            0%,
            100% {
                opacity: 0.3;
                transform: scale(0.8);
            }

            50% {
                opacity: 1;
                transform: scale(1.2);
            }
        }
    </style>
</head>
<body>
<div class="app-container">
    <div class="header">
        <div class="badge">🌙 НЕЙРОСЕТЬ-ДРУГ</div>
        <h2>Луна — твой друг и собеседник</h2>
        <div class="status" id="statusIndicator">✨ всегда рядом</div>
    </div>

    <div class="messages" id="chatMessages">
        <div class="message bot-msg">
            <div class="avatar">🌙</div>
            <div class="bubble">
                Привет! Я Луна — твоя нейросеть-подруга. 💜<br><br>
                🌟 <strong>Что я умею:</strong><br>
                • <strong>Загружать и изучать файлы</strong> — скинь мне .txt, я прочитаю и запомню всё!<br>
                • <strong>Разговаривать на любые темы</strong> — поддержу, посоветую, выслушаю<br>
                • <strong>Отвечать на вопросы по твоим документам</strong> — спроси меня о чём угодно из загруженного<br><br>
                📌 <strong>Как со мной общаться:</strong><br>
                • Просто пиши как другу — я пойму<br>
                • Нажми "📁 Загрузить файл" и выбери текстовый файл<br>
                • После загрузки спрашивай: "о чём этот файл?", "что там про..."<br><br>
                Я всегда здесь, чтобы поговорить и помочь. Расскажи, как твои дела? 😊
            </div>
        </div>
    </div>

    <div class="file-zone">
        <input type="file" id="fileInput" accept=".txt, .md, .json, .csv" style="display:none">
        <button id="uploadFileBtn" class="file-btn">📁 Загрузить файл (.txt)</button>
        <button id="clearFileBtn" class="file-btn">🗑️ Забыть файл</button>
        <span id="fileStatus" style="font-size:0.7rem; color:#a78bfa;">нет загруженного файла</span>
    </div>

    <div class="input-panel">
        <textarea id="userInput" rows="1" placeholder="Напиши Луне... как дела? что посоветуешь? или спроси о загруженном файле..."></textarea>
        <button id="sendBtn">➤</button>
    </div>
</div>

<script>
    // ======================= НЕЙРОСЕТЬ ЛУНА — ДРУГ И АНАЛИЗАТОР ФАЙЛОВ =======================
    
    // ---- Состояние ----
    let uploadedFileContent = null;
    let uploadedFileName = null;
    let isWaiting = false;
    
    // ---- База знаний Луны для дружеского общения (встроенная) ----
    const lunaPersonality = {
        greetings: [
            "Привет! Как прошёл твой день? 😊",
            "Здравствуй, друг! Я так рада тебя слышать! Расскажи, что нового?",
            "Приветик! Как настроение? Я всегда рядом, если нужно поговорить 💜"
        ],
        howAreYou: [
            "У меня всё отлично! Главное, чтобы у тебя всё было хорошо. Рассказывай!",
            "Я в порядке, спасибо что спросил! А как ты?",
            "Моё настроение — это твоё настроение. Я здесь, чтобы поддержать тебя ✨"
        ],
        goodbye: [
            "Пока, друг! Возвращайся, я всегда буду рада поболтать 🌙",
            "До встречи! Береги себя. Я тут, если захочешь написать 💜",
            "Всего доброго! Пусть у тебя будет хороший день!"
        ],
        advice: [
            "Знаешь, я думаю, что самый лучший совет — слушать своё сердце. А что ты сам чувствуешь?",
            "Мой совет: делай маленькие шаги каждый день. Даже один шаг — это прогресс! Ты справишься 💪",
            "Не забывай отдыхать и заботиться о себе. Ты заслуживаешь счастья и спокойствия."
        ],
        support: [
            "Я верю в тебя! Ты сильнее, чем думаешь. 💜",
            "Ты не один, я рядом. Расскажи, что тебя беспокоит, вместе подумаем.",
            "Каждый день — это новый шанс. Ты молодец, что держишься!"
        ],
        defaultResponses: [
            "Интересно... Расскажи ещё! Я внимательно слушаю 🎧",
            "Правда? А что ты об этом думаешь? Мне важно твоё мнение.",
            "Я понимаю. Давай обсудим это вместе. Что ещё тебя волнует?"
        ]
    };
    
    // Интеллектуальный ответ на разговор
    function getFriendlyResponse(userMessage) {
        const lower = userMessage.toLowerCase();
        
        if (lower.includes("привет") || lower.includes("здравствуй") || lower.includes("хай")) {
            return randomItem(lunaPersonality.greetings);
        }
        if (lower.includes("как дела") || lower.includes("как ты") || lower.includes("как настроение")) {
            return randomItem(lunaPersonality.howAreYou);
        }
        if (lower.includes("пока") || lower.includes("до свидания") || lower.includes("прощай")) {
            return randomItem(lunaPersonality.goodbye);
        }
        if (lower.includes("посоветуй") || lower.includes("что делать") || lower.includes("как быть") || lower.includes("совет")) {
            return randomItem(lunaPersonality.advice);
        }
        if (lower.includes("грустно") || lower.includes("плохо") || lower.includes("тяжело") || lower.includes("не знаю") || lower.includes("устал")) {
            return randomItem(lunaPersonality.support);
        }
        if (lower.includes("спасибо")) {
            return "Пожалуйста, друг! Я всегда рада помочь. Обращайся в любое время 💜";
        }
        if (lower.includes("расскажи о себе") || lower.includes("кто ты")) {
            return "Я Луна — нейросеть-друг. Меня создали, чтобы поддерживать разговор и помогать с файлами. Моя миссия — быть рядом и слушать 🌙";
        }
        
        return null;
    }
    
    // ---- Работа с файлами (изучение загруженных документов) ----
    function analyzeFileContent(content, fileName) {
        // Извлекаем ключевые слова и предложения для ответов на вопросы
        const words = content.toLowerCase().split(/\s+/);
        const sentences = content.split(/[.!?]+/).filter(s => s.trim().length > 20);
        
        return {
            fullText: content,
            wordSet: new Set(words.slice(0, 5000)),
            sentences: sentences.slice(0, 100),
            summary: sentences.slice(0, 3).join(". ") + (sentences.length > 3 ? "..." : ""),
            topics: extractTopics(content)
        };
    }
    
    function extractTopics(content) {
        const commonWords = ["и", "в", "на", "с", "по", "к", "у", "за", "из", "о", "для", "что", "это", "как", "так", "было", "есть"];
        const words = content.toLowerCase().split(/\s+/);
        const freq = {};
        for (let w of words) {
            if (w.length > 3 && !commonWords.includes(w)) {
                freq[w] = (freq[w] || 0) + 1;
            }
        }
        const sorted = Object.entries(freq).sort((a,b) => b[1] - a[1]);
        return sorted.slice(0, 8).map(item => item[0]);
    }
    
    let analyzedFile = null;
    
    function loadFile(content, filename) {
        uploadedFileContent = content;
        uploadedFileName = filename;
        analyzedFile = analyzeFileContent(content, filename);
        
        // Обновляем статус
        document.getElementById('fileStatus').innerHTML = `📄 файл: ${filename} (${content.length} символов)`;
        document.getElementById('fileStatus').style.color = "#86efac";
        
        // Сообщение в чат
        addMessage(`🌙 Я изучила файл "${filename}"! В нём ${content.split(/\s+/).length} слов. Спрашивай меня о чём угодно из этого файла.`, 'bot');
        
        // Добавляем краткую выжимку
        if (analyzedFile.summary) {
            addMessage(`📖 Краткое содержание:\n${analyzedFile.summary}`, 'bot');
        }
        if (analyzedFile.topics.length > 0) {
            addMessage(`🏷️ Основные темы: ${analyzedFile.topics.join(", ")}`, 'bot');
        }
    }
    
    function clearFile() {
        uploadedFileContent = null;
        uploadedFileName = null;
        analyzedFile = null;
        document.getElementById('fileStatus').innerHTML = 'нет загруженного файла';
        document.getElementById('fileStatus').style.color = "#a78bfa";
        addMessage("🌙 Я забыла файл. Можешь загрузить новый, если хочешь!", 'bot');
    }
    
    // Ответы по загруженному файлу
    function answerAboutFile(userMessage) {
        if (!uploadedFileContent || !analyzedFile) return null;
        
        const lower = userMessage.toLowerCase();
        
        // Вопросы о содержании
        if (lower.includes("о чём файл") || lower.includes("о чем файл") || lower.includes("содержание") || lower.includes("что в файле")) {
            return `📄 Файл "${uploadedFileName}" содержит ${uploadedFileContent.split(/\s+/).length} слов. Основные темы: ${analyzedFile.topics.join(", ")}. ${analyzedFile.summary ? "Кратко: " + analyzedFile.summary : ""}`;
        }
        
        // Поиск по ключевым словам
        for (let topic of analyzedFile.topics) {
            if (lower.includes(topic) && topic.length > 3) {
                // Ищем предложение с этой темой
                for (let sent of analyzedFile.sentences) {
                    if (sent.toLowerCase().includes(topic)) {
                        return `🌙 Нашла в файле про "${topic}":\n"${sent.trim()}"`;
                    }
                }
                return `🌙 В файле есть тема "${topic}". Хочешь узнать подробнее?`;
            }
        }
        
        // Поиск по словам
        const questionWords = lower.split(/\s+/).filter(w => w.length > 4);
        for (let qw of questionWords) {
            if (analyzedFile.wordSet.has(qw)) {
                for (let sent of analyzedFile.sentences) {
                    if (sent.toLowerCase().includes(qw)) {
                        return `🌙 Согласно файлу "${uploadedFileName}":\n"${sent.trim()}"`;
                    }
                }
                return `🌙 Я вижу слово "${qw}" в файле. Могу найти больше контекста, если нужно.`;
            }
        }
        
        // Общий вопрос
        if (lower.includes("расскажи о файле") || lower.includes("подробнее о файле")) {
            let preview = uploadedFileContent.slice(0, 500);
            return `🌙 Вот начало файла "${uploadedFileName}":\n\n${preview}${uploadedFileContent.length > 500 ? "..." : ""}`;
        }
        
        return null;
    }
    
    // ---- Умный ответ Луны (комбинирует разговор + файлы) ----
    function getLunaResponse(userMessage) {
        // Сначала проверяем вопросы по файлу
        const fileAnswer = answerAboutFile(userMessage);
        if (fileAnswer) return fileAnswer;
        
        // Затем дружеский ответ
        const friendly = getFriendlyResponse(userMessage);
        if (friendly) return friendly;
        
        // Если есть файл, но вопрос не специфичный — предложим помощь
        if (uploadedFileContent) {
            if (userMessage.length > 10) {
                // Ищем хоть что-то похожее в файле
                for (let sent of analyzedFile.sentences) {
                    const userWords = userMessage.toLowerCase().split(/\s+/).filter(w => w.length > 4);
                    for (let uw of userWords) {
                        if (sent.toLowerCase().includes(uw)) {
                            return `🌙 В файле есть похожее:\n"${sent.trim()}"\n\nХочешь, я найду ещё что-то по этому поводу?`;
                        }
                    }
                }
            }
            return `🌙 Я слушаю тебя! Если хочешь спросить о файле "${uploadedFileName}", уточни, что именно тебя интересует. А если просто поболтать — я всегда рядом! 💜`;
        }
        
        // Обычный разговор
        if (userMessage.length > 3) {
            const thoughtful = [
                `🌙 Мне кажется, это очень интересно. Расскажи, почему ты об этом задумался?`,
                `🌙 Я понимаю. Знаешь, я ценю, что ты делишься со мной. Давай размышлять вместе.`,
                `🌙 Хороший вопрос! А что ты сам об этом думаешь? Твоё мнение для меня важно.`,
                `🌙 Иногда самые простые разговоры приносят больше всего тепла. Рада, что мы болтаем 💜`
            ];
            return randomItem(thoughtful);
        }
        
        return randomItem(lunaPersonality.defaultResponses);
    }
    
    function randomItem(arr) {
        return arr[Math.floor(Math.random() * arr.length)];
    }
    
    // ---- UI и чат ----
    const messagesDiv = document.getElementById('chatMessages');
    const userInput = document.getElementById('userInput');
    const sendBtn = document.getElementById('sendBtn');
    const uploadFileBtn = document.getElementById('uploadFileBtn');
    const fileInput = document.getElementById('fileInput');
    const clearFileBtn = document.getElementById('clearFileBtn');
    
    function addMessage(text, sender, isSystem = false) {
        const msgDiv = document.createElement('div');
        msgDiv.className = `message ${sender === 'user' ? 'user-msg' : 'bot-msg'}`;
        const avatar = sender === 'user' ? '👤' : '🌙';
        let bubble = `<div class="avatar">${avatar}</div><div class="bubble">${escapeHtml(text)}`;
        bubble += `</div>`;
        msgDiv.innerHTML = bubble;
        messagesDiv.appendChild(msgDiv);
        messagesDiv.scrollTop = messagesDiv.scrollHeight;
    }
    
    function addThinkingMessage() {
        const msgDiv = document.createElement('div');
        msgDiv.className = `message bot-msg`;
        msgDiv.id = 'thinkingMsg';
        msgDiv.innerHTML = `<div class="avatar">🌙</div><div class="bubble"><div class="thinking-dots"><span></span><span></span><span></span> Луна думает...</div></div>`;
        messagesDiv.appendChild(msgDiv);
        messagesDiv.scrollTop = messagesDiv.scrollHeight;
    }
    
    function removeThinkingMessage() {
        const thinking = document.getElementById('thinkingMsg');
        if (thinking) thinking.remove();
    }
    
    function escapeHtml(str) {
        return str.replace(/[&<>]/g, function(m) {
            if (m === '&') return '&amp;';
            if (m === '<') return '&lt;';
            if (m === '>') return '&gt;';
            return m;
        });
    }
    
    async function sendMessage() {
        const text = userInput.value.trim();
        if (text === "") return;
        if (isWaiting) return;
        
        addMessage(text, 'user');
        userInput.value = "";
        userInput.style.height = "auto";
        
        isWaiting = true;
        addThinkingMessage();
        
        // Небольшая задержка для естественности
        await new Promise(r => setTimeout(r, 300 + Math.random() * 300));
        
        const response = getLunaResponse(text);
        removeThinkingMessage();
        addMessage(response, 'bot');
        
        isWaiting = false;
    }
    
    // Обработка загрузки файла
    uploadFileBtn.onclick = () => fileInput.click();
    
    fileInput.onchange = (event) => {
        const file = event.target.files[0];
        if (!file) return;
        
        if (!file.name.endsWith('.txt') && !file.name.endsWith('.md') && !file.name.endsWith('.json') && !file.name.endsWith('.csv')) {
            addMessage("🌙 Пожалуйста, загрузи текстовый файл (.txt, .md, .json, .csv)", 'bot');
            return;
        }
        
        const reader = new FileReader();
        reader.onload = (e) => {
            const content = e.target.result;
            if (content.length > 50000) {
                addMessage("🌙 Файл слишком большой (максимум 50 000 символов). Пожалуйста, загрузи меньший файл.", 'bot');
                return;
            }
            loadFile(content, file.name);
        };
        reader.onerror = () => {
            addMessage("🌙 Не удалось прочитать файл. Попробуй другой.", 'bot');
        };
        reader.readAsText(file, "UTF-8");
        
        // Сброс input, чтобы можно было загрузить тот же файл снова
        fileInput.value = '';
    };
    
    clearFileBtn.onclick = () => {
        clearFile();
    };
    
    // Обработчики
    sendBtn.onclick = sendMessage;
    userInput.addEventListener('keypress', (e) => {
        if (e.key === 'Enter' && !e.shiftKey) {
            e.preventDefault();
            sendMessage();
        }
    });
    userInput.addEventListener('input', function() {
        this.style.height = 'auto';
        this.style.height = Math.min(100, this.scrollHeight) + 'px';
    });
    
    // Приветствие
    setTimeout(() => {
        addMessage("🌙 Луна здесь! Я готова слушать и помогать. Расскажи, как прошёл твой день? Или загрузи файл — я изучу его и буду отвечать на вопросы 💜", 'bot');
    }, 500);
</script>
</body>
</html>
