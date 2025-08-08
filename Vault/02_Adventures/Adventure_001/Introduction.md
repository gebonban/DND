---
title: "Вступление - Налоги в Рассветной Лощине"
tags:
  - adventure
  - introduction
  - tax-collection
cssclass: adventure
parent_adventure: "Adventure_001.md"
scene_type: "introduction"
examined_streets: false
talked_aurili: false
found_symbols: false
---

# 📖 Вступление

<!-- TEXT HERE -->

## 🏰 Таверна «Последняя Встреча»

<!-- TEXT HERE -->

```dataviewjs
// Диалоги в таверне с Надеей Мисливет
const tavernDialogs = [
  {name: "Спросить о главе деревни", player: "Где глава деревни?", npc: "Глава…? Он редко выходит при чужаках."},
  {name: "Спросить о погоде", player: "Погода здесь всегда такая туманная?", npc: "Туман у нас частый гость, особенно летом."},
  {name: "Спросить о детях в деревне", player: "Детей почти не видно, где они?", npc: "Дети играют у старого колодца, но сейчас их лучше не тревожить."},
  {name: "Спросить о странных звуках ночью", player: "По ночам слышны странные звуки, что это?", npc: "Лес живой, а иногда и не только лес…"},
  {name: "Спросить о барде в углу", player: "Кто этот бард?", npc: "Он приходит редко, но всегда поёт печальные песни."},
  {name: "Спросить о старых обычаях", player: "Какие у вас тут обычаи?", npc: "Старые традиции… не все из них стоит вспоминать."},
  {name: "Спросить о недавних гостях", player: "Были ли тут чужаки недавно?", npc: "Почти никто не заходит, кроме вас и сборщиков налогов."},
  {name: "Спросить о еде и напитках", player: "Что у вас сегодня на ужин?", npc: "Похлёбка из корнеплодов и немного эля, если остался."},
  {name: "Спросить о слухах", player: "Какие слухи ходят в деревне?", npc: "Говорят, в лесу кто-то водится… и не зверь."},
  {name: "Спросить о работе в деревне", player: "Чем здесь живут люди?", npc: "Рыбачим, собираем травы, иногда торгуем с проезжими."},
  {name: "Спросить о праздниках", player: "Какие праздники отмечаете?", npc: "Праздники у нас редкость, но на солнцестояние собираемся всем селом."}
];

const tavernDialogContainer = dv.container;

for (let d of tavernDialogs) {
  dv.el("button", d.name, {onclick: () => {
    // Удаляем предыдущие диалоги
    const existingDialogs = tavernDialogContainer.querySelectorAll('.tavern-dialog-result');
    existingDialogs.forEach(el => el.remove());
    // Добавляем новый диалог
    const dialog = dv.paragraph(`> **Вы:** «${d.player}»\n> **Надея:** «${d.npc}»`);
    dialog.classList.add('tavern-dialog-result');
  }});
}

// Кнопка сброса диалогов в таверне
dv.el("button", "🔄 Сбросить диалоги в таверне", {onclick: () => {
  const dialogs = tavernDialogContainer.querySelectorAll('.tavern-dialog-result');
  dialogs.forEach(el => el.remove());
  dv.paragraph("**Диалоги в таверне сброшены.**");
}});
```

## 🌆 Обход деревни

<!-- TEXT HERE -->

```dataviewjs
// Простые кнопки для действий с автоматическим отслеживанием
const actions = [
  {name: "Осмотреть заросшие улицы", flag: "examined_streets"},
  {name: "Поговорить с Аурили (лесник)", flag: "talked_aurili"},
  {name: "Поиcкать следы культов", flag: "found_symbols"}
];

// Создаём контейнер для результатов
const resultsContainer = dv.container;

for (let a of actions) {
  dv.el("button", a.name, {onclick: () => {
    // Удаляем предыдущие результаты
    const existingResults = resultsContainer.querySelectorAll('.action-result');
    existingResults.forEach(el => el.remove());
    
    // Добавляем новый результат
    const result = dv.paragraph(`**${a.name} — выполнено**`);
    result.classList.add('action-result');
  }});
}

// Кнопка сброса для этого раздела
dv.el("button", "🔄 Сбросить результаты обхода", {onclick: () => {
  const results = resultsContainer.querySelectorAll('.action-result');
  results.forEach(el => el.remove());
  dv.paragraph("**Результаты обхода сброшены.**");
}});

// Проверка всех действий
dv.paragraph("**После выполнения всех действий: Вы обнаружили улики о ритуалах — двигайтесь к Дому главы.**");
```

