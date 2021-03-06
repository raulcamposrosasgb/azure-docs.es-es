---
title: Lenguaje de marcado de síntesis de voz
description: Uso el lenguaje de marcado de síntesis de voz para controlar la pronunciación y la prosodia en la conversión de texto a voz.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 09/08/2018
ms.author: v-jerkin
ms.openlocfilehash: 48e2f965cdc482f9070497f9a4e7e016bfeb92ea
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46978155"
---
# <a name="speech-synthesis-markup-language"></a>Lenguaje de marcado de síntesis de voz

El lenguaje de marcado de síntesis de voz (SSML) es un lenguaje de marcado basado en XML que proporciona una manera de controlar la pronunciación y la *prosodia* de la conversión de texto a voz. El término "prosodia" hace referencia al ritmo y al tono de voz (su música, si lo prefiere). Puede especificar palabras fonéticamente, proporcionar sugerencias para interpretar números, insertar pausas, controlar el tono, el volumen y la velocidad, etc.

Para obtener más información, consulte [Speech Synthesis Markup Language (SSML) Version 1.0](http://www.w3.org/TR/2009/REC-speech-synthesis-20090303/) [Lenguaje de marcado de síntesis de voz (SSML) versión 1.0] en W3C.

Los ejemplos siguientes muestran cómo usar SSML para satisfacer las necesidades de síntesis de voz comunes.

## <a name="add-a-break"></a>Adición de una pausa
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)'>
    Welcome to Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.
</voice> </speak>
```

## <a name="change-speaking-rate"></a>Cambio de la velocidad de habla
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)'>
<prosody rate="+30.00%">
    Welcome to Microsoft Cognitive Services Text-to-Speech API.
</prosody></voice> </speak>
```

## <a name="pronunciation"></a>Pronunciación
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)'>
    <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme>
</voice> </speak>
```

## <a name="change-volume"></a>Cambio de volumen
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
<prosody volume="+20.00%">
    Welcome to Microsoft Cognitive Services Text-to-Speech API.
</prosody></voice> </speak>
```

## <a name="change-pitch"></a>Cambio de tono
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
    <voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)'>
    Welcome to <prosody pitch="high">Microsoft Cognitive Services Text-to-Speech API.</prosody>
</voice> </speak>
```

## <a name="change-pitch-contour"></a>Cambio de curva melódica
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
<prosody contour="(80%,+20%) (90%,+30%)" >
    Good morning.
</prosody></voice> </speak>
```

## <a name="use-multiple-voices"></a>Uso de varias voces
```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
    Good morning!
</voice>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)'>
    Good morning to you too Jessa!
</voice> </speak>
```

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Vea cómo funciona el reconocimiento de voz en C#](quickstart-csharp-dotnet-windows.md)
