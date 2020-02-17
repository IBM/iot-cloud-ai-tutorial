# Placeholder

## Setup IBM Cloud account

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

## Install Android Apps

In this section you will install the [IoTool](https://iotool.io/) app and its extensions on your Android phone. This app will read the sensor measurements and send them to IBM Cloud using the [MQTT](https://en.wikipedia.org/wiki/MQTT) protocol.

1. On your Android phone, open the **Play Store** app, search for **"IoTool"** and install the following apps:
    * [IoTool - Internet of Things (IoT) sensor platform](https://play.google.com/store/apps/details?id=io.senlab.iotoolapp)
    * [IoTool IBM Watson Cloud](https://play.google.com/store/apps/details?id=io.senlab.iotool.extension.ibmwatsoncloud)
    * [IoTool Device Sensors](https://play.google.com/store/apps/details?id=io.senlab.iotool.serviceandroid)
1. Open the **IoTool** app and choose **allow** for every question it asks.
1. Familiarize yourself with the app by following the on-screen tutorial.
1. On the main app screen, where a 2x2 grid shows **AccelX**, **AccelY** and **AccelZ**, press the **Play** icon (triangle) on the top right to start a measurement. Move your phone and observe the numbers.
1. Click the 3 boxes with the acceleration readings in **X**, **Y** and **Z** to enable a live graph. Move your phone and observe the live graph.
1. Press the **Stop** icon (square) to stop the measurement.
1. Long press the empty box to the right of **AccelZ** and select **Accelerometer Absolute Value** under the **Device internal** drop-down menu. Click the **AccelAbs** box to enable it in the live graph.
1. Press the **Play** icon (triangle) on the top right to start a new measurement. Observe that **AccelAbs** approximately corresponds to the norm of the acceleration `(AccelX, AccelY, AccelZ)` vector.
1. Press the **Stop** icon (square) to stop the measurement.

![IoTool-dashboard](assets/IoTool-dashboard.png)

## Create IBM Watson IoT Platform instance

In this section you will create an instance of the [IBM Watson IoT Platform](https://www.ibm.com/cloud/watson-iot-platform) service and register your Android device for secure communication over MQTT.

1. Log in to [IBM Cloud](https://cloud.ibm.com/).
1. Click **Create resource** in the top right corner.
1. In the **Services** tab, click the **Internet of Things** entry in menu on the left side.
1. Click the [Internet of Things Platform](https://cloud.ibm.com/catalog/services/internet-of-things-platform) card.
1. Select the following options and click **Create**.
    * *Region*: Frankfurt.
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

1. On your Android phone, open the **IoTool**.
1. Open the **Drawer** by clicking the **Hamburger** icon (three horizontal lines) on the top left corner and choose **Settings**.
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
* `json` is the format string, which means that the message will be formatted in JSON.

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
1. Click the white arrow icon (:arrow_right:) on the right of the blue horizontal bar to see a different view on the same information in the **Device Drilldown** screen.


## Settings

* Settings > Sensors > continuous internal sensors
* Settings > Dashboard > 7 buttons: ax, ay, az, lax, lay, laz, abs
