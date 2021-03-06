# API для взаимодействия с Мобильным терминалом 2can

API предназначено для интеграции вашего мобильного приложения и Мобильного терминала 2can.

## Основной сценарий использования

1. Ваше мобильное приложение передает Мобильному терминалу 2can назначение и сумму платежа
2. Мобильный терминал 2can проводит операцию с платежной картой
3. Мобильный терминал 2can возвращает результат операции и передает управление обратно вашему приложению

## Что здесь?

Здесь находится исходный код приложения, демонстрирующий возможности API.

Если на вашем телефоне установлен и активирован
Мобильный терминал 2can, то демо-приложение вызовет его для проведения операции. 

## Версии Мобильного терминала 2can, поддерживающие данное API

- 2can mPos https://play.google.com/store/apps/details?id=ru.toucan.merchant 
- 2can Касса https://play.google.com/store/apps/details?id=ru.toucan.ecommerce

## Контакты и ресурсы

- Сайт проекта: http://www.2can.ru
- Адрес службы поддержки: android-support@smart-fin.ru

## Описание API

### Отправка запроса на проведение платежа

#### Формирование и отправка запроса
```java
Intent intent = new Intent("ru.toucan.CALL_FOR_PAYMENT");
intent.putExtra("packageName", packageName);
intent.putExtra("Pin", Pin);
intent.putExtra("amount", amount);
intent.putExtra("desc", desc);
startActivityForResult(intent, PAYMENT_RESULT_CODE);
```


Имя параметра|Тип|Описание|Обязательный или нет
-------------|---|--------|-------------------
packageName | String | имя пакета вашего приложения | обязательный
amount | double | сумма платежа | обязательный
desc | String | назначение платежа | обязательный
Pin | String | код доступа к Мобильному терминалу, при отсутствии в параметрах запроса будет запрошен у пользователя | не обязательный

PAYMENT_RESULT_CODE - requestCode, см. описание методов [startActivityForResult]( https://developer.android.com/reference/android/app/Activity.html#startActivityForResult(android.content.Intent,%20int) ) и [onActivityResult]( https://developer.android.com/reference/android/app/Activity.html#onActivityResult(int,%20int,%20android.content.Intent) ) 

#### Получение ответа

Для получения ответа необходимо переопределить метод 
```java
void onActivityResult(int requestCode, int resultCode, Intent data) 
```

resultCode — результат выполнения операции

Код | Описание
----|---------
0 | Успешно
1 | Подпись успешно поставлена в очередь на отправку
3 | Сервер недоступен
4 | Интернет недоступен
6 | Системная ошибка сервиса
7 | Укажите сумму
8 | Укажите назначение
9 | Укажите packageName
13 | Ошибка. Вставьте карту чипом
14 | Ошибка проведения платежа. Попробуйте провести платеж заново
15 | Платеж не подтвержден картой
16 | Платеж отменен
17 | Некорректный ID устройства, выполните повторную активацию
20 | Ошибка сервера
21 | Ошибка сервера
101 | Отмена активации
200 | Отмена операции
400 | Превышено время ожидания. Сессия закрыта
1001 | Ридер не инициализирован
1002 | Неизвестный ридер
1003  | Операция отклонена картой
1004 | Карта заблокирована
1005 | Не принимайте эту карту к оплате
1006 | Платеж не может быть обработан
1010 | Ошибка регистрации ридера. Обратитесь в службу поддержки

