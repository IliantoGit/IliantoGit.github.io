<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Как делать диплом</title>
  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #f4f6fb;
      color: #1f2937;
      line-height: 1.6;
    }
    header {
      background: linear-gradient(135deg, #1e3a8a, #2563eb);
      color: white;
      padding: 48px 20px;
      text-align: center;
    }
    header h1 { margin: 0 0 12px; font-size: 2.4rem; }
    header p { margin: 0; font-size: 1.1rem; opacity: 0.95; }
    nav {
      background: #111827;
      color: white;
      padding: 14px 20px;
      position: sticky;
      top: 0;
      z-index: 10;
    }
    nav a {
      color: white;
      text-decoration: none;
      margin-right: 18px;
      font-weight: bold;
    }
    .container {
      max-width: 1100px;
      margin: 0 auto;
      padding: 30px 20px 60px;
    }
    .card {
      background: white;
      border-radius: 16px;
      box-shadow: 0 8px 24px rgba(0,0,0,0.08);
      overflow: hidden;
      margin-bottom: 24px;
    }
    .hero-img, .section-img {
      width: 100%;
      display: block;
      object-fit: cover;
    }
    .hero-img { height: 340px; }
    .section-img { height: 260px; }
    .content { padding: 24px; }
    h2 { color: #1d4ed8; margin-top: 0; }
    .grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
      gap: 20px;
    }
    .step {
      background: #fff;
      padding: 22px;
      border-radius: 16px;
      box-shadow: 0 8px 24px rgba(0,0,0,0.06);
      border-left: 5px solid #2563eb;
    }
    .step h3 { margin-top: 0; }
    .tips ul { padding-left: 20px; }
    footer {
      text-align: center;
      padding: 24px;
      background: #111827;
      color: #d1d5db;
    }
    .tag {
      display: inline-block;
      background: #dbeafe;
      color: #1d4ed8;
      padding: 6px 12px;
      border-radius: 999px;
      margin: 4px 8px 0 0;
      font-size: 0.9rem;
    }
  </style>
</head>
<body>
  <header>
    <h1>Как делать диплом</h1>
    <p>Простой сайт-памятка о том, как подготовить и написать дипломную работу.</p>
  </header>

  <nav>
    <a href="#about">О сайте</a>
    <a href="#steps">Этапы</a>
    <a href="#tips">Советы</a>
    <a href="#contacts">Контакты</a>
  </nav>

  <div class="container">
    <section class="card" id="about">
      <img class="hero-img" src="https://images.unsplash.com/photo-1455390582262-044cdead277a?auto=format&fit=crop&w=1400&q=80" alt="Рабочее место студента" />
      <div class="content">
        <h2>С чего начать</h2>
        <p>Диплом легче делать, если разбить работу на этапы: выбрать тему, согласовать план, найти источники, написать текст и подготовить защиту.</p>
        <span class="tag">План</span>
        <span class="tag">Источники</span>
        <span class="tag">Оформление</span>
        <span class="tag">Защита</span>
      </div>
    </section>

    <section id="steps">
      <h2>Основные этапы</h2>
      <div class="grid">
        <div class="step">
          <img class="section-img" src="https://images.unsplash.com/photo-1516321318423-f06f85e504b3?auto=format&fit=crop&w=1200&q=80" alt="Команда за ноутбуками" />
          <h3>1. Выбор темы</h3>
          <p>Выбирай тему, которая тебе понятна и по которой можно найти достаточно материалов. Чем конкретнее тема, тем проще писать.</p>
        </div>
        <div class="step">
          <img class="section-img" src="https://images.unsplash.com/photo-1455390582262-044cdead277a?auto=format&fit=crop&w=1200&q=80" alt="Изучение материалов" />
          <h3>2. Сбор информации</h3>
          <p>Используй учебники, статьи, научные публикации и официальные источники. Сразу сохраняй ссылки и делай заметки.</p>
        </div>
        <div class="step">
          <img class="section-img" src="https://images.unsplash.com/photo-1517048676732-d65bc937f952?auto=format&fit=crop&w=1200&q=80" alt="Работа с документами" />
          <h3>3. Написание текста</h3>
          <p>Пиши по главам: введение, теория, практика, выводы. Не пытайся сделать всё за один день — лучше работать постепенно.</p>
        </div>
      </div>
    </section>

    <section class="card tips" id="tips">
      <img class="section-img" src="https://images.unsplash.com/photo-1521791136064-7986c2920216?auto=format&fit=crop&w=1400&q=80" alt="Обсуждение проекта" />
      <div class="content">
        <h2>Полезные советы</h2>
        <ul>
          <li>Сделай содержание ещё до написания основного текста.</li>
          <li>Проверяй оформление по требованиям вуза.</li>
          <li>Оставь время на проверку орфографии и исправления.</li>
          <li>Сделай презентацию и короткую речь для защиты.</li>
        </ul>
      </div>
    </section>

    <section class="card" id="contacts">
      <div class="content">
        <h2>Контакты</h2>
        <p>Этот сайт можно доработать: добавить раздел с комментариями, форму обратной связи, а также отдельные страницы для глав диплома.</p>
      </div>
    </section>
  </div>

  <footer>
    © 2026 Как делать диплом
  </footer>
</body>
</html>
