**NORMAL FORMS: ONE STEP AT A TIME**

![[Characteristics of a Relation.png]]

### Characteristics of a Relation

1. **Entity Representation**
   - Each row of the table holds data that pertain to some entity or a portion of some entity.

2. **Attribute Representation**
   - Each column of the table contains data that represent an attribute of the entity. For example, in an EMPLOYEE table:
     - Each row contains data about a particular employee.
     - Each column represents an attribute of the employee, such as LastName, Phone, or EmailAddress.

3. **Single Value per Cell**
   - The cells of the table must hold a single value. Repeating elements within a cell are not allowed.

4. **Consistent Data Types**
   - All entries in any column must be of the same kind. For example, if a column contains EmployeeNumber, every entry in that column must be an EmployeeNumber.

5. **Unique Column Names**
   - Each column must have a unique name.

6. **Unimportant Column Order**
   - The order of the columns within the table is unimportant.

7. **Unimportant Row Order**
   - The order of the rows is unimportant.

8. **Unique Rows**
   - The set of data values in each row must be unique. No two rows in the table may have identical sets of data values.
---
Instead of normalizing directly to BCNF, some people prefer to take a step-by-step approach, starting at 1NF and then moving through successive normal forms until BCNF. Here is a brief discussion of the successive normal forms and how to reach them.

A table and a spreadsheet are very similar to one another in that we can think of both as having rows, columns, and cells. Edgar Frank (E. F.) Codd, the originator of the relational model, defined three normal forms in an early paper on the relational model. He defined any table that meets the definition of a relation (see Figure 2-1 on page 71) as being in first normal form (1NF).

This definition, however, brings us back to the “To Key or Not to Key” discussion. Codd’s set of conditions for a relation does not require a primary key, but one is clearly implied by the condition that all rows must be unique. Thus, there are various opinions on whether a relation has to have a defined primary key to be in 1NF. For practical purposes, we will define 1NF as it is used in this book as a table that:

1. Meets the set of conditions for a relation, and
2. Has a defined primary key, and
3. No repeating groups.

For 1NF, ask yourself: Does the table meet the definition in Figure 2-1 such that there are no repeating groups, and does it have a defined primary key? If the answer is yes, then the table is in 1NF.

Codd pointed out that such tables can have anomalies (which are referred to elsewhere in the text as normalization problems), and he defined a second normal form (2NF) that eliminated some of those anomalies.

A relation is in 2NF if and only if (1) it is in 1NF and (2) all nonkey attributes are determined by the entire primary key. This means that if the primary key is a composite primary key, no nonkey attribute can be determined by an attribute or attributes that make up only part of the key. Thus, if you have a relation (A, B, N, O, P) with the composite key (A, B), then none of the nonkey attributes—N, O, or P—can be determined by just A or just B.

For 2NF, ask yourself: (1) Is the table in 1NF, and (2) are all nonkey attributes determined by only the entire primary key rather than part of the primary key? If the answers are yes and yes, then the table is in 2NF.

Note that the problem solved by 2NF can only occur in a table with a composite primary key—if the table has a single column primary key, then this problem cannot occur and if the table is in 1NF it will also be in 2NF.

However, the conditions of 2NF did not eliminate all the anomalies, so Codd defined third normal form (3NF). A relation is in 3NF if and only if (1) it is in 2NF and (2) there are no nonkey attributes determined by another nonkey attribute. Technically, the situation described by the preceding condition is called a transitive dependency. Thus, in our relation (A, B, N, O, P) none of the nonkey attributes—N, O, or P—can be determined by N, O, or P or any combination of them.

For 3NF, ask yourself: (1) Is the table in 2NF, and (2) are there any nonkey attributes determined by another nonkey attribute or attributes? If the answers are yes and no, then the table is in 3NF.

Not long after Codd published his paper on normal forms, it was pointed out to him that even relations in 3NF could have anomalies. As a result, he and R. Boyce defined the Boyce-Codd Normal Form (BCNF), which eliminated the anomalies that had been found with 3NF. As stated earlier, a relation is in BCNF if and only if every determinant is a candidate key.

For BCNF, ask yourself: (1) Is the table in 3NF, and (2) are all determinants also candidate keys? If the answers are yes and yes, then the table is in BCNF.

1NF through BCNF are summed up in a widely known phrase:

I swear to construct my tables so that all nonkey columns are dependent on
- the key,  
[This is 1NF]
- the whole key,  
[This is 2NF]
- and nothing but the key,  
[This is 3NF and BCNF]
so help me Codd!

Also note that all these definitions were made in such a way that a relation in a higher normal form is defined to be in all lower normal forms. Thus, a relation in BCNF is automatically in 3NF, a relation in 3NF is automatically in 2NF, and a relation in 2NF is automatically in 1NF.

