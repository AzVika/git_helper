# Git Helper

## Графічні інтерфейси

| Інструмент |	Визначення |
| ------ | ------ |
| git-gui |	Для використання достатньо запустити з основного каталогу репозиторію, набравши git gui або git-gui. Інструмент дозволяє переглядати історії, вносити зміни, комміти, працювати з віддаленими репозиторіями, порівнювати гілки тощо. |
| gitk |	Цей браузер репозиторію може бути викликаний з git-gui і є дещо старішим інтерфейсом, більш призначеним для перегляду історії проєкту. |
| cgit |	Може бути встановлений разом з вебсервером для забезпечення дуже ефективного перегляду репозиторію. Для ознайомлення з його можливостями спробуйте https://www.kernel.org. |
| gitweb |	Старіший інтерфейс, ніж cgit, але працює за схожим принципом. |


## Документація

основні команди:
```sh
$ git help
```
Детальну довідку про будь-яку команду:
```sh
$ git help <command>
```
Переглянути повніший набір команд:
```sh
git help --all
```

Офіційний путівник "Git User's Manual" https://mirrors.edge.kernel.org/pub/software/scm/git/docs/user-manual.html

Якщо у вас встановлений Git, ви можете побачити, яку версію ви використовуєте:
```sh
$ git --version 
```


## Початок роботи з Git
Якщо ви створюєте новий репозиторій, найпростіший спосіб зробити це з самого початку - зробити щось на кшталт:
```sh
$ git init
$ git checkout -b main
```

Для вже наявних репозиторіїв ви можете перейменувати як локальну гілку, так і віддалену гілку на сервері з:
```sh
$ git checkout master
# Змінити локальну назву
$ git branch -m master main

# Змінити віддалену назву
$ git push -u origin main
$ git symbolic-ref refs/remotes/origin/HEAD refs/remotes/origin/main

# Підтвердити імена!
$ git branch -a
```
Залежно від налаштувань віддаленого сервера (наприклад, GitHub) це може спрацювати, а може і не спрацювати. Простіший підхід - не видаляти гілку master, а просто скопіювати її в main і працювати звідти, як і раніше:
```sh
$ git checkout master
$ git branch main
$ git checkout main
$ git push -u origin main
```
а потім просто ігноруйте гілку master надалі. 

### Простий приклад
Спочатку ми створимо робочий каталог, а потім ініціалізуємо Git для роботи з ним:
```sh
$ mkdir git-test
$ cd git-test
$ git init
```
Під час ініціалізації проєкту створюється каталог .git, який міститиме всю інформацію про керування версіями. Основні каталоги, що входять до складу проєкту, залишаються недоторканими. 
Далі ми створюємо файл і додаємо його до проєкту:
```sh
$ echo some junk > somejunkfile
$ git add somejunkfile
```
Ми можемо бачити поточний стан нашого проєкту за допомогою:
```sh
$ git status
```
Давайте скажемо Git'у, хто відповідає за цей репозиторій:
```sh
$ git config user.name "Another Genius"
$ git config user.email "a_genius@linux.com"
```
Це потрібно робити для кожного нового проєкту, якщо тільки він не визначений у глобальному файлі конфігурації.

Тепер давайте змінимо файл, а потім подивимося історію відмінностей:
```sh
$ echo another line >> somejunkfile
$ git diff
```
Для того, щоб фактично закоммітити зміни в репозиторії, ми це робимо: 
```sh
$ git add somejunkfile
$ git commit -m "My initial commit"
```
Ви можете переглянути свою історію за допомогою:
```sh
$ git log
```
Підпис коммітів

Важливо знати, хто відповідає за всі зміни в репозиторії, щоб відстежувати історію і розуміти, хто гарантує, що код, який вноситься до репозиторію, робиться з належним ліцензуванням і правом власності.

Найпростіше це зробити, додавши до комміту рядок Signed-off-by з опцією -s. Так ми могли б зробити:
```sh
$ git commit -s -m "My initial commit"
```

## Керування файлами та змістом

