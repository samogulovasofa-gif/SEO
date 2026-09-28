Итоговая работа: SEO-оптимизация сайта «Подземка»
Введение
Данный проект представляет собой учебное задание по SEO-продвижению. Сайт банкетного зала «Подземка» в Чите (ул. Курнатовского, 17) выполнен в стиле лофт и оформлен как станции метро. Проект реализован на Django, база данных SQLite уже заполнена.

Важно: сайт изначально создан с намеренными SEO-ошибками. Задача — найти и устранить все недочёты, чтобы подготовить ресурс к индексации в Яндексе и Google.

В коде оставлены подсказки — комментарии с метками SEO-ЗАДАНИЕ, ПОДСКАЗКА, SEO-ВОПРОС. Для поиска используйте Ctrl+Shift+F в VS Code.

Технологии
Python 3.10+

Django 5.2+

SQLite (база db.sqlite3 в репозитории)

Чистые HTML, CSS, JS — без сборщиков

Инструкция по запуску
1. Клонирование репозитория
bash
git clone <ссылка>
cd <папка-проекта>
2. Виртуальное окружение
Windows (PowerShell):

powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
Если PowerShell блокирует скрипты: Set-ExecutionPolicy -Scope CurrentUser RemoteSigned или используйте .venv\Scripts\activate.bat.

macOS / Linux:

bash
python3 -m venv .venv
source .venv/bin/activate
Признак успешной активации — (.venv) в начале строки.

3. Зависимости
bash
pip install -r requirements.txt
4. База данных
База уже готова. При добавлении полей в модели:

bash
python manage.py makemigrations
python manage.py migrate
Если база повреждена:

bash
python manage.py seed_demo --flush
В крайнем случае удалите db.sqlite3, выполните migrate, затем seed_demo.

5. Админка
Логин	Пароль
admin	podzemka2026
⚠️ Пароль только для локального использования. Свой администратор: python manage.py createsuperuser.

6. Запуск сервера
bash
python manage.py runserver
Сайт: http://127.0.0.1:8000/

Админка: http://127.0.0.1:8000/admin/

Тесты
bash
python manage.py test
Проверяют доступность страниц, работу формы, скрытие прошедших событий. Допишите тесты под свои SEO-доработки.

Проверка 404
Windows: $env:DJANGO_DEBUG="0"; python manage.py runserver --insecure
Mac/Linux: DJANGO_DEBUG=0 python manage.py runserver --insecure
(--insecure — чтобы статика грузилась при DEBUG=False.)

Частые проблемы
Проблема	Решение
python не найден	py (Windows) или python3
No module named django	Активировать venv
no such table	migrate
Сайт пустой	seed_demo
Не пускает в админку	changepassword admin
no such column	makemigrations + migrate
Порт занят	runserver 8080
Структура проекта
text
podzemka/     настройки (settings.py, urls.py)
venue/        models.py, views.py, urls.py, sitemaps.py, admin.py, seed_demo.py
templates/    base.html, includes/, venue/
static/       css/, js/, img/
Страницы: /, /home/ (⚠️ дубль), /halls/ и /halls/1..4/ (⚠️ по id), /afisha/ и /afisha/1/ (⚠️ по id), /menu/, /events/, /gallery/, /contacts/, /booking/.

ЗАДАНИЕ
Перед началом проведите аудит: откройте все страницы, изучите исходный код (Ctrl+U), зафиксируйте проблемы для отчёта.

Блок 1. Мета-теги (обязательно)
<html lang> — указать язык документа в templates/base.html.

Уникальный <title> для каждой страницы (сейчас везде «Подземка»).

Длина 50–65 символов, ключ в начале, бренд в конце.

На странице зала — динамически.

<meta name="description"> — уникальный, 120–160 символов, с призывом к действию. На зале — динамический.

<link rel="canonical"> — абсолютный URL.

Мета-тег robots для /booking/: noindex, follow. Объяснить в отчёте, почему это лучше Disallow.

Favicon: favicon.ico или .svg, apple-touch-icon, theme-color.

