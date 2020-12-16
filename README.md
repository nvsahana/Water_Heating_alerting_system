# Water_Heating_alerting_system
Code Explanation

In the code, we first have to import our conf file which has all the credentials. 
The python json and time libraries are also imported in the same line. Since we have saved our conf file with the .py extension, we can directly import it.

import conf, json, time

json is a python library used for handling all operations on JSON objects. 
JSON is nothing but a data communication format widely used on the Internet for sending/receiving data between a client and server. More information on JSON can be found here.
Remember, 'json' is the python library used for handling JSON objects and JSON is a data communication format. 
Now we will import Bolt python library which will let us fetch the data stored in Bolt Cloud.
To send the SMS, the Sms library is also imported. The below line of code imports the required libraries.

from boltiot import Sms, Bolt

In the above line, we are importing two objects.
First one is SMS which will be used to send SMS alerts and the other one is Bolt which is used for accessing data from your Bolt device like the temperature reading.
We will create a new variable t and initialize it to int(), which defines what kind of data it can store.
Now we will initialize two variables which will storeintermediate and maximum threshold value. You can initialize the values 820 and 919 limits to them.
This would send an alert if the temperature reading above the intermediate limit and also when the temperature limit goes above the maximum limit value.

t=int() 
intermediate_limit = 820   #the limit when the water is heated and boiler can be switched off
maximum_limit = 919 #the limit after which water is going beyond its maximum limit.

Now to fetch the data from Bolt Cloud, we will create an object called 'mybolt' using which you can access the data on your Bolt.
For the Bolt Cloud to identify your device, you will need to provide the API key and the Device ID when creating the mybolt object. 
Since the conf file holds the API key and Device ID variables, you can use them as follows,

mybolt = Bolt(conf.API_KEY, conf.DEVICE_ID)

The above code will automatically fetch your API key and Device ID that you have initialized in conf.py file.
Now to send an SMS, we will create an object of the same.

sms = Sms(conf.SID, conf.AUTH_TOKEN, conf.TO_NUMBER, conf.FROM_NUMBER)

The above code will automatically fetch your SID, AUTH_TOKEN, TO_NUMBER and FROM_NUMBER that you have initialized in conf.py file. 
Make sure you have given correct value in conf.py file.
Since we want to continuously monitor the temperature reading, we will enclose our logic to fetch, compare and send the SMS inside an infinite loop using the
`while True:` statement.
An infinite loop is a special loop which executes its code continuously since its exit condition is never going to be valid.
To exit the loop, we will need to forcibly exit the code by holding CTRL + C.



In the rest of the program,
The code continuously fetches the temperature value using `analogRead` function.
Since the sensor is connected to A0 pin of the Bolt, we will execute the analogRead() function on the pin A0.
The response from the Bolt Cloud using the analogRead() function is in a JSON format, so we will need to load the JSON data sent by the cloud using Python's json library.
The temperature value is inside a field labelled as "value" in the response. We can access the JSON values using the statement `sensor_value = int(data['value'])`. 
This line also converts the sensor reading to integer data type for comparing the temperature range.
This is enclosed inside a try-except block to handle any error that may occur in the code.
The next line of code checks if the temperature reading is above the maximum limit or above the intermediate limit. If it exceeds, then the SMS will be sent.
The SMS to be sent will contain the text "Water is heated with a temperature of: " followed by the temperature in degree celcius value for intermediate limit.
The response from Twilio will be stored inside the `response` variable.
The SMS to be sent will contain the text "Water is heating beyond maximum limit!Curent temperature: " followed by the temperature in degree celcius value for maximum limit.
Once the temperature reading has been sent, we will need to wait for 10 seconds to get the next reading. For this, we will put the program to sleep once every loop iteration.
The statement `time.sleep(10)` puts the program execution on hold for 10 seconds. This means that the program would not execute for a period of 10 seconds.
In the above code, we are fetching the data every 10sec. You can change the value but ideally, it should be good if the time interval between 2 data points is more than 10sec.

View water_heating_alert.py for complete code.

