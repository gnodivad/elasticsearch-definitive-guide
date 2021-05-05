## Deprecated

### Removal of mapping types
> 299 "[types removal] Specifying types in search requests is deprecated."


Specifying types in the search requests is deprecated in ES7.x and will remove in ES8.0. 

All the query that using type will get the warning in the response's header. For example, `employee` is a type in the query below.
```
GET {{host}}/megacorp/employee/_search
```

*References:*

- https://www.elastic.co/guide/en/elasticsearch/reference/7.x/removal-of-types.html

