Run a simple C sample on sS2E
===
---

# Table of Contents
-   [Introduction](#Introduction)
-   [Step 1: Prerequisites](#Prerequisites)
-   [Step 2: Prepare your Device](#PrepareDevice)
-   [Step 3: Build Run the Sample](#Build)
-   [Next Steps](#NextSteps)

<a name="Introduction"></a>
# Introduction

**About this document**

This document describes how to connect sS2E board together to the Microsoft Azure IoT Hub, by leveraging on Azure IoT Device SDK. This multi-step process includes:
-   Configuring Azure IoT Hub
-   Registering sS2E to Azure IoT Hub
-   Build and deploy Azure IoT SDK on sS2E board
 

<a name="Prerequisites"></a>
# Step 1: Prerequisites

You should have the following items ready before beginning the process.

## Development environment
- Installed in your PC : [GCC Compiler][lnk-GCC]
- Firmware download tool : [MS500 Firmware Initializer][lnk-MS500-FI]
- A serial terminal installed in your PC (e.g. [TeraTerm][lnk-teraterm] for Windows) 
- [Setup your IoT hub][lnk-setup-iot-hub]
- [DeviceExplorer][lnk-dev-exp] (Windows OS only)
- [Provision your device and get its credentials][lnk-manage-iot-hub]


### NOTE
How to install the ARM-GCC environment is not described here. Please refer to the following site. https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads
 

<a name="PrepareDevice"></a>
# Step 2: Prepare your Device
sS2E as shown in the figure below. Then connect the sS2E board to your PC using a mini USB cable.
![][1]![][2]


<a name="Build"></a>
# Step 3: Build and Run the sample


<a name="Load"></a>
### 3.1 Build SDK sample code

1. Download [sS2E SDK Project]. The project contains all the required drivers to use the sS2E board, together with pre-integrated Microsoft Azure IoT SDK.
2. Unzip the package. Open one of the pre-configured project files available in MS500_BSP_CMSIS/Application/Azure.
3. Move to the MS500_BSP_CMSIS/Application/Azure project.
4. Open file AzureTest.c and update connectionString with the credentials retrieved once completed device registration in IoT Hub as described in [Step 1][lnk-setup-iot-hub]. You have also to set mac-address by replacing DEFAULT_MAC to open file ss2e_init.c for using gateway.
5. Build the project according to the ARM-GCC make. In MS500_BSP_CMSIS/Application/Azure/makefile -> run make
 

### 3.2 Upload firmware image

1. Follow the steps below to configure the board.
  - Set boot mode to 'UART to QSPI Boot'.
  ![][3]
  - Connect your sS2E I/O board and PC using the USB cable. 
  - Turn the sS2E main board and I/O board switches ON.
  - Check the connected COM port on PC.

2.	Run the MS500 Firmware Initializer program.
![][4]
3.	Check the COM Port and choose the BaudRate..
4.	Click 'Connect’ to connect the COM Port
5.	Select the boot mode to 'QSPI'
6.	Input the 'Firmware image' file path for QSPI boot mode. Click the button to select the sS2E firmware image.
7.	Check the 'FW Binary' path. Then, click 'FW Image Upload’.
8.	The progress bar will update as the FW image uploads.
9.	Once the 'FW Image Upload' is complete, a pop-up message on your program will indicate success.
10.	Click 'Disconnect’ to disconnect the COM Port. Then, remove the power cable.
11.	Change the set for the boot mode to 'ROM Boot' Then connect your sS2E board and power cable for board booting
   ![][5] 
12.	Check the FW image booting and operation.



### 3.3 Connect sS2E board to a gateway 

To visualize log messages from sS2E, configure your serial terminal (e.g. TeraTerm for Windows) with the following parameters
-	BaudRate : 115200
-	Data : 8 bit
-	Parity : none
-	Stop : 1 bit
-	Flow Control : none

Press RESET button onboard sS2E to restart the application; LED will blink once connection with Azure IoT Hub is established. Once connected to the IoT Hub, the application transmits periodically messages containing emulated sensors data. See Manage IoT Hub to learn how to observe in DeviceExplorer the messages IoT Hub receives from sS2E board.
Messages successfully transmitted to your Azure IoT Hub are also printed over your serial terminal interface.



### 3.4 Send messages to Azure IoT Hub
Once connected to the IoT Hub, the application transmits periodically messages containing inertial and environmental data read from emulated sensors onboard sS2E.
![][6]

### 3.5 Receive messages from IoT Hub

See Manage IoT Hub to learn how to send cloud-to-device messages from IoT Hub. Messages received by sS2E are printed over serial terminal interface once received. The following cloud-to-device messages are interpreted by the application:
![][7]



<a name="Nextsteps"></a>
# Next steps

You have now learned how to run a sample application that collects sensor data and sends it to your IoT hub. To explore how to store, analyze and visualize the data from this application in Azure using a variety of different services, please click on the following lessons:

-   [Manage cloud device messaging with iothub-explorer](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-explorer-cloud-device-messaging)
-   [Save IoT Hub messages to Azure data storage](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-store-data-in-azure-table-storage)
-   [Use Power BI to visualize real-time sensor data from Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)
-   [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)
-   [Weather forecast using the sensor data from your IoT hub in Azure Machine Learning](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-weather-forecast-machine-learning)
-   [Remote monitoring and notifications with ​​Logic ​​Apps](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)
-   [Device management with iothub-explorer](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-device-management-iothub-explorer)

[lnk-GCC]:https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads
[lnk-MS500-FI]:http://ewbmsvr.synology.me:5000/sharing/pPpTfG5yz
[lnk-sS2E-SDK-Project]:http://ewbmsvr.synology.me:5000/sharing/3NaYGU0mb

[lnk-setup-iot-hub]:https://github.com/neeraj-khanna/azure-iot-device-ecosystem/blob/master/setup_iothub.md
[lnk-manage-iot-hub]:https://github.com/neeraj-khanna/azure-iot-device-ecosystem/blob/master/manage_iot_hub.md
[lnk-teraterm]:https://ttssh2.osdn.jp
[lnk-iothub-explorer]:https://github.com/Azure/iothub-explorer
[lnk-direct-methods]:https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-direct-methods

[lnk-desired-prop]:https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-device-twins
[lnk-dev-man]:https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-device-management-overview
[lnk-dev-exp]:https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iot-exp]:https://github.com/Azure/iothub-explorer 



[1]: ./sS2E_Board.png
[2]: ./miniUSB.png
[3]: ./BootMode.png
[4]: ./MS500_Firmware_Initializer.png
[5]: ./RomBoot.png
[6]: ./DeviceToAzure.png
[7]: ./AzureToDevice.png

