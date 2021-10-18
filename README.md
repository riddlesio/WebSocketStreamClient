# WebSocketStreamClient

A WebSocketClient that implements Client.h so that the PubCubClient MQTT library can use it - with wss or ws.

## Adjustments

The mosquitto websocket implementation requires the ***Sec-WebSocket-Protocol*** header. To achive this I needed to fork [arduino-libraries/ArduinoHttpClient](https://github.com/arduino-libraries/ArduinoHttpClient) to [riddlesio/ArduinoHttpClient](https://github.com/riddlesio/ArduinoHttpClient)


## Dependencies

* [Fork of ArduinoHttpClient]((https://github.com/riddlesio/ArduinoHttpClient) and all of it's dependencys. 
<s>Because this works with esp8266/2.4.2 but not with esp8266/2.5.0, use the supplied WebSocketClient250 class instead.</s>

WITH Board version 2.7.X : (don't work on previous versions)
no more use of websocketclient250 
(websocketstream updated to meet client.h declarations)

<!> IF the size of MQTT MESSAGE is > 128 bytes, the websocket hang
=> add (pubsubclient)   .setBufferSize(512)
=> change in websocket.h iTxBuffer[512]

<s>Tested with esp8266/2.5.0 AND ALSO 2.6.3 Board libraries. There is an example node.js server that you can use here with this library at [web-socket-mqtt](https://github.com/areve/web-socket-mqtt)</s>.


## Usage

See examples folder for a complete program.

```cpp

void onMqttPublish(char *topic, byte *payload, unsigned int length)
{
  // handle mqtt messages here
}

WiFiClientSecure wiFiClient;
WebSocketClient wsClient(wiFiClient, host, port);
WebSocketStreamClient wsStreamClient(wsClient, path, protocol);
PubSubClient mqtt(wsStreamClient);

wiFiClient.setFingerprint(fingerprint);
mqtt.setCallback(onMqttPublish);

if (mqtt.connect("your_identifier"))
{
  mqtt.publish("topic1", "hello world");
  mqtt.subscribe("topic2");
}

```
