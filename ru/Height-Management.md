# Контракт Управление высотами - Требования

## Описание проблемы
Право на каждый участок содержит право на то, что находится под участком (недра) и над ним (воздух). В связи с этим воникает проблема учета, определения и изменения этих границ.
Нам нужен механизм определения границ участков без выполнения дорогостоящей геодезической съемки.

Так же нужен механизм честного определения границ для регулирования высотности застройки. Мы не хотим, чтобы была возможность построить десять небоскребов, которые расположены вплотную друг к другу. Жить в такой среде будет невозможно.

Для этих целей мы разрабатываем этот контракт.

## Цели контракта
- Сделать честную систему, позволяющую без геодезической съемки определить границы земельных участков, а так же регулировать высоту застройки, учитывая права всех владельцев участков, расположенных в одной области;
- Определить для нескольких, расположенных рядом участков, общую верхнюю и нижнюю границу путем голосования владельцев участков;
- Дать возможность пользователю продать часть верхней и нижней границы соседнему участку;
- Дать возможность пользователю купить часть верхней и нижней границы соседнего участка;

## Сценарий контракта
### Сценарий 1: Изменение границ с соседями
Значение верхней и нижней границы хранится в виде реестра в самом контракте.

| id участка | Нижняя граница, м. | Верхняя граница, м. |
| ---------- | -------------- | --------------- |
| sezu0h | 250 | 330 |
| sezu1m | 250 | 330 |
| sezu4g | 250 | 330 |
| geohash | h1 | h2 |

Здесь h1 - нижняя граница, выраженная в абсолютной координате "высота над уровнем моря" в метрах, а h2 - верхняя граница, в метрах над уровнем моря.

1. Пользователь создает заявку на изменение общей высоты для участков. В заявке пользователь указывает геохеш своего участка, новые значения границ и geohash'ы участков, владельцы которых должны проголосовать ( не ниже X min, где X min - это минимальное количество голосующих владельцев ).
Например:
- sexu0h;
- новая верхняя граница: 450;
- новая нижняя граница: 320;
- [ sezu0q, sezu0k, ... ];
2. Происходит определение, являются ли заданные участки соседями. Если все ок, результат фиксируются в контракте. Эти X участков - соседи.
Пусть ниже представлены текущие границы соседних участков:

| id участка | Нижняя граница, м. | Верхняя граница, м. |
| ---------- | -------------- | --------------- |
| sezu0h | 250 | 330 |
| sezu0q | 250 | 330 |
| sezu0k | 250 | 330 |

3. Каждый владелец участка может проголосовать. Если достигнут консенсус 66%, то происходит изменение высот. 1 участок = 1 голос.

Голосование: 

| id участка | Голос |
| ---------- | -------------- |
| sezu0h | За | 
| sezu0q | За | 
| sezu0k | Против | 

4. Каждый владелец, выполняя транзакцию, увеличивает счетчик голосов за принятие решения. Если значение счетчика превысило n ( где n - это 66% от количества голосующих ), то заявка считается выполненной.
Необходимый порог в n голосов был набран. Контракт внес изменения:

| id участка | Нижняя граница, м. | Верхняя граница, м. |
| ---------- | -------------- | --------------- |
| sezu0h | 250 | 400 |
| sezu0q | 250 | 400 |
| sezu0k | 250 | 400 |

### Сценарий 2: Покупка-продажа высоты
Возьмем пример из Сценария 1:

| id участка | Нижняя граница, м. | Верхняя граница, м. |
| ---------- | -------------- | --------------- |
| sezu0h | 320 | 450 |
| sezu0q | 320 | 450 |
| Остальные 22 участка | 320 | 450 |
| sezu0k | 320 | 450 |

1. Каждый владелец земли может создать заявку на покупку Верхней границы и нижней границы. В заявке пользователь фиксирует сколько он хочет приобрести по каждой границе, а так же переводит оплату в GALT на контракт.
Например:

| id участка | Увеличение по нижней границе, м. | Увеличение по верхней границе, м. | Цена, GALT|
| ---------- | -------------- | --------------- | --------------- |
| sezu0h | 0 | 500 | 10 000 |

Это означает, что пользователь в примере хочет увеличить высоту только своего участка на 500 метров относительно текущей. Например, чтобы построить небоскреб. И готов заплатить за это 10 000 GALT, которые он переводит на контракт.

2. Каждый из 8 владельцев соседних участков может ответить на заявку, указав id своего участка и высоту, которую он хочет продать.

| id участка | Продаваемая нижняя граница, м. | Продаваемая верхняя граница, м. |
| ---------- | -------------- | --------------- |
| sezu0q | 0 | 50 |

3. Контракт рассчитывает стоимость продаваемой высоты, переводит владельцу sezu0q 50 / 500 * 10000 = 1000 GALT и вносит изменение в Реестр учета высот:

| id участка | Нижняя граница, м. | Верхняя граница, м. |
| ---------- | -------------- | --------------- |
| sezu0h | 320 | 500 |
| sezu0q | 320 | 400 |
| Остальные 22 участка | 320 | 450 |
| sezu0k | 320 | 450 |

4. Для того, чтобы участки, на которых возможно высотное строительство, находились на большом расстоянии покупка высот возможна только у непосредственных 8 соседей. Таким образом, чтобы купить большую высоту, ее должны приобрести сначала эти 8 соседей, а потом перепродать.
5. Владелец участки может в любой момент отозвать заявку и вернуть свои GALT. В приведенном примере владелец sezu0h может вернуть 9000 GALT.

### Сценарий 3: Голосование трёх пользователей за увеличение высоты
Вводные данные трёх пользователей:

| Адрес владельца | id участка | Нижняя граница, м. | Верхняя граница, м. |
| --------------- | ---------- | -------------- | --------------- |
| 0xbfc...2fb | sezu0h | 250 | 330 |
| 0x3bc...2aa | sezu0k | 250 | 330 |
| 0x9f4...sjb | sezu0j | 250 | 330 |

1. Три пользователя имеют участки на горе, высота которой 320 метров над уровнем моря. Один из владельцев (0xbfc...2fb) решает увеличить общую верхнюю границу для постройки высокого здания. 
2. Он создает заявку о повышении общей верхней границы на 70 метров. В заявке пользователь указывает геохеш своего участка (sezu0h), новые значения границ (НГ: 230, ВГ: 400) и geohash'ы участков, владельцы которых должны проголосовать ( не ниже X min, где X min - это минимальное количество голосующих владельцев ).
Например:
- sexu0h;
- новая верхняя граница: 400;
- новая нижняя граница: 250;
- [ sezu0j, sezu0k ];
3. Три владельца участвуют в голосовании: 

| id участка | Голос |
| ---------- | -------------- |
| 0xbfc...2fb | sezu0h | За | 
| 0x3bc...2aa | sezu0q | За | 
| 0x9f4...sjb | sezu0k | Против | 

Голосование прошло успешно. Новая верхняя граница записана в реестр:

| Адрес владельца | id участка | Нижняя граница, м. | Верхняя граница, м. |
| --------------- | ---------- | -------------- | --------------- |
| 0xbfc...2fb | sezu0h | 250 | 400 |
| 0x3bc...2aa | sezu0k | 250 | 400 |
| 0x9f4...sjb | sezu0j | 250 | 400 |

4. Один из владельцев (0xbfc...2fb) решает увеличить верхнюю границу засчет покупки пространств. В заявке владелец фиксирует сколько он хочет приобрести по каждой границе, а так же переводит оплату в GALT на контракт.
Например:

| id участка | Увеличение по нижней границе, м. | Увеличение по верхней границе, м. | Цена, GALT|
| ---------- | -------------- | --------------- | --------------- |
| sezu0h | 0 | 50 | 1 000 |

5. Два владельца отвечают на заявку и продают по 50 только что полученных путем голосования метров:

| id участка | Продаваемая нижняя граница, м. | Продаваемая верхняя граница, м. |
| ---------- | -------------- | --------------- |
| sezu0k | 0 | 20 |
| sezu0j | 0 | 30 |

6. Контракт рассчитывает стоимость продаваемой высоты, переводит владельцу sezu0k 20 / 50 * 1000 = 400 GALT и sezu0j 30 / 50 * 1000 = 600 GALT и вносит изменение в Реестр учета высот:

| Адрес владельца | id участка | Нижняя граница, м. | Верхняя граница, м. |
| --------------- | ---------- | -------------- | --------------- |
| 0xbfc...2fb | sezu0h | 250 | 450 |
| 0x3bc...2aa | sezu0k | 250 | 380 |
| 0x9f4...sjb | sezu0j | 250 | 370 |

7. Владелец sezu0h создает голосование с двумя своими соседями еще раз, дабы увеличить общую верхнюю границу на 50 метров:

| id участка | Голос |
| ---------- | -------------- |
| 0xbfc...2fb | sezu0h | За | 
| 0x3bc...2aa | sezu0q | Против | 
| 0x9f4...sjb | sezu0k | Против | 

Голосование не принято.

7. Владелец sezu0h создает голосование с двумя своими соседями еще раз, дабы увеличить общую верхнюю границу на 100 метров:

| id участка | Голос |
| ---------- | -------------- |
| 0xbfc...2fb | sezu0h | За | 
| 0x3bc...2aa | sezu0q | За | 
| 0x9f4...sjb | sezu0k | За | 

Голосование проходит успешно и реестр высот меняется:

| Адрес владельца | id участка | Нижняя граница, м. | Верхняя граница, м. |
| --------------- | ---------- | -------------- | --------------- |
| 0xbfc...2fb | sezu0h | 230 | 550 |
| 0x3bc...2aa | sezu0k | 230 | 480 |
| 0x9f4...sjb | sezu0j | 230 | 470 |

## Спецификация контракта
### Структуры данных
- Реестр заявок на изменение высот: mapping <адрес инициатора, список голосующих участков, новая верхняя граница, новая нижняя граница>.

## Детальный Сценарий пользователя и описание интерфейса

## Ограничения для реализации на Solidity
- В solidity нет отрицательных чисел, соответственно необходимо обсудить нулевую нижнюю границу и насколько это должно быть глубоко под землей

## Ограничения для реализации на TypeScript

## Неоднозначные вопросы и ответы на них
### Кто должен голосовать за изменение границ? Как определять список участков, которые должны голосовать за изменение?
Есть 2 возможных варианта:
#### Вариант №1
Можно задавать радиус вокруг участка и исходя из него определять участки, которые должны голосовать.

| Плюсы | Минусы |
| ---------- | -------------- |
|  | Создает неопредленность. Участки могут иметь разные размеры. аже если мы будем использовать расстояние от границы участка, то это не является идеальным решением, так как соседний участок может быть огромного размера. |
|  | Сложность реализации. Нужно выполнять вычисления на геохешах в контракте. |

#### Вариант №2
Определить количество соседних участков, которые должны голосовать.

| Плюсы | Минусы |
| ---------- | -------------- |
| Так как здесь основная задача - достигнуть общественного консенсуса по высоте застройки, то нужно определить, какое количество владельцев должны участвовать в достижении этого консенсуса. Остановимся на использовании просто переменной количества соседей, которые должны проголосовать. У каждого участка есть 8 соседей, которые граничат с ним и еще 16 соседей, которые граничат с первыми восьмью. Примем за достаточный консенсус - 66% голосов от 24 соседних участков. Проблема остается для Владельцев участков, расположенных на границе, области, для которой происходят изменения. Они не голосуют, но могут получить негативные последствия в виде небоскреба за окном, если 24 владельца решили увеличить общую верхнюю границу до 1000 метров над уровнем моря. |  |

### Какое количество соседей должно участвовать в голосовании за участок? 
С точки зрения реализации on chain проще всего сделать выборку из 8-ми соседей

### Должен ли пользователь платить делегатам за получение дополнительных границ? 

Выбираем Вариант №2.
### Сколько нужно набрать голосов, чтобы сделать изменения? Как считать голоса?