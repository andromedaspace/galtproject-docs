# Контракт Аукцион кандидатов в делегаты - Требования

* Epic issue https://github.com/andromedaspace/WIJG/issues/374
* UI Mindmap https://www.mindmeister.com/1117687549?t=OUJDFUPVcL

## Описание проблемы
Необходимо создать механизм предоставления права выдвигать свою кандидатуру для голосования за место в списке делегатов.
Этот механизм должен обеспечивать конкурентную среду и требовать большие денежные вложения со стороны `Участников проекта`, чтобы доказать свою состоятельность.

## Цели контракта
- Необходимо реализовать аукцион на право быть `Кандидатом в делегаты` базируясь на абстрактном классе [Аукциона Викри](Аукцион-Викри.md) со всеми его требованиями.
- Обеспечить корретную работу аукциона в условиях существования множества `Фондов`.

## Основные требования:
- Период `Аукциона Делагатов` должен быть настраиваемым.
- По окончанию периода `Аукциона Делагатов` - он должен автоматически начинаться заново.
- Минимальная ставка - 100 000 GALT.

## Сценарий контракта
### Сценарий 0: Формирование начального списка `Кандидатов в делегаты`.
1. Каждый `Владелец репутации` в течение 27 дней с момента деплоя контракта может выполнить метод comitReputation() контракта DelegateVoting и записать свою `Репутацию` в маппинг.

 Номер Раунда | Адрес `Владелца репутации` | `Репутация` |
| ---------- | -------------- | --------------- |
| 0 | 0x466...n12 | 1000 |
| 0 | 0x488...123 | 500 |
| 0 | 0x477...lol | 700 |

2. На 28 день - в течении дня `Владелец репутации`, которые ранее записывали свои `Репутации` должны зафиксировать свои результаты отправив транзанкции, записывать свою `Репутацию` в этот день нельзя.

Результат выполнения функции фиксирования своих результатов от нескольких `Владельцев репутации` на основе маппинга адресов `Владельцев репутации` к сумме `Репутации`:

| Номер Раунда | Место в топе `Владельца репутации` | Адрес `Владелца репутации` |
| ---------- | -------------- | --------------- |
| 0 | 1 | 0x466...n12 |
| 0 | 2 | 0x477...lol |
| 0 | 3 | 0x488...123 |
| 0 | 4 | 0x499...345 |
| 0 | 5 | 0x400...678 |
| 0 | 6 | 0x411...910 |
| 0 | 7 | 0x422...160 |

3. После 28 дней, по окончании нулевого раунда - любой может выполнить транзанкцию finishVoting, чтобы первые 12 `Владельцев репутации` с наибольшей `Репутации` сохранились в маппинг `Кандидатов в делегаты` Контракта DelegateVoting. За них можно будет голосовать.

### Сценарий 1: Аукцион на кандидатство
![image](https://user-images.githubusercontent.com/4842007/42165971-e4856c94-7e09-11e8-8fed-a182a2295922.png)

## Спецификация контракта
- Название класса: CandidateAuction
- Наследуется от абстрактного класса [Аукциона Викри](VickreyAuction.md)
- Добавлена связь с токеном DaoRegistry для передачи информации о том что пользователь успешно выиграл аукцион на кандидатство

## Допущения для DEMO:
- Данный функционал описан только для DEMO, конечное решение для Solidity мы найдем позже
