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
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            background: linear-gradient(135deg, #0088cc, #005f99);
            color: #ffffff;
            transition: background-color 0.3s, color 0.3s;
        }

        h1 {
            margin-bottom: 20px;
            font-size: 2.5rem;
            text-align: center;
            color: #ffffff;
            animation: fadeInDown 0.6s ease;
        }

        form {
            display: flex;
            flex-direction: column;
            gap: 15px;
            background: rgba(255, 255, 255, 0.9);
            padding: 25px;
            border-radius: 15px;
            box-shadow: 0 8px 15px rgba(0, 0, 0, 0.2);
            max-width: 450px;
            width: 100%;
            animation: fadeInUp 0.6s ease;
        }

        label {
            font-size: 1rem;
            color: #333;
            animation: fadeIn 0.6s ease;
        }

        select, input {
            padding: 10px;
            font-size: 1rem;
            border: 1px solid #ddd;
            border-radius: 10px;
            outline: none;
            transition: border-color 0.3s;
            animation: fadeIn 0.7s ease;
        }

        select:focus, input:focus {
            border-color: #0088cc;
        }

        button {
            padding: 12px;
            font-size: 1rem;
            background: linear-gradient(135deg, #0088cc, #005f99);
            color: white;
            border: none;
            border-radius: 15px;
            cursor: pointer;
            transition: background 0.3s, transform 0.2s;
            animation: fadeInUp 0.8s ease;
        }

        button:hover {
            background: linear-gradient(135deg, #005f99, #003f66);
        }

        button:active {
            transform: scale(0.98);
        }

        .close-btn {
            margin-top: 10px;
        }

        .hidden {
            display: none;
        }

        @keyframes fadeIn {
            from {
                opacity: 0;
            }
            to {
                opacity: 1;
            }
        }

        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @keyframes fadeInDown {
            from {
                opacity: 0;
                transform: translateY(-20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .result {
            margin-top: 20px;
            font-size: 1.2rem;
            color: #ffffff;
            text-align: center;
            animation: fadeIn 0.6s ease;
        }
    </style>
</head>
<body>
    <h1>Калькулятор стоимости</h1>
    <form id="mainForm">
        <label for="projectType">Тип проекта:</label>
        <select id="projectType">
            <option value="site">Сайт</option>
            <option value="chatbot">Чат-бот</option>
            <option value="webapp">Веб-приложение</option>
        </select>

        <div id="complexitySection">
            <label for="complexity">Сложность:</label>
            <input type="number" id="complexity" placeholder="Введите уровень сложности (1-10)">
        </div>

        <div id="chatbotSection" class="hidden">
            <label>Тип чат-бота:</label>
            <div>
                <input type="checkbox" id="infoBot" value="info">
                <label for="infoBot">Информационный</label>
            </div>
            <div>
                <input type="checkbox" id="tradeBot" value="trade">
                <label for="tradeBot">Торговый</label>
            </div>
            <div>
                <input type="checkbox" id="serviceBot" value="service">
                <label for="serviceBot">Сервисный</label>
            </div>

            <label for="socialNetworks">Сколько соц. сетей?</label>
            <select id="socialNetworks">
                <option value="1">1</option>
                <option value="2">2</option>
                <option value="3">3</option>
                <option value="4">4</option>
                <option value="5">5</option>
            </select>

            <label for="advancedFeatures">Расширенные функции:</label>
            <div>
                <input type="checkbox" id="crmIntegration" value="crm">
                <label for="crmIntegration">Интеграция с CRM</label>
            </div>
            <div>
                <input type="checkbox" id="paymentProcessing" value="payment">
                <label for="paymentProcessing">Обработка платежей</label>
            </div>
            <div>
                <input type="checkbox" id="aiProcessing" value="ai">
                <label for="aiProcessing">Использование AI для обработки запросов</label>
            </div>
        </div>

        <label for="currency">Валюта:</label>
        <select id="currency">
            <option value="rub">Российский рубль</option>
            <option value="usd">Доллар</option>
            <option value="eur">Евро</option>
            <option value="byn">Белорусский рубль</option>
        </select>

        <button type="button" id="calculateBtn">Рассчитать стоимость</button>
    </form>

    <div class="result" id="result"></div>

    <button class="close-btn" onclick="Telegram.WebApp.close()">Закрыть</button>

    <script>
        const tg = window.Telegram.WebApp;
        tg.ready();

        document.body.style.backgroundColor = tg.themeParams.bg_color || "#f4f4f9";
        document.body.style.color = tg.themeParams.text_color || "#ffffff";

        const projectType = document.getElementById("projectType");
        const complexitySection = document.getElementById("complexitySection");
        const chatbotSection = document.getElementById("chatbotSection");
        const calculateBtn = document.getElementById("calculateBtn");
        const resultDiv = document.getElementById("result");

        projectType.addEventListener("change", () => {
            if (projectType.value === "chatbot") {
                complexitySection.classList.add("hidden");
                chatbotSection.classList.remove("hidden");
            } else {
                chatbotSection.classList.add("hidden");
                complexitySection.classList.remove("hidden");
            }
        });

        calculateBtn.addEventListener("click", () => {
            let cost = 0;
            const currency = document.getElementById("currency").value;
            let rate = 1;
            let currencySymbol = "руб.";

            switch (currency) {
                case "usd":
                    rate = 0.013;
                    currencySymbol = "$";
                    break;
                case "eur":
                    rate = 0.012;
                    currencySymbol = "€";
                    break;
                case "byn":
                    rate = 0.033;
                    currencySymbol = "BYN";
                    break;
            }

            if (projectType.value === "chatbot") {
                const socialNetworks = parseInt(document.getElementById("socialNetworks").value) || 1;
                const functions = [
                    document.getElementById("crmIntegration").checked,
                    document.getElementById("paymentProcessing").checked,
                    document.getElementById("aiProcessing").checked
                ].filter(Boolean).length;
                cost = (socialNetworks * 500 + functions * 1000) * rate;
            } else {
                const complexity = parseInt(document.getElementById("complexity").value) || 1;
                cost = complexity * 1000 * rate;
            }

            resultDiv.textContent = `Стоимость проекта: ${Math.round(cost)} ${currencySymbol}`;
        });
    </script>
</body>
</html>
