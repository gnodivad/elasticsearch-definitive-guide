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


### The query/filter merge

> unknown query [filtered]

We will see the error above if we performed search by using the following query.
``` json
{
  "query": {
    "filtered": {
      "filter": {
        "range": {
          "age": {
            "gt": 30
          }
        }
      },
      "must": {
        "match": {
          "last_name": "smith"
        }
      }
    }
  }
}
```
should replaced with

```json
{
  "query": {
    "bool": {
      "filter": {
        "range": {
          "age": {
            "gt": 30
          }
        }
      },
      "must": {
        "match": {
          "last_name": "smith"
        }
      }
    }
  }
}
```

*References:*

- https://www.elastic.co/guide/en/elasticsearch/reference/6.8/query-dsl-filtered-query.html#query-dsl-filtered-query
- https://www.elastic.co/blog/better-query-execution-coming-elasticsearch-2-0


### Text Field with fielddata

> Text fields are not optimised for operations that require per-document field data like aggregations and sorting, so these operations are disabled by default. Please use a keyword field instead. Alternatively, set fielddata=true on [interests] in order to load field data by uninverting the inverted index. Note that this can use significant memory.

Text fields is searchable by default but are not available for aggregations, sorting or scription. We will see the exception when execute the following query.
```json
{
  "aggs": {
    "all_interests": {
      "terms": {
        "field": "interests"
      }
    }
  }
}
```
should replaced with
```json
{
  "aggs": {
    "all_interests": {
      "terms": {
        "field": "interests.keyword"
      }
    }
  }
}
```

*References:*
- https://www.elastic.co/guide/en/elasticsearch/reference/current/text.html#fielddata-mapping-param

### Optimistic Concurrency Control
> internal versioning can not be used for optimistic concurrency control

`version` cannot used for optimistic concurrency control anymore. To achieve that, we need to use `if_primary_term` and `if_seq_no`. The primary term `_primary_term` and sequence number `_seq_no` are uniquely identify a change. Primary term will be increase when a primary is promoted and sequence numbers will be increase and change when a operation is issued.

*References:*
- https://www.elastic.co/blog/elasticsearch-sequence-ids-6-0
- https://www.elastic.co/guide/en/elasticsearch/reference/6.8/breaking-changes-6.7.html#_deprecated_usage_of_literal_internal_literal_versioning_for_optimistic_concurrency_control
- https://www.elastic.co/guide/en/elasticsearch/reference/current/optimistic-concurrency-control.html


### File scripts
> unknown field [params]

File scripts have been removed. We need to use stored scripts for that.

*References:*
- https://www.elastic.co/guide/en/elasticsearch/reference/6.0/breaking_60_scripting_changes.html#_file_scripts_removed
- https://www.elastic.co/guide/en/elasticsearch/reference/6.2/modules-scripting-using.html#modules-scripting-stored-scripts
