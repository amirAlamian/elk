# elk

elk commands
to <span style="color:cyan"> create </span> an <span style="color:lightgreen">index </span> use this code

```json
PUT /index_name
{
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 0
  },
  "mappings": {
    "dynamic": false,
    "properties": {
      "name": {
        "type": "text",
        "analyzer": "english"
      },
      "lastName": {
        "type": "text",
        "analyzer": "whitespace"
      },
      "age": {
        "type": "integer"
      }
    }
  }
}

```

to <span style="color:cyan">test </span> and <span style="color:lightgreen">Analyzer</span>, use this code.

```json
GET _analyze
{
  "analyzer": "english",
  "text":"i you they he she we "

}
```

<h2 style="color:lightgrey">Update</h2>

to <span style="color:cyan"> update </span>an <span style="color:lightgreen">document </span>

```json
post amir/_update/id_of_document
{
  "doc":{
  "age":40,
  "email":"alinozari@gmail.com"

  }
}
```

<hr>

to <span style="color:cyan"> replace </span>an <span style="color:lightgreen">document </span>

```json
PUT amir/_doc/id_of_document
{
  "doc":{
  "age":40,
  "email":"alinozari@gmail.com",
  "phoneNumber":9121364728,
  "age":40,
  }
}
```

<hr>
to  <span style="color:cyan"> update </span>all <span style="color:lightgreen">documents </span>

```json
post amir/_update
{
  "doc":{
  "name": "ali",
  "lastName":"nouzari",
  "phoneNumber":9121364728,
  "age":40,
  "email":"alinozari@gmail.com"

  }
}
```

<h2 style="color:lightgrey">Query</h2>

to <span style="color:cyan"> match </span> <span style="color:red"> all </span> documents in an <span style="color:lightgreen">index </span>

```json
GET amir/_search
{
  "query": {
    "match_all": {}
  }
}
```

<hr>
to  <span style="color:cyan"> match  </span> an  <span style="color:red">specefic </span> field in an <span style="color:lightgreen">document </span>

```json
GET amir/_search
{
  "query": {
    "match": {
      "name":"amir"
    }
  }
}
```

<hr>
to  <span style="color:cyan"> match  </span>
<span style="color:red">several </span> fields in an <span style="color:lightgreen">document </span>

```json
GET amir/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "name": "amir"
          }
        },
        {
          "match": {
            "email": "amiralamian10@gmail.com"
          }
        }
      ]
    }
  }
}


or

GET courses/_search
{
  "query": {
    "multi_match": {
      "fields": ["name", "professor.department"],
      "query": "accounting"
    }
  }
}

```

to <span style="color:cyan"> check </span> that a field is
<span style="color:red"> exits </span> in <span style="color:lightgreen">documents </span>

```json
GET amir/_search
{
  "query": {
    "exists": {
      "field": "adress.lat"
    }

  }
}
or
GET amir/_search
{
  "query": {
    "exists": {
      "field": "email"
    }

  }
}
```

the result will be those documents that have that field or fields

<hr>
to  <span style="color:cyan"> check  </span> that multiple fields is
<span style="color:red"> exits </span>   in  <span style="color:lightgreen">documents </span>

```json
GET amir/_search
{
  "query": {
    "bool": {
      "filter": [
        {
          "exists": {
            "field": "name"
          }
        },
        {
          "exists": {
            "field": "age"
          }
        }
      ]
    }
  }
}

```

<hr>
<span style="color:cyan"> must and not_must  </span> in
<span style="color:lightgreen">query </span>

```json
GET courses/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "name": "computer"
          }
        },

        {
          "match": {
            "professor.department.": "computer"
          }
        }
      ],
      "must_not": [
        {
          "match": {
            "professor.email": "payneg@onuni.com"
          }
        }
      ]
    }
  }
}

```

<hr>
<span style="color:cyan"> sholud  </span> in
<span style="color:lightgreen">query </span>

```json
GET courses/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "name": "computer"
          }
        }
      ],
      "should": [
        {
          "match": {
            "professor.email": "amiralamian10@gmail.com"
          }
        }
      ]
    }
  }
}

```

