# Work 16 — Серия Reels/Shorts AI-видео в едином стиле

## Продукт
3 самостоятельных вертикальных ролика (5 сек каждый) + demo-склейка 15.19 сек 720x1280 (9:16) h264.
Wan 2.2 14B fp8 T2V + Lightning LoRA (4+4 шага, cfg 1.0), полностью локально.

## Файлы
- Склейка: `...\output\video\reels_series_15sec.mp4` (7.5 MB)
- Обложка: `...\output\video\reels_series_cover.jpg`
- Отдельные клипы: `reels_seg1_00001_.mp4` (океан), `reels_seg2_00001_.mp4` (горы), `reels_seg3_00001_.mp4` (кафе)

## Сцены (один стиль через $style-хвост промта)
| # | Сцена | Камера |
|---|---|---|
| 1 | Океан: прибой slow motion, закатные хмары | low angle у воды |
| 2 | Горы: рассветный туман между вершинами, солнце | aerial push-in |
| 3 | Кафе-терраса: боке-гирлянда, пар от кофе | slow dolly forward |

## Единый стиль-хвост ($style)
`cinematic vertical social media b-roll, golden hour lighting, warm amber and teal color palette, gentle slow camera movement, film grain, ultra detailed, no text, no people close-up`

## Тайминги
3 шота POST-нулись одновременно 22:22, склейка 23:20 → вся серия ~58 мин (по ~15 мин/шот + ожидание очереди).

## Ноты
- Lightning-пайплайн даёт одинаковую палитру при смене сюжета - ключ продаж «серия контента без рассыпания стиля» (видео-аналог Work 13).
- Пар от кофе в стоп-кадре выглядит как «золотое пламя»; в движении это мягкий vapor. Если клиенту нужен чистый пар - реролл сегмента (15 мин).
- Пейзажные B-roll - ходовая тема у блогеров для фонов под текст/voiceover.

## Озвучка (28.10.2026, финал)
- Финал со звуком: `output\video\reels_series_15sec_voiced.mp4` (видеопоток оригинала не тронут, аудио aac 192k)
- Голос: `ru-RU-SvetlanaNeural` (edge-tts, venv `tts\tts_env`), настройки Work 15 (+4%, Svetlana), текст v3 со словом «важен» (замена «дорог» — обходит неверно ударение edge-tts)
- Раскладка: 5 фраз-сегментов; 2 фразы под океан, «Волна.» и «Рассвет.» под горы, «Кофе…» на кафе; релизация до ~14.6 c; микрофейды 40/60 мс + лёгкий aecho (85|170)
- Музыка: процедурный warm-major bed m2 (`work16_music_m2_pluck.wav`, прогрессия C→G→Am→F, пэды+саб+шиммер+нотки, numpy/scipy), громкость 0.34, ducking под речью (0.14) через volume-автомацию, fade in 1.2 c / fade out с 14.0 c
- Избранные артефакты: `tts\vo\work16_edge_female_w15style.mp3` (голос), `work16_music_m1_pad.wav` и `m2_pluck.wav` (музыкальные варианты), финальный микс `mix16_m2.wav`
- Тех-урок (перенесён из Work 15): финальный микс собирается в 2 стадии (голос → mix wav → мукс); `sidechaincompress` в ffmpeg 9.0.2 обрезает аудиопоток — не использовать, проверять `ffprobe -select_streams a:0 stream=duration`.
