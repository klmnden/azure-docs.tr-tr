---
title: LUIS ile Konuşma C# SDK'sını kullanma
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti ses almak ve JSON nesneleri LUIS tahmin dönmek için tek bir istek kullanmanıza olanak tanır. Bu makalede, indirin ve bir mikrofona bir utterance konuşmak ve LUIS tahmin bilgi almak için Visual Studio'da C# projesi kullanın. Proje zaten bir başvuru olarak dahil konuşma NuGet paketini kullanır.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.technology: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 14956fd716a6939d5e7dd9d670cc78b58adf7f45
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47042083"
---
# <a name="integrate-speech-service"></a>Konuşma hizmeti tümleştirin
[Konuşma hizmeti](https://docs.microsoft.com/azure/cognitive-services/Speech-Service/) ses almak ve JSON nesneleri LUIS tahmin dönmek için tek bir istek kullanmanıza olanak tanır. Bu makalede, indirin ve bir mikrofona bir utterance konuşmak ve LUIS tahmin bilgi almak için Visual Studio'da C# projesi kullanın. Konuşma kullandığından [NuGet](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech/) paket, bir başvuru olarak zaten eklendi. 

Bu makale için ücretsiz bir gereksinim [LUIS] [ LUIS] uygulamayı içeri aktarmak için Web sitesi hesabı.

## <a name="create-luis-endpoint-key"></a>LUIS uç nokta anahtarı oluşturma
Azure portalında [oluşturma](luis-how-to-azure-subscription.md#create-luis-endpoint-key) bir **Language Understanding** (LUIS) anahtarı. 

## <a name="import-human-resources-luis-app"></a>İnsan Kaynakları LUIS alma uygulaması
İnsan Kaynakları LUIS uygulaması kullanılabilir amacı ve bu makalede konuşma arasındadır [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples) Github deposu. İndirme [HumanResources.json](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/HumanResources.json) kaydedin *.json uzantılı bir dosya ve [alma](luis-how-to-start-new-app.md#import-new-app) LUIS içine. 

Bu uygulamanın amacı, varlıkları ve İnsan Kaynakları etki ilgili konuşma yok. Örnek konuşma şunlardır:

|Örnek konuşmalar|
|--|
|John Smith'in manager kimdir?|
|John Smith yöneten?|
|Form 123456 nerede?|
|Ücretli dilediğiniz zaman zorundayım?|


## <a name="add-keyphrase-prebuilt-entity"></a>Anahtar cümlesi ekleyin önceden oluşturulmuş varlık
Uygulamayı aldıktan sonra Seç **varlıkları**, ardından **önceden oluşturulmuş varlıklarla yönetme**. Ekleme **anahtar cümlesi** varlık. Anahtar cümlesi varlık anahtar konuya utterance ayıklar.

## <a name="train-and-publish-the-app"></a>Uygulamayı eğitme ve yayımlama
1. Üst, sağ gezinti çubuğunda **eğitme** LUIS uygulaması geliştirmek için düğme.

2. Seçin **Yönet** çubuğunun sağ üst bölümde seçip **anahtarları ve uç noktaları** sol gezinti bölmesinde. 

3. Üzerinde **anahtarları ve uç noktaları** sayfasında, oluşturduğunuz LUIS tuşu atama [oluşturma LUIS uç noktası anahtarı](#create-luis-endpoint-key) bölümü.

  Uygulama Kimliği bu sayfada toplamak, yayımlama bölge ve abonelik kimliği LUIS anahtarın oluşturulduğu [oluşturma LUIS uç noktası anahtarı](#create-luis-endpoint-key) bölümü. Bu makalenin sonraki bölümlerinde bu değerleri kullanmak için kodu değiştirmeniz gerekir. 
  
  Yapmak **değil** bu alıştırma için ücretsiz başlangıç anahtarı kullanın. Yalnızca bir **Language Understanding** Azure portalında oluşturulan anahtarı bu alıştırma için çalışır. 

  https://**bölge**.api.cognitive.microsoft.com/luis/v2.0/apps/**APPID**? abonelik anahtarı =**LUISKEY**& q =


4. Seçerek LUIS uygulaması yayımlama **Yayımla** çubuğunun sağ üst köşesindeki düğme. 

## <a name="audio-device"></a>Ses cihazı
Bu makalede, bilgisayarınızda ses cihazı kullanılmıştır. Kulaklık mikrofon veya yerleşik bir ses cihazı olabilir. Ses giriş düzeylerini ses cihazı tarafından algılanan konuşma için normalde çok daha yüksek sesle konuşurken, görmek için kontrol edin. 

## <a name="download-the-luis-sample-project"></a>LUIS örnek projeyi indirin
 Kopyala veya indir [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples) depo. Açık [konuşma niyetini projesine](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/tutorial-speech-intent-recognition) Visual Studio ile ve NuGet paketlerini geri yükleyin. VS çözüm.\LUIS-Samples-master\documentation-samples\tutorial-speech-intent-recognition\csharp\csharp_samples.sln dosyasıdır.

Speech SDK'sı zaten bir başvuru olarak dahil edilir. 

[![](./media/luis-tutorial-speech-to-intent/nuget-package.png "Visual Studio 2017 ekran görüntüleme Microsoft.CognitiveServices.Speech NuGet paketi")](./media/luis-tutorial-speech-to-intent/nuget-package.png#lightbox)

## <a name="modify-the-c-code"></a>C# kodu değiştirin
Açık **LUIS_samples.cs** dosyasını açıp aşağıdaki değişkenleri değiştirin:

|Değişken adı|Amaç|
|--|--|
|luisSubscriptionKey|Uç nokta URL'SİNİN abonelik anahtarı değerine Yayımla sayfasından karşılık gelir.|
|luisRegion|Uç nokta URL'SİNİN ilk alt etki alanı için karşılık gelen|
|luisAppId|Uç nokta URL'SİNİN yol aşağıdaki karşılık gelen **uygulamalar /**|

[![](./media/luis-tutorial-speech-to-intent/change-variables.png "Ekran görüntüsü, Visual Studio LUIS_samples.cs değişkenleri görüntüleme 2017")](./media/luis-tutorial-speech-to-intent/change-variables.png#lightbox)

Eşlenen İnsan Kaynakları ıntents dosya zaten var.

[![](./media/luis-tutorial-speech-to-intent/intents.png "Ekran görüntüsü, Visual Studio LUIS_samples.cs ıntents görüntüleme 2017")](./media/luis-tutorial-speech-to-intent/intents.png#lightbox)

Derleme ve uygulamayı çalıştırın. 

## <a name="test-code-with-utterance"></a>Utterance koduyla test
Seçin **1** ve mikrofona "Kim John Smith Yöneticisi".

```cmd
1. Speech recognition of LUIS intent.
0. Stop.
Your choice: 1
LUIS...
Say something...
ResultId:cc83cebc9d6040d5956880bcdc5f5a98 Status:Recognized IntentId:<GetEmployeeOrgChart> Recognized text:<Who is the manager of John Smith?> Recognized Json:{"DisplayText":"Who is the manager of John Smith?","Duration":25700000,"Offset":9200000,"RecognitionStatus":"Success"}. LanguageUnderstandingJson:{
  "query": "Who is the manager of John Smith?",
  "topScoringIntent": {
    "intent": "GetEmployeeOrgChart",
    "score": 0.617331
  },
  "entities": [
    {
      "entity": "manager of john smith",
      "type": "builtin.keyPhrase",
      "startIndex": 11,
      "endIndex": 31
    }
  ]
}

Recognition done. Your Choice:

```

Doğru amaç **GetEmployeeOrgChart**, %61 güvenle bulundu. Anahtar cümlesi varlık döndürdü. 

Speech SDK'sı LUIS yanıtın tamamını döndürür. 

## <a name="clean-up-resources"></a>Kaynakları temizleme
LUIS İnsanKaynakları uygulamayı artık gerekli değilse silin. Bunu yapmak için uygulama listesinde uygulama adının yanındaki üç nokta (***...***) düğmesini ve sonra da **Delete** (Sil) öğesini seçin. Açılan **Delete app?** (Uygulama silinsin mi?) iletişim kutusunda **Ok** (Tamam) öğesini seçin.

İşiniz bittiğinde LUIS örnekler dizini silmeyi unutmayın örnek kodu kullanarak.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [LUIS bir BOT ile tümleştirme](luis-csharp-tutorial-build-bot-framework-sample.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website