⭐ Добавить в модель Hall поля meta_title и meta_description: заполнены — выводим, нет — генерируем.

Блок 2. Open Graph (обязательно)
og:title, og:description, og:type, og:url, og:image, og:site_name, og:locale

twitter:card (summary_large_image)

og:image — абсолютный URL, 1200×630, PNG/JPG (SVG не поддерживается).

На странице зала — картинка этого зала.

Проверка: Facebook Debugger или отправка в Telegram (через ngrok).

Блок 3. robots.txt (обязательно)
Создать templates/robots.txt и маршрут /robots.txt.

Закрыть: админку, /booking/, служебные URL.

Директива Sitemap: с абсолютным адресом.

Отдавать с Content-Type: text/plain.

Блок 4. sitemap.xml (обязательно)
Подключить django.contrib.sitemaps.

Доделать venue/sitemaps.py: статика + активные залы.

Маршрут /sitemap.xml.

Исключить дубли и служебные страницы.

⭐ Поле updated_at в Hall + отдавать lastmod.

Блок 5. Структура и контент (обязательно)
Один <h1> на страницу, без пропуска уровней. Сейчас: на главной два h1, в галерее h1 нет, после h2 идут h4, часть h1 слишком общие.

У всех <img>: осмысленный alt, width/height, loading="lazy" для картинок ниже первого экрана.

Заменить анкоры «Подробнее»/«Смотреть» на информативные.

Внешним ссылкам в подвале — rel="noopener" (подумать про nofollow).

Хлебные крошки на всех внутренних страницах.

Блок 6. URL и дубли (обязательно)
ЧПУ: /halls/depo/ вместо /halls/1/, /afisha/kviz-60-sekund/ вместо /afisha/1/ (добавить slug).

Старые URL → 301 на новые.

/home/ → 301 на /.

Своя templates/404.html в стиле сайта.

Блок 7. Schema.org (JSON-LD, обязательно)
EventVenue/LocalBusiness в base.html: название, адрес, телефон, часы, координаты, sameAs, legalName. Данные в settings.SITE_INFO → {{ site.* }}.

BreadcrumbList для крошек.

FAQPage на главной.

Event на странице события: name, startDate, location, image, organizer, eventStatus. ⭐ Для регулярных — eventSchedule.

⭐ AggregateRating для отзывов, Menu/Offer для меню.

Блок 8. ⭐ Дополнительно
Посадочные /events/svadby/, /events/korporativy/ (h1, текст 1500+ символов, мета-теги, семантика из Вордстата).

Семантическое ядро 30–50 запросов + таблица «URL → запросы → title → h1».

Lighthouse, WebP/AVIF, <picture>/srcset.

Яндекс.Метрика с целью «Отправка формы».

Мета-теги подтверждения для Вебмастера и Search Console.

Карте на контактах: loading="lazy" и title.

site.webmanifest.

Картинка тамады: сжать в WebP до ~100 КБ, alt, размеры, lazy, разметка Offer с validThrough, сравнить Lighthouse до/после.

Фото залов: единый формат/размер, srcset. Чем плохи имена 1.jpg, 2.webp, 3.JPG?

Картинка квиза (~2,5 МБ, исходник 60 секунд.jpg): чем плохи пробел и кириллица, как назвать правильно?

Чек-лист
□ Уникальные title/description везде
□ <html lang="ru">
□ canonical везде
□ OG + twitter:card, абсолютный og:image
□ /booking/ — noindex
□ favicon
□ /robots.txt с Sitemap:
□ /sitemap.xml валиден
□ В sitemap нет /home/ и /booking/
□ Ровно один h1 на страницу
□ Нет <img> без alt
□ /halls/<slug>/
□ /halls/1/ и /home/ → 301
□ Своя 404
□ JSON-LD проходит валидатор
□ Крошки на внутренних
Инструменты: Ctrl+U, curl -I, расширения SEO META in 1 CLICK / Detailed SEO, Lighthouse (F12), Screaming Frog.