# Create IBM Cloud account and install IoTool apps

In this tutorial, you will create an account on IBM Cloud and install an application on your Android phone.

## Set up IBM Cloud account

In this section, you will create a free account on [IBM Cloud](https://cloud.ibm.com/) which will allow you create your own applications, services, databases, etc. in the cloud. Any email address can be used to create a free account that has enough resources for the purposes of this tutorial. In case you have an email address issued by one of the registered academic institutions, you can ask for more resources through the [IBM Academic Initiative](https://www.ibm.com/academic).

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

In this section, you will install the [IoTool](https://iotool.io/) app and its extensions on your Android phone. This app will read the sensor measurements and send them to IBM Cloud using the [MQTT](https://en.wikipedia.org/wiki/MQTT) protocol.

1. On your Android phone, open the **Play Store** app, search for **"IoTool"** and install the following apps:
    * [IoTool - Internet of Things (IoT) sensor platform](https://play.google.com/store/apps/details?id=io.senlab.iotoolapp)
    * [IoTool IBM Watson Cloud](https://play.google.com/store/apps/details?id=io.senlab.iotool.extension.ibmwatsoncloud)
1. Open the **IoTool** app and choose **allow** for every question it asks.
1. Familiarize yourself with the app by following the on-screen tutorial.
1. On the main app screen, where a 2x2 grid shows **AccelX**, **AccelY** and **AccelZ**, press the **Play** icon (triangle) on the top right to start a measurement. Move your phone and observe the numbers.
1. Click the 3 boxes with the acceleration readings **AccelX**, **AccelY** and **AccelZ** to enable a live graph. Move your phone and observe the live graph.
1. Press the **Stop** icon (square) on the top right to stop the measurement.
1. Long press the empty box to the right of **AccelZ** and select **Accelerometer Absolute Value** under the **Device internal** drop-down menu. Click the **AccelAbs** box to enable it in the live graph.
1. Press the **Play** icon (triangle) on the top right to start a new measurement. Observe that **AccelAbs** approximately corresponds to the norm of the acceleration `(AccelX, AccelY, AccelZ)` vector.
    * *Note*: Your screen should look [like this](../assets/IoTool-dashboard.png).
1. Press the **Stop** icon (square) on the top right to stop the measurement.
