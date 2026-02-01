<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Мой первый сайт</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background-color: #f0f0f0; /* Светло-серый фон */
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
        }

        .container {
            text-align: center;
            padding: 20px;
        }

        h1 {
            color: #333; /* Темно-серый цвет */
            font-size: 48px;
            margin-bottom: 20px;
            animation: blink 2s infinite;
        }

        p {
            color: rgba(0, 0, 0, 0.3); /* Очень прозрачный черный цвет */
            font-size: 16px;
            margin-bottom: 10px;
        }

        @keyframes blink {
            50% {
                opacity: 0.5;
            }
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>Это мой первый созданный и опубликованный сайт!!!</h1>
        <p style="font-size:12px;">Арсений иди нахуй</p>
    </div>

</body>
</html>
