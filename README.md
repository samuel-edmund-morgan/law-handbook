# De Iure — `law-handbook`

[![Deploy](https://github.com/samuel-edmund-morgan/law-handbook/actions/workflows/deploy.yml/badge.svg)](https://github.com/samuel-edmund-morgan/law-handbook/actions/workflows/deploy.yml)

Конспект підготовки до вступу в **Національну академію Служби безпеки України** на спеціальність **D8 «Право»** (заочна, друга вища) і подальшого навчання (бакалавр 5 років → магістр 2 роки → аспірантура).

Сайт: <https://law.morgan-dev.com>
Стек: [MkDocs Material](https://squidfunk.github.io/mkdocs-material/) + GitHub Actions + GitHub Pages.

---

## Quick start (локальна розробка)

```bash
# 1. Клонувати
git clone git@github.com:samuel-edmund-morgan/law-handbook.git
cd law-handbook

# 2. Створити venv (одноразово)
python3 -m venv .venv
source .venv/bin/activate

# 3. Залежності
pip install -r requirements.txt

# 4. Локальний preview-сервер з live-reload
mkdocs serve
# → http://127.0.0.1:8000
```

`mkdocs serve` слідкує за `docs/` і `mkdocs.yml`, перебудовує сайт при кожному save файлу.

## Як додати нову сторінку (3 кроки)

1. **Створити markdown-файл** у потрібній директорії під `docs/`:
   ```bash
   touch docs/підготовка-до-вступу/історія/блоки/02-стародавня-історія.md
   ```
2. **Додати запис у `nav:`** у `mkdocs.yml` — інакше сторінка існуватиме, але не з'явиться у сайдбарі. Дотримуйся існуючого порядку розділів.
3. **Закоммітити й запушити**:
   ```bash
   git add docs/.../нова-сторінка.md mkdocs.yml
   git commit -m "Add page: <короткий опис>"
   git push origin main
   ```

GitHub Actions автоматично запустить білд і деплой. За ~1-2 хв сторінка буде на `https://law.morgan-dev.com/...`.

## Як вставити LaTeX

Сайт використовує [pymdownx.arithmatex](https://facelessuser.github.io/pymdown-extensions/extensions/arithmatex/) + MathJax.

- Inline: `\\(E = mc^2\\)` → \(E = mc^2\)
- Block: `\\[ \\int_0^\\infty e^{-x^2}\\,dx = \\tfrac{\\sqrt{\\pi}}{2} \\]`

## Адмонішени, табси, чек-листи

```markdown
!!! tip "Назва"
    Текст підказки.

!!! question "Питання білета"
    Що є об'єктом історії України?

=== "Варіант А"
    Контент варіанту А.

=== "Варіант Б"
    Контент варіанту Б.

- [x] Виконано
- [ ] У процесі
```

Повний перелік розширень — у `mkdocs.yml`, секція `markdown_extensions`.

## Деплой і автодеплой

- **Тригер**: push у `main` (також запускається вручну через GitHub Actions UI → `Deploy MkDocs to GitHub Pages` → `Run workflow`).
- **Workflow**: `.github/workflows/deploy.yml`. Кроки: checkout → setup-python 3.12 → `pip install -r requirements.txt` → `mkdocs build --strict` → `mkdocs gh-deploy --force`.
- **Артефакт**: гілка `gh-pages` (її не варто редагувати руками — перезаписується автодеплоєм).

## Custom domain (`law.morgan-dev.com`)

Налаштовується **один раз**:

### 1. DNS у Cloudflare

Додати запис у зону `morgan-dev.com`:

| Type | Name | Content | Proxy |
|---|---|---|---|
| `CNAME` | `law` | `samuel-edmund-morgan.github.io` | DNS only (сірий значок) |

> Якщо хочеш проксі (помаранчевий значок), вмикай тільки після того, як GH Pages видасть сертифікат. Інакше TLS-handshake впаде.

### 2. GitHub Repo → Settings → Pages

- **Source**: Deploy from a branch.
- **Branch**: `gh-pages` / `(root)`.
- **Custom domain**: `law.morgan-dev.com`.
- Дочекатися повідомлення «DNS check successful».
- Увімкнути **Enforce HTTPS** (доступне через 5-30 хв після успішного DNS-check).

Файл `docs/CNAME` уже містить `law.morgan-dev.com`, тому MkDocs кладе його в білд автоматично — GH Pages підхопить домен.

## Backlog і workflow-доки

Технічна координація проєкту ведеться в окремому workflow control plane:
`~/Development/Projects/law-project/` (поза цим репо).

- `docs/BACKLOG.md` — задачі.
- `docs/DECISIONS.md` — довговічні архітектурні рішення (DEC-002 — стек, DEC-003 — структура курсів, DEC-004 — workflow окремо від репо).
- `docs/PROJECT_BRIEF.md` — INIT.

## Структура

```
law-handbook/
├── .github/workflows/deploy.yml   # CI/CD
├── docs/
│   ├── CNAME                       # → law.morgan-dev.com
│   ├── index.md                    # Корінь
│   ├── assets/{logo,favicon}.svg   # Лого Феміди
│   ├── stylesheets/extra.css       # Додаткові стилі
│   ├── javascripts/mathjax.js      # MathJax config
│   ├── підготовка-до-вступу/      # Дашборд + 4 предмети + довідники
│   ├── 1-курс/ … 7-курс/           # Заглушки під майбутнє навчання
│   └── аспірантура/                # Заглушка PhD
├── mkdocs.yml                      # Конфігурація сайту
├── requirements.txt                # mkdocs-material
└── README.md                       # цей файл
```

## Ліцензія

Контент: персональний навчальний матеріал автора, без комерційного використання.
