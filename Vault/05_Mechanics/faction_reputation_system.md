---
title: Репутация фракций
aliases:
  - Faction Reputation
  - Отношения с фракциями
tags:
  - репутация
  - фракции
  - отношения
  - интерактив
authors:
  - Gleuderoff
date: 2025-08-05
cssclasses:
  - faction-system
  - interactive-reputation
factions: '[{"name":"Гильдия Магов","reputation":0},{"name":"Воры и Убийцы","reputation":0},{"name":"Мастерская!!!","reputation":0}]'

---

# ⚔️ Репутация фракций

> _«В мире политики и интриг каждое действие имеет последствия...»_
> 
> *Следите за отношениями с различными фракциями мира. Союзники сегодня могут стать врагами завтра.*

---

```dataviewjs
const { update } = app.plugins.plugins["metaedit"].api;
const filePath = dv.current().file.path;

// Парсим фракции из YAML frontmatter
let factionsData = [];
try {
  factionsData = JSON.parse(dv.current().factions || '[]');
} catch (e) {
  factionsData = [
    {"name": "Гильдия Магов", "reputation": 0},
    {"name": "Торговый Союз", "reputation": 25},
    {"name": "Воры и Убийцы", "reputation": -30}
  ];
}

// Функция для сохранения данных фракций
async function saveFactions(factions) {
  await update('factions', JSON.stringify(factions), filePath);
}

// Функция создания фантазийного ползунка репутации
function createFantasySlider(faction, index) {
  const container = this.container.createEl('div', { cls: 'faction-block fantasy-block' });
  
  // Заголовок фракции с декоративными элементами
  const header = container.createEl('div', { cls: 'faction-header fantasy-header' });
  
  // Декоративная рамка заголовка
  const titleFrame = header.createEl('div', { cls: 'title-frame' });
  const title = titleFrame.createEl('h3', { text: faction.name, cls: 'faction-name fantasy-title' });
  
  // Кнопка удаления в стиле фэнтези
  const deleteBtn = header.createEl('button', { 
    text: '⚔️', 
    cls: 'btn fantasy-btn btn-delete',
    attr: { title: 'Изгнать фракцию' }
  });
  deleteBtn.onclick = async () => {
    if (confirm(`Изгнать фракцию "${faction.name}" из списка?`)) {
      factionsData.splice(index, 1);
      await saveFactions(factionsData);
      this.container.empty();
      dv.current().file.renderer.render();
    }
  };
  
  // Текущий статус репутации
  const status = getReputationStatus(faction.reputation);
  const statusContainer = container.createEl('div', { cls: 'status-container' });
  const statusEl = statusContainer.createEl('div', { 
    cls: `reputation-badge ${status.class}`,
    text: `${status.icon} ${status.text}`
  });
  
  // Значение репутации
  const valueEl = statusContainer.createEl('div', { 
    cls: 'reputation-value',
    text: `(${faction.reputation})`
  });
  
  // Контейнер для ползунка в стиле игры
  const sliderContainer = container.createEl('div', { cls: 'fantasy-slider-container' });
  
  // Метки "Неприятель" и "Гость"
  const leftLabel = sliderContainer.createEl('div', { 
    cls: 'slider-label left-label',
    text: 'Неприятель'
  });
  const rightLabel = sliderContainer.createEl('div', { 
    cls: 'slider-label right-label',
    text: 'Гость'
  });
  
  // Основная полоса ползунка
  const trackContainer = sliderContainer.createEl('div', { cls: 'track-container' });
  const track = trackContainer.createEl('div', { cls: 'fantasy-track' });
  
  // Заполненная часть (прогресс)
  const progress = track.createEl('div', { cls: 'track-progress' });
  
  // Ползунок (орб)
  const thumb = track.createEl('div', { cls: 'fantasy-thumb' });
  thumb.innerHTML = '💎'; // Кристалл как ползунок
  
  // Позиционирование ползунка
  const position = Math.max(0, Math.min(100, (faction.reputation + 100) / 2));
  thumb.style.left = `${position}%`;
  progress.style.width = `${position}%`;
  
  // Обновление цвета прогресса
  updateProgressColor(progress, faction.reputation);
  
  // Кнопки управления в фэнтези стиле
  const controls = container.createEl('div', { cls: 'fantasy-controls' });
  
  // Кнопка уменьшения
  const decreaseBtn = controls.createEl('button', { 
    text: '−', 
    cls: 'fantasy-btn control-btn decrease-btn',
    attr: { title: 'Ухудшить отношения (-5)' }
  });
  decreaseBtn.onclick = async () => {
    faction.reputation = Math.max(-100, faction.reputation - 5);
    factionsData[index] = faction;
    await saveFactions(factionsData);
    updateDisplay();
  };
  
  // Большая кнопка уменьшения
  const bigDecreaseBtn = controls.createEl('button', { 
    text: '−−', 
    cls: 'fantasy-btn control-btn big-decrease-btn',
    attr: { title: 'Серьёзно испортить отношения (-25)' }
  });
  bigDecreaseBtn.onclick = async () => {
    faction.reputation = Math.max(-100, faction.reputation - 25);
    factionsData[index] = faction;
    await saveFactions(factionsData);
    updateDisplay();
  };
  
  // Поле точного ввода
  const valueInput = controls.createEl('input', { 
    cls: 'fantasy-input',
    attr: { 
      type: 'number', 
      min: '-100', 
      max: '100', 
      value: faction.reputation.toString(),
      title: 'Точное значение репутации'
    }
  });
  valueInput.onchange = async () => {
    const newValue = Math.max(-100, Math.min(100, parseInt(valueInput.value) || 0));
    faction.reputation = newValue;
    factionsData[index] = faction;
    await saveFactions(factionsData);
    updateDisplay();
  };
  
  // Большая кнопка увеличения
  const bigIncreaseBtn = controls.createEl('button', { 
    text: '++', 
    cls: 'fantasy-btn control-btn big-increase-btn',
    attr: { title: 'Значительно улучшить отношения (+25)' }
  });
  bigIncreaseBtn.onclick = async () => {
    faction.reputation = Math.min(100, faction.reputation + 25);
    factionsData[index] = faction;
    await saveFactions(factionsData);
    updateDisplay();
  };
  
  // Кнопка увеличения
  const increaseBtn = controls.createEl('button', { 
    text: '+', 
    cls: 'fantasy-btn control-btn increase-btn',
    attr: { title: 'Улучшить отношения (+5)' }
  });
  increaseBtn.onclick = async () => {
    faction.reputation = Math.min(100, faction.reputation + 5);
    factionsData[index] = faction;
    await saveFactions(factionsData);
    updateDisplay();
  };
  
  // Функция обновления отображения
  function updateDisplay() {
    const newStatus = getReputationStatus(faction.reputation);
    statusEl.textContent = `${newStatus.icon} ${newStatus.text}`;
    statusEl.className = `reputation-badge ${newStatus.class}`;
    valueEl.textContent = `(${faction.reputation})`;
    
    const newPosition = Math.max(0, Math.min(100, (faction.reputation + 100) / 2));
    thumb.style.left = `${newPosition}%`;
    progress.style.width = `${newPosition}%`;
    updateProgressColor(progress, faction.reputation);
    valueInput.value = faction.reputation.toString();
  }
  
  return container;
}

// Функция обновления цвета прогресса
function updateProgressColor(progressEl, reputation) {
  if (reputation >= 50) {
    progressEl.style.background = 'linear-gradient(90deg, #4CAF50, #8BC34A)';
  } else if (reputation >= 0) {
    progressEl.style.background = 'linear-gradient(90deg, #FFC107, #4CAF50)';
  } else if (reputation >= -50) {
    progressEl.style.background = 'linear-gradient(90deg, #FF9800, #FFC107)';
  } else {
    progressEl.style.background = 'linear-gradient(90deg, #F44336, #FF9800)';
  }
}

// Функция определения статуса репутации
function getReputationStatus(rep) {
  if (rep >= 75) return { text: 'Верный союзник', icon: '👑', class: 'status-hero' };
  if (rep >= 50) return { text: 'Союзник', icon: '⚔️', class: 'status-ally' };
  if (rep >= 25) return { text: 'Дружелюбны', icon: '🤝', class: 'status-friend' };
  if (rep >= 0) return { text: 'Нейтральны', icon: '⚖️', class: 'status-neutral' };
  if (rep >= -25) return { text: 'Подозрительны', icon: '👁️', class: 'status-wary' };
  if (rep >= -50) return { text: 'Враждебны', icon: '⚡', class: 'status-hostile' };
  return { text: 'Заклятые враги', icon: '💀', class: 'status-enemy' };
}

// === ОСНОВНОЙ ИНТЕРФЕЙС ===
dv.header(2, '🏛️ Текущие отношения');

// Отображаем все фракции
factionsData.forEach((faction, index) => {
  createFantasySlider.call(this, faction, index);
});

// === ДОБАВЛЕНИЕ НОВОЙ ФРАКЦИИ ===
dv.header(3, '➕ Управление фракциями');

const addContainer = this.container.createEl('div', { cls: 'add-faction-fantasy' });

// Поле ввода в фэнтези стиле
const inputContainer = addContainer.createEl('div', { cls: 'input-container' });
const factionInput = inputContainer.createEl('input', {
  cls: 'fantasy-name-input',
  attr: { 
    type: 'text', 
    placeholder: 'Название новой фракции...', 
    maxlength: '50'
  }
});

// Кнопка добавления
const addBtn = addContainer.createEl('button', {
  text: '🏰 Добавить фракцию',
  cls: 'fantasy-btn add-faction-btn'
});

// Обработчик добавления
addBtn.onclick = async () => {
  const factionName = factionInput.value.trim();
  if (!factionName) {
    alert('Введите название фракции!');
    factionInput.focus();
    return;
  }
  
  if (factionsData.some(f => f.name.toLowerCase() === factionName.toLowerCase())) {
    alert('Фракция с таким названием уже существует!');
    factionInput.focus();
    return;
  }
  
  factionsData.push({
    name: factionName,
    reputation: 0
  });
  
  await saveFactions(factionsData);
  factionInput.value = '';
  
  this.container.empty();
  dv.current().file.renderer.render();
};

// Enter для добавления
factionInput.addEventListener('keypress', (e) => {
  if (e.key === 'Enter') {
    addBtn.click();
  }
});

// Автофокус на поле ввода
setTimeout(() => factionInput.focus(), 100);

// === СТАТИСТИКА ===
if (factionsData.length > 0) {
  dv.header(3, '📊 Общая статистика');
  
  const totalRep = factionsData.reduce((sum, f) => sum + f.reputation, 0);
  const avgRep = Math.round(totalRep / factionsData.length);
  const allies = factionsData.filter(f => f.reputation >= 25).length;
  const enemies = factionsData.filter(f => f.reputation <= -25).length;
  const neutral = factionsData.length - allies - enemies;
  
  const statsGrid = this.container.createEl('div', { cls: 'stats-grid' });
  
  const stats = [
    { label: 'Союзников', value: allies, icon: '⚔️', class: 'stat-allies' },
    { label: 'Нейтральных', value: neutral, icon: '⚖️', class: 'stat-neutral' },
    { label: 'Врагов', value: enemies, icon: '💀', class: 'stat-enemies' },
    { label: 'Средняя репутация', value: avgRep, icon: '📈', class: 'stat-average' }
  ];
  
  stats.forEach(stat => {
    const card = statsGrid.createEl('div', { cls: `stat-card ${stat.class}` });
    card.createEl('div', { text: stat.icon, cls: 'stat-icon' });
    card.createEl('div', { text: stat.value.toString(), cls: 'stat-number' });
    card.createEl('div', { text: stat.label, cls: 'stat-text' });
  });
  
  // Предупреждения
  if (enemies > allies) {
    dv.paragraph('> [!warning] ⚠️ **Тревожные времена!** Врагов больше, чем союзников. Стоит укрепить дипломатические связи.');
  } else if (allies > factionsData.length / 2) {
    dv.paragraph('> [!success] 🎉 **Превосходная дипломатия!** Большинство фракций поддерживает вас.');
  }
}

// === БЫСТРЫЕ ДЕЙСТВИЯ ===
if (factionsData.length > 1) {
  dv.header(4, '⚡ Быстрые действия');
  
  const actionContainer = this.container.createEl('div', { cls: 'quick-actions-fantasy' });
  
  const resetBtn = actionContainer.createEl('button', {
    text: '🔄 Дипломатический сброс',
    cls: 'fantasy-btn action-btn reset-btn'
  });
  
  resetBtn.onclick = async () => {
    if (confirm('Установить нейтральные отношения со всеми фракциями?')) {
      factionsData.forEach(faction => faction.reputation = 0);
      await saveFactions(factionsData);
      this.container.empty();
      dv.current().file.renderer.render();
    }
  };
  
  const randomBtn = actionContainer.createEl('button', {
    text: '🎲 Случайное событие',
    cls: 'fantasy-btn action-btn random-btn'
  });
  
  randomBtn.onclick = async () => {
    const randomFaction = factionsData[Math.floor(Math.random() * factionsData.length)];
    const change = Math.floor(Math.random() * 31) - 15; // от -15 до +15
    const oldRep = randomFaction.reputation;
    randomFaction.reputation = Math.max(-100, Math.min(100, randomFaction.reputation + change));
    
    await saveFactions(factionsData);
    
    const changeText = change > 0 ? `+${change}` : change.toString();
    const event = getRandomEvent(change);
    alert(`🎲 Случайное событие!\n\n${event}\n\n${randomFaction.name}: ${oldRep} → ${randomFaction.reputation} (${changeText})`);
    
    this.container.empty();
    dv.current().file.renderer.render();
  };
}

// Функция случайных событий
function getRandomEvent(change) {
  if (change > 10) return "🎉 Ваши герои помогли фракции в трудную минуту!";
  if (change > 5) return "🤝 Удачная дипломатическая встреча.";
  if (change > 0) return "📜 Небольшая услуга была оценена.";
  if (change === 0) return "⚖️ Нейтральное взаимодействие.";
  if (change > -5) return "😐 Незначительное недопонимание.";
  if (change > -10) return "😠 Конфликт интересов.";
  return "⚔️ Серьёзное противостояние!";
}
```

---