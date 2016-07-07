# TakeProfit Club API

## Function API

### Function to get idMark

``` javascript
function getIdMark() {
  var windowHistory = window.history.state, idMarkWindowHistory;
  if (windowHistory) idMarkWindowHistory = window.history.state.idMark;
  else idMarkWindowHistory = undefined;
  return localStorage.getItem('idMark') || sessionStorage.getItem('idMark') || getCookie('idMark') || idMarkWindowHistory || undefined;
}
```

### Проверка был ли заход с ресурса вебмастера на сайт рекламодателя
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
idMark | `string` | [Link to function](#function-to-get-idmark)
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
  <dd>register</dd>

  <dt>Не обязательные поля</dt>
  <dd>phone</dd>
</dl>

Value | Type | Description
--------| ----- | ---
idMark | `string` | [Link to function](#function-to-get-idmark)
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
  <dd>order</dd>

  <dt>Не обязательные поля</dt>
  <dd>comment</dd>
</dl>

Value | Type | Description
--------| ----- | ---
idMark | `string` | [Link to function](#function-to-get-idmark)
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
  <dd>changeOrder</dd>

  <dt>Не обязательные поля</dt>
  <dd>comment</dd>
</dl>

Value | Type | Description
--------| ----- | ---
idMark | `string` | [Link to function](#function-to-get-idmark)
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
  <dd>event</dd>

  <dt>Не обязательные поля</dt>
  <dd>comment</dd>
</dl>


Value | Type | Description
--------| ----- | ---
idMark | `string` | [Link to function](#function-to-get-idmark)
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
