Тестовое задание Green API Лазарев В.Б.



<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GREEN-API Interface</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        .container {
            max-width: 600px;
            margin: auto;
        }
        input, button {
            width: 100%;
            padding: 10px;
            margin: 5px 0;
        }
        .response {
            border: 1px solid #ccc;
            padding: 10px;
            height: 200px;
            overflow-y: scroll;
            white-space: pre-wrap;
        }
    </style>
</head>
<body>
    <div class="container">
        <h2>GREEN-API Interface</h2>
        <label for="idInstance">idInstance:</label>
        <input type="text" id="idInstance" placeholder="Enter idInstance">

        <label for="apiTokenInstance">ApiTokenInstance:</label>
        <input type="text" id="apiTokenInstance" placeholder="Enter ApiTokenInstance">

        <button onclick="getSettings()">getSettings</button>
        <button onclick="getStateInstance()">getStateInstance</button>
        <button onclick="sendMessage()">sendMessage</button>
        <button onclick="sendFileByUrl()">sendFileByUrl</button>

        <h3>Response:</h3>
        <div class="response" id="responseField"></div>
    </div>

    <script>
        async function callApi(endpoint, method = 'GET', data = null) {
            const idInstance = document.getElementById('idInstance').value;
            const apiTokenInstance = document.getElementById('apiTokenInstance').value;

            if (!idInstance || !apiTokenInstance) {
                document.getElementById('responseField').innerText = 'Error: idInstance and apiTokenInstance are required';
                return;
            }

            const url = `https://api.green-api.com/waInstance${idInstance}/${endpoint}/${apiTokenInstance}`;

            const options = {
                method: method,
                headers: { 'Content-Type': 'application/json' },
                body: data ? JSON.stringify(data) : null
            };

            try {
                const response = await fetch(url, options);
                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`);
                }
                const result = await response.json();
                document.getElementById('responseField').innerText = JSON.stringify(result, null, 2);
            } catch (error) {
                document.getElementById('responseField').innerText = `Error: ${error.message}`;
            }
        }

        function getSettings() {
            callApi('getSettings');
        }

        function getStateInstance() {
            callApi('getStateInstance');
        }

        function sendMessage() {
            const data = {
                chatId: "123456789@c.us",
                message: "Hello from GREEN-API!"
            };
            callApi('sendMessage', 'POST', data);
        }

        function sendFileByUrl() {
            const data = {
                chatId: "123456789@c.us",
                urlFile: "http://example.com/image.jpg",
                fileName: "image.jpg"
            };
            callApi('sendFileByUrl', 'POST', data);
        }
    </script>
</body>
</html>
