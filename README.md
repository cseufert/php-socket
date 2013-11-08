php-socket
==========

A PHP Comet Library

An example of how to use (server):

daemon.php
```php
<?php
$comet = new comet();
//This is not very 'PHP' like way of doing things, also means all code must reside in daemon.php.
//This daemon would have to be restarted with any/every change to any code that it executes.
$comet->on('connection', function($client) {
  $client->raise('ping');
  //Add example for sending data
  $client->raise('updated',$data);
  $client->on('pong', function($data) {
    if($data['clientid'] == $client->id) {
      // Connection OK!
  });
});
```

You can also send messages to clients outside an active comet session:

```php
<?php
$message = "You have mail";
$comet = new comet();
$client = $comet->getClient(123); // Client ID is 123 here
$client->raise('message', ['message'=>$message]);
//Perhaps an alternative way could be 
comentClient::client($clientid)::raise('message',$mydata);
```

comet.php
```php
<?php
$session = new cometSession();
$session->process();
//here also could be a static method
cometSession::listen();
```

And an example of the JavaScript code:

```javascript
comet.on('ping', function(data) {
  comet.raise('pong', {'clientid': comet.client.id});
});
comet.on('message', function(data) {
  console.log("Message received: " + data['message']);
});
//Should the client ID be able to be automatically assigned by the server?
comet.start({'server': '/comet.php', 'clientid': 123});
```

