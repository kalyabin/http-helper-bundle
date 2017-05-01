# Хелпер для работы с HTTP в Symfony

## CSRF Protection

После загрузки бандла все HTTP-запросы должны будут подписываться HTTP-заголовком X-CSRF-Token. Код токена для получения в сессии - csrf_protection.

Для отключения проверки по CSRF-токену контроллеры следует подписывать аннотацией @DisableCsrfProtection. Например:

```php
/**
 * @DisableCsrfProtection()
 */
class DefaultController extends Controller
{
    ...
}
```

или
```php
class DefaultController extends Controller
{
    /**
     * @DisableCsrfProtection()
     */
    public function actionIndex()
    {
    }
}
```

Для отключения проверки CSRF-заголовка (например, в тестах) следует в конфигурационном файле прописать:
```yml
parameters:
    csrf_protection_listener: HttpHelperBundle\Listener\IgnoreCsrfHeaderProtectionListener
```

## JSON Payload Request

Для того, чтобы не перепрописывать приложение для работы с запросами Content-type: application/json и записью в массив POST параметров из payload data установлен сервис JsonRequestListener. 
В случае JSON-запросов он заменяет POST-данные, данными из payload data.

## JSON Error Response

В случае RESTFul сервисов можно подключить сервис JsonErrorResponseListener:
```yml
services:
    ...
    json_error_response:
        class: HttpHelperBundle\Listener\JsonErrorResponseListener
        tags:
          - { name: kernel.event_listener, event: kernel.exception, method: onKernelException }
```

Данный сервис заменяет стандартные страницы ошибок на JSON-ответы.
