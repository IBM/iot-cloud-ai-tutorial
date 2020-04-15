# Build an AI classifier using IBM Watson Studio

In this tutorial, ...

## Create training data on Cloundant

In this section, we will create a Node-RED flow that stores the measured acceleration data into a Cloudant database. The sensor data is labelled with a Boolean class identifier that represents whether the device was being shaken or not during data collection.

1. Log in to [IBM Cloud](https://cloud.ibm.com/).
1. Click **View resources** to open your [Resource list](https://cloud.ibm.com/resources).
1. Expand the **Apps** menu and click the name of the entry whose **Offering** reads `Cloud Application`.
1. In the **App details** screen, click the **Visit App URL** link to open your Node-RED app.
1. In the Node-RED welcome screen, click the **Go to your Node-RED flow editor** and provide your login credentials to open the web IDE.
1. In the **Flow 1** tab, click the `mqtt in` node called **Subscribe to MQTT event** and press the keyboard shortcut associated with **Copy** in your operational system (`Ctrl+C` or `Cmd+C`). A pop-up message will confirm that 1 node was copied successfully.
1. Click the **Add Flow** button, a :heavy_plus_sign: icon (plus) to the right of the **Flow 1** tab, to create a new tab called **Flow 2**.
1. In the **Flow 2** tab, press the keyboard shortcut associated with **Paste** in your operational system (`Ctrl+V` or `Cmd+V`). Move your cursor to place the node in an empty area and confirm with a mouse click. A pop-down message will confirm that 1 node was imported successfully.
1. In the node menu on the left of the **Flow 2** tab, locate the `change` node under **Function**.
1. Drag and drop the `change` node to the editor tab.
1. Double-click the `change` node to open its configuration window.
1. In the **Properties** tab of the **Edit change node** window, enter the information below. To add new rules, click the `+ add` button in the bottom left of the window.
    * *Name*: `Build complete JSON object`.
    * *Rule 1*: Set (msg) `payload.class` to (number) `0`.
    * *Rule 2*: Set (msg) `payload` to the following (expression) content

        ```JSON
        msg.payload.{
            "time" : $millis(),
            "ax" : d."AccelerometerX@Device",
            "ay" : d."AccelerometerY@Device",
            "az" : d."AccelerometerZ@Device",
            "lax" : d."LinearAccelerationX@Device",
            "lay" : d."LinearAccelerationY@Device",
            "laz" : d."LinearAccelerationZ@Device",
            "a" : d."AccelerometerAbsolute@Device"
        }
        ````

1. Click **Done** to return to the flow editor.
1. In the node menu on the left, locate the `debug` node under **Common**.
1. Drag and drop the `debug` node to the editor tab.
1. In the node menu on the left, locate the `cloudant out` node under **Storage**.
1. Drag and drop the `cloudant out` node to the editor tab.
1. Double-click the `cloudant out` node to open its configuration window.
1. In the **Properties** tab of the **Edit cloudant out node** window, provide the following information below.
    * *Service*: If not already selected automatically, pick your Cloudant service from the menu.
    * *Database*: `training`.
    * *Only store msg.payload object?*: `ON`.
    * *Name*: `Training dataset`.
1. Click **Done** to return to the flow editor.
1. Connect the `mqtt in` node called **Subscribe to MQTT event** to the `change` node called **Build complete JSON object**.
1. Connect the `change` node called **Build complete JSON object** to the `debug` node called **msg.payload** and to the `cloudant out` node called **Training dataset**.
1. Click the **Deploy** button in the top right corner.

### Store class `0` examples in the training database

1. On your Android phone, open the **IoTool** app.
1. Place your phone horizontally on a flat surface, with the screen poiting upwards.
1. In the app main screen, press the **Play** icon (triangle) on the top right to start a measurement.
1. Let the experiment running for **60 seconds**.
1. On your Android phone, press the **Stop** icon (square) on the top right to stop the measurement.

### Store class `1` examples in the training database

1. Double-click the `change` node called **Build complete JSON object** to open its configuration window.
1. In the **Properties** tab of the **Edit change node** window, change first rule according to the instructions below.
    * *Rule 1*: Set (msg) `payload.class` to (number) `1`.
1. On your Android phone, open the **IoTool** app.
1. In the app main screen, press the **Play** icon (triangle) on the top right to start a measurement.
1. Shake your phone vigorously for **60 seconds**.
1. On your Android phone, press the **Stop** icon (square) on the top right to stop the measurement.

## Create IBM Watson Studio project

In this section, ...

## Train classifier model

In this section, ...

## Test classifier model

In this section, ...

## Deploy classifier model

In this section, ...

## Consume classifier model

In this section, ...
