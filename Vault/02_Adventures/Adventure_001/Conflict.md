---
title: "Конфликт - Налоги в Рассветной Лощине"
tags:
  - adventure
  - conflict
  - tax-collection
cssclass: adventure
parent_adventure: "Adventure_001.md"
scene_type: "conflict"
battle_started: false
forest_wizard_defeated: false
cultists_defeated: 0
total_cultists: 3
---

# ⚔️ Конфликт

> [!quote] *«Когда последние лучи солнца уступают место теням, а древние силы пробуждаются от векового сна...»*

Сердце **Рассветной Лощины** бьётся всё быстрее: ветер поёт унылые псалмы между разломанными стенами, а запутанные коридоры подземного комплекса сжимают грудь, словно щупальца древнего ужаса. Офицеры стоят плечом к плечу, отдавая дань древнему воинскому ритуалу: один держит факел, другой — щит, третий — кинжал, четвёртый — посох предков. В их глазах отражается холодный свет рунических резов на камнях, а дыхание смолкает, когда они пересекают порог древнего хранилища.

> [!warning] **Напряжение нарастает**
> Каменный пол под ногами вибрирует от надвигающейся власти потустороннего мира. На мгновение кажется, что кто-то шепчет ваши имена из тени. Это зов Лесного колдуна — его вихрь тьмы уже близко.

Сердца героев ощутят истинное испытание: рука об руку они вступят в бой, где каждая рана станет актом мужества, а каждый заклинатель — вихрем бездны.


## 🌲 Встреча с Лесным колдуном

> [!danger] **Появление антагониста**
> Когда последние отблески факельного света уступают место лунному сиянию, из глубины леса он появляется — **Лесной колдун**.

Его лицо сокрыто в полумраке капюшона, а уголки губ скривлены насмешкой. В руках он держит посох, оплетённый жадными лозами, словно само сердце Ардены стремится вырваться наружу.

> [!note] **Его прислужники**
> Вокруг стоят три его послушника: их глаза горят холодным пламенем, а дыхание отдаёт запахом забвения. Они встретят вас фанатичной решимостью, готовые обрушить на вас силу древнего культа.

Колдун заговорит тихим, ласковым шёпотом, убаюкивая лес:

> [!quote] **Лесной колдун:**
> «Вы пришли, стражи Империи. Здесь, в сердце Ардены, давно забыли законы людей.  
> Я предлагаю вам выбор: отступить и покинуть эти земли…  
> или стать частью вечного балета природы, где смерть — лишь цветок, распустившийся в чёрном лесу.»

В его голосе слышится смешение древней печали и безумия, подобно яду, растворённому в нектаре. Но даже здесь, среди тёмных теней, мерцает огонёк надежды. И когда первый из прислужников поднимет руки, прозвучит призыв к битве, от исхода которой зависит сама душа Рассветной Лощины.

