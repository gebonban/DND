```dataview
TABLE file.link AS "Приключение", file.cday AS "Создано"
FROM "02_Adventures"
WHERE file.name != "Template_Adventure"
SORT file.cday asc
```