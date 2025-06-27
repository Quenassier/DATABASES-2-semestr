# Финальная работа по базам данных

## Список таблиц (15 штук)

| №  | Название таблицы       | Описание содержимого               |
|----|------------------------|-----------------------------------|
| 1  | attendance       | Посещаемость занятий              |
| 2  | classrooms       | Аудитории и помещения             |
| 3  | curriculum       | Учебные планы                     |
| 4  | departments      | Кафедры университета              |
| 5  | disciplines      | Учебные дисциплины                |
| 6  | faculties        | Факультеты                        |
| 7  | grades           | Оценки студентов                  |
| 8  | payments         | Платежи и штрафы                  |
| 9  | research_works   | Научные работы                    |
| 10 | schedule         | Расписание занятий                |
| 11 | student_audit    | История изменений данных студентов|
| 12 | student_works    | Учебные работы студентов          |
| 13 | students         | Данные студентов                  |
| 14 | study_groups     | Учебные группы                    |
| 15 | teacher_workload| Нагрузка преподавателей           |
| 16 | teachers         | Данные преподавателей             |

## Детальный анализ ключевых таблиц
![image](https://github.com/user-attachments/assets/1b70c3b2-d78b-444c-a7e6-3ebaaa89bbfb)

## 1. Основные таблицы и их структура

### 1.1 Студенты
**student_id** (PK): Уникальный идентификатор студента
**group_id** (FK): Ссылка на учебную группу
**full_name**: Полное имя студента (макс. 100 символов)
**birth_date**: Дата рождения
**address**: Адрес проживания (макс. 200 символов)
**phone_number**: Контактный телефон (макс. 20 символов)
**avg_grade**: Средний балл (число с 2 знаками после запятой)

*Связи:*
Связана с `grades` через student_id (один ко многим)
Связана с `study_groups` через group_id (многие к одному)

![image](https://github.com/user-attachments/assets/39748658-69d4-4221-ab6b-a34f62ee2057)

### 1.2 Учебные группы
**group_id** (PK): Уникальный идентификатор группы
**group_name**: Название группы (напр. "EC-201")
**faculty_id** (FK): Ссылка на факультет
**curriculum_id** (FK): Ссылка на учебный план

*Связи:*
Связана с `students` (одна группа - много студентов)
Связана с `faculties` и `curriculum`

### 1.3 Оценки
**grade_id** (PK): Уникальный идентификатор оценки
**student_id** (FK): Ссылка на студента
**discipline_id** (FK): Ссылка на дисциплину
**teacher_id** (FK): Ссылка на преподавателя
**grade_value**: Оценка (целое число 1-5)
**exam_date**: Дата экзамена
**semester**: Номер семестра (1-12)

*Ограничения:*
Оценка только от 1 до 5
Номер семестра от 1 до 12

![image](https://github.com/user-attachments/assets/9083cb55-ff64-439b-b5ca-90a7c7ff8c8d)

## 2. Вспомогательные таблицы

### 2.1 Teachers (Преподаватели)
**teacher_id** (PK)
**full_name**
**department_id** (FK)
**academic_degree**
![image](https://github.com/user-attachments/assets/24d417fe-e8df-4b1c-929a-d734bba48ff7)
### 2.2 Disciplines (Дисциплины)
**discipline_id** (PK)
**discipline_name**
**hours_count**
**semester**

## 3. Таблицы для учета деятельности

### 3.1 Attendance (Посещаемость)
**attendance_id** (PK)
**student_id** (FK)
**schedule_id** (FK)
**status** (присутствовал/отсутствовал)
![image](https://github.com/user-attachments/assets/dce54f49-e9f1-42b5-9ff2-f8df70e638d6)
### 3.2 Payments (Платежи)
**payment_id** (PK)
**student_id** (FK)
**amount**
**payment_type**
**payment_date**

## Детализация ключевых связей

1. **Студенты → Оценки** (1:M)
Каждый студент может иметь несколько оценок
Связь: students.student_id → grades.student_id

2. **Группы → Студенты** (1:M)
В каждой группе может быть много студентов
Связь: study_groups.group_id → students.group_id

3. **Преподаватели → Оценки** (1:M)
Преподаватель может выставить много оценок
Связь: teachers.teacher_id → grades.teacher_id

4. **Дисциплины → Оценки** (1:M)
По каждой дисциплине может быть много оценок
Связь: disciplines.discipline_id → grades.discipline_id

## Важные ограничения данных

1. **Оценки** (grades.grade_value):
Только целые числа от 1 до 5
Проверка: `CHECK (grade_value >= 1 AND grade_value <= 5)`

2. **Семестры** (grades.semester):
Только значения от 1 до 12
Проверка: `CHECK (semester >= 1 AND semester <= 12)`

3. **Телефоны студентов** (students.phone_number):
Максимальная длина 20 символов

## Автоматизированные процессы

1. **Триггер обновления среднего балла**:
Автоматически пересчитывает avg_grade при изменении оценок
Название: `tr_update_avg`

2. **Триггер аудита студентов**:
Фиксирует все изменения в данных студентов
Название: `tr_student_audit`

![image](https://github.com/user-attachments/assets/24d417fe-e8df-4b1c-929a-d734bba48ff7)
![image](https://github.com/user-attachments/assets/4897cc75-4b41-40f4-b48a-d02e5a5d46d1)
![image](https://github.com/user-attachments/assets/4e18197d-d253-4d87-b144-d186d56152fc)
![image](https://github.com/user-attachments/assets/0fd060c5-59d4-435d-9c0e-d16878d38a57)
![image](https://github.com/user-attachments/assets/71995096-45ea-42f2-8180-71f507af1497)
![image](https://github.com/user-attachments/assets/1ded175a-d11f-4650-ae0e-d2213db2ad80)
![image](https://github.com/user-attachments/assets/6d5dc1b4-a145-4d40-aebc-34fa0470c893)
![image](https://github.com/user-attachments/assets/9083cb55-ff64-439b-b5ca-90a7c7ff8c8d)
![image](https://github.com/user-attachments/assets/5a4f943e-ee05-4d0a-961b-75e66eaf0716)
![image](https://github.com/user-attachments/assets/1953bde7-58a4-44c2-a048-7e3c5bb56203)
