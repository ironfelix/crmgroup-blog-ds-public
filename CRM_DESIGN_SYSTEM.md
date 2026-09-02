# CRM-group Blog Design System (v6)
> Updated: 2026-02-23
> Scope: WordPress `content_hub`, hub `/blog/` + article pages `body.single-blog`

## Source Of Truth (Priority)
1. `01-crmgroup/draft-wp-metrics-v2.txt`  
   Content structure, anchors, FAQ+Schema, repeated block patterns.
2. `01-crmgroup/draft-crm-metrics.html`  
   Production-like TOC behavior (desktop sticky + mobile + active state JS).
3. `01-crmgroup/ds-updated.html`  
   Updated visual tokens and typographic fixes (`#c01020`, `1.58`, `660px`).
4. `01-crmgroup/crmgroup-blog-components.html`  
   Legacy reference only.

---

## What Is Updated In v6
- **Двушрифтовая система TT + Inter** — формализована система зон (см. ниже).
- **Тёплый фон** — `#faf9f6` (paper) для статей, снижает усталость от яркого белого.
- **Мягкий контраст** — текст `#222` (14:1 vs 17:1, оба WCAG AAA).
- **EXPERT text rendering — протестировано и убрано из статей.** См. секцию "Что было убрано" ниже.

### v5 (без изменений)
- **Contributors v2** — единый блок авторов+экспертов, Mindbox-style.
- v4: production TOC, accent `#c01020`, body `17px/1.58`, reading `660px`, H3 fix.

### Deprecated
- `.author-block` + `.author-block__label/name/post` → заменить на `.contributors`
- `.experts-section` + `.experts-grid` + `.expert-card` → заменить на `.contributors`

---

## Design Tokens

```css
--crm-page-bg:       #f5f5f5;
--crm-surface:       #ffffff;
--crm-surface-warm:  #fff9f0;
--crm-border:        #e4e2dc;
--crm-text:          #1a1a1a;
--crm-text-muted:    #8a8a8a;
--crm-red:           #c01020;  /* was #dc1327 */
--crm-red-soft:      #fde8e8;
--crm-radius:        12px;
--crm-font:          Inter, -apple-system, BlinkMacSystemFont, sans-serif;
--crm-font-brand:    "TTSmalls", sans-serif;
--crm-font-display:  "TTRuns", sans-serif;
--crm-surface-paper: #faf9f6;   /* warm paper bg for articles */
--crm-text-soft:     #222;      /* softer contrast: 14:1, WCAG AAA */
```

---

## Двушрифтовая система TT + Inter

Два шрифта — стандартная практика (Medium, Stripe, Linear, Notion).
Ключ: переход между ними осознанный, совпадает со сменой контекста пользователя.

### Зоны

| Зона | Шрифт | Размер | Почему |
|------|-------|--------|--------|
| **Навигация, хедер, футер** | TTSmalls 500 | 14-15px | Бренд. Короткий текст, TT в своей стихии |
| **Hub /blog/ — карточки** | TTSmalls opt | 16px / lh 1.62 | Описания ≤3 строк. Брендовый характер |
| **Hub — заголовки карточек** | TTRuns 700 | 18-20px | Display-шрифт в display-контексте |
| **Hub — мета** | TTSmalls 500 | 12-13px | Служебный текст, вес 500 для плотности |
| **Слайдер Editorial Dark** | TTRuns 700 | 28-36px | Hero-заголовок. TTRuns рождён для этого |
| — | — *граница: хаб → статья* | — | — |
| **Статья — H1** | Inter 700 | 32-38px | Читатель «внутри» статьи. Inter задаёт тон |
| **Статья — H2, H3** | Inter 700/600 | 26px / 17px | Единая гарнитура с текстом |
| **Статья — тело** | Inter 400 | 17px / lh 1.58 | Лонгрид. Максимальный комфорт |
| **Статья — лид** | Inter 400 | 18px / lh 1.65 | Первый абзац крупнее — приглашение |
| **Статья — TOC** | Inter 400/600 | 13.5px | Мелкий навигационный текст |
| **Статья — block-comment** | Inter 400i | 16px | Курсив отделяет от основного текста |
| **Статья — CTA** | Inter 600 | 18-20px | Призыв к действию |

### Как связать TT и Inter (мост)

