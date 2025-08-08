---
title: "Расследование - Налоги в Рассветной Лощине"
tags:
  - adventure
  - investigation
  - tax-collection
cssclass: adventure
parent_adventure: "Adventure_001.md"
scene_type: "investigation"
collected_clues: 0
rumor_count: 0
found_rumor_1: false
found_rumor_2: false
found_rumor_3: false
found_rumor_4: false
found_rumor_5: false
rumor_truth_1: false
rumor_truth_2: false
rumor_truth_3: false
rumor_truth_4: false
rumor_truth_5: false
cultist_rumor: false
trust_level: 0
---

# 🕵️ Расследование

<!-- TEXT HERE -->

## 🔍 Сбор улик

<!-- TEXT HERE -->

```dataviewjs
// Счётчик улик и система расследования
let clueCount = 0;
const maxClues = 5;

// Создаём контейнер для результатов
const investigationContainer = dv.container;

// Отображаем текущий прогресс
const progressDisplay = dv.paragraph(`**🔍 Собрано улик: ${clueCount}/${maxClues}**`);
progressDisplay.classList.add('investigation-progress');

// Кнопки для сбора улик
const investigationActions = [
  {name: "Опрос Надеи", desc: "Расспросить трактирщицу о последних событиях"},
  {name: "Осмотр часовни", desc: "Исследовать полуразрушенную часовню"},
  {name: "Изучение Перелога", desc: "Изучить древние записи и документы"}
];

for (let action of investigationActions) {
  dv.el("button", action.name, {onclick: () => {
    // Удаляем предыдущие результаты
    const existingResults = investigationContainer.querySelectorAll('.investigation-result');
    existingResults.forEach(el => el.remove());
    
    // Добавляем новый результат
    const result = dv.paragraph(`**${action.name} — выполнено**\n${action.desc}`);
    result.classList.add('investigation-result');
    
    // Увеличиваем счётчик улик
    clueCount = Math.min(clueCount + 1, maxClues);
    
    // Обновляем отображение прогресса
    const progressElements = investigationContainer.querySelectorAll('.investigation-progress');
    progressElements.forEach(el => el.remove());
    
    const newProgress = dv.paragraph(`**🔍 Собрано улик: ${clueCount}/${maxClues}**`);
    newProgress.classList.add('investigation-progress');
    
    // Проверяем завершение расследования
    if (clueCount >= maxClues) {
      const completion = dv.paragraph("**✅ Расследование завершено! Достаточно улик для перехода к конфликту.**");
      completion.classList.add('investigation-result');
    }
  }});
}

// Кнопка сброса расследования
dv.el("button", "🔄 Сбросить расследование", {onclick: () => {
  const results = investigationContainer.querySelectorAll('.investigation-result, .investigation-progress');
  results.forEach(el => el.remove());
  clueCount = 0;
  const newProgress = dv.paragraph(`**🔍 Собрано улик: ${clueCount}/${maxClues}**`);
  newProgress.classList.add('investigation-progress');
  dv.paragraph("**Расследование сброшено.**");
}});
```

## 🗣️ Система слухов

<!-- TEXT HERE -->

```dataviewjs
// Механика слухов: 5 слухов (3 правдивых, 2 ложных)
const rumors = [
  {n: 1, text: "В лесу по ночам видят огоньки — это души пропавших.", truth: true},
  {n: 2, text: "Глава деревни хранит под полом старый меч с рунами.", truth: true},
  {n: 3, text: "В колодце поселился злой дух, и теперь вода проклята.", truth: false},
  {n: 4, text: "Каждое полнолуние кто-то исчезает — это жертва для леса.", truth: true},
  {n: 5, text: "Трактирщица Надея — ведьма, она умеет говорить с воронами.", truth: false}
];

const rumorContainer = dv.container;

for (let r of rumors) {
  dv.el("button", `Спросить у жителей слух №${r.n}`, {
    onclick: () => {
      // Показываем callout с текстом слуха
      const callout = dv.paragraph(`> [!${r.truth ? 'tip' : 'warning'}] Слух №${r.n}\n${r.text}`);
      callout.classList.add('rumor-callout');
      // В реальной механике здесь бы выставлялись флаги found_rumor_N и увеличивался rumor_count
    }
  });
}

// Кнопка сброса слухов
dv.el("button", "🔄 Сбросить слухи", {onclick: () => {
  const callouts = rumorContainer.querySelectorAll('.rumor-callout');
  callouts.forEach(el => el.remove());
  dv.paragraph("**Слухи сброшены.**");
}});
```

## 🔍 Проверка слухов

<!-- TEXT HERE -->

```dataviewjs
// Кнопка проверки слуха (пример для слуха №1)
dv.el("button", "Проверить слух №1 (Интеллект/Проницательность DC 13)", {
  onclick: () => {
    // Симуляция проверки (рандом)
    const success = Math.random() > 0.5;
    const callout = dv.paragraph(`> [!${success ? 'tip' : 'danger'}] ${success ? 'Слух подтверждён!' : 'Слух оказался выдумкой.'}`);
    callout.classList.add('rumor-check-callout');
    // В реальной механике здесь бы выставлялся флаг rumor_truth_1
  }
});

// Кнопка сброса проверки слуха
dv.el("button", "🔄 Сбросить проверку слуха", {onclick: () => {
  const callouts = document.querySelectorAll('.rumor-check-callout');
  callouts.forEach(el => el.remove());
  dv.paragraph("**Проверка слуха сброшена.**");
}});
```

## 🎭 Влияние колдуна

<!-- TEXT HERE -->

```dataviewjs
// Кнопка: Колдун подбрасывает ложный слух
dv.el("button", "Подбросить ложный слух", {
  onclick: () => {
    const callout = dv.paragraph("> [!danger] Жители встревожены новыми слухами…\nКолдун распространил ложный слух, доверие к офицерам падает!");
    callout.classList.add('cultist-rumor-callout');
    // В реальной механике здесь бы выставлялся флаг cultist_rumor и уменьшался trust_level
  }
});

// Кнопка сброса
dv.el("button", "🔄 Сбросить эффект колдуна", {onclick: () => {
  const callouts = document.querySelectorAll('.cultist-rumor-callout');
  callouts.forEach(el => el.remove());
  dv.paragraph("**Влияние колдуна сброшено.**");
}});
```

## 📊 Статистика расследования

<!-- TEXT HERE -->

```dataviewjs
// Итоговая таблица слухов (пример)
const total = 5; // всего слухов
const found = [dv.current().found_rumor_1, dv.current().found_rumor_2, dv.current().found_rumor_3, dv.current().found_rumor_4, dv.current().found_rumor_5].filter(Boolean).length;
const trueRumors = [dv.current().rumor_truth_1, dv.current().rumor_truth_2, dv.current().rumor_truth_3, dv.current().rumor_truth_4, dv.current().rumor_truth_5].filter(Boolean).length;
const falseRumors = found - trueRumors;

dv.table([
  "Всего собранных слухов", "Правдивых слухов", "Ложных слухов"
], [
  [found, trueRumors, falseRumors]
]);
```

---

<button type="link" target="Conflict.md" class="btn btn-primary">Вперёд → Конфликт</button>
