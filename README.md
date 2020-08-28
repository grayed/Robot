# РОБОТ НА КЛЕТЧАТОМ ПОЛЕ СО СТОРОНАМИ ГОРИЗОНТА

Данное ПО предназначено для поддержки начального курса программирования для студентов-первокурсников, целью которого является научить писать хорошо структурированный программный код, основываясь на технологии проектирования сверху вниз, и включая использование идей обобщенного программирования. Оно помогает легко формулировать учебные задачи, сложность которых можно варьировать в достаточно широких пределах, и на которых можно научить всему заявленному, по крайней мере, в некотором приближении.

Лежащие в основе методические идеи восходят к учебнику Кушниренко А.Г., Лебедева Г.В. "Программирование для математиков", 1988.

Важно также, что для достижения поставленной цели язык программирования Julia, с его ясным и удобным синтаксисом, развитой системой типов, поддержкой основных на сегодняшний день парадигм программирования, а также с его интерактивными возможностями, является весьма подходящим. О рекомендуемых источниках информации о новом, еще только набирающем популярность, но очень интересном и перспективном языке программирования Julia см. ниже. 

Файл robot.jl содержит определение модуля HorizonSideRobot, включающего в себя определения следующих экспортируемых типов данных.

 1. @enum HorizonSide Nord=0 West=1 Sud=2 Ost=3 - перечисление, определяющее обозначения сторон горизонта (Cевер, Запад, Юг, Восток, соответственно). Фактически HorizonSide - это тип данных с возможными значениями: Nord, West, Sud, Ost
 (при этом HorizonSide(0) - означает Nord, HorizonSide(1) - означает West и т.д.)    

 2. Тип Robot с экспортируемыми функциями: 

        Robot(...) - имеются разные варианты вызова этого конструктора, см. ниже

        move!(::Robot, ::HorizonSide)::Nothing
        
        isborder(::Robot,::HorizonSide)::Bool
        
        putmarker!(::Robot)::Nothing
        
        ismarker(::Robot)::Bool
        
        temperature(::Robot)::Int
        
        show(::Robor; edit=false | true)::Nothing
        
        save(::Robot, ::AbstractString)
    
     Этот пользовательский тип предназначен для создания (с помощью Robot(...)) объектов - "роботов на клетчатом поле со сторонами горизонта". Каждый такой объект позволяет имитировать программное управление роботом, движущимся по клетчатому полю с заданной обстановкой, и выполняющим на нем те или иные действия.    

В дальнейшем, уже для работы с "роботом", импортировать модуль HorizonSideRobot не придется, т.к. соответствующий оператор импортирования уже содержится в конце файла robot.jl (достаточно будет простого включения содержимого файла с помощью include, см. ниже).

Обстановка на поле определяется наличием или отсутствием ограничивающей поле прямоугольной рамки (поле может быть ограниченным или неограниченным), наличием или отсутствием внутренних (межклеточных) перегородок, наличием или отсутствием маркеров в клетках поля, положением робота на поле. Робот не может убирать имеющиеся перегородки или устанавливать новые, но может проверять их наличие, он может делать шаг ровно на 1 клетку в кажом из 4-х направлений горизонта (наличие на пути перегородки приведет к фатальной ошибке), может устанавливать маркер в клетке поля, в которой робот находится в данный момент, проверять наличие маркера в этой клетке, но убрать ранее поставленный маркер он не может. Также робот умеет измерять температуру в той клетке поля, в которой находится.

## Подготовка к работе с модулем