1. **Общие токены:** один акцент `#c01020`, одна палитра серых, один 8px-grid
2. **Shared metrics:** TT hub 16px ≈ Inter article 17px (TT визуально крупнее из-за x-height)
3. **Transition = смена контекста:** хаб = навигация (TT), статья = чтение (Inter)

### EXPERT text rendering — что было убрано и почему

Все свойства ниже были протестированы в продакшне и **убраны из статей** (Inter),
потому что каждое из них меняло раскладку текста по строкам — «пляска» концов строк:

| Свойство | Проблема |
|----------|----------|
| `text-wrap: pretty` | Браузер перераспределяет текст чтобы избежать orphans → строки переносятся в других местах |
| `text-rendering: optimizeLegibility` | Включает кернинг-пары → меняет расстояние между буквами → другие переносы строк |
| `font-feature-settings: "kern" 1, "liga" 1, "calt" 1` | Дублирует кернинг + лигатуры → тот же эффект |
| `hanging-punctuation: first last` | Сдвигает кавычки/точки за поля → визуально строки «пляшут» |
| `word-spacing: 0.05em` | Помогает TTSmalls (узкий), но **ухудшает** Inter (и так комфортный) |
| `hyphens: auto` | Русские авто-переносы некрасивые: «кли-», «цен-», «о-» |

**Вывод:** Inter уже из коробки отлично рендерится. Дополнительный кернинг и переносы ему
не нужны — они для шрифтов с плохими дефолтными настройками.

**На хабе (`#blog`)** `text-rendering: optimizeLegibility` и `font-feature-settings` оставлены —
там TTSmalls, и ему эти свойства реально помогают (у него менее проработанный дефолтный кернинг).

**Что осталось в статьях (не влияет на раскладку строк):**
```css
body.single-blog { background: #faf9f6; }  /* тёплый фон */
.single-blog .section-content { color: #222; } /* мягкий контраст */
```

---

## Typography

### Статьи (Inter)

| Element | Size | Weight | Line-height | Notes |
|---|---:|---:|---:|---|
| Body text | 17px | 400 | 1.58 | long-read baseline, без доп. rendering свойств |
| Lead | 18px | 400 | 1.65 | left border accent |
| H2 | 24-26px | 700 | 1.25 | top divider allowed |
| H3 | 17px | 600 | 1.35 | `#3d3d3d`, `border-left: 2px solid #e8e8e8` |
| TOC item | 13.5-14px | 400/600 | 1.4-1.45 | active state required |
| FAQ Q | 15.5-16px | 600 | 1.4 | summary row |
| FAQ A | 15-15.5px | 400 | 1.7-1.75 | answer text |

### Hub /blog/ (TTSmalls + TTRuns)

| Element | Size | Weight | Line-height | Notes |
|---|---:|---:|---:|---|
| Card desc | 17px | 400 | 1.65 | `ls: 0.008em`, `ws: 0.05em`, clamp 3 |
| Card title | 18-20px | 700 | 1.35 | TTRuns, `ls: -0.01em` |
| Hero title | 28-36px | 700 | 1.15 | TTRuns, `ls: -0.03em`, `text-wrap: balance` |
| Meta | 12-13px | 500 | 1.65 | `ls: 0.01em`, Medium для плотности |
| Nav/header | 14-15px | 500 | — | TTSmalls, фирменный стиль |

Reading width target: about `660px` (65-75 chars in RU).
Background: `#faf9f6` (warm paper) for articles.

### Homepage (body.home) — TTSmalls + TTRuns

Главная — корпоративный лендинг (не блог). Использует шрифты темы, НЕ Inter.
Тема задаёт `--text-size: 22px` и `--heading-size` — мы переопределяем `--text-size: 18px`.

