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
