# Wisper Protocol

RPC (remote procedure call) is a way to invoke a method in a remote environment. There are a lot of protocols implementing--- JSON RPC...

Wisper is derived from JSON-RPC but extends it in


### Features

###### Notification

A message that does not require a response

```json
{
    "method" : "log",
    "params" : ["Hello world!"]
}
```

###### Plain Error
Something went wrong.
```json
{
    "id" : "abc123",
}
```

###### Request
A message that expects a response
```json
{
    "id" : "abc123",
    "method" : "concat",
    "params" : ["Hello ", "world!"]
}
```


###### Result Response

Whenever

```json
{
    "id" : "abc123",
    "result" : "Hello world!"
}
```

###### Error Response

```json
{
    "id" : "abc123",
    "error" : {
      "message" : "could not invoke method",
      ...
    }
}
```



# Wisper Object Protocol -Extends Wisper Protocol-

###### Constructor Call

**Request**
```json
{
    "id" : "abc123",
    "method" : "some.path.Foo~",
    "params" : []
}
```

**Response**
```json
{
    "id" : "abc123",
    "result" : {
      "id" : "something-uniquely-identifying-an-instance",
      "props" : {}
    }
}
```

###### Destructor Call

**Notification**

```json
{
    "method" : "some.path.Foo:~",
    "params" : ["something-uniquely-identifying-an-instance"]
}
```


###### Function Call

**Notification**

```json
{
    "method" : "some.path.Foo.bar",
    "params" : []
}
```


###### Method Call

**Notification**

```json
{
    "method" : "some.path.Foo:bar",
    "params" : ["something-uniquely-identifying-an-instance"]
}
```


###### Event Call

**Notification**

```json
{
    "method" : "some.path.Foo:!",
    "params" : ["something-uniquely-identifying-an-instance", "EventName", "some type of event value, could be any type supported by json"]
}
```