| Element | Tag.class | Font | Size | Weight | Line-height | Notes |
|---|---|---|---:|---:|---:|---|
| Hero H1 | `h1.h1.section-heading` | TTRuns | clamp(36-50px) | 700 | 1.1 | `ls: -0.02em` |
| Section H2 | `h2.h2` | TTRuns | var(--heading-size) | 700 | 1.15 | `ls: -0.02em`, `text-wrap: balance` |
| Service title | `p.h4.service-title` | TTSmalls | .h4 | 700 | 1.3 | Семантика: `<p>`, не `<h4>` |
| Service desc | `.service-desc p` | TTSmalls | 18px | 400 | 1.5 | |
| Case number | `h2.case-info` | TTRuns | ~46px | 700 | 1.0 | **Тег `<h2>` — тема.** Должен быть `<div>` (SEO) |
| Case desc | `p.case-desc` | TTSmalls | 16px | 400 | 1.5 | opacity 0.85 |
| Case tag | `p.tag` | TTSmalls | 12px | — | 1.4 | |
| Award header | `.award-header` | TTSmalls | 16px | 700 | 1.3 | Внутри `.dark-block` |
| Award desc | `.award-desc` | TTSmalls | 18px | 400 | 1.45 | |
| Card title | `.card-article__textual__head` | TTSmalls | 18px | — | 1.35 | TTRuns на хабе, TTSmalls на homepage |
| Button CTA | `a.button.button--bg` | TTSmalls | theme | 600 | — | |
| Header CTA | `a.to-form` | TTSmalls-600 | theme | 600 | — | Класс: `--color-accent-red`, НЕ `.button` |
| Footer links | `.social-networking a` | TTSmalls-500 | 14px | 500 | — | |
| Footer fine | `.copyright`, `.conditions` | theme | 13px | — | — | `rgba(26,26,26,0.5)` |

#### Известные проблемы (тема, не CSS-fixable)

1. **`<h2 class="case-info">` для display-чисел** — 9 штук, ломает heading outline.
   Правильно: `<div class="case-info">`. Требует правки шаблона темы.
2. **Дубликат H2 «Доказываем делом»** — в cases-section и awards-section.
   Рекомендация: awards → «Признание индустрии» или «Награды».
3. **`<br>` в H2** — хрупкая разбивка строк. CSS `text-wrap: balance` как подстраховка.
4. **SVG `stroke='#FD3939'`** — хардкод в HTML. Override через CSS `.slider__navigation path`.

### Правила выравнивания для лендингов (UI/UX)

Базовые правила:
- Один общий контейнер для всех секций: одинаковый `max-width` и одинаковые боковые `padding`.
- Одна левая вертикаль: заголовки секций, тексты и CTA стартуют с одной X-координаты.
- Единая сетка: одинаковые `gap` между колонками в похожих блоках (например, везде `64` или везде `48`).
- Вертикальный ритм: одинаковые отступы между секциями и внутри секции по шкале (например `96/64/40/24/16`).
- Заголовки одного уровня выглядят одинаково: все `H2` с одним размером, интерлиньяжем, весом и одинаковым `margin-bottom`.
- Пары «заголовок + подзаголовок» имеют одинаковый паттерн отступов на всей странице.
- В двухколоночных секциях верх левой и правой колонки должен совпадать по горизонтали.
- Поля форм и кнопки: одинаковая высота, радиус, внутренние отступы, ширина по логике (`100%` в форме).
- Карточки в одной группе: одинаковые внутренние отступы и одинаковая логика выравнивания контента.
- На мобиле сохраняется та же логика, меняется только количество колонок (обычно в `1` колонку), без скачков отступов.

Практический минимум для главной:
- Контейнер: один стандарт (например `max-width: 1200`, `padding-inline: 40/24/20`).
- Секции: один стандарт вертикальных отступов.
- Все `H2`: один стиль и один `margin-bottom`.
- Все двухколоночные блоки: один `gap` и одинаковое выравнивание по верху.

---

## Layout (WordPress / content_hub)

```text
body.single-blog
  main.singlepost
    .column:nth-of-type(1)   -> hidden spacer
    .column:nth-of-type(2)   -> article content column
    .column:nth-of-type(3)   -> sticky right aside (.navigation + subscription)
```

Rules:
- Keep right sticky aside TOC as primary desktop navigation.
- Keep separate mobile TOC (`<details class="mobile-toc">`) inside content.
- Hide in-content theme TOC (`.navigation-page`) to avoid duplicates.

---

## TOC Contract (Production)

Desktop TOC selectors:
- `.single-blog .singlepost .sticky-block .navigation`
- `.single-blog .singlepost .sticky-block .navigation__item`
- `.single-blog .singlepost .sticky-block .navigation__item.active`

Mobile TOC selectors:
- `.mobile-toc`
- `.mobile-toc-list a`

