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

<img width="800" alt="Tools > Board > esp32 > Xiao_ESP32S3" src="https://github.com/user-attachments/assets/a845e1cd-56e4-4840-bf60-e96b34548f16" />

Then you should be able to select your device from the Arduino IDE. Select the `XIAO_ESP32S3` from the devices selector.

#### Install the Camera Server example

Now we will deploy the Camera Server example in the device in order to take images from the camera and start training the machine learning model with Edge Impulse.

Go to File > Examples > ESP32 > Camera > CameraWebServer and click.

<img width="800" alt="Go to File > Examples > ESP32 > Camera > CameraWebServer and click" src="https://github.com/user-attachments/assets/09e4f72d-0cfe-4c13-bffe-4fedd44e1af3" />

The new application will be opened in a new Arduino IDE window. Change the `ssid` and `password` credentials in the initial lines of the application to connect to your local WiFi.

<img width="800" alt="Change the `ssid` and `password` in the initial lines of the application to connect to your local WiFi" src="https://github.com/user-attachments/assets/cf3c965e-44b8-4e41-9846-a044f2d1b7b9" />

Go to the file `board_config.h` and select the camera model `CAMERA_MODEL_XIAO_ESP32S3` instead of the default `CAMERA_MODEL_ESP_EYE`.

<img width="800" alt="Select the camera model CAMERA_MODEL_XIAO_ESP32S3" src="https://github.com/user-attachments/assets/32226719-b1da-4328-a104-da92653cf840" />

Afterthat, go to Tools > PSRAM and select `OPI PSRAM` instead of Disabled as this device is going to use the OPI PSRAM.

Compile and Deploy and the most important is to copy the local IP address where the device is going to expose a tiny Web Server to stream and capture images.

And paste the local IP address into your browser connected to the same WiFi than the Xiao ESP32S3.

<img width="800" alt="Xiao ESP32S3 Web Server" src="https://github.com/user-attachments/assets/30257d13-cab7-4891-9265-0807344cf0fe" />

Now decide the ressolution that you want and click `Start stream`. 

<img width="800" alt="Start Stream and Save images" src="https://github.com/user-attachments/assets/40f52cca-fad8-4850-8c6d-cad303dfda2a" />

And click `Save` to save locally in your computer the images taken from the stream. 

Once you will have collected multiple images from the objects that you want to detect (in my case Edge Impulse socks VS Seeed Studio Xiao plastic bags) you can stop streaming.


## Train and Deploy the Machine Learning model

Ideally try to collect as many images as possible from the objects that you want to detect and go to [Edge Impulse to create a new project](https://studio.edgeimpulse.com/signup?utm_medium=live_event&utm_source=conference&utm_campaign=21157154-new-user-acquisition_fall2025&utm_content=iotmeetup-github-tutorial-link2). 

Create a new project and go to add data. Upload the images that you have collected from the Web Server application from the Xiao ESP32S3.

<img width="800" alt="Upload the images" src="https://github.com/user-attachments/assets/30f8d918-7914-4457-afc2-339853852998" />

Go to the `Labeling queue` and start labeling the objects in the images. Have in mind that you can use YoloV5 to detect objects in the image automatically.

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

Add the XIAO ESP32S3 Camera pin definition.

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

And deploy the code to the hardware. Open the Serial monitor and test the objects detected.

<img width="547" height="84" alt="Captura de pantalla 2025-09-15 a les 15 22 16" src="https://github.com/user-attachments/assets/699a9244-d2c2-4529-a031-7b37623f64f0" />


## Troubleshooting

If you have any questions feel free to ask in the [Edge Impulse forum](https://forum.edgeimpulse.com) or in the [Edge Impulse Discord channel](https://www.edgeimpulse.com/blog/announcing-the-edge-impulse-developers-discord-server/#:~:text=this%20invite%20link%3A-,Edge%20Impulse%20Developers,-What%20is%20Discord).

## Attribution

* This project is in part working thanks of the work of Marcelo Rovai. Check his courses about [image classification](https://harvard-edge.github.io/cs249r_book/contents/labs/seeed/xiao_esp32s3/image_classification/image_classification.html) or [here](https://mjrovai.github.io/XIAO_Big_Power_Small_Board-ebook/chapter_4-4.html) to learn more.

* Also thanks to the [Seeed Studio documentation](https://wiki.seeedstudio.com/xiao_esp32s3_keyword_spotting/#getting-started), [Xiao ESP32S3 Sense documentation](https://docs.edgeimpulse.com/hardware/boards/seeed-xiao-esp32s3-sense) and tutorial such as this about [Fruit ripeness detection using Seeed Studio Xiao ESP32S3 Sense camera and Edge Impulse machine learning](https://markelthinkslearnscreates.wordpress.com/2025/02/14/fruit-ripeness-detection-using-seed-studio-xiao-esp32s3-sense-camera-and-edge-impulse-machine-learning/).

