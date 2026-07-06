# ashville51-sudo.github.io — сайт-полигон для SEO Monitor

Статический сайт с **намеренно посаженными SEO-проблемами** — постоянный
E2E-стенд для [seo-monitor](https://github.com/ashville51-sudo/seo-monitor).
Полный перечень посаженных проблем и ожидаемых срабатываний — [PASSPORT.md](PASSPORT.md).

Легенда: «Глиняный двор», мастерская керамики. 23 страницы: каталог товаров
с пагинацией, статьи, служебные страницы.

## Как это работает

- Хостинг — GitHub Pages (Settings → Pages → Deploy from branch `main`, `/ (root)`).
- Обновление сайта = пуш в `main`; «сломать» полигон (уронить sitemap,
  добавить 404) — тоже коммитом, история поломок видна в git.
- Прогон аудита: `seo-monitor demo <URL полигона>` — сверить находки с паспортом.

С 2026-07-06 репозиторий переименован в `ashville51-sudo.github.io` —
полигон стал user-сайтом GitHub Pages и живёт в корне домена
`https://ashville51-sudo.github.io/`; robots.txt виден краулерам,
проверки CR-020/CR-021 работают в полную силу (см. PASSPORT.md).

## Привязка кастомного домена (когда куплен)

1. Заменить базовый URL во всех файлах (canonical, sitemap.xml, robots.txt):
   `https://ashville51-sudo.github.io` → `https://<домен>`.
2. Добавить в корень файл `CNAME` со строкой `<домен>`.
3. DNS у регистратора: 4 A-записи на `185.199.108.153 / 109.153 / 110.153 / 111.153`,
   CNAME `www` → `ashville51-sudo.github.io`.
4. В Settings → Pages включить Enforce HTTPS после выпуска сертификата.

## Метки для сервисов

Во всех страницах в `<head>` есть строка
`<!-- VERIFICATION_PLACEHOLDER … -->` — на её место вставляются метатеги
подтверждения Яндекс.Вебмастера / Google Search Console и счётчик Метрики.