Active-state logic:
- Use `IntersectionObserver`.
- Observe `[id^="anchor_"]`.
- Match links by `href="#anchor_N"`.

---

## Content Structure Contract (From `draft-wp-metrics-v2.txt`)

Required anchors and sections:
1. `anchor_1`: LTV
2. `anchor_2`: Retention Rate
3. `anchor_3`: Churn Rate
4. `anchor_4`: AOV
5. `anchor_5`: NPS
6. `anchor_6`: Как метрики связаны
7. `anchor_7`: Как улучшить метрики (8 инструментов)
8. `anchor_8`: FAQ по метрикам CRM
9. `anchor_9`: Бесплатная консультация

Stable reusable patterns:
- `acf/block-content-list`
- `acf/block-sidenote` (multiple inserts across article, not intro-only)
- `block-comment` (`block-comment__text`, `block-comment__autor`, `autor-photo`, `autor-name`, `autor-post`)
- `block-result` + `block-result__heading`
- `wp-block-table`

---

## FAQ + Schema Requirement

For FAQ section:
1. JSON-LD block with `@type: FAQPage`.
2. Visible FAQ markup in body (`details/summary`) with Schema microdata.

Minimum policy:
- At least 5 Q&A items.
- Content in visible FAQ and JSON-LD must be semantically aligned.
- FAQ section must have stable anchor (`anchor_8`).

---

## Additional CSS (v4, production baseline)

