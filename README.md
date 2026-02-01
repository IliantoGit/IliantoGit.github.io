<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>To-Do List</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background-color: #f0f0f0;
            margin: 0;
            padding: 0;
        }

        .container {
            width: 80%;
            margin: 20px auto;
            background-color: white;
            padding: 20px;
            border-radius: 5px;
        }

        h1 {
            text-align: center;
            color: #333;
        }

        .note {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
            padding: 10px;
            border-bottom: 1px solid #ccc;
        }

        .note-text {
            flex-grow: 1;
        }

        .priority {
            width: 50px;
            height: 50px;
            display: inline-block;
            border-radius: 25px;
            margin-left: 10px;
        }

        .priority-green {
            background-color: #a8d8ea; /* Светло-зеленый */
        }

        .priority-orange {
            background-color: #ffcc80; /* Светло-оранжевый */
        }

        .priority-red {
            background-color: #f26534; /* Красный */
        }

        .actions button {
            background-color: #ddd;
            border: none;
            padding: 5px 10px;
            cursor: pointer;
        }

        #new-note input {
            width: 100%;
            padding: 10px;
            margin-bottom: 10px;
        }

        #new-note button {
            background-color: #4CAF50;
            color: white;
            border: none;
            padding: 10px 20px;
            cursor: pointer;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>To-Do List</h1>

        <div id="new-note">
            <input type="text" placeholder="Введите новую заметку...">
            <button onclick="addNote()">Добавить</button>
        </div>

        <ul id="notes-list">
            <!-- Заметки будут добавляться сюда динамически с помощью JavaScript -->
        </ul>
    </div>

    <script>
        function addNote() {
            // Здесь должна быть логика добавления новой заметки в список.
            // Ты должен использовать JavaScript для работы с DOM и манипулированием элементами.
        }
    </script>

</body>
</html>
