# MongoDB CheatSheet

----


## Table of Contents

* **[Nested Snippets](#nested-snippets)**
  * [Max/Min By](#maxmin-by)

----


## Speed Snippets


## Nested Snippets
Operandores sendo usados juntos para criar novas operações
### Max/Min By
Usando $max ou $min é possível retornar o elemento máximo/mínimo que satifaz a condição.
```json
{
   "$project":{
      "last_object_matched":{
         "$arrayElemAt":[
            "$objects_matched",
            {
               "$indexOfArray":[
                  "$objects_matched.updated_at",
                  {
                     "$max":"$objects_matched.updated_at"
                  }
               ]
            }
         ]
      }
   }
}
```

### Velocímetro
Retorna a quantidade de documentos atualizados na última hora
```javascript
db.collection.find({updated_at: {$gte: new Date(ISODate().getTime() - 1000 * 60 * 60)}}).count();
```

### Substring
Retornar uma substring com base em um caractere
```json
{
   "$project":{
      "user_email":{
         "$substr":[
            "$_id.user_email",
            0,
            {
               "$indexOfBytes":[
                  "$_id.user_email",
                  "@"
               ]
            }
         ]
      }
   }
}
```

### Agrupamento condicional
Usando o $cond dentro dos operadores de acumulação como o $sum e $addToSet é possível realizar um agrupamento de acordo com determinada condição.

```json
{
   "$group":{
      "_id":{
         "user_working":"$user_working"
      },
      "not_found":{
         "$sum":{
            "$cond":[
               {
                  "$eq":[
                     "$status",
                     "not_found"
                  ]
               },
               1,
               0
            ]
         }
      },
      "found":{
         "$sum":{
            "$cond":[
               {
                  "$in":[
                     "$status",
                     [
                        "done",
                        "in_production"
                     ]
                  ]
               },
               1,
               0
            ]
         }
      }
   }
}
```


## More Tips

* [CRUD MongoDB](https://gist.github.com/bradtraversy/f407d642bdc3b31681bc7e56d95485b6)
* [MongoDB Shell Commands Cheat Sheet](https://gist.github.com/michaeltreat/d3bdc989b54cff969df86484e091fd0c)
* [MongoDB Cheat Sheet](https://www.zuar.com/blog/mongodb-cheat-sheet/)
* [MongoDB - Cheat Sheet (Codecentric)](https://blog.codecentric.de/files/2012/12/MongoDB-CheatSheet-v1_0.pdf)
