# Dummy_API
В данном разделе собраны коллеции Postman, связанные с тестированием сервиса DummyAPI, окружение к ним, а так же маинд-карта, дающая представление о структуре сервиса.

## Оглавление

- [Описание](https://github.com/lazaretov/Dummy_API#%D0%BE%D0%BF%D0%B8%D1%81%D0%B0%D0%BD%D0%B8%D0%B5)
- [Объект тестирования **POST**](https://github.com/lazaretov/Dummy_API#post)
    - [Запрос GET](https://github.com/lazaretov/Dummy_API#get-post-get-list)
    - [Запрос POST](https://github.com/lazaretov/Dummy_API#post)
- [Майнд-карта](https://github.com/lazaretov/Dummy_API#%D0%BC%D0%B0%D0%B9%D0%BD%D0%B4-%D0%BA%D0%B0%D1%80%D1%82%D0%B0)
- [Коллекции Postman](https://github.com/lazaretov/Dummy_API#%D0%BA%D0%BE%D0%BB%D0%BB%D0%B5%D0%BA%D1%86%D0%B8%D0%B8-postman)
    - [Окружение](https://github.com/lazaretov/Dummy_API/edit/main/README.md#%D0%BE%D0%BA%D1%80%D1%83%D0%B6%D0%B5%D0%BD%D0%B8%D0%B5environments)
    - [Тесты](https://github.com/lazaretov/Dummy_API/edit/main/README.md#%D1%82%D0%B5%D1%81%D1%82%D1%8B)
    - [Автотесты](https://github.com/lazaretov/Dummy_API/edit/main/README.md#%D0%B0%D0%B2%D1%82%D0%BE%D1%82%D0%B5%D1%81%D1%82%D1%8B)

## Описание
https://dummyapi.io/ представляет собой сервис для отработки навыков тестирования API. Для выполнения запросов необходима регистрация и получение app-id. В качестве объекта тестирования был выбран **POST**.

### POST
____
#### GET /post (get list)
Возвращает список публикаций, отсортированных по дате создания
- доступен query params для вывод определенной страницы
- доступен query params для вывод создателя

**List**
``` js
{
  data: Array(Model)
  total: number(total items in DB)
  page: number(current page)
  limit: number(number of items on page)
}
```

**Post Preview**
``` js
{
  id: string(autogenerated)
  text: string(length: 6-50, preview only)
  image: string(url)
  likes: number(init value: 0)
  tags: array(string)
  publishDate: string(autogenerated)
  owner: object(User Preview)
}
```
____
#### POST /post/create (create post)
Создаёт новую публикацию пользователя и возврощает информацию о публикации.

**Request Body**
owner и text обязательные поля.

``` js
{
  text: string(length: 6-50, preview only)
  image: string(url)
  likes: number(init value: 0)
  tags: array(string)
  owner: string(User id)
}
```

**Response Body**

``` js
{
id: string(autogenerated)
text: string(length: 6-1000)
image: string(url)
likes: number(init value: 0)
link: string(url, length: 6-200)
tags: array(string)
publishDate: string(autogenerated)
owner: object(User Preview)
}
```
____
## Майнд-карта
Представляет собой набор тестов для объекта **POST**

![Майнд-карта](https://i.ibb.co/S7m59Vq/Dummy-API.png "Майнд-карта")

Так же майнд-карта доступна по ссылке https://github.com/lazaretov/Dummy_API/blob/main/Dummy_API.pdf

____
## Коллекции Postman

### [Окружение/Environments](https://github.com/lazaretov/Dummy_API/blob/main/Local.postman_environment.json)

Окружение и локальные переменные, использованные при выполнении запросов.
____
### [Тесты](https://github.com/lazaretov/Dummy_API/blob/main/DummyAPI_post.postman_collection.json)

- Комбинации позитивных тестов для query params
- Негативные тест для query params
- Комбинации позитивных тестов для **POST /post/create (create post)**
- Негативные тесты для **POST /post/create (create post)**
____
### [Автотесты](https://github.com/lazaretov/Dummy_API/blob/main/Snipetts%2Bchai_post.postman_collection.json)

Автотесты Postman с использованием встроенных сниппетов, а так же библиотеки [chai.js](https://www.chaijs.com/api/bdd/). Проверяется как минимум код ответа и тело ответа на соответствие требованиям.

Например:

**Get post by id**

``` js
var jsonData = pm.response.json();

pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Status code name has OK", function () {
    pm.response.to.have.status("OK");
});

pm.test("Check body id", function () {
    pm.expect(jsonData.id).to.eql(pm.collectionVariables.get("postID"));
});

pm.test("Check body image is string", function () {
    pm.expect(jsonData.image).to.be.a('string');
});

pm.test("Check body likes", function () {
    pm.expect(jsonData.likes).to.eql(0);
});

pm.test("Check body link is string", function () {
    pm.expect(jsonData.link).to.be.a('string');
});

pm.test("Check body tags is array", function () {
    pm.expect(jsonData.tags).to.be.an('array');
});

pm.test("Check body text", function () {
    pm.expect(jsonData.text).to.eql("Этот текст написан чтобы проверить, что он тут появляется");
});

pm.test("Check body publishDate is string", function () {
    pm.expect(jsonData.publishDate).to.be.a('string');
});

pm.test("Check body publishDate is string", function () {
    pm.expect(jsonData.updatedDate).to.be.a('string');
});

pm.test("Check body owner is object", function () {
    pm.expect(jsonData.owner).to.be.an('object');
});

pm.test("Check body owner firstName is string", function () {
    pm.expect(jsonData.owner.firstName).to.be.a('string');
});
```
____
