# Hands-on Workshop Deploy Edge AI ML model Seeed Xiao ESP32s3 camera with Edge Impulse

This is a project that trains and deploy an object detection machine learning model to a Seeed Studio Xiao ESP32S3 Sense using Edge Impulse. 

This is part of the [IoT Barcelona meetup workshop from September 2025](https://www.meetup.com/iotbarcelona/events/310847002/).


## Getting Started

### Hardware

* [Seeed Studio Xiao ESP32S3 Sense](https://www.seeedstudio.com/XIAO-ESP32S3-Sense-p-5639.html)
* SD Card
* USB-C cable

### Software

* [Arduino IDE](https://www.arduino.cc/en/software/)
* [Edge Impulse free account](https://studio.edgeimpulse.com/signup?utm_medium=live_event&utm_source=conference&utm_campaign=21157154-new-user-acquisition_fall2025&utm_content=iotmeetup-github-tutorial)

## Train the Machine Learning model

The first step to train a machine learning model is to collect data. In this case we are going to detect the Edge Impulse socks from other objects on a table. However it's highly recommendable to train the machine learning model with the camera that you are going to use in production. 

To train the machine learning model with the Xiao ESP32S3 camera we are going to use the camera web server example program available in the Arduino IDE. So we are going to collect the images taken by the camera and store them locally before start upload them to train a new ML model.

### Arduino IDE

First of all, go to the Arduino IDE preferences and add this URL in the `Additional boards manager URLs`: 

```
https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
```

Afterwards go to the Boards Manager and install the `ESP32` by Espressif Systems library. This is important, install the latest version (in my case the 3.3.0) if the Xiao ESP32S3 is the version 1.1. If your hardware is the v1.0 install the ESP32 library version 2.0.7.

Then go to Tools > Board > esp32 > Xiao_ESP32S3

<img width="500" alt="Tools > Board > esp32 > Xiao_ESP32S3" src="https://github.com/user-attachments/assets/5a6004f5-98a8-42b7-a41a-ac5d81e7a2d4" />

Then you should be able to select your device from the Arduino IDE. Select the `XIAO_ESP32S3` from the devices selector.

#### Install the Camera Server example

Now we will deploy the Camera Server example in the device in order to take images from the camera and start training the machine learning model with Edge Impulse.

Go to File > Examples > ESP32 > Camera > CameraWebServer and click.

<img width="500" alt="Go to File > Examples > ESP32 > Camera > CameraWebServer" src="https://github.com/user-attachments/assets/a9c254c3-e1f9-41db-a863-11e41d5d53e1" />

The new application will be opened in a new Arduino IDE window. Change the `ssid` and `password` credentials in the initial lines of the application to connect to your local WiFi.

<img width="500" alt="Change the `ssid` and `password` in the initial lines of the application to connect to your local WiFi" src="https://github.com/user-attachments/assets/cf3c965e-44b8-4e41-9846-a044f2d1b7b9" />

Go to the file `board_config.h` and select the camera model `CAMERA_MODEL_XIAO_ESP32S3` instead of the default `CAMERA_MODEL_ESP_EYE`.

<img width="500" alt="Select the camera model CAMERA_MODEL_XIAO_ESP32S3" src="https://github.com/user-attachments/assets/32226719-b1da-4328-a104-da92653cf840" />

Afterthat, go to Tools > PSRAM and select `OPI PSRAM` instead of Disabled as this device is going to use the OPI PSRAM.

Compile and Deploy and the most important is to copy the local IP address where the device is going to expose a tiny Web Server to stream and capture images.


## Deploy the Machine Learning model


## Troubleshooting


## Attribution

* This project is in part working thanks of the work of Marcelo Rovai. Check his courses about [image classification](https://harvard-edge.github.io/cs249r_book/contents/labs/seeed/xiao_esp32s3/image_classification/image_classification.html) to learn more.

* Also thanks to the [Seeed Studio documentation](https://wiki.seeedstudio.com/xiao_esp32s3_keyword_spotting/#getting-started), [Xiao ESP32S3 Sense documentation](https://docs.edgeimpulse.com/hardware/boards/seeed-xiao-esp32s3-sense) and tutorial such as this about [Fruit ripeness detection using Seeed Studio Xiao ESP32S3 Sense camera and Edge Impulse machine learning](https://markelthinkslearnscreates.wordpress.com/2025/02/14/fruit-ripeness-detection-using-seed-studio-xiao-esp32s3-sense-camera-and-edge-impulse-machine-learning/).