There the matter rested until others discovered another kind of dependency, called a multivalued dependency, which is discussed earlier in this chapter and is illustrated in exercises 2.40 and 2.41. To eliminate multivalued dependencies, fourth normal form (4NF) was defined. To put tables into 4NF, the initial table must be split into tables such that the multiple values of any multivalued attribute are moved into the new tables. These are then accessed via 1:N relationships between the original table and the tables holding the multiple values.

For 4NF, ask yourself: (1) Have the multiple values determined by any multivalued dependency been moved into a separate table? If the answer is yes, then the tables are in 4NF.

A little later, another kind of anomaly involving tables that can be split apart but not correctly joined back together was identified, and fifth normal form (5NF) was defined to eliminate that type of anomaly. A discussion of 5NF is beyond the scope of this book.

You can see how the knowledge evolved: None of these normal forms was perfect—each one eliminated certain anomalies, and none asserted that it was vulnerable to no anomaly at all. At this stage, in 1981, R. Fagin took a different approach and asked why, rather than just chipping away at anomalies, we do not look for conditions that would have to exist in order for a relation to have no anomalies at all. He did just that and, in the process, defined domain/key normal form (DK/NF), and, no, that is not a typo—the name has the slash between domain and key, while the acronym places it between DK and NF! Fagin proved that a relation in DK/NF can have no anomalies, and he further proved that a relation that has no anomalies is also in DK/NF.

For some reason, DK/NF never caught the fancy of the general database population, but it should have. As you can tell, no one should brag that their relations are in BCNF—instead we should all brag that our relations are in DK/NF. But for some reason (perhaps because there is fashion in database theory, just as there is fashion in clothes), it just is not done.

You are probably wondering what the conditions of DK/NF are. Basically, DK/NF requires that all the constraints on data values be logical implications of the definition of domains and keys. To the level of detail of this text, and to the level of detail experienced by 99 percent of all database practitioners, this can be restated as follows: Every determinant of a functional dependency must be a candidate key. This is exactly where we started and what we have defined as BCNF.

You can broaden this statement a bit to include multivalued dependencies and say that every determinant of a functional or multivalued dependency must be a candidate key. The trouble with this is that as soon as we constrain a multivalued dependency in this way, it is transformed into a functional dependency. Our original statement is fine. It is like saying that good health comes to overweight people who lose weight until they are of an appropriate weight. As soon as they lose their excess weight, they are no longer overweight. Hence, good health comes to people who have appropriate weight.

For DK/NF, ask yourself: Is the table in BCNF? For our purposes in this book, the two terms are synonymous, so if the answer is yes, we will consider that the table is also in DK/NF.

So, as Paul Harvey used to say, “Now you know the rest of the story.” Just ensure that every determinant of a functional dependency is a candidate key (BCNF), and you can claim that your relations are fully normalized. You do not want to say they are in DK/NF until you learn more about it, though, because someone might ask you what that means. However, for most practical purposes, your relations are in DK/NF as well.

Note: For more information on normal forms, see David M. Kroenke and David J. Auer, *Database Processing: Fundamentals, Design, and Implementation*, 14th ed. (Upper Saddle River, NJ: Prentice Hall, 2016): 149–167.

