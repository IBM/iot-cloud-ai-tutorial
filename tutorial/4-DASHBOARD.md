# Create live dashboard on Node-RED

In this tutorial, you will create a live dashboard to display device acceleration data, enhance you dashboard with linear acceleration data and add a rudimentary shake detector algorithm.

## Create live dashboard with acceleration data

In this section, you will create a live dashboard that will display the acceleration data streamed from the device in real-time. You will learn how to manipulate of the original `msg` object to extract the relevant information to be passed along to the dashboard nodes.

1. Log in to [IBM Cloud](https://cloud.ibm.com/).
1. Click **View all** in the **Resource summary** card to open your [resource list](https://cloud.ibm.com/resources).
1. Expand the **Apps** menu and click the name of the entry whose **Product** reads `Cloud Application`.
1. In the **App details** screen, click the **App URL** link to open your Node-RED app.
1. In the Node-RED welcome screen, click the **Go to your Node-RED flow editor** and provide your login credentials to open the web IDE.
1. Click the **Hamburger** icon (three horizontal lines) in the top right corner and pick **Manage palette**.
1. Go to the **Install** tab and search for `dashboard`.
1. Find the `node-red-dashboard` package and click the **Install** button to the right. Confirm the installation by clicking **Install** again in the pop-up screen.
1. Close the window to return to the **Flow 1** editor tab.
1. In the node menu on the left, locate the `change` node under **Function**.
1. Drag and drop the `change` node to the editor tab.
1. Double-click the `change` node to open its configuration window.
1. In the **Properties** tab of the **Edit change node** window, enter the information below. To add new rules, click the `+ add` button in the bottom left of the window.
    * *Name*: `Extract acceleration X`.
    * *Rule 1*: Set (msg) `payload` to (msg) `payload.d.AccelerometerX@StarterSensor`.
    * *Rule 2*: Set (msg) `topic` to (string) `X`.
1. Click **Done** to return to the flow editor.
1. Drag and drop another `change` node and fill it with the configuration below.
    * *Name*: `Extract acceleration Y`.
    * *Rule 1*: Set (msg) `payload` to (msg) `payload.d.AccelerometerY@StarterSensor`.
    * *Rule 2*: Set (msg) `topic` to (string) `Y`.
1. Click **Done** to return to the flow editor.
1. Drag and drop another `change` node and fill it with the configuration below.
    * *Name*: `Extract acceleration Z`.
    * *Rule 1*: Set (msg) `payload` to (msg) `payload.d.AccelerometerZ@StarterSensor`.
    * *Rule 2*: Set (msg) `topic` to (string) `Z`.
1. Click **Done** to return to the flow editor.
1. In the node menu on the left, locate the `chart` node under **Dashboard**.
1. Drag and drop the `chart` node to the editor tab.
1. Double-click the `chart` node to open its configuration window.
1. In the **Properties** tab of the **Edit chart node** window, fill in the configuration with the information below.
    * *Label*: `Accelerometer [m/s^2]`.
    * *Enlarge points*: `ON`.
    * *X-axis*: Last `2 minutes` or `200` points.
    * *legend*: `Show`.
    * *Name*: `Accelerometer`.
1. Click the :pencil2: icon (pencil) next to the **Group** options.
1. In the **Add new dashboard group config node** window, fill the *Name* field with `Acceleration` and click the :pencil2: icon (pencil) next to the **Tab** options.
1. In the **Add new dashboard tab config node** window, fill the *Name* field with `Acceleration dashboard`.
1. Click **Add** twice to close these windows and return to **Edit chart node**. Click **Done** to return to the flow editor.
1. In the node menu on the left, locate the `inject` node under **Common**.
1. Drag and drop the `inject` node to the editor tab.
1. Double-click the `inject` node to open its configuration window.
1. In the **Properties** tab of the **Edit inject node** window, fill in the form with information below.
    * *Payload*: (json) `[]`.
    * *Topic*: `clear`.
    * *Name*: `Clear graph`.
1. Click **Done** to return to the flow editor.
1. Connect the `mqtt in` node called **Subscribe to MQTT event** to each one of the 3 `change` nodes called **Extract acceleration X/Y/Z**.
1. Connect each one of the 3 `change` nodes called **Extract acceleration X/Y/Z** to the `chart` node called **Accelerometer**.
1. Connect the `inject` node called **Clear graph** to the `chart` node called **Accelerometer**.
1. Click the **Deploy** button in the top right corner.
1. Click the :bar_chart: icon (graph) in the top right corner to open the **Dashboard** tab and click the :arrow_upper_right: icon (open in new window) in the top right corner, to open the Node-RED dashboard in a new window.
    * *Note*: Your Node-RED flow should look [like this](../assets/accel-flow.png).

### Try it out: live dashboard

1. On your Android phone, open the **IoTool** app.
1. In the app main screen, press the **Play** icon (triangle) on the top right to start a measurement.
1. On the Node-RED dashboard screen, note the acceleration measurements appearing periodically in the chart.
1. On your Android phone, press the **Stop** icon (square) on the top right to stop the measurement.

## Enable linear acceleration sensors on your phone

In this section, you will add linear acceleration to the list of sensor measurements streamed by your phone.

1. On your Android phone, open the **IoTool** app.
1. Open the **Drawer** by clicking the **Hamburger** icon (three horizontal lines) on the top left corner and choose **Settings**.
1. In the **Settings** menu, choose **Dashboard**.
1. In the **No. of buttons** option, set **7** as the new value.
1. Set the **3 empty buttons** to:
    * *Button 5*: `Device internal > Linear Acceleration X`.
    * *Button 6*: `Device internal > Linear Acceleration Y`.
    * *Button 7*: `Device internal > Linear Acceleration Z`.
1. Return to the app dashboard (main screen) and press the **Play** icon (triangle) on the top right to start a measurement.
1. Click the 3 new boxes with linear acceleration readings **LAccX**, **LAccY** and **LAccZ** to enable a live graph. Move your phone and observe the live graph.
1. Press the **Stop** icon (square) on the top right to stop the measurement.

## Enhance live dashboard with linear acceleration data

In this section, you will add linear acceleration data to the live dashboard. You will learn how to copy, paste and edit existing nodes for better reuse of an existing flow.

1. Log in to [IBM Cloud](https://cloud.ibm.com/).
1. Click **View all** in the **Resource summary** card to open your [resource list](https://cloud.ibm.com/resources).
1. Expand the **Apps** menu and click the name of the entry whose **Product** reads `Cloud Application`.
1. In the **App details** screen, click the **App URL** link to open your Node-RED app.
1. In the Node-RED welcome screen, click the **Go to your Node-RED flow editor** and provide your login credentials to open the web IDE.
1. In the **Flow 1** tab, drag the cursor to select the **4 nodes** below.
    * `Extract acceleration X`.
    * `Extract acceleration Y`.
    * `Extract acceleration Z`.
    * `Accelerometer`.
1. After selecting the **4 nodes** mentioned above, press the keyboard shortcut associated with **Copy** in your operational system (`Ctrl+C` or `Cmd+C`). A pop-up message will confirm that those 4 nodes were copied successfully.
1. Press the keyboard shortcut associated with **Paste** in your operational system (`Ctrl+V` or `Cmd+V`). Move your cursor to place them in an empty area and confirm with a mouse click. A pop-down message will confirm that those 4 nodes were imported successfully.
1. Double-click the duplicated **Extract acceleration X** node to open its configuration window. In the **Properties** tab of the **Edit change node** window, **replace** the contents mentioned below and **keep** the contents of the unmentioned fields.
    * *Name*: `Extract linear acceleration X`.
    * *Rule 1*: (msg) `payload.d.LinearAccelerationX@StarterSensor`.
1. Click **Done** to return to the flow editor.
1. Double-click the duplicated **Extract acceleration Y** node to open its configuration window. In the **Properties** tab of the **Edit change node** window, **replace** the contents mentioned below and **keep** the contents of the unmentioned fields.
    * *Name*: `Extract linear acceleration Y`.
    * *Rule 1*: (msg) `payload.d.LinearAccelerationY@StarterSensor`.
1. Click **Done** to return to the flow editor.
1. Double-click the duplicated **Extract acceleration Z** node to open its configuration window. In the **Properties** tab of the **Edit change node** window, **replace** the contents mentioned below and **keep** the contents of the unmentioned fields.
    * *Name*: `Extract linear acceleration Z`.
    * *Rule 1*: (msg) `payload.d.LinearAccelerationZ@StarterSensor`.
1. Click **Done** to return to the flow editor.
1. Double-click the duplicated **Accelerometer** node to open its configuration window. In the **Properties** tab of the **Edit chart node** window, **replace** the contents mentioned below and **keep** the contents of the unmentioned fields.
    * *Label*: `Linear Accelerometer [m/s^2]`.
    * *Name*: `Linear Accelerometer`.
1. Click **Done** to return to the flow editor.
1. Connect the `mqtt in` node called **Subscribe to MQTT event** to each one of the 3 `change` nodes called **Extract linear acceleration X/Y/Z**.
1. Make sure that all 3 `change` nodes called **Extract linear acceleration X/Y/Z** are connected to the `chart` node called **Linear Accelerometer**.
1. Connect the `inject` node called **Clear graph** to the `chart` node called **Linear Accelerometer**.
1. Click the **Deploy** button in the top right corner.
1. Click the :bar_chart: icon (graph) in the top right corner to open the **Dashboard** tab and click the :arrow_upper_right: icon (open in new window) in the top right corner, to open the Node-RED dashboard in a new window.
    * *Note*: Your Node-RED flow should look [like this](../assets/linear-accel-flow.png).

### Try it out: enhanced live dashboard

1. On your Android phone, open the **IoTool** app.
1. In the app main screen, press the **Play** icon (triangle) on the top right to start a measurement.
1. Place your phone on a table and slide it laterally.
1. On the Node-RED dashboard screen, observe both the acceleration and linear acceleration measurements appearing periodically in the chart.
1. On your Android phone, press the **Stop** icon (square) on the top right to stop the measurement.

## Add rudimentary shake detection to live dashboard

In this section, you will add a rudimentary shake detection feature to the dashboard using the absolute acceleration as a figure-of-merit. When a shake is detected, a notification will be displayed.

1. Log in to [IBM Cloud](https://cloud.ibm.com/).
1. Click **View all** in the **Resource summary** card to open your [resource list](https://cloud.ibm.com/resources).
1. Expand the **Apps** menu and click the name of the entry whose **Product** reads `Cloud Application`.
1. In the **App details** screen, click the **App URL** link to open your Node-RED app.
1. In the Node-RED welcome screen, click the **Go to your Node-RED flow editor** and provide your login credentials to open the web IDE.
1. In the node menu on the left of the **Flow 1** tab, locate the `change` node under **Function**.
1. Drag and drop the `change` node to the editor tab.
1. Double-click the `change` node to open its configuration window.
1. In the **Properties** tab of the **Edit change node** window, enter the information below. To add new rules, click the `+ add` button in the bottom left of the window.
    * *Name*: `Extract absolute acceleration`.
    * *Rule 1*: Set (msg) `payload` to (msg) `payload.d.AccelerometerAbsolute@Device`.
    * *Rule 2*: Set (msg) `topic` to (string) `abs(A)`.
1. Click **Done** to return to the flow editor.
1. In the node menu on the left of the **Flow 1** tab, locate the `switch` node under **Function**.
1. Drag and drop the `switch` node to the editor tab.
1. Double-click the `switch` node to open its configuration window.
1. In the **Properties** tab of the **Edit switch node** window, enter the information below. To add new rules, click the `+ add` button in the bottom left of the window.
    * *Name*: `0.5g < abs(A) < 1.5g`.
    * *Rule 1*: Is between (number) `4.9` and (number) `14.7`.
    * *Rule 2*: Otherwise.
1. Click **Done** to return to the flow editor.
1. In the node menu on the left, locate the `gauge` node under **Dashboard**.
1. Drag and drop the `gauge` node to the editor tab.
1. Double-click the `gauge` node to open its configuration window.
1. In the **Properties** tab of the **Edit gauge node** window, fill in the configuration with the information below.
    * *Label*: `Absolute acceleration`.
    * *Value format*: `{{value | number:2}}`.
    * *Units*: `m^2/s`.
    * *Range*: min `0`, max `20`.
    * *Colour radient*: `red - green - red`.
    * *Sectors*: `0 - 4.9 - 14.7 - 20`.
    * *Name*: `Absolute acceleration`.
1. In the **Group** menu, select `Add new ui_group` and click the :pencil2: icon (pencil).
1. In the **Add new dashboard group config node** window, fill the *Name* field with `Shake detection` and click **Update**.
1. Click **Done** to return to the flow editor.
1. In the node menu on the left, locate the `notification` node under **Dashboard**.
1. Drag and drop the `notification` node to the editor tab.
1. Double-click the `notification` node to open its configuration window.
1. In the **Properties** tab of the **Edit notification node** window, fill in the configuration with the information below.
    * *Topic*: `Shake detected!`.
    * *Name*: `Shake notification`.
1. Click **Done** to return to the flow editor.
1. Connect the `mqtt in` node called **Subscribe to MQTT event** to the `change` node called **Extract absolute acceleration**.
1. Connect the `change` node called **Extract absolute acceleration** to the `switch` node called **0.5g < abs(A) < 1.5g** and to the `gauge` node called **Absolute acceleration**.
1. Connect the `Otherwise` (second) output of the `switch` node called **0.5g < abs(A) < 1.5g** to the `notification` node called **Shake notification**.
1. Click the **Deploy** button in the top right corner.
1. Click the :bar_chart: icon (graph) in the top right corner to open the **Dashboard** tab and click the :arrow_upper_right: icon (open in new window) in the top right corner, to open the Node-RED dashboard in a new window.
    * *Note*: Your Node-RED flow should look [like this](../assets/notification-accel-flow.png).

### Try it out: rudimentary shake detection

1. On your Android phone, open the **IoTool** app.
1. In the app main screen, press the **Play** icon (triangle) on the top right to start a measurement.
1. Shake your phone **gently**, at first, and then **vigorously**.
1. On the Node-RED dashboard screen, observe both the **Absolute acceleration** gauge meter and the notification in the top right corner whenever a shake is detected.
1. On your Android phone, press the **Stop** icon (square) on the top right to stop the measurement.