```css
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

.single-blog {
  background: #f5f5f5 !important;
}

/* Layout normalization */
.single-blog .column:nth-of-type(1) { display: none !important; }
.single-blog .column:nth-of-type(2) { max-width: 860px !important; }
.single-blog .section-content .navigation-page { display: none !important; }

/* Base typography */
.single-blog .section-content,
.single-blog .section-content p,
.single-blog .section-content li,
.single-blog .section-content td {
  font-family: Inter, -apple-system, BlinkMacSystemFont, sans-serif;
  font-size: 17px;
  line-height: 1.58;
  color: #1a1a1a;
}

.single-blog .section-content a {
  color: #c01020 !important;
}

.single-blog .section-content li::marker {
  color: #c01020;
  opacity: .75;
}

/* Lead paragraph */
.single-blog .section-content .block-sidenote__container:first-child .block-sidenote__text > p:first-child {
  font-size: 18px !important;
  line-height: 1.65 !important;
  color: #4a4a4a !important;
  border-left: 3px solid #c01020 !important;
  padding-left: 16px !important;
  margin-bottom: 28px !important;
}

/* Headings */
.single-blog .section-content h2.wp-block-heading {
  font-size: 24px !important;
  font-weight: 700 !important;
  line-height: 1.25 !important;
  color: #1a1a1a !important;
  border-top: 2px solid #e4e2dc !important;
  padding-top: 24px !important;
  margin-top: 52px !important;
  margin-bottom: 18px !important;
}

.single-blog .section-content h3.wp-block-heading {
  font-size: 17px !important;
  font-weight: 600 !important;
  line-height: 1.35 !important;
  color: #3d3d3d !important;
  margin-top: 32px !important;
  margin-bottom: 12px !important;
  border-left: 2px solid #e8e8e8 !important;
  padding-left: 12px !important;
}

/* Sidenote */
.single-blog .section-content .block-sidenote {
  position: static !important;
  transform: none !important;
  left: auto !important;
  max-width: 100% !important;
  background: #fff8f0 !important;
  border: 1px solid #f0e4d0 !important;
  border-left: 3px solid #c01020 !important;
  border-radius: 0 8px 8px 0 !important;
  padding: 12px 18px !important;
  margin: 8px 0 24px !important;
}

.single-blog .section-content .block-sidenote__content .mb-16 {
  display: block !important;
  margin-bottom: 6px !important;
  font-size: 11px !important;
  font-weight: 700 !important;
  text-transform: uppercase !important;
  letter-spacing: .8px !important;
  color: #8a8a8a !important;
}

/* Expert comment */
.single-blog .section-content .block-comment {
  background: #fffbf5 !important;
  border: 1px solid #f0e8d4 !important;
  border-left: 4px solid #e8a838 !important;
  border-radius: 0 12px 12px 0 !important;
  padding: 22px 24px 18px !important;
  margin: 28px 0 !important;
}

.single-blog .section-content .block-comment__text p {
  font-size: 16px !important;
  line-height: 1.65 !important;
  color: #1a1a1a !important;
}

.single-blog .section-content .block-comment__autor {
  display: flex !important;
  align-items: center !important;
  gap: 12px !important;
  margin-top: 16px !important;
  padding-top: 14px !important;
  border-top: 1px solid #f0e8d4 !important;
}

.single-blog .section-content .block-comment__autor img {
  width: 44px !important;
  height: 44px !important;
  border-radius: 50% !important;
}

/* Tables */
.single-blog .section-content .wp-block-table,
.single-blog .section-content .table-wrapper {
  overflow-x: auto !important;
}

.single-blog .section-content .wp-block-table table,
.single-blog .section-content .table-wrapper table {
  width: 100%;
  border-collapse: collapse !important;
  font-size: 15px !important;
  background: #fff !important;
}

.single-blog .section-content .wp-block-table thead th,
.single-blog .section-content .table-wrapper thead th {
  background: #f4f3ef !important;
  border-bottom: 2px solid #e4e2dc !important;
  padding: 12px 16px !important;
  font-size: 13.5px !important;
  font-weight: 600 !important;
}

.single-blog .section-content .wp-block-table tbody td,
.single-blog .section-content .table-wrapper tbody td {
  padding: 12px 16px !important;
  border-bottom: 1px solid #e4e2dc !important;
}

.single-blog .section-content .wp-block-table tbody tr:hover td,
.single-blog .section-content .table-wrapper tbody tr:hover td {
  background: rgba(192,16,32,.04) !important;
}

/* Sticky TOC (real production TOC in right aside) */
.single-blog .singlepost .sticky-block .navigation {
  border-left-color: #e4e2dc !important;
}

.single-blog .singlepost .sticky-block .navigation__item {
  font-family: Inter, sans-serif !important;
  font-size: 14px !important;
  line-height: 1.45 !important;
  color: #8a8a8a !important;
  padding: 7px 0 7px 16px !important;
  transition: color .2s !important;
}

.single-blog .singlepost .sticky-block .navigation__item:hover {
  color: #1a1a1a !important;
}

.single-blog .singlepost .sticky-block .navigation__item.active {
  border-left: 2px solid #c01020 !important;
  color: #1a1a1a !important;
  font-weight: 600 !important;
}

/* FAQ (works for both #crm-faq and inline Schema FAQ markup) */
.single-blog .section-content #crm-faq details,
.single-blog .section-content [itemtype="https://schema.org/FAQPage"] details {
  border: 1px solid #e4e2dc !important;
  border-radius: 10px !important;
  margin-bottom: 8px !important;
  overflow: hidden !important;
}

.single-blog .section-content #crm-faq summary,
.single-blog .section-content [itemtype="https://schema.org/FAQPage"] summary {
  display: flex !important;
  justify-content: space-between !important;
  align-items: center !important;
  gap: 12px !important;
  cursor: pointer !important;
  padding: 14px 18px !important;
  font-size: 15.5px !important;
  font-weight: 600 !important;
  line-height: 1.4 !important;
  color: #1a1a1a !important;
  list-style: none !important;
}

.single-blog .section-content #crm-faq summary::-webkit-details-marker,
.single-blog .section-content [itemtype="https://schema.org/FAQPage"] summary::-webkit-details-marker {
  display: none !important;
}

.single-blog .section-content #crm-faq summary::after,
.single-blog .section-content [itemtype="https://schema.org/FAQPage"] summary::after {
  content: '+' !important;
  color: #c01020 !important;
  font-size: 22px !important;
  font-weight: 300 !important;
  transition: transform .2s !important;
}

.single-blog .section-content #crm-faq details[open] summary::after,
.single-blog .section-content [itemtype="https://schema.org/FAQPage"] details[open] summary::after {
  transform: rotate(45deg) !important;
}

.single-blog .section-content #crm-faq [itemprop="text"],
.single-blog .section-content [itemtype="https://schema.org/FAQPage"] [itemprop="text"],
.single-blog .section-content #crm-faq .fb {
  color: #555 !important;
  font-size: 15px !important;
  line-height: 1.7 !important;
}

/* CTA block-result */
.single-blog .section-content .block-result {
  background: #fff9f0 !important;
  border-radius: 12px !important;
}

.single-blog .section-content .block-result__heading a {
  color: #c01020 !important;
  text-decoration: none !important;
}
```

