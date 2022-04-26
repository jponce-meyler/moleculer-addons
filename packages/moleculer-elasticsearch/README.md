![Moleculer logo](http://moleculer.services/images/banner.png)

# moleculer-elasticsearch [![NPM version](https://img.shields.io/npm/v/moleculer-elasticsearch.svg)](https://www.npmjs.com/package/moleculer-elasticsearch)

[Elasticsearch](https://www.elastic.co/) service for Moleculer.

# Features
- support [5.4 API](https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/api-reference.html)
- straightforward actions & params

# Install

```bash
$ npm install moleculer-elasticsearch --save
```

# Usage

**Running Elasticsearch server for development**
```bash
 $ docker run -p 9200:9200 -e "http.host=0.0.0.0" -e "transport.host=127.0.0.1" docker.elastic.co/elasticsearch/elasticsearch:5.5.0
```
[More information about Elasticsearch Docker image.](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html#docker-cli-run)

**Start service**
```js
"use strict";

let { ServiceBroker } 	= require("moleculer");
let ESService 			= require("moleculer-elasticsearch");

// Create broker
let broker = new ServiceBroker({ logger: console });

// Create service
broker.createService({
    mixins: [ESService],
    settings: {
        elasticsearch: {
            node: "http://elastic:changeme@<docker-hostname>:9200",
        }
    }
});

broker.start()

    // Create a document
    .then(() => broker.call("elasticsearch.create", { 
        index: "demo", 
        id: "1", 
        body: { name: "John Doe", age: 36 }
    }))

    .delay(500)

    // Search documents
    .then(() => broker.call("elasticsearch.search", { 
        body: {
            query: {
                match: {
                    name: "john"
                }
            }
        } 
    }).then(res => console.log("Hits:", res.hits.hits)))
    
    // Remove document
    .then(() => broker.call("elasticsearch.delete", { index: "demo", id: "1" }))
```

# Settings

<!-- AUTO-CONTENT-START:SETTINGS -->
| Property | Type | Default | Description |
| -------- | ---- | ------- | ----------- |
| `elasticsearch` | `Object` | **required** | Elasticsearch constructor options. [More options](https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/configuration.html) |
| `elasticsearch.node` | `String` | **required** | Host |

<!-- AUTO-CONTENT-END:SETTINGS -->

<!-- AUTO-CONTENT-TEMPLATE:SETTINGS
| Property | Type | Default | Description |
| -------- | ---- | ------- | ----------- |
{{#each this}}
| `{{name}}` | {{type}} | {{defaultValue}} | {{description}} |
{{/each}}
{{^this}}
*No settings.*
{{/this}}

-->

# Actions
<!-- AUTO-CONTENT-START:ACTIONS -->
## `bulk` 

Perform many index/delete operations in a single API call.

More info: https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/api-reference.html#api-bulk

### Parameters
| Property | Type | Default | Description |
| -------- | ---- | ------- | ----------- |
| `index` | `String` | - | Default index for items which don’t provide one |
| `body` | `Array` | **required** | The request body, as either an array of objects or new-line delimited JSON objects |

### Results
**Type:** `Object`

Elasticsearch response object


## `create` 

Adds a typed JSON document in a specific index, making it searchable. If a document with the same index, type, and id already exists, an error will occur.

More info: https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/api-reference.html#api-create

### Parameters
| Property | Type | Default | Description |
| -------- | ---- | ------- | ----------- |
| `index` | `String` | **required** | The name of the index |
| `id` | `String` | **required** | Document ID |
| `body` | `Object` | **required** | The request body, as either JSON or a JSON serializable object. |

### Results
**Type:** `Object`

Elasticsearch response object


## `get` 

Get a typed JSON document from the index based on its id.

More info: https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/api-reference.html#api-get

### Parameters
| Property | Type | Default | Description |
| -------- | ---- | ------- | ----------- |
| `index` | `String` | **required** | The name of the index |
| `id` | `String` | - | Document ID |

### Results
**Type:** `Object`

Found document


## `update` 

Update (reindex) the document with the specified unique id.

More info: https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/api-reference.html#api-update

### Parameters
| Property | Type | Default | Description |
| -------- | ---- | ------- | ----------- |
| `index` | `String` | **required** | The name of the index |
| `id` | `String` | **required** | Document ID |
| `body` | `Object` | **required** | The request body, as either JSON or a JSON serializable object. |

### Results
**Type:** `Object`

Elasticsearch response object


## `delete` 

Delete the document with the specified unique id.

More info: https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/api-reference.html#api-delete

### Parameters
| Property | Type | Default | Description |
| -------- | ---- | ------- | ----------- |
| `index` | `String` | **required** | The name of the index |
| `id` | `String` | **required** | Document ID |

### Results
**Type:** `Object`

Elasticsearch response object


## `search` 

Return documents matching a query, aggregations/facets, highlighted snippets, suggestions, and more.

More info: https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/api-reference.html#api-search

### Parameters
| Property | Type | Default | Description |
| -------- | ---- | ------- | ----------- |
| `index` | `String`, `Array.<String>` | **required** | A comma-separated list of index names to search; use _all or empty string to perform the operation on all indices |
| `q` | `String` | - | Query in the [Lucene query string](https://lucene.apache.org/core/2_9_4/queryparsersyntax.html) syntax. |
| `body` | `Object` | - | The request body, as either JSON or a JSON serializable object. |

### Results
**Type:** `Object`

Elasticsearch response object


## `count` 

Get the number of documents for the cluster, index, type, or a query.

More info: https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/api-reference.html#api-count

### Parameters
| Property | Type | Default | Description |
| -------- | ---- | ------- | ----------- |
| `index` | `String`, `Array.<String>` | **required** | A comma-separated list of indices to restrict the results. |
| `q` | `String` | - | Query in the Lucene query string syntax. |
| `body` | `Object` | - | The request body, as either JSON or a JSON serializable object. |

### Results
**Type:** `Object`

Elasticsearch response object


## `call` 

Call any Elasticsearch API

More info: https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/api-reference.html

### Parameters
| Property | Type | Default | Description |
| -------- | ---- | ------- | ----------- |
| `api` | `String` | **required** | The name of the API |
| `params` | `Object` | **required** | Params of request |

### Results
**Type:** `Object`

Elasticsearch response


<!-- AUTO-CONTENT-END:ACTIONS -->

<!-- AUTO-CONTENT-TEMPLATE:ACTIONS
{{#each this}}
## `{{name}}` {{#each badges}}{{this}} {{/each}}
{{#since}}
_<sup>Since: {{this}}</sup>_
{{/since}}

{{description}}

### Parameters
| Property | Type | Default | Description |
| -------- | ---- | ------- | ----------- |
{{#each params}}
| `{{name}}` | {{type}} | {{defaultValue}} | {{description}} |
{{/each}}
{{^params}}
*No input parameters.*
{{/params}}

{{#returns}}
### Results
**Type:** {{type}}

{{description}}
{{/returns}}

{{#hasExamples}}
### Examples
{{#each examples}}
{{this}}
{{/each}}
{{/hasExamples}}

{{/each}}
-->

# Methods

<!-- AUTO-CONTENT-START:METHODS -->
<!-- AUTO-CONTENT-END:METHODS -->

<!-- AUTO-CONTENT-TEMPLATE:METHODS
{{#each this}}
## `{{name}}` {{#each badges}}{{this}} {{/each}}
{{#since}}
_<sup>Since: {{this}}</sup>_
{{/since}}

{{description}}

### Parameters
| Property | Type | Default | Description |
| -------- | ---- | ------- | ----------- |
{{#each params}}
| `{{name}}` | {{type}} | {{defaultValue}} | {{description}} |
{{/each}}
{{^params}}
*No input parameters.*
{{/params}}

{{#returns}}
### Results
**Type:** {{type}}

{{description}}
{{/returns}}

{{#hasExamples}}
### Examples
{{#each examples}}
{{this}}
{{/each}}
{{/hasExamples}}

{{/each}}
-->


# Test
```
$ npm test
```

In development with watching

```
$ npm run ci
```

# License
The project is available under the [MIT license](https://tldrlegal.com/license/mit-license).

# Contact
Copyright (c) 2016-2019 MoleculerJS

[![@moleculerjs](https://img.shields.io/badge/github-moleculerjs-green.svg)](https://github.com/moleculerjs) [![@MoleculerJS](https://img.shields.io/badge/twitter-MoleculerJS-blue.svg)](https://twitter.com/MoleculerJS)
