# ax.files — ориентир для агента (Cursor)

Статический фронтенд **MVP интерфейса KMS «AxFiles»**: один основной SPA в `index.html`, отдельные HTML-черновики и ассеты. Сборки и бэкенда нет — правки напрямую в HTML/CSS/JS в браузере.

## Быстрая карта файлов

| Файл | Назначение |
|------|------------|
| `index.html` | **Главный прототип.** Один файл: разметка всех экранов + встроенные стили + роутинг по `location.hash`. Точка входа для GitHub Pages. |
| `profile-page.html` | Отдельная полноэкранная версия «Мой профиль» (сетка header / rail / main / виджеты), не подключена к хеш-роутеру `index.html`. |
| `profile-lowfi-wireframe.html` | Копия/вариант основного шелла (исторически low-fi); при правках основного UI чаще править `index.html`. |
| `drinkit.html` | Независимый Tailwind-макет (вне продукта AxFiles), можно не трогать при работе над KMS. |
| `profile-wireframe.pdf` | PDF wireframe профиля. |
| `icon-*.png` | Иконки шапки, favicon, кнопка «новый документ». |

## Роутинг в `index.html`

- Формат: `#/segment` или `#/segment/sub` (лидирующий `#/` нормализуется в коде).
- Обработчик: `hashchange` + `route()` в конце файла (IIFE).
- `parts()` парсит hash; по первому сегменту выбирается `data-view` блок (`view-dashboard`, `view-profile`, …) и активный пункт `.nav a[data-nav-key]`.
- Дефолт при пустом hash: `#/dashboard` через `location.replace`.
- Документы: ключи в объекте `DOCS` (`policy`, `presentation`, `api`); проекты — `PROJECTS` (`alpha`, `beta`). Карточки документов подставляют текст из этих объектов.
- `sessionStorage.docFrom` — откуда перешли в документ (влияет на хлебные крошки «назад»).

Перед изменением навигации: найти все `href="#/..."` и соответствующие ветки в `route()`.

## Дизайн и стек

- Чистый HTML/CSS, без npm.
- Шрифты: Google Fonts (Geologica в `index.html`, Inter в `profile-page.html`).
- `drinkit.html` тянет Tailwind с CDN.

## Локальный просмотр

Из корня репозитория:

```bash
python3 -m http.server 8080
```

Открыть `http://localhost:8080/` — важен сервер, а не `file://`, если появятся относительные пути к ресурсам.

## GitHub Pages

- В репозитории: workflow `.github/workflows/deploy-pages.yml` (деплой при push в `main`).
- **Один раз в GitHub:** Settings → Pages → **Build and deployment** → Source: **GitHub Actions** (не «Deploy from branch»).
- После успешного run сайт: `https://koolesoo.github.io/ax.files/` (путь совпадает с именем репозитория).

Добавлен `.nojekyll`, чтобы GitHub Pages не обрабатывал корень как Jekyll.

## Что не делать без запроса

- Не подключать тяжёлый фреймворк ради маленькой правки.
- Не синхронизировать правки между `index.html` и `profile-lowfi-wireframe.html`, если явно не просят оба варианта.
- `drinkit.html` — отдельный эксперимент; не смешивать с логикой KMS.

## Старый remote

Ранее `origin` мог указывать на другой репозиторий; целевой для этого проекта — **`https://github.com/koolesoo/ax.files`**.