---

## Minimal JS For TOC Active State

```js
const headings = document.querySelectorAll('[id^="anchor_"]');
const links = document.querySelectorAll('.toc-list a, .navigation__item');

const observer = new IntersectionObserver((entries) => {
  entries.forEach((e) => {
    if (!e.isIntersecting) return;
    links.forEach((l) => l.classList.remove('active'));
    const selector = `[href="#${e.target.id}"]`;
    document.querySelectorAll(selector).forEach((el) => el.classList.add('active'));
  });
}, { rootMargin: '-20% 0px -70% 0px' });

headings.forEach((h) => observer.observe(h));
```

---

## Contributors v2 — Unified Block (Mindbox-style)

Все участники статьи (авторы + редактор + эксперты) в одной горизонтальной полосе.
Без коробок, без лейблов «Автор» / «Эксперты». Аватарка 40px + имя + роль.

```html
<div class="contributors">
  <div class="contributors__person">
    <img class="contributors__photo" src="…" alt="Имя">
    <div>
      <div class="contributors__name">Имя Фамилия</div>
      <div class="contributors__role">должность</div>
    </div>
  </div>
  <!-- повторить для каждого участника -->
</div>
```

```css
/* Contributors v2 (Mindbox-style) */
.contributors {
  display: flex;
  flex-wrap: wrap;
  gap: 4px 0;
  align-items: center;
  max-width: var(--measure, 660px);
  padding-top: 20px;
  border-top: 1px solid #e4e4e4;
}
.contributors__person {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 8px 16px 8px 0;
  border-right: 1px solid #e4e4e4;
  margin-right: 16px;
}
.contributors__person:last-child {
  border-right: none;
  margin-right: 0;
}
.contributors__photo {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  object-fit: cover;
  flex-shrink: 0;
}
.contributors__name {
  font-size: 13px;
  font-weight: 600;
  color: #1a1a1a;
  line-height: 1.3;
  white-space: nowrap;
}
.contributors__role {
  font-size: 11.5px;
  color: #8a8a8a;
  line-height: 1.3;
  margin-top: 1px;
  white-space: nowrap;
}
```

Порядок участников: Автор → Редактор → Эксперты (по количеству упоминаний в статье).

---

## Блок: Russian Case Cards (`.cvm-ru-cases`)

Используется когда нужно показать 2–4 российских кейса с метриками.
Левый красный бордер, название компании + тег-бэдж, описание, результат с большой цифрой.

```html
<div class="cvm-ru-cases">
  <div class="cvm-ru-card">
    <div class="cvm-ru-head">
      <span class="cvm-ru-name">Название компании</span>
      <span class="cvm-ru-tag">Отрасль</span>
    </div>
    <p class="cvm-ru-desc">Описание что делали и как.</p>
    <div class="cvm-ru-result">
      <span class="cvm-ru-num">+12%</span>
      <span class="cvm-ru-lbl">расшифровка<br>метрики</span>
    </div>
  </div>
</div>
```

```css
.cvm-ru-cases{display:flex;flex-direction:column;gap:14px;margin:20px 0 28px}
.cvm-ru-card{background:#fff;border:1px solid #e8e5df;border-left:4px solid #dc1327;
             border-radius:0 10px 10px 0;padding:18px 22px}
.cvm-ru-head{display:flex;align-items:center;gap:10px;margin-bottom:8px;flex-wrap:wrap}
.cvm-ru-name{font-weight:700;font-size:15.5px;color:#1a1a1a}
.cvm-ru-tag{font-size:11.5px;color:#888;background:#f5f3ef;padding:2px 9px;
            border-radius:100px;white-space:nowrap}
.cvm-ru-desc{font-size:14px;color:#444;line-height:1.65;margin:0 0 12px}
.cvm-ru-result{display:inline-flex;align-items:center;gap:10px;
               background:#fff9f5;border:1px solid rgba(220,19,39,.18);
               border-radius:7px;padding:7px 12px}
.cvm-ru-num{font-size:20px;font-weight:700;color:#dc1327;line-height:1}
.cvm-ru-lbl{font-size:12.5px;color:#666;line-height:1.35}
.cvm-ru-badge{display:inline-block;font-size:12px;font-weight:600;color:#dc1327;
              background:rgba(220,19,39,.07);padding:3px 10px;border-radius:100px}
```

