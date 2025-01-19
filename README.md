<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Калькулятор стоимости</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Poppins', Arial, sans-serif;
            font-weight: 600;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #ff9a9e, #fad0c4);
            color: #333;
        }

        h1 {
            margin-bottom: 20px;
            font-size: 1.6rem;
            text-align: center;
            color: white;
            transition: all 0.3s ease-in-out;
        }

        form {
            display: flex;
            flex-direction: column;
            gap: 15px;
            background: #fff;
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
            width: 100%;
            max-width: 350px; /* Уменьшена ширина формы */
        }

        label {
            font-size: 1rem;
            color: #555;
        }

        .radio-group {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .radio-group div {
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 0.9rem;
            color: #555;
        }

        .radio-group input[type="radio"] {
            width: 18px;
            height: 18px;
            appearance: none;
            border-radius: 50%;
            border: 2px solid #ddd;
            cursor: pointer;
            transition: background 0.3s, border-color 0.3s;
        }

        .radio-group input[type="radio"]:checked {
            background-color: #ff6f61;
            border-color: #ff6f61;
        }

        select, input {
            padding: 10px;
            font-size: 1rem;
            border: 1px solid #ddd;
            border-radius: 8px;
            outline: none;
            transition: border-color 0.3s;
        }

        select:focus, input:focus {
            border-color: #ff6f61;
        }

        button {
            padding: 12px;
            font-size: 1rem;
            background: linear-gradient(135deg, #ff6f61, #de6262);
            color: white;
            border: none;
            border-radius: 12px;
            cursor: pointer;
            transition: background 0.3s, transform 0.2s;
        }

        button:hover {
            background: linear-gradient(135deg, #de6262, #bf4c4c);
        }

        button:active {
            transform: scale(0.98);
        }

        .close-btn {
            margin-top: 15px;
            padding: 10px;
            font-size: 1rem;
            background-color: #ff6f61;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            width: 80%; /* Уменьшена ширина кнопки закрытия */
        }

        .close-btn:hover {
            background-color: #de6262;
        }

        .hidden {
            display: none;
        }

        .result {
            margin-top: 15px;
            font-size: 1rem;
            color: #333;
            text-align: center;
        }

        .social-logo {
            width: 20px;
        }
    </style>
</head>
<body>
    <h1 id="formTitle">Калькулятор стоимости</h1>
    <form id="mainForm">
        <label for="projectType">Тип проекта:</label>
        <select id="projectType">
            <option value="site">🌐 Сайт</option>
            <option value="chatbot">🤖 Чат-бот</option>
            <option value="webapp">📱 Веб-приложение</option>
        </select>

        <div id="complexitySection">
            <label for="siteType">Тип сайта:</label>
            <div class="radio-group">
                <div>
                    <input type="radio" id="singlePage" name="siteType" value="singlePage">
                    <label for="singlePage">Одностраничный сайт</label>
                </div>
                <div>
                    <input type="radio" id="multiPage" name="siteType" value="multiPage">
                    <label for="multiPage">Многостраничный сайт</label>
                </div>
                <div>
                    <input type="radio" id="onlineStore" name="siteType" value="onlineStore">
                    <label for="onlineStore">Интернет-магазин</label>
                </div>
            </div>
        </div>

        <div id="chatbotSection" class="hidden">
            <label>Социальные сети:</label>
            <div>
                <input type="checkbox" id="telegram" value="telegram">
                <img class="social-logo" src="https://upload.wikimedia.org/wikipedia/commons/8/82/Telegram_logo.svg" alt="Telegram"> Telegram
            </div>
            <div>
                <input type="checkbox" id="whatsapp" value="whatsapp">
                <img class="social-logo" src="https://upload.wikimedia.org/wikipedia/commons/6/6b/WhatsApp.svg" alt="WhatsApp"> WhatsApp
            </div>
        </div>

        <label for="currency">Валюта:</label>
        <select id="currency">
            <option value="rub">🇷🇺 Рубль</option>
            <option value="usd">🇺🇸 Доллар</option>
        </select>

        <button type="button" id="calculateBtn">🔢 Рассчитать стоимость</button>
    </form>

    <div class="result" id="result"></div>

    <button class="close-btn" onclick="Telegram.WebApp.close()">Закрыть</button>

    <script>
        const tg = window.Telegram.WebApp;
        tg.ready();

        const formTitle = document.getElementById("formTitle");
        const projectType = document.getElementById("projectType");
        const complexitySection = document.getElementById("complexitySection");
        const chatbotSection = document.getElementById("chatbotSection");
        const result = document.getElementById("result");

        projectType.addEventListener("change", () => {
            const projectValue = projectType.value;

            // Изменяем заголовок
            if (projectValue === "site") {
                formTitle.textContent = "Калькулятор стоимости сайта";
                complexitySection.classList.remove("hidden");
                chatbotSection.classList.add("hidden");
            } else if (projectValue === "chatbot") {
                formTitle.textContent = "Калькулятор стоимости чат-бота";
                complexitySection.classList.add("hidden");
                chatbotSection.classList.remove("hidden");
            } else {
                formTitle.textContent = "Калькулятор стоимости веб-приложения";
                complexitySection.classList.add("hidden");
                chatbotSection.classList.add("hidden");
            }
        });

        document.getElementById("calculateBtn").addEventListener("click", () => {
            const projectValue = projectType.value;
            const currency = document.getElementById("currency").value;
            let cost = 0;

            if (projectValue === "site") cost = 500;
            if (projectValue === "chatbot") cost = 350;
            if (projectValue === "webapp") cost = 1200;

            const currencySymbol = currency === "usd" ? "$" : "₽";
            result.textContent = `Стоимость: ${cost} ${currencySymbol}`;
        });
    </script>
</body>
</html>
