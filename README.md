# Охота на баги

Аркадный QA-тренажёр для начинающих тестировщиков: лови жуков-багов курсором или пальцем, различай severity и не путай баги с фичами.

**Играть:** https://bug-hunt.denis-timoshin.ru/

Автор: **Денис Тимошин** · курс по тестированию: https://stepik.org/a/254843

## Как запустить

Откройте `index.html` в любом современном браузере — установка и сборка не нужны. Игра работает на компьютере и телефоне.

Игра размещена в Yandex Object Storage (бакет `bug-hunt.denis-timoshin.ru`, хостинг статического сайта) с HTTPS-сертификатом Let's Encrypt из Certificate Manager, который продлевается автоматически.

Каждый push в ветку `main` публикует сайт через GitHub Actions (`.github/workflows/deploy.yml`): в бакет загружаются только файлы сайта — `index.html`, `404.html`, `robots.txt`, `sitemap.xml`, файл подтверждения Google и папка `assets/`. Запустить публикацию вручную можно на вкладке Actions → Deploy to Yandex Object Storage → Run workflow.

Для публикации в репозитории нужны секреты `YC_S3_ACCESS_KEY_ID` и `YC_S3_SECRET_ACCESS_KEY` — статический ключ сервисного аккаунта Yandex Cloud с ролью `storage.editor`.

## Правила

- Кликай или тапай по жукам, чтобы закрыть тикет.
- Баг, который уполз за край экрана, «утекает в прод» и снижает качество релиза. При 0% игра заканчивается.
- Спринт длится 40 секунд, каждый следующий сложнее.
- Промах сбрасывает комбо, каждые 5 попаданий подряд увеличивают множитель (до ×5).
- Фиолетовые плашки «фича» трогать нельзя: −50 очков.
- P / Esc — пауза, M — звук.

## Виды багов

| Severity | Очки | Удары | Что это |
|---|---|---|---|
| Trivial | 10 | 1 | Опечатка, съехавший пиксель |
| Minor | 20 | 1 | Мелкий сбой, есть обход |
| Major | 40 | 1 | Ломает важную функцию |
| Critical | 80 | 2 | Падение, потеря данных |
| Blocker | 150 | 3 | Тестировать дальше нельзя |

## Структура

```
bug-hunt/
├── index.html              — игра (HTML, CSS и JS в одном файле)
├── 404.html                — страница «не найдено» в виде баг-репорта
├── robots.txt              — правила для поисковых роботов и ссылка на карту сайта
├── sitemap.xml             — карта сайта для Яндекса и Google
├── google887333dffbabf9cb.html — подтверждение сайта в Google Search Console (не удалять)
├── .github/workflows/
│   └── deploy.yml          — автопубликация в Yandex Object Storage
└── assets/
    ├── og-image.jpg        — превью для соцсетей, 1200×617
    ├── favicon-32.png      — иконка вкладки
    └── apple-touch-icon.png — иконка для iOS, 180×180
```

## SEO и Open Graph

Мета-теги находятся в `<head>` файла `index.html`: title, description, canonical, Open Graph, Twitter Card и разметка Schema.org.

Все ссылки в тегах абсолютные и указывают на https://bug-hunt.denis-timoshin.ru/ — соцсети принимают только полные адреса картинки превью. Если игра переедет на другой домен, замените этот адрес в `index.html`.

Проверить, как выглядит превью ссылки:
- Telegram — отправить ссылку боту [@WebpageBot](https://t.me/WebpageBot) (он же сбрасывает кеш превью);
- VK — https://vk.com/dev/pages.clearCache;
- общий валидатор — https://www.opengraph.xyz.

Для поисковиков есть `robots.txt` (разрешает индексацию и указывает на карту сайта) и `sitemap.xml`. При заметных изменениях игры обновляйте в `sitemap.xml` дату в `<lastmod>`.

Чтобы игра быстрее попала в поиск, добавьте сайт в [Яндекс Вебмастер](https://webmaster.yandex.ru) и [Google Search Console](https://search.google.com/search-console) и отправьте там ссылку на `sitemap.xml`.

## Технологии

Чистые HTML, CSS и JavaScript без зависимостей: Canvas 2D для игрового поля, Web Audio API для звуков, `localStorage` для рекорда. Шрифты — Unbounded, Onest и JetBrains Mono из Google Fonts.
