# Hopper CORS
A [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) extension for [Hopper](https://github.com/alex-snezhko/hopper), an HTTP microframework for the Grain programming language.

## Installation
This library is contained within a single Grain file, which should be installed into the same directory as your `hopper.gr`
```
curl https://raw.githubusercontent.com/alex-snezhko/hopper-cors/main/hopper-cors.gr -o hopper-cors.gr
```

## Overview
The library exposes a single function, `withCors`, taking a list of options and returning a middleware that enables CORS support for the routes in its scope. Consult the [API docs](/api-docs.md) for all of the possible options and default behaviors.

`withCors` also automatically handles [preflight requests](https://developer.mozilla.org/en-US/docs/Glossary/Preflight_request) for "non-simple" requests.

## Example Usage

### Basic - enable all CORS requests
```
import Hopper from "./hopper"
import Cors from "./hopper-cors"

Hopper.serveWithMiddleware(Cors.withCors([]), [
  Hopper.get("/hello", req => {
    // ...
  }),
  Hopper.put("/preflighted", req => {
    // ...
  }),
])
```

### With dynamic origin checking and only on certain scope
```
import List from "list"
import Hopper from "./hopper"
import Cors from "./hopper-cors"

let withCorsDynamicOrigin = Cors.withCors([
  Cors.AllowOrigin(origin => {
    let allowedOrigins = // ...
    List.contains(origin, allowedOrigins)
  })
])

Hopper.serve([
  Hopper.scope("/with-cors", withCorsDynamicOrigin, [
    Hopper.get("/resource", req => {
      // ...
    })
  ])
])
```

## API Docs
`grain doc`-generated API docs are available [here](/api-docs.md)

## Acknowledgements
Aspects of the API and implementation of this library were adapted from [Gin's Cors Library](https://github.com/gin-contrib/cors) and [Express' Cors Library](https://github.com/expressjs/cors).
