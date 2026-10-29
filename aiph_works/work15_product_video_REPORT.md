# Work 15 — AI-ролик для товара (маркетплейс/реклама)

## Продукт
Рекламный ролик парфюма, 15.19 сек, вертикаль 720x1280 (9:16), h264, все шоты локальной генерацией Wan 2.2 14B fp8 T2V, RTX 4070 12GB.

## Файлы
- Финал: `D:\AI\ComfyUI\ComfyUI_windows_portable\ComfyUI\output\video\product_wan_15sec.mp4` (8.3 MB)
- Обложка: `...\output\video\product_wan_cover.jpg` (900x900, кадр из финала)
- Исходники шотов: `product_wan_seg1_a_00001_.mp4` (подиум, финальная версия), `product_wan_seg2_00001_.mp4` (спрей), `product_wan_seg3_00001_.mp4` (шёлк)
- Отбраковки: `product_wan_seg1_00001_.mp4` (шот 1 в кирпичной[locац], стиль вне брифа), `product_wan_seg1_f_00001_.mp4` (шот 1, дубль-коридор сборки)

## Тайминги
| Шот | Пайплайн | Время |
|---|---|---|
| 1 (подиум) | Lightning 4+4 шага, cfg 1.0 | ~15 мин |
| 2 (спрей) | Lightning 4+4 | ~14 мин |
| 3 (шёлк) | Lightning 4+4 | ~15 мин |
| итого | 3 шота + склейка | ~45 мин от сети до финала |

Справочно: стандартный пайплайн (20 шагов) — 69-85 мин на 5-сек шот; ускорение Lightning LoRA_x3.

## Структура (3 шота)
1. **Макро-подиум**: флакон на чёрном глянцевом подиуме, орбитальный медленный проход камеры, драматичный top-light, золотой боке
2. **Спрей**: экстремальный клоз-ап на сопло, золотой мист в контровом свете, slow motion
3. **Финал-шёлк**: шампан-шёлк вокруг флакона на глянцевой чёрной поверхности, pull-back камеры

## Промты (единый стиль через $hero)
hero = `an elegant translucent amber glass perfume bottle with a slim matte gold cap and a small engraved label, expensive luxury design`
- Шот 1: `Luxury commercial macro shot: $hero standing on a round black glossy stone podium, seamless pitch black studio background with no walls and no windows, single dramatic spotlight from above, golden rim light, soft reflections in the polished black glass, slow elegant orbital camera move, subtle floating dust particles, luxury advertising style, ultra detailed`
- Шот 2: с `$hero` + `spraying a fine golden perfume mist from the nozzle, glowing droplets floating in backlit air / luxury advertising style, ultra detailed`
- Шот 3: с `$hero` + `smooth champagne silk fabric slowly swirling in the air / gentle slow camera pull-back`

Негатив — китайский стандартный (Wan), конфиги всundles на Lightning 4+4, cfg 1.0, shift 5, 81 кадр/шот, fps 16.

## Ноты / hiểu riesgo
- **Текст этикетки на видео — каша (T2V-лимит).** Для реальных клиентов — через I2V с их фото товара (веса I2V ещё не на диске — предложить докачать при нужде).
- **Ключевой hardening lesson**: я Fast-шоты 2/3 в очереди, потом старый раннер из непонимания решил сам — дубли. Урок: при апгрейде пайплайна в фоне держалась очередь, старый раннер убивался/менялся СРАЗУ, потом постиjson.
- **Списание этикетки не мешает портфолио-демо**: здесь мы продаём способность генерять product-video pipeline локально, а не сам флакон.

## Экономика
- Старый пайплайн: ~4.5 часа за такие 15 сек (3 шота >81 кадр, 20 шагов)
- Новый пайплайн: ~45 минут → **x6 ускорения без потери пользы**
- (+ обложка и превью — мгновенно через ffmpeg)

## Озвучка (28.10.2026, финал)
- Финал со звуком: `output\video\product_wan_15sec_voiced.mp4` (9.0 MB, 15.19 c, видеопоток оригинала не тронут)
- Голос: `ru-RU-SvetlanaNeural` (edge-tts), тейк v1 (rate +4%), текст варианта A («Аромат, который слышно...»)
- Раскладка: 5 фраз-сегментов, паузы ровно 1.25 c, старт 0.9 c, речь до ~14.3 c; микрофейды 40/60 мс на краях фраз + лёгкий тёплый эхо (`aecho 85|170`)
- Музыка: процедурный ambient bed m1 (`work15_music_m1_pad.wav`, Am→Em→F→G, пэды+саб+шиммер, сгенерирован numpy/scipt локально), громкость 0.34, fade in 1.2 c / fade out с 14.0 c
- Промежуточные тейки голоса: `tts\vo\work15_edge_female_v1..v4.mp3`; музыкальные варианты: `work15_music_m1/m2/m3`
- Установка: локальная, edge-tts venv: `C:\Users\Gigabyte\OpenCode\tts\tts_env` (edge-tts via pip; текст ушёл на MS-сервер один раз при генерации тейков, публикация — всё локально)
- Тех-урок: `sidechaincompress` в этой сборке ffmpeg 9.0.2 обрезал аудиопоток до 13.45 c (хвост ролика стало «мёртвой тишиной») — контейнер показывал полную длину. Даккинг заменён ручной автоматизацией громкости; финал собран в 2 стадии (wav голоса → микс → мукс). Проверяй scroll: `ffprobe -select_streams a:0 stream=duration`.
