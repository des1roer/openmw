OpenMW
======

OpenMW — движок RPG с открытым миром и открытым исходным кодом, позволяющий играть в Morrowind от Bethesda Softworks. Чтобы играть через OpenMW, у вас должна быть собственная копия игры.

В комплект также входит OpenMW-CS — замена редактора Construction Set от Bethesda.

> **Android.** Если вы ищете сборку для телефона или планшета — она находится в отдельном проекте [des1roer/OpenMW-Android](https://github.com/des1roer/OpenMW-Android). В его релизе уже есть собранный APK со встроенной игрой: после установки игру не нужно отдельно копировать на устройство. Этот репозиторий (`des1roer/openmw`) — исходники самого движка для Windows, macOS и Linux; готовых релизов с игрой здесь нет.

* Версия: 0.52.0
* Лицензия: GPLv3 (подробности в [LICENSE](https://gitlab.com/OpenMW/openmw/-/raw/master/LICENSE))
* Сайт: https://www.openmw.org
* IRC: [#openmw на irc.libera.chat](https://web.libera.chat/#openmw)
* Discord: [официальный сервер](https://discord.com/servers/openmw-260439894298460160)
* Matrix #openmw-space:openmw.org
* _Примечание_: IRC, Discord и Matrix связаны мостами друг с другом.

Лицензии шрифтов:
* DejaVuLGCSansMono.ttf: собственная лицензия (подробности в [files/data/fonts/DejaVuFontLicense.txt](https://gitlab.com/OpenMW/openmw/-/raw/master/files/data/fonts/DejaVuFontLicense.txt))
* DemonicLetters.ttf: SIL Open Font License (подробности в [files/data/fonts/DemonicLettersFontLicense.txt](https://gitlab.com/OpenMW/openmw/-/raw/master/files/data/fonts/DemonicLettersFontLicense.txt))
* MysticCards.ttf: SIL Open Font License (подробности в [files/data/fonts/MysticCardsFontLicense.txt](https://gitlab.com/OpenMW/openmw/-/raw/master/files/data/fonts/MysticCardsFontLicense.txt))

Текущее состояние
--------------

Основные сюжетные линии Morrowind, Tribunal и Bloodmoon полностью проходимы. Возможны (но редки) проблемы с побочными квестами. Список задач, которые нужно решить перед релизом «1.0», смотрите в [баг-трекере](https://gitlab.com/OpenMW/openmw/-/issues/?milestone_title=openmw-1.0). Впрочем, и до релиза «1.0» OpenMW уже предлагает новые [возможности](https://wiki.openmw.org/index.php?title=Features), такие как улучшенная графика и интерфейс.

Работа существующих модов, созданных для оригинального движка Morrowind, не гарантирована. Компилятор скриптов OpenMW проверяет ошибки строже, чем оригинальный движок, поэтому мод, написанный для Morrowind, не обязательно заработает в OpenMW. Некоторые моды также полагаются на особенности поведения или ошибки оригинального движка. Такие случаи рассматриваются индивидуально: где-то можно добавить обходное решение в OpenMW, где-то — только исправить сам мод. Если вы знаете моды, которые работают или не работают, дополните страницу [Mod status](https://wiki.openmw.org/index.php?title=Mod_status) на вики.

Поддержка платформ
------------------

Официально поддерживаемые платформы на данный момент:

- Windows 10 и новее
- macOS 14.8 и новее
- Linux

32-битные платформы не поддерживаются.

Мы приветствуем усилия по добавлению поддержки других платформ!

С чего начать
---------------

* [Инструкции по установке](https://openmw.readthedocs.io/en/latest/manuals/installation/index.html)
* [Сборка из исходного кода](https://wiki.openmw.org/index.php?title=Development_Environment_Setup)
* [Сообщить об ошибке](https://gitlab.com/OpenMW/openmw/issues) — перед первым багрепортом прочитайте [рекомендации](https://wiki.openmw.org/index.php?title=Bug_Reporting_Guidelines)!
* [Известные проблемы](https://gitlab.com/OpenMW/openmw/issues?label_name%5B%5D=Bug)

Как помочь проекту
---------

Не знаете, куда девать свободное время? OpenMW всегда рад новым участникам. [Инструкция по участию в проекте.](./CONTRIBUTING.md)

Путь к данным
-------------

Путь к данным сообщает OpenMW, где искать файлы Morrowind. Если запустить лаунчер, OpenMW должен сам найти эти файлы — при условии, что и Morrowind, и OpenMW установлены корректно (установка Morrowind через WINE считается корректной).

Параметры командной строки
--------------------

    Синтаксис: openmw <параметры>
    Allowed options:
      --config arg                          additional config directories
      --replace arg                         settings where the values from the
                                            current source should replace those
                                            from lower-priority sources instead of
                                            being appended
      --user-data arg                       set user data directory (used for
                                            saves, screenshots, etc)
      --resources arg (=resources)          set resources directory
      --help                                print help message
      --version                             print version information and quit
      --data arg (=data)                    set data directories (later directories
                                            have higher priority)
      --data-local arg                      set local data directory (highest
                                            priority)
      --fallback-archive arg (=fallback-archive)
                                            set fallback BSA archives (later
                                            archives have higher priority)
      --start arg                           set initial cell
      --content arg                         content file(s): esm/esp, or
                                            omwgame/omwaddon/omwscripts
      --groundcover arg                     groundcover content file(s): esm/esp,
                                            or omwgame/omwaddon
      --no-sound [=arg(=1)] (=0)            disable all sounds
      --script-all [=arg(=1)] (=0)          compile all scripts (excluding dialogue
                                            scripts) at startup
      --script-all-dialogue [=arg(=1)] (=0) compile all dialogue scripts at startup
      --script-console [=arg(=1)] (=0)      enable console-only script
                                            functionality
      --script-run arg                      select a file containing a list of
                                            console commands that is executed on
                                            startup
      --script-warn [=arg(=1)] (=1)         handling of warnings when compiling
                                            scripts
                                            0 - ignore warnings
                                            1 - show warnings but consider script as
                                            correctly compiled anyway
                                            2 - treat warnings as errors
      --load-savegame arg                   load a save game file on game startup
                                            (specify an absolute filename or a
                                            filename relative to the current
                                            working directory)
      --skip-menu [=arg(=1)] (=0)           skip main menu on game startup
      --new-game [=arg(=1)] (=0)            run new game sequence (ignored if
                                            skip-menu=0)
      --encoding arg (=win1252)             Character encoding used in OpenMW game
                                            messages:

                                            win1250 - Central and Eastern European
                                            such as Polish, Czech, Slovak,
                                            Hungarian, Slovene, Bosnian, Croatian,
                                            Serbian (Latin script), Romanian and
                                            Albanian languages

                                            win1251 - Cyrillic alphabet such as
                                            Russian, Bulgarian, Serbian Cyrillic
                                            and other languages

                                            win1252 - Western European (Latin)
                                            alphabet, used by default
      --fallback arg                        fallback values
      --no-grab [=arg(=1)] (=0)             Don't grab mouse cursor
      --export-fonts [=arg(=1)] (=0)        Export Morrowind .fnt fonts to PNG
                                            image and XML file in current directory
      --activate-dist arg (=-1)             activation distance override
      --random-seed arg (=<impl defined>)   seed value for random number generator
