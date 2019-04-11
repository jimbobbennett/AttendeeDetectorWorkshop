# Call the Web Api from the photo taking app

In the [previous step](./SaveTheResultsToADatabase.md) you created a Cosmos DB collection, and saved the results of the analysis into a database. In this step you will connect the photo taking app to the Web Api to test it out.

## Calling a Web Api

Web Apis can be called from Python, passing data to and receiving data from the Api. There is a package called `requests` that provides a simple to use way to call Web Apis.

## Install the requests package

* Open the `requirements.txt` file in Visual Studio Code.

* Add the following to the bottom of the file:

  ```python
  requests
  ```

* Save the file

* Install the new package from the terminal using the following command:
  
  ```sh
  pip install -r requirements.txt
  ```

## Write the code

* Open the `picturetaker.py` file in Visual Studio Code.

* Add an import for the requests package, as well as a system library below the other imports
  
  ```python
  import requests
  import binascii
  ```

* Add the following code just after the imports:
  
  ```python
  functionUrl = 'https://<Your Web App>.azurewebsites.net/image'

  def upload(frame):
    img = cv2.imencode('.jpg', frame)[1]
    data = {}
    data['image'] = binascii.b2a_base64(img).decode()
    requests.post(url=functionUrl, json=data)
  ```

* Update the `functionUrl` to be the URL of your Web App. Keep the `/image` part on the end.

* Add a call to this function before the `break`:
  
  ```python
  ...
  if k%256 == 32:
    upload(frame)
    break
  ...
  ```

## Run the code

* Start running the code. There are two ways to run this code:

  * Start debugging by either:
    * Select *Debug -> Start Debugging*
    * Press **F5**
    * Select the Debug pane from the toolbar on the left. Make sure the configuration is set to *Pyyhon: Current file* and select the green *Start Debugging* button.

    If you use one of these methods you will be able to set breakpoints and debug your code.

  * Run this directly from the Visual Studio Code terminal using the command
  
    ```sh
    python picturetaker.py
    ```

    If you use this method you will not be able to set breakpoints and debug your code.

* When the code runs, a window will appear showing the view from your camera. Press *Space* to take a picture with one or more faces in it and end the app.

The picture will be uploaded to the Web Api, faces detected and the results inserted into Cosmos DB. To verify it all worked, you can use the Cosmos DB explorer.

* Open the *Azure* tab, and expand the *COSMOS DB* section.

* Expand your Cosmos DB account, expand the database and the collection.
  
* In the collection, expand the documents node. You will see one document for every photo you've uploaded.

* Click on a document to see its contents. You will see the detected age, if the person was smiling and the emotion.
  
  ![The Cosmos DB explorer showing the document, and the document in a pane with an age of 44, no smile and neutral emotion](../Images/CosmosDocument.png)

* Try with different people, more than one person in a picture and faces showing different emotions and check the results.

## Next step

In this step you connected the photo taking app to the Web Api and tested it out, uploading a picture and seeing the results of the analysis in Cosmos DB. In the [next step](./ViewTheResults.md) you will create a web page to view the data from the database.