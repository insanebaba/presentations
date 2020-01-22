---
title: Fastest and most reliable Graphql on planet
separator: <!--s-->
verticalSeparator: <!--v-->
theme: 'moon'
revealOptions:
  transition: 'convex'
  width: 1280
  parallaxBackgroundHorizontal: 200
  parallaxBackgroundVertical: 50
  showSlideNumber: 'speaker'
  slideNumber: true
---
<style>
.reveal section img { background:none; border:none; box-shadow:none; }
.reveal table tr td {font-size:large}
.reveal .slides > section.present, .reveal .slides > section{
  margin-top:0;
  padding:0;
}
</style>
#### Fastest <!-- .element: class="fragment grow highlight-green" --> 
#### and most 
#### Reliable <!-- .element: class="fragment grow highlight-blue" -->
#### Graphql <!-- .element: class="fragment grow  highlight-red" -->
#### API on planet
<img src="https://media.giphy.com/media/WlBUAWG03Zic8/giphy-downsized.gif" class="fragment appear" width="150" height="100"/>

<!--s-->
## Introduction

* Vikram Mishra <!-- .element: class="fragment appear" -->
* Senior Developer <!-- .element: class="fragment appear" -->
* Team Scupa/NEBULA <!-- .element: class="fragment appear" -->
  
<!--s-->
## Topics for presentation

1. What is GraphQL?
   * **Why** and **when** to use it 
2. Is it Slow?
   * Define parameters of NOT being slow 
3. How Did I solve this problem?
4. Questions

Note: We can only define what's fast by comparison and not by any other means
<!--s-->
# what is **Graphql API**
<!--v-->

GraphQL
---
is a **query** language and **server-side runtime** for APIs that prioritizes giving clients exactly the data they request and no more.
<!--v-->
* GraphQL is designed to make APIs ~**fast**~, <u>flexible</u>, and <u>**developer-friendly**</u>. 
* GraphQL lets developers construct requests that **pull** data from **multiple** data sources in a **single** API call. 
<!--v-->
### Back end <!-- .element: class="fragment grow " --> 
### for 
### front end <!-- .element: class="fragment grow " --> 

<img src="https://sensedia.com/wp-content/uploads/2019/01/sensedia-graphql-bff-1200x565.jpg" alt="drawing" width="600" height="300" class="fragment appear" />
<img src="bff.png" alt="drawing" height="300" class="fragment appear" />
<p>BFF presentation</p> <!-- .element: class="fragment appear" -->
<!--v-->

## Graphql sample
<table width="100%">
    <tr>
        <td>
            Request
            <pre><code data-trim data-noescape>
    {
      product(name: "Orange") {
        <mark>price</mark>
      }
    }
        </code></pre>
        </td>
        <td class="fragment appear">
            Response
            <pre><code  data-trim data-noescape>
        {
          "product": {
            <mark>"price": "$ 12.50"</mark>
          }
        }
        </code></pre>
        </td>
    </tr>
    <tr>
        <td class="fragment appear">
            <img src="graphql_org.png" alt="drawing" height="400" />
            <p>graphql.org</p>
        </td>
        <td class="fragment appear">
            <img src="spec_graphql_org.png" alt="drawing" height="400" />
            <p>spec.graphql.org</p>
        </td>
    </tr>
</table>
<!--s-->

## **Why** use it?
1. It's specification <!-- .element: class="fragment appear" -->
2. It's developer friendly <!-- .element: class="fragment appear" -->
3. It improves data usage over network <!-- .element: class="fragment appear" -->
4. It empowers front end developers to experiment with data. <!-- .element: class="fragment appear" -->
5. It provides granular control over your data graph <!-- .element: class="fragment appear" -->
<!--v-->
# Also 
#### Every organisation data looks like this
![recursive data graphq](https://media.giphy.com/media/r6ctTp6RxTBvO/giphy.gif)
<!--s-->
## **When** to use it ?
1. When you have multiple applications/consumers using it(web apps, mobile apps)?<!-- .element: class="fragment appear" -->
2. When want to have granular control over data?<!-- .element: class="fragment appear" -->
3. When you know you'll extend your existing APIs for more data<!-- .element: class="fragment appear" -->
4. ..... too many to list<!-- .element: class="fragment appear" -->
<!--s-->

## Is It slow? 
# Yes <!-- .element: class="fragment appear" -->
<!--s-->
## Defining Parameters 
<!--v-->
# Define Fastest
1. Requests per second <!-- .element: class="fragment appear " --> 
2. Time out per request <!-- .element: class="fragment appear " --> 
<!--v-->
## Define Reliability
1. Uptime <!-- .element: class="fragment appear " --> 
2. Secure <!-- .element: class="fragment appear " --> 
3. timeout <!-- .element: class="fragment appear " --> 
4. Valid data <!-- .element: class="fragment appear " --> 
<!--v-->
# Reliability
* Not in network   <!-- .element: class="fragment appear " --> 
* Not in architecture   <!-- .element: class="fragment appear " --> 
* Not in infra structure  <!-- .element: class="fragment appear " --> 
* Not just for Graphql  <!-- .element: class="fragment appear " --> 
### In API <!-- .element: class="fragment appear fade-up" --> 
Note: Not covered in today's presentation , today's target
<!--s-->
## Reasons for graphql to be slow
1. Query/Field resolver mapping is tricky<!-- .element: class="fragment appear" -->
2. Merging results from different resources<!-- .element: class="fragment appear" -->
3. query tracing (Apollo tracer)<!-- .element: class="fragment appear" -->
4. N+1 problem<!-- .element: class="fragment appear" -->
Note: Each resolver function really only knows about its own parent object. Coding coming now
<!--v-->
## Evidence for the claim

Note: We are going to look at the projects proving these points missing from current state of the art GRAPHQL servers.
<!--s-->
<!-- .slide: data-transition="zoom" -->
# CODING ALERT
![Coding meme](https://media.giphy.com/media/9nKJN5jf0AFFK/giphy.gif)
<!--s-->
#### BEN Awad BENchmarking graphql frameworks
<iframe src="https://www.youtube.com/embed/JbV7MCeEPb8" height="600" width="800" ></iframe>
<img src="ben_awad_video.png" alt="drawing" height="400" class="fragment appear" >
<p>Ben Awad - Benchmarking Graphql frameworks</p>
<!--v-->

## Data Graph 

```
type Author {
    id: ID!
    name: String!
    md5: String!
    company: String!
    books: [Book!]!
  }
  type Book {
    id: ID!
    name: String!
    numPages: Int!
  }
  type Query {
    authors: [Author!]!
  }
```

<!--v-->
## Serving the results
```javascript
const faker = require("faker");

faker.seed(4321);

function genData() {
  const authors = [];
  for (let i = 0; i < 20; i++) {
    const books = [];

    for (let k = 0; k < 3; k++) {
      books.push({
        id: faker.random.uuid(),
        name: faker.internet.domainName(),
        numPages: faker.random.number()
      });
    }

    authors.push({
      id: faker.random.uuid(),
      name: faker.name.findName(),
      company: faker.company.bs(),
      books
    });
  }

  return authors;
}

module.exports.data = genData();
```
<!--v-->

## Query 

```
{
  authors {
    id
    name
    books {
      id
      name
    }
  }
}

```
<!--v-->
#### Current Benchmarks using autocannon
5 connections for 5 seconds

| Server                                                                                                                                                            | Requests/s | Latency | Throughput/Mb |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------: | :-----: | ------------: |
| [core-graphql-jit-str](https://github.com/benawad/node-graphql-benchmarks/tree/master/benchmarks/core-graphql-jit-str.js)                                         |     5973.2 |  0.30   |         36.93 |
| [core-graphql-jit-buf](https://github.com/benawad/node-graphql-benchmarks/tree/master/benchmarks/core-graphql-jit-buf.js)                                         |     5858.8 |  0.30   |         36.23 |
| [uWebSockets-graphql+jit](https://github.com/benawad/node-graphql-benchmarks/tree/master/benchmarks/uWebSockets-graphql+jit.js)                                   |     5727.6 |  0.30   |         35.20 |
| [core-graphql-jit-buf-fjs](https://github.com/benawad/node-graphql-benchmarks/tree/master/benchmarks/core-graphql-jit-buf-fjs.js)                                 |     5679.6 |  0.30   |         35.13 |
| [fastify-REST](https://github.com/benawad/node-graphql-benchmarks/tree/master/benchmarks/fastify-REST.js)                                                         |     5486.8 |  0.35   |         43.88 |
| [fastify-gql+graphql-jit](https://github.com/benawad/node-graphql-benchmarks/tree/master/benchmarks/fastify-gql+graphql-jit.js)                                   |     5437.2 |  0.34   |         33.87 |
| [express-REST](https://github.com/benawad/node-graphql-benchmarks/tree/master/benchmarks/express-REST.js)                                                         |     4278.2 |  0.53   |         34.49 |
| [express-gql](https://github.com/benawad/node-graphql-benchmarks/tree/master/benchmarks/express-gql.js)                                                           |     3600.2 |  0.86   |         22.65 |
| [graphql-api-koa+graphql-jit](https://github.com/benawad/node-graphql-benchmarks/tree/master/benchmarks/graphql-api-koa+graphql-jit.js)                           |     2817.8 |  1.34   |         17.55 |
| [express-graphql+graphql-jit](https://github.com/benawad/node-graphql-benchmarks/tree/master/benchmarks/express-graphql+graphql-jit.js)                           |     2374.2 |  1.57   |         14.94 |
| [fastify-gql](https://github.com/benawad/node-graphql-benchmarks/tree/master/benchmarks/fastify-gql.js)                                                           |     2367.0 |  1.58   |         14.74 |
| [express-graphql+graphql-jit+type-graphql](https://github.com/benawad/node-graphql-benchmarks/tree/master/benchmarks/express-graphql+graphql-jit+type-graphql.js) |     2176.2 |  1.72   |         13.70 |
| [apollo-server-fastify+graphql-jit](https://github.com/benawad/node-graphql-benchmarks/tree/master/benchmarks/apollo-server-fastify+graphql-jit.js)               |     1740.6 |  2.33   |         10.88 |
| [graphql-api-koa](https://github.com/benawad/node-graphql-benchmarks/tree/master/benchmarks/graphql-api-koa.js)                                                   |     1726.8 |  2.50   |         10.76 |
| [express-graphql](https://github.com/benawad/node-graphql-benchmarks/tree/master/benchmarks/express-graphql.js)                                                   |     1564.0 |  2.79   |          9.84 |
| [apollo-schema+async](https://github.com/benawad/node-graphql-benchmarks/tree/master/benchmarks/apollo-schema+async.js)                                           |     1555.8 |  2.79   |          9.79 |
| [express-graphql+type-graphql](https://github.com/benawad/node-graphql-benchmarks/tree/master/benchmarks/express-graphql+type-graphql.js)                         |     1481.0 |  2.90   |          9.32 |
| [type-graphql+async](https://github.com/benawad/node-graphql-benchmarks/tree/master/benchmarks/type-graphql+async.js)                                             |     1455.8 |  2.93   |          9.16 |
| [type-graphql+middleware](https://github.com/benawad/node-graphql-benchmarks/tree/master/benchmarks/type-graphql+middleware.js)                                   |     1452.0 |  2.94   |          9.14 |
Note: Your rest apis are beating these benchmarks already
<!--v-->
### Git repository
![ben_awad](github.combenawadnode-graphql-benchmarks.png)
<br>
[node-graphql-benchmarks](https://github.com/benawad/node-graphql-benchmarks)
Note: 
<!--s-->
### How to Solve this **slow** graphql performance
1. Choose Fastest Web framework <!-- .element: class=" fragment appear "-->
   *  **actix** (75% faster than closest java framework )<!-- .element: class=" fragment appear "-->  
2. Choose Rust (more reliable than any in the benchmarks)  <!-- .element: class=" fragment appear "-->
<table>
<tr>
<td><img src="https://mir-s3-cdn-cf.behance.net/project_modules/disp/fe36cc42774743.57ee5f329fae6.gif" class="fragment appear" width="200" height="200"/></td>
<td>
<p  class="fragment appear">
<img src="web_server_compared.png" width="200" height="200"/>
<br>
<a href="https://www.techempower.com/benchmarks/">https://www.techempower.com/benchmarks/</a>
</p>
</td>
</tr>
</table>
Note: This is what you'll see when you go to the site
<!--v-->
![benchmarks](benchmarks_for_web.png)
<!--s-->
## Setting up project for crushing benchmarks
1. Use **actix-web** for web server 
2. Use **Juniper** for Graphql
3. Use **Fake** for data generation

<!--v-->
# Code for actix server
<!--v-->
### File : schema.rs
#### Object Types
```
use fake::{Dummy, Fake, Faker};
use juniper::FieldResult;
use juniper::RootNode;

#[derive(GraphQLObject, Debug, Dummy)]
#[graphql(description = "An author")]
struct Author {
    id: String,
    name: String,
    company: String,
    books: Vec<Books>,
}

#[derive(GraphQLObject, Debug, Dummy)]
#[graphql(description = "A Book")]
struct Books {
    id: String,
    name: String,
    num_pages: i32,
}
```
<!--v-->
### Query Structs
```
pub struct QueryRoot;

graphql_object!(QueryRoot: () |&self| {
    field authors(&executor) -> FieldResult<Vec<Author>> {
        Ok(gen_data())
    }
});

pub struct MutationRoot;

graphql_object!(MutationRoot: () |&self| {
});

pub type Schema = RootNode<'static, QueryRoot, MutationRoot>;

pub fn create_schema() -> Schema {
    Schema::new(QueryRoot {}, MutationRoot {})
}
```
<!--v-->

## Data generator
```
fn gen_data() -> Vec<Author> {
    let mut result: Vec<Author> = Vec::new();
    for _ in 0..=20 {
        let mut auth = Faker.fake::<Author>();
        for _ in 0..4 {
            auth.books.push(Faker.fake::<Books>());
        }
        result.push(auth);
    }
    result
}
```
<!--v-->
### File : schema.rs
### setting up graphql playground
```
use std::io;
use std::sync::Arc;

#[macro_use]
extern crate juniper;

use actix_web::{middleware, web, App, Error, HttpResponse, HttpServer};
use juniper::http::graphiql::graphiql_source;
use juniper::http::GraphQLRequest;

mod schema;

use crate::schema::{create_schema, Schema};

async fn graphiql() -> HttpResponse {
    let html = graphiql_source("http://127.0.0.1:8080/graphql");
    HttpResponse::Ok()
        .content_type("text/html; charset=utf-8")
        .body(html)
}

```

<!--v-->
### setting up graphql handler

```

async fn graphql(
    st: web::Data<Arc<Schema>>,
    data: web::Json<GraphQLRequest>,
) -> Result<HttpResponse, Error> {
    let user = web::block(move || {
        let res = data.execute(&st, &());
        Ok::<_, serde_json::error::Error>(serde_json::to_string(&res)?)
    })
    .await?;
    Ok(HttpResponse::Ok()
        .content_type("application/json")
        .body(user))
}

```

<!--v-->
##### Fire up The Server
```

#[actix_rt::main]
async fn main() -> io::Result<()> {
    let schema = std::sync::Arc::new(create_schema());

    // Start http server
    HttpServer::new(move || {
        App::new()
            .data(schema.clone())
            .wrap(middleware::Logger::default())
            .service(web::resource("/graphql").route(web::post().to(graphql)))
            .service(web::resource("/graphiql").route(web::get().to(graphiql))))
    })
    .bind("127.0.0.1:8080")?
    .run()
    .await
}
```
<!--s-->
# AtLast the Results
<!--v-->

### Benchmark with load_runner.js(autocannon)
5 connections for 5 seconds

| Stat    | 2.5% | 50% | 97.5% | 99% | Avg    | Stdev  | Max      |
|---------|-----|-----|-------|-----|--------|--------|----------|
| Latency | 0ms | 0ms | 0ms   | 0ms | 0.01ms | 0.36ms | 111.62ms |

| Stat      | 1%     | 2.5%   | 50%    | 97.5%  | Avg     | Stdev   | Min    |
|-----------|--------|--------|--------|--------|---------|---------|--------|
| Req/Sec   | 18431  | 18431  | 22863  | 22927  | 21982.4 | 1779.49 | 18421  |
| Bytes/Sec | 3.91MB | 3.91MB | 4.85MB | 4.86MB | 4.66MB  | 378KB   | 3.91MB |
<!--s-->

# Reliability using Rust

1. Static memory management, no segfaults, no surprises
2. no dangling pointers
3. Zero cost abstraction
4. Small memory footprint
5. No Runtimes. 
6. No race conditions

<!--s-->

## Github link
![github link to othe benchmark repo](benchmark_rust_graphql_juniper.png)
<br/>
[benchmark_rust_graphql_juniper](https://github.com/insanebaba/benchmark_rust_graphql_juniper/)

<!--s-->

### Questions?
![Resting here](https://i.pinimg.com/originals/d2/40/dd/d240ddbcf97be8749949be6360a02bd9.gif)