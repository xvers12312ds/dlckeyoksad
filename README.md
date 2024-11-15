# dlckeyoksad
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Генератор ключей</title>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Roboto', sans-serif;
            background-color: #121212;
            color: #fff;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .container {
            background-color: #1e1e1e;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
            padding: 30px;
            text-align: center;
            width: 400px;
            transition: transform 0.3s ease;
        }
        input[type="password"], input[type="number"] {
            padding: 10px;
            width: calc(100% - 22px);
            border: 1px solid #444;
            border-radius: 5px;
            background-color: #282828;
            color: #fff;
            margin-bottom: 10px;
        }
        input[type="password"]:focus, input[type="number"]:focus {
            border-color: #66afe9;
            outline: none;
        }
        button {
            background-color: #5cb85c;
            color: white;
            border: none;
            padding: 10px;
            border-radius: 5px;
            cursor: pointer;
            width: 100%;
            margin-bottom: 10px;
            transition: background-color 0.3s ease;
        }
        button:hover {
            background-color: #4cae4c;
        }
        #keyGenerator {
            display: none;
            margin-top: 20px;
        }
        .key {
            background-color: #333;
            padding: 8px;
            border-radius: 5px;
            margin: 5px 0;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .copy-button {
            background-color: #007bff;
            padding: 5px;
            border-radius: 5px;
            color: white;
            cursor: pointer;
            transition: background-color 0.3s ease;
            font-size: 14px;
        }
        .copy-button:hover {
            background-color: #0056b3;
        }
        #copyAllButton {
            background-color: #ffcc00;
        }
        #copyAllButton:hover {
            background-color: #ffbb00;
        }
        h2, h3 {
            margin: 0;
        }
    </style>
</head>
<body>
    <div class="container">
        <h2>Введите пароль</h2>
        <input type="password" id="password" placeholder="Пароль">
        <button onclick="checkPassword()">Вход</button>

        <div id="keyGenerator">
            <h3>Генератор ключей</h3>
            <input type="number" id="keyCount" placeholder="Количество ключей" min="1" max="15">
            <button onclick="generateKeys()">Сгенерировать ключи</button>
            <button id="copyAllButton" onclick="copyAllKeys()">Скопировать все ключи</button>
            <div id="keys"></div>
        </div>
    </div>

    <script>
        const correctPassword = "xvers";

        function checkPassword() {
            const passwordInput = document.getElementById('password').value;
            if (passwordInput === correctPassword) {
                document.getElementById('keyGenerator').style.display = 'block';
                document.getElementById('password').value = ''; // очищаем поле пароля
            } else {
                alert('Неверный пароль. Попробуйте снова.');
            }
        }

        function generateKeys() {
            const count = parseInt(document.getElementById('keyCount').value);
            if (count < 1 || count > 15) {
                alert('Пожалуйста, выберите количество ключей от 1 до 15.');
                return;
            }

            const keysContainer = document.getElementById('keys');
            keysContainer.innerHTML = ''; // очищаем предыдущие ключи

            for (let i = 0; i < count; i++) {
                const key = generateKey();
                const keyElement = document.createElement('div');
                keyElement.className = 'key';
                keyElement.textContent = key;

                const copyButton = document.createElement('button');
                copyButton.className = 'copy-button';
                copyButton.textContent = 'Копировать';
                copyButton.onclick = () => copyToClipboard(key);

                keyElement.appendChild(copyButton);
                keysContainer.appendChild(keyElement);
            }
        }

        function generateKey() {
            const length = 15; // длина ключа без префикса
            const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
            let result = 'xvers'; // префикс
            for (let i = 0; i < length; i++) {
                result += chars.charAt(Math.floor(Math.random() * chars.length));
            }
            return result;
        }

        function copyToClipboard(text) {
            navigator.clipboard.writeText(text).then(() => {
                alert('Ключ скопирован в буфер обмена!');
            }, () => {
                alert('Ошибка при копировании ключа.');
            });
        }

        function copyAllKeys() {
            const keys = document.getElementById('keys').innerText;
            if (keys) {
                navigator.clipboard.writeText(keys).then(() => {
                    alert('Все ключи скопированы в буфер обмена!');
                }, () => {
                    alert('Ошибка при копировании ключей.');
                });
            } else {
                alert('Нет ключей для копирования.');
            }
        }
    </script>
</body>
</html>