Кожен каталог може містити файл з назвою .gitignore Наприклад, зміст файлу: не відстежити усі файли з типом .ko, крім файла my_driver.ko, буде виглядати так:
```sh
*.ko
!my_driver.ko
```
Додає файл до індексації:
```sh
$ git add myfile
```
Видаляє файл з індексу та рабочого дерева:
```sh
$ git rm
```
Видаляє файл, який було стадійовано, але не закоммітено:
```sh
$ git rm myfile --cached
```
Перейменування файлів:
```sh
$ git mv oldfile newfile
```
Перейменування файлів (суть така сама):
```sh
$ git mv oldfile newfile ; git rm oldfile ; git add newfile
```
Показує інформацію про файли в змістовому (індексному) та робочому дереві:
```sh
git ls-files
```
Показує інформацію про невідстежувані файли, де параметр --otheres показує невідстежувані файли, а параметр --exclude-standard вказує ігнорувати стандартні винятки, такі як файли .gitignore.:
```sh
$ git ls-files --others --exclude-standard
```

## Робимо комміти та теги

Зробить комміт одного файла:
```sh
$ git commit -s file1
```

Якщо ви хочете закоммітити всі зміни, скористайтеся будь-якою з цих форм:
```sh
$ git commit -s         // або
$ git commit ./ -s      // або
$ git commit -a -s
```
Покаже всі відмінності між вашими робочими каталогами і тим, що було закоммітино раніше. Після того, як ви виконаєте комміт, він не покаже жодних відмінностей:
```sh
$ git diff
```

Подивитися 10 останніх коммітів у скороченному вигляді:
```sh
$ git log | grep "^commit" | head -10
```
Стровити тег до комміту 08d869:
```sh
$ git tag ver_10 08d869
```
Повернутися до точки розробки, позначеної ver_10:
```sh
$ git checkout ver_10
```
Комміти показані скорочено у зворотному порядку введення:
```sh
$ git log --pretty=oneline
```

## Скасування (reverting) та скидання (resetting) коммітів

Час від часу ви будете усвідомлювати, що здійснили зміни не дуже правильно. Можливо, ви внесли зміни, які не повинні були робити, або взяли на себе зобов'язання передчасно.

Ви можете відмовитися від певного зобов'язання: 
```sh
$ git revert commit_name
```
де `commit_name` може бути вказано різними способами і не обов'язково має бути найсвіжішим.

Комміти можна розмежовувати за допомогою:

- HEAD: найсвіжіший комміт
- HEAD~: попередній комміт (батько HEAD)
- HEAD~~ або
- HEAD~2: дідусь або бабуся HEAD
- {hash number}: конкретний комміт за повним або частковим sha1-хеш-номером 
- {tag name}: назва для комміта

Зауважте, що `git revert` вносить до нового комміту набір змін, тобто відмінений патч як додатковий комміт. Це доречно робити, якщо хтось інший завантажив дерево, що містить зміни, які було відкореговано. Це змінить вашу робочу копію вихідних файлів.

Якщо коротко, `git revert` збирає і додає новий об'єкт комміту, встановлює йому HEAD і оновлює робочий каталог.

У випадку, коли ви єдиний, хто бачив оновлений репозиторій, краще скористатися командою `git reset`. Наприклад, якщо ви хочете відкликати останні два комміти, ви можете зробити так:
```sh
$ git reset HEAD~2
```

Це не змінить вашу робочу копію вихідних файлів. Вона просто змінить місцями комміти і зробить так, щоб зміст (індекс) відповідав вказаному комміту.

При використанні будь-якої з опції `--soft`, `--mixed`, `--hard`, `--merge` або `--keep`, поведінка відрізняється. У цьому випадку посилання HEAD у поточній гілці також буде встановлено на вказаний комміт, додатково змінюючи зміст (індекс) та вихідні файли відповідно:

- `--soft` просто переміщує поточну гілку до об'єкта попереднього комміту (зміст (індекс) у цьому випадку не змінюється)
- `--mixed` (за замовчуванням): також оновлює  зміст (індекс) відповідно до нового заголовка (все знімається зі стадіювання) 
- `--hard` те саме, що й --mixed, але також оновлює робочий каталог відповідно до нового заголовка (не редагує ваші файли).

