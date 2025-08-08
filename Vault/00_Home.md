---
title: "🏛️ Мир Глёудероффа"
tags: [home, index]
cssclass: home
---

<div class="column-container">

<div class="column-left">

# 🏛️ Добро пожаловать в мир Лоры

Здесь собраны все основные материалы:  
- 🌍 [[01_World/Lore|Мир и Лор]]  
- 📜 [[01_World/Pantheon|Пантеон]]  
- 🗓️ [[01_World/Calendar|Календарь]]  

## 🗺️ Приключения

```dataview
TABLE file.link AS "Приключение", file.cday AS "Создано"
FROM "02_Adventures"
WHERE file.name != "Template_Adventure"
SORT file.cday asc
```

- [[02_Adventures/Adventure_001/Adventure_001|Налоги в Рассветной Лощине]]

## 🔊 Быстрые ссылки

📋 [[03_NPC/Index|Все NPC]]  
🌆 [[04_Locations/Index|Все локации]]

</div>

<div class="column-right">

> [!tip] Навигация
> Используйте ссылки слева для быстрого перехода к нужным разделам мира.

> [!warning] Важно
> Все приключения автоматически индексируются через Dataview.

| Раздел | Описание |
|--------|----------|
| Мир | Основная информация о мире |
| Приключения | Все ваншоты и кампании |
| NPC | Персонажи мира |
| Локации | Места и локации |

![Карта мира](06_Assets/рассветная_лощина.png){: .scroll-image}

</div>

</div>