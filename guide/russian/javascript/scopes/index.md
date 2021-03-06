---
title: Scopes
localeTitle: Области применения
---
Если вы некоторое время программируете в JavaScript, вы, несомненно, сталкиваетесь с концепцией, известной как `scope` . Что такое `scope` ? Зачем вам тратить время на это?

Говоря программистом, `scope` - это **текущий контекст исполнения** . Смущенный? Давайте рассмотрим следующий фрагмент кода:
```
var foo = 'Hi, I am foo!'; 
 
 var baz = function () { 
  var bar = 'Hi, I am bar too!'; 
    console.log(foo); 
 } 
 
 baz(); // Hi, I am foo! 
 console.log(bar); // ReferenceError... 
```

Это простой пример, но он хорошо иллюстрирует то, что известно как _лексический охват_ . JavaScript, и почти любой другой язык программирования имеет _лексический охват_ . Существует еще один вид области действия, называемый _динамическим масштабом_ , но мы не будем обсуждать это.

Теперь термин _Lexical scope_ кажется фантастическим, но, как вы увидите, в принципе это очень просто. В Лексическом Области есть два вида областей: _глобальная область действия_ и _локальная область_ .

Прежде чем вводить первую строку кода в свою программу, для вас создается _глобальная область_ . Это содержит все переменные, которые вы заявляете в своей программе **вне любых функций** .

В приведенном выше примере переменная `foo` находится в глобальной области действия программы, в то время как переменная `bar` объявляется внутри функции и, следовательно, находится **в локальной области этой функции** .

Давайте разложим пример строки за строкой. Хотя вы можете быть смущены в этот момент, я обещаю, что к тому времени, когда вы это прочитаете, у вас будет гораздо лучшее понимание.

В строке 1 мы объявляем переменную `foo` . Здесь нет ничего необычного. Давайте назовем эту ссылку левым (LHS) ссылкой на `foo` , потому что мы назначаем значение `foo` и оно находится в левой части знака `equal` .

В строке 3 мы объявляем функцию и присваиваем ее переменной `baz` . Это еще одна ссылка LHS на `baz` . Мы присваиваем ему значение (помните, что функции тоже являются значениями!). Затем эту функцию вызывают в строке 8. Это RHS или правая ссылка на `baz` . Мы извлекаем значение `baz` , которое в этом случае является функцией и затем вызывает ее. Другая ссылка RHS на `baz` была бы, если бы мы присвоили ее значение другой переменной, например `foo = baz` . Это будет ссылка LHS на `foo` и ссылку RHS на `baz` .

Ссылки LHS и RHS могут показаться запутанными, но они важны для обсуждения области. Подумайте об этом так: ссылка LHS присваивает значение переменной, в то время как ссылка RHS возвращает значение переменной. Это всего лишь более короткий и удобный способ сказать «получение значения» и «присвоение значения».

Давайте теперь разбиваем, что происходит внутри самой функции.

Когда компилятор компилирует код внутри функции, он входит в **локальную область функции** .

В строке 4 объявляется переменная `bar` . Это ссылка LHS на `bar` . На следующей строке мы имеем ссылку RHS для `foo` внутри `console.log()` . Помните, что мы извлекаем значение `foo` , а затем передаем его в качестве аргумента в метод `console.log()` .

Когда у нас есть ссылка RHS на `foo` , компилятор ищет объявление переменной `foo` . Компилятор не находит его в самой функции или **локальной области функции,** поэтому она **поднимается на один уровень: в глобальную область** .

На данный момент вы, вероятно, думаете, что область действия имеет какое-то отношение к переменным. Это верно. Область может рассматриваться как контейнер для переменных. Все переменные, созданные в локальной области, доступны только в этой локальной области. Однако все локальные области доступа могут получить доступ к глобальной области. (Я знаю, что вы, вероятно, еще больше запутались прямо сейчас, но просто несите меня еще несколько абзацев).

Таким образом, компилятор переходит в глобальную область, чтобы найти ссылку LHS на переменную `foo` . Он находит один в строке 1, поэтому он извлекает значение из ссылки LHS, которая представляет собой строку: `'Hi, I am foo!'` , Эта строка отправляется в метод `console.log()` и выводится на консоль.

Компилятор завершил выполнение кода внутри функции, поэтому мы возвращаемся к строке 9. В строке 9 мы имеем ссылку RHS для переменной `bar` .

Теперь, `bar` был объявлен в локальном объеме `baz` , но есть ссылка RHS для `bar` в глобальном масштабе. Поскольку в глобальной области нет ссылки LHS для `bar` , компилятор не может найти значение для `bar` и выбрасывает ReferenceError.

Но вы можете спросить, может ли функция выглядеть вне себя для переменных или локальная область может заглянуть в глобальную область, чтобы найти ссылки LHS, почему глобальная область не может заглянуть в локальную область? Ну, вот как работает лексический охват!
```
... // global scope 
 var baz = function() { 
  ... // baz's scope 
 } 
 ... /// global scope 
```

Это тот же код сверху, который иллюстрирует область действия. Это создает иерархию, которая поднимается до глобальной области:

`baz -> global` .

Итак, если есть ссылка RHS для переменной внутри области `baz` , она может быть выполнена с помощью ссылки LHS для этой переменной в глобальной области. Но противоположное **не соответствует действительности** .

Что, если бы у нас была другая функция внутри `baz` ?
```
... // global scope 
 var baz = function() { 
  ... // baz's scope 
 
  var bar = function() { 
     ... // bar's scope. 
  } 
 
 } 
 ... /// global scope 
```

В этом случае иерархия или **цепочка областей** будут выглядеть так:

`bar -> baz -> global`

Любые ссылки RHS внутри локальной области `bar` могут быть заполнены ссылками LHS в глобальном масштабе или в области `baz` , но ссылка RHS в области `baz` не может быть заполнена ссылкой LHS в области `bar` .

**Вы можете перемещаться по цепочке видимости, а не вверх.**

Есть еще две важные вещи, которые вы должны знать о облаках JavaScript.

1.  Объекты объявляются функциями, а не блоками.
2.  Функции могут быть переданы по прямой ссылке, переменные не могут.

Наблюдайте (каждый комментарий описывает область действия в строке, на которой она написана):

\`\` \` // outer () здесь в области видимости, поскольку функции могут быть переданы по прямой ссылке
```
function outer() { 
 
    // only inner() is in scope here 
    // because only functions are forward-referenced 
 
    var a = 1; 
 
    //now 'a' and inner() are in scope 
 
    function inner() { 
        var b = 2 
 
        if (a == 1) { 
            var c = 3; 
        } 
 
        // 'c' is still in scope because JavaScript doesn't care 
        // about the end of the 'if' block, only function inner() 
    } 
 
    // now b and c are out of scope 
    // a and inner() are still in scope 
 
 } 
 
 // here, only outer() is in scope 
```

\`\` \`

# Рекомендации

1.  [Области и закрытие](https://github.com/getify/You-Dont-Know-JS/tree/master/scope%20%26%20closures) Кайла Симпсона. В нем подробно описывается, как работает механизм видимости, а также имеет поверхностное описание того, как работает компилятор JavaScript, поэтому, если вас это интересует, обязательно дайте ему прочитать! Это бесплатно на GitHub и можно купить у O'Reilly.
2.  [Секреты JavaScript ниндзя](https://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/1617292850/ref=pd_lpo_sbs_14_img_0?_encoding=UTF8&psc=1&refRID=YMC2TB2C0DFHTQ3V62CA) Джона Ресига и Медведя Бибо. Отличное руководство для более глубокого понимания JavaScript.