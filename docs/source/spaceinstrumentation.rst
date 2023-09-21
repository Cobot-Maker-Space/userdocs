Space Instrumentation
=====================

Over time, the Cobot Maker Space will become instrumented with a variety of sensors. The data from these sensors will be made available, both live and persisted. This will be an ongoing process that will be documented on this page. Access to the systems described here is only available through the Cobot Maker Space network, there is no general internet access.

Data Sources
^^^^^^^^^^^^

OpenHAB
^^^^^^^
Address:: 
    https://10.0.10.10

OpenHAB provides an integration point for a variety of smart home sensors, along with visulisation and interaction elements.


Sensor Types
^^^^^^^^^^^^
Currently, three types of sensors are connected to the OpenHAB system.

Motion Sensors
^^^^^^^^^^^^^^
Fibaro Motion sensors that provide the following sensor reading types:
    - Battery level, as a percentage.
    - Motion alarm, binary value, on for motion detected.
    - Luminance, in lux.
    - Seismic intensity, a number in the modified Mercalli scale.
    - Tamper alarm, binary value, on for tamper detected.
    - Temperature, in degrees Celcius.
    - Open/Close Sensors
    - Fibaro Door/Window sensors that provide the following sensor reading types:
    - Battery level, as a percentage.
    - Heat alarm, binary value, on for alarm.
    - Open/Close, binary value, on for open.
    - Power alarm, binary value, on for alarm.
    - Tamper alarm, binary value, on for tamper detected.
    - Temperature, in degrees Celcius.
    - Wall Plug Sensors
    - Fibaro Wall Plug sensors that provide the following sensor reading types:
    - Switch, binary value, the on/off status of the power switch.
    - Mains electric meter, power usage in kilo-Watt hours used by the mains socket.
    - Sensor electric meter, power usage in kilo-Watt hours used by the sensor itself.
    - USB electric meter, power usage in kilo-Watt hours used by the USB socket.
    - Mains electric meter, current power usage in Watts used by the mains socket.
    - Sensor electric meter, current power usage in Watts used by the sensor itself.
    - USB electric meter, current power usage in Watts used by the USB socket.


Live Data
^^^^^^^^^

MQTT Broker
Address: 10.0.10.12
TLS Port: 1883
Websocket Port: 9001
The broker does not require credentials for subscribing to published topics. TLS is set and will require you to trust the Cobot Maker Space root certificate.

Topics
^^^^^^
OpenHAB sensor readings are published to individual topics in the format instrumentation/<sensor measurement name>/state (e.g., instrumentation/WallPlugCoffeeMachine_Meter_Watts1_Mains/state).

Sample Python Client
Requires the Cobot Maker Space root certificate and the Paho MQTT client library.::

    import paho.mqtt.client as mqtt
 
    def on_connect(client, userdata, flags, rc):
        print(f"Connected with result code {rc}")
        client.subscribe("#")
 
    def on_message(client, userdata, msg):
        print(f"{msg.topic} {msg.payload}")
 
    client = mqtt.Client()
    client.on_connect = on_connect
    client.on_message = on_message
    client.tls_set(ca_certs="cobotmakerspace.org.crt")
    client.connect("10.0.10.12", 1883, 60)
    client.loop_forever()


Persisted Data
^^^^^^^^^^^^^^

InfluxDB
^^^^^^^^

IP Address.::
    10.0.10.11

Read only API token.:: 
    <AVAILABLE_ON_REQUEST>


This InfluxDB is used only for writing data from the OpenHAB system to and only read access is provided to other systems and users. If you want to explore using the InfluxDB UI for creating dashboards etc, then you will need to install your own instance and replicate the data to your installation. A quick way of getting an InfluxDB instance is to use Docker and the officical InfluxDB image