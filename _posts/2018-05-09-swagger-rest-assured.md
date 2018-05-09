---
title: Генерация Rest-assured клиента из документации с помощью Swagger
date: 2018-05-09
description: 
categories: 
- Автотесты на API
tags:
- swagger
- rest-assured
---
[Swagger](https://swagger.org) - один из самых известных фреймворков для работы с REST API. Он поддерживает большое количество языков программирования, с помощью него можно документировать код и генерировать API клиенты из документации.
Из коробки swagger предоставляет генерацию следующих клиентов: Jersey1.x, Jersey2.x, OkHttp, Retrofit1.x, Retrofit2.x, Feign, RestTemplate, RESTEasy, Vertx, Google API Client Library for Java. Но попробовав большинство из них мы пришел к выводу, что для автоматизации тестирования ни один из них не подходит. Они не имеют достаточно точек расширения, ответ всегда мапится на ответ из документации, запрос не всегда удобно кастомизировать, что крайне важно для автоматизации тестирования. Также нет возможности сразу валидировать ответ. 
Хотелось получить клиент, который получался генерацией из документации и при этом отвечал всем требованиям, которому мы к нему предъявляли. **Поэтому мы написали свой клиент на основе Rest-assured и добавили его в [swagger-codegen](https://github.com/swagger-api/swagger-codegen).**

# Почему Rest-assured
[Rest-assured](http://rest-assured.io/) это библиотека, которая была создана для автоматизации тестирования, в основе нее Apache HTTP Client. В ней есть множество фич, которые облегчают написание тестов, такие как [fluent interface](https://ru.wikipedia.org/wiki/Fluent_interface), валидация ответа из коробки, маппинг и прочее. 

# Как это работает
Генерация клиентов в swagger-е происходит на основе шаблонизатора [mustache](http://mustache.github.io/). 
Использование шаблонов позволяет убрать ограничение на язык генерируемых файлов. Также можно использовать свои шаблоны, что дает возможность легко кастомизировать текущие клиенты, а также сильно упрощает добавление шаблонов для новых клиентов. 
Структура Java клиентов и файлы задаются в `JavaClientCodegen`. Поэтому все клиенты имееют примерно одну и ту же структуру: `ApiClient.class` - входная точка API. Там задается маппер и другие настройки клиента. Далее классы, соответствующие группе операции `{api}Api`. Он содержит конкретные методы для выполнения запросов и получения ответов.

# Плюсы и минусы генерации клиента из документации
## Плюсы
Swagger из коробки имеет интеграцию с [maven](https://github.com/swagger-api/swagger-codegen/blob/master/modules/swagger-codegen-maven-plugin/README.md), также есть интеграция с [gradle](https://github.com/int128/gradle-swagger-generator-plugin). Достаточно иметь ссылку на файл или url с документацией. Используя эти плагины, можно перед запуском тестов каждый раз перегенеривать код клиента, что гарантирует актуальность клиента и проверок. Т.е. если мы выпилили или что-то поменяли в API, это отразится на нашем клиенте. Весь служебный код, такой как методы для добавления различных параметров, объекты для ответов и тел запроса, будет генерироваться. Можно пойти дальше и сгенерировать с помощью [assertJ](http://joel-costigliola.github.io/assertj/) плагина матчеры по уже сгенеренным объектам ответа. Таким образом остается только код автотестов на основе сгенеренного клиента. 
 Следующиий плюс заключается в том, что код клиента и автотестов будет стандартизирован и его будет меньше, его будет проще ревьюить и поддерживать. В случае большого количества проектов это очень важно. 
 Также есть возможность иметь несколько разных клиентов для разных версий API. До этого достаточно просто добавить генерацию по еще одной спецификации в плагин (в случае maven - execution). 

## Минусы
К сожалению со сгенеренным кодом есть несколько проблем. Сгенеренный код невозможно изменить. Чтобы что-то поправить нужно менять либо документацию, либо принцип генерации клиента. Также существует проблемы с обратной совместимости. При схеме с генерацией клиента перед автотестами, мы ее не проверяем. Т.е. например, при изменении запроса с `GET /bar/foo` на `GET /foo/bar`, тесты на измененную версию API будут работать, а потребитель сервиса скорее всего нет. 

# Как попробовать 

Достаточно добавить maven plugin версии `2.4.0-SNAPSHOT` в `pom.xml`. При этом указать свою спецификацию в `inputSpec`.
```xml
<plugin>
     <groupId>io.swagger</groupId>
     <artifactId>swagger-codegen-maven-plugin</artifactId>
     <!--Артефакт должен быть в локальном репо-->
     <version>2.4.0-SNAPSHOT</version>
     <executions>
         <execution>
             <goals>
                 <goal>generate</goal>
             </goals>
             <configuration>
                 <!--Ваша спецификация-->
                 <inputSpec>http://petstore.swagger.io/v2/swagger.json</inputSpec>
                 <output>${project.build.directory}/generated-sources/swagger</output>
                 <language>java</language>
                 <configOptions>
                     <dateLibrary>java8</dateLibrary>
                 </configOptions>
                 <library>rest-assured</library>
                 <!--Нужна ли генерация тестов-->
                 <generateApiTests>true</generateApiTests>
                 <generateApiDocumentation>false</generateApiDocumentation>
                 <generateModelDocumentation>false</generateModelDocumentation>
                 <apiPackage>${default.package}.api</apiPackage>
                 <modelPackage>${default.package}.model</modelPackage>
                 <invokerPackage>${default.package}</invokerPackage>
             </configuration>
         </execution>
     </executions>
 </plugin>
```
Более детальное описание, как настроить плагин, можно найти в документации [swagger-codegen-maven-plugin](https://github.com/swagger-api/swagger-codegen/blob/master/modules/swagger-codegen-maven-plugin/README.md).

Необходимые зависимости для клиента:

```xml
<dependency>
    <groupId>io.swagger</groupId>
    <artifactId>swagger-annotations</artifactId>
    <version>${swagger-core-version}</version>
</dependency>
<dependency>
    <groupId>io.rest-assured</groupId>
    <artifactId>rest-assured</artifactId>
    <version>${rest-assured.version}</version>
</dependency>
<dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>${gson-version}</version>
</dependency>
<dependency>
    <groupId>io.gsonfire</groupId>
    <artifactId>gson-fire</artifactId>
    <version>${gson-fire-version}</version>
</dependency>
<dependency>
    <groupId>com.squareup.okio</groupId>
    <artifactId>okio</artifactId>
    <version>${okio-version}</version>
</dependency>
```

Если будут сгенерены тесты, необходимые добавить junit4:

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>${junit.version}</version>
</dependency>
```

Для генерации API клиента и тестов нужно выполнить `mvn clean compile`.

После генерации API клиент будет находится в папке `target`, его можно использовать для написания тестов. Шаблоны простейших тестов также будут сгенерены.

## Простейший тест
Простейший тест на junit4 выглядит так:

```java
private ApiClient api;
    
    @Before
    public void createApi() {
        api = ApiClient.api(ApiClient.Config.apiConfig().reqSpecSupplier(
                () -> new RequestSpecBuilder().setConfig(config().objectMapperConfig(objectMapperConfig().defaultObjectMapper(gson())))
                        .addFilter(new ErrorLoggingFilter())
                        .setBaseUri("http://petstore.swagger.io:80/v2")));
    }

    @Test
    public void someTest() {
        Map<String, Integer> inventory = api.store().getInventory().executeAs(validatedWith(shouldBeCode(SC_OK)));
        assertThat(inventory.keySet().size(), greaterThan(0));
    }
```