For another view of normalization see Marc Rettig’s *5 Rules of Data Normalization* presented in poster form at [Marc Rettig’s 5 Rules of Normalization Poster](http://www.databaseanswers.org/downloads/Marc_Rettig_5_Rules_of_Normalization_Poster.pdf).


---
Нормальные формы (NF) — это правила для организации данных в реляционных базах данных, которые помогают устранить избыточность и аномалии при вставке, обновлении и удалении данных. Эти формы строятся на основе функциональных зависимостей и многозначных зависимостей и позволяют создавать более структурированные и эффективные базы данных.

Вот подробное описание основных нормальных форм:

### Первая Нормальная Форма (1NF)

**Определение**:
Таблица находится в первой нормальной форме (1NF), если:
1. Все атрибуты содержат атомарные (неделимые) значения.
2. Каждый атрибут содержит одно значение на одну строку (нет повторяющихся групп или множественных значений).

**Пример**:
Таблица, содержащая списки курсов студентов в одном поле:

| StudentID | Name      | Courses          |
|-----------|-----------|------------------|
| 1         | Alice     | Math, Science    |
| 2         | Bob       | Math, History    |

**После приведения к 1NF**:

| StudentID | Name      | Course   |
|-----------|-----------|----------|
| 1         | Alice     | Math     |
| 1         | Alice     | Science  |
| 2         | Bob       | Math     |
| 2         | Bob       | History  |

### Вторая Нормальная Форма (2NF)

**Определение**:
Таблица находится во второй нормальной форме (2NF), если:
1. Она находится в первой нормальной форме (1NF).
2. Все неключевые атрибуты полностью функционально зависят от всего составного первичного ключа (если он есть). 

**Пример**:
Таблица с составным ключом `StudentID` и `CourseID`:

| StudentID | CourseID | InstructorName | InstructorPhone |
|-----------|----------|----------------|-----------------|
| 1         | 101      | Dr. Smith      | 123-456-7890    |
| 1         | 102      | Dr. Jones      | 234-567-8901    |
| 2         | 101      | Dr. Smith      | 123-456-7890    |

**После приведения ко 2NF**:
Разделите таблицу на две, чтобы устранить частичную зависимость:

**Таблица `StudentCourses`**:

| StudentID | CourseID |
|-----------|----------|
| 1         | 101      |
| 1         | 102      |
| 2         | 101      |

**Таблица `Instructors`**:

| CourseID | InstructorName | InstructorPhone |
|----------|----------------|-----------------|
| 101      | Dr. Smith      | 123-456-7890    |
| 102      | Dr. Jones      | 234-567-8901    |

### Третья Нормальная Форма (3NF)

**Определение**:
Таблица находится в третьей нормальной форме (3NF), если:
1. Она находится во второй нормальной форме (2NF).
2. Все атрибуты, не являющиеся ключами, не зависят транзитивно от первичного ключа (т.е. если A → B и B → C, то A не должен определять C).

**Пример**:
Таблица `Students` с избыточной информацией:

| StudentID | Name      | Department | DepartmentHead |
|-----------|-----------|------------|----------------|
| 1         | Alice     | CS         | Dr. White      |
| 2         | Bob       | Math       | Dr. Black      |

**После приведения к 3NF**:
Разделите таблицу, чтобы устранить транзитивные зависимости:

**Таблица `Students`**:

| StudentID | Name      | Department |
|-----------|-----------|------------|
| 1         | Alice     | CS         |
| 2         | Bob       | Math       |

**Таблица `Departments`**:

| Department | DepartmentHead |
|------------|----------------|
| CS         | Dr. White      |
| Math       | Dr. Black      |

### Четвертая Нормальная Форма (4NF)

**Определение**:
Таблица находится в четвертой нормальной форме (4NF), если:
1. Она находится в третьей нормальной форме (3NF).
2. Она не содержит многозначных зависимостей.

**Пример**:
Таблица `StudentSkills` с многозначными зависимостями:

| StudentID | Skill       | Language   |
|-----------|-------------|------------|
| 1         | Programming | Python     |
| 1         | Programming | Java       |
| 1         | Language    | English    |
| 2         | Design      | Photoshop  |
| 2         | Language    | French     |

**После приведения к 4NF**:
Разделите таблицу, чтобы устранить многозначные зависимости:

**Таблица `StudentSkills`**:

| StudentID | Skill       |
|-----------|-------------|
| 1         | Programming |
| 1         | Language    |
| 2         | Design      |

**Таблица `StudentLanguages`**:

| StudentID | Language   |
|-----------|------------|
| 1         | Python     |
| 1         | Java       |
| 1         | English    |
| 2         | French     |

**Таблица `StudentDesigns`**:

| StudentID | DesignTool |
|-----------|------------|
| 2         | Photoshop  |

### Пятая Нормальная Форма (5NF)

**Определение**:
Таблица находится в пятой нормальной форме (5NF), если:
1. Она находится в четвертой нормальной форме (4NF).
2. Она не содержит соединительных (join) зависимостей, которые могут привести к потере информации при выполнении объединений таблиц.

**Пример**:
Рассмотрим таблицу `ProjectAssignments`:

| EmployeeID | ProjectID | Role    |
|------------|-----------|---------|
| 1          | 101       | Developer |
| 1          | 102       | Tester   |
| 2          | 101       | Tester   |
| 2          | 102       | Developer |

**После приведения к 5NF**:
Разделите таблицу на три:

**Таблица `EmployeeProjects`**:

| EmployeeID | ProjectID |
|------------|-----------|
| 1          | 101       |
| 1          | 102       |
| 2          | 101       |
| 2          | 102       |

**Таблица `EmployeeRoles`**:

| EmployeeID | Role    |
|------------|---------|
| 1          | Developer |
| 1          | Tester   |
| 2          | Tester   |
| 2          | Developer |

**Таблица `ProjectRoles`**:

| ProjectID | Role    |
|-----------|---------|
| 101       | Developer |
| 101       | Tester   |
| 102       | Developer |
| 102       | Tester   |

### Шестая Нормальная Форма (6NF)

**Определение**:
Таблица находится в шестой нормальной форме (6NF), если:
1. Она находится в пятой нормальной форме (5NF).
2. Она не содержит временных зависимостей.

**Пример**:
Эта форма применяется в основном в системах, где данные изменяются со временем, например, в хранилищах данных и в системах временных данных. Системы, использующие 6NF, часто включают атрибуты времени и даты, чтобы отслеживать изменения данных во времени.

В общем, процесс нормализации направлен на структурирование данных таким образом, чтобы уменьшить избыточность и предотвратить аномалии, что делает работу с данными более эффективной и надежной.