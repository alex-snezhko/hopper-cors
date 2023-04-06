---
title: HopperCors
---

An extension to Hopper allowing handling of CORS requests.

Version 0.1.0

### HopperCors.**AllowOriginType**

```grain
enum AllowOriginType {
  Origins(List<String>),
  AllowOriginPredicate((String -> Bool)),
}
```

Represents possible values for specifying allowed Origins for requests.

`Origins` can be used to give a list of concrete origins allowed, or `"*"`
for all origins.

`AllowOriginPredicate` can be used to enable dynamic origin validation; it
takes a function where the argument is the origin name and the returned
value is `true` if that origin should be allowed, or `false` otherwise.

### HopperCors.**CorsOption**

```grain
enum CorsOption {
  AllowOrigin(AllowOriginType),
  AllowMethods(List<Hopper.Method>),
  AllowHeaders(List<String>),
  ExposeHeaders(List<String>),
  MaxAge(Number),
}
```

Represents an option to configure CORS middleware with, corresponding to
the `Access-Control-Allow-...` HTTP headers.

`AllowOrigin` corresponds to the `Access-Control-Allow-Origin` header for
specifying allowed origins; default value: `Origins(["*"])`.

`AllowMethods` corresponds to the `Access-Control-Allow-Methods` header for
preflight requests; default value: `[GET, HEAD, PUT, PATCH, POST, DELETE]`.

`AllowHeaders` corresponds to the `Access-Control-Allow-Headers` header for
preflight requests for specifying which headers can be sent for the actual
request; default value: `["*"]`

`ExposeHeaders` corresponds to the `Access-Control-Expose-Headers` header
for specifying which response headers are exposed to the client; default
value is `[]`

`MaxAge` corresponds to the `Access-Control-Max-Age` header for specifying
how long the response from preflight requests should be cached; unset by
default.

### HopperCors.**OptionInternal**

```grain
type OptionInternal
```

### HopperCors.**withCors**

```grain
withCors :
  List<CorsOption> ->
   (Hopper.Request -> Hopper.Response) -> Hopper.Request -> Hopper.Response
```

Middleware for enabling CORS requests.

Parameters:

|param|type|description|
|-----|----|-----------|
|`options`|`List<CorsOption>`|A list of `CorsOption`s to configure the CORS middleware with|

Returns:

|type|description|
|----|-----------|
|`(Hopper.Request -> Hopper.Response) -> Hopper.Request -> Hopper.Response`|A middleware that enables handling of CORS requests on routes|