1. Работа данного модуля требует, чтобы кроме Julia на комьютере был установлен интерпретатор Python 3 c библиотекой matplotlib. Для этого проще всего воспользоваться дистрибутивом Anaconda, и тогда все это будет гарантированно установлено (инструкцию по установке см., например, здесь https://pythonru.com/baza-znanij/kak-ustanovit-anaconda-na-windows). Саму Julia желательно установить уже после установки Anaconda (тогда все необходимые действия по интеграции установщик Julia выполнит автоматически). Скачать ее надо с сайта разработчиков https://julialang.org/downloads (если потребуется, то инструкцию по установке см., например, здесь https://techytok.com/julia-vscode/ - при необходимости можно воспользоваться goole-переводчиком).

2. Желательно также установить популярный редактор программного кода VS Code с расширением Julia (по-видимому, на сегодняшний день это наиболее удобный выриант для программирования на языке Julia, по крайней мере для начинающих). Также надо будет позаботиться, чтобы путь к папке с julia.exe был известен VS Code (инструкцию о том, как этого добиться, можно найти, например, здесь https://techytok.com/julia-vscode/ - при необходимости можно воспользоваться goole-переводчиком).

После установки VS Code с расширением для Julia для запуска REPL (интерактивного окна) Julia потребуется из терминала, имеющегося в VS, выполнить команду: 

    cmd> julia

3. После этого, уже из REPL Julia, необходимо будет скачать с GitHab пакет PyPlot.jl.   

Делается это с помощью встроенного пакетного менеджера очень просто. Сначала надо из стандартного режима REPL перейти в режим пакетного менеджера, набрав 

    julia>]+Enter

После чего выполнить скачивание: 

    pkg> add PyPlot.jl+Enter

Процесс скачивания займет некоторое время. 
После его завершения следует вернуться в стандартный режим REPL нажатием клавиши Backspace ("забой"). 

4. Наконец, требуется скачать файл "robot.jl" из репозитория GitHub.

Для этого необходимо, используя веб-браузер, зайти по адресу https://github.com/Vibof/Robot в соответствующий репозиторий GitHub (читающий этот текст, скорее всего, это уже сделал) и скачать его содержимое на свой компьютер.  Для этого можно на странице репозитория кликнуть мышью по кнопке Code (она находится над перечнем файлов справа и выделена зеленым цветом) и в открывшемся списке выбрать "Download Zip". 
   
Затем полученный таким образом файл robot.jl (после разархивирования всего содержимого репозитория) надо поместить в заранее созданную на компьютере специальную рабочую папку, которую требуется сделать текущей (просто открыв ее в VS Code), и в которой впоследствии следует сохранять все свои файлы и папки с программным кодом и другие файлы (например, файлы с обстановкой на поле). 

## Использование файла robot.jl
    
Фактически этот файл содержит определение перечисления HorizonSide (определяющего имена сторон горизонта) и определение пользовательского типа Robot и связанных с ним функций.

Для начала работы с "роботом" требуется только выполнить включение содержимого файла "robot.jl" в область REPL (интерактивное коммандное окно Julia) VS Code

    julia> include("robot.jl")

(делать это включение будет необходимо повторно в начале каждого нового сеанса работы с Julia).

После этого включения имена HorizonSide, Nord, West, Sud, Ost, Robot, move!, isborder, putmarker!, ismarker, temperature, show, save, а так же - sitedit, sitcreate окажутся доступными в соответствующем пространстве имен. 
В результате, в частности, станет возможным получить более детальную информацию о каждом из них с помощью встроенной системы помощи (help). 

Чтобы из стандартного режима REPL перейти в режим help, требуется набрать 

    julia> ?+Enter

После этого можно будет, например, набрать 

    help?> Robot+Enter

и получить интересующую информацию (возврат в стандартный режим REPL так же осуществляется с помощью клавиши Backspace). 

Стоит иметь в виду, что, поскольку функция include фактически осуществляет вставку (как бы) соответствующего программного кода из указанного файла в текущую позицию, то после выполнения include("robot.jl") более одного раза в последующем может возникнуть не нужная поблема (связанная с дублированием некоторых определений).

## Создание объекта типа Robot с той или иной начальной обстановкой на поле

Предусмотрены следующие варианты вызова конструктора Robot.

1. r=Robot()

2. r=Robot(<число_строк_клеток_поля>,<число_столбцов_клеток_поля>)

3. r=Robot(<имя_файла_с_oбстановкой>)

4. r=Robot(...; animate=true)

В первом случае создается ограниченное рамкой поле 11x12 без внутренних перегородок и маркеров в клетках, с положением робота в юго-восточном (нижнем левом) углу.

Во втором - поле будет иметь указанные размеры.

В третьем - обстановка на поле будет соответствовать данным, загруженным из соответствующего файла; рекомендуется для таких файлов использовать расширение .sit (от слова situation).

Во всех 3-х случаях создается объект r типа Robot (точнее говоря, r - это ссылка на объект), внутри себя содержащий структуру данных с соответствующей обстановкой. При выполнении команд робота эта внутренняя структура данных может изменяться, но визуализации обстановки на поле при этом еще не будет.

При желании посмотреть текущую обстановку следует воспользоваться функцией show(r).

В результате ее выполнения будет открыто графическое окно с текущей обстановкой.

При необходимости отредактировать текущуюю обстановку с помощью мыши, функцию show следует вызывать с ключевым словом edit при значении true: show(r; edit=true).

При желании сохранить текущую обстановку в файле следует воспользоваться функцией 
save(r, <имя_файла>).

Кроме этого, еще имеется возможность запустить робота с анимацией выполняемых им действий, указав значение true ключевого слово animate:  

    julia> r=Robot(...; animate=true)

где вместо многоточия должны быть указаны фактические параметры (или они могут отсутствовать) в соответствии с первыми 3-мя вариантами использования конструктора. 

При этом функцию show использовать будет уже нельзя (это привело бы к ошибке времени выполнения). Но все функции графического окна, открывавшегося прежде при вызове функции show, будут присущи аналогичному графическому окну, открывающемуся теперь автоматически при вызове конструктора. 

С помощью этого окна можно отслеживать все действия "робота" в режиме анимации. Однако этот режим приводит к дополнительным временным задержкам, не желательным, может быть, при выполненнии "роботом" объемных действий.

## Пример программы, управляющей "роботом" 

Пусть требуется решить следующую задачу.

ДАНО: Робот находится в произвольной клетке ограниченного прямоугольного поля без внутренних перегородок и маркеров

РЕЗУЛЬТАТ: Робот - в исходном положении в центре прямого креста из маркеров, расставленных вплоть до внешней рамки

При правильном подходе к программированию целью должно быть не просто написать код, который хоть как-нибудь даст решение данной конкретной задачи, но написать его так, чтобы он был при этом еще и ХОРОШО СТPУКТУРИРОВАН. Это особенно важно, если предполагается дальнейшая модернизация кода и/или работа в команде программистов. Ну и просто - если иметь целью научится  с минимальными усилиями писать нажежно работающие программы, в которых может разобраться кто-нибудь еще кроме их автора, и то лишь в момент разработки. Тут еще надо иметь в виду тот общеизвестный факт, что, уже спустя довольно короткий интервал времени, свой собственный код начинает восприниматься как чужой. Но чтобы этому научиться, требуется приложить определенные усилия, и, часто, - перебороть в себе, в общем, вполне естественное желание в творческом порыве, никого не слушая, все делать по-своему.

Вот код на языке Julia, который достигает этой цели. Он состоит из главной функции и 3-х вспомогательных (подпрограмм).

    function mark_kross!(r::Robot) # - главная функция  
        for side in (HorizonSide(i) for i=0:3) # - перебор всех возможных направлений
            putmarkers!(r,side)
            move_by_markers(r,inverse(side))
        end
        putmarker!(r)
    end

    putmarkers!(r::Robot,side::HorizonSide) = while isborder(r,side)==false 
        move!(r,side)
        putmarker!(r)
    end

    move_by_markers(r::Robot,side::HorizonSide) = while ismarker(r)==true 
        move!(r,side) 
    end

    inverse(side::HorizonSide) = HorizonSide(mod(Int(side)+2, 4)) 

По-видимому, необходимо сразу дать пояснение, почему имена некоторых функций завершаются символом "!". Дело в том, что в языке Julia действует неформальное  
соглашение, согласно которому все функции, изменяющие (потенциально способные изменять) данные, содержащиеся в своих аргументах, получаемых по сслылке, следует именовать с восклицательным знаком в конце.

Хотя определения функций и могут быть сделаны непосредственно в REPL, обычно все нужные определения, решающие задачу, размещают и хранят в отдельном файле (или - в нескольких файлах). Пусть в нашем случае такой файл будет иметь имя example.jl. 

Тогда, чтобы выполнить функцию mark_kross! из REPL нужно будет опредения этой и вспомогательных для нее функций вставить в REPL с помощью include("example.jl"). Вот вся последовательность необходимых дейсвий.

    julia> include("robot.jl")

    julia> include("example.jl")

    lulia> r=Robot(animate=true) # возможно редактирование обстановки 

    julia> mark_kross!(r)

Разумеется, после внесения каких-либо изменений в файл example.jl, перед повторным вызовом mark_kross!(r) необходимо будет заново выполнить include("example.jl"). Причем, поскольку файл "example.jl" содержит лишь определения функций вне отдельного модуля (в языке Julia есть специальная синтаксичская конструкция - module, ограничивающая область видимости имен, и которая в данном случае не используется), то перезапуск REPL julia не потребуется.

Приведенное выше решение не является единственно возможным. Так, функцию putmarkers! уместно было бы реализовать еще и так:

    putmarkers!(r::Robot, side::HorizonSide) = while move_if_possible!(r, side) == true
        putmarker!(r)
    end

    move_if_possible!(r::Robot, side::HorizonSide)::Bool = if isborder(r, side)
        return false
    else 
        move!(r,side)
        return true
    end

Если иметь в виду, что в дальнейшем может понадобиться усовершенствовать функцию mark_kross! так, что бы она работала и при наличии на поле внутренних перегородок, то эта реализация, возможно, будет даже более предпочтительной (при необходимости это позволит ограничиться модернизацией только функции move_if_posible, оставив весь остальной код без изменений).

ЗАМЕЧАНИЕ. Во всех приведенных примерах функций типы их аргументы были аннотированы. Однако в языке Julia такая аннотация не является обязательной, т.е. можно было бы и не указывать типы аргументов функций, так же как это, например, имеет место в языке Python. Поэтому, если во всех приведенных примерах функций, убрать аннотацию типов аргументов, то по-прежнему все будет работать. 

Возможность аннотирования типов, в принципе, дает следующие преимущества.

- Функции с одним и темже именем, но с разными типами аргументов (и/или разным количеством аргументов) - это разные функции (в терминологии Julia - разные методы одной функции). Это то, что называется множественной диспетчерезацией (см., например, https://ru.wikipedia.org/wiki/%D0%9C%D1%83%D0%BB%D1%8C%D1%82%D0%B8%D0%BC%D0%B5%D1%82%D0%BE%D0%B4). Множественная диспетчеризация позволяет иметь специализированные вырианты (методы) функции для каждого набора типов аргументов (т.е. выполнять те или иные действия, в зависимости от типов).
- Заголовок определения функции с аннотированными типами аргументов делает более понятным их назначение (но отсутствие аннотирования всегда можно компенсировать в этом смысле подробным документирванием функции).
- Аннотирование типов аргументов предохраняет от потенциальных ошибок использования функции, причем такого рода ошибки могут быть обнаружены анализатором кода еще на этапе его редактирования (но отсутствие аннотирования аргументов функции компенсируется в этом смысле тем, что ошибки типизаци обычно легко выявляются при отладке программы).
  
Вообще, типы бывают абстрактными и конкретными. Все типы аргументов в приведенных примерах - конкретные. Абстрактные типы предназначены исключительно для создания иерархии типов путем использования механизма наследования. Использование же конкретных типов при аннотировании аргументов - не всегда правильное решение, потому что это перпятствует обобщенному подходу к программированию, основанному на так называемых обобщенных функциях. Цель разработки обобщенной функции - сделать сферу ее применения максимально широкой без изменения кода реализации. 

Однако то, в каких именно случаях, и как лучше всего аннотировать аргументы функций, и как программировать обобщенно - это предмет отдельного обсуждения. В приведенном примере эта тема полностью проигнорирована.

## О возможных интерпретациях заданий на программирование "робота"

Во-первых, можно представлять себе, что "робот" - это реальное физическое устройство (автомат), по какому-либо каналу связи соединенному с компьютером, и допускающее программное управление посредством предоставляемого им программного (командного) интерфейса. В этом случае надо считать, что конструктор Robot создает в памяти компьютера некоторый объект (и возвращает на него ссылку), обеспечивающий указанный интерфейс. И тогда главная функция, которую требуется написать, согласно тому или иному заданию, должна будет получать указатель на этот объект через соответствующий входной параметр. Можно считать, что именно так и было в приведенном выше примере.

Во-вторых, можно считать, что главная функция (программа) на вход получает не ссылку на объект типа Robot, а считывает файл, содержащий информацию об обстановке на клетчатом поле (включающую расположение перегородок, маркеров, самого робота и массив "температур" клеток поля). А результатом работы программы должна быть некоторая информация, извлеченная из исходного файла с обстановкой и записываемая в некотором формате в выходной файл, или выходной файл с новой обстановкой. В этом случае "робот", которого тогда надо еще создавать в этой программе, будет выступать просто как виртуальный исполнитель, "умеющий" извлекать информацию из файла с обстановкой, и записывать в другой файл в нужном формате итоговую информацию. Для этого, если речь идет о выходном файле, содержащем итоговую обстановку, и предусмотрена специальная функция save(r::Robot, <имя_файда>::AbstractString). 

Тут имеется в виду, что при выполнении конструктора

    r=Robot(<имя_файла_с_исходной_обстановкой>)

происходит копирование данных из файла во внутренний буфер, содержащийся в объекте r, и при выполнении различных действий "робота" каждый раз происходит с обновление содержания этого буфера. При желании сохранить текущее содержание буфера в соответствующем файле следует использовать функцию save.

При этом считается, что программисту не известен формат данных, хранящихся в файлах с обстановкой (для работы с этими данными как раз и предназначен "робот").

Ясно, что если интерпретировать "робота" просто как обработчика и формирователя файлов специального формата, то главная функция всегда может быть просто оберткой вокруг функции, получающей на вход непосредственно объект типа Robot (как в первом случае).

## Утилиты для предварительной подготовки файлов с обстановкой на поле

Для предварительной подготовки файла с какой-либо исходной обстановкой имеется отдельная специльная функция sitcreate, а для визуального редактирования файла с уже имеющейся обстановкой - sitedit. Использование этих функций также не предполагает знания внутреннего формата данных, все делается с помощью мыши.

Создать файл с исходной обстановкой можно также и непосредственно с помощью конструктора Robot (без использования sitcreate), если только использовать его в режиме анимации: 

    Robot(animate=true)

При этом целевой файл всегда создается автоматически и будет иметь имя "untitled.sit" (впоследствии его, конечно, можно будет переименовать).

Именам файлов с обстановкой рекомендуется давать расширение .sit (от слова situation).

## О рекомендуемых источниках информации о языке Julia

Для изучения основ языка Julia (julialang) может быть рекомендован интернет-ресурс https://techytok.com/from-zero-to-julia/ (это англоязычный ресур, но с использованием google-переводчика им вполне могут пользоваться даже не владеющие английским языком). 

Дополнительно полезную информацию в виде статей о julialang на русском языке можно найти, на https://habr.com/ru/hub/julia/. 

Разумеется, имеется фирменная документация https://docs.julialang.org/en/v1/ вполне хорошего качества. 

Имеется перевод на русский язык книги Малкольма Шеррингтона "Осваиваем язык Julia". На английском языке Julia посвящены уже десятки книг, как вводного характера, так и для специалистов; эту информацию можно найти на официальном сайте https://julialang.org/learning/. 

В интернете также можно найти и некоторое количество источников информации о языке Julia, на русском языке. Например, http://cmp.phys.msu.ru/ru/staff/antonyuk, http://www.lib.unn.ru/students/src/JULIA_tutorial.pdf (это пособие содержит также уже некоторые устаревшие сведения, т.к. язык бурно развивался в последующие годы), https://habr.com/ru/post/423811/, https://biologo.ru/korotko-o-yazike-programmirovaniya/index.pdf (некоторые содержащиеся в этом источнике свединия также могут оказаться уже устаревшими). Еще имеется https://github.com/YermolenkoIgor/Julia_tutorial_rus.

Важно отметить некоторое (поверхностное) сходство языка Julia с языком Python, благодаря чему, например, часто удается выяснить имя и описание какой-либо вдруг понадобившейся стандартной функции Julia (если только это не удалось сделать напрямую): велика вероятность того, что нужная функция называется и действует также как и в Python. Однако, как можно понять из вышесказанного, на сегодняшнй день инфрормацию на русском языке по языку Python найти значительно проще. 
