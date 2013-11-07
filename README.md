php-socket
==========

A PHP Comet Library

An example of how to use (server):

daemon.php
```php
<?php
$comet = new comet();
$comet->on('connection', function($client) {
  $client->raise('ping');
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
```

comet.php
```php
<?php
$session = new cometSession();
$session->process();
```

And an example of the JavaScript code:

```javascript
comet.on('ping', function(data) {
  comet.raise('pong', {'clientid': comet.client.id});
});
comet.on('message', function(data) {
  console.log("Message received: " + data['message']);
});
comet.connect({'server': '/comet.php'});
```

