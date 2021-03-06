
Oписание
======

MineOSInterface - это библиотека, поставляющаяся в комплекте с операционной системой MineOS. Она реализует основные системные виджеты, а также отвечает за все манипуляции с окнами. В качестве интерфейсной основы она использует библиотеки **[GUI](https://github.com/IgorTimofeev/OpenComputers/blob/master/Documentation/GUI.md)** и **[doubleBuffering](https://github.com/IgorTimofeev/OpenComputers/blob/master/Documentation/doubleBuffering.md)**.

Кроме того, она предоставляет таблицу с цветами интерфейса ОС по умолчанию:

![](https://i.imgur.com/xm40hG3.png)

Для произвольного изменения цветов отдельных элементов окон обращайтесь к ним напрямую. К примеру, через:

```lua
<окно>.backgroundPanel.colors.background = 0xFF00FF
```

Основные методы
======

MineOSInterface.**addWindow**( window ): *table* mainContainer, *table* window
-----------------------------------------------------------

| Тип | Аргумент | Описание |
| ------ | ------ | ------ |
| *table* | window | Объект окна, созданный методами, описанными ниже |

Добавляет созданное окно в окружение MineOS, регистрирует его иконку в Dock, устанавливает обработчики событий, а также добавляет ему несколько методов для пользовательской манипуляции.

| Тип свойства | Свойство |Описание |
| ------ | ------ | ------ |
| *function* | :**resize**(*int* width, *int* height) | Изменяет размеры окна на указанные, вызывая при этом callback-функцию .**onResize** |
| *callback-function* | .**onResize**(*int* newWidth, *int* newHeight) | Вызывается при изменении размеров окна. Как правило, этот метод используется для изменения размеров и координат содержимого окна после изменения размеров |
| *function* | :**close**() | Закрывает окно и удаляет его из системного окружения |
| *function* | :**minimize**() | Скрывает окно, однако оставляет возможность обратного показа путем клика на его иконку в Dock |
| *function* | :**maximize**() | Изменяет размеры окна под размер экрана, вызывая при этом callback-функцию .**onResize**. Повторный вызов функции вернет размер окна к прежним значениям. Последующие вызовы будут эквивалентны первым двум |

Примеры реализации описаны ниже.

Методы для создания окон
======

MineOSInterface.**window**( x, y, width, height ): *table* window
-----------------------------------------------------------

| Тип | Аргумент | Описание |
| ------ | ------ | ------ |
| *int* | x | Координата окна по оси X |
| *int* | x | Координата окна по оси Y |
| *int* | width | Ширина окна |
| *int* | height | Высота окна |

Создает пустое окно без каких-либо элементов интерфейса. Данный объект является шаблоном для всех остальных, описанных ниже.

Пример реализации:

```lua
local GUI = require("GUI")
local MineOSInterface = require("MineOSInterface")

------------------------------------------------------------------------------------------------------

local mainContainer, window = MineOSInterface.addWindow(MineOSInterface.window(1, 1, 88, 25))
window:addChild(GUI.panel(1, 1, window.width, window.height, 0x888888))
```

Результат:

![](https://i.imgur.com/lhrm0z6.png?1)

MineOSInterface.**filledWindow**( x, y, width, height [, color]): *table* window
-----------------------------------------------------------

| Тип | Аргумент | Описание |
| ------ | ------ | ------ |
| *int* | x | Координата окна по оси X |
| *int* | x | Координата окна по оси Y |
| *int* | width | Ширина окна |
| *int* | height | Высота окна |
| [*int* | color] | Опциональный цвет фоновой панели |

Создает окно с добавленной фоновой панелью, а также кнопками для закрытия/минимизации/максимизации, являющееся наследником  *MineOSInterface*.**window**. Если цвет панели не указывается, то используется *MineOSInterface.colors.windows.backgroundPanel*.

| Тип свойства | Свойство |Описание |
| ------ | ------ | ------ |
| *table* | .**backgroundPanel** | Указатель на объект фоновой панели (GUI.**panel**) |
| *table* | .**actionButtons** | Указатель на объект кнопок действия (GUI.**actionButtons**) |

Пример реализации:

```lua
local MineOSInterface = require("MineOSInterface")

------------------------------------------------------------------------------------------------------

local mainContainer, window = MineOSInterface.addWindow(MineOSInterface.filledWindow(1, 1, 88, 25, 0xF0F0F0))
```

Результат:

![](https://i.imgur.com/YlCOx68.png?1)

MineOSInterface.**titledWindow**( x, y, width, height, titleText, [addTitlePanel]): *table* window
-----------------------------------------------------------

| Тип | Аргумент | Описание |
| ------ | ------ | ------ |
| *int* | x | Координата окна по оси X |
| *int* | x | Координата окна по оси Y |
| *int* | width | Ширина окна |
| *int* | height | Высота окна |
| *string* | titleText | Текст заголовка |
| [*int* | addTitlePanel] | Опциональное добавление подложки под текст заголовка |

Создает окно с текстовым заголовком. Если указан аргумент, добавляющий подложку, то текст будет находиться на сером прямоугольнике.

| Тип свойства | Свойство |Описание |
| ------ | ------ | ------ |
| *table* | .**backgroundPanel** | Указатель на объект фоновой панели (GUI.**panel**) |
| *table* | .**actionButtons** | Указатель на объект кнопок действия (GUI.**actionButtons**) |
| *table* | .**titleLabel** | Указатель на объект лейбла заголовка (GUI.**label**) |
| *table* | .**titlePanel** | Указатель на объект лейбла заголовка (GUI.**panel**). Создается только при наличии соответствующего аргумента |

Пример реализации:

```lua
local MineOSInterface = require("MineOSInterface")

------------------------------------------------------------------------------------------------------

local mainContainer, window = MineOSInterface.addWindow(MineOSInterface.titledWindow(1, 1, 88, 25, "Hello world", true))
```

Результат:

![](https://i.imgur.com/4q7DCDD.png?1)

MineOSInterface.**tabbedWindow**( x, y, width, height ): *table* window
-----------------------------------------------------------

| Тип | Аргумент | Описание |
| ------ | ------ | ------ |
| *int* | x | Координата окна по оси X |
| *int* | x | Координата окна по оси Y |
| *int* | width | Ширина окна |
| *int* | height | Высота окна |

Создает окно с панелью вкладок по шаблонным цветам.

| Тип свойства | Свойство |Описание |
| ------ | ------ | ------ |
| *table* | .**tabBar** | Указатель на объект панели вкладок (GUI.**tabBar**) |
| *table* | .**backgroundPanel** | Указатель на объект фоновой панели (GUI.**panel**) |
| *table* | .**actionButtons** | Указатель на объект кнопок действия (GUI.**actionButtons**) |

Пример реализации:

```lua
local MineOSInterface = require("MineOSInterface")

------------------------------------------------------------------------------------------------------

local mainContainer, window = MineOSInterface.addWindow(MineOSInterface.tabbedWindow(1, 1, 88, 25))
window.tabBar:addItem("Приложения")
window.tabBar:addItem("Библиотеки")
window.tabBar:addItem("Обои")
window.tabBar:addItem("Обновления")
```

Результат:

![](https://i.imgur.com/294FatT.png?1)
