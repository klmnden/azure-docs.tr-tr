---
title: 'Hızlı Başlangıç: Node.js - konuşma Hizmetleri konuşma tanıma'
titleSuffix: Azure Cognitive Services
description: Speech SDK'sı için Node.js kullanarak bir konuşma metin konsol uygulaması oluşturmak için bu kılavuzu kullanın. İşiniz bittiğinde konuşmayı metne gerçek zamanlı dönüştürmek için bilgisayarınızın mikrofonunu kullanabilirsiniz.
services: cognitive-services
author: fmegen
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 1/16/2019
ms.author: fmegen
ms.openlocfilehash: e0ae916687ca32835dd8daf6e5059b8f6eea0ff6
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2019
ms.locfileid: "54382176"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-nodejs"></a>Hızlı Başlangıç: Node.js için Speech SDK'sı ile Konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, Konuşmayı metne dönüştürme özelliği Bilişsel hizmetler konuşma SDK'sı JavaScript bağlantısını kullanarak bir Node.js projesi oluşturulacağını öğreneceksiniz.
Microsoft Web tabanlı uygulaması [Bilişsel hizmetler konuşma SDK'sı](https://aka.ms/csspeech/npmpackage).

## <a name="prerequisites"></a>Önkoşullar

* Konuşma hizmeti için bir Azure aboneliği anahtarı. [Ücretsiz edinin](get-started.md).
* Güncel bir sürümünü [Node.js](https://nodejs.org).

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Yeni bir klasör oluşturun ve bir proje başlatın.

```sh
npm init -f
```

Bu varsayılan değerleri olan package.json dosyaları init olur. Daha sonra bu dosyayı düzenlemek büyük olasılıkla isteyeceksiniz.

## <a name="install-the-speech-sdk"></a>Konuşma SDK'sını yükleme

Speech SDK'sı için Node.js projenize ekleyin.

```
npm install microsoft-cognitiveservices-speech-sdk
```

Bu indirmeleri ve npmjs Speech SDK'sı ve gerekli Önkoşullar en son sürümünü yükler. SDK'sı yüklenecektir `node_modules` projeniz klasörü içinde dizin.

## <a name="use-the-speech-sdk"></a>SDK'sı Konuşmayı kullanın

Klasörde `index.js` adlı yeni bir dosya oluşturun ve bu dosyayı bir metin düzenleyiciyle açın.

> [!NOTE]
> Node.js'de Speech SDK'sı mikrofon ya da dosya veri türünü desteklemediğini unutmayın. Her ikisi de yalnızca tarayıcılarda desteklenir. Bunun yerine, Stream arabirimi üzerinden konuşma SDK kullanın `AudioInputStream.createPushStream()` veya `AudioInputStream.createPullStream()`.

Bu örnekte, kullanacağız `PushAudioInputStream` arabirimi.

Aşağıdaki JavaScript kodunu ekleyin:

[!code-javascript[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/js-node/index.js#code)]

## <a name="run-the-sample"></a>Örneği çalıştırma

Uygulamayı başlatmak için uyum `YourSubscriptionKey`, `YourServiceRegion`, ve `YourAudioFile.wav` yapılandırmanıza. Ardından, aşağıdaki komutu çağırarak yürütebilirsiniz:

```sh
node index.js
```

Bu, sağlanan dosya adı kullanılarak bir tanıma tetikleyin ve konsol çıktısı sunmak.

Çalışan bir örnek çıktısı şöyledir `index.js` abonelik anahtarı güncelleştiriliyor ve dosyası kullanılarak sonra `whatstheweatherlike.wav`.

```json
SpeechRecognitionResult {
  "privResultId": "9E30EEBD41AC4571BB77CF9164441F46",
  "privReason": 3,
  "privText": "What's the weather like?",
  "privDuration": 15900000,
  "privOffset": 300000,
  "privErrorDetails": null,
  "privJson": {
    "RecognitionStatus": "Success",
    "DisplayText": "What's the weather like?",
    "Offset": 300000,
    "Duration": 15900000
  },
  "privProperties": null
}
```

## <a name="install-and-use-the-speech-sdk-with-visual-studio-code"></a>Yükleme ve Speech SDK'sı, Visual Studio Code ile kullanma

Visual Studio Code'dan örnek de çalıştırabilirsiniz. Yükleme, açın ve Hızlı Başlangıç'ı çalıştırmak için aşağıdaki adımları izleyin:

1. Visual Studio Code başlatın ve "Üzerinde klasör Aç"'a tıklayın, ardından Hızlı klasöre gidin

   ![Klasör Aç ekran görüntüsü](media/sdk/qs-js-node-01-open_project.png)

1. Visual Studio Code'da bir terminal açın

   ![Terminal penceresinin ekran görüntüsü](media/sdk/qs-js-node-02_open_terminal.png)

1. Npm bağımlılıkları yüklemek için çalıştırın

   ![Npm yükleme ekran görüntüsü](media/sdk/qs-js-node-03-npm_install.png)

1. Açmak artık `index.js`bir kesme noktası ayarlayın

   ![16 satırında bir kesme noktası ile index.js ekran görüntüsü](media/sdk/qs-js-node-04-setup_breakpoint.png)

1. Hata ayıklamayı başlatmak için F5'e basın veya hata ayıklama/Start Debugging menüden seçim yapın

   ![Hata ayıklama menüsünün ekran görüntüsü](media/sdk/qs-js-node-05-start_debugging.png)

1. Bir kesme noktası isabet edildiğinde, çağrı yığını ve değişkenleri inceleyebilirsiniz

   ![Hata ayıklayıcı ekran görüntüsü](media/sdk/qs-js-node-06-hit_breakpoint.png)

1. Herhangi bir çıkış hata ayıklama konsol penceresinde gösterilir.

   ![Hata ayıklama Konsolu ekran görüntüsü](media/sdk/qs-js-node-07-debug_output.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Github'da node.js örneklerini keşfedin](https://aka.ms/csspeech/samples)
