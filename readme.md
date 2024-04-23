#  What is synch/async code in JavaScript ?

Synchronous means the code runs in a particular sequence of instructions given
in the program. Each instruction waits for the previous instruction to complete
its execution.
___
![](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Promise/promises.png)
___
![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTCxeW0ghBnIEjLIwg_XOqZZGpi5tDEK6M4YtlMeD5p-A&s)
___
``` JavaScript
var promiseCount = 0;
function testPromise() {
  var thisPromiseCount = ++promiseCount;

  var log = document.getElementById('log');
  log.insertAdjacentHTML('beforeend', thisPromiseCount +
      ') Запуск (запуск синхронного кода)
');

  // Создаём промис, возвращающее 'result' (по истечении 3-х секунд)
  var p1 = new Promise(
    // Функция разрешения позволяет завершить успешно или
    // отклонить промис
    function(resolve, reject) {
      log.insertAdjacentHTML('beforeend', thisPromiseCount +
          ') Запуск промиса (запуск асинхронного кода)
');
      // Это всего лишь пример асинхронности
      window.setTimeout(
        function() {
          // Промис исполнен!
          resolve(thisPromiseCount)
        }, Math.random() * 2000 + 1000);
    });

  // Указываем, что сделать с исполненным промисом
  p1.then(
    // Записываем в протокол
    function(val) {
      log.insertAdjacentHTML('beforeend', val +
          ') Промис исполнен (асинхронный код завершён)
');
    });

  log.insertAdjacentHTML('beforeend', thisPromiseCount +
      ') Промис создан (синхронный код завершён)
');
}
```
![](data:image/png;base64,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)
___
# Async/await
Существует специальный синтаксис для работы с промисами, который называется «async/await». Он удивительно прост для понимания и использования.
___
``` JavaScript
async function f() {

  let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("готово!"), 1000)
  });

  let result = await promise; // будет ждать, пока промис не выполнится (*)

  alert(result); // "готово!"
}

f();
```
Asynchronous programming provides opportunities for a program to continue
running other code while waiting for a long-running task to complete. The timeconsuming task is executed in the background while the rest of the code continues to
execute.
___
![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQcZ8w5EOesydLz3gJiZQmVr64bFYgww2G-3wO0HnbYJQ&s)
