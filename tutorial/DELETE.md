# Placeholder

## Set up IBM Cloud account

In this section you will create a free account on [IBM Cloud](https://cloud.ibm.com/) which will allow you create your own applications, services, databases, etc. in the cloud. Any email address can be used to create a free account that has enough resources for the purposes of this tutorial. In case you have an email address issued by one of the registered academic institutions, you can ask for more resources through the [IBM Academic Initiative](https://www.ibm.com/academic).

1. Log in to [IBM Cloud](https://cloud.ibm.com/). If you do not have an account, [create one](https://cloud.ibm.com/registration) using your email address.
1. (Optional) In case you have an email address issued by an academic institution, go to [IBM Academic Initiative](https://www.ibm.com/academic) portal.
    * Click **Register now** in the center of the screen.
    * Enter your academic institution issued email in the text box and click **Submit**.
    * Fill in the form and click **Register**.
    * Follow the instructions to activate your account.
    * Once logged in return to the [main page](https://www.ibm.com/academic), access **IBM Cloud** under **Technology** in the top bar.
    * In the next page, scroll down and open the **Software** tab.
    * Follow the instructions to retrieve your **IBM Cloud Feature Code**.
1. (Optional) In case you have been able to retrieve your **IBM Cloud Feature Code**, follow these steps to add it to your IBM Cloud account.
    * Return to [IBM Cloud](https://cloud.ibm.com/) and click **Manage** in the top bar, then **Account**.
    * In the left side menu, choose **Account settings**.
    * In the new page, scroll down for **Subscription and feature codes** and click **Apply code**.
    * Enter your **IBM Cloud Feature Code** and click **Apply**.

## Install Android apps

In this section you will install the [IoTool](https://iotool.io/) app and its extensions on your Android phone. This app will read the sensor measurements and send them to IBM Cloud using the [MQTT](https://en.wikipedia.org/wiki/MQTT) protocol.

1. On your Android phone, open the **Play Store** app, search for **"IoTool"** and install the following apps:
    * [IoTool - Internet of Things (IoT) sensor platform](https://play.google.com/store/apps/details?id=io.senlab.iotoolapp)
    * [IoTool IBM Watson Cloud](https://play.google.com/store/apps/details?id=io.senlab.iotool.extension.ibmwatsoncloud)
    * [IoTool Device Sensors](https://play.google.com/store/apps/details?id=io.senlab.iotool.serviceandroid)
1. Open the **IoTool** app and choose **allow** for every question it asks.
1. Familiarize yourself with the app by following the on-screen tutorial.
1. On the main app screen, where a 2x2 grid shows **AccelX**, **AccelY** and **AccelZ**, press the **Play** icon (triangle) on the top right to start a measurement. Move your phone and observe the numbers.
1. Click the 3 boxes with the acceleration readings in **X**, **Y** and **Z** to enable a live graph. Move your phone and observe the live graph.
1. Press the **Stop** icon (square) on the top right to stop the measurement.
1. Long press the empty box to the right of **AccelZ** and select **Accelerometer Absolute Value** under the **Device internal** drop-down menu. Click the **AccelAbs** box to enable it in the live graph.
1. Press the **Play** icon (triangle) on the top right to start a new measurement. Observe that **AccelAbs** approximately corresponds to the norm of the acceleration `(AccelX, AccelY, AccelZ)` vector.
    * *Note*: Your screen should look [like this](../assets/IoTool-dashboard.png).
1. Press the **Stop** icon (square) on the top right to stop the measurement.

## Create IBM Watson IoT Platform instance

In this section you will create an instance of the [IBM Watson IoT Platform](https://www.ibm.com/cloud/watson-iot-platform) service and register your Android device for secure communication over MQTT.

1. Log in to [IBM Cloud](https://cloud.ibm.com/).
1. Click **Create resource** in the top right corner.
1. In the **Services** tab, pick the **Internet of Things** entry in menu on the left side.
1. Click the [Internet of Things Platform](https://cloud.ibm.com/catalog/services/internet-of-things-platform) card.
1. Select the following options and click **Create**.
    * *Region*: Choose the one closest to your location.
    * *Plan*: Lite.
1. In the next screen, click **Launch** to open the IBM Watson IoT Platform.
1. In the **Browse Devices** screen, click on **Create a device**.
1. In the **Add Device** screen, fill in the form with the **Identity** information and then click **Next**.
    * *Device Type*: `android`.
    * *Device ID*: `phone`.
1. In the **Add Device** screen, the **Device Information** form contains *optional* fields that are not used for authentication. Fill in whatever you see fit, just for the purpose of differentiating between devices, then click **Next**.
1. In the **Add Device** screen, you can enter a custom **Authentication Token** or leave it blank for automatically generating one, then click **Next**.
1. Confirm the information displayed before clicking **Finish**.
1. **Write down** the **Device Credentials** information. *You cannot retrieve the token after you leave this page*.
    * *Organization ID*: this value will be referred to as `ORG_ID`.
    * *Device Type*: this value will be referred to as `DEV_TYPE`.
    * *Device ID*: this value will be referred to as `DEV_ID`.
    * *Authentication Method*: this value will be referred to as `AUTH_METHOD`.
    * *Authentication Token*: this value will be referred to as `AUTH_TOKEN`.

## Connect your device to IBM Watson IoT Platform

In this section you will configure your Android phone to connect to your IoT Platform instance over MQTT using the IoTool app. The MQTT communication protocol works under the *publish-subscribe* model. Device(s) can *publish* (send) data onto a `topic` that other device(s) can *subscribe* (listen) to.

1. On your Android phone, open the **IoTool** app.
1. Open the **Drawer** by clicking the **Hamburger** icon (three horizontal lines) on the top left corner and choose **Settings**.
1. In the **Settings** menu, choose **Sensors**.
1. Make sure the **Record readings** and **Continuous internal sensors data** options are `ON`.
1. Return to the **Settings** menu.
1. In the **Settings** menu, choose **Cloud**.
1. Make sure the **Use cloud**, **Send data** and **Sync after session** options are `ON`.
1. Go to **Select cloud service** and pick **IBM Watson IoT** from the list.
1. Open the **IBM Watson IoT Settings** and fill in the information below.
    * *Send interval*: `1s`.
    * *Hostname*: `ssl://ORG_ID.messaging.internetofthings.ibmcloud.com`.
    * *Port*: `8883`.
    * *Username*: `AUTH_METHOD`.
    * *Password*: `AUTH_TOKEN`.
    * *Client identification*: `d:ORG_ID:DEV_TYPE:DEV_ID`.
    * *Topic*: `iot-2/evt/accel/fmt/json`.
    * *Clean session*: `ON`.
    * *Quality of Service*: `2`.
    * *Condensed*: `OFF`.
1. Return to the app dashboard (main screen) and press the **Play** icon (triangle) on the top right to start a measurement.

### Understanding your MQTT topic

The `iot-2/evt/accel/fmt/json` MQTT topic string follows the format expected by IBM Watson IoT Platform and can be broken down as follows:

* `iot-2` is the prefix associated to the IBM Watson IoT Platform MQTT server.
* `evt` stands for *event* and represents the MQTT message type. There are two types of message: **events** and **commands**. In a nutshell, *events* are messages about something that happened in the past and *commands* are messages that trigger an action that should happen in the future.
* `accel` is the event ID string. This could be any string, but we chose `accel` because we are streaming *acceleration* data.
* `fmt` stands for **format**.
* `json` is the **format string**, which means that the message will be formatted in JSON.

## Monitor sensor data in the platform

In this section you will stream sensor data from your Android device to the cloud and monitor its contents in the IBM Watson IoT Platform.

1. Log in to [IBM Cloud](https://cloud.ibm.com/).
1. Click **View resources** to open your [Resource list](https://cloud.ibm.com/resources).
1. Expand the **Services** menu and click the name of the entry whose **Offering** reads `Internet of Things Platform`.
    * *Note*: Do **not** click the **Cloud Foundry services** entry with the :link: icon after its name. This one is just an *alias* to the real **Service** entry.
1. In the next screen, click **Launch** to open the IBM Watson IoT Platform.
1. In the **Browse Devices** screen, check if the status of your device is **Connected**.
1. Click your device entry and open the **Recent events** tab.
1. Wait for events to start showing and click one event to inspect its contents. You should see something like the JSON below.

    ```JSON
    {
        "d": {
            "AccelerometerX@Device": 0.8376312255859375,
            "AccelerometerY@Device": -0.297119140625,
            "AccelerometerZ@Device": 9.83221435546875,
            "AccelerometerAbsolute@Device": 9.87230190916425
            }
    }
    ```

1. Open the **State** tab, click the `>` icon to expand the contents of the `d` JSON object and observe the values changing in real time.
1. Click the white arrow icon (:arrow_right:) on the right side of the blue horizontal bar to see a different view on the same information in the **Device Drilldown** screen.

## Deploy Node-RED app

In this section you will create a [Node-RED](https://nodered.org/) app using [IBM Cloud Starter Kits](https://cloud.ibm.com/docs/apps?topic=creating-apps-starter-kits). The deployment will be automated using the [Continuous Delivery](https://www.ibm.com/garage/method/practices/deliver/tool_continuous_delivery/) cloud service.

1. Log in to [IBM Cloud](https://cloud.ibm.com/).
1. Click **Create resource** in the top right corner.
1. In the **Software** tab, pick the **Web and Application** entry in menu on the left side.
1. Click the **Node-RED app** card and then the **Create app** button in the top right corner.
1. Select the following options and click **Create**.
    * *Region*: Choose the one closest to your location.
    * *Plan*: Lite.
1. Wait for the provision of the Cloudant service to finish.
1. In the **App details** tab, click **Deploy your app** to configure the Continuous Delivery feature.
1. Choose **IBM Cloud Foundry** as deployment target.
1. Click the **New +** button and then **OK** to create an API key for your app.
1. Set the **Region** to the one closest to your location.
1. Click **Create** in the top right corner and wait while the code is deployed.
1. When the **Status** field in the Delivery Pipeline card reads **Success**, the deployment is done.

## Connect Node-RED app to IBM Watson IoT Platform

In this section you will connect the recently deployed [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry) application (i.e. your Node-RED app) to the Watson IoT Platform service. This will generate an API Key that will be used in the near future to authenticate your app.

1. Log in to [IBM Cloud](https://cloud.ibm.com/).
1. Click **View resources** to open your [Resource list](https://cloud.ibm.com/resources).
1. Expand the **Cloud Foundry apps** menu and click the name of the entry whose **Offering** reads `SDK for Node.js™`.
1. Go to the **Connections** tab in the left side menu and click the **Create connection +** button in the top right corner.
1. Locate the **Internet of Things Platform** entry and click the **Connect** button to the right.
1. In the new screen, click **Connect** to accept the automatic generation of a new API Key.
1. In the **Restage App** screen click the **Restage** button to reload the app with the new configuration.
1. Wait until the marker in the top center turns green and reads **Running**.
1. Go to the **Runtime** tab in the left side menu and click **Environment variables** in the centre of the screen.
1. In the **VCAP_SERVICES** text box, you will find a big JSON object with two smaller objects (`iotf-service` and `cloudantNoSQLDB`), each one with their own `credentials` object.
1. Look for the `credentials` object inside `iotf-service` and take note of its contents. You may copy and paste it somewhere else or **Export** the full JSON to a file.
1. The `credentials` object in `iotf-service` should have, at least, the following fields:

    ```JSON
    "credentials": {
        "apiKey": "API_KEY",
        "apiToken": "API_TOKEN",
        "http_host": "ORG_ID.internetofthings.ibmcloud.com",
        "mqtt_host": "ORG_ID.messaging.internetofthings.ibmcloud.com",
        "mqtt_s_port": 8883,
        "mqtt_u_port": 1883,
        "org": "ORG_ID"
    }
    ```

   * *Note*: If your `credentials` object does not have these fields above, your are probably looking at the credentials for the `cloudantNoSQLDB` service.

## Create a simple Node-RED app

In this section you will develop a simple application using the Node-RED web IDE. The application will receive acceleration data streamed from the device via MQTT.

1. Log in to [IBM Cloud](https://cloud.ibm.com/).
1. Click **View resources** to open your [Resource list](https://cloud.ibm.com/resources).
1. Expand the **Apps** menu and click the name of the entry whose **Offering** reads `Cloud Application`.
1. In the **App details** screen, click the **Visit App URL** link to open your Node-RED app.
1. Follow the instructions to protect your app from unwanted edits by providing a `username / password` pair. Click **Finish** when you are done.
1. In the Node-RED welcome screen, click the **Go to your Node-RED flow editor** and provide your login credentials to open the web IDE.
1. In the **Flow 1** editor tab, click and delete each one of the existing nodes to clear the screen.
1. In the node menu on the left, locate the `mqtt in` node under **Network**.
1. Drag and drop the `mqtt in` node to the editor tab.
1. Double-click the `mqtt in` node to open its configuration window.
1. In the **Properties** tab of the **Edit mqtt in node** window, click the :pencil2: icon (pencil) next to the **Server** options to configure the MQTT broker.
1. Fill in the **Connection** tab with the information below.
    * *Server*: `ORG_ID.messaging.internetofthings.ibmcloud.com`, as in `credentials.mqtt_host`.
    * *Port*: `8883`, as in `credentials.mqtt_s_port`.
    * *Client ID*: `a:ORG_ID:APP_ID` (where `ORG_ID` is `credentials.org` and `APP_ID` can be any string).
    * *Enable secure (SSL/TLS) connection*: `ON`.
1. Click the :pencil2: icon (pencil) next to **TLS Configuration** and fill the next screen with the information below.
    * *Verify server certificate*: `ON`.
    * *Server Name*: `ORG_ID.messaging.internetofthings.ibmcloud.com`, as in `credentials.mqtt_host`.
1. Click **Update** to close this window and return to the previous one.
1. Go to the **Security** tab and fill the fields with the information retrieved from the `credentials` JSON object.
    * *Username*: `API_KEY`, as in `credentials.apiKey`.
    * *Password*: `API_TOKEN`, as in `credentials.apiToken`.
1. Click **Update** to close this window and return to the previous one.
1. Back to the **Properties** tab, fill the rest of the fields with the information below.
    * *Topic*: `iot-2/type/DEV_TYPE/id/DEV_ID/evt/accel/fmt/json`.
    * *QoS*: `2`.
    * *Output*: `a parsed JSON object`.
    * *Name*: `Subscribe to MQTT event`.
1. Click **Done** to return to the flow editor.
1. In the node menu on the left, locate the `debug` node under **Common**.
1. Drag and drop the `debug` node to the editor tab.
1. Double-click the `debug` node to open its configuration window.
1. In the **Properties** tab of the **Edit debug node** window, change the **Output** field to `complete msg object`.
1. Click **Done** to return to the flow editor.
1. Click the gray circle on the right side of the `Subscribe to MQTT event` node and drag it to the gray circle on the left of the `msg` node. This will connect the two nodes.
1. Click the **Deploy** button in the top right corner.
1. In a few seconds a green marker should appear under the `Subscribe to MQTT event` node and display **connected** status.
1. Your Node-RED flow should look [like this](../assets/simple-flow.png).

## Test simple Node-RED app

In this section you will test the simple application you just created. The application will display the streamed data in the debug console.

1. Log in to [IBM Cloud](https://cloud.ibm.com/).
1. Click **View resources** to open your [Resource list](https://cloud.ibm.com/resources).
1. Expand the **Apps** menu and click the name of the entry whose **Offering** reads `Cloud Application`.
1. In the **App details** screen, click the **Visit App URL** link to open your Node-RED app.
1. In the Node-RED welcome screen, click the **Go to your Node-RED flow editor** and provide your login credentials to open the web IDE.
1. Click the :beetle: icon (bug) in the top right corner to open the **Debug** tab.
1. On your Android phone, open the **IoTool** app.
1. In the app dashboard (main screen), press the **Play** icon (triangle) on the top right to start a measurement.
1. On the Node-RED editor screen, note the JSON objects appearing periodically in the **Debug** tab on the right.
1. On your Android phone, press the **Stop** icon (square) on the top right to stop the measurement.
1. Pick one JSON object and click the `>` icon to expand it and open its contents. You should see something similar to the example below.

    ```JSON
    {
        "topic":"iot-2/type/DEV_TYPE/id/DEV_ID/evt/accel/fmt/json",
        "payload":{
            "d":{
                "AccelerometerX@Device":-0.935272216796875,
                "AccelerometerY@Device":5.72149658203125,
                "AccelerometerZ@Device":7.0712890625,
                "AccelerometerAbsolute@Device":9.14403555675171
            }
        },
        "qos":2,
        "retain":false,
        "_msgid":"afb1c702.e3e838"
    }
    ```

1. Double-click the `debug` node to open its configuration window.
1. In the **Properties** tab of the **Edit debug node** window, change the **Output** field to `msg.payload`.
1. Click **Done** to return to the flow editor.
1. Click the **Deploy** button in the top right corner.
1. On your Android phone, press the **Play** icon (triangle) on the top right to start a measurement.
1. On the Node-RED editor screen, note the JSON objects appearing periodically in the **Debug** tab on the right.
1. On your Android phone, press the **Stop** icon (square) on the top right to stop the measurement.
1. Pick one JSON object and click the `>` icon to expand it and open its contents. You should see something similar to the example below.

    ```JSON
    {
        "d":{
            "AccelerometerX@Device":-0.935272216796875,
            "AccelerometerY@Device":5.72149658203125,
            "AccelerometerZ@Device":7.0712890625,
            "AccelerometerAbsolute@Device":9.14403555675171
        }
    }
    ```

## Create live dashboard with acceleration data

In this section, you will create a live dashboard that will display the acceleration data streamed from the device in real-time. You will learn how to manipulate of the original `msg` object to extract the relevant information to be passed along to the dashboard nodes.

1. Log in to [IBM Cloud](https://cloud.ibm.com/).
1. Click **View resources** to open your [Resource list](https://cloud.ibm.com/resources).
1. Expand the **Apps** menu and click the name of the entry whose **Offering** reads `Cloud Application`.
1. In the **App details** screen, click the **Visit App URL** link to open your Node-RED app.
1. In the Node-RED welcome screen, click the **Go to your Node-RED flow editor** and provide your login credentials to open the web IDE.
1. Click the **Hamburger** icon (three horizontal lines) in the top right corner and pick **Manage palette**.
1. Go to the **Install** tab and search for `dashboard`.
1. Find the `node-red-dashboard` package and click the **Install** button to the right. Confirm the installation by clicking **Install** again.
1. Close the window to return to the **Flow 1** editor tab.
1. In the node menu on the left, locate the `change` node under **Function**.
1. Drag and drop the `change` node to the editor tab.
1. Double-click the `change` node to open its configuration window.
1. In the **Properties** tab of the **Edit change node** window, enter the information below. To add new rules, click the `+ add` button in the bottom left of the window.
    * *Name*: `Extract acceleration X`.
    * *Rule 1*: Set (msg) `payload` to (msg) `payload.d.AccelerometerX@Device`.
    * *Rule 2*: Set (msg) `topic` to (string) `X`.
1. Click **Done** to return to the flow editor.
1. Drag and drop another `change` node and fill it with the configuration below.
    * *Name*: `Extract acceleration Y`.
    * *Rule 1*: Set (msg) `payload` to (msg) `payload.d.AccelerometerY@Device`.
    * *Rule 2*: Set (msg) `topic` to (string) `Y`.
1. Click **Done** to return to the flow editor.
1. Drag and drop another `change` node and fill it with the configuration below.
    * *Name*: `Extract acceleration Z`.
    * *Rule 1*: Set (msg) `payload` to (msg) `payload.d.AccelerometerZ@Device`.
    * *Rule 2*: Set (msg) `topic` to (string) `Z`.
1. Click **Done** to return to the flow editor.
1. In the node menu on the left, locate the `chart` node under **Function**.
1. Drag and drop the `chart` node to the editor tab.
1. Double-click the `chart` node to open its configuration window.
1. In the **Properties** tab of the **Edit chart node** window, click the :pencil2: icon (pencil) next to the **Group** options.
1. In the **Edit dashboard group node** window, fill the *Name* field with `Acceleration` and click the :pencil2: icon (pencil) next to the **Tab** options.
1. In the **Edit dashboard tab node** window, fill the *Name* field with `Acceleration dashboard`.
1. Click **Update** twice to close these windows and return to **Edit chart node**.
1. Fill in the configuration with the information below.
    * *Label*: `Accelerometer [m/s^2]`.
    * *Enlarge points*: `ON`.
    * *X-axis*: Last `2 minutes` or `200` points.
    * *legend*: `Show`.
    * *Name*: `Accelerometer`.
1. In the node menu on the left, locate the `inject` node under **Common**.
1. Drag and drop the `inject` node to the editor tab.
1. Double-click the `inject` node to open its configuration window.
1. In the **Properties** tab of the **Edit inject node** window, fill in the form with information below.
    * *Payload*: (json) `[]`.
    * *Topic*: `clear`.
    * *Name*: `Clear graph`.
1. Connect the `mqtt in` node called **Subscribe to MQTT event** to each one of the 3 `change` nodes called **Extract acceleration X/Y/Z**.
1. Connect each one of the 3 `change` nodes called **Extract acceleration X/Y/Z** to the `chart` node called **Accelerometer**.
1. Connect the `inject` node called **Clear graph** to the `chart` node called **Accelerometer**.
1. Click the **Deploy** button in the top right corner.
1. Click the :bar_chart: icon (graph) in the top right corner to open the **Dashboard** tab.
1. Click the :arrow_upper_right: icon (open in new window) in the top right corner, to open the Node-RED dashboard in a new window.
    * *Note*: Your Node-RED flow should look [like this](../assets/accel-flow.png).
1. On your Android phone, open the **IoTool** app.
1. In the app main screen, press the **Play** icon (triangle) on the top right to start a measurement.
1. On the Node-RED dashboard screen, note the acceleration measurements appearing periodically in the chart.
1. On your Android phone, press the **Stop** icon (square) on the top right to stop the measurement.

## Enhance live dashboard with linear acceleration data

## Add rudimentary shake detection to live dashboard

## Settings

* Settings > Dashboard > 7 buttons: ax, ay, az, lax, lay, laz, abs
