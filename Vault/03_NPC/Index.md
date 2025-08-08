# 📋 Индекс NPC

## 👥 Список NPC

```dataview
TABLE file.link AS "NPC", alignment AS "Мировоззрение"
FROM "03_NPC"
WHERE contains(tags, "npc")
SORT file.name ASC
```

## 📍 Локации NPC

```dataview
TABLE file.link AS "NPC", location AS "Локация"
FROM "03_NPC"
WHERE contains(tags, "npc") AND location
SORT file.name ASC
```

## 📋 События NPC

```dataview
TABLE file.link AS "NPC", event AS "Событие"
FROM "03_NPC"
WHERE contains(tags, "npc") AND event
FLATTEN event
SORT file.name ASC
```