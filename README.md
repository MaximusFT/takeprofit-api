# TakeProfit Club: API and Tracking

##### Table of Contents
[Event API](#Event-api)
[Action: Регистрация клиента](#Action-регистрация-клиента)
[Action: Финансовая активность клиента](#Action-финансовая-активность-клиента)
[Action: Изменение состояния финансовой активности](#Action-изменение-состояния-финансовой-активности)
[Action: Любое другое событие на усмотрение Рекламодателя](#Action-любое-другое-событие-на-усмотрение-рекламодателя)
[Function API](#Function-api)
[Функция получения idMark](#Функция-получения-idmark)
[Функция проверки был ли заход с ресурса вебмастера на сайт рекламодателя](#Функция-проверки-был-ли-заход-с-ресурса-вебмастера-на-сайт-рекламодателя)

## Внедрение трекинговых модулей для Рекламодателя (Владельца)
Чтобы начать работать с партнерской программой вам необходимо произвести внедрение отслеживающего кода на всех страницах ваших ресурсов связанных с Оффером, в продвижении которого вы заинтересованы. Рекомендуется размещать код перед закрывающим `</head>`, но также код можно разместить в любой части HTML документа.

``` html
<script type="text/javascript" src="https://static.acrm.io/script/analytic.min.js"></script>
```
Данный код предназначен для сбора аналитики и отслеживани действий посетителей на ваших реусрсах.

### Дополнительная аналитика
Чтобы ваша аналитика была более полная и максимально информативная рекомендуем добавлять трекинг код на все ваши смежные реусрсы. Это позволит вам видеть перемещения ваших Клиентов по вашей сети сайтов.

## Внедрение трекинговых модулей для Вебмастеров (Агентов)
Если вы хотите работать с "красивыми urls" без рефссылки, то вам необходимо пройти несколько шагов. Первый шаг&nbsp;— это добавление своей площадки через панель Вебмастера, а затем — верификация. Для верификации домена вам неободимо добавить на свои ресурсы скрипт и после этого подтвердить в панели Вебмастера. Рекомендуется размещать код перед закрывающим `</head>`, но также код можно разместить в любой части HTML документа.

``` html
<script type="text/javascript" src="https://static.acrm.io/script/metrica.min.js"></script>
```

Для некоторых ресурсов может поднадобиться дополнительная верификация через HTML тег `<meta>`.
XXXXXX - код вашего ресурса, этот код будет для вас сгенирирован автоматически.

``` html
<meta name="takeprofit" content="XXXXXX"/>
```

## Event API
<dl>
  <dt>Type</dt>
  <dd>POST</dd>

  <dt>URL</dt>
  <dd>https://acrm.io/api/takeProfit/v1/event</dd>

  <dt>Поле: <code>eventName</code></dt>
  <dd>Данное поле заполняется опционально для удобства отслеживания событий по Клиентам<br>
      *Примеры*: 'Регистрация', 'Registation', 'Заявка на обратный звонок', 'Лид на покупку часов'...
  </dd>
</dl>

В зависимости от типа события нужно передавать следующие параметры:

### Action: Регистрация клиента
Данное событие нужно отправлять при совершении Посетителем (Клиентом) сайта действий, при которых он оставляет свои контактные данные (**Lead**). Это могу быть события "Регистрации" в системе, отправление заявки через контактную форму, оформление заявки на обратный звонок, подписка пользователя на подписку, оставление заявки на покупку Продукта (Услуги) и т.д... Эти первичное событие совершенное Посетителем при котором он становится Клиентом с какими-то контактными данными. В некоторых частных случаях это могут быть события за которые вы как рекламодатель будете выплачивать вознаграждение.

<dl>
  <dt>Event type</dt>
  <dd><code>register</code></dd>

  <dt>Не обязательные поля</dt>
  <dd><code>phone</code>, <code>surname</code></dd>
</dl>

Value | Type | Description
--------| ----- | ---
idMark | `string` | [Link to function](#Функция-получения-idmark)
email | `string` | Почта пользователя
phone | `string || array` | `string` если 1 телефон, `array` если несколько
name | `string` | Имя пользователя
surname | `string` | Фамилия пользователя
dealerId | `string` | Идентификатор Рекламодателя
advertiserClientId | `string` | id клиента в базе рекламодателя
eventType | `string` | `register`
eventName | `string` | заполняется рекламодателем
offerId | `string` | Идентификатор Офера
comment | `string` | Комментарий к Событию


**Example code**

**Упрощенное обращение через "пиксель"**

``` javascript
tpOptions = {
    offerId: 'XXXXXX',
    dealerId: 'XXXXXX',
    advertiserClientId: 'yourClientID',
    name: 'FirstName',
    email: 'userMail@mail.com'
}
tpe('register', 'Заявка на обратный звонок', tpOptions);
```

**Обращение через javascript**

``` javascript
$scope.testTPApi = function() {
    $http.post('https://acrm.io/api/takeProfit/v1/event', {
        eventType: 'register',
        eventName: 'Заявка на обратный звонок',
        idMark: 'qwerty1234uiopasdfghjklzxcvbnmqazxswedcv',
        dealerId: 'XXXXXX',
        offerId: 'XXXXXX',
        advertiserClientId: 'yourClientID',
        email: 'userMail@mail.com',
        name: 'FirstName'
    }).success(function(response) {
        $log.info('testTPApi', response);
    }).error(function(err, status) {
        $log.error(err, status);
    });
};
```

**Обращение через php**

```php
$tpData = array();
$tpData['eventType'] = 'register';
$tpData['eventName'] = 'Заявка на обратный звонок';
$tpData['idMark'] = 'qwerty1234uiopasdfghjklzxcvbnmqazxswedcv';
$tpData['dealerId'] = 'XXXXXX';
$tpData['offerId'] = 'XXXXXX';
$tpData['advertiserClientId'] = 'yourClientID';
$tpData['email'] = 'userMail@mail.com';
$tpData['name'] = 'FirstName';

$tpc = curl_init();
curl_setopt($tpc, CURLOPT_URL,"https://acrm.io/api/takeProfit/v1/event");
curl_setopt($tpc, CURLOPT_POST, 1);
curl_setopt($tpc,CURLOPT_POSTFIELDS, $tpData);
curl_setopt($tpc, CURLOPT_RETURNTRANSFER, true);
$tpServerOutput = curl_exec($tpc);
curl_close ($tpc);
```

**Response** : none

### Action: Финансовая активность клиента
Данное событие нужно отправлять при совершении оплаты Посетителем (Клиентом) услуг или товаров.

<dl>
  <dt>Event type</dt>
  <dd><code>order</code></dd>

  <dt>Не обязательные поля</dt>
  <dd><code>comment</code></dd>
</dl>

Value | Type | Description
--------| ----- | ---
idMark | `string` | [Link to function](#Функция-получения-idmark)
advertiserClientId | `string` | id клиента в базе рекламодателя
advertiserActionId | `string` | id события в базе рекламодателя
offerService | `string` | код оффер сервиса
currency | `integer` | валюта в формате (USD - доллар, UAH - гривна и прочее по стандарту iso 4217)
fullCost | `integer` | Стоимость товара
state | `integer` | 0 = не оплачен, 1 = оплачен
tpPercent | `integer` | процент от fullCost, который рекламодатель отдает TP
tpIncome | `integer` | сумма от fullCost, которую рекламодатель оплачивает TP
eventType | `string` | `order`
eventName | `string` | заполняется рекламодателем
comment | `string` | Комментарий к Событию
offerId | `string` | Идентификатор Офера


**Example code**

**Упрощенное обращение через "пиксель"**

``` javascript
tpOptions = {
    dealerId: 'XXXXXX',
    offerId: 'XXXXXX',
    offerService: 'XXXXXX',
    advertiserClientId: 'yourClientID',
    advertiserActionId: 'yourClientActionID',
    currency: 'uah',
    fullCost: '1200.5',
    state: '1',
    tpPercent: '7.5',
}
tpe('order', 'Оплата заказа №123', tpOptions);
```

**Обращение через javascript**

``` javascript
$scope.testTPApi = function() {
    $http.post('https://acrm.io/api/takeProfit/v1/event', {
        eventType: 'order',
        eventName: 'Оплата заказа №123',
        idMark: 'qwerty1234uiopasdfghjklzxcvbnmqazxswedcv',
        dealerId: 'XXXXXX',
        offerId: 'XXXXXX',
        offerService: 'XXXXXX',
        advertiserClientId: 'yourClientID',
        advertiserActionId: 'yourClientActionID',
        currency: 'uah',
        fullCost: '1200.5',
        state: '1',
        tpPercent: '7.5'
    }).success(function(response) {
        $log.info('testTPApi', response);
    }).error(function(err, status) {
        $log.error(err, status);
    });
};
```

**Обращение через php**

```php
$tpData = array();
$tpData['dealerId'] = 'XXXXXX';
$tpData['offerId'] = 'XXXXXX';
$tpData['offerService'] = 'XXXXXX';
$tpData['advertiserClientId'] = 'yourClientID';
$tpData['advertiserActionId'] = 'yourClientActionID';
$tpData['currency'] = 'uah';
$tpData['fullCost'] = '1200.5';
$tpData['state'] = '1';
$tpData['tpPercent'] = '7.5';

$tpc = curl_init();
curl_setopt($tpc, CURLOPT_URL,"https://acrm.io/api/takeProfit/v1/event");
curl_setopt($tpc, CURLOPT_POST, 1);
curl_setopt($tpc,CURLOPT_POSTFIELDS, $tpData);
curl_setopt($tpc, CURLOPT_RETURNTRANSFER, true);
$tpServerOutput = curl_exec($tpc);
curl_close ($tpc);
```


**Response** : none

### Action: Изменение состояния финансовой активности
Данное событие нужно отправлять если проданная Услуга (Продукт) была в течении периода Hold (ужержания) возвращена и Продажа была отменена.

<dl>
  <dt>Event type</dt>
  <dd><code>changeOrder</code></dd>

  <dt>Не обязательные поля</dt>
  <dd><code>comment</code></dd>
</dl>

Value | Type | Description
--------| ----- | ---
idMark | `string` | [Link to function](#Функция-получения-idmark)
advertiserClientId | `string` | id клиента в базе рекламодателя
advertiserActionId | `string` | id события в базе рекламодателя
eventType | `string` | `changeOrder`
eventName | `string` | заполняется рекламодателем
comment | `string` | Комментарий к Событию
offerId | `string` | Идентификатор Офера
state | `integer` | 0 = не оплачен, 1 = оплачен


**Example code**

В примере кода показывается изменение финансовой активности по Продаже, в состояние **не совершена**. Это могло произойти, к примеру, если у ваших услуг есть "Moneyback" или был возврат товара с последующим возвратом денег Клиенту.

**Упрощенное обращение через "пиксель"**

``` javascript
tpOptions = {
    offerId: 'XXXXXX',
    advertiserClientId: 'yourClientID',
    advertiserActionId: 'yourClientActionID',
    state: '0',
}
tpe('order', 'Оплата заказа №123', tpOptions);
```

**Обращение через javascript**

``` javascript
$scope.testTPApi = function() {
    $http.post('https://acrm.io/api/takeProfit/v1/event', {
        eventType: 'changeOrder',
        eventName: 'change',
        idMark: 'qwerty1234uiopasdfghjklzxcvbnmqazxswedcv',
        offerId: 'XXXXXX',
        advertiserClientId: 'yourClientID',
        advertiserActionId: 'yourClientActionID',
        state: '0',
    }).success(function(response) {
        $log.info('testTPApi', response);
    }).error(function(err, status) {
        $log.error(err, status);
    });
};
```

**Обращение через php**

```php
$tpData = array();
$tpData['offerId'] = 'XXXXXX';
$tpData['advertiserClientId'] = 'XXXXXX';
$tpData['advertiserActionId'] = 'XXXXXX';
$tpData['state'] = '0';

$tpc = curl_init();
curl_setopt($tpc, CURLOPT_URL,"https://acrm.io/api/takeProfit/v1/event");
curl_setopt($tpc, CURLOPT_POST, 1);
curl_setopt($tpc,CURLOPT_POSTFIELDS, $tpData);
curl_setopt($tpc, CURLOPT_RETURNTRANSFER, true);
$tpServerOutput = curl_exec($tpc);
curl_close ($tpc);
```

**Response** : none

### Action: Любое другое событие на усмотрение Рекламодателя
Данное событие нужно отправлять при совершении Посетителем (Клиентом) сайта каких-либо действий, котрые можно охарактеризовать как **Action**. К примеру, это могу быть события после "*Регистрации*" в системе, такие как "*Заполненный профиль*", "*Подтвержденный телефон*", "*Был созвон с клиентом и он подтвердил свое желание на приобритение продукта*" и т.д... Эти промежуточные события, которые вы как рекламодатель можете фиксировать в партнерке для отслежвиания действий Клиентов.

Если ваши запланированные события имеют более сложный путь реализации, свяжитесь с нами мы поможем настроить все правильно.
К примеру: **Регистрация** --> **Заполненный профиль** --> **Подтвержденный телефон по СМС** --> **Заявка на продукт** --> **Телефонное подтвержение на покупку** --> **Выставление счета (*но еще не оплата*)**

В этом примере нету события финансовой активности, чтобы показать необходимость только промежуточных Событий Клиента.

<dl>
  <dt>Event type</dt>
  <dd><code>event</code></dd>

  <dt>Не обязательные поля</dt>
  <dd><code>comment</code></dd>
</dl>


Value | Type | Description
--------| ----- | ---
idMark | `string` | [Link to function](#Функция-получения-idmark)
advertiserClientId | `string` | id клиента в базе рекламодателя
eventType | `string` | `event`
eventName | `string` | заполняется рекламодателем для отображения в статистике
comment | `string` | Комментарий к Событию
offerId | `string` | Идентификатор Офера

**Example code**

**Упрощенное обращение через "пиксель"**

``` javascript
tpOptions = {
    offerId: 'XXXXXX',
    comment: 'Произвольный комментарий',
    advertiserClientId: 'yourClientID'
}
tpe('event', 'Заполненный профиль', tpOptions);
```

**Обращение через javascript**

``` javascript
$scope.testTPApi = function() {
    $http.post('https://acrm.io/api/takeProfit/v1/event', {
        eventType: 'event',
        eventName: 'Заполненный профиль',
        idMark: 'qwerty1234uiopasdfghjklzxcvbnmqazxswedcv',
        offerId: 'XXXXXX',
        advertiserClientId: 'XXXXXX',
        comment: 'Some event for Adver'
    }).success(function(response) {
        $log.info('testTPApi', response);
    }).error(function(err, status) {
        $log.error(err, status);
    });
};
```

**Обращение через php**

```php
$tpData = array();
$tpData['eventType'] = 'event';
$tpData['eventName'] = 'Заполненный профиль';
$tpData['idMark'] = 'qwerty1234uiopasdfghjklzxcvbnmqazxswedcv';
$tpData['offerId'] = 'XXXXXX';
$tpData['advertiserClientId'] = 'yourClientID';
$tpData['comment'] = 'Произвольный комментарий';

$tpc = curl_init();
curl_setopt($tpc, CURLOPT_URL,"https://acrm.io/api/takeProfit/v1/event");
curl_setopt($tpc, CURLOPT_POST, 1);
curl_setopt($tpc,CURLOPT_POSTFIELDS, $tpData);
curl_setopt($tpc, CURLOPT_RETURNTRANSFER, true);
$tpServerOutput = curl_exec($tpc);
curl_close ($tpc);
```

**Response** : none


## Function API

### Функция получения idMark

``` javascript
function getIdMark() {
  var windowHistory = window.history.state, idMarkWindowHistory;
  if (windowHistory) idMarkWindowHistory = window.history.state.idMark;
  else idMarkWindowHistory = undefined;
  return localStorage.getItem('idMark') || sessionStorage.getItem('idMark') || getCookie('idMark') || idMarkWindowHistory || undefined;
}
```

### Функция проверки был ли заход с ресурса вебмастера на сайт рекламодателя
<dl>
  <dt>Type</dt>
  <dd>GET</dd>

  <dt>URL</dt>
  <dd>https://acrm.io/api/takeProfit/v1/isTPVisitor</dd>

  <dt>Response</dt>
  <dd>Object</dd>
</dl>


Value | Type | Description
--------| ----- | ---
idMark | `string` | [Link to function](#Функция-получения-idmark)
dealerCode | `string` | Идентификатор рекламодателя

**Example code**

``` javascript
$scope.testTPApi = function() {
    $http.get('/api/takeProfit/v1/isTPVisitor', {
        params: {
            dealerCode: 'XXXXXX',
            idMark: 'qwerty1234uiopasdfghjklzxcvbnmqazxswedcv'
        }
    }).success(function(response) {
        $log.info('testTPApi', response);
    }).error(function(err, status) {
        $log.error(err, status);
    });
};
```

**Response**:

В случае подтверждения захода с ресурса партнера на сайт рекламодателя передаются два параметра isTpVisitor = true и action - который содержит информацию откуда был совершен переход, куда и дату. В случае не подтверждения передается один параметр isTpVisitor = false

**If `true`**

``` javascript
testTPApi Object {
  isTpVisitor: true,
  dealerTrackings:
    date: "2016-06-23T08:55:34.094Z",
    localPage: "http://somepage.somedomen.com/?partner=XXXXXX",
    referrerPage: "http://altsomedomen.com/"
}
```

**If `false`**

``` javascript
testTPApi Object {
  isTpVisitor: false
};
```
