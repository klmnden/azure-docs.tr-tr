---
title: "Hızlı Başlangıç: Konuşma hizmeti SDK'sını kullanarak node.js'de JavaScript dilinde konuşma tanıma"
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti SDK'sını kullanarak node.js'de JavaScript dilinde Konuşma tanımayı öğrenmesine
services: cognitive-services
author: fmegen
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 12/18/2018
ms.author: fmegen
ms.openlocfilehash: 35652b169067bc545fa0d1fcc977bbaee79ec3aa
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53724445"
---
# <a name="quickstart-recognize-speech-in-javascript-in-nodejs-using-the-speech-service-sdk"></a>Hızlı Başlangıç: Konuşma hizmeti SDK'sını kullanarak node.js'de JavaScript dilinde konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, Konuşmayı metne dönüştürme özelliği Bilişsel hizmetler konuşma SDK'sı JavaScript bağlantısını kullanarak bir Node.js projesi oluşturulacağını öğreneceksiniz.
Microsoft Web tabanlı uygulaması [Bilişsel hizmetler konuşma SDK'sı](https://aka.ms/csspeech/npmpackage).

## <a name="prerequisites"></a>Önkoşullar

* Konuşma hizmeti için bir Azure aboneliği anahtarı. [Ücretsiz edinin](get-started.md).
* Güncel bir sürümünü [Node.js](https://nodejs.org).

## <a name="create-a-new-project-folder"></a>Yeni bir proje klasörü oluşturun

Yeni, boş bir klasör oluşturun ve yeni bir JavaScript ve Node.js projesi olarak başlatılamıyor.

```sh
npm init -f
```

Bu varsayılan değerleri olan package.json dosyaları init olur. Daha sonra bu dosyayı düzenlemek büyük olasılıkla isteyeceksiniz.

## <a name="install-the-speech-sdk-for-javascript-into-that-folder"></a>Konuşma SDK için JavaScript bu klasöre yükleyin.

Konuşma SDK'sı aracılığıyla ekleme `npm install microsoft-cognitiveservices-speech-sdk` Node.js projenize.

Bu, indirin ve npmjs Speech SDK'sı ve gerekli Önkoşullar en son sürümünü yükleyin. SDK'sı yüklenecektir `node_modules` projeniz klasörü içinde dizin.

## <a name="using-the-speech-sdk"></a>Konuşma SDK'sını kullanma

Klasörde `index.js` adlı yeni bir dosya oluşturun ve bu dosyayı bir metin düzenleyiciyle açın.

> [!NOTE]
> Node.js'de Speech SDK'sı mikrofon ya da dosya veri türünü desteklemediğini unutmayın. Her ikisi de yalnızca tarayıcılarda desteklenir. Bunun yerine, Stream arabirimi üzerinden konuşma SDK kullanın `AudioInputStream.createPushStream()` veya `AudioInputStream.createPullStream()`.

Bu örnekte, kullanacağız `PushAudioInputStream` arabirimi.

Aşağıdaki JavaScript kodunu ekleyin:

[!code-javascript[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/js-node/index.js#code)]

## <a name="running-the-sample-from-command-line"></a>Örnek komut satırından çalıştırma

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

## <a name="running-the-sample-from-visual-studio-code"></a>Visual Studio Code'dan örneği çalıştırma

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
