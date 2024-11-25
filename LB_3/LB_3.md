## Комп'ютерні системи імітаційного моделювання
## СПм-23-3, **Сергеєв Данило Володимирович**
### Лабораторна робота №**3**. Використання засобів обчислювального интелекту для оптимізації імітаційних моделей

<br>

### Варіант 5, модель у середовищі NetLogo:
[Fire Simple Extension 2](http://www.netlogoweb.org/launch#http://www.netlogoweb.org/assets/modelslib/IABM%20Textbook/chapter%203/Fire%20Extensions/Fire%20Simple%20Extension%202.nlogo)

<br>

### Вербальний опис моделі:

Модель симулює поширення вогню через ліс і демонструє, як сила вітру впливає на поширення пожежі. Ліс представлений як сітка клітинок (певних ділянок, кожна з яких може бути деревом), де кожне дерево має шанс загорітися, якщо його сусіднє дерево вже горить. Ця модель є модифікацією попередньої Fire Simple Extension 1, додаючи елемент вітру, що змінює ймовірність загоряння дерев в залежності від напрямку та сили вітру.

### Керуючі параметри:

- **density** регулює щільність дерев у лісі, що впливає на кількість зелених клітин на початку симуляції; чим більша щільність, тим більше дерев у лісі.
- **probability-of-spread** визначає ймовірність того, що вогонь перекинеться з одного дерева на інше. Його можна змінювати, щоб контролювати, наскільки швидко або повільно поширюється пожежа.
- **south-wind-speed** впливає на те, як вітер з півдня або з півночі змінює ймовірність поширення вогню. Якщо значення позитивне, це означає, що вітер дме з півдня, якщо ж значення від’ємне, це означає, що вітер дме з півночі.
- **west-wind-speed** регулює вітер з заходу або зі сходу, і його вплив на поширення вогню. Позитивне значення означає, що вітер дме із заходу, якщо ж значення негативне, це вказує на вітер зі сходу.

### Внутрішні параметри:

- **initial-trees** показує скільки дерев було на початку симуляції. Іншими словами, з якою кількістю буде починатись симуляція.
- **probability** визначає ймовірність розповсюдження вогню, враховуючи силу вітру.
- **direction** напрямок від поточного зеленого дерева до палаючого дерева.

### Показники роботи системи:

- відсоток згорілих дерев, що дозволяє оцінити масштаб руйнування лісу.
- швидкість поширення вогню, яка залежить від налаштувань ймовірності поширення та впливу вітру.

<br>

### Налаштування середовища BehaviorSearch:

**Обрана модель**:
<pre>
E:\Programs\NetLogo 6.4.0\models\IABM Textbook\chapter 3\Fire Extensions\Fire Simple Extension 2.nlogo
</pre>
**Параметри моделі** (вкладка Model):  
Натиснувши на кнопку "Load parameter ranges from model interface" можна у вкладці Model отримати усі параметри поточної моделі. Для лабораторної роботи було змінено початкові значення параметрів density та probability-of-spread на 40. Значення вітру також було змінено на 5. Значення вітру було змінено через те, що при використанні негативного значення west-wind-speed значення ураження було мінімальним завжди. Це пов'язано з недоліком моделі.
<pre>
["density" [40 1 100]]
["probability-of-spread" [40 1 100]]
["south-wind-speed" [5 1 25]]
["west-wind-speed" [5 1 25]]
</pre>
Використовувана **міра**:  
Для фітнес-функції було обрано **значення ураження лісу від пожежі**, вираз для її розрахунку взято з налаштувань монітору аналізованої імітаційної моделі в середовищі NetLogo  

![Редагування параметрів монітору вихідної моделі](Measure.png) 

та вказано у параметрі "**Measure**":
<pre>
((count patches with [shade-of? pcolor red]) / initial-trees) * 100
</pre>

Відсоток ураження лісу від пожежі повинно вираховуватися завдяки кількості червоних патчів ділених на початкову кількість дерев, та результат цього розрахунку помноженого на 100, для того щоб отримати значення у відсотках. Період симуляції рівний 100 тактів, починаючи з 0 такту симуляції.
Параметр зупинки за умовою ("**Stop if**") у разі не використовувався.  
Загальний вигляд вкладки налаштувань параметрів моделі:

![Вкладка налаштувань параметрів моделі](Parameters.png)

**Налаштування цільової функції** (вкладка Search Objective):  
Метою підбору параметрів імітаційної моделі, що описує рух вогню по лісу, є мінімізація значення ураження лісу від пожежі – це вказано через параметр "**Goal**" зі значенням **Minimize Fitness**. Тобто необхідно визначити такі параметри налаштувань моделі, у яких ліс згоряє з мінімальним значенням ураження. При цьому цікавить значення на останньому кроці симуляції. Для цього у параметрі "**Collected measure**", що визначає спосіб обліку значень обраного показника, вказано **AT_FINAL_STEP**.  
Щоб уникнути викривлення результатів через випадкові значення, що використовуються в логіці самої імітаційної моделі, **кожна симуляція повторюється по 10 разів**, результуюче значення розраховується як **середнє арифметичне**.
Загальний вигляд вкладки налаштувань цільової функції:

![Вкладка налаштувань цільової функції](Objective.png)

**Налаштування алгоритму пошуку** (вкладка Search Algorithm):  
На цьому етапі було визначено модель, налаштовано її параметри, і обрано міру, що лежить в основі функції пристосованості, що дозволяє оцінити якість кожного перевіряємого BehaviorSearch варіантів рішення.  
У ході дослідження на лабораторній роботі використовувалися два алгоритми: випадковий пошук (**RandomSearch**) і простий генетичний алгоритм (**StandardGA**).  
Для цих алгоритмів, що вирішують завдання пошуку такого набору параметрів імітаційної моделі, щоб задовольнити вимоги користувача, необхідно вказати "**Evaluation limit**" рівним 300, та "**Search Space Encoding Representation**" в значення StandardBinaryChromosome.
Загальний вид вкладки налаштувань алгоритму пошуку:

![Вкладка налаштувань пошуку](Search.png)

<br>

### Результати використання BehaviorSearch:
Діалогове вікно запуску пошуку:

![Вікно запуску пошуку](Dialog.png)

Результат пошуку параметрів імітаційної моделі, використовуючи **генетичний алгоритм**:  

![Результати пошуку за допомогою ГА](Result-ga.png)

Результат пошуку параметрів імітаційної моделі, використовуючи **випадковий пошук**:  

![Результати випадкового пошуку](Result-rs.png)