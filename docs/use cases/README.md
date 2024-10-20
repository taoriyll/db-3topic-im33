# Модель прецедентів

## Діаграма прецедентів

<center style="
    border-radius:4px;
    border: 1px solid #cfd7e6;
    box-shadow: 0 1px 3px 0 rgba(89,105,129,.05), 0 1px 1px 0 rgba(0,0,0,.025);
    padding: 1em;"
>
    
@startuml

actor "Гість" as Guest
actor "Користувач" as User
actor "Адміністратор системи" as Admin

usecase "UserRegistration\nРеєстрація користувача в системі" as UC_1.1
usecase "UserAuthorization\nАвторизація користувача" as UC_1.2

Guest -u-> UC_1.1
Guest -u-> UC_1.2

usecase "Media content management\nПошук та управління медіа-контентом" as UC_2

User -u-|> Guest
User -d-> UC_2

usecase "User management\nКерування користувачем" as UC_3

Admin -u-|> Guest
Admin -d-> UC_3

@enduml

</center>

### Схема гостя
<center style="
    border-radius:4px;
    border: 1px solid #cfd7e6;
    box-shadow: 0 1px 3px 0 rgba(89,105,129,.05), 0 1px 1px 0 rgba(0,0,0,.025);
    padding: 1em;"
>
    
@startuml

actor "Гість" as Guest

usecase "UserRegistration\nРеєстрація користувача в системі" as UC_1.1
usecase "UserAuthorization\nАвторизація користувача" as UC_1.2

Guest -u-> UC_1.1
Guest -u-> UC_1.2

@enduml

</center>

### Схема користувача
<center style="
    border-radius:4px;
    border: 1px solid #cfd7e6;
    box-shadow: 0 1px 3px 0 rgba(89,105,129,.05), 0 1px 1px 0 rgba(0,0,0,.025);
    padding: 1em;"
>

@startuml

actor "Користувач" as User

usecase "Media content management\nПошук та управління медіа-контентом" as UC_1.1

usecase "MediaFind\nПошук медіа-контенту користувачем" as UC_2.1
usecase "MediaCreate\nСтворення медіа-контенту користувачем" as UC_2.2
usecase "CommentModeration\nМодерація коментарів у соціальній мережі" as UC_2.3
usecase "EditContent\nРедагування медіа-контенту" as UC_2.4
usecase "DeleteContent\nВидалення медіа-контенту" as UC_2.5

User -d-> UC_1.1

UC_2.1 .r.> UC_1.1 : extends
UC_2.2 .u.> UC_1.1 : extends
UC_2.3 .u.> UC_1.1 : extends
UC_2.4 .u.> UC_1.1 : extends
UC_2.5 .l.> UC_1.1 : extends

@enduml

</center>

### Схема адміністратора
<center style="
    border-radius:4px;
    border: 1px solid #cfd7e6;
    box-shadow: 0 1px 3px 0 rgba(89,105,129,.05), 0 1px 1px 0 rgba(0,0,0,.025);
    padding: 1em;"
>
    
@startuml

actor "Адміністратор" as Administrator

usecase "User management\nКерування користувачем" as UC_1.1

usecase "DeleteUser\nВидалення користувача" as UC_2.1
usecase "PromoteUser\nПідвищення ролі користувача" as UC_2.2

Administrator -d-> UC_1.1

UC_2.1 .u.> UC_1.1 : extends
UC_2.2 .u.> UC_1.1 : extends

@enduml

</center>

### Сценарій CommentModeration

   | ID               | CommentModeration |
|----------------------|-----------------|
| НАЗВА            | Модерація коментарів у соціальній мережі |
| УЧАСНИКИ         | Адміністратор, Система|
| ПЕРЕДУМОВИ       | Користувач залишив коментар, адміністратор має права на модерацію |
| РЕЗУЛЬТАТ        |Коментар опубліковано або видалено залежно від рішення адміністратора |
| ВИКЛЮЧНІ СИТУАЦІЇ| 1. Користувач залишив недопустимий коментар |
|                      | 2 Коментар вже видалено |
| ОСНОВНИЙ СЦЕНАРІЙ| 1. Адміністратор переглядає нові коментарі в системі|
|                      | 2.  Система перевіряє наявність недопустимого контенту|
|                      |3. Адміністратор вибирає коментар для перегляду |
|                      |4. Система надає опції для публікації або видалення коментаря|
|                      |5. Адміністратор приймає рішення про публікацію або видалення|
|                      |6.Система повідомляє користувача про результат (коментар опубліковано або видалено) |

