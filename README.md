# 20 функций JavaScript


### **1. Оператор нулевого слияния**

Оператор нулевого слияния `??` используется для предоставления значения по умолчанию на случай,
если значение переменной будет равно `null` или `undefined`.
````js
let foo = null;
let bar = foo ?? 'default value';
console.log(bar); // Output: 'default value'
````
Используйте оператор нулевого слияния для обработки случаев, когда могут появиться значения `null` или `undefined`.
Благодаря значениям по умолчанию ваш код будет работать без проблем.

### **2. Оператор опциональной последовательности**

Оператор `?.` обеспечивает безопасный доступ к глубоко вложенным свойствам [объектов](https://techrocks.ru/2023/06/02/javascript-objects/).
Он позволяет избежать ошибок во время выполнения, которые могут возникнуть, если какое-нибудь из промежуточных свойств не существует.
````js
const user = {
  profile: {
    name: 'Alice'
  }
};
const userProfileName = user.profile?.name;
console.log(userProfileName); // Output: 'Alice'

const userAccountName = user.account?.name;
console.log(userAccountName); // Output: undefined
````
Используйте оператор опциональной последовательности, чтобы избежать ошибок при обращении к свойствам объектов,
потенциально равных `null` или `undefined`. Это сделает ваш код более надежным.

### **3. Числовые разделители**

Числовые разделители `_` делают большие числа более читаемыми, визуально разделяя цифры.
````js
const largeNumber = 1_000_000;
console.log(largeNumber); // Output: 1000000
````
Используйте числовые разделители, чтобы улучшить читаемость больших чисел в коде, особенно при финансовых расчетах
или работе с большими наборами данных.

### **4. Promise.AllSettled**

`Promise.allSettled` ждет выполнения всех промисов (т.е. пока каждый из них будет либо выполнен, либо отклонен),
и возвращает массив объектов, описывающих результат.
````js
const promises = [Promise.resolve('Success'), Promise.reject('Error'), Promise.resolve('Another Success')];

Promise.allSettled(promises).then((results) => {
  results.forEach((result) => console.log(result));
});
// Output:
// { status: 'fulfilled', value: 'Success' }
// { status: 'rejected', reason: 'Error' }
// { status: 'fulfilled', value: 'Another Success' }
````
Используйте `Promise.allSettled`, когда вам нужно обработать несколько промисов и гарантировать обработку всех результатов,
независимо от результатов выполнения каждого отдельного промиса.

### **5. Приватные поля класса**

Приватные поля класса — это свойства, которые могут быть доступны и изменены только в пределах класса,
в котором они объявлены.
````js
class MyClass {
  #privateField = 42;

  getPrivateField() {
    return this.#privateField;
  }
}

const instance = new MyClass();
console.log(instance.getPrivateField()); // Output: 42
console.log(instance.#privateField); // Uncaught Private name #privateField is not defined.
````
Используйте приватные поля класса для инкапсуляции данных внутри класса. Это гарантирует, что конфиденциальные данные
не будут открыты или изменены за пределами класса.

### **6. Логические операторы присваивания**

Логические операторы присваивания (`&&=`, `||=`, `??=`) объединяют логические операторы и присваивание,
обеспечивая краткий способ обновления переменных на основе условия.
````js
let a = true;
let b = false;

a &&= 'Assigned if true';
b ||= 'Assigned if false';

console.log(a); // Output: 'Assigned if true'
console.log(b); // Output: 'Assigned if false'
````
Используйте логические операторы присваивания, чтобы упростить условные присваивания и сделать ваш код более читаемым
и лаконичным.

### **7. Метки для циклов и блоков**

Метки — это идентификаторы, за которыми следует двоеточие. Они используются для обозначения циклов или блоков,
чтобы на них можно было ссылаться в операторах `break` или `continue`.
````js
outerLoop: for (let i = 0; i < 3; i++) {
  console.log(`Outer loop iteration ${i}`);
  for (let j = 0; j < 3; j++) {
    if (j === 1) {
      break outerLoop; // Break out of the outer loop
    }
    console.log(`  Inner loop iteration ${j}`);
  }
}
// Output:
// Outer loop iteration 0
//   Inner loop iteration 0

labelBlock: {
  console.log('This will be printed');
  if (true) {
    break labelBlock; // Exit the block
  }
  console.log('This will not be printed');
}
// Output:
// This will be printed
````
Используйте метки для управления сложным поведением циклов. Это упрощает управление вложенными циклами
и повышает ясность кода.

### **8. Теговые шаблоны**

Теговые шаблоны позволяют разбирать шаблонные литералы с помощью функции, обеспечивая пользовательскую обработку
строковых литералов.

Пример 1:
````js
function logWithTimestamp(strings, ...values) {
  const timestamp = new Date().toISOString();
  return (
    `[${timestamp}] ` +
    strings.reduce((result, str, i) => {
      return result + str + (values[i] || '');
    })
  );
}

const user = 'JohnDoe';
const action = 'logged in';
console.log(logWithTimestamp`User ${user} has ${action}.`);
// Outputs: \[2024-07-10T12:34:56.789Z\] User JohnDoe has logged in.
````
Пример 2:
````js
function validate(strings, ...values) {
  values.forEach((value, index) => {
    if (typeof value !== 'string') {
      throw new Error(`Invalid input at position ${index + 1}: Expected a string`);
    }
  });
  return strings.reduce((result, str, i) => {
    return result + str + (values[i] || '');
  });
}

try {
  const validString = validate`Name: ${'Alice'}, Age: ${25}`;
  console.log(validString); // This will throw an error
} catch (error) {
  console.error(error.message); // Outputs: Invalid input at position 2: Expected a string
}
````
Используйте теговые шаблоны для расширенной обработки строк, например для создания безопасных шаблонов HTML
или локализации строк.

### **9. Побитовые операторы для быстрых вычислений**

Побитовые операторы в JavaScript выполняют операции над двоичными представлениями чисел. Они часто используются
для низкоуровневых задач программирования, но могут быть полезны и для быстрых математических операций.

Список побитовых операторов:

* `&` (AND)
* `|` (OR)
* `^` (XOR)
* `~` (NOT)
* `<<` (сдвиг влево)
* `>>` (сдвиг вправо)
* `>>>` (беззнаковый сдвиг вправо)

**Пример 1.** С помощью оператора AND можно проверить, является ли число четным или нечетным.
````js
const isEven = (num) => (num & 1) === 0;
const isOdd = (num) => (num & 1) === 1;

console.log(isEven(4)); // Outputs: true
console.log(isOdd(5)); // Outputs: true
````
**Пример 2.** Операторы сдвига влево (`<<`) и сдвига вправо (`>>`) можно использовать для умножения и деления
на степень 2.
````js
const multiplyByTwo = (num) => num << 1;
const divideByTwo = (num) => num >> 1;

console.log(multiplyByTwo(5)); // Outputs: 10
console.log(divideByTwo(10)); // Outputs: 5
````
**Пример 3.** С помощью оператора AND можно проверить, является ли число степенью 2.
````js
const isPowerOfTwo = (num) => num > 0 && (num & (num - 1)) === 0;

console.log(isPowerOfTwo(16)); // Outputs: true
console.log(isPowerOfTwo(18)); // Outputs: false
````
Используйте побитовые операторы для критически важных приложений, где требуется низкоуровневое манипулирование
двоичными данными, или для быстрых математических операций.

### **10. Оператор in для проверки свойств**

Оператор `in` проверяет, существует ли свойство в объекте.
````js
const obj = { name: 'Alice', age: 25 };
console.log('name' in obj); // Output: true
console.log('height' in obj); // Output: false
````
Использование оператора `in` для проверки существования свойств в объектах, гарантирует, что ваш код
будет изящно обрабатывать объекты с отсутствующими свойствами.

### **11. Оператор debugger**

Оператор `debugger` вызывает любые доступные функции отладки, например, установку точки останова в коде.
````js
function checkValue(value) {
  debugger; // Execution will pause here if a debugger is available
  return value > 10;
}
checkValue(15);
````
Используйте оператор `debugger` во время разработки, чтобы приостанавливать выполнение и контролировать поведение кода.
Это поможет вам эффективнее выявлять и исправлять ошибки.

### **12. Присваивание по цепочке**

Присваивание по цепочке используется для назначения нескольким переменным одинакового значения.
````js
let a, b, c;
a = b = c = 10;
console.log(a, b, c); // Output: 10 10 10
````
Использование присваивания по цепочке для инициализации нескольких переменных одним и тем же значением
сокращает избыточность кода.

### **13. Динамические имена функций**

Динамические имена функций позволяют определять функции с именами, вычисляемыми во время выполнения.
````js
const funcName = 'dynamicFunction';
const obj = {
  [funcName]() {
    return 'This is a dynamic function';
  }
};

console.log(obj.dynamicFunction()); // Output: 'This is a dynamic function'
````
Создание функций с именами, зависящими от данных, полученных во время выполнения, повышает гибкость и удобство
повторного использования кода.

### **14. Получение аргументов функции**

Объект `arguments` — это объект типа массива, который содержит аргументы, переданные функции.

````js
function sum() {
  let total = 0;
  for (let i = 0; i < arguments.length; i++) {
    total += arguments[i];
  }
  return total;
}

console.log(sum(1, 2, 3)); // Outputs: 6
````
Использование объекта `arguments` для доступа ко всем аргументам, переданным функции, полезно для функций
с аргументами переменной длины.

### **15. Унарный оператор +**

Унарный оператор `+` преобразует свой операнд в число.
````js
console.log(+'abc'); // Outputs: NaN
console.log(+'123'); // Outputs: 123
console.log(+'45.67'); // Outputs: 45.67 (converted to a number)

console.log(+true); // Outputs: 1
console.log(+false); // Outputs: 0

console.log(+null); // Outputs: 0
console.log(+undefined); // Outputs: NaN
````
Используйте унарный оператор для быстрого преобразования типов, особенно при работе с пользовательским вводом
или данными из внешних источников.

### **16. Оператор возведения в степень**

Оператор возведения в степень `**` возводит число (левый операнд) в степень (правый операнд).
````js
const base = 2;
const exponent = 3;
const result = base ** exponent;
console.log(result); // Output: 8
````
Используйте оператор `**` для получения кратких и читабельных математических выражений, например,
в научных или финансовых расчетах.

### **17. Свойства функций**

Функции в JavaScript являются объектами и могут иметь свойства.

**Пример 1:**
````js
function myFunction() {}
myFunction.description = 'This is a function with a property';
console.log(myFunction.description); // Output: 'This is a function with a property'
````
**Пример 2:**
````js
function trackCount() {
  if (!trackCount.count) {
      trackCount.count = 0;
  }
  trackCount.count++;
  console.log(`Function called ${trackCount.count} times.`);
}
trackCount(); // Outputs: Function called 1 times
trackCount(); // Outputs: Function called 2 times
trackCount(); // Outputs: Function called 3 times
````
Используйте свойства функции для хранения метаданных или настроек, связанных с функцией. Это повысит гибкость
и организованность вашего кода.

### **18. Геттеры и сеттеры объектов**

Геттеры и сеттеры — это методы, которые получают или устанавливают значение свойства объекта.
````js
const obj = {
  firstName: 'John',
  lastName: 'Doe',
  _age: 0, // Conventionally use an underscore for the backing property
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },
  set age(newAge) {
    if (newAge >= 0 && newAge <= 120) {
      this._age = newAge;
    } else {
      console.log('Invalid age assignment');
    }
  },
  get age() {
    return this._age;
  }
};

console.log(obj.fullName); // Outputs: 'John Doe'
obj.age = 30; // Setting the age using the setter
console.log(obj.age); // Outputs: 30

obj.age = 150; // Attempting to set an invalid age
// Outputs: 'Invalid age assignment'
console.log(obj.age); // Still Outputs: 30 (previous valid value remains)
````
Используйте геттеры и сеттеры для инкапсуляции внутреннего состояния объекта, чтобы обеспечить контролируемый способ
доступа к свойствам и изменения свойств.

### **19. Двойной восклицательный знак**

Оператор логического отрицания `!`, повторенный дважды, преобразует значение в его логический (булев) эквивалент.
````js
const value = 'abc';
const value1 = 42;
const value2 = '';
const value3 = null;
const value4 = undefined;
const value5 = 0;

console.log(!!value); // Outputs: true (truthy value)
console.log(!!value1); // Outputs: true (truthy value)
console.log(!!value2); // Outputs: false (falsy value)
console.log(!!value3); // Outputs: false (falsy value)
console.log(!!value4); // Outputs: false (falsy value)
console.log(!!value5); // Outputs: false (falsy value)
````
Использование `!!` для быстрого преобразования значений в булевы значения полезно в условных выражениях.

### **20. Объекты Map и Set**

`Map` и `Set` — это коллекции с уникальными свойствами. `Map` хранит пары ключ-значение, а `Set` — уникальные значения.

**Пример 1:**
````js
// Creating a Map
const myMap = new Map();

// Setting key-value pairs
myMap.set('key1', 'value1');
myMap.set(1, 'value2');
myMap.set({}, 'value3');

// Getting values from a Map
console.log(myMap.get('key1')); // Outputs: 'value1'
console.log(myMap.get(1)); // Outputs: 'value2'
console.log(myMap.get({})); // Outputs: undefined (different object reference)
````
**Пример 2:**
````js
// Creating a Set
const mySet = new Set();

// Adding values to a Set
mySet.add('apple');
mySet.add('banana');
mySet.add('apple'); // Duplicate value, ignored in a Set

// Checking size and values
console.log(mySet.size); // Outputs: 2 (only unique values)
console.log(mySet.has('banana')); // Outputs: true

// Iterating over a Set
mySet.forEach((value) => {
  console.log(value);
});
// Outputs:
// 'apple'
// 'banana'
````
Используйте `Map` для коллекций пар ключ-значение с любым типом данных в качестве ключей,
а `Set` — для коллекций уникальных значений. Это обеспечит эффективный способ управления данными.


# Примеры решений некоторых типовых задач

## Браузер

### **Переход на полноэкранный режим**

Чтобы перейти к полноэкранному режиму:
````js
function fullScreen() {
  const el = document.documentElement
  const rfs =
  el.requestFullScreen ||
  el.webkitRequestFullScreen ||
  el.mozRequestFullScreen ||
  el.msRequestFullscreen
  if (typeof rfs != "undefined" && rfs) {
      rfs.call(el)
  }
}  
fullScreen()
````
### **Выход из полноэкранного режима**

Чтобы выйти из полноэкранного режима:
````js
function exitScreen() {
  if (document.exitFullscreen) {
    document.exitFullscreen()
  } else if (document.mozCancelFullScreen) {
    document.mozCancelFullScreen()
  } else if (document.webkitCancelFullScreen) {
    document.webkitCancelFullScreen()
  } else if (document.msExitFullscreen) {
    document.msExitFullscreen()
  }
  if (typeof cfs != "undefined" && cfs) {
    cfs.call(el)
  }
}  
exitScreen()
````
### **Вывод страницы**

Чтобы вывести текущую страницу:
````js
window.print()
````
### **Изменение стиля выводимого контента**

Чтобы при выводе страницы изменить текущий макет:
````html
<style>
  /* Используйте @media print, чтобы настроить стиль вывода */
  @media print {
    .noprint {
      display: none;
    }
  }
</style>
<div class="print">print</div>
<div class="noprint">noprint</div>
````
### **Блокировка события закрытия**

Чтобы оградить пользователя от обновления или закрытия браузера, запустите событие `beforeunload`
(некоторые браузеры не кастомизируют текстовой контент):
````js
window.onbeforeunload = function() {  
  return 'Are you sure you want to leave the haorooms blog？';  
};
````
### **Запись экрана**

Чтобы сделать запись текущего экрана для ее передачи или загрузки:
````js
const streamPromise = navigator.mediaDevices.getDisplayMedia()
streamPromise.then(stream => {
  var recordedChunks = []; // записанные видеоданные 
  var options = { mimeType: "video/webm; codecs=vp9" }; // Установите формат кодирования
  var mediaRecorder = new MediaRecorder(stream, options); // Инициализируйте экземпляр MediaRecorder
  mediaRecorder.ondataavailable = handleDataAvailable; // Установите обратный вызов, когда данные будут доступны (конец записи экрана)
  mediaRecorder.start();
  // Фрагментация видео 
  function handleDataAvailable(event) {
    if (event.data.size > 0) {
      recordedChunks.push(event.data); // Добавление данных, event.data - объект BLOB
      download(); // Инкапсуляция в объект BLOB и загрузка
    }
  }
  // Загрузка файла
  function download() {
    var blob = new Blob(recordedChunks, {
      type: "video/webm"
    });
    // Видео можно загрузить здесь в бэкенд
    var url = URL.createObjectURL(blob);
    var a = document.createElement("a");
    document.body.appendChild(a);
    a.style = "display: none";
    a.href = url;
    a.download = "test.webm";
    a.click();
    window.URL.revokeObjectURL(url);
  }
})
````
### **Определение состояния горизонтального и вертикального экранов**

Чтобы оценить состояние горизонтального или вертикального экрана мобильного телефона:
````js
function hengshuping(){
  if (window.orientation === 180 || window.orientation === 0) {
    alert("Portrait state！")
  }
  if (window.orientation === 90 || window.orientation === -90) {
    alert("Landscape state！")
  }
}
window.addEventListener("onorientationchange" in window ? "orientationchange" : "resize", hengshuping, false);
````
### **Различение стилей для горизонтального и вертикального экранов**

Чтобы установить разные стили для горизонтального и вертикального экранов:
````html
<style>
@media all and (orientation: landscape) {
  body {
    background-color: #ff0000;
  }
}
@media all and (orientation: portrait) {
  body {
    background-color: #00ff00;
  }
}
</style>
````
### **Скрытие страницы вкладок**

Чтобы отслеживать события отображения и скрытия вкладок:
````js
const { hidden, visibilityChange } = (() => {
  let hidden, visibilityChange;
  if (typeof document.hidden !== "undefined") {
    // Поддержка версий Opera 12.10, Firefox 18 и выше
    hidden = "hidden";
    visibilityChange = "visibilitychange";
  } else if (typeof document.msHidden !== "undefined") {
    hidden = "msHidden";
    visibilityChange = "msvisibilitychange";
  } else if (typeof document.webkitHidden !== "undefined") {
    hidden = "webkitHidden";
    visibilityChange = "webkitvisibilitychange";
  }
  return {
    hidden,
    visibilityChange
  }
})();

const handleVisibilityChange = () => {
  console.log("currently hidden", document[hidden]);
};
document.addEventListener(
  visibilityChange,
  handleVisibilityChange,
  false
);
````
## Изображение

### **Предварительный просмотр локального изображения**

Чтобы перед загрузкой на сервер просмотреть полученное от клиента изображение:
````html
<div class="test">
  <input type="file" name="" id="">
  <img src="" alt="">
</div>

<script>
  const getObjectURL = (file) => {
    let url = null;
    if (window.createObjectURL !== undefined) { // базовый
      url = window.createObjectURL(file);
    } else if (window.URL !== undefined) { // webkit или chrome
      url = window.URL.createObjectURL(file);
    } else if (window.URL !== undefined) { // mozilla(firefox)
      url = window.URL.createObjectURL(file);
    }
    return url;
  }
  document.querySelector('input').addEventListener('change', (event) => {
    document.querySelector('img').src = getObjectURL(event.target.files[0])
  })
</script>
````
### **Предварительная загрузка изображений**

Чтобы предварительно загрузить большое количество изображений и избежать белого экрана:
````js
const images = []
function preloader(args) {
  for (let i = 0, len = args.length; i < len; i++) {
    images[i] = new Image()
    images[i].src = args[i]
  }
}
preloader(['1.png', '2.jpg'])
````
## JS

### **Создание строковых скриптов**

Чтобы преобразовать строку/строки в js-скрипт (этот метод отличается xss-уязвимостью, поэтому требует осмотрительности):
````js
const obj = eval('({ name: "jack" })')
// obj будет преобразован в object{ name: "jack" }
const v = eval('obj')
// v станет переменной obj
````
### **Разделение имен рекурсивной функции**

Чтобы после изменения имени рекурсивной функции изменить внутреннее имя функции
(`argument` — это внутренний объект функции, включающий все параметры, переданные в функцию,
а `arguments.callee` представляет имя функции):
````js
// Это базовая последовательность Фибоначчи  
function fibonacci (n) {
  const fn = arguments.callee
  if (n <= 1) return 1
  return fn(n - 1) + fn(n - 2)
}
````
## DOM-элементы

### **Проверка неявно определенного элемента**

Чтобы определить, появляется ли DOM-элемент в данный момент в представлении страницы, используйте `IntersectionObserver`:
````html
<style>
  .item {
    height: 350px;
  }
</style>

<div class="container">
  <div class="item" data-id="1">Invisible</div>
  <div class="item" data-id="2">Invisible</div>
  <div class="item" data-id="3">Invisible</div>
</div>

<script>
  if (window?.IntersectionObserver) {  
    let items = [...document.getElementsByClassName("item")]; // парсится как настоящий массив, также доступно Array.prototype.slice.call()
    let io = new IntersectionObserver(  
      (entries) => {  
        entries.forEach((item) => {  
          item.target.innerHTML =
            item.intersectionRatio === 1 // Коэффициент отображения элемента, когда он равен 1, элемент полностью виден, а когда он равен 0, то совершенно невидим.
              ? `Element is fully visible`
              : `Element is partially invisible`;
        });
      },
      {
        root: null,
        rootMargin: "0px 0px",
        threshold: 1, // Порог устанавливается равным 1, и функция обратного вызова срабатывает, только когда отношение достигает 1
      }
    );
    items.forEach((item) => io.observe(item));
  }  
</script>
````
### **Редактирование DOM-элемента**

Чтобы отредактировать DOM-элемент, кликните на него, как на текстовую область:
````html
<div contenteditable="true">here can be edited</div>
````
### **Мониторинг атрибутов элементов**
````html
<div id="test">test</div>
<button onclick="handleClick()">OK</button>

<script>
  const el = document.getElementById("test");
  let n = 1;
  const observe = new MutationObserver((mutations) => {
    console.log("attribute is changede", mutations);
  })
  observe.observe(el, {
    attributes: true
  });
  function handleClick() {
    el.setAttribute("style", "color: red");
    el.setAttribute("data-name", `${n++}`);
  }
  setTimeout(() => {
    observe.disconnect(); // наблюдение прекращается
  }, 5000);
</script>
````
### **Вывод DOM-элемента**

Чтобы в процессе разработки вывести DOM-элемент с внутренними атрибутами, используйте `console.dir`
(`console.log` часто выводит DOM-элемент, но не позволяет просмотреть его внутренние атрибуты):
````js
console.dir(document.body)
````
## Прочее

### **Активирование приложений**

Чтобы при разработке мобильного приложения открывать другие приложения, используйте следующие методы присвоения
`location.href`:
````html
<a href="tel:12345678910">phone</a>
<a href="sms:12345678910,12345678911?body=hello">android message</a>
<a href="sms:/open?addresses=12345678910,12345678911&body=hello">ios message</a>
<a href="wx://">ios message</a>

<style>
  code.hljs {
    font-size:14px;
  }
</style>
````


# Шаблоны реактивности

Реакция на вводимые пользователем данные, взаимодействие с серверами, ведение журналов, выполнение действий и т.д.
Все эти задачи включают обновления пользовательского интерфейса. Запросы Ajax, URL-адреса браузера и изменения навигации
делают каскадные изменения данных ключевым аспектом веб-разработки.

Реактивность ассоциируется с фреймворками, но можно многому научиться, внедряя реактивность на чистом JavaScript.
Мы можем смешивать и сопоставлять эти шаблоны, чтобы связать поведение с изменениями данных.

Применение основных шаблонов с помощью чистого JavaScript приведет к уменьшению количества кода и повышению
производительности веб-приложений, независимо от того, какой инструмент или структура используется.


### [PubSub Pattern (Publish Subscriber](#pubsub-pattern-publish-subscriber)

PubSub - один из самых фундаментальных шаблонов реактивности. Запуск события с помощью `publish()` позволяет любому
подписчику прослушивать это событие `subscribe()` и выполнять любую обработку, в отрыве от того, что запускает это событие.

````js
const pubSub = {
  events: {},
  subscribe(event, callback) {
    if (!this.events[event]) this.events[event] = [];
    this.events[event].push(callback);
  },
  publish(event, data) {
    if (this.events[event]) {
      this.events[event].forEach(callback => callback(data));
    }
  }
};

pubSub.subscribe('update', data => console.log(data));
pubSub.publish('update', 'Some update'); // Some update
````

Обратите внимание, что издатель _не знает_ о том, что его слушают, поэтому нет возможности отказаться от подписки
или убрать за собой с этой простой реализацией.


### [Custom Events: Native Browser API for PubSub](#custom-events-native-browser-api-for-pubsub)

В браузере имеется API для запуска и подписки на пользовательские события. Оно позволяет отправлять данные вместе с
пользовательскими событиями, используя `dispatchEvent`.

````js
const pizzaEvent = new CustomEvent("pizzaDelivery", {
  detail: {
    name: "supreme"
  }
});

window.addEventListener("pizzaDelivery", (e) => console.log(e.detail.name));
window.dispatchEvent(pizzaEvent);
````

Имеется возможность распространить пользовательские события на любой узел DOM. В примере кода используется глобальный
объект `window`, также известный как глобальная шина событий, поэтому все в нашем приложении может прослушивать данные
события и что-то делать с ними.

````html
<div id="pizza-store"></div>
````

````js
const pizzaEvent = new CustomEvent("pizzaDelivery", {
  detail: {
    name: "supreme"
  }
});

const pizzaStore = document.querySelector('#pizza-store');

pizzaStore.addEventListener("pizzaDelivery", (e) => console.log(e.detail.name));
pizzaStore.dispatchEvent(pizzaEvent);
````


### [Class Instance Custom Events: Subclassing EventTarget](#class-instance-custom-events-subclassing-eventtarget)

Мы можем создать подкласс EventTarget для отправки событий в экземпляр класса,
к которому наше приложение может привязаться:

````js
class PizzaStore extends EventTarget {
  constructor() {
    super();
  }
  
  addPizza(flavor) {
    // fire event directly on the class
    this.dispatchEvent(new CustomEvent("pizzaAdded", {
      detail: {
        pizza: flavor
      }
    }));
  }
}

const Pizzas = new PizzaStore();

Pizzas.addEventListener("pizzaAdded", (e) => console.log('Added Pizza:', e.detail.pizza));
Pizzas.addPizza("supreme");
````

Самое приятное в этом то, что события не происходят глобально на `window`. Можно запустить событие непосредственно
в классе, и любая сущьность приложения может подключить прослушиватели событий непосредственно к этому классу.


### [Observer Pattern](#observer-pattern)

Шаблон наблюдателя имеет ту же основную предпосылку, что и шаблон PubSub. Это позволяет «подписаться» на поведение.
И когда субъект запускает `notify`, он уведомляет всех подписчиков.

````js
class Subject {
  constructor() {
    this.observers = [];
  }
  
  addObserver(observer) {
    this.observers.push(observer);
  }
  
  removeObserver(observer) {
    const index = this.observers.indexOf(observer);
    if (index > -1) {
      this.observers.splice(index, 1);
    }
  }
  
  notify(data) {
    this.observers.forEach(observer => observer.update(data));
  }
}

class Observer {
  update(data) {
    console.log(data);
  }
}

const subject = new Subject();
const observer = new Observer(); 

subject.addObserver(observer);
subject.notify('Everyone gets pizzas!');
````

Основное отличие между от PubSub заключается в том, что субъект знает о своих наблюдателях и может их удалить.
Они не _полностью_ отделены друг от друга, как в PubSub.


### [Reactive Object Properties with Proxies](#reactive-object-properties-with-proxies)

Прокси в JavaScript могут быть основой для реагирования после установки или получения свойств объекта.

````js
const handler = {
  get: function(target, property) {
    console.log(`Getting property ${property}`);
    return target[property];
  },
  set: function(target, property, value) {
    console.log(`Setting property ${property} to ${value}`);
    target[property] = value;
    return true; // indicates that the setting has been done successfully
  }
};

const pizza = { name: 'Margherita', toppings: ['tomato sauce', 'mozzarella'] };
const proxiedPizza = new Proxy(pizza, handler);

console.log(proxiedPizza.name);  // Outputs "Getting property name" and "Margherita"
proxiedPizza.name = 'Pepperoni'; // Outputs "Setting property name to Pepperoni"
````

Когда вы получаете доступ к свойству `proxyedPizza` или изменяете его, оно записывает сообщение в консоль.
Но вы можете представить себе подключение любой функциональности.


### [Reactive Individual Properties: Object.defineProperty](#reactive-individual-properties-object-defineproperty)

Возможно сделать то же самое для определенного свойства, используя `Object.defineProperty`. Определить геттеры и сеттеры
для свойств и запускать код при доступе или изменении свойства.

````js
const pizza = {
  _name: 'Margherita' // Internal property
};

Object.defineProperty(pizza, 'name', {
  get: function() {
    console.log(`Getting property name`);
    return this._name;
  },
  set: function(value) {
    console.log(`Setting property name to ${value}`);
    this._name = value;
  }
});

// Example usage:
console.log(pizza.name);  // Outputs "Getting property name" and "Margherita"
pizza.name = 'Pepperoni'; // Outputs "Setting property name to Pepperoni"
````

Здесь мы используем `Object.defineProperty` для определения метода получения и установки свойства name объекта пиццы.
Фактическое значение хранится в приватном свойстве `_name`, а методы получения и установки предоставляют доступ
к этому значению.

`Object.defineProperty` более многословен, чем использование `Proxy`, особенно если вы хотите применить то же поведение
ко многим свойствам. Это мощный и гибкий способ определить индивидуальное поведение для отдельных свойств.


### [Asynchronous Reactive Data with Promises](#asynchronous-reactive-data-with-promises)

Давайте сделаем использование наблюдателей асинхронным! Таким образом, мы можем обновлять данные и запускать
несколько наблюдателей асинхронно.

````js
class AsyncData {
  constructor(initialData) {
    this.data = initialData;
    this.subscribers = [];
  }

  // Subscribe to changes in the data
  subscribe(callback) {
    if (typeof callback !== 'function') {
      throw new Error('Callback must be a function');
    }
    
    this.subscribers.push(callback);
  }
  
  // Update the data and wait for all updates to complete
  async set(key, value) {
    this.data[key] = value;
    
    // Call the subscribed function and wait for it to resolve
    const updates = this.subscribers.map(async (callback) => {
      await callback(key, value);
    });
    
    await Promise.allSettled(updates);
  }
}
````

Вот класс, который оборачивает объект данных и запускает обновление при изменении данных.


#### [Awaiting Our Async Observers](#awaiting-our-async-observers)

Допустим, мы хотим дождаться обработки всех подписок на наши асинхронные реактивные данные:

````js
const data = new AsyncData({ pizza: 'Pepperoni' });

data.subscribe(async (key, value) => {
  await new Promise(resolve => setTimeout(resolve, 500));
  console.log(`Updated UI for ${key}: ${value}`);
});

data.subscribe(async (key, value) => {
  await new Promise(resolve => setTimeout(resolve, 1000));
  console.log(`Logged change for ${key}: ${value}`);
});

// function to update data and wait for all updates to complete
async function updateData() {
  await data.set('pizza', 'Supreme'); // This will call the subscribed functions and wait for their promises to resolve
  console.log('All updates complete.');
}

updateData();
````

Функция `updateData` теперь асинхронна, поэтому мы можем дождаться разрешения всех подписанных функций, прежде чем
продолжить программу. Этот шаблон позволяет немного упростить манипулирование асинхронной реактивностью.

### Reactive Systems](#reactive-systems)

В основе популярных библиотек и фреймворков лежит множество более сложных реактивных систем: хуки в React,
сигналы в Solid, Observables в Rx.js и многое другое. Обычно они имеют одну и ту же основную предпосылку:
когда данные меняются, повторно отобразить компоненты или связанные фрагменты DOM.


#### [Observables (Pattern of Rx.js)](#observables-pattern-of-rx-js)

Observables и Observer Pattern — это не одно и то же, хотя это почти одно и то же слово.

Observables позволяют определить способ создания последовательности значений с течением времени.
Простой примитив Observable, который отправляет подписчикам последовательность значений, позволяя им реагировать
по мере создания этих значений.

````js
class Observable {
  constructor(producer) {
    this.producer = producer;
  }

  // Method to allow a subscriber to subscribe to the observable
  subscribe(observer) {
    // Ensure the observer has the necessary functions
    if (typeof observer !== 'object' || observer === null) {
      throw new Error('Observer must be an object with next, error, and complete methods');
    }

    if (typeof observer.next !== 'function') {
      throw new Error('Observer must have a next method');
    }

    if (typeof observer.error !== 'function') {
      throw new Error('Observer must have an error method');
    }

    if (typeof observer.complete !== 'function') {
      throw new Error('Observer must have a complete method');
    }

    const unsubscribe = this.producer(observer);

    // Return an object with an unsubscribe method
    return {
      unsubscribe: () => {
        if (unsubscribe && typeof unsubscribe === 'function') {
          unsubscribe();
        }
      }
    }
  }
}
````

Использование:

````js
// Create a new observable that emits three values and then completes
const observable = new Observable(observer => {
  observer.next(1);
  observer.next(2);
  observer.next(3);
  observer.complete();
  
  // Optional: Return a function to handle any cleanup if the observer unsubscribes
  return () => {
    console.log('Observer unsubscribed');
  };
});

// Define an observer with next, error, and complete methods
const observer = {
  next: value => console.log('Received value:', value),
  error: err => console.log('Error:', err),
  complete: () => console.log('Completed')
};

// Subscribe to the observable
const subscription = observable.subscribe(observer);

// Optionally, you can later unsubscribe to stop receiving values
subscription.unsubscribe();
````

Важнейшим компонентом Observable является метод `next()`, который отправляет данные наблюдателям. Метод `complete()`
когда поток Observable закрывается. И метод `error()`, когда что-то идет не так. Кроме того, должен быть метод
`subscribe()`, чтобы прослушивать изменения, и `unsubscribe()`, чтобы прекратить получение данных из потока.

Наиболее популярные библиотеки, использующие этот шаблон - это [Rx.js](https://rxjs.dev) и [MobX](https://mobx.js.org).


### ["Signals" (Pattern of SolidJS)](#signals-pattern-of-solidjs)

Подсказка Райана Карниато [Курс «Реактивность с SolidJS»](https://frontendmasters.com/courses/reactivity-solidjs/?utm_source=blog&utm_medium=website&utm_campaign=reactivity).

````js
const context = [];

export function createSignal(value) {
  const subscriptions = new Set();
  
  const read = () => {
    const observer = context[context.length - 1]
    if (observer) subscriptions.add(observer);
    return value;
  }
  
  const write = (newValue) => {
    value = newValue;
    
    for (const observer of subscriptions) {
      observer.execute()
    }
  }
  
  return [read, write];
}

export function createEffect(fn) {
  const effect = {
    execute() {
      context.push(effect);
      fn();
      context.pop();
    }
  }
  
  effect.execute();
}
````

Использование реактивной системы:

````js
import { createSignal, createEffect } from "./reactive";
const [count, setCount] = createSignal(0);

createEffect(() => {
  console.log(count());
}); // 0

setCount(10); // 10
````

Полный код [vanilla reactivity system](https://gist.github.com/1Marc/09e739caa6a82cc176ab4c2abd691814)
на примере, который Райан пишет в своем курсе.


### ["Observable-ish" Values (Frontend Masters)](#observable-ish-values-frontend-masters)

["Observable-ish" Values](https://github.com/FrontendMasters/observablish-values) представляет собой еще один вариант
реактивной системы в ванильном JavaScript. Это менее 100 строк кода! Смесь PubSub с возможностью вычислять значения
путем добавления результатов нескольких издателей (publishers) вместе.

Использование значений Observable-ish. Публикация изменений в функциях подписчика при изменении значений:

````js
const fn = function(current, previous) {}
const obsValue = ov('initial');

obsValue.subscribe(fn);    // subscribe to changes
obsValue();                // 'initial'
obsValue('initial');       // identical value, no change
obsValue('new');           // fn('new', 'initial')
obsValue.value = 'silent'; // silent update`
````

Изменение массивов и объектов не будет опубликовано, но их замена будет.

````js
const obsArray = ov([1, 2, 3]);
obsArray.subscribe(fn);
obsArray().push(4);          // silent update
obsArray.publish();          // fn([1, 2, 3, 4]);
obsArray([4, 5]);            // fn([4, 5], [1, 2, 3]);`
````

Передача функции кэширует результат как значение. Любые дополнительные аргументы будут переданы в функцию.
На любые наблюдатели, вызываемые внутри функции, можно будет подписаться, а обновления этих наблюдателей
будут пересчитывать значение.

Должны быть вызваны дочерние наблюдаемые объекты; простые ссылки игнорируются. Если функция возвращает обещание,
значение присваивается асинхронно после разрешения.

````js
const a = ov(1);
const b = ov(2);
const computed = ov(arg => { a() + b() + arg }, 3);
computed.subscribe(fn);
computed();             // fn(6)
a(2);                   // fn(7, 6)
````

## [Reactive Rendering of UI](#reactive-rendering-of-ui)

Вот несколько шаблонов для записи и чтения из DOM и CSS.


### [Render Data to HTML String Literals](#render-data-to-html-string-literals)

Пример рендеринга пользовательского интерфейса рецепта пиццы на основе данных.

````js
function PizzaRecipe(pizza) {
  return `<div class="pizza-recipe">
    <h1>${pizza.name}</h1>
    <h3>Toppings: ${pizza.toppings.join(', ')}</h3>
    <p>${pizza.description}</p>
  </div>`;
}

function PizzaRecipeList(pizzas) {
  return `<div class="pizza-recipe-list">
    ${pizzas.map(PizzaRecipe).join('')}
  </div>`;
}

var allPizzas = [
  {
    name: 'Margherita',
    toppings: ['tomato sauce', 'mozzarella'],
    description: 'A classic pizza with fresh ingredients.'
  },
  {
    name: 'Pepperoni',
    toppings: ['tomato sauce', 'mozzarella', 'pepperoni'],
    description: 'A favorite among many, topped with delicious pepperoni.'
  },
  {
    name: 'Veggie Supreme',
    toppings: ['tomato sauce', 'mozzarella', 'bell peppers', 'onions', 'mushrooms'],
    description: 'A delightful vegetable-packed pizza.'
  }
];

// Render the list of pizzas
function renderPizzas() {
  document.querySelector('body').innerHTML = PizzaRecipeList(allPizzas);
}

renderPizzas(); // Initial render

// Example of changing data and re-rendering
function addPizza() {
  allPizzas.push({
    name: 'Hawaiian',
    toppings: ['tomato sauce', 'mozzarella', 'ham', 'pineapple'],
    description: 'A tropical twist with ham and pineapple.'
  });

  renderPizzas(); // Re-render the updated list
}
  
// Call this function to add a new pizza and re-render the list
addPizza();
````

`addPizza` демонстрирует, как изменить данные, добавив в список новый рецепт пиццы, а затем повторно отрисовав список
чтобы отразить изменения.

Главный недостаток этого подхода - при каждом рендеринге стирается весь DOM. Вы можете более разумно обновлять
только те части DOM, которые изменяются с использованием такой библиотеки, как [lit-html](https://www.npmjs.com/package/lit-html)
([руководство по использованию lit-html](https://lit.dev/docs/libraries/standalone-templates)).

См. примеры других подходов в [репозитории Vanilla TodoMVC](https://github.com/1Marc/modern-todomvc-vanillajs) и
связанная [статья Vanilla TodoMVC](https://frontendmasters.com/blog/vanilla-javascript-todomvc/).


### [Reactive DOM Attributes: MutationObserver](#reactive-dom-attributes-mutationobserver)

Один из способов сделать DOM реактивным - добавлять и удалять атрибуты. Мы можем прослушивать изменения атрибутов,
используя API `MutationObserver`.

````js
const mutationCallback = (mutationsList) => {
  for (const mutation of mutationsList) {
    if (mutation.type !== "attributes" || mutation.attributeName !== "pizza-type") return;
    
    console.log('old:', mutation.oldValue)
    console.log('new:', mutation.target.getAttribute("pizza-type"))
  }
}

const observer = new MutationObserver(mutationCallback);
observer.observe(document.getElementById('pizza-store'), { attributes: true });
````

Теперь мы можем обновить атрибут `pizza-type` из любого места нашей программы, а сам элемент
привязан к обновлению этого атрибута!


### [Reactive Attributes in Web Components](#reactive-attributes-in-web-components)

Веб-компоненты предоставляют нативный способ прослушивания обновлений атрибутов и реагирования на них.

````js
class PizzaStoreComponent extends HTMLElement {
  static get observedAttributes() {
    return ['pizza-type'];
  }
  
  constructor() {
    super();
    
    const shadowRoot = this.attachShadow({ mode: 'open' });
    shadowRoot.innerHTML = `<p>${this.getAttribute('pizza-type') || 'Default Content'}</p>`;
  }
  
  attributeChangedCallback(name, oldValue, newValue) {
    if (name === 'my-attribute') {
      this.shadowRoot.querySelector('div').textContent = newValue;
      console.log(`Attribute ${name} changed from ${oldValue} to ${newValue}`);
    }
  }
}

customElements.define('pizza-store', PizzaStoreComponent);
````

````html
<pizza-store pizza-type="Supreme"></pizza-store>
````

````js
document.querySelector('pizza-store').setAttribute('pizza-type', 'BBQ Chicken!');
````

Это немного проще, но для использования этого API нам придется использовать веб-компоненты.


### [Reactive Scrolling: IntersectionObserver](#reactive-scrolling-intersectionobserver)

Мы можем подключить реактивность к элементам DOM, прокручивающимся в поле зрения.

````js
var pizzaStoreElement = document.getElementById('pizza-store');
var observer = new IntersectionObserver(function(entries, observer) {
  entries.forEach(function(entry) {
    if (entry.isIntersecting) {
      entry.target.classList.add('animate-in');
    } else {
      entry.target.classList.remove('animate-in');
    }
  });
});

observer.observe(pizzaStoreElement);
````

Вот пример [анимации прокрутки на CodePen](https://codepen.io/1Marc/pen/wvEKOEr)
в нескольких строках кода с использованием `IntersectionObserver`.


### [Animation & Game Loop: requestAnimationFrame](#animation-game-loop-requestanimationframe)

При разработке игр, Canvas, WebGL или других сайтах, анимации часто требуют записи в буфер, а затем записываем
результаты в заданном цикле, когда поток рендеринга становится доступным. Делаем это с помощью `requestAnimationFrame`.

````js
function drawStuff() { 
  // This is where you'd do game or animation rendering logic
}

// function to handle the animation
function animate() {
  drawStuff();
  requestAnimationFrame(animate); // Continually calls animate when the next render frame is available
}

// Start the animation
animate();
````

Этот метод используется в играх и во всем, что связано с рендерингом в реальном времени, для отрисовки сцены,
когда кадры станут доступны.


### [Reactive Animations: Web Animations API](#reactive-animations-web-animations-api)

Также возможно создавать реактивную анимацию с помощью `Web Animations` API. Здесь мы анимируем масштаб элемента,
положение и цвет с помощью API анимации.

````js
const el = document.getElementById('animatedElement');

// Define the animation properties
const animation = el.animate([
  // Keyframes
  { transform: 'scale(1)', backgroundColor: 'blue', left: '50px', top: '50px' },
  { transform: 'scale(1.5)', backgroundColor: 'red', left: '200px', top: '200px' }
], {
  // Timing options
  duration: 1000,
  fill: 'forwards'
});

// Set the animation's playback rate to 0 to pause it
animation.playbackRate = 0;

// Add a click event listener to the element
el.addEventListener('click', () => {
  // If the animation is paused, play it
  if (animation.playbackRate === 0) {
    animation.playbackRate = 1;
  } else {
    // If the animation is playing, reverse it
    animation.reverse();
  }
});
````

Реактивным в этом является то, что анимация может воспроизводиться относительно того места, где она расположена
когда происходит взаимодействие (в данном случае меняющее его направление). Стандартные CSS-анимации и переходы
не относительны их текущего положения.


### [Reactive CSS: Custom Properties and `calc`](#reactive-css-custom-properties-and-calc)

Наконец, мы можем написать реактивный CSS, объединив пользовательские свойства и `calc`.

````js
barElement.style.setProperty('--percentage', newPercentage);
````

In JavaScript, you can set a custom property value.

````css
.bar {
  width: calc(100% / 4 - 10px);
  height: calc(var(--percentage) * 1%);
  background-color: blue;
  margin-right: 10px;
  position: relative;
}
````

И в CSS мы теперь можем выполнять вычисления на основе процентов. Очень здорово, что мы можем добавлять расчеты
прямо в CSS и выполнять работу по стилизации, не сохраняя логику рендеринга в JavaScript.

К сведению: вы также можете прочитать эти свойства, если хотите внести изменения относительно текущего значения.

````js
getComputedStyle(barElement).getPropertyValue('--percentage');
````


# Selenium

## Преимущества совместного использования Selenium и JavaScript

* _Кросс-браузерность._ Позволяет беспроблемно запускать скрипты автоматизации в разных браузерах.
* _Интеграция с фреймворками для тестирования JavaScript._ Обеспечивает легкую интеграцию с такими фреймворками, как Mocha и Jest, для эффективного управления тестированием.
* _Мощная поддержка сообщества._ Активно действующие сообщества Selenium и JavaScript предоставляют доступ к обширной документации, обучающим руководствам и ресурсам.

## Установка Selenium WebDriver в JavaScript

Чтобы начать работу с Selenium WebDriver в JavaScript, устанавливаем пакет `Selenium WebDriver` с помощью `npm`.
После этого импортируем необходимые компоненты и создаем экземпляр `WebDriver` для управления веб-браузерами.
````shell
npm install selenium-webdriver
````
Импортируем необходимые компоненты:
````js
const { Builder, By, Key, until } = require('selenium-webdriver');
````
Создаем экземпляр `WebDriver`:

const driver = new Builder().forBrowser('chrome').build();

## Автоматизация браузера с помощьюSelenium

Selenium WebDriver позволяет разработчикам автоматизировать действия браузера, предоставляя богатый набор методов
и свойств. Вы можете выполнять следующие задачи: нажимать кнопки, заполнять формы, переходить между страницами
и извлекать данные из веб-элементов. Рассмотрим на простом примере, как открыть веб-сайт,
используя Selenium WebDriver:
````js
const { Builder } = require('selenium-webdriver');
const driver = new Builder().forBrowser('chrome').build();

async function openWebsite(url) {
  try {
    await driver.get(url);
    console.log('Website opened successfully!');
  } catch (error) {
    console.error('Failed to open website:', error);
  } finally {
    await driver.quit();
  }
}

openWebsite('https://www.example.com');
````
Пример автоматизации действий браузера:
````js
async function performBrowserActions() {
  try {
    await driver.get('https://www.example.com');
    await driver.findElement(By.id('myButton')).click();
    await driver.findElement(By.name('username')).sendKeys('John Doe');
  } catch (error) {
    console.error('Failed to perform browser actions:', error);
  } finally {
    await driver.quit();
  }
}
````
* С помощью `driver.get(url)` открываем определенный сайт.
* Посредством различных локаторов (`By.id`, `By.name` и т.д.) находим и взаимодействуем с веб-элементами.
* Выполняем такие действия, как нажатие кнопок и заполнение полей формы.

## Примеры фрагментов кода

### 1.  Переход по URL-адресу:
````js
driver.get('https://www.example.com');
````
### 2. Нажатие кнопки:
````js
const buttonElement = await driver.findElement(By.id('buttonId'));
await buttonElement.click();
                          -OR-  
driver.findElement(By.id('buttonId')).click();
````
### 3. Ввод значения:
````js
driver.findElement(By.name('inputName')).sendKeys('Text to input');
````
### 4. Очистка поля ввода:
````js
const inputElement = await driver.findElement(By.id('inputId'));
await inputElement.clear();
````
### 5. Выбор варианта из выпадающего списка:
````js
const dropdownElement = await driver.findElement(By.id('dropdownId'));
await dropdownElement.click();
const optionElement = await dropdownElement.findElement(By.xpath('//option\[text()="Option Value"\]'));
await optionElement.click();
````
### 6. Извлечение текста из элемента:
````js
const textElement = await driver.findElement(By.className('textClass'));
const extractedText = await textElement.getText();
````
### 7. Создание скриншота:
````js
const textElement = await driver.findElement(By.className('textClass'));
const extractedText = await textElement.getText();
````
### 8. Обработка предупреждающих уведомлений:
````js
const alert = await driver.switchTo().alert();
const alertText = await alert.getText();
await alert.accept(); // or alert.dismiss() to dismiss the alert
````
### 9. Максимальное развертывание окна браузера:
````js
await driver.manage().window().maximize();
````
### 10. Прокрутка к определенному элементу:
````js
const element = await driver.findElement(By.id('elementId'));
await driver.executeScript('arguments\[0\].scrollIntoView()', element);
````
### 11. Переключение на элемент `iframe`:
````js
const iframeElement = await driver.findElement(By.tagName('iframe'));
await driver.switchTo().frame(iframeElement);
````
### 12. Выполнение кода JavaScript на странице:
````js
await driver.executeScript('alert("Hello, Selenium!");');
````
### 13. Управление несколькими окнами или вкладками:
````js
const handles = await driver.getAllWindowHandles();
await driver.switchTo().window(handles[1]); // Переключение на второе окно/вкладку
````
### 14. Ожидание отображения элемента:
````js
const element = await driver.wait(until.elementLocated(By.id('elementId')));
````
### 15. Получение текущего URL-адреса:
````js
const currentUrl = await driver.getCurrentUrl();
console.log(currentUrl);
````
### 16. Проверка видимости элемента:
````js
const isVisible = await driver.findElement(By.id('elementId')).isDisplayed();
console.log(isVisible);
````
### 17. Обработка флажков (англ. checkbox):
````js
const checkboxElement = await driver.findElement(By.id('checkboxId'));
await checkboxElement.click(); // Переключение состояния флажка
````
### 18. Загрузка файла:
````js
const filePath = '/path/to/file.txt';  
const fileInput = await driver.findElement(By.id('fileInputId'));  
await fileInput.sendKeys(filePath);
````
## Распространенные случаи применения

* Автоматическое заполнение формы для оптимизации ввода повторяющихся данных.
* Веб-скрейпинг и извлечение данных для сбора ценной информации.
* Регрессионное тестирование. Проводится с целью подтвердить, что новые изменения не нарушают существующую функциональность.
* Кросс-браузерное тестирование для проверки корректного поведения в различных браузерах.
* Мониторинг веб-приложений для выявления проблем производительности и ошибок.
