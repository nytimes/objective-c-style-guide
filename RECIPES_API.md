HTTP Endpoints
==============

Base URL: http://hyper-recipes.herokuapp.com

## Retrieve recipes

`GET`    /recipes

Sample:

```json
[
  {
    "id": 437,
    "name": "Strawberries and Cream Cake",
    "description": "Makes an elegant presentation without too much fuss.",
    "instructions": "eee",
    "favorite": false,
    "difficulty": "3.0",
    "created_at": "2014-09-29T10:43:00.072Z",
    "updated_at": "2014-11-26T11:53:58.451Z",
    "photo": {
      "url": "https://hyper-recipes.s3.amazonaws.com/uploads/recipe/photo/437/Strawberries_and_Cream_Cake.jpg"
    }
  }
]
```

## Create recipes

`POST`   /recipes

* **name:string** (obligatory field)
* **difficulty:integer** (obligatory field) [Valid values: 1, 2 and 3]
* description:text
* instructions:text
* favorite:boolean
* recipe[photo]:image == Multipart form request

Sample:

```json
{
  "recipe": {
    "name": "New name",
    "difficulty": 1
  }
}
```

You will get the image_url in the response.

## Update recipes

`PUT` or `PATCH`    /recipes/:id

* **name:string** (obligatory field)
* **difficulty:integer** (obligatory field) [Valid values: 1, 2 and 3]
* description:text
* instructions:text
* favorite:boolean
* recipe[photo]:image == Multipart form request

Sample:

```json
{
  "recipe": {
    "name": "New name",
    "difficulty": 1
  }
}
```

## Delete recipes

`DELETE` /recipes/:id
