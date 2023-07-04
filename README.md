# Telegram бот для планирования - принимает от пользователя сообщения в формате "01.01.2022 20:00 Сделать домашнюю работу" и присылает пользователю сообщение в 20:00 1 января 2022 года с текстом “Сделать домашнюю работу”
## 1. Обработать команду /start и выводить приветственное сообщение
В методе `public int process(List<Update> updates)` обработать команду `/start`, отправляемую пользователем нашему боту в Telegram.
Для этого необходимо проитерироваться по всем апдейтам, которые наш метод принимает в качестве аргумента и, в случае, если мы получили сообщение `/start` — вывести приветственное сообщение. 
## 2. Создать использую liquibase таблицу notification_task
Теперь настало самое время понять, как мы будем хранить наши таски в базе данных и спроектировать для этого схему базы данных. Предположу, что нам потребуется иметь первичный ключ в таблице, идентификатор чата, 
в который нужно отправить уведомление, текст уведомления и дату+время, когда требуется отправить уведомление. Возможно, вы захотите хранить какие-то дополнительные данные. Примерно такой структуры нужно создать таблицу использую liquibase. 
## 3. Создать сущность notification task
После того, как мы создали таблицу, необходимо создать сущность, с которой мы будем работать в коде нашего приложения.
## 4. Создать репозиторий для сущности notification task
Создать расширение JpaRepository<T, ID>, который был бы пригоден для работу с сущностью, созданной в предыдущем шаге.
## 5. Научиться парсить сообщения с создаваемым напоминанием и сохранять их в БД
Обрабатать получаемое от пользователя сообщения вида `01.01.2022 20:00 Сделать домашнюю работу` , вычленять из него дату+время (01.01.2022 20:00) и текст напоминания (Сделать домашнюю работу), создавать на основе этих данных сущность
и сохранять её в БД.
Для распознавания в строке подстроки с датой и подстроки с сообщением нам потребуются регулярные выражения.
 После того, как мы отделили дату и текст напоминания во входящем сообщении, нам необходимо сконструировать сущность.
## 6. Научиться по шедулеру раз в минуту выбирать записи из БД, для которых должны быть отправлены нотификацииИмея сконструированную сущность необходимо сохранить её использую методы репозитория.
Шедулинг — механизм выполнения некоторых методов по расписанию, подробнее об этом в методичке.
Необходимо выполнять раз в минуту метод, который ищет записи в БД с датой выполнения — текущая минута.
Написать в репозитории метод, который ищет записи, у которых время совпадает с текущим.
## 7. Научиться для выбранных записей рассылать уведомления
Для полученной коллекции записей из прошлой задачи необходимо рассылать уведомления в нужные чаты.