---
title: Información sobre la mensajería de IoT Hub de Azure | Microsoft Docs
description: 'Guía del desarrollador: mensajería de dispositivo a nube y de nube a dispositivo con IoT Hub Incluye información sobre los formatos de mensaje y protocolos de comunicación compatibles.'
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: b0667f820145f16c75a07ebe1849e20d2de36cc7
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185516"
---
# <a name="device-to-cloud-and-cloud-to-device-messaging-with-iot-hub"></a>Mensajería de dispositivo a nube y de nube a dispositivo con IoT Hub

Use la mensajería de IoT Hub para comunicarse con los dispositivos de los siguientes modos:

* Enviando mensajes [de dispositivo a nube][lnk-d2c] desde los dispositivos al back-end de su solución.
* Enviando mensajes [de nube a dispositivo][lnk-c2d] desde la solución de back-end a sus dispositivos.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Las propiedades básicas de la funcionalidad de mensajería de IoT Hub son la confiabilidad y durabilidad de los mensajes. Estas propiedades permiten la resistencia a la conectividad intermitente en el dispositivo y a los picos de carga del procesamiento de eventos en la nube. IoT Hub implementa *al menos una vez* garantías de entrega para la mensajería del dispositivo a la nube y de la nube al dispositivo.

Para acceder a una introducción sobre las funcionalidades de IoT Hub, vea el artículo de [información general sobre el servicio Azure IoT Hub][lnk-iot-hub-overview].

## <a name="when-to-use-iot-hub-messaging"></a>Cuándo se debe usar la mensajería de IoT Hub

Los mensajes de dispositivo a nube se utilizan para enviar telemetría y alertas de series temporales desde la aplicación para dispositivo y los mensajes de nube a dispositivo, para las notificaciones unidireccionales a su aplicación para dispositivo.

* Si duda entre el uso de mensajes de dispositivo a nube, propiedades notificadas o carga de archivos, consulte la [Guía de comunicación de dispositivo a nube][lnk-d2c-guidance].
* Si duda entre el uso de mensajes de nube a dispositivo, propiedades deseadas o métodos directos, consulte la [Guía de comunicación de nube a dispositivo][lnk-c2d-guidance].

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información acerca de la [mensajería de dispositivo a nube][lnk-d2c] de IoT Hub.
* Obtenga más información acerca de la [mensajería de nube a dispositivo][lnk-c2d] de IoT Hub.

[lnk-azure-iot]: ../iot-fundamentals/index.yml
[lnk-iot-hub-overview]: about-iot-hub.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md