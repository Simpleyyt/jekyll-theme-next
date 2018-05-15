---
title: Allure аттачи для Rest-assured, Retrofit и Apache HTTP Client
date: 2018-05-05
description: 
categories: 
- Автотесты на API
tags:
- java
- allure
- httpclient
- okhttp
- rest-assured
---
Что должно быть в Allure отчете для API теста? 
Для тестов на web интерфейс большую ценность предоставляет скриншот браузера или видео. Этого достаточно, чтобы понять в чем проблема и воспроизвести ее в случае необходимости. 
Для тестов на API это запрос и ответ со всеми подробностями: стартовой строкой, заголовком и телом, в хорошо читаемом виде. Кроме того, хорошо бы иметь способ быстро выполнить этот запрос. **Все это реализовано нами в [allure-java](https://github.com/allure-framework/allure-java).**

# Аттачи из коробки
Мы сделали поддержку аттачей для отчета для популярных http клиентов в [allure-java](https://github.com/allure-framework/allure-java):
 * [rest-assured](https://github.com/allure-framework/allure-java#rest-assured), 
 * [okhttp](https://github.com/allure-framework/allure-java#okhttp) 
 * httpclient
 
После подключения фильтра, вы получите два простейших аттача.

## Стандартный аттач запроса
![Alt text](/images/2018-05-05-request-attachment.jpg)

## Стандартный аттач ответа
![Alt text](/images/2018-05-05-response-attachment.jpg)

# Как это работает
Большинство API клиентов поддерживает фильтры. Они нужны для логирования, добавления кастомной аунтификации или любого другого изменения и получения запроса до его фиксации, а также получения и изменения ответа до валидации. 
Через механизм фильтров мы получаем готовый запрос и сам ответ для http клиентов. После этого извлекаем стартовую строку, заголовки и тело, которые передаем в соответствующий builder класс `HttpRequestAttachment` или `HttpResponseAttachment`, попутно сформировав `curl`, чтобы можно было легко запрос воспроизвести. 
Аттачи представляют собой html-страницы, который рендерится из [FreeMarker](https://freemarker.apache.org/) шаблонов и данных которые мы получили. Подробнее как это происходит можно посмотреть в модуле `allure-attachment`. 

# Кастомизация аттачей
Использование шаблонизатора позволяет гибко настраивать аттач на любой вкус. Для этого достаточно добавить новые шаблоны в Ваш проект в папку `resources/tpl`, либо указать путь до шаблона прямо в фильтре.
Добавим немного css стилей и подцветки кода. 

## Мой аттач запроса
![Alt text](/images/2018-05-05-custom-request-attachment.jpg)

## Мой аттач ответа
![Alt text](/images/2018-05-05-custom-response-attachment.jpg)

Их можно найти в папке [examples](https://github.com/allure-framework/allure-java/tree/master/examples/rest-assured/src/test/resources/tpl).

Также можно добавить шаг, к которому будет прикрепляться аттачи. Для этого достаточно отнаследоваться от фильтра. 
Например для Rest-assured, такой фильтр будет выглядеть так:

```java
public class MyAllureRestAssured extends AllureRestAssured {
    public MyAllureRestAssured() {
    }

    public Response filter(FilterableRequestSpecification requestSpec, FilterableResponseSpecification responseSpec, FilterContext filterContext) {
        AllureLifecycle lifecycle = Allure.getLifecycle();
        lifecycle.startStep(UUID.randomUUID().toString(), (new StepResult()).withStatus(Status.PASSED).withName(String.format("%s: %s", requestSpec.getMethod(), requestSpec.getURI())));

        Response response;
        try {
            response = super.filter(requestSpec, responseSpec, filterContext);
        } finally {
            lifecycle.stopStep();
        }

        return response;
    }
}
```

Получим в итоге:
![Alt text](/images/2018-05-05-custom-step.jpg)
