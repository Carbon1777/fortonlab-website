# fortonlab-website

Статический одностраничник студии **Forton Lab** для домена
[fortonlab.ru](https://fortonlab.ru).

Чистый HTML + CSS, без сборщиков. Деплой — Cloudflare Pages
(бесплатный план), привязка домена — на стороне Cloudflare.

## Локальный просмотр

Просто открой `index.html` в браузере:

```bash
open index.html
```

Или подними локальный сервер (нужен для тестов с правильными
относительными путями):

```bash
python3 -m http.server 8080
# открыть http://localhost:8080
```

## Структура

```
fortonlab-website/
├── index.html        # разметка лендинга
├── styles.css        # стили (палитра, типографика, адаптив)
├── favicon.svg       # фавикон (упрощённая печать)
├── assets/
│   ├── seal.png      # печать FORTONLAB (hero-логотип)
│   ├── centry-icon.png
│   ├── diktum-icon.png
│   └── og.png        # 1200×630 для шеринга в соцсетях
└── README.md
```

---

## Деплой через Cloudflare Pages

Делается один раз. Дальше каждый `git push` в `main` автоматически
триггерит новый деплой.

### Шаг 1. Создать репозиторий на GitHub

1. Зайди на [github.com/new](https://github.com/new).
2. Имя репозитория: `fortonlab-website`. Описание — необязательно.
   Сделай **Public** (Cloudflare Pages умеет деплоить и приватные,
   но публичный проще).
3. **Не добавляй** README, .gitignore, лицензию (они уже есть в проекте).
4. Нажми **Create repository**. GitHub покажет инструкции — нам
   нужны только команды из блока *push an existing repository*.

### Шаг 2. Запушить код

В терминале из корня `fortonlab-website/`:

```bash
git init
git add .
git commit -m "feat: initial landing"
git branch -M main
git remote add origin git@github.com:Carbon1777/fortonlab-website.git
git push -u origin main
```

> Если используешь HTTPS вместо SSH — `git remote add origin
> https://github.com/Carbon1777/fortonlab-website.git`.

Проверь: на странице репозитория на GitHub видны все файлы.

### Шаг 3. Подключить Cloudflare Pages

1. Зайди на [dash.cloudflare.com](https://dash.cloudflare.com/) —
   нужен аккаунт Cloudflare (бесплатный).
2. В левом меню: **Workers & Pages** → **Create application** →
   вкладка **Pages** → **Connect to Git**.
3. Авторизуй Cloudflare в GitHub (откроется окно) и выбери репозиторий
   `fortonlab-website`.
4. **Set up builds and deployments:**
   - **Project name:** `fortonlab-website` (или любое другое — оно
     попадёт в URL `*.pages.dev`).
   - **Production branch:** `main`.
   - **Framework preset:** `None`.
   - **Build command:** *оставить пустым*.
   - **Build output directory:** `/` (просто слэш — корень репозитория).
5. Нажми **Save and Deploy**. Сборка займёт 30–60 секунд.
6. После успешного деплоя получишь временный URL вида
   `https://fortonlab-website.pages.dev` — открой его, проверь, что
   сайт работает.

### Шаг 4. Привязать домен fortonlab.ru

1. На странице проекта в Cloudflare → вкладка **Custom domains** →
   **Set up a custom domain**.
2. Введи `fortonlab.ru` → **Continue**.
3. Cloudflare покажет, какие DNS-записи нужно добавить. Возможно
   два сценария:
   - **Если домен уже на Cloudflare DNS** — он предложит добавить
     запись автоматически, нажми **Activate domain**. Готово.
   - **Если домен на reg.ru (или другом регистраторе)** — Cloudflare
     покажет CNAME (или A-записи) для добавления. Запиши их.

### Шаг 5. (если нужно) Настроить DNS на reg.ru

1. Зайди в личный кабинет [reg.ru](https://www.reg.ru/) → твои
   домены → `fortonlab.ru` → **Управление DNS / DNS-серверы и
   управление зоной**.
2. Добавь записи, которые показал Cloudflare. Обычно это:
   - **CNAME** для `www` → `fortonlab-website.pages.dev`.
   - **A-запись** для корня (`@`) → IP, который указал Cloudflare.
   Либо Cloudflare предложит сменить NS-серверы домена на свои —
   это даёт больше контроля и проще для будущего.
3. Сохрани. Подожди 5–30 минут (иногда дольше) — пока DNS
   пропагируется. Проверять можно на [whatsmydns.net](https://www.whatsmydns.net/).

### Шаг 6. Проверка

1. Открой `https://fortonlab.ru` — должен показать лендинг.
2. SSL Cloudflare настраивает автоматически, замочек в адресной
   строке появится сам в течение нескольких минут после привязки.
3. Открой `https://www.fortonlab.ru` — должен либо работать, либо
   редиректить на основной домен (зависит от настройки в Cloudflare).

---

## Дальнейшие изменения

После любого редактирования файлов:

```bash
git add .
git commit -m "описание изменений"
git push
```

Cloudflare Pages автоматически задеплоит обновление за минуту.
