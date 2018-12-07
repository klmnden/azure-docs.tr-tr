---
title: 'Hızlı Başlangıç: amaca dönüştürme-Node.js Al'
titleSuffix: Language Understanding - Azure Cognitive Services
description: Bu hızlı başlangıçta, kullanılabilir genel bir LUIS uygulamasını kullanarak konuşma metninden bir kullanıcının amacını belirleyin. Node.js kullanarak kullanıcının amacını genel uygulamanın HTTP tahmin uç noktasına metin olarak gönderin. Uç noktada, LUIS genel uygulamanın modelini uygulayarak anlam oluşturmak için doğal dil metnini analiz eder, böylece genel amacı belirler ve uygulamanın konu etki alanıyla ilgili verileri ayıklar.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 8a355b59a7674e21eddddbc311d90a9798931f89
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53012717"
---
# <a name="quickstart-get-intent-using-nodejs"></a>Hızlı Başlangıç: Node.js kullanarak amacı alma

Bu hızlı başlangıçta, amaç ve varlıkları döndürmek için bir LUIS uç noktasına konuşma iletin.

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Önkoşullar

* [Node.js](https://nodejs.org/) programlama dili 
* [Visual Studio Code](https://code.visualstudio.com/)
* Genel uygulama kimliği: df67dcdb-c37d-46af-88e1-8b97951ca1c2


> [!NOTE] 
> Node.js çözümünün tamamı [**LUIS-Samples** Github deposunda](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/analyze-text/node) mevcuttur.

## <a name="get-luis-key"></a>LUIS anahtarını alma

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>Amacı tarayıcı ile alma

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent-programmatically"></a>Amacı programlamayla alma

Node.js kullanarak bir önceki adımda tarayıcı penceresinde gördüğünüz sonuçlara ulaşabilirsiniz.

1. Aşağıdaki kod parçacığını kopyalayın:

   [!code-nodejs[Console app code that calls a LUIS endpoint for Node.js](~/samples-luis/documentation-samples/quickstarts/analyze-text/node/call-endpoint.js)]

2. Aşağıdaki metin ile `.env` dosyasını oluşturun veya sistem ortamında bu değişkenleri ayarlayın:

    ```CMD
    LUIS_APP_ID=df67dcdb-c37d-46af-88e1-8b97951ca1c2
    LUIS_ENDPOINT_KEY=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    ```

3. Anahtarınıza `LUIS_ENDPOINT_KEY` ortam değişkenini ayarlayın.

4. Komut satırında şu komutu çalıştırarak bağımlılıkları yükleyin: `npm install`.

5. Kodu `npm start` ile çalıştırın. Tarayıcı penceresinde daha önce gördüğünüz değerleri görüntüler.

## <a name="luis-keys"></a>LUIS anahtarları

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Node.js dosyasını silin.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Konuşma ekleme](luis-get-started-node-add-utterance.md)