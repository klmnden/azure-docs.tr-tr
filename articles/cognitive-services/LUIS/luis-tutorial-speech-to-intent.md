---
title: LUIS - Azure ile Konuşma C# SDK'sını kullanma | Microsoft Docs
titleSuffix: Azure
description: Mikrofona ve döndürülen LUIS amaç ve varlıkları Öngörüler elde etmek için örnek konuşma C# SDK'sını kullanın.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/26/2018
ms.author: diberry;
ms.openlocfilehash: 286efcd97c0c9ab95a8241215bc36799c486a8b6
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39247724"
---
# <a name="integrate-speech-service"></a>Konuşma hizmeti tümleştirin
[Konuşma hizmeti](https://docs.microsoft.com/azure/cognitive-services/Speech-Service/) ses almak ve JSON nesneleri LUIS tahmin dönmek için tek bir istek kullanmanıza olanak tanır.

Bu makalede, indirin ve bir mikrofona bir utterance konuşmak ve LUIS tahmin bilgi almak için Visual Studio'da C# projesi kullanın. Konuşma kullandığından [NuGet](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech/) paket, bir başvuru olarak zaten eklendi. 

Bu makale için ücretsiz bir gereksinim [LUIS] [ LUIS] uygulamayı içeri aktarmak için Web sitesi hesabı.

## <a name="create-luis-endpoint-key"></a>LUIS uç noktası anahtarı oluşturma
Azure portalında [oluşturma](luis-how-to-azure-subscription.md#create-luis-endpoint-key) bir **Language Understanding** (LUIS) anahtarı. 

## <a name="import-human-resources-luis-app"></a>İnsan Kaynakları LUIS alma uygulaması
İnsan Kaynakları LUIS uygulaması kullanılabilir amacı ve bu makalede konuşma arasındadır [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples) Github deposu. İndirme [HumanResources.json](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/HumanResources.json) kaydedin *.json uzantılı bir dosya ve [alma](luis-how-to-start-new-app.md#import-new-app) LUIS içine. 

Bu uygulamanın amacı, varlıkları ve İnsan Kaynakları etki ilgili konuşma yok. Örnek konuşma şunlardır:

```
Who is John Smith's manager?
Who does John Smith manage?
Where is Form 123456?
Do I have any paid time off?
```

## <a name="add-keyphrase-prebuilt-entity"></a>Anahtar cümlesi ekleyin önceden oluşturulmuş varlık
Uygulamayı aldıktan sonra Seç **varlıkları**, ardından **önceden oluşturulmuş varlıklarla yönetme**. Ekleme **anahtar cümlesi** varlık. Anahtar cümlesi varlık anahtar konuya utterance ayıklar.

## <a name="train-and-publish-the-app"></a>Uygulamayı eğitme ve yayımlama
1. Üst, sağ gezinti çubuğunda **eğitme** LUIS uygulaması geliştirmek için düğme.

2. Seçin **Yayımla** Yayımla sayfasına gidin. 

3. Sayfanın alt kısmında **Yayımla** sayfasında, oluşturduğunuz LUIS anahtarı eklemek [oluşturma LUIS uç noktası anahtarı](#create-luis-endpoint-key) bölümü.

4. LUIS uygulaması seçerek yayımlama **Yayımla** Yayımla yuvasının sağdaki düğme. 

  Üzerinde **Yayımla** sayfasında uygulama kimliğini, bölge ve abonelik kimliği oluşturulan LUIS anahtarının yayımlama [oluşturma LUIS uç noktası anahtarı](#create-luis-endpoint-key) bölümü. Bu makalenin sonraki bölümlerinde bu değerleri kullanmak için kodu değiştirmeniz gerekir. 

  Bu değerleri tüm uç nokta URL'sini sayfanın alt kısmında bulunan **Yayımla** oluşturduğunuz anahtarı için sayfa. 
  
  Yapmak **değil** bu alıştırma için ücretsiz başlangıç anahtarı kullanın. Yalnızca bir **Language Understanding** Azure portalında oluşturulan anahtarı bu alıştırma için çalışır. 

  https://**bölge**.api.cognitive.microsoft.com/luis/v2.0/apps/**APPID**? abonelik anahtarı =**LUISKEY**& q =

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
LUIS İnsanKaynakları uygulamayı artık gerekli değilse silin. Bunu yapmak için üç noktayı seçin (***...*** ) düğmesini seçin uygulama listesinde uygulama adının sağındaki **Sil**. Açılan **Delete app?** (Uygulama silinsin mi?) iletişim kutusunda **Ok** (Tamam) öğesini seçin.

İşiniz bittiğinde LUIS örnekler dizini silmeyi unutmayın örnek kodu kullanarak.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [LUIS bir BOT ile tümleştirme](luis-csharp-tutorial-build-bot-framework-sample.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website