Припустимо, що ви несамовито кодуєте і розумієте, що ваші останні три комміти все ще в роботі, але все, що було до цього, має бути доступним для інших. Тоді вам слід створити нову робочу гілку work, відкинувши гілку main на три комміти назад:
```sh
$ git branch work
$ git reset --hard HEAD~3
$ git checkout work
```
яка відновлює гілку `main` до попереднього стану, залишаючи спекулятивну роботу в `work`, де ви продовжите гратись.


## «Прибирання» в репозиторії

Коли ваш проєкт зростає через серію коммітів, ваш репозиторій може збільшуватися у розмірі. Ви можете оптимізувати і стиснути репозиторій, скориставшись `git gc`: 
```sh
$ du -shc .git
$ git gc
$ du -shc .git
```
де `gc` означає «збір сміття». 

Ви також можете перевірити репозиторій на наявність певних типів помилок за допомогою команди `git fsck`. Найбільш ймовірними і нешкідливими помилками, які вона знайде, будуть об'єкти, що бовтаються; хоча вони іноді бувають корисними для відновлення пошкоджених репозиторіїв, їх зазвичай можна безпечно видалити за допомогою `git prune`, як це зроблено тут:
```sh
$ git prune -n
$ git prune
```
де перша команда просто перевіряє, що буде зроблено, і якщо вас це влаштовує, ви можете віддати другу команду очищення.
Можна призначити відповідальних за певний набір рядків у файлі:
```sh
$ git blame file2
```

## Поділ навпіл (бісекція)
Припустимо, у вас була версія коду, яка працювала, а тепер, через багато версій (і коммітів), ви виявили, що вона більше не працює.

Git має можливість поділити навпіл (бісектувати), щоб швидко знайти набір змін, який все зіпсував. Кількість кроків не перевищує логарифм за основою 2 від кількості коммітів, що набагато швидше, ніж при звичайному переборі. Іншими словами, якщо погана зміна була зроблена десь в останніх 1024 коммітах, ви можете знайти її не більше ніж за 10 кроків поділу навпіл (бісекції).

Спочатку вам потрібно зробити:
```sh
$ git bisect start
$ git bisect bad
$ git bisect good V_10
```
де передбачається, що поточний комміт поганий, а версія `V_10` відома як хороша. Після цього Git залишить вас на комміті на півдорозі між цими двома версіями. Після цього ви тестуєте код, щоб перевірити, чи помилка все ще там.
Якщо так, ви вводите код:
```sh
$ git bisect bad
```
Якщо в коді ще немає помилки, ви вводите:
```sh
$ git bisect good
```
Ви продовжуєте це ітеративно, поки не знайдете баг.
Потім ви вводите:
```sh
$ git bisect reset
```
щоб повернутися до поточного робочого стану.

Перегляньте лог:
```sh
$ git bisect log
```

## Гілки

Основною командою для створення нової гілки є:
```sh
$ git branch [branch_name] [starting_point]
```

Точкою відліку може бути будь-який комміт, якщо є тег, який його описує, ви можете використати його замість довгого рядка. Якщо ви не вкажете аргумент, буде створено копію активної гілки з моменту її останнього комміту. Отже, ви можете зробити так:
```sh
$ git branch devel
```
Ви можете видалити гілку devel за допомогою:
```sh
$ git branch -d devel
```
яка не може бути поточною робочою гілкою.
Отримати список гілок:
```sh
$ git branch
```
Дуже детальну історію гілок можна отримати за допомогою:
```sh
$ git show-branch
```
У процесі переходу (checkout) ви можете переключитися на іншу гілку:
```sh
$ git checkout devel
```
Також можна об'єднати операції створення нової гілки і її вилучення за допомогою опції -b до операції вилучення (checkout):
```sh
$ git checkout -b newbranch startpoint
```
повністю еквівалентне: 
```sh
$ git branch newbranch startpoint
$ git checkout newbranch
```

Припустимо, ви хочете побачити попередню версію певного файлу. Ви можете зробити це за допомогою git show, якщо вкажете ім'я шляху на додачу до тегу як ось тут (зверніть увагу на двокрапку): 
```sh
$ git show v2.4.1:src/myfile.c
```
Якщо ви дійсно хочете відновити цю версію, ви можете зробити це за допомогою:
```sh
$ git checkout v2.4.1 src/myfile.c
```
де немає двокрапки.
