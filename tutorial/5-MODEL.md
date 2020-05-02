# Build an AI classifier using IBM Watson Studio

In this tutorial, ...

## Store training data on Cloundant

In this section, you will create a Node-RED flow that stores the measured acceleration data into a Cloudant database. The sensor data is labelled with a Boolean `class` identifier that represents whether the device was being shaken or not during data collection.

1. Log in to [IBM Cloud](https://cloud.ibm.com/).
1. Click **View all** in the **Resource summary** card to open your [resource list](https://cloud.ibm.com/resources).
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

        ```Javascript
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
        ```

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
1. Make sure your Node-RED flow looks [like this](../assets/training-flow.png).

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
1. Click the **Deploy** button in the top right corner.
1. On your Android phone, open the **IoTool** app.
1. In the app main screen, press the **Play** icon (triangle) on the top right to start a measurement.
1. Shake your phone vigorously for **60 seconds**.
1. On your Android phone, press the **Stop** icon (square) on the top right to stop the measurement.

### Reset the training database

**Warning: Do not perform these steps unless you have invalid data in your training database!!!**

1. Log in to [IBM Cloud](https://cloud.ibm.com/).
1. Click **View all** in the **Resource summary** card to open your [resource list](https://cloud.ibm.com/resources).
1. Expand the **Services** menu and click the name of the entry whose **Offering** reads `Cloudant`.
    * *Note*: Do **not** click the **Cloud Foundry services** entry with the :link: icon after its name. This one is just an *alias* to the real **Service** entry.
1. In the next screen, click **Launch Cloundant Dashboard**.
1. In the **Databases** screen, locate the `training` database and click the rightmost trashbin icon in the **Actions** column.
1. Enter `training` as the name of the database and click **Delete Database**.
1. Redo the previous two steps to populate the training database with examples of both classes.

## Create IBM Watson Studio project

In this section, you will create an empty project on [IBM Watson Studio](https://www.ibm.com/cloud/watson-studio) alongside its companion [Cloud Object Storage](https://www.ibm.com/br-pt/cloud/object-storage) service instance. You will create an empty notebook to delevop a classifier model.

1. Log in to [IBM Cloud](https://cloud.ibm.com/).
1. Click **Create resource +** in the top right corner.
1. In the **Services** menu on the left, select the **AI** category.
1. Click the [Watson Studio](https://cloud.ibm.com/catalog/services/watson-studio) card.
1. Select the following options and then click **Create**.
    * *Region*: Choose the one closest to your location.
    * *Plan*: Lite.
1. In the next screen, click **Get Started** to open the IBM Watson Studio.
1. In the **Welcome** screen, click on **Create a project**.
1. In the **Create a project** screen, click on **Create an empty project**.
1. In the **New project** screen, you should configure your project by providing the information below.
    * *Define project details*: Give it a name and a description of your choosing.
    * *Define storage*: Choose an existing **Cloud Object Storage** (COS) service instance if you already have one. Otherwise, click **Add**. A new window will appear, in which you should select the **Lite** plan and then click **Create** and **Confirm**. Once you go back to the **New project** screen, click **Refresh** after a few moments to display the COS instance name.
1. In the **New project** screen, finish the project creation by clicking **Create**.
1. In the project overview page, click the **Add to project +** button on the top right.
1. In the **Choose asset type** window, pick **Notebook**.
1. In the **New notebook** screen, stay in the **Blank** tab and configure your notebook by providing the information below.
    * *Name*: Give it a name of your choosing.
    * *Select runtime*: `Default Spark Python 3.6 (Driver: 1 vCPU 4 GB RAM, 2 Executors: 1 vCPU 4 GB RAM)`.
1. Click **Create** on the bottom right.
1. After a short loading time, you should see a Jupyter-like notebook in your screen.

## Create Cloudant credentials for IBM Watson Studio

In this section, you will create a service credential to allow your IBM Watson Studio project access the data stored in your Cloudant database.

1. Log in to [IBM Cloud](https://cloud.ibm.com/).
1. Click **View all** in the **Resource summary** card to open your [resource list](https://cloud.ibm.com/resources).
1. Expand the **Services** menu and click the name of the entry whose **Offering** reads `Cloudant`.
    * *Note*: Do **not** click the **Cloud Foundry services** entry with the :link: icon after its name. This one is just an *alias* to the real **Service** entry.
1. In the next screen, choose **Service credentials** from the menu on the left side.
1. In the **Service credentials** window, click the **New credential +** button.
1. In the **Add new credential** window, change the name to `Credentials-WS` to make it easier to keep track of where each credential is being used. Click **Add** to confirm.
1. After a few seconds, a new entry should appear in the list. Click the twistie `>` to display its contents.
1. Make sure the JSON object has, at least, these **3 fields** below.

    ```JSON
    {
        "host": "CLOUDANT_HOST",
        "password": "CLOUDANT_PASSWORD",
        "username": "CLOUDANT_USERNAME"
    }
    ```

1. Copy the contents of the credentials JSON to a file for future use.

## Train classifier model

In this section, your will load the sensor data from Cloudant into a [Pandas](https://pandas.pydata.org/) `DataFrame` and use [PySpark](https://spark.apache.org/docs/latest/api/python/index.html) machine learning functions to train a logistic regression classifier model.

1. Log in to [IBM Cloud](https://cloud.ibm.com/).
1. Click **View all** in the **Resource summary** card to open your [resource list](https://cloud.ibm.com/resources).
1. Expand the **Services** menu and click the name of the entry whose **Offering** reads `Watson Studio`.
1. In the next screen, click **Get Started** to open the IBM Watson Studio.
1. In the **Welcome** screen, click on your project name in the **Recently updated projects** card.
1. Go to the **Assets** tab, and click the name of your notebook in the **Notebooks** card.
1. In the notebook visualisation screen, click the :pencil2: icon (pencil) in the top horizontal bar to enable editing.
1. In an empty cell, paste and execute (with `Shift+Enter`) the Python code below to install the [SQL-Cloundant Connector](https://developer.ibm.com/clouddataservices/docs/ibm-data-science-experience/integrate/use-python-notebook-to-load-cloudant-data-into-spark/).

    ```Python
    import pixiedust
    pixiedust.installPackage("org.apache.bahir:spark-sql-cloudant_2.11:0")
    ```

1. In an empty cell, paste and execute the Python code below to instantiate and start a Spark session.

    ```Python
    from pyspark.sql import SparkSession
    spark = SparkSession.builder.getOrCreate()
    ```

1. In an empty cell, paste and execute the Python code below to define the reader function. The `CLOUDANT_HOST`, `CLOUDANT_USERNAME` and `CLOUDANT_PASSWORD` variables are those defined in the `Credentials-WS` JSON file.

    ```Python
    def readDataFrameFromCloudant(database):

        cloudantdata = spark.read.format("org.apache.bahir.cloudant")\
            .option("cloudant.host",'CLOUDANT_HOST')\
            .option("cloudant.username", 'CLOUDANT_USERNAME')\
            .option("cloudant.password",'CLOUDANT_PASSWORD')\
            .load(database)

        return cloudantdata
    ```

1. In an empty cell, paste and execute the Python code below to load the data from Cloudant and display the class breakdown.
    * *Note*: It is desirable to have approximately the same number of entries for each class.

    ```Python
    df = readDataFrameFromCloudant('training')
    df.createOrReplaceTempView('df')
    spark.sql('select class, count(class) from df group by class').show()
    ```

1. In an empty cell, paste and execute the Python code below to display the dataset contents.
    * *Note*: Use the **Search table** text field to search for `NaN`.

    ```Python
    display(df)
    ```

1. In an empty cell, paste and execute the Python code below to import the machine learning libraries.

    ```Python
    from pyspark.ml.feature import StringIndexer, OneHotEncoder
    from pyspark.ml.linalg import Vectors
    from pyspark.ml.feature import VectorAssembler
    from pyspark.ml.feature import Normalizer
    from pyspark.ml import Pipeline
    from pyspark.ml.classification import LogisticRegression
    from pyspark.ml.evaluation import MulticlassClassificationEvaluator
    ```

1. In an empty cell, paste and execute the Python code below to train the machine learning model.

    ```Python
    vectorAssembler = VectorAssembler(inputCols=["a","ax","ay","az","lax","lay","laz"], outputCol="features")
    lr = LogisticRegression(maxIter=1000).setLabelCol("class")
    pipeline = Pipeline(stages=[vectorAssembler, lr ])
    model = pipeline.fit(df)
    result = model.transform(df)
    ```

1. In an empty cell, paste and execute the Python code below to print the parameters of the trained model.

    ```Python
    print('intercept = ', model.stages[1].intercept)
    print('coefficients = ', model.stages[1].coefficients)
    ```

1. In an empty cell, paste and execute the Python code below to evaluate the classification accuracy.

    ```Python
    binEval = MulticlassClassificationEvaluator().setMetricName("accuracy").setPredictionCol("prediction").setLabelCol("class")
    binEval.evaluate(result)
    ```

1. In an empty cell, paste and execute the Python code below to verify the classification predictions.

    ```Python
    new_df = readDataFrameFromCloudant('training')
    new_df.createOrReplaceTempView('new_df')
    result = model.transform(new_df)
    result.createOrReplaceTempView('result')
    spark.sql("select a, ax, ay, az, lax, lay, laz, class, prediction from result").show(50)
    ```

## Test classifier model

In this section, ...

## Deploy classifier model

In this section, ...

## Consume classifier model

In this section, ...
