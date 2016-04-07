# Wisper Protocol

Wisper is an RPC protocol that defines how two different nodes should communicate. The protocol is heavily inspired by JSON RPC but extended to allow Class like operations.

### Features

###### Notification

A simple fire and forget type of message that instructs a remote node to perform a method call with a bunch of parameters.

```json
{
    "method" : "log",
    "params" : ["Hello world!"]
}
```

###### Plain Error

When something goes wrong and it is not a result of a request, a plain error will be sent.

```json
{
    "error" : {
      "message" : "Something went wrong!"
    },
}
```

###### Request

Whenever you want to get some sort of result from a method invocation a request is the way to go. A request will always receive a response from the other node with the result or an error as a result of handling the request.

```json
{
    "id" : "abc123",
    "method" : "concat",
    "params" : ["Hello ", "world!"]
}
```


###### Result Response

After handling a request a response should be sent to deliver the final result of that request.

```json
{
    "id" : "abc123",
    "result" : "Hello world!"
}
```

###### Error Response

If we for some reason run into problems when handling a request we can return an error response.

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

The basic wisper protocol is extended by an object protocol that allows Class like behaviour with constructors, destructors, instance methods and events.

[Wisper Object Protocol](./OBJECT_EXTENSIONS.md)
