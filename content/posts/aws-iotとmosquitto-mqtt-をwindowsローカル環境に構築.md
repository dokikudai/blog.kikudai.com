+++
date = 2020-04-15T15:00:00Z
draft = true
tags = ["iot"]
title = "AWS IoT、Mosquitto(MQTT)、Windowsローカル環境構築"

+++
## テーマ

AWS IoTを利用してローカルPCからMQTTメッセージをS3に保存してみる。

## 構築内容

構築時の環境、ミドルウェアのバージョン等

1. AWS IoT - [IoT Core](https://console.aws.amazon.com/iot/home)  
   MQTT Broker、Subscriberの利用
2. AWS S3  
   MQTT経由データの保存
3. MQTT - Mosquitto 1.69 （Windows 10 Pro 64bit）[Download | Eclipse Mosquitto](https://mosquitto.org/download/)  
   MQTT Publisherとして利用

![](/img/nlm/aws-iot-to-s3.png "MQTT - AWS IoT to S3")

### AWS IoT - IoT Core

### AWS S3

### MQTT - Mosquitto

[Download | Eclipse Mosquitto](https://mosquitto.org/download/) から

## 参考サイト

* MQTTの仕組み理解に  
  [世界一スケーラブルなMQTTブローカー - IoT Edge Connect - - Akamai Japan Blog](https://blogs.akamai.com/jp/2019/06/iot---iot-egge-connect--.html)
* MQTT構築・実践  
  [MQTT通信を試してみよう！](\[http://take6shin.blogspot.com/2019/01/mqtt.html)