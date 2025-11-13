# Hands-on Workshop Deploy Edge AI ML model Seeed Xiao ESP32s3 camera with Edge Impulse

This is a project that trains and deploy an object detection machine learning model to a Seeed Studio Xiao ESP32S3 Sense using Edge Impulse. 

This is part of the [IoT Barcelona meetup workshop from September 2025](https://www.meetup.com/iotbarcelona/events/310847002/).

And this is also part of the [IoT Orange County meetup workshop from November 2025](https://www.meetup.com/mfoc-meetup/events/311750530/).


## Getting Started

### Hardware

* [Seeed Studio Xiao ESP32S3 Sense](https://www.seeedstudio.com/XIAO-ESP32S3-Sense-p-5639.html)
* Micro SD Card
* USB-C cable

### Software

* [Arduino IDE](https://www.arduino.cc/en/software/) (latest version or Version: 2.3.6)
* [Edge Impulse free account](https://studio.edgeimpulse.com/signup?utm_medium=live_event&utm_source=conference&utm_campaign=21157154-new-user-acquisition_fall2025&utm_content=iotmeetup-github-tutorial)

  

## Train the Machine Learning model

The first step in training a machine learning model is gathering data. In this workshop, we will train the model to detect Edge Impulse socks, apples or rubber ducks among other objects on a table.

Important: For the best results, always train your model using the exact camera you will deploy in production and in the same conditions where the camera will be deployed. Here, that means the Xiao ESP32S3 Sense from Seeed Studio. However you can follow this workshop using your phone's camera (as Edge Impulse Studio supports mobile capture too).

To train the machine learning model with the Xiao ESP32S3 Sense camera we will collect images using the `Camera Web Server` example sketch available in the Arduino IDE after installing the ESP32 board packages. Once the sketch is running successfully in the hardware, you are going to be able to collect the images taken by the camera and store them locally on your computer before start uploading them to train a new ML object classification model.

Let's start!


### Getting started with the Hardware

If you purchased the Xiao ESP32S3 Sense from Seeed Studio, it will arrive disassembled in a small plastic bag with separate parts. Let's put it together before starting the project.

1. Attach the antenna - Connect the external antenna to the UFL connector on the XIAO ESP32S3 board.
2. Mount the camera module - Align the camera module with the connector board (located next to the UFL connector) and gently snap it into place.
3. Prepare and insert the MicroSD card
* Format a MicroSD card using the FAT32 file system.
* Insert it into the MicroSD card slot underneath the camera.

For a visual step-by-step guide, watch this assembly video.

[![Learn how to assemble the Xiao ESP32 S3 Sense](https://img.youtube.com/vi/6IZlnY3FD2c/1.jpg)](https://youtu.be/6IZlnY3FD2c?t=84)

Now your Xiao ESP32S3 Sense is now ready for the workshop!


### Arduino IDE

First of all, if you don't have the ESP32 libraries installed, go to the Arduino IDE preferences and add this URL in the `Additional boards manager URLs`: 

```
https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
```

Afterwards go to the Boards Manager and install the `ESP32` by Espressif Systems library. 

This is important, install the latest version (in my case the 3.3.0) if the Xiao ESP32S3 is the version 1.1. If your hardware is the v1.0 install the ESP32 library version 2.0.7.

Then go to `Tools > Board > esp32 > Xiao_ESP32S3`

Then you should be able to select your device from the Arduino IDE. Select the `XIAO_ESP32S3` from the devices selector. It's painful to find as it's not ordered alphabetically.

<img width="800" alt="Captura de pantalla 2025-09-12 a les 14 16 35" src="https://github.com/user-attachments/assets/142dd43e-7e5a-4e16-bfad-824d76af6273" />


#### Install the Camera Server example

Now we will deploy the `Camera Server` example in the device in order to take images from the camera and start training the machine learning model with Edge Impulse.

Go to `File > Examples > ESP32 > Camera > CameraWebServer` and click.

The new application will be opened in a new Arduino IDE window. Change the `ssid` and `password` credentials in the initial lines of the application to connect to your local WiFi. Use the same WiFi credentials where your computer is connected.

<img width="800" alt="Change the `ssid` and `password` in the initial lines of the application to connect to your local WiFi" src="https://github.com/user-attachments/assets/cf3c965e-44b8-4e41-9846-a044f2d1b7b9" />

Go to the file `board_config.h` and select the camera model `CAMERA_MODEL_XIAO_ESP32S3` instead of the default `CAMERA_MODEL_ESP_EYE`.

<img width="800" alt="Select the camera model CAMERA_MODEL_XIAO_ESP32S3" src="https://github.com/user-attachments/assets/32226719-b1da-4328-a104-da92653cf840" />

Afterthat, go to `Tools > PSRAM` and select `OPI PSRAM` instead of Disabled as this device is going to use the OPI PSRAM.

Now compile and Deploy. Once successfully deployed in the hardware, go to the Serial Monitor and copy the local IP address where the device is going to expose a tiny Web Server to stream and capture images.

Paste the local IP address into your browser connected to the same WiFi than the Xiao ESP32S3.

<img width="800" alt="Xiao ESP32S3 Web Server" src="https://github.com/user-attachments/assets/30257d13-cab7-4891-9265-0807344cf0fe" />

Now decide the ressolution that you want and click `Start stream`. My recommendation is 96x96, 320x240 or similar.

<img width="800" alt="Start Stream and Save images" src="https://github.com/user-attachments/assets/40f52cca-fad8-4850-8c6d-cad303dfda2a" />

Add the objects that you want to use to train the Machine Learning model in front of the camera and click `Save` to save locally in your computer the images taken from the stream. 

Once you will have collected multiple images (50-100) from the objects that you want to detect (in my case Edge Impulse socks, apples or Rubber Ducks) you can stop the application.


## Train and Deploy the Machine Learning model

Go to [Edge Impulse to create a new project](https://studio.edgeimpulse.com/signup?utm_medium=live_event&utm_source=conference&utm_campaign=21157154-new-user-acquisition_fall2025&utm_content=iotmeetup-github-tutorial-link2). 

Create a new project and go to add data. Upload the images that you have collected from the Web Server application from the Xiao ESP32S3. I used `Upload a folder` to upload them at once.

<img width="800" alt="Upload the images" src="https://github.com/user-attachments/assets/30f8d918-7914-4457-afc2-339853852998" />

Go to the `Labeling queue` and start labeling the objects in the images. Have in mind that you can use YoloV5 to detect objects in the image automatically in case that you want to detect objects available in the Yolo5 model.

<img width="800" alt="Start labeling the objects in the images" src="https://github.com/user-attachments/assets/3813d39a-0130-4542-95fc-330a2fee3000" />

Define the Impulse to detect objects in the images.

<img width="800" alt="Create the image impulse" src="https://github.com/user-attachments/assets/65902da8-778e-471c-819e-5d239a5441bd" />

Define the parameters and generate the features of the objects.

<img width="800" alt="Define paraments and generate the features" src="https://github.com/user-attachments/assets/a8482025-2b6b-4a42-8582-5e7c7ac0b43b" />

Train the ML model.

<img width="800" alt="Train the ML model" src="https://github.com/user-attachments/assets/a979d5c2-ae80-4990-a7c8-02da6a250db1" />

Deploy the model for Arduino with `TensorFlow Lite` model optimization instead of using any EON Tuner.

<img width="800" alt="Deploy the ML model" src="https://github.com/user-attachments/assets/00239b4b-01e0-4b13-83b4-858d6b0a5dd2" />

Download the `.zip` library and add it to the Arduino IDE. 

<img width="800" height="1080" alt="Captura de pantalla 2025-09-12 a les 16 24 40" src="https://github.com/user-attachments/assets/0c088a7c-6fba-4c21-a576-9036bfa1cf17" />

Add the XIAO ESP32S3 Camera pin definition copying and pasting in the code the following lines.

```
#define PWDN_GPIO_NUM     -1 
#define RESET_GPIO_NUM    -1 
#define XCLK_GPIO_NUM     10 
#define SIOD_GPIO_NUM     40 
#define SIOC_GPIO_NUM     39
#define Y9_GPIO_NUM       48 
#define Y8_GPIO_NUM       11 
#define Y7_GPIO_NUM       12 
#define Y6_GPIO_NUM       14 
#define Y5_GPIO_NUM       16 
#define Y4_GPIO_NUM       18 
#define Y3_GPIO_NUM       17 
#define Y2_GPIO_NUM       15 
#define VSYNC_GPIO_NUM    38 
#define HREF_GPIO_NUM     47 
#define PCLK_GPIO_NUM     13
```

Before deploying, go to `Tools > PSRAM` and select `OPI PSRAM` instead of Disabled as this device is going to use the OPI PSRAM.

And finally deploy the code to the hardware. Open the Serial monitor and test the objects detected.

<img width="547" height="84" alt="Captura de pantalla 2025-09-15 a les 15 22 16" src="https://github.com/user-attachments/assets/699a9244-d2c2-4529-a031-7b37623f64f0" />


## Troubleshooting

If you have any questions feel free to ask in the [Edge Impulse forum](https://forum.edgeimpulse.com) or in the [Edge Impulse Discord channel](https://www.edgeimpulse.com/blog/announcing-the-edge-impulse-developers-discord-server/#:~:text=this%20invite%20link%3A-,Edge%20Impulse%20Developers,-What%20is%20Discord).

## Attribution

* This project is in part working thanks of the work of Marcelo Rovai. Check his courses about [image classification](https://harvard-edge.github.io/cs249r_book/contents/labs/seeed/xiao_esp32s3/image_classification/image_classification.html) or [here](https://mjrovai.github.io/XIAO_Big_Power_Small_Board-ebook/chapter_4-4.html) to learn more.

* Also thanks to the [Seeed Studio documentation](https://wiki.seeedstudio.com/xiao_esp32s3_keyword_spotting/#getting-started), [Xiao ESP32S3 Sense documentation](https://docs.edgeimpulse.com/hardware/boards/seeed-xiao-esp32s3-sense) and tutorial such as this about [Fruit ripeness detection using Seeed Studio Xiao ESP32S3 Sense camera and Edge Impulse machine learning](https://markelthinkslearnscreates.wordpress.com/2025/02/14/fruit-ripeness-detection-using-seed-studio-xiao-esp32s3-sense-camera-and-edge-impulse-machine-learning/).

