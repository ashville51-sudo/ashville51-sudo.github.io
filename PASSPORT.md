# Паспорт полигона — посаженные SEO-проблемы

> Эталон для E2E-приёмки [seo-monitor](https://github.com/ashville51-sudo/seo-monitor):
> прогон по полигону должен найти всё из таблицы «Срабатывания» и ничего
> не найти сверх неё (кроме оговорённых побочных эффектов).
> Сайт: тематика «мастерская керамики», 23 страницы.

## Срабатывания (проверки краулера)

| Проверка | Что посажено | Где | Ожидание |
|---|---|---|---|
| CR-002 битые внутренние ссылки | ссылки на несуществующие страницы | `/catalog/kruzhka-prazdnichnaya/` (inlinks: главная, каталог), `/articles/sekrety-mastera/` (inlink: статьи) | 2 находки; у первой — 2 входящие ссылки |
| CR-003 нет title | страница без `<title>` | `/contacts/` | 1 находка |
| CR-004 дубли title | одинаковый title у двух товаров | `/catalog/kruzhka-molochnaya/` + `/catalog/kruzhka-grafit/` | группа из 2 URL (см. «Побочные эффекты») |
| CR-005 нет description | товар без meta description | `/catalog/tarelka-glubokaya/` | 1 находка |
| CR-006 дубли description | одинаковый description у двух ваз | `/catalog/vaza-polevaya/` + `/catalog/vaza-lesnaya/` | группа из 2 URL |
| CR-007 нет h1 | товар без `<h1>` | `/catalog/gorshok-dlya-zapekaniya/` | 1 находка |
| CR-008 несколько h1 | два `<h1>` на странице | `/catalog/salatnik-bolshoy/` | 1 находка |
| CR-010 битые внешние ссылки | ссылка на несуществующий репозиторий GitHub (404) | главная → `github.com/ashville51-sudo/net-takogo-repo-404` | 1 находка |
| CR-014 одинаковый h1 на разных страницах | один h1 у двух статей | `/articles/kak-vybrat-glinu/` + `/articles/obzhig-v-domashnih-usloviyah/` | группа из 2 URL |
| CR-015 нет canonical | страница без canonical | `/articles/istoriya-goncharnogo-kruga/` | 1 находка |
| CR-016 canonical не self | canonical на другую статью | `/articles/glazur-dlya-novichkov/` → `/articles/kak-vybrat-glinu/` | 1 находка |
| CR-017 несколько canonical | два тега canonical | `/articles/uhod-za-keramikoy/` | 1 находка |
| CR-018 noindex на ссылаемой странице | noindex + ссылка с главной | `/services/` | 1 находка |
| CR-019 пагинация не закрыта | страница пагинации без meta robots | `/catalog/page-3/` | 1 находка (page-2 и page-4 закрыты `noindex, follow` — норма для контраста) |
| CR-020 важные страницы заблокированы | пагинация закрыта в robots.txt | `/catalog/page-4/` (`Disallow`) | 1 находка |
| CR-021 параметрические URL не закрыты | UTM-ссылка не закрыта в robots.txt | `/catalog/?utm_source=poligon-test` | 1 находка; `/catalog/?filter=…` закрыт — норма, `/catalog/?color=sinij` сработает после добавления `color` в `junk_query_params` (демо Settings) |
| WM-001 sitemap | корректный `/sitemap.xml` (18 URL) | корень | `ok`; для негативного теста ломается коммитом |

## Не срабатывают (норма)

- CR-009 (5xx): на статическом хостинге не воспроизводится — остаётся на фикстурах/моках.
- CR-011 (лимит внешних ссылок): на `/links/` — 12 внешних ссылок при дефолтном пороге 100.
  Для демонстрации срабатывания: `settings set check.CR-011.max_external_links 10`.

## Заделы под будущие проверки

- `css/blocked.css` закрыт в robots.txt и подключён на всех страницах — расширение CR-020 (стили).
- `/articles/istoriya-goncharnogo-kruga/` намеренно отсутствует в sitemap.xml — будущая WM-проверка «страницы включены в sitemap».
- `/services/` (noindex) и `/catalog/page-4/` (Disallow) тоже не в sitemap — консистентность сигналов.

## Побочные эффекты (ожидаемы, не баги)

Параметрические варианты каталога (`?color=`, `?filter=`, `?utm_source=`) отдают
тот же HTML, что и `/catalog/` — как на настоящих сайтах. Это добавляет находки:

- CR-004 и CR-006 — по дополнительной дубль-группе (title/description каталога
  и его параметрических вариантов);
- CR-014 — группа «одинаковый h1» из `/catalog/` и вариантов;
- CR-016 — 2 находки «canonical не self» (у параметрических URL `?color=`
  и `?utm_source=` canonical ведёт на `/catalog/` — что, строго говоря,
  корректная настройка, кандидат в «ложное срабатывание» для демо этого
  статуса); `?filter=` закрыт в robots.txt и не обходится.

**С 2026-07-06** полигон — user-сайт GitHub Pages: репозиторий переименован
в `ashville51-sudo.github.io`, сайт живёт в корне домена. robots.txt виден
краулеру: CR-020 срабатывает, `/catalog/page-4/` и `/catalog/?filter=…`
не обходятся (фиксируются как заблокированные), `/css/blocked.css` закрыт.

Поисковая индексация (данные для проверок Вебмастера/GSC) появляется с лагом
1–3 недели после регистрации в сервисах.

## Эталон первого прогона

До переезда в корень (проверено 2026-07-06, путь `/seo-poligon-test/`,
robots.txt не виден): **22 проблемы** — CR-002 ×2, CR-003 ×1, CR-004 ×2,
CR-005 ×1, CR-006 ×2, CR-007 ×1, CR-008 ×1, CR-010 ×1, CR-014 ×2, CR-015 ×1,
CR-016 ×4, CR-017 ×1, CR-018 ×1, CR-019 ×1, CR-021 ×1; WM-001 — `ok`.
Повторный прогон: новых 0, подтверждено 22 (дедупликация Issue).

После переезда в корень ожидание — тоже **22 проблемы**, но состав другой:
CR-016 ×3 вместо ×4 (`?filter=` заблокирован и не обходится)
и появляется CR-020 ×1. Требует переподтверждения прогоном.
