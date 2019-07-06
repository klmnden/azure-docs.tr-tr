---
title: 'Hızlı Başlangıç: Node.js - konuşma Hizmetleri konuşma tanıma'
titleSuffix: Azure Cognitive Services
description: Speech SDK'sı için Node.js kullanarak bir konuşma metin konsol uygulaması oluşturmak için bu kılavuzu kullanın. İşiniz bittiğinde konuşmayı metne gerçek zamanlı dönüştürmek için bilgisayarınızın mikrofonunu kullanabilirsiniz.
services: cognitive-services
author: fmegen
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: fmegen
ms.openlocfilehash: 9d233de8a9cdd4b9a3637edcd1c6196b4ad16fd2
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67605125"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-nodejs"></a>Hızlı Başlangıç: Node.js için Speech SDK'sı ile Konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, Konuşmayı metne dönüştürme özelliği Speech SDK'sı Azure Bilişsel hizmetler için JavaScript bağlantısını kullanarak bir Node.js projesi oluşturma işlemini gösterir.
Uygulama dayanır [Speech SDK'sı için JavaScript](https://aka.ms/csspeech/npmpackage).

## <a name="prerequisites"></a>Önkoşullar

* Konuşma hizmeti için bir Azure aboneliği anahtarı. [Ücretsiz edinin](get-started.md).
* Güncel bir sürümünü [Node.js](https://nodejs.org).

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Yeni bir klasör oluşturun ve projeyi başlatın:

```sh
npm init -f
```

Bu komut başlatır **package.json** varsayılan değerlerle dosyaları. Daha sonra bu dosyayı düzenlemek isteyebilirsiniz.

## <a name="install-the-speech-sdk"></a>Konuşma SDK'sını yükleme

Speech SDK'sı için Node.js projenize ekleyin:

```
npm install microsoft-cognitiveservices-speech-sdk
```

Bu komut indirir ve Speech SDK'sı ve tüm gerekli önkoşulları en son sürümünü yükler **npmjs**. SDK'sını yükler `node_modules` projeniz klasörü içinde dizin.

## <a name="use-the-speech-sdk"></a>SDK'sı Konuşmayı kullanın

Adlı klasörde yeni bir dosya oluşturmak `index.js`ve bu dosyayı bir metin düzenleyiciyle açın.

> [!NOTE]
> Node.js ile Speech SDK'sı mikrofon desteklemiyor veya **dosya** veri türü. Her ikisi de yalnızca tarayıcılarda desteklenir. Bunun yerine, **Stream** arabirimi üzerinden konuşma SDK `AudioInputStream.createPushStream()` veya `AudioInputStream.createPullStream()`.

Bu örnekte `PushAudioInputStream` arabirimi.

Bu JavaScript kodunu ekleyin:

[!code-javascript[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/js-node/index.js#code)]

## <a name="run-the-sample"></a>Örneği çalıştırma

Uygulamayı açmak için uyum `YourSubscriptionKey`, `YourServiceRegion`, ve `YourAudioFile.wav` yapılandırmanıza. Ardından çalıştırın, bu komut çağırarak:

```sh
node index.js
```

Sağlanan dosya adını kullanarak bir tanıma tetikler. Ve konsol çıktısı sunar.

Programını çalıştırdığınızda, bu örnek çıkış alınır `index.js` abonelik anahtarını güncelleştirin ve bu dosyayı kullanın sonra `whatstheweatherlike.wav`:

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

Visual Studio Code'dan örnek de çalıştırabilirsiniz. Hızlı başlangıcı çalıştırın yüklemek ve açmak için aşağıdaki adımları izleyin:

1. Visual Studio Code'u başlatın. Seçin **Klasör Aç**. Ardından Hızlı Başlangıç klasörüne göz atın.

   ![Klasör Aç](media/sdk/qs-js-node-01-open_project.png)

1. Visual Studio Code'da bir terminal açın.

   ![Terminal penceresi](media/sdk/qs-js-node-02_open_terminal.png)

1. Çalıştırma `npm` bağımlılıklarını yükleyin.

   ![npm yükleme](media/sdk/qs-js-node-03-npm_install.png)

1. Açmaya hazırsınız artık `index.js`bir kesme noktası ayarlayın.

   ![16 satırında bir kesme noktası ile index.js](media/sdk/qs-js-node-04-setup_breakpoint.png)

1. Hata ayıklamayı başlatmak için F5'i seçin veya seçebilirsiniz **hata ayıklama/Start Debugging** menüsünde.

   ![Hata ayıklama menüsüne](media/sdk/qs-js-node-05-start_debugging.png)

1. Bir kesme noktası isabet edildiğinde, çağrı yığını ve değişkenleri inceleyebilirsiniz.

   ![Hata Ayıklayıcısı](media/sdk/qs-js-node-06-hit_breakpoint.png)

1. Herhangi bir çıkış, hata ayıklama konsol penceresinde gösterir.

   ![Konsol hata ayıklama](media/sdk/qs-js-node-07-debug_output.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Github'da node.js örneklerini keşfedin](https://aka.ms/csspeech/samples)