## 🏠 Дом главы

<!-- TEXT HERE -->

```dataviewjs
// Диалоги с Антоном Шварцем в доме главы
const houseDialogs = [
  {
    name: "Про прошлое семьи Шварц",
    player: "Расскажите о своей семье, господин Шварц. Что случилось с вашей женой?",
    npc: "Моя жена… Эльза была прекрасной женщиной. Она умерла десять лет назад при странных обстоятельствах. Говорят, что это была болезнь, но я знаю правду. Она верила в старые пути, как и я когда-то.",
    flag: "asked_about_family"
  },
  {
    name: "Про таинственные исчезновения",
    player: "Мы слышали о пропавших жителях. Что происходит в вашей деревне?",
    npc: "Исчезновения… Да, они случаются. Я говорю людям, что это несчастные случаи в лесу, но это не вся правда. Лес опасен, но не только дикими зверями. Есть силы, которые я не могу контролировать.",
    flag: "asked_about_disappearances"
  },
  {
    name: "Про ритуальные узоры на столах",
    player: "Мы видели странные вырезы в таверне. Что означают эти символы?",
    npc: "Узоры… Это древние символы защиты. Они должны оберегать деревню от злых сил. Но иногда защита требует жертв. Я не хотел этого, но старые обычаи сильнее нас.",
    flag: "asked_about_rituals"
  },
  {
    name: "Про культ лесного колдуна",
    player: "Говорят, в лесу живёт колдун. Что вы знаете о нём?",
    npc: "Лесной колдун… Он был здесь задолго до меня. Я знаю его, и он знает меня. Мы заключили сделку много лет назад — он защищает деревню, а я… я обеспечиваю ему то, что ему нужно. Но цена оказалась слишком высокой.",
    flag: "asked_about_cult"
  },
  {
    name: "Про истинную роль Антона",
    player: "Кто вы на самом деле, господин Шварц? Просто староста или нечто большее?",
    npc: "Моя роль… Я староста, но не всегда был им. Двадцать лет назад я был другим человеком. Я верил в силу древних ритуалов, в могущество лесного колдуна. Я принёс жертвы, думая, что спасаю деревню. Теперь я понимаю, что навлёк на неё проклятие.",
    flag: "asked_about_role"
  }
];

const houseDialogContainer = dv.container;
let houseDialogCount = 0;
let askedQuestions = new Set();

for (let d of houseDialogs) {
  dv.el("button", d.name, {onclick: () => {
    // Удаляем предыдущие диалоги
    const existingDialogs = houseDialogContainer.querySelectorAll('.house-dialog-result');
    existingDialogs.forEach(el => el.remove());
    
    // Добавляем новый диалог
    const dialog = dv.paragraph(`> **Вы:** «${d.player}»\n> **Антон:** «${d.npc}»`);
    dialog.classList.add('house-dialog-result');
    
    // Отслеживаем уникальные вопросы
    if (!askedQuestions.has(d.flag)) {
      askedQuestions.add(d.flag);
      houseDialogCount++;
    }
    
    // Показываем прогресс
    const progress = dv.paragraph(`**Вопросов задано: ${houseDialogCount}/5**`);
    progress.classList.add('house-dialog-result');
    
    // Проверяем, готов ли Антон раскрыть секреты
    if (houseDialogCount >= 3) {
      const reveal = dv.paragraph("**🎭 Антон колеблется и прикрывает лицо рукой, словно скрывая боль.**\n**«Я надеялся хранить это в тайне, но теперь…»**");
      reveal.classList.add('house-dialog-result');
      
      // Показываем скрытую дверцу
      const door = dv.paragraph("**🚪 В зашторенном окне вы замечаете скрытую дверцу — переход к следующей сцене.**");
      door.classList.add('house-dialog-result');
    }
  }});
}

// Кнопка сброса диалогов в доме
dv.el("button", "🔄 Сбросить диалоги в доме", {onclick: () => {
  const dialogs = houseDialogContainer.querySelectorAll('.house-dialog-result');
  dialogs.forEach(el => el.remove());
  houseDialogCount = 0;
  askedQuestions.clear();
  dv.paragraph("**Диалоги в доме сброшены.**");
}});
```

---

<button type="link" target="Investigation.md" class="btn btn-primary">Вперёд → Расследование</button>