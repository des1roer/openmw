# OpenMW для Android с встроенным Morrowind

## Итог

- APK: `C:\dev\openmw\buid\openmw-morrowind-0.51.0-14-embedded-release.apk` (961 МБ, release, не debuggable, `targetSdk 35`, arm64-v8a, пакет `com.ast.openmw`).
- Внутри полный Morrowind из `C:\games\The Elder Scrolls - Morrowind. Расширенное издание` (21 104 файла, 1,3 ГБ).
- При первом нажатии «Играть» игра распаковывается в `Android/data/com.ast.openmw/files/Morrowind`, путь и кодировка `win1251` прописываются сами. Прерванная распаковка продолжается с места остановки.
- Код: https://github.com/des1roer/OpenMW-Android, ветка `main`, коммит `a175a2f`.
- В `C:\dev\openmw` поменяны только remote: `origin` = `des1roer/openmw`, `upstream` = `OpenMW/openmw`.
- APK публично не выкладывается: в нём проприетарные данные Bethesda/1С.

## Ключ подписи

- Лежит в репозитории: `source/signing/release.jks`, параметры в `source/signing/release.properties`. Gradle подхватывает их сам.
- Копия вне репозитория: WSL `~/proj/omw-keys/` (`release.jks`, `signing.env`).
- Alias: `openmw`
- Пароль хранилища и ключа: `vVdvk6QUEwMf6jwhTdNEHcIX`
- SHA-256 сертификата: `D9:26:20:AA:90:FD:BA:1F:47:30:12:70:1F:E9:12:AB:4D:D7:CD:D7:43:F9:26:67:FE:38:15:9D:B8:39:AB:39`
- Обновления ставятся поверх только с этим ключом. Ключ лежит в публичном репозитории, так что подписать им APK может кто угодно.

## Как пересобрать

Всё в WSL (`~/proj/openmw-android-wrapper/source`), сборка в Docker-образе `omw-android-build` (Ubuntu 24.04, JDK 17, autotools; Dockerfile в `~/proj/omw-docker`). Android SDK: `~/Android/sdk`, кэш Gradle: `~/proj/omw-gradle-home`.

1. Данные игры: `./tools/stage-embedded-game.sh "/mnt/c/games/The Elder Scrolls - Morrowind. Расширенное издание" win1251` → `app/embedded-game/` (в `.gitignore`).
2. Нативные библиотеки и ресурсы движка: взять из официального релиза Andiweli `0.51.0-14` (`OpenMW.0.51-14.apk`):
   - `lib/arm64-v8a/{libopenmw,libGL,libSDL2,libcollada-dom2.5-dp,libopenal,libc++_shared}.so` → `app/src/main/jniLibs/arm64-v8a/`
   - `assets/libopenmw/` → `app/src/main/assets/libopenmw/`
   - `assets/android_omwfx/*.omwfx` → `app/src/main/assets/android_omwfx/`
3. APK:
   ```
   docker run --rm --user $(id -u):$(id -g) -e HOME=/tmp/h -e GRADLE_USER_HOME=/gradle-home \
     -v "$PWD":/src -v ~/Android/sdk:/sdk -v ~/proj/omw-gradle-home:/gradle-home -w /src omw-android-build \
     bash -c 'mkdir -p /tmp/h; ./gradlew --no-daemon :app:assembleMainlineRelease'
   ```
   Результат: `app/build/outputs/apk/mainline/release/app-mainline-release.apk`.

## Грабли

- Master форка `des1roer/openmw` (0.52-dev) под Android не собрать: Android-патчи обёртки написаны под OpenMW 0.51.0 (`f4bec41`). Android-задача в `.gitlab-ci.yml` апстрима мёртвая.
- `buildscripts/build.sh` собирает `libopenmw.so` без ~40 графических патчей из `tools/*.ps1`, и Gradle-проверка `verifyOpenMwPayload` отклоняет её по SHA-256. Эталонный хеш (`a99f89d2…`) у библиотеки из релиза 0.51.0-14, поэтому используем её.
- `libsdl.org` из этой сети недоступен, загрузка висит молча. SDL2 2.0.22 брать с GitHub (`libsdl-org/SDL` releases), SHA256 совпадает; положить в `buildscripts/downloads/`.
- `build.sh --lto`: полная LTO-линковка съедает >28 ГБ (WSL: 20 ГБ + 8 ГБ swap) и уходит в трешинг. Собирать без `--lto`.
- В WSL нет sudo, поэтому всё через Docker с `--user $(id -u):$(id -g)`. Каталоги для монтирования создавать заранее, иначе Docker создаст их от root.
- Скрипты обёртки закоммичены без флага исполнения: исправлено в `a175a2f`.
- AAPT по умолчанию выкидывает каталоги `_*` (в данных есть `Sound/Cr/_OLD`): в `build.gradle` переопределён `ignoreAssetsPattern`.
- Ролики базовой игры в этом издании лежат в корневой `Video`, а не в `Data Files/Video`: скрипт подготовки их домерживает. BSA в издании нет, ресурсы распакованы.
- `targetSdk 29` (как было у `mainline`): Play Защита блокирует установку. Поднято до 35.
- Новые строки лаунчера обязаны быть переведены на de, fr, pl, ru, sv, иначе `verifyOpenMwPayload` падает. Апостроф во французском: `’` или `\'`.
- При передаче скрипта в WSL через `wsl.exe ... bash -s <<'EOF'` теряются обратные слэши: правки с `\` делать через редактор файлов.
- Глобальная git-идентичность в WSL рабочая (`n.zuev`): для личных репо коммитить с `-c user.name=des1roer -c user.email=des1roer@gmail.com`.
