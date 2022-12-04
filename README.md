# Ожидаемый эффект

При работе в IDE QtCreator, по нажатию клавиши F1 происходит анализ текущего слова, на котором стоит курсор. В рамках анализа необходимо искать в слове заданные пользователем строки. Для каждой такой строки есть небольшое текстовое описание и ссылка на статью из базы знаний проекта (для примера и тестирования возьмем https://www.jetbrains.com/ru-ru/youtrack/). Если в слове встретилось несколько таких строк, то в одном окне выводится несколько форм подряд, в том порядке, в котором слова встретились.

# Как получить слово из файла

В QtCreator есть раздел настроек ExternalTools. Наша програмулина будет лежать там, это позволит использовать внутренний контекст IDE. В рамках нашей задачи, на вход програмулине будет поступать 3 аргумента - путь до файла, номер строки, номер колонки. Все эти 3 аргумента можно выцепить, используя следующую картинку
![image](https://user-images.githubusercontent.com/71624171/205485019-0601ce21-072d-480d-b483-6f15ced174ff.png)

Поле Arguments это как раз аргументы, которые пойдут на вход програмулине.
Соответственно внутри програмулины необходимо найти нужный файл, нужную строку и колонку и получить слово, двигаясь влево и вправо от заданной колонки, в поисках символов, которыми нельзя называть переменные и классы (пробелы, знаки препинания и тд). Например, рассмотрим ситуацию
![image](https://user-images.githubusercontent.com/71624171/205485315-669acae1-9ce0-4668-b692-904636d878cf.png)

Видно что курсор стоит на 16 строке и 16 колонке. Ожидаем, что программа сможет вычленить отсюда "ClangFormatExecutor". Двигаясь влево от курсора, мы уткнемся в пробел, а двигаясь вправо от курсора мы уткнемся в ';', Вот у нас и получилось слово

# Как с этим работать пользователю

Пользователю должно быть удобно создавать новые слова и соответственно UI форму к ним. Также должен быть файл настроек с именем ".redkit-concepts-doc". Этот файл с настройками будет располагаться в корне репозитория проекта пользователя. Программа должна найти этот файл, рекурсивно поднимаясь вверх по директориям, начиная от директории, в которой лежит файл (с которым работал пользователь, когда нажал F1). В файле настроек будет информация о директориях, в которых искать инфу о том, какие слова будут в списке, который необходимо анализировать.

# Зачем это вообще?

В больших проектах много различных имен сущностей, причем как со стороны предметной области, так и со стороны архитектурного смысла внутри программы, который мы вкладываем. Например IEC104ServerBindingTreeModel. Здесь можно выделить сущности IEC104 (протокол передачи данных), Server(термин из модели клиент-сервер, выступает источником каких-то данных), Binding (привязка), TreeModel (древовидная модель). Если бы в документации пользователя были бы описаны все эти сущности, то по F1 на IEC104ServerBindingTreeModel мы бы увидели 4 мощных обьяснения. Таких отдельных сущностей и терминов очень много. Это повысит общее понимание проекта для всех членов команды, позволит новым сотрудникам быстрее вливаться в понимание происходящего, повысит информативность кода.

# В будущем возможно добавиться, сейчас нет

Конфигурационные файлы .redkit-concepts-doc могут лежать и в промежуточных директориях, например в конкретной папке с исходниками, или в папке библиотеки. Причем этот файл будет содержать какие-то свои настройки и будет промаркирован, что он промежуточный, чтобы програмулина искала дальше корневой файл. Среди настроек этого промежуточного файла настроек может быть указание показать пользователю дополнительную какую-то документацию, которая имеет отношение к текущие директории. И програмулина, при открытии, будет содержать опцию, что здесь есть еще дополнительная документация.