if <span style="color:cyan"> sholud </span> conditions matches a document, it will <span style="color:lightgreen"> increments </span> the score number of that document and if does not match, it has not any affect on that document.

<hr>
<span style="color:cyan"> match_phrase  </span> in
<span style="color:lightgreen">query </span>

```json
GET courses/_search
{
  "query": {
    "match_phrase": {
      "course_description": "cpt Int 250 gives students an integrated and"
    }
  }
}

```

<span style="color:cyan"> match_phrase </span> checks that a feild has this phrase or not. if there is any wrong word in any kind, this query will <span style="color:lightgreen"> fail</span>.

<hr>

<span style="color:cyan"> match_phrase_prefix </span> in
<span style="color:lightgreen">query </span>

```json
GET courses/_search
{
  "query": {
    "match_phrase_prefix": {
      "course_description": "cpt Int 250 gives students an integrated and"
    }
  }
}

```

<span style="color:cyan"> match_phrase </span> checks that a feild has this phrase or not. if there is any wrong word in any kind, this query will <span style="color:lightgreen">not fail</span>.

<hr>

<span style="color:cyan"> range </span> in
<span style="color:lightgreen">query </span>

```json
GET courses/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "name": "computer"
          }
        }
      ],
      "should": [
        {
          "range": {
            "students_enrolled": {
              "gte": 10,
              "lte": 20
            }
          }
        }
      ]
    }
  }
}

```

it's for numeric fields

<hr>

<span style="color:cyan"> filter </span> in
<span style="color:lightgreen">query </span>

```json
GET courses/_search
{
  "query": {
    "bool": {
      "filter": {
        "bool": {
          "must": [{ "match": { "name": "accounting" } }]
        }
      },
      "should": [
        {
          "match": {
            "room": "e3"
          }
        },
        {
          "range": {
            "students_enrolled": {
              "gte": 10,
              "lte": 20
            }
          }
        }
      ]
    }
  }
}
or

GET amir/_search
{
  "query": {
    "bool": {
      "filter": [
        {
          "exists": {
            "field": "name"
          }
        },
        {
          "exists": {
            "field": "age"
          }
        }
      ]
    }
  }
}


```

every thing can put into the <span style="color:cyan"> filter </span> like bool, match,match_all and... it works like query but it does not return any scores and everything that has been put inside the filter, does not have any effects on the score. also filter cashes datas and because of that searching would be more faster than usual.

<hr>

<span style="color:cyan"> field boost </span> in
<span style="color:lightgreen">query </span>

you can specify a number to a field and make it more or less important than the other fields. example:

```json
GET courses/_search
{
  "query": {
    "bool": {
      "filter": {
        "bool": {
          "must": [{ "match": { "name": "accounting" } }]
        }
      },
      "should": [
        {
          "match": {
            "room": "e3"
          }
        },
        {
          "range": {
            "students_enrolled": {
              "gte": 10,
              "lte": 20
            }
          }
        },
        {
          "multi_match": {
            "query": "market",
            "fields": ["name", "course_description^2"]
          }
        }
      ]
    }
  }
}

```

pay attention to the ^2 infront of course_description feild in multi_match. it will improve it's effect on score number.

<hr>
<span style="color:cyan"> bulk insert  </span> in
<span style="color:lightgreen">elasticsearch </span>

```json
POST /vehicle/_bulk
{ "index": {}}
{ "price" : 10000, "color" : "white", "make" : "honda", "sold" : "2016-10-28", "condition": "okay"}
{ "index": {}}
{ "price" : 20000, "color" : "white", "make" : "honda", "sold" : "2016-11-05", "condition": "new" }
{ "index": {}}
{ "price" : 30000, "color" : "green", "make" : "ford", "sold" : "2016-05-18", "condition": "new" }
{ "index": {}}
{ "price" : 15000, "color" : "blue", "make" : "toyota", "sold" : "2016-07-02", "condition": "good" }
```

<hr>
<h2 style="color:lightgrey">aggregations</h2>
aggregates are using to analysing datas not searcing and they not return any scores.<br><br>

<span style="color:cyan"> aggregations and query </span> in
<span style="color:lightgreen">elasticsearch </span>