Первое использование: статья про CVM (WP post 12845, anchor_5 "Российские примеры").

---

## Блок: Component Grid Cards (`.cvm-comps`)

3 карточки в ряд — для схематичного показа составных частей концепции/фреймворка.
Нумерация 01/02/03, одна карточка может быть акцентной (`.is-key`) с предупреждением.

```html
<div class="cvm-comps">
  <div class="cvm-comp">
    <div class="cvm-comp-n">01</div>
    <h4 class="cvm-comp-h">Заголовок компонента</h4>
    <p class="cvm-comp-d">Описание что входит и зачем нужно.</p>
  </div>
  <div class="cvm-comp">
    <div class="cvm-comp-n">02</div>
    <h4 class="cvm-comp-h">Второй компонент</h4>
    <p class="cvm-comp-d">Описание.</p>
  </div>
  <div class="cvm-comp is-key">
    <div class="cvm-comp-n">03</div>
    <h4 class="cvm-comp-h">Ключевой компонент</h4>
    <p class="cvm-comp-d">Описание.</p>
    <div class="cvm-comp-w">Без этого компонента вся конструкция не работает.</div>
  </div>
</div>
```

```css
.cvm-comps{display:grid;grid-template-columns:repeat(3,1fr);gap:14px;margin:4px 0 20px}
.cvm-comp{background:#fff;border:1px solid #e8e5df;border-radius:12px;padding:20px 18px}
.cvm-comp.is-key{border-color:rgba(220,19,39,.3);background:#fffaf9}
.cvm-comp-n{font-size:11px;font-weight:700;color:rgba(220,19,39,.5);
            letter-spacing:.1em;margin-bottom:10px}
.cvm-comp-h{font-size:14.5px;font-weight:700;color:#1a1a1a;margin:0 0 8px;line-height:1.35}
.cvm-comp-d{font-size:13px;color:#666;line-height:1.6;margin:0}
.cvm-comp-w{display:flex;align-items:flex-start;gap:6px;margin-top:14px;padding-top:12px;
            border-top:1px solid rgba(220,19,39,.15);font-size:12.5px;
            color:#dc1327;font-weight:600}
.cvm-comp-w::before{content:"⚠";flex-shrink:0}
@media(max-width:680px){.cvm-comps{grid-template-columns:1fr}}
```

Первое использование: статья про CVM (WP post 12845), блок "Три компонента по PwC".

---

## Form Checkbox (DS component)

Единый стиль для всех форм (homepage, hub, articles).

| Свойство | Значение |
|----------|----------|
| Размер | 18×18px |
| Border | 1.5px solid #ccc |
| Radius | 3px |
| Checked bg | `#c01020` |
| Checked border | `#c01020` |
| Checkmark | Белая галочка (CSS `::after`, border trick) |
| Label font | 13px / lh 1.5 / `#8a8a8a` |
| Link color | `#c01020` |
| Transition | background + border-color 150ms |

### CSS-классы по контексту

| Страница | CSS-класс | Подход |
|----------|-----------|--------|
| Homepage | `.form-checkbox` | Custom `appearance: none` + `::after` checkmark |
| Hub /blog/ | `._form-checkbox` (CF7) | Custom `::before` on `span` |
| Articles | `.single-blog .form-checkbox` | Наследует от глобального `.form-checkbox` |

```html
<!-- Homepage / Articles pattern -->
<div class="form-checkbox">
  <input type="checkbox" id="cb-terms" name="terms">
  <label for="cb-terms">
    <span>Текст с <a href="...">ссылкой</a></span>
  </label>
</div>
```

---

## Practical Checklist Before Publish
1. `block-content-list` matches current anchors and section order.
2. TOC links point to existing `anchor_N` ids only.
3. FAQ has both JSON-LD and visible HTML.
4. Sticky TOC active state works on scroll.
5. Mobile TOC is present and usable.
6. Colors and link accents use `#c01020`.
7. Contributors block uses `.contributors` (не старые author-block / expert-card).
