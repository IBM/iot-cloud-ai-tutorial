# Use Node-RED programming tool

In this tutorial, you will deploy a Node-RED starter kit, connect it to IBM Watson IoT Platform, create and test your own app.

## Deploy Node-RED starter kit

In this section, you will create a [Node-RED](https://nodered.org/) app using [IBM Cloud Starter Kits](https://cloud.ibm.com/docs/apps?topic=creating-apps-starter-kits). The deployment will be automated using the [Continuous Delivery](https://www.ibm.com/garage/method/practices/deliver/tool_continuous_delivery/) cloud service.

1. Log in to [IBM Cloud](https://cloud.ibm.com/).
1. Click **Create resource +** in the top right corner.
1. Open the **Software** menu on the left, then select the **Starter kits** software category.
1. Click the **Node-RED App** card and then the **Get started** button in the bottom of the page.
1. Select the following options and click **Create**.
    * *Region*: Choose the one closest to your location.
    * *Plan*: Lite.
1. Wait for the provision of the Cloudant service to finish.
1. In the **Deployment Automation** card, click the **Deploy your app** button to configure the Continuous Delivery feature.
1. Choose **IBM Cloud Foundry** as deployment target.
1. Click the **New +** button and then **OK** to create an API key for your app.
1. Set the **Region** to the one closest to your location and hit **Next**.
1. In the next screen, click **Create** and wait while the code is deployed.
1. When the **Status** field in the **Deployment Automation** card reads **Success**, the deployment is finished.

## Connect Node-RED app to IBM Watson IoT Platform

In this section, you will connect the recently deployed [Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry) application (i.e. your Node-RED app) to the Watson IoT Platform service. This will generate an API Key that will be used in the near future to authenticate your app.

1. Log in to [IBM Cloud](https://cloud.ibm.com/).
1. Click **View all** in the **Resource summary** card to open your [resource list](https://cloud.ibm.com/resources).
1. Expand the **Cloud Foundry apps** menu and click the name of the entry whose **Product** reads `SDK for Node.js™`.
1. Go to the **Connections** tab in the left side menu and click the **Create connection +** button in the top right corner.
1. Select the **Internet of Things Platform** entry and click **Next** at the bottom right corner.
1. In the new screen, click **Connect** to accept the automatic generation of a new API Key.
1. In the **Restage App** pop-up window, click the **Restage** button to reload the app with the new configuration.
1. Wait until the marker in the top center turns green and reads **Running**.
1. Go to the **Runtime** tab in the left side menu and select the **Environment variables** tab in the centre of the screen.
1. Click the 👁️ icon to show the **`VCAP_SERVICES`** content and you will find a big JSON object with two smaller objects (`iotf-service` and `cloudantNoSQLDB`), each one with their own `credentials` object.
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

In this section, you will develop a simple application using the Node-RED web IDE. The application will receive acceleration data streamed from the device via MQTT.

1. Log in to [IBM Cloud](https://cloud.ibm.com/).
1. Click **View all** in the **Resource summary** card to open your [resource list](https://cloud.ibm.com/resources).
1. Expand the **Apps** menu and click the name of the entry whose **Product** reads `Cloud Application`.
1. In the **App details** screen, click the **Visit App URL** link to open your Node-RED app.
1. Follow the instructions to protect your app from unwanted edits by defining a `username / password` pair. Click **Finish** when you are done.
    * *Note*: Write down this `username / password` pair. You will need it in the future.
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

In this section, you will test the simple application you just created. The application will display the streamed data in the debug console.

1. Log in to [IBM Cloud](https://cloud.ibm.com/).
1. Click **View all** in the **Resource summary** card to open your [resource list](https://cloud.ibm.com/resources).
1. Expand the **Apps** menu and click the name of the entry whose **Product** reads `Cloud Application`.
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