```json
GET vehicle/_search
{
  "size": 2,
  "query": {
    "match": {
      "color": "red"
    }
  },
  "aggs": {
    "popular_carsss": {
      "terms": {
        "field": "make.keyword"
      },
      "aggs": {
        "avrage_price": {
          "avg": {
            "field": "price"
          }
        },
        "max_price": {
          "max": {
            "field": "price"
          }
        }
      }
    }
  },
  "sort": [
    {
      "price": { "order": "desc" }
    }
  ]
}


or

GET vehicle/_search
{
  "size": 2,
  "query": {
    "match": {
      "color": "red"
    }
  },
  "aggs": {
    "popular_carsss": {
      "terms": {
        "field": "make.keyword"
      },
      "aggs": {
        "stat_price": {
          "stats": {
            "field": "price"
          }
        }
      }
    }
  }
}

```

<p style="color:white">
the first aggs will create an bucket and second one will create metrics of that bucket.
</p>
size will serilize the number of showing datas not hitted datas.<br>
sort is sortting document base on what ever you want.<br>
stat will do all of avg, min,max,sum for us<br>
.keyword is for textual fields.<br>
result of query will forwarded to aggs. it means that query specify the documents to be aggregate on.
<hr>

more examples of buckets and metrics:

<span style="color:cyan"> range </span> in
<span style="color:lightgreen">aggregations </span>

```json
GET vehicle/_search
{
  "size":2,
  "query": {

      "match": {
        "color": "red"
      }
  },
  "aggs": {
    "popular_carsss": {
      "terms": {
        "field": "make.keyword"
      },
    "aggs":{
      "sold_date_ranges":{
        "range": {
          "field": "sold",
          "ranges": [
            {
              "from": "2016-01-01",
              "to": "2016-05-18"
            },
            {
              "from": "2016-05-08",
              "to": "2017-01-01"
            }

          ]
        }
        }
    }
    }
  }
}

```

<hr>

<p style="color:white">more complex <span style="color:cyan"> aggregation  </span> in
<span style="color:lightgreen">elasticsearch </span> using several buckets and metrics</p>

```json
GET vehicle/_search
{
  "size": 2,
  "query": {
    "match": {
      "color": "red"
    }
  },
  "aggs": {
    "popular_carsss": {
      "terms": {
        "field": "make.keyword"
      },
      "aggs": {
        "sold_date_ranges": {
          "range": {
            "field": "sold",
            "ranges": [
              {
                "from": "2016-01-01",
                "to": "2016-05-18"
              },
              {
                "from": "2016-05-08",
                "to": "2017-01-01"
              }
            ]
          },
          "aggs": {
            "avg_price_for_range": {
              "avg": {
                "field": "price"
              }
            }
          }
        }
      }
    }
  }
}

```

```json
GET vehicle/_search
{
  "size": 2,
  "aggs": {
    "conditions_carsss": {
      "terms": {
        "field": "condition.keyword"
      },
      "aggs": {
        "stat_data": {
          "stats": {
            "field": "price"
          }
        }
      }
    }
  }
}

```

---

<h2 style="color:lightgray"> Scripting </h2>

<p style="color:white">you can use script to update your documents with extra awesome features.  </p> <span style="color:lightgreen">Examples:</span>

```json
POST /vehicles/_update/id
{
  "script":{
    "source":"ctx._source.price +=100"
  }
}
```

you can also use params and if else ...

```json
POST /vehicles/_update/id
{
  "script":{
    "source":"""
      if(params.quan >= 1) {
        ctx._source.price -= 1500
      }else{
         ctx.op= "delete"
      }
    """
  ,
  "params":{
    "quan":0
  }
  }
}
```

also you can use upserts.<br>
upserts will generate the document if not exists otherwise they do anything.

```json
POST /vehicles/_update/id
{
  "script":{
    "source":""" 

        ctx._source.price -= 1500 
    """
  },
  "upsert":{
    "price" : 30000,
    "color" : "gray",
    "make" : "bmw",
    "sold" : "2016-11-20",
    "condition": "good" 
  }
}

```
