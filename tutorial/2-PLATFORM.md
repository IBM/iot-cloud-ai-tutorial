# Use IBM Watson IoT Platform

In this tutorial, you will create an instance of the IBM Watson IoT Platform service, connect your device to it and monitor device data remotely.

## Create IBM Watson IoT Platform instance

In this section, you will create an instance of the [IBM Watson IoT Platform](https://www.ibm.com/cloud/watson-iot-platform) service and register your Android device for secure communication over MQTT.

1. Log in to [IBM Cloud](https://cloud.ibm.com/).
1. Click **Create resource +** in the top right corner.
1. In the **Services** menu on the left, select the **Internet of Things** category.
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

In this section, you will configure your Android phone to connect to your IoT Platform instance over MQTT using the IoTool app. The MQTT communication protocol works under the *publish-subscribe* model. Device(s) can *publish* (send) data onto a `topic` that other device(s) can *subscribe* (listen) to.

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

In this section, you will stream sensor data from your Android device to the cloud and monitor its contents in the IBM Watson IoT Platform.

1. Log in to [IBM Cloud](https://cloud.ibm.com/).
1. Click **View all** in the **Resource summary** card to open your [resource list](https://cloud.ibm.com/resources).
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
