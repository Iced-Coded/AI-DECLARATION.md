# AI-DECLARATION.md

[![AI-DECLARATION: copilot](https://img.shields.io/badge/䷼%20AI--DECLARATION-copilot-fee2e2?labelColor=fee2e2)](https://ai-declaration.md)
[![Dimwit Pledge](https://dimwit.me/pledge.svg)](https://dimwit.me/pledge)

## Вступ

Код, згенерований ШІ вже став реальністю, і це як плюс так і мінус. Проблема не в самому коді, а в звітності та чистоті. Принаймні, це як в теорії повинно працювати. Пропозиція доволі проста: запропонувати усім мати структурований файл `AI-DECLARATION.md`, де вони чітко позначають використання ШІ, _та в процесі_, зробити це загальноприйнятою річчю.

Це робиться не задля того аби відштовхнути використання LLM та інших генераторів коду в майбутньому. Навпаки, це робиться задля довіри. Коли ви чесно декларуєте які частини коду були насправді згенеровані - скептики можуть одразу подивитися туди, і перевірити самостійно чи це співпадає їх стандартам. Та, дозволяє авторам показувати свої навички в коді, плануванні та інші софт-скіли одночасно, та чесно.

### Опис

Файл `AI-DECLARATION.md` використовує формат YAML для структуризації полів, з обов'язковою секцією `## Notes` у markdown, задля контексту для людини. Що найменш, цей файл повинен містити секції `version`, `level`, та `## Notes`.

Додатково, ви можете декларувати в якій частині розробки використовували ШІ завдяки `processes`, надаючи кожній частині свій рівень залучення ШІ індивідуально. Проте, глобальне значення `level` повинне бути найвищим зі значень залученості ШІ. Будь-які частини, не описані в документі признаються такими, що мають рівень `none`. Ви також можете декларувати в якому файлі або папці ви використовували ШІ, задаючи їм індивідуальні рівні завдяки `components`.

Тож у підсумку, для опису вашого проєкту ви можете використовувати `version`, `level`, `processes`, and `components`.

#### Рівні

Рівні намагаються зачепити не лише генерацію кода як такого, але й пов'язані речі такі як перевірка коду. Вони мають комбінацію слів `працював` та `використовував` разом з такими суб'єктами як `Людина`, `ШІ` та `завдання`

- `none`: Людина працювала над завданням(и) самостійно, без жодного залучення ШІ.
- `hint`: Людина працювала над завданням(и), та запитувала ШІ поверхневі питання.
- `assist`: Людина використовувала ШІ як помічника під час праці над завданням(и), проте більшість роботи виконувала людина.
- `pair`: Людина та ШІ разом працювали над завданнями у рівномірному об'ємі, та людина чітко розуміє написане.
- `copilot`: Людина використовувала ШІ, а той у свою чергу виконував усю роботу, лише запитуючи у людини на дозволи, абощо.
- `auto`: Людина використовувала ШІ, а той у свою чергу автономно працював над завданням.

#### Частини

- `design`: Архітектура, системний дизайн, та рішення.
- `implementation`: Написання коду.
- `testing`: Написання тестів, та загальний відлов багів.
- `documentation`: Написання документації, коментарів, README, та ченджлогів.
- `review`: Перевірка коду та написання коментарів до пул реквестів.
- `deployment`: Конфігурація CI/CD, інфраструктура, та релізні скріпти.

### Схема

Наступні YAML схеми показують структуру файлу `AI-DECLARATION.md`. Використовуйте це для того, аби самостійно написати такий файл, або зробити автоматичний інструмент для цього.

```yaml
type: object
required: [version, level]
definitions:
  level:
    type: string
    enum: [none, hint, assist, pair, copilot, auto]
properties:
  version:
    type: string
    pattern: "^[0-9]+\\.[0-9]+\\.[0-9]+$"
  level:
    $ref: "#/definitions/level"
  processes:
    type: object
    propertyNames:
      enum: [design, implementation, testing, documentation, review, deployment]
    additionalProperties:
      $ref: "#/definitions/level"
  components:
    type: object
    additionalProperties:
      $ref: "#/definitions/level"
additionalProperties: false
```

### Приклади

Нижче були наведені декілька прикладів

#### Простий

Найпростіша `AI-DECLARATION.md` вимагає поля `version`, `level`, та секцію `## Notes`.

```markdown
---
version: "0.1.1"
level: none
---

This format is based on [AI-DECLARATION.md](https://ai-declaration.md/en/0.1.1).

## Notes

- No AI tools were used.
```

```markdown
---
version: "0.1.1"
level: auto
---

This format is based on [AI-DECLARATION.md](https://ai-declaration.md/en/0.1.1).

## Notes

- Claude Code was used to create the whole application.
```

#### З частинами

Використовуйте `processes` аби більш детально описати як та де був використаний ШІ під час розробки. Глобальне значення `level` має бути найвищим значенням з присутніх. Будь-які процеси, які не були описані вважаються як `none`.

```markdown
---
version: "0.1.1"
level: auto
processes:
  design: auto
  testing: copilot
---

This format is based on [AI-DECLARATION.md](https://ai-declaration.md/en/0.1.1).

## Notes

- AI drove architecture decisions and test generation. All output was reviewed by a human.
```

#### З компонентамий

Використовуйте `components` аби декларувати використання ШІ у якихось окремих файлах чи папках.

```markdown
---
version: "0.1.1"
level: auto
components:
  src/helpers: auto
---

This format is based on [AI-DECLARATION.md](https://ai-declaration.md/en/0.1.1).

## Notes

- The helpers directory was fully generated. All other code is human-written.
```

## Бейджики

Add a badge to your `README` to declare your `AI-DECLARATION` level at a glance. Please note, this is just for convenience and to comply with the specification, you _must_ include an `AI-DECLARATION.md` file.

- [![AI-DECLARATION: none](https://img.shields.io/badge/䷼%20AI--DECLARATION-none-dcfce7?labelColor=dcfce7)](https://ai-declaration.md)
- [![AI-DECLARATION: hint](https://img.shields.io/badge/䷼%20AI--DECLARATION-hint-ecfccb?labelColor=ecfccb)](https://ai-declaration.md)
- [![AI-DECLARATION: assist](https://img.shields.io/badge/䷼%20AI--DECLARATION-assist-fef9c3?labelColor=fef9c3)](https://ai-declaration.md)
- [![AI-DECLARATION: pair](https://img.shields.io/badge/䷼%20AI--DECLARATION-pair-ffedd5?labelColor=ffedd5)](https://ai-declaration.md)
- [![AI-DECLARATION: copilot](https://img.shields.io/badge/䷼%20AI--DECLARATION-copilot-fee2e2?labelColor=fee2e2)](https://ai-declaration.md)
- [![AI-DECLARATION: auto](https://img.shields.io/badge/䷼%20AI--DECLARATION-auto-ede9fe?labelColor=ede9fe)](https://ai-declaration.md)

## Часті Питання

### А що, якщо я буду брехати?
Щож, це тоді руйнує усю ідею, чи не так? Загалом ідея про те, аби всі ми мали джентельменську угоду, якій ми можемо довіряти, і коли ми б бачили репозиторій з файлом `AI-DECLARATION.md` у ньому, ми могли використовувати його як першоджерело правди.

### Can I build tooling to generate this automatically?
Be my guest. I envision tooling to build it automatically as well as parse it. While I will do it at some point, I appreciate any or all contributions.

### Can I contribute a translation?
Absolutely! Please. Just fork the repository and add a `README_<locale>.md` e.g. `README_es.md`. Then, raise a PR. I'll take it from there.

### I want to suggest a change to the specification?
Well, good thing it is open-source then. I see the specification evolving naturally with feedback and PRs. So, let us all discuss.

### Do I need to include the file if I added a badge to my README?
Yes, the recommendation is to include an `AI-DECLARATION.md` as the primary source of truth. The badge in the `README` is just a glanceable way for someone to check that A, the `AI-DECLARATION.md` would be available and B, the level.

### Що це за логотип такий?
䷼ Хексаграм 61 або Хексаграм Внутрішньої Правди (Unicode: `U+4DFC`) є одним з 64 хексаграмів Їі Чінг використовував для іллюстрації принципів, де кожна лінія або Їнь (поламана), або Янь (ціла). ([джерело](https://en.wikipedia.org/wiki/List_of_hexagrams_of_the_I_Ching#Hexagram_61))

## Resources
- [yujqiao/ai-declare](https://github.com/yujqiao/ai-declare) agent skill to generate `AI-DECLARATION.md`
- [DimwitLabs/ai-declare](https://github.com/DimwitLabs/ai-declare) fork of the above that reads the spec and validates through the [API](https://ai-declaration.md/api/)
- [ai-declaration Reddit bot](https://developers.reddit.com/apps/ai-declaration/) official bot that detects a linked GitHub repository and comments what it declares, through the [API](https://ai-declaration.md/api/)
