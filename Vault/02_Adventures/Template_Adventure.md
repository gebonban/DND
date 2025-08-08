---
title: "<% tp.file.prompt('Название приключения') %>"
aliases:
  - ""
tags:
  - adventure
  - one-shot
cssclass: adventure
date: <% tp.date.now("YYYY-MM-DD") %>
---

<div class="column-container">

<div class="column-left">

# 🏰 {{title}}

## 📖 Введение

## 🕵️ Расследование

```dataviewjs
// Шаблон для интерактивных действий с кнопкой сброса
const actions = [
  {name: "Действие 1", flag: "action_1"},
  {name: "Действие 2", flag: "action_2"},
  {name: "Действие 3", flag: "action_3"}
];

const resultsContainer = dv.container;

for (let a of actions) {
  dv.el("button", a.name, {onclick: () => {
    const existingResults = resultsContainer.querySelectorAll('.action-result');
    existingResults.forEach(el => el.remove());
    
    const result = dv.paragraph(`**${a.name} — выполнено**`);
    result.classList.add('action-result');
  }});
}

// Кнопка сброса
dv.el("button", "🔄 Сбросить все результаты", {onclick: () => {
  const results = resultsContainer.querySelectorAll('.action-result');
  results.forEach(el => el.remove());
  dv.paragraph("**Все результаты сброшены.**");
}});
```

## ⚔️ Конфликт

## 🎯 Кульминация

## 🔚 Развязки

```dataviewjs
// Шаблон для развязок с кнопкой сброса
const endings = [
  {name: "Развязка 1", desc: "Описание первой развязки."},
  {name: "Развязка 2", desc: "Описание второй развязки."},
  {name: "Развязка 3", desc: "Описание третьей развязки."},
  {name: "Развязка 4", desc: "Описание четвёртой развязки."}
];

const endingContainer = dv.container;

for (let e of endings) {
  dv.el("button", e.name, {onclick: () => {
    const existingEndings = endingContainer.querySelectorAll('.ending-result');
    existingEndings.forEach(el => el.remove());
    
    const result = dv.paragraph(e.desc);
    result.classList.add('ending-result');
  }});
}

// Кнопка сброса для развязок
dv.el("button", "🔄 Сбросить развязки", {onclick: () => {
  const endings = endingContainer.querySelectorAll('.ending-result');
  endings.forEach(el => el.remove());
  dv.paragraph("**Развязки сброшены.**");
}});
```

</div>

<div class="column-right">

> [!tip] Заметки Мастера
> Добавьте здесь важную информацию для ведущего.

> [!warning] Сложность
> Укажите уровень сложности и рекомендации.

| Персонаж | Роль | Локация |
|----------|------|---------|
| Пример | Роль | Локация |

> [!danger] Секреты
> Скрытая информация для Мастера.

![Иллюстрация](путь_к_изображению.png){: .scroll-image}

</div>

</div>
