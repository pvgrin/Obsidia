---
Version: "0.2"
Description: "Блок выбора заметок, содержащих аналогичные топики"
Topics:
  - Метаданные
  - Template Block
---

#Meta/Metadata #Meta/Type/NoteBlock #Meta/Infrastructure
## Журнал изменений

### "0.2" 26-05-2023
- Добавлен вывод топиков для найденных заметок, для обоснования выбора
- Два блока заметок:
	- Поиск по топикам
	- Все заметки, которые ссылаются на текущую (`[[]]` lets you query from all files linking to the current file)

%% === NoteBlock Content === %%
>[!info]- Близкие заметки 
> ```dataview
>LIST WITHOUT ID rows[0].file.link  + choice(rows[0].Description, ": " + rows[0].Description, "") + " {"+rows[0].topics+"}"
>FROM ("") AND !#Meta/Metadata AND !#Meta/Infrastructure AND !#Meta/Hidden
>FLATTEN topics as flattenedTopics
>FLATTEN tags as flattenedTags
>WHERE (this.topics != null) AND (flattenedTopics != null) AND contains(this.topics, flattenedTopics) 
>	AND (file.name != this.file.name)
>FLATTEN file.mtime as mtime
>GROUP BY mtime
>SORT  mtime DESC
>LIMIT 10 
> ```
> ```dataview
> LIST WITHOUT ID rows[0].file.link  + choice(rows[0].Description, ": " + rows[0].Description, "") + " {"+rows[0].topics+"}"
> FROM [[]] AND !#Meta/Metadata AND !#Meta/Infrastructure AND !#Meta/Hidden
> FLATTEN file.mtime as mtime
> GROUP BY mtime
> SORT  mtime DESC
> LIMIT 10 
> ```
