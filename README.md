# MLCT ✨
**MLCT** *(**M**ine**L**and **C**ode **T**ranslator)* (эм эл кэ тэ) - Мод, для установки письменного варианта блочного кода на сервере Mineland.

## Содержание 🗂️

1. [Установка](#установка-)
2. [Синтаксис](#синтаксис-)
3. [Документация](#документация-)
4. [Примеры](#примеры-)
5. [Подсветка кода](#подсветка-кода-)

# Установка 🤙

1. Необходимо иметь Python версии не ниже 3.11.1 \(Установить можно [тут](https://www.python.org/ftp/python/3.11.1/python-3.11.1-amd64.exe)\)
2. Откройте раздел [релизов](https://github.com/mee8yt/mlct-compiler/releases/latest ) и нажмите "Download ZIP"
3. Распакуйте архив
4. Запустите `Main.py`

> [!IMPORTANT]
> Без Python вы банально не сможете скомпилировать ваш код

# Синтаксис 💻

> [!NOTE]
> Перед использованием `MLCT` следует знать его синтаксис.

Типы данных:
```js
"Текст"      // Текстовое значение
`Переменная` // Переменная / сложное значение / свободный текст
123          // Числовое значение
0.5          // Числовое значение с точкой
```

Если перед свободным текстом (переменной) поставить букву(ы), то переменная может превратиться в сложное значение:
```js
`Переменная`                    // Так и осталось
s`Сохранённая переменная`       // Переменная стала сохранённой
a`Массив`                       // Теперь это массив
as`Массив`                      // Массив тоже умеет сохраняться
l`Местоположение`               // Набор из пяти чисел, или местоположение
v`Значение`                     // Игровое значение, то есть яблочко
i`Предмет`                      // Любой предмет
potion`Зелье`                   // Эффект зелья
particle`Частица`               // Эффект частицы

"Текст"                         // Обычный текст
c"Текстовый компонент"          // Текстовый компонент
cls"Безцветный текст"           // Символ & не заменится на цветной символ
```

Такая буква (буквы), делающая из одного значение другое называется "оболочка значения" (Value shell).

Установка переменных:
```js
var `test` = 0;
var s`%player% money` = 0;
```

Вызов действий:
```js
player.send("Привет!");
```

Эта строка кода описывает "Действие игрока - Отправить сообщение"

Теперь разребём строение этой строки кода:

```js
   player.send("Привет!");
// ^^^^^^
// Блок

player.send("Привет!");
//    ^
//    Разделитель

player.send("Привет!");
//     ^^^^
//     Имя действия

player.send("Привет!");
//          ^^^^^^^^^
//          Аргументы

player.send("Привет!");
//                    ^
//                   Конец блока
```

> [!NOTE]
>Каждая строка кода должна оканчиваться знаком ";" (точка с запятой)

На очереди заполнение аргументов

Например возьмём действие "Отправить титул". У него есть аргументы "text1, text2, number1, number2, number3". Их можно указать разными способами:

*Позиционный:*
```js
player.title("Привет", "Это мой мир", 20, 20, 20);
```

*По имени:*
```js
player.title(text1="Привет", number1=0);
```
*Комбинированный:*
```js
player.title(text1="Привет", "Это мой мир");

//Позиционные аргументы устанавливаются на следующее неустановленное значение:
player.title(number2=5, "Привет", "Это мой мир"); // Тоже самое, что (text1="Привет", text2="Это мой мир", number2=5);
```

Аргументы бывают двух видов: значение и переключатель. Переключатели задаются числом, которое отражает количество нажатий по переключателю:
```js
player.send(["Вы прошли паркур", "Хотите ещё раз? /play"], 2); // Переключатель нажмётся 2 раза, и будет "разделение на строки".
```

Аргументы, которые отражают значение могут принимать сразу много значений, для этого используется "список значений". В квадратных скобках `[]` через запятую устанавливаются несколько значений для одного аргумента
```js
player.send(["Игрок лайкнул игру", "Хочешь так-же? /like"]);
```

У действий есть селекторы (когда нажимаешь по табличке ШИФТ + ПКМ):
```js
player.send<all>("&fИгрок &e%player%&f зашёл в игру!");
```
>[!WARNING]
>Селекторы можно ставить только *перед* круглыми скобками


Теперь научимся писать код, посмотрим на события и условия:
```js
PlayerEvent(join) {
   player.send<all>("&fИгрок &e%player%&f зашёл в игру!");
   ifPlayer.havePermissions() {
      player.setGamemode(3);
   }
   game.startLoop("random_item");
}

Loop(random_item, 100) {
   player.giveRandomItem<all>(i`diamond`, i`emeralnd`, i`stone`);
}
```

>[!WARNING]
>Для циклов, функций и событий, у которых имя содержит пробелы используются спец. символы ``
>
>Например, если функция или цикл называется "анти гм", то в строке Loop() или Function()
> имя пишется так: \`анти гм\`

# Документация 📜
Более подробная информация по всем событиям, условиям и действиям кода.
   - [Активаторы](documentation/activators.md) 
     -  [Событие игрока](documentation/activators.md#событие-игрока---playereventevent--none--)
     -  [Событие мира](documentation/activators.md#событие-мира---worldeventevent--none--)
     -  [Циклы](documentation/activators.md#циклы---loopname-0--none--)
     -  [Функции](documentation/activators.md#функции---functionname--none--)
   - [Действия](documentation/actions.md)
     - [Действие игрока](documentation/actions.md#действие-игрока---playeractionargs-)
     - [Игровое действие](documentation/actions.md#игровое-действие---gameactionargs-)
     - [Установить переменную](documentation/actions.md#установить-переменную---varactionargs-)
     - [Работа с массивами](documentation/actions.md#работа-с-массивами---arrayactionargs-)
     - [Выбрать объект](documentation/actions.md#выбрать-объект---selectdefault-)
   - [Условия](documentation/conditions.md)
     - [Если игрок](documentation/conditions.md)
     - [Если значение](documentation/conditions.md)
     - [Если существо](documentation/conditions.md)
     - [Если игра](documentation/conditions.md)
     - [Иначе](documentation/conditions.md)

# Примеры 📧
код на топы, на гм, на cex м прочую фигню

# Подсветка кода 💡
тут гайд про вс код