```dataviewjs
// Система битвы с лесным колдуном
let battleStarted = false;
let forestWizardDefeated = false;
let cultistsDefeated = 0;
const totalCultists = 3;

// Создаём контейнер для результатов битвы
const battleContainer = dv.container;

// Функция для отображения текущего состояния
function updateBattleStatus() {
  // Удаляем предыдущие статусы
  const existingStatuses = document.querySelectorAll('.battle-status');
  existingStatuses.forEach(el => el.remove());
  
  const statusDiv = dv.paragraph(`
    **📊 Статус битвы:**
    - Битва началась: ${battleStarted ? '✅' : '❌'}
    - Лесной колдун: ${forestWizardDefeated ? '💀 Побеждён' : '🌲 Жив'}
    - Прислужники: ${cultistsDefeated}/${totalCultists} побеждено
  `);
  statusDiv.classList.add('battle-status');
}

// Функция проверки готовности к развязкам
function checkReadinessForEndings() {
  // Удаляем предыдущие сообщения о готовности
  const existingReadiness = document.querySelectorAll('.ending-readiness');
  existingReadiness.forEach(el => el.remove());
  
  if (forestWizardDefeated && cultistsDefeated >= totalCultists) {
    const readiness = dv.paragraph("**✅ Конфликт разрешён! Готовы к развязкам.**");
    readiness.classList.add('ending-readiness');
  }
}

// Кнопка начала битвы
dv.el("button", "⚔️ Начать битву с лесным колдуном", {
  onclick: () => {
    if (!battleStarted) {
      battleStarted = true;
      
      // Удаляем предыдущие результаты
      const existingResults = battleContainer.querySelectorAll('.battle-result');
      existingResults.forEach(el => el.remove());
      
      // Показываем начало битвы
      const battleStart = dv.paragraph("**⚔️ Битва началась! Лесной колдун и его прислужники атакуют!**");
      battleStart.classList.add('battle-result');
      
      // Показываем статистику врагов
      const enemyStats = dv.paragraph(`**🌲 Лесной колдун** (CR 5, способности некроманта)\n**👥 Прислужники:** ${totalCultists - cultistsDefeated}/${totalCultists} осталось`);
      enemyStats.classList.add('battle-result');
      
      // Обновляем статус
      updateBattleStatus();
      checkReadinessForEndings();
    }
  }
});

// Кнопка атаки на колдуна
dv.el("button", "🗡️ Атаковать лесного колдуна", {
  onclick: () => {
    if (battleStarted && !forestWizardDefeated) {
      // Симуляция атаки (рандом)
      const success = Math.random() > 0.3; // 70% шанс успеха
      
      if (success) {
        forestWizardDefeated = true;
        const victory = dv.paragraph("**🎉 Лесной колдун побеждён!**");
        victory.classList.add('battle-result');
        
        // Проверяем завершение битвы
        if (cultistsDefeated >= totalCultists) {
          const battleEnd = dv.paragraph("**🏆 Битва завершена! Все враги побеждены!**");
          battleEnd.classList.add('battle-result');
        }
      } else {
        const miss = dv.paragraph("**💥 Атака промахнулась! Колдун уклоняется.**");
        miss.classList.add('battle-result');
      }
      
      // Обновляем статус
      updateBattleStatus();
      checkReadinessForEndings();
    }
  }
});

// Кнопка атаки на прислужников
dv.el("button", "⚔️ Атаковать прислужников", {
  onclick: () => {
    if (battleStarted && cultistsDefeated < totalCultists) {
      // Симуляция атаки (рандом)
      const success = Math.random() > 0.4; // 60% шанс успеха
      
      if (success) {
        cultistsDefeated++;
        const cultistDefeat = dv.paragraph(`**💀 Прислужник побеждён! Осталось: ${totalCultists - cultistsDefeated}**`);
        cultistDefeat.classList.add('battle-result');
        
        // Проверяем завершение битвы
        if (forestWizardDefeated && cultistsDefeated >= totalCultists) {
          const battleEnd = dv.paragraph("**🏆 Битва завершена! Все враги побеждены!**");
          battleEnd.classList.add('battle-result');
        }
      } else {
        const miss = dv.paragraph("**💥 Атака промахнулась! Прислужники уклоняются.**");
        miss.classList.add('battle-result');
      }
      
      // Обновляем статус
      updateBattleStatus();
      checkReadinessForEndings();
    }
  }
});

// Кнопка сброса битвы
dv.el("button", "🔄 Сбросить битву", {onclick: () => {
  const results = battleContainer.querySelectorAll('.battle-result');
  results.forEach(el => el.remove());
  battleStarted = false;
  forestWizardDefeated = false;
  cultistsDefeated = 0;
  dv.paragraph("**Битва сброшена.**");
  
  // Обновляем статус
  updateBattleStatus();
  checkReadinessForEndings();
}});

// Инициализация отображения статуса
updateBattleStatus();

// Проверяем готовность к развязкам один раз
checkReadinessForEndings();
```

## 🎭 Контейнер для результатов

<!-- TEXT HERE -->

*Статус битвы отображается автоматически выше в интерактивном блоке.*

## 🔚 Подготовка к развязкам

> [!success] **Кульминация близка**
> Когда последний вздох колдуна рассеется в ночном воздухе и руины обретут мёртвое спокойствие, ваши раны и решения сольются в единую нить судьбы.

Оттого, что останется стоять, зависит, какими красками замкнутся страницы этого приключения:

> [!tip] **Возможные исходы:**
> - **🕯️ Светлая победа** — рассветная заря очистит землю от праха  
> - **⚖️ Моральная сделка** — граница между добром и злом растворится в клятвах  
> - **💀 Трагедия** — жертва героя станет свечой, что сожгла тьму, но погасла сама  
> - **☠️ Хаос** — лес возьмёт своё, и рассвет никогда не озарит эти земли вновь

> [!info] **Следующий шаг**
> Нажмите **«Вперёд → Развязки»**, чтобы узнать, как сложатся ваши судьбы и завершится это эпическое испытание. 

---

<button type="link" target="Развязки.md" class="btn btn-primary">Вперёд → Развязки</button> 