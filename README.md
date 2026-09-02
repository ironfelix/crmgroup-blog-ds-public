# CRM-group Blog — Design System

Дизайн-система блога [crmgroup.ru/blog](https://crmgroup.ru/blog/): токены, типографика, компоненты статьи и хаба, прототипы вёрстки.

**Смотреть:** https://ironfelix.github.io/crmgroup-blog-ds-public/

## Что где

| Файл | Что это |
|------|---------|
| [`ds-updated.html`](ds-updated.html) | **Актуальная DS.** Обновлённые токены и компоненты: sidenote, flow, scenario, CJM, tooltips, contributors, progress bar |
| [`index.html`](index.html) | Исходный каталог компонентов v3 + разбор 30 статей блога, статистика CSS-классов, рекомендации UX |
| [`CRM_DESIGN_SYSTEM.md`](CRM_DESIGN_SYSTEM.md) | Спецификация v6: source of truth, токены, двушрифтовая система TT + Inter, deprecated-компоненты |
| [`css/wp-custom.css`](css/wp-custom.css) | Боевой CSS, который подключается в WordPress |
| [`crmgroup-blog-components.html`](crmgroup-blog-components.html) | Legacy-справочник, только для сверки |
| [`prototypes/`](prototypes/) | Прототипы главной, карточек, футера, trust-блока + чейнджлог DS |
| [`blog-hub-redesign/`](blog-hub-redesign/) | Редизайн хаба `/blog/`: было / стало + рекомендации |
| [`font-comparison.html`](font-comparison.html) | Сравнение TTSmalls/TTRuns и Inter |
| [`no-image-slide-concept.html`](no-image-slide-concept.html) | Три концепта слайда без обложки |

Порядок приоритета при расхождениях описан в `CRM_DESIGN_SYSTEM.md` → *Source Of Truth*.

## Ключевые токены

```css
--crm-red:           #c01020;  /* было #dc1327 — снижена яркость на 15%, WCAG AA 7.2:1 */
--crm-page-bg:       #f5f5f5;
--crm-surface:       #ffffff;
--crm-surface-warm:  #fff9f0;
--crm-surface-paper: #faf9f6;
--crm-border:        #e4e2dc;
--crm-text:          #1a1a1a;
--crm-text-muted:    #8a8a8a;
--crm-font:          Inter, -apple-system, sans-serif;   /* статьи */
--crm-font-brand:    "TTSmalls", sans-serif;             /* хаб */
--crm-font-display:  "TTRuns", sans-serif;               /* заголовки хаба */
```

Тело статьи — 17px / 1.58, ширина строки 660px, шкала отступов 8 / 16 / 24 / 40 / 64px.

## Шрифты

Файлы TTSmalls и TTRuns в репозиторий не входят — шрифты проприетарные (TypeType). Демо-страницы отрисуются системным fallback; на боевом сайте лица подключены темой `content_hub`.
