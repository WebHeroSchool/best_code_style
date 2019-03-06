# Лучшие практики

### 1. Имя переменной должно максимально чётко соответствовать хранимым в ней данным
Благодаря этой практике название переменных станут удобны для поиска, что очень помогает при рефакторинге и сопровождении кода
```
// плохо
let nameString
let theUsers

// хорошо
let name
let users
```


### 2. Не использовать конструктор для создания примитивных типов (число, строка, булево значение и т.д.)
Это не одно и то же. Это означает, что если проверить тип для Number(10) или String("hello") с помощью typeof, то мы получим object. К тому же, использование "объектных оберток" может привести к неожиданному поведению программы, отличному от ее поведения при работе с примитивными значениями.
```
// плохо
var x1 = new Object()
var x2 = new String()
var x3 = new Number()
var x4 = new Boolean()
var x5 = new Array()
var x6 = new RegExp()
var x7 = new Function()

// хорошо
var x1 = {};           // new object
var x2 = "";           // new primitive string
var x3 = 0;            // new primitive number
var x4 = false;        // new primitive boolean
var x5 = [];           // new array object
var x6 = /()/;         // new regexp object
var x7 = function(){}; // new function object 
```


### 3. Используйте стрелочные функции
Обладают более коротким и удобным синтаксисом, к тому же, используя их, не возникает путаницы с this. Стрелочные функции не содержат собственный контекст this, а используют значение this окружающего контекста.
```
// плохо
[1, 2, 3].map(function (x) {
  const y = x + 1;
  return x * y;
});

// хорошо
[1, 2, 3].map((x) => {
  const y = x + 1;
  return x * y;
});
```


### 4. Не передавать строку в SetInterval и SetTimeOut. Вместо этого передавать название функции
```
// плохо
setInterval(
"document.getElementById('container').innerHTML += 'My new number: ' + i", 3000
);

// хорошо
setInterval(someFunction, 3000);
```

### 5. Всегда использовать точку с запятой
Отсутсвие точки с запятой может привести к ошибкам при выполнении кода, если он большой её к тому же будет сложно найти
```
// плохо
var someItem = 'some string'
function doSomething() {
  return 'something'
}

// хорошо
var someItem = 'some string';
function doSomething() {
  return 'something';
}
```


### 6. Использовать параметр по умолчанию
Если функция вызывается с отсутствующим аргументом, значение отсутствующего аргумента устанавливается равным неопределенному. Неопределенные значения могут привести к неожиданным значениям. 
```
//хорошо
function myFunction(x, y) {
  if (y === undefined) {
    y = 0;
  }
}
```

### 7. Eval = зло
Метод eval() выполняет JavaScript код, представленный строкой. Он дает доступ к компилятору JavaScript. Т.е. мы можем выполнить команду записанную в строковой переменной, которую передадим в качестве параметра в eval. Eval() - опасная функция, которая выполняет код, проходящий со всеми привилегиями вызывателя. Если вы запускаете eval() со строкой, на которую могут влиять злоумышленники, то вы можете запустить вредоносный код на устройство пользователя с правами вашей веб-страницы/расширения. Наиболее важно, код третьей стороны может видеть область видимости, в которой был вызван eval(), что может может привести к атакам, похожим на Function.
Также eval(),как правило, медленнее альтернатив, так как вызывает интерпретатор JS, тогда как многие другие конструкции оптимизированы современными JS движками.
```
// плохо 
var obj = { a: 20, b: 30 };
var propname = getPropName();  // возвращает "a" или "b"
eval( "var result = obj." + propname );

// хорошо
var obj = { a: 20, b: 30 };
var propname = getPropName();  // возвращает "a" или "b"
var result = obj[ propname ];  //  obj[ "a" ] тоже, что и obj.a
```


### 8. Используйте === вместо ==
```
//плохо
if (1 == '1') {
    console.log("Это верно!");
}

//хорошо
if (1 === '1') {
    console.log("Это не верно!");
}
```
Оператор двойного выполняет преобразование типов, поэтому в данном случае он просто конвертирует строку "1" в число 1, что и приводит к выполнению равенства. Тройное равенство не производит преобразования типов, это помогает избежать ошибок, например, таких:
```
'' == '0' // false  
'0' == ''  // true  
false == '0' // true  
' \t\r\n ' == 0   // true 
```

### 9. Добавляйте элементы в DOM не поштучно, а все разом
Добавление каждого нового элемента заставляет браузер заново отрисовывать всю страницу целиком, поэтому, если нужно добавить на страницу сразу много элементов, то лучше делать это не поштучно. 
```
// плохо
var list = document.getElementById("list"),
    items = ["one", "two", "three", "four"],
    el;

for (var i = 0; items[i]; i++) {
  el = document.createElement("li");
  el.appendChild( document.createTextNode(items[i]) );
  list.appendChild(el); 
}

// хорошо
var list = document.getElementById("list"),
    frag = document.createDocumentFragment(),
    items = ["one", "two", "three", "four"],
    el;

for (var i = 0; items[i]; i++) {
  el = document.createElement("li");
  el.appendChild( document.createTextNode(items[i]) );
  frag.appendChild(el); 
}

list.appendChild(frag);
```

### 10. Писать короткие функции, решающие одну задачу
Функции легче поддерживать, они становятся гораздо более понятными, читабельными, если они направлены на решение лишь какой-то одной задачи. Если мы сталкиваемся с ошибкой, то, при использовании маленьких функций, найти источник этой ошибки становится гораздо легче. Кроме того, улучшаются возможности по повторному использованию кода.
```
// плохо
function averageArray(array) {
   let sum = array.reduce((number, currentSum) => number + currentSum)
   return sum / array.length
}

// хорошо
function sumArray(array) {
      return array.reduce((number, currentSum) => number + currentSum)
}

function averageArray(array) {
      return sumArray(array) / array.length
}
```
