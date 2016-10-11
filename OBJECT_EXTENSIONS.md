# Wisper Object Protocol - `Extends Wisper Protocol`

Extends the wisper protocol to allow the emulation of classes and instances. You are responsible for the whole life cycle of the instances you create so you need to clean up after yourself by destroying what you create.

The object extension provides a few special characters that you should know about when dealing with classes.

* `~` (Tilde) The constructor/destructor character
* `:` (Colon) The instance method character
* `!` (Bang) The event character

The special characters `~` and `!` can used after the special character `:` to address an instance. As an example, if you want to send an event to only a specific instance of `Foo` you specify a method like this `some.path.Foo:!` or if you want to destroy an instance of `Foo` you can specify a method like this `some.path.Foo:~`.


### Constructor Call

Create an instance of the Class `Foo`. Create messages are identified by the special character `~` (tilde) right after a path component that starts with a capital character.

The wisper protocol also supports parameters to the constructors should you need it.

Allowed message types: [ `Request` ]

**Request**
```json
{
    "id" : "abc123",
    "method" : "some.path.Foo~",
    "params" : []
}
```

**Response**

The instance was created and you are given the unique id to reference this instance in future calls. If the instance has some default values it wants you to know about it will send them under `props`.

```json
{
    "id" : "abc123",
    "result" : {
      "id" : "something-uniquely-identifying-an-instance",
      "props" : {}
    }
}
```


### Destructor Call

To destroy an instance of a class you need to call the special create character `~` (tilde) as an instance method. This will effectively destroy the instance and it will no longer be addressable.

Allowed message types: [ `Notification` | `Request` ]

**Notification**

```json
{
    "method" : "some.path.Foo:~",
    "params" : ["something-uniquely-identifying-an-instance"]
}
```

**Request**
```json
{
    "id" : "abc123",
    "method" : "some.path.Foo:~",
    "params" : ["something-uniquely-identifying-an-instance"]
}
```

**Response**

The response does not have to carry any specific result in this case but a recommendation for a framework implementing this protocol is to return the id of the destroyed instance.

```json
{
    "id" : "abc123",
    "result" : "something-uniquely-identifying-an-instance"
}
```


### Static method Call

A static method call is very similar to a regular Wisper message except that it is mapped under a class name.

Allowed message types: [ `Notification` | `Request` ]

**Notification**

```json
{
    "method" : "some.path.Foo.bar",
    "params" : []
}
```

**Request**

```json
{
    "id" : "abc123",
    "method" : "some.path.Foo.append",
    "params" : ["Hello ", "world!"]
}
```

**Response**

```json
{
    "id" : "abc123",
    "result" : "Hello world!"
}
```


### Instance method Call

A call to a specific instance of a class, the first argument will always be the instance identifier followed by any arguments to the method.

Allowed message types: [ `Notification` | `Request` ]

**Notification**

```json
{
    "method" : "some.path.Foo:bar",
    "params" : ["something-uniquely-identifying-an-instance"]
}
```

**Request**

```json
{
    "id" : "abc123",
    "method" : "some.path.Foo:append",
    "params" : ["something-uniquely-identifying-an-instance", "Hello ", "world!"]
}
```

**Response**

```json
{
    "id" : "abc123",
    "result" : "Hello world!"
}
```


### Event Message

A special bi-directional message that is used to tell either side that something has changed that might be requiring some action. Usually its a good idea to cache this value to read it back at a later time. This type of message can be good for syncing properties between the two nodes.

Events can happen both at class level and instance level. Just like all other messages this is indicated by the `:` (colon) character for instance events.

One example use case could be a remote audio player that sends progress events as the playhead moves along in the audio file being played.

Allowed message types: [ `Notification` ]

**Class Event**

```json
{
    "method" : "some.path.Foo!",
    "params" : ["status", "AWESOME"]
}
```

**Instance Event**

```json
{
    "method" : "some.path.Foo:!",
    "params" : ["something-uniquely-identifying-an-instance", "progress", 134.45]
}
```