@startuml
|Адміністратор|
start;
: Переглядає нові коментарі;

|Система|
: Надсилає список нових коментарів;

|Адміністратор|
: Вибирає коментар для перегляду;

|Система|
: Показує вміст коментаря;

|Адміністратор|
: Перевіряє наявність недопустимого контенту;
note right #FF6969
<b> Можлива виключна ситуація
<b> CommentAlreadyDeleted
end note

: Приймає рішення про публікацію або видалення;

|Система|
: Публікує або видаляє коментар;
note right #FF6969
<b> Можлива виключна ситуація
<b> InappropriateComment
end note

|Адміністратор|
: Перевіряє статус коментаря;

stop;
@enduml

### Сценарій MediaCreate

| **ID**              | MediaCreate                             |
|-----------------|----------------------------------------|
| **НАЗВА**       | Створення медіа-контенту      |
| **УЧАСНИКИ**   | Користувач, система      |
| **ПЕРЕДУМОВИ**  | 1. Користувач має доступ до інформаційної системи.<br>2. Користувач зареєстрований в системі.<br>3. Інформаційна система готова до отримання нового контенту (інтерфейс, ресурси, тощо) |
| **РЕЗУЛЬТАТ**   | Новий медіа-контент успішно створено та збережено в системі. Користувач отримує підтвердження про успішне створення контенту та можливість перегляду та редагування. |
| **ВИКЛЮЧНІ СИТУАЦІЇ** |1. InsufficientPermissionsException — користувач намагається створити контент без необхідних прав доступу.<br>2. FileUploadException - помилка при завантаженні медіа-файлу (наприклад, недопустимий формат, або перевищено ліміт розміру.<br>3. ServiceUnavailableException — система тимчасово недоступна, через технічні роботи або збої.|
| **ОСНОВНИЙ СЦЕНАРІЙ** | 1. Користувач відкриває систему.<br>2. Користувач вибирає спеціальну опцію "Створити медіа-контент".<br>3.Система відображає форму для введення деталей медіа-контенту, та завантаження медіа-файлу.<br>4. Користувач заповнює необхідні поля, завантажує медіа-файл та натискає кнопку "зберегти".<br>5. Система перевіряє дані. Якщо дані коректні, система зберігає контент і повертає повідомлення про успішне стаорення. Якщо дані некоректні, система повідомляє помилки та пропонує їх виправити.<br>6. Користувач може переглядати новий контент у своєму профілі, або продовжувати створення нового контенту. |

@startuml

|Користувач|
start;
: Відкриває інформаційну систему;

: Вибирає опцію "Створити медіа-контент";

|Система|
: Відображає форму для введення деталей медіа-контенту;

|Користувач|
: Заповнює необхідні поля та завантажує медіа-файл;
: Натискає кнопку "зберегти";

|Система|
: Перевіряє дані;

note right #FF6969
<b> Можлива виключна ситуація
<b> InsufficientPermissionsException
end note

note right #FF6969
<b> Можлива виключна ситуація
<b> FileUploadException
end note

: Якщо дані коректні, зберігає контент;
: Повертає повідомлення про успішне створення;

|Користувач|
: Отримує підтвердження про успішне створення контенту;

: Може переглядати новий контент у своєму профілі;

stop;
@enduml

### Сценарій MediaEdit

| **ID**              | MediaEdit                             |
|-----------------|----------------------------------------|
| **НАЗВА**       | Редагування медіа-контенту      |
| **УЧАСНИКИ**   | Користувач, система      |
| **ПЕРЕДУМОВИ**  | 1. Користувач має доступ до інформаційної системи.<br> 2. Користувач зареєстрований в системі.<br> 3. Медіа-контент вже створений та доступний для редагування.<br> 4. Користувач має права на редагування контенту. |
| **РЕЗУЛЬТАТ**   | Медіа-контент успішно відредагований та збережений в системі. Користувач отримує підтвердження про успішне редагування контенту. |
| **ВИКЛЮЧНІ СИТУАЦІЇ** |1. InsufficientPermissionsException — користувач намагається редагувати контент без необхідних прав доступу. <br> 2. FileUploadException — помилка при завантаженні нового медіа-файлу (недопустимий формат або перевищення ліміту розміру). <br>  3. ContentNotFoundException — система не може знайти медіа-контент для редагування. <br> 4. ServiceUnavailableException — система тимчасово недоступна через технічні роботи або збої.|
| **ОСНОВНИЙ СЦЕНАРІЙ** | 1. Користувач відкриває систему.<br>2. Користувач вибирає опцію "Редагувати медіа-контент".<br> 3. Система відображає форму з поточними деталями медіа-контенту.<br> 4. Користувач вносить зміни до деталей контенту або завантажує новий медіа-файл.<br> 5. Користувач натискає кнопку "зберегти".<br> 6. Система перевіряє дані.<br> 7. Якщо дані коректні, система зберігає відредагований контент і повертає повідомлення про успішне редагування.<br> 8. Користувач отримує підтвердження та може переглядати відредагований контент.|

@startuml
|Користувач|
start;
: Відкриває систему;

: Вибирає опцію "Редагувати медіа-контент";

|Система|
: Відображає форму з поточними деталями контенту;

|Користувач|
: Вносить зміни або завантажує новий медіа-файл;
: Натискає кнопку "зберегти";

|Система|
: Перевіряє внесені зміни;

note right #FF6969
<b> Можлива виключна ситуація
<b> InsufficientPermissionsException
end note

note right #FF6969
<b> Можлива виключна ситуація
<b> FileUploadException
end note

note right #FF6969
<b> Можлива виключна ситуація
<b> ContentNotFoundException
end note

: Якщо дані коректні, зберігає зміни;

: Повертає повідомлення про успішне редагування;

|Користувач|
: Отримує підтвердження та переглядає відредагований контент;

stop;
@enduml

### Сценарій MediaFind

| **ID**              | MediaFind                              |
|-----------------|----------------------------------------|
| **НАЗВА**       | Пошук медіа-контенту користувачем      |
| **УЧАСНИКИ**   | Користувач, Система                    |
| **ПЕРЕДУМОВИ**  | 1. Користувач має доступ до інформаційної системи.<br>2. Користувач зареєстрований в системі.<br>3. База даних медіа-контенту актуальна і містить інформацію. |
| **РЕЗУЛЬТАТ**   | Користувач отримує релевантні результати медіа-контенту згідно з його запитом. |
| **ВИКЛЮЧНІ СИТУАЦІЇ** | 1. NoAccessException — користувач не може здійснити пошук через проблеми з підключенням, або проблеми на стороні сервера.<br>2. ContentNotFoundException — запит користувача не дав жодних результатів. Користувач отримує повідомлення про те, що контент не знайдено.<br>3. InsufficientPermissionException — користувач намагається отримати доступ до контенту, який потребує спеціальних прав, яких не має користувач. |
| **ОСНОВНИЙ СЦЕНАРІЙ** | 1. Користувач відкриває інформаційну систему.<br>2. Вводить ключові слова для пошуку медіа-контенту у відповідне поле.<br>3. Система обробляє запит і здійснює пошук в базі даних.<br>4. Система повертає список релевантних даних.<br>5. Користувач переглядає результати.<br>6. Користувач завершує сесію, або продовжує пошук. |
                                                                             

@startuml
|Користувач|
start;
: Відкриває інформаційну систему;

|Система|
: Показує інтерфейс пошуку;

|Користувач|
: Вводить ключові слова для пошуку;

|Система|
: Обробляє запит;
: Здійснює пошук у базі даних;

note right #FF6969
<b> Можливі виключні ситуації:
<b> NoAccessException, InsufficientPermissionException
end note

: Повертає список результатів;

|Користувач|
: Переглядає результати пошуку;

note right #FF6969
<b> Можлива виключна ситуація:
<b> ContentNotFoundException
end note

: Завершує сесію або продовжує пошук;
stop;
@enduml


### Сценарій MediaDelete
| **ID**              | MediaDelete                            |
|-----------------|----------------------------------------|
| **НАЗВА**       | Видалення медіа-контенту      |
| **УЧАСНИКИ**   | Користувач, система      |
| **ПЕРЕДУМОВИ**  | 1. Користувач має доступ до інформаційної системи. <br> 2. Користувач зареєстрований в системі. <br> 3. Медіа-контент створений і доступний для видалення. <br> 4. Користувач має права на видалення контенту. |
| **РЕЗУЛЬТАТ**   | Медіа-контент успішно видалено з системи. Користувач отримує підтвердження про видалення. |
| **ВИКЛЮЧНІ СИТУАЦІЇ** |1. InsufficientPermissionsException — користувач намагається видалити контент без необхідних прав доступу. <br> 2. ContentNotFoundException — система не може знайти контент для видалення. <br> 3. ServiceUnavailableException — система тимчасово недоступна через технічні роботи або збої.|
| **ОСНОВНИЙ СЦЕНАРІЙ** | 1. Користувач відкриває систему. <br> 2. Користувач вибирає опцію "Видалити медіа-контент". <br> 3. Система запитує підтвердження для видалення контенту. <br> 4. Користувач підтверджує видалення. <br> 5. Система видаляє контент. <br> 6. Система повертає повідомлення про успішне видалення. <br> 7. Користувач отримує підтвердження, що контент успішно видалено. |

@startuml
|Користувач|
start;
: Відкриває систему;

: Вибирає опцію "Видалити медіа-контент";

|Система|
: Запитує підтвердження видалення;

|Користувач|
: Підтверджує видалення;

|Система|
: Видаляє контент;

note right #FF6969
<b> Можлива виключна ситуація
<b> InsufficientPermissionsException
end note

note right #FF6969
<b> Можлива виключна ситуація
<b> ContentNotFoundException
end note

: Повертає повідомлення про успішне видалення;

|Користувач|
: Отримує підтвердження про видалення контенту;

stop;
@enduml

### Сценарій UserRegistration

| **ID**              | UserRegistration | 
|----------------------|-----------------| 
| **НАЗВА**            | Реєстрація користувача в системі | 
| **УЧАСНИКИ**        | Користувач, Система | 
| ПЕРЕДУМОВИ       | 1. Користувач не зареєстрований у системі.| 
|        |2. Користувач увійшов за допомогою уснуючого облікового запису. | 
| **РЕЗУЛЬТАТ**        | Створення облікового запису | 
| **ВИКЛЮЧНІ СИТУАЦІЇ**| - 1. AlreadyRegistered — користувач вже зареєстрований  2. MissingData — відсутні необхідні реєстраційні дані  3. InvalidData — некоректні дані | 
| **ОСНОВНИЙ СЦЕНАРІЙ**| 1. Користувач відкриває сторінку реєстрації     
|                      | 2. Вводить особисті дані (ім'я, прізвище, email, пароль)  | 
|                       | 3.  Система перевіряє введені дані | 
|                       |4. Створюється новий обліковий запис | 
|                       |5. Користувач отримує підтвердження про успішну реєстрацію|

@startuml

|Користувач|
start;
: Відкриває сторінку реєстрації;
: Вводить особисті дані (ім'я, прізвище, email, пароль);

|Система|
: Перевіряє введені дані;
note right #FF6969
<b> Можлива виключна ситуація
<b> MissingData
end note
note right #FF6969
<b> Можлива виключна ситуація
<b> InvalidData
end note
: Перевіряє, чи вже зареєстрований користувач;
note right #FF6969
<b> Можлива виключна ситуація
<b> AlreadyRegistered
end note
: Створює новий обліковий запис;

|Користувач|
: Отримує підтвердження про успішну реєстрацію;

stop;
@enduml

### Сценарій UserAuthorization

| ID               | UserAuthorization | 
|----------------------|-----------------| 
| **НАЗВА**           | Авторизація користувача | 
| **УЧАСНИКИ**        | Користувач, Система | 
| **ПЕРЕДУМОВИ**       |Користувач вже зареєстрований у системі|      
| **РЕЗУЛЬТАТ**        | Створення облікового запису | 
| **ВИКЛЮЧНІ СИТУАЦІЇ**|1. InvalidCredentials — введено некоректні дані | 
|                      | 2. NotRegistered — користувач не зареєстрований | 
|                      |3. TooManyAttempts — забагато спроб входу | 
| **ОСНОВНИЙ СЦЕНАРІЙ**| 1. Користувач відкриває сторінку входу |    
|                      | 2. Вводить дані для авторизації  | 
|                       |3. Система перевіряє обліковий запис  | 
|                       |4. Перевіряються дані авторизації | 
|                       |5. Користувач отримує доступ до свого облікового запису| 

@startuml

|Користувач|
start;
: Відкриває сторінку входу;

: Вводить дані для авторизації;

|Система|
: Перевіряє обліковий запис;
: Перевіряє дані авторизації;

note right #FF6969
<b> Можлива виключна ситуація
<b> InvalidCredentials
end note

note right #FF6969
<b> Можлива виключна ситуація
<b> NotRegistered
end note

note right #FF6969
<b> Можлива виключна ситуація
<b> TooManyAttempts
end note

: Якщо дані коректні, надає доступ до облікового запису;

|Користувач|
: Отримує підтвердження та доступ до свого профілю;

stop;
@enduml

### Сценарій DeleteAccount

| **ID**              | DeleteAccount                        |
|-----------------|----------------------------------------|
| **НАЗВА**       | Видалення облікового запису             |
| **УЧАСНИКИ**   | Адміністратор, Система                  |
| **ПЕРЕДУМОВИ**  | 1. Адміністратор має доступ до системи управління.<br>2. Адміністратор зареєстрований в системі.<br>3. Обліковий запис існує і доступний для видалення. |
| **РЕЗУЛЬТАТ**   | Обліковий запис успішно видалено з системи. |
| **ВИКЛЮЧНІ СИТУАЦІЇ** | 1. NoAccessException — адміністратор не може здійснити видалення через проблеми з підключенням або проблеми на стороні сервера.<br>2. AccountNotFoundException — обліковий запис не знайдено, або він вже видалений.<br>|
| **ОСНОВНИЙ СЦЕНАРІЙ** | 1. Адміністратор відкриває панель управління.<br>2. Вибирає користувача, чий обліковий запис потрібно видалити.<br>3. Система обробляє запит і видаляє обліковий запис.<br>4. Система повідомляє про успішне видалення облікового запису.<br>5. Адміністратор завершує сесію. |

@startuml
|Адміністратор|
start;
: Відкриває панель управління;

|Система|
: Показує список користувачів;

|Адміністратор|
: Вибирає обліковий запис для видалення;

note right #FF6969
<b> Можлива виключна ситуація:
<b> AccountNotFoundException
end note

|Система|
: Обробляє запит;
: Видаляє обліковий запис;

note right #FF6969
<b> Можливі виключні ситуації:
<b> NoAccessException
end note

: Повідомляє про успішне видалення;

|Адміністратор|
: Завершує сесію;
stop;
@enduml

### Сценарій PromoteUserRole

| **ID**              | PromoteUserRole                         |
|-----------------|-----------------------------------------|
| **НАЗВА**       | Підвищення ролі користувача              |
| **УЧАСНИКИ**   | Адміністратор, Система                   |
| **ПЕРЕДУМОВИ**  | 1. Адміністратор має доступ до системи управління.<br>2. Адміністратор зареєстрований в системі.<br>3. Користувач, чия роль має бути підвищена, існує в системі. |
| **РЕЗУЛЬТАТ**   | Роль користувача успішно змінена.         |
| **ВИКЛЮЧНІ СИТУАЦІЇ** | 1. NoAccessException — адміністратор не може підвищити роль через проблеми з доступом або проблеми на стороні сервера.<br>2. UserNotFoundException — користувача не знайдено.<br> |
| **ОСНОВНИЙ СЦЕНАРІЙ** | 1. Адміністратор відкриває панель управління.<br>2. Вибирає користувача, чию роль потрібно підвищити.<br>3. Адміністратор вибирає нову роль для користувача.<br>4. Система обробляє запит і змінює роль користувача.<br>5. Система повідомляє про успішне підвищення ролі.<br>6. Адміністратор завершує сесію. |

@startuml
|Адміністратор|
start;
: Відкриває панель управління;

|Система|
: Показує список користувачів;

|Адміністратор|
: Вибирає користувача для підвищення ролі;
: Вибирає нову роль;

|Система|
: Обробляє запит;
: Змінює роль користувача;

note right #FF6969
<b> Можливі виключні ситуації:
<b> NoAccessException, UserNotFoundException
end note

: Повідомляє про успішне підвищення ролі;

|Адміністратор|
: Завершує сесію;
stop;
@enduml
