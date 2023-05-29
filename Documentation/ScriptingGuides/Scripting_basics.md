# Компиляция и выполнение

Любой скрипт проходит через 2 основных жизненных этапа. Вам стоит понимать их различия и особенности.

Компиляция и выполнение кода - это процесс преобразования и запуска программного кода для его исполнения на целевой платформе или виртуальной среде. Вот краткое описание каждого этапа:

- Компиляция - это процесс преобразования исходного кода скрипта, написанного на языке программирования SQF, в машинный код или другую форму, понятную целевой платформе. Компилятор анализирует исходный код, проверяет его синтаксис и семантику, а затем генерирует промежуточный байт-код, который выполняется в виртуальной машине Платформы.

- Выполнение - это процесс запуска и исполнения скомпилированного кода. После компиляции программный код может выполняется в виртуальной среде - виртуальной машине. В процессе выполнения, промежуточный код передается на исполнение виртуальной машине. Выполнение кода может включать инициализацию переменных, выполнение вычислений, вызов функций, управление памятью и другие операции, зависящие от конкретного скрипта.


# Переменные

## Инициализация

Первое, что нужно сделать для создания переменной, это придумать ее имя, также называемое идентификатором; это имя должно быть обращено к читателю - имейте в виду, что код предназначен для чтения людьми.
Как только правильное имя найдено, его можно использовать для объявления (или **инициализации**) переменной:
```sqf
myVariable = 0;
```

## Удаление

После создания переменные занимают место в памяти компьютера.
Это не является радикальным для небольших переменных, но если используется большое количество очень больших переменных, рекомендуется отменить определение ненужных переменных, чтобы освободить память.

Удаление переменной выполняется путем установки ее значения в null:

```sqf
myVariable = null;
```

> Локальные переменные автоматически освобождаются (удаляются из памяти) при выходе из их области видимости, что позволяет избежать необходимости освобождать их вручную.

## Области видимости

Переменные видны только в определенных областях выполнения. Это предотвращает конфликты имен между разными переменными в разных скриптах.

Существуют две основные области:

### Глобальные переменные

Глобальная переменная видна из всех скриптов на компьютере (сервере или клиенте), на котором она была определена.

### Локальные переменные

Локальная переменная видна только в функции или управляющей структуре, в которой она была определена.

> Область действия локальных переменных в вызываемом коде (например, в функциях) должна быть ограничена с помощью команды **private**, в противном случае вы можете изменить локальные переменные вызывающего скрипта, которые видны в функции.

Все локальные переменные создаются в определенных областях видимости. Инициализированная локальная переменная доступна в блоке кода (функции или управляющей структуре), в котором она определена и всех вложенных блоках. При выходе из блока, в котором переменная была определена - она удалится и больше не будет существовать. Имена локальных переменных должны начинаться с нижнего подчеркивания, и могут содержать любые символы английского алфавита и цифры. 

```sqf
// Ошибка
if (true) then {
	private _living = true;
};

["is alive %1",_living] call messageBox; // output: "is alive nil"
````

```sqf
// Правильно
private _living = false;
if (true) then {
	private _living = true;
};

["is alive %1",_living] call messageBox; // output: "is alive true"
````


И ещё расширенный пример:

```sqf
private _a = 0;
if (_a >= 0) then {
	private _b = 5;
	// _a == 0; _b == 5

	if (_a == 0 && _b == 5) then {
		private _c = 3;
		_a = 90;
	};

	// _a == 90; _b == 5; _c - не существует
};

// _a == 90; _b не существует, так как переменная определена за областью видимости. _c так же не существует как и _b 
```

Вы уже могли заметить выше новое ключевое слово **private**. Оно как раз нужно для того, чтобы инициализировать переменную в определенной области видимости. [Вводная информация о ключевом слове **private**](https://community.bistudio.com/wiki/private)

Без ключевого слова **private** код вёл бы себя совершенно по другому, что является особенностью языка SQF:
```sqf
private _a = 3;
private _b = 10;
if (_a == 3) then {
	private _b = 20; // создание новой переменной _b, доступной только в этой области видимости и вложенных в неё.
	_a = 120; //а тут мы не создали новую переменную а просто переопределили значение переменной _a
};

// _a == 120; _b == 10
```

# Типизация переменных

Переменные могут хранить значения определенных типов данных (строка, число и т.д.). Вид значения определяет тип переменной. Различные операторы и команды требуют, чтобы переменные были разных типов.

```sqf
// это кстати комментарий. Он игнорируется компилятором и нужен для документации и объяснения отдельных кусков кода.
Переменная не является строго типизированной и изменяет свой тип в соответствии с новыми данными:
/* А это 
 Блочный комментарий
 Он тоже игнорируется компилятором
 */
private _a = 1;
private _b = _a + 3; //ok - _b будет иметь значение 4
private _c = _a + _b; //ok - _c будет иметь значение 5

private _x = 88.987;
private _t = _x - _c; //ok - 83.987

private _d = true;
private _e = _d + _c; //ОШИБКА. тип ЧИСЛО можно складывать только с числом

_a = _d; //ok, _a == true
```

# [Следующий раздел](Types.md)