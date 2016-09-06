# Overview

NUBOMEDIA capabilities can be accessed through any kind of signaling protocol including SIP, XMPP or REST. However, for the sake of simplicity, we have created a very simple signaling protocol based on JSON-RPCs over WebSockets that is suitable for most applications not requiring specific interoperability features. This API is split into two complementary parts:

- NUBOMEDIA Server Signaling API. This is a client-side API enabling the creation of JSON-RPC sessions and the sending and receiving of messages through them.
- NUBOMEDIA Client Signaling API. This is a server-side API enabling the creation of JSON-RPC sessions and the implementation of server-side business logic for messages following a request/response communication scheme.


## NUBOMEDIA Server Signaling API

The NUBOMEDIA server signaling API is available in ***Java v6.0*** or later. This API turns around the notion of handlers - developers create custom handlers that provide the logic for handling incoming JSONRPC requests. Just for illustration, consider the following simple handler.

```java
public class EchoJsonRpcHandler extends DefaultJsonRpcHandler<JsonObject>
{
  @Override
  public void handleRequest(Transaction transaction, Request<JsonObject> request)
    throws Exception
  {
    if("echo".equalsIgnoreCase(request.getMethod()))
    {
      transaction.sendResponse(request.getParams());
    }
  }
}
```
As it can be observed, developers just need to override the handleRequest method in order to provide their custom processing logic. In that case, only “echo” messages are processed. In order to program handlers, developers have some helper classes:

- Transactions: A Transaction represents a request/response pair. Hence, through the transaction developers access the ability of sending back responses to requests.

- Sessions: The Transaction also exposes a Session object. Sessions represent state among different transactions so that attributes can be stored and recovered across them. It is important to remark that this JSON-RPC session concept may spawn across different HTTP (i.e. WebSocket) sessions. This high-level notion of session is enforced through a sessionId property which is exchanged between clients and servers as part of the params JSON object, in requests, and of the result JSON object, in responses.

## NUBOMEDIA Client Signaling API

The NUBOMEDIA client signaling API is available in ***JavaScript*** for WWW browsers, ***Java*** for Android 4.0 and later, ***Objective C*** for iOS on versions 7.0 and later. These implementations are just a wrapper of the JSON-RPC v2 over WebSockets protocol.Hence, further programming languages might be supported by creating the appropriate wrappers on those APIs.The NUBOMEDIA Client Signaling API has the main objective of enabling clients to send JSON-RPC messages to servers and of making possible to receive the corresponding answers and process them. The mechanism for doing so is quite straightforward and can be appreciated in the following code snippet.

```java
  JsonRpcClient client = new JsonRpcClientWebSocket("ws://localhost:8080/echo");
  Request<JsonObject> request = new Request<>();

  request.setMethod("echo");
  JsonObject params = new JsonObject();
  params.addProperty("some property", "Some Value"); 
  request.setParams(params);
  Response<JsonElement> response = client.sendRequest(request);
```
As it can be observed, once the client object is created pointing to the appropriate WebSocket address, developers can create the requests messages of their choice and ask the client to send them to the server. As a result, a response message is received that can
be later analyzed by the client for providing the appropriate processing.

## Example Use Case

The signaling API can be used for establishing any kind of communication between clients and application server logic. Both the NUBOMEDIA room and tree APIs have been created on top of the signaling API and they can be considered as advanced and realistic examples of using it. However, for the objectives of this document, we wish to introduce a very simple example illustrating how the signaling API is used. To this aim, we are going to program the most simple signaling mechanism we can imagine: an echo.

In the echo signaling, clients send text messages to the server which, in turn, answers back the message to the client. Programming an echo signaling with our signaling API is straightforward. First, we need to determine how to create the signaling messages.

This is quite simple given the JSON-RPC message structure specified above: our signaling message just uses the params field and inserts into it the echo string. After that, we can program the server side logic, which can be implemented with the code below.

```java
  public class EchoJsonRpcHandler extends DefaultJsonRpcHandler<JsonObject>
  {
    @Override
    public void handleRequest(Transaction transaction, Request<JsonObject> request)
      throws Exception
    {
      log.info("Request id:"+request.getId());
      log.info("Request method:"+request.getMethod());
      log.info("Request params:"+request.getParams());
      transaction.sendResponse(request.getParams());
    }
  }
```

The client side is also very simple to create, as it can be observed in code snippet below:

```java
  static class Params
  {
    String  text;
  }

  JsonRpcClient client = new JsonRpcClientWebSocket(“ws://my.ip.com/path”);
  Params params = new Params();
  params.text = "Hello world!";
  Params result = client.sendRequest("echo", params, Params.class);
  LOG.info("Response:"+result);
  log.info(result.text);
  client.close();
```

## Resources

**Full Documentation**

- [Server signaling API](http://doc-kurento-jsonrpc.readthedocs.org/en/latest/)
- [JavaScript client signaling API](http://doc-kurento-jsonrpc.readthedocs.org/en/latest/)
- [Android client signaling API](http://jsonrpc-ws-android.readthedocs.org/en/latest/)
- [iOS client signaling API](http://kurento-ios.readthedocs.org/en/latest/dev_guide.html#json-rpc)

**API Source Code**

The repositories containing the relevant artifacts involved in this API are the following:

- [Server signaling API](https://github.com/Kurento/kurento-java/tree/master/kurentojsonrpc)
- [JavaScrpt signaling API](https://github.com/Kurento/kurento-jsonrpc-js)
- [Android signaling API](https://github.com/nubomedia-vtt/jsonrpc-ws-android)
- [iOS signaling API](https://github.com/nubomediaTI/Kurento-iOS)
