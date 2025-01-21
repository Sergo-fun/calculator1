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
                <div>
                    <input type="radio" id="portfolioBlog" name="siteType" value="portfolioBlog">
                    <label for="portfolioBlog">Портфолио или блог</label>
                </div>
            </div>
        </div>

        <div id="chatbotSection" class="hidden">
    <label>Тип чат-бота:</label>
    <div>

        <input type="radio" id="infoBot" name="chatbotType" value="info">
        <label for="infoBot">📄 Информационный</label>
    </div>
    <div>
        <input type="radio" id="tradeBot" name="chatbotType" value="trade">
        <label for="tradeBot">🛒 Торговый</label>
    </div>
    <div>
        <input type="radio" id="serviceBot" name="chatbotType" value="service">
        <label for="serviceBot">🔧 Сервисный</label>
    </div>

<br>

         <label>Выберите количество социальных сетей:</label>
<select id="social-count">
    <option value="1">1</option>
    <option value="2">2</option>
    <option value="3">3</option>
    <option value="4">4</option>
    <option value="5">5</option>
</select>

    <br>
            <label for="advancedFeatures">Расширенные функции:</label>
            <div>

                <input type="checkbox" id="crmIntegration" value="crm">
                <label for="crmIntegration">🔗 Интеграция с CRM</label>
            </div>
            <div>
                <input type="checkbox" id="paymentProcessing" value="payment">
                <label for="paymentProcessing">💳 Обработка платежей</label>
            </div>
            <div>
                <input type="checkbox" id="aiProcessing" value="ai">
                <label for="aiProcessing">🧠 Использование AI</label>
            </div>
        </div>

        <label for="currency">Валюта:</label>
        <select id="currency">
            <option value="byn">🇧🇾 Белорусский рубль</option>
            <option value="rub">🇷🇺 Российский рубль</option>
            <option value="usd">🇺🇸 Доллар</option>
            <option value="eur">🇪🇺 Евро</option>

        </select>

        <button type="button" id="calculateBtn">🔢 Рассчитать стоимость</button>
    </form>

    <div class="result" id="result"></div>

    <button class="close-btn" onclick="Telegram.WebApp.close()">Закрыть</button>

    <script>
        const tg = window.Telegram.WebApp;
        tg.ready();

        document.body.style.backgroundColor = tg.themeParams.bg_color || "#f4f4f9";
        document.body.style.color = tg.themeParams.text_color || "#333";

        const projectType = document.getElementById("projectType");
        const complexitySection = document.getElementById("complexitySection");
        const chatbotSection = document.getElementById("chatbotSection");
        const calculateBtn = document.getElementById("calculateBtn");
        const resultDiv = document.getElementById("result");

        projectType.addEventListener("change", () => {
            if (projectType.value === "chatbot") {
                complexitySection.classList.add("hidden");
                chatbotSection.classList.remove("hidden");
            } else if (projectType.value === "site") {
                chatbotSection.classList.add("hidden");
                complexitySection.classList.remove("hidden");
            } else {
                chatbotSection.classList.add("hidden");
                complexitySection.classList.add("hidden");
            }
        });

      calculateBtn.addEventListener("click", async () => {
    let cost = 0;
    const currency = document.getElementById("currency").value;
    let currencySymbol = "BYN";
    let rate = 1; // Ставка по умолчанию для BYN

    // Получаем курс через API
    const response = await fetch(`https://api.exchangerate-api.com/v4/latest/BYN`); // API для актуальных курсов
    const data = await response.json();

    // Устанавливаем символ валюты и курс обмена
    if (currency === "rub") {
        currencySymbol = "₽";
        rate = data.rates.RUB; // Курс к рублю
    } else if (currency === "usd") {
        currencySymbol = "$";
        rate = data.rates.USD; // Курс к доллару
    } else if (currency === "eur") {
        currencySymbol = "€";
        rate = data.rates.EUR; // Курс к евро
    } else if (currency === "byn") {
        currencySymbol = "BYN";
        rate = 1; // Ставка для BYN
    } else if (currency === "usdt") {
        currencySymbol = "USDT";
        rate = data.rates.USD; // Преобразуем курс к USDT, так как USDT привязан к доллару
    }

    // Стоимость 1 бота (в BYN)
    const botCost = 349;

    // Получаем количество выбранных ботов
    const selectedSocialNetworks = document.querySelectorAll("#chatbotSection input[type='checkbox']:checked");
    const numberOfBots = selectedSocialNetworks.length;

    // Корректируем стоимость в зависимости от количества ботов
    if (projectType.value === "chatbot") {
        let totalCost = 0;

        // Добавляем стоимость первого бота
        totalCost += botCost;



        const socialCountSelect = document.querySelector("#social-count");
const selectedSocialCount = parseInt(socialCountSelect.value);

// Добавляем 10% за каждую выбранную социальную сеть
if (selectedSocialCount === 1) totalCost *= 1.00;  // 1 сеть
if (selectedSocialCount === 2) totalCost *= 1.90;  // 2 сети
if (selectedSocialCount === 3) totalCost *= 2.70;  // 3 сети
if (selectedSocialCount === 4) totalCost *= 3.40;  // 4 сети
if (selectedSocialCount === 5) totalCost *= 4.00;  // 5 сетей




        const crmSelected = document.querySelector("#crmIntegration").checked;
        const paymentSelected = document.querySelector("#paymentProcessing").checked;
        const aiSelected = document.querySelector("#aiProcessing").checked;



       if (selectedSocialCount === 1) {
    // Добавляем 10% за каждую выбранную расширенную функцию
    if (crmSelected) totalCost *= 1.10;  // 10% за CRM
    if (paymentSelected) totalCost *= 1.10;  // 10% за Payment
    if (aiSelected) totalCost *= 1.10;  // 10% за AI
} else if (selectedSocialCount === 2) {
    // Добавляем 20% за каждую выбранную расширенную функцию
    if (crmSelected) totalCost *= 1.10;  // 20% за CRM
    if (paymentSelected) totalCost *= 1.10;  // 20% за Payment
    if (aiSelected) totalCost *= 1.10;  // 20% за AI
} else if (selectedSocialCount === 3) {
    // Добавляем 30% за каждую выбранную расширенную функцию
    if (crmSelected) totalCost *= 1.10;  // 30% за CRM
    if (paymentSelected) totalCost *= 1.10;  // 30% за Payment
    if (aiSelected) totalCost *= 1.10;  // 30% за AI
}else if (selectedSocialCount === 4) {
    // Добавляем 30% за каждую выбранную расширенную функцию
    if (crmSelected) totalCost *= 1.10;  // 30% за CRM
    if (paymentSelected) totalCost *= 1.10;  // 30% за Payment
    if (aiSelected) totalCost *= 1.10;  // 30% за AI
}else if (selectedSocialCount === 5) {
    // Добавляем 30% за каждую выбранную расширенную функцию
    if (crmSelected) totalCost *= 1.30;  // 30% за CRM
    if (paymentSelected) totalCost *= 1.30;  // 30% за Payment
    if (aiSelected) totalCost *= 1.30;  // 30% за AI
}







        // Присваиваем итоговую стоимость
        cost = totalCost;
    }

   // Если выбран другой тип проекта
if (projectType.value === "site") {
    const siteType = document.querySelector('input[name="siteType"]:checked');
    if (!siteType) {
        alert("Пожалуйста, выберите тип сайта.");
        return;
    }

    if (siteType.value === "singlePage" || siteType.value === "portfolioBlog") {
        cost = 399; // Стоимость одностраничного сайта или портфолио/блога в BYN
    } else if (siteType.value === "multiPage") {
        cost = 529; // Стоимость многостраничного сайта в BYN
    } else if (siteType.value === "onlineStore") {
        cost = 1049; // Стоимость интернет-магазина в BYN
    }
} else if (projectType.value === "webapp") {
    cost = 1199; // Стоимость веб-приложения в BYN
}
    // Преобразуем стоимость в нужную валюту
    cost *= rate;

    // Выводим цену
    resultDiv.textContent = `Стоимость: ${cost.toFixed(2)} ${currencySymbol}`;
});

    </script>
</body>
</html>
