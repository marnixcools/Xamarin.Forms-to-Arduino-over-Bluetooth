# Xamarin.Forms-to-Arduino-over-Bluetooth
A complete mobile cross-platform xamarin app architecture with basic bluetooth implementations to Arduino Bluetooth communication.


Notes:
This code is meant as a bootstrap for fast start up, it is by no means idealy written and is supposed to be edited according to youre specific project.
The toturial will show and demonstrate the architectural guidelines and provide a fast start up code for the mobile application.


Each platform has specific Interfaces and implementations for Bluetooth communication, therefor, in order to achieve flexible and reusable code, a OOP design is mandatory.


The entire project architecture:
 ![](Tutorial_Images/ArduinoApp_The_project_with_unimplemented.png)


Let's go over the diagram and I'll explain what we see.

The Blue sqaure are the classes implemented in te common logic. The yellow squares are the android implementation parts, and in red the IOS(currently not implemented).
On the upper left corner we have the name of the directory.
The arrows represent hiearchy levels or inheritance.

As the diagram shows, most of the code is implemented in the shared logic part of the project(Blue squares). Whereas the android has only the MainActivity(only couple lines of code added to this part of the code), and the AndroidBluetoothClient in which the implementing for the domain specific communication is implemented, which is a "Xamariny" implementation.

For simplicity and convience the explanation of the project will be devided into two parts - the "Bluetooth part" and the "usage/application part".
In the "Bluetooth part" we have: AndroidBluetoothClient which is a class with Android domain specific implementations for - connect , send message , Listen task for receiving messages. All of those, and some additional bluetooth functionalities are wrapped with the base class BaseBluetoothClient, wrapped by the interface IBluetoothClient(explanation for the necessity of BaseBluetoothClient will be provided later).

In the "application/usage part" we have two pages - "ConnectionPage" and "ControllerPage".
The "ConnectionPage" is the first page of the application, allowing you choose  device(from you're paired deviceses) and you may connect to it, if the connection is succeded you move on to the ControllerPage.
In the "ControllerPage" a connection has already been astblished. Therefore, the app is listening to incoming messages(if a message is received a toast will be shown on the screen) and you may send messages.
Now have an understanding of the Basic flow:
 ![](Tutorial_Images/ArduinoApp_Basic_flow.png)

# the Bluetooth part
In a nutshell, lets look at the properties and some functions of AndroidBluetoothClient.
   deviceIdToBluetoothDevice - a dictionary from string - deviceName to BluetoothDevice which is an Android object. We don't want to                                    Android objects in the common logic, so a dictionary from name to device is reasonable choice.
   myBluetoothAdapter - this can be thought of as the wrapper for the acual Bluetooth device
   btSocket - is "the connection itself"
   inputStream, outputStream - as you can see these are properties of the btSocket(BTW creating seprate variables for them is bad                                      practice but it helps explain the code)
   myUUID - the "mac adress" of the device
   
   List<string> GetPairedDeviceIds() - this function will inciate the dictionary *with the bond devices* mapping string to                                                      BluetoothDevice
   Listen() - is a "endless loop function" that basic listens to the inputstream and if something comes up - calls a "common logic                     function" - OnByteReceived((byte)inputByte) -  to raise the event
 
   Connect(string deviceId) - most of the function is self explanetory, but if you look closely, we use a Task for Listen().
                              the reason to do so is that Listen() is a "endless loop function", and we want it to be. we want to listen 
                              constantly for incoming messages, but we also want the screen to "endlesly listen" to us touching the                                   screen. This is exactly what tasks solve, and the reason for doing so here.

Now lets look at BaseBluetoothClient
     void OnByteReceived(byte receivedByte) - this function raises the event(reminder: called by Listen())

additional note - OnByteReceived is the reason we couldn't just have an interface and an implementation for the bluetooth client. We                       want a "common logic function" to raise the event, which needs to be implemented.

IBluetoothClient is self explantory from here I hope.
# The Clever Shtik
Now that we have all the generalizm we need 


# --- Under Construction --- 


Bluetooth further explantion:
 ![](Tutorial_Images/ArduinoApp_Bluetooth.png)

Whole program - send and receive flow example:
 ![](Tutorial_Images/ArduinoApp_Send_Receive_Example.png)

