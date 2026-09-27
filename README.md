# PersonalityHelper/zaPrEt-WardOgs-SetUp-diSCord

Форк [PersonalityHelper/zaPrEt-WardOgs-SetUp-diSCord](https://github.com/PersonalityHelper/zaPrEt-WardOgs-SetUp-diSCord), заранее настроенный так, чтобы работали **War Dogs** и **Discord** одновременно.

Главное отличие от оригинала — файл **`AUTO-PICK-ZAPRET.bat`**: он сам перебирает стратегии и останавливается на той, что работает сегодня. Руками по одной запускать больше не нужно.

## Как пользоваться

1. Скачать репозиторий: зелёная кнопка **Code → Download ZIP**
2. ПКМ по архиву → **Свойства** → галочка **«Разблокировать»** → ОК
3. Распаковать в путь **без кириллицы и пробелов** — например `C:\zapret`
4. ПКМ по **`AUTO-PICK-ZAPRET.bat`** → **«Запуск от имени администратора»**

Скрипт перебирает стратегии и на каждой проверяет три адреса Discord — сайт, gateway и CDN. Останавливается на первой, где отвечают все три, и оставляет её работать. Занимает от нескольких секунд до пары минут.

Доступность античита War Dogs проверяется отдельно и **на выбор стратегии не влияет** — почему, см. ниже.

Первой всегда пробуется `general (ALT11)` — она подтверждённо работает с War Dogs.

Результат последнего запуска пишется в `auto-pick-last-run.txt`.

## Что уже настроено

| Настройка | Значение | Зачем |
|---|---|---|
| Game Filter | `enabled (UDP)` | War Dogs использует UDP на портах выше 1023 |
| IPSet Filter | `any` | под фильтр попадает любой IP игры |
| `lists/list-general.txt` | 105 строк | штатные домены + Cloudflare + `codefusion.technology` (античит) |
| `lists/list-exclude.txt` | 129 строк | штатные исключения + Яндекс, Teams и прочее |

Списки — **надстройка** над оригинальными: все домены Discord и YouTube из штатной сборки на месте, ничего не удалено. Поэтому Discord не ломается.

### Почему античит не влияет на выбор стратегии

Все 8 доменов античита (`ac-*.codefusion.technology`) присутствуют **одновременно** в `list-general.txt` и в `list-exclude.txt`. В winws исключения имеют приоритет над списком обхода, поэтому zapret пропускает этот трафик нетронутым — при любой стратегии.

Скорее всего так и задумано: вмешиваться в трафик античита — плохая идея. Но следствие важно понимать: **по доступности античита нельзя отличить одну стратегию от другой**, результат будет одинаковым хоть с обходом, хоть без него. Поэтому `AUTO-PICK` выбирает стратегию по Discord, а античит показывает справочно — если он не отвечает, менять стратегию бессмысленно, проблема в сети.

Если захотите, чтобы античит всё же обходился, удалите строки `codefusion` из `lists/list-exclude.txt`. Конфиг из Discord-гайда я оставил как есть, потому что он у вас рабочий.

## Если не работает

- **Ни одна стратегия не прошла** — скрипт напишет, какая была лучшей, и запустит её. Часто этого достаточно.
- **`winws.exe` не стартует** — почти всегда антивирус забрал `WinDivert`. Добавьте папку в исключения или отключите детект PUA.
- **Нет прав администратора** — без них WinDivert не загрузится, обход не заработает в принципе.
- **Голосовой чат Discord висит на «подключении»** — нужны записи в `hosts`: запустите `service.bat` → пункт **9. Update Hosts File**. На машине, где диск откатывается после сессии, это придётся делать каждый раз.
- **Тормозят посторонние сайты** — это цена режима `IPSet any`. Вернуть: `service.bat` → **5. IPSet Filter**. Полный список лежит в `lists/ipset-all.txt.backup`.

## Ручной режим

Автоподбор ничего не ломает в оригинальной сборке — все штатные файлы на месте. Если хочется по-старому, просто запускайте `general (ALT11).bat` или любой другой напрямую, как раньше.

## Откуда взялся конфиг

Связку «ALT11 + Game Filter UDP + IPSet any + подменённые списки» подсказали на официальном Discord-сервере War Dogs. Стратегии со временем перестают работать и зависят от провайдера — поэтому и нужен автоподбор.

## Лицензия и происхождение

Оригинальный README сохранён рядом — [README-UPSTREAM.md](README-UPSTREAM.md), там полная документация по всем стратегиям и настройкам.

Оригинальный проект — [PersonalityHelper/zaPrEt-WardOgs-SetUp-diSCord](https://github.com/PersonalityHelper/zaPrEt-WardOgs-SetUp-diSCord), лицензия MIT. Бинарники в `bin/` взяты из [bol-van/zapret-win-bundle](https://github.com/bol-van/zapret-win-bundle) — их можно сверить по контрольным суммам с оригиналом.

Отдельная благодарность [bol-van](https://github.com/bol-van) — автору [zapret](https://github.com/bol-van/zapret).

---

<details>
<summary>NEW</summary>

*   **`AUTO-PICK-ZAPRET.bat`**: новый скрипт для автоматического подбора рабочей стратегии.
*   **`general (FAKE TLS AUTO ALT).bat`**: новая стратегия для автоподбора.
*   **`general (FAKE TLS AUTO ALT2).bat`**: новая стратегия для автоподбора.
*   **`general (FAKE TLS AUTO ALT3).bat`**: новая стратегия для автоподбора.
*   **`general (SIMPLE FAKE ALT).bat`**: новая стратегия для автоподбора.
*   **`general (SIMPLE FAKE ALT2).bat`**: новая стратегия для автоподбора.
*   **`general (SIMPLE FAKE).bat`**: новая стратегия для автоподбора.
*   **`auto-pick-last-run.txt`**: файл для записи результата последнего запуска автоподбора.

</details>

<details>
<summary>ФЕЙКИ</summary>

*   **TLS/DTLS**: подмена TLS/DTLS-заголовков для обхода блокировок.
*   **HTTP/HTTPS**: подмена HTTP/HTTPS-заголовков.
*   **QUIC**: подмена QUIC-заголовков.

</details>

<details>
<summary>АНТИВИРУСЫ</summary>

<p>
  <img src="https://img.shields.io/badge/Avast-✔-brightgreen" alt="Avast">
  <img src="https://img.shields.io/badge/Kaspersky-✔-brightgreen" alt="Kaspersky">
  <img src="https://img.shields.io/badge/Windows Defender-✔-brightgreen" alt="Windows Defender">
  <img src="https://img.shields.io/badge/ESET-✔-brightgreen" alt="ESET">
  <img src="https://img.shields.io/badge/Dr.Web-✔-brightgreen" alt="Dr.Web">
  <img src="https://img.shields.io/badge/Malwarebytes-✔-brightgreen" alt="Malwarebytes">
  <img src="https://img.shields.io/badge/Norton-✔-brightgreen" alt="Norton">
  <img src="https://img.shields.io/badge/McAfee-✔-brightgreen" alt="McAfee">
</p>

**Внимание:** некоторые антивирусы могут детектировать `WinDivert` как потенциально нежелательное ПО (PUA). В этом случае добавьте папку с `zapret` в исключения антивируса или отключите детект PUA.

</details>

---

<p align="center">
  <a href="https://github.com/PersonalityHelper/zaPrEt-WardOgs-SetUp-diSCord/releases/tag/1.10.3"> Releases </a> |
  <a href="https://github.com/PersonalityHelper/zaPrEt-WardOgs-SetUp-diSCord/releases/latest"> Latest </a> |
  <a href="https://github.com/PersonalityHelper/zaPrEt-WardOgs-SetUp-diSCord/issues"> Issues </a> |
  <a href="https://github.com/PersonalityHelper/zaPrEt-WardOgs-SetUp-diSCord/blob/main/LICENSE.txt"> License </a> |
  <a href="https://github.com/PersonalityHelper/zaPrEt-WardOgs-SetUp-diSCord/graphs/contributors"> Contributors </a>
</p>

---

<div align="center">
  <a href="https://github.com/PersonalityHelper/zaPrEt-WardOgs-SetUp-diSCord">
    <img src="https://img.shields.io/github/stars/PersonalityHelper/zaPrEt-WardOgs-SetUp-diSCord.svg?style=social&label=Star" alt="GitHub Stars">
  </a>
  <a href="https://github.com/PersonalityHelper/zaPrEt-WardOgs-SetUp-diSCord">
    <img src="https://img.shields.io/github/forks/PersonalityHelper/zaPrEt-WardOgs-SetUp-diSCord.svg?style=social&label=Fork" alt="GitHub Forks">
  </a>
  <a href="https://github.com/PersonalityHelper/zaPrEt-WardOgs-SetUp-diSCord">
    <img src="https://img.shields.io/github/watchers/PersonalityHelper/zaPrEt-WardOgs-SetUp-diSCord.svg?style=social&label=Watch" alt="GitHub Watchers">
  </a>
</div>

---

## поддержка проекта

<p align="center">
  <a href="https://github.com/PersonalityHelper/zaPrEt-WardOgs-SetUp-diSCord/blob/main/README.md#readme">
    <img src="https://img.shields.io/badge/Donate-PayPal-blue.svg" alt="PayPal">
  </a>
  <a href="https://github.com/PersonalityHelper/zaPrEt-WardOgs-SetUp-diSCord/blob/main/README.md#readme">
    <img src="https://img.shields.io/badge/Donate-Bitcoin-orange.svg" alt="Bitcoin">
  </a>
  <a href="https://github.com/PersonalityHelper/zaPrEt-WardOgs-SetUp-diSCord/blob/main/README.md#readme">
    <img src="https://img.shields.io/badge/Donate-Ethereum-purple.svg" alt="Ethereum">
  </a>
</p>

---

<p align="center">
  © 2024 PersonalityHelper. All rights reserved.
</p>