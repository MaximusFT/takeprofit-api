# TakeProfit Club: API and Tracking

function tpe(eventType, eventName, options, callback) {
    if (options) {
        options.eventType = eventType || options.eventType;
        options.eventName = eventName || options.eventName;
        options.idMark = getIdMark();

        $.ajax({
            url: apiServer + '/api/takeProfit/v1/event',
            type: 'POST',
            dataType: 'json',
            data: options,
            crossDomain: true,
            success: function(response) {
                if (callback) {
                    callback(null, response);
                }
            },
            error: function(err) {
                if (callback) {
                    callback(err);
                }
            }
        });
    } else {
        callback(new Error('Empty options object not supported'));
    }
};
window.tpe = tpe;

## Внедрение трекинговых модулей для Рекламодателя (Владельца)
Чтобы начать работать с партнерской программой вам необходимо произвести внедрение отслеживающего кода на всех страницах ваших ресурсов связанных с Оффером, в продвижении которого вы заинтересованы. Рекомендуется размещать код перед закрывающим `</head>`, но также код можно разместить в любой части HTML документа.

``` html
<script type="text/javascript" src="https://static.acrm.io/script/analytic.min.js"></script>
```
Данный код предназначен для сбора аналитики и отслеживани действий посетителей на ваших реусрсах.

### Дополнительная аналитика
Чтобы ваша аналитика была более полная и максимально информативная рекомендуем добавлять трекинг код на все ваши смежные реусрсы. Это позволит вам видеть перемещения ваших Клиентов по вашей сети сайтов.

## Внедрение трекинговых модулей для Вебмастеров (Агентов)
Если вы хотите работать с "красивыми urls" без рефссылки, то вам необходимо пройти несколько шагов. Первый шаг — это добавление своей площадки через панель Вебмастера, а затем — верификация. Для верификации домена вам неободимо добавить на свои ресурсы скрипт и после этого подтвердить в панели Вебмастера. Рекомендуется размещать код перед закрывающим `</head>`, но также код можно разместить в любой части HTML документа.

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
</dl>

В зависимости от типа события нужно передавать следующие параметры:

### Action: Регистрация клиента

<dl>
  <dt>Event type</dt>
  <dd><code>register</code></dd>

  <dt>Не обязательные поля</dt>
  <dd><code>phone</code></dd>
</dl>

Value | Type | Description
--------| ----- | ---
idMark | `string` | [Link to function](#Функция получения idMark)
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

``` javascript
$scope.testTPApi = function() {
    $http.post('/api/takeProfit/v1/event', {
        eventType: 'register',
        eventName: 'Register',
        idMark: 'qwerty1234uiopasdfghjklzxcvbnmqazxswedcv',
        offerId: 'XXXXXX',
        advertiserClientId: 'XXXXXX',
        dealerId: 'XXXXXX',
        email: 'userMail@mail.com',
        name: 'userName'
    }).success(function(response) {
        $log.info('testTPApi', response);
    }).error(function(err, status) {
        $log.error(err, status);
    });
};
```

**Response** : none

### Action: Финансовая активность клиента

<dl>
  <dt>Event type</dt>
  <dd><code>order</code></dd>

  <dt>Не обязательные поля</dt>
  <dd><code>comment</code></dd>
</dl>

Value | Type | Description
--------| ----- | ---
idMark | `string` | [Link to function](#Функция получения idMark)
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

``` javascript
$scope.testTPApi = function() {
    $http.post('/api/takeProfit/v1/event', {
        eventType: 'order',
        eventName: 'Order',
        idMark: 'qwerty1234uiopasdfghjklzxcvbnmqazxswedcv',
        offerService: 'XXXXXX',
        currency: 'uah',
        fullCost: '20000',
        advertiserActionId: 'XXXXXX',
        state: '1',
        tpPercent: '1.5',
        offerId: 'XXXXXX',
        advertiserClientId: 'XXXXXX',
        dealerId: 'XXXXXX'
    }).success(function(response) {
        $log.info('testTPApi', response);
    }).error(function(err, status) {
        $log.error(err, status);
    });
};
```

**Response** : none

### Action: Изменение состояния финансовой активности

<dl>
  <dt>Event type</dt>
  <dd><code>changeOrder</code></dd>

  <dt>Не обязательные поля</dt>
  <dd><code>comment</code></dd>
</dl>

Value | Type | Description
--------| ----- | ---
idMark | `string` | [Link to function](#Функция получения idMark)
advertiserClientId | `string` | id клиента в базе рекламодателя
advertiserActionId | `string` | id события в базе рекламодателя
eventType | `string` | `changeOrder`
eventName | `string` | заполняется рекламодателем
comment | `string` | Комментарий к Событию
offerId | `string` | Идентификатор Офера
state | `integer` | 0 = не оплачен, 1 = оплачен


**Example code**

``` javascript
$scope.testTPApi = function() {
    $http.post('/api/takeProfit/v1/event', {
        eventType: 'changeOrder',
        eventName: 'change',
        idMark: 'qwerty1234uiopasdfghjklzxcvbnmqazxswedcv',
        advertiserActionId: 'XXXXXX',
        state: '0',
        offerId: 'XXXXXX',
        advertiserClientId: 'XXXXXX',
    }).success(function(response) {
        $log.info('testTPApi', response);
    }).error(function(err, status) {
        $log.error(err, status);
    });
};
```

**Response** : none

### Action: Любое другое событие на усмотрение Рекламодателя

<dl>
  <dt>Event type</dt>
  <dd><code>event</code></dd>

  <dt>Не обязательные поля</dt>
  <dd><code>comment</code></dd>
</dl>


Value | Type | Description
--------| ----- | ---
idMark | `string` | [Link to function](#Функция получения idMark)
advertiserClientId | `string` | id клиента в базе рекламодателя
eventType | `string` | `event`
eventName | `string` | заполняется рекламодателем для отображения в статистике
comment | `string` | Комментарий к Событию
offerId | `string` | Идентификатор Офера

**Example code**

``` javascript
$scope.testTPApi = function() {
    $http.post('/api/takeProfit/v1/event', {
        eventType: 'event',
        eventName: 'event',
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
idMark | `string` | [Link to function](#Функция получения idMark)
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
