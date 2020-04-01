# Create live dashboard on Node-RED

In this tutorial, you will create a live dashboard to display device acceleration data, enhance you dashboard to contains also linear acceleration data and add a rudimentary shake detector algorithm.

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
