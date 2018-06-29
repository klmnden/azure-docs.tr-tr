---
title: C# SDK HALUK - Azure konuşarak | Microsoft Docs
titleSuffix: Azure
description: Konuşma C# SDK'sı örneği mikrofona ve döndürülen HALUK amacını ve varlıkları Öngörüler almak için kullanın.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/26/2018
ms.author: v-geberr;
ms.openlocfilehash: b681598f953d217ca636fb5c0adc3de4ddbebd60
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37031796"
---
# <a name="integrate-speech-service"></a>Konuşma hizmeti tümleştirmek
[Konuşma hizmet](https://docs.microsoft.com/azure/cognitive-services/Speech-Service/) ses almak ve JSON nesnelerinin HALUK tahmin dönmek için tek bir istek kullanmanıza olanak sağlar.

Bu makalede, indirin ve mikrofon içine bir utterance seslendir ve HALUK tahmin bilgi almak için Visual Studio'da bir C# projesi kullanın. Konuşma projenin kullandığı [NuGet](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech/) paket, önceden bir başvuru olarak eklendi. 

Bu makalede, ücretsiz bir gereksinim duyduğunuz [HALUK] [ LUIS] uygulama içe aktarmak için Web sitesi hesabı.

## <a name="create-luis-endpoint-key"></a>HALUK uç noktası anahtarı oluşturma
Azure portalında [oluşturma](luis-how-to-azure-subscription.md#create-luis-endpoint-key) bir **dil anlama** (HALUK) anahtarı. 

## <a name="import-human-resources-luis-app"></a>İnsan Kaynakları HALUK Al uygulama
Kullanılabilir İnsan Kaynakları HALUK uygulamadan amaçları ve bu makalenin utterances olan [HALUK-Samples](https://github.com/Microsoft/LUIS-Samples) Github depo. Karşıdan [HumanResources.json](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/HumanResources.json) dosya *.json uzantısıyla kaydedin ve [alma](create-new-app.md#import-new-app) HALUK içine. 

Bu uygulamanın amaçları, varlıkları ve İnsan Kaynakları etki alanına ilgili utterances yok. Örnek utterances şunları içerir:

```
Who is John Smith's manager?
Who does John Smith manage?
Where is Form 123456?
Do I have any paid time off?
```

## <a name="add-keyphrase-prebuilt-entity"></a>Anahtar cümlesi eklemek önceden oluşturulmuş varlık
Uygulama aldıktan sonra Seç **varlıklar**, ardından **önceden oluşturulmuş varlıkları Yönet**. Ekleme **anahtar cümlesi** varlık. Anahtar cümlesi varlık utterance anahtar konuyla ilgili ayıklar.

## <a name="train-and-publish-the-app"></a>Eğitmek ve uygulama yayımlama
1. Üst, sağ gezinti çubuğunda seçin **eğitmek** düğmesi HALUK uygulama eğitmek için.

2. Seçin **Yayımla** Yayımla sayfasına gidin. 

3. Ekranın alt kısmındaki **Yayımla** sayfasında, eklemek oluşturulan HALUK anahtar [oluşturma HALUK uç noktası anahtarı](#create-luis-endpoint-key) bölümü.

4. Seçerek HALUK uygulamayı yayımlama **Yayımla** Yayımla yuvası sağdaki düğme. 

  Üzerinde **Yayımla** sayfasında, uygulama kimliği toplamak, bölgeye ve abonelik kimliği oluşturulan HALUK anahtarının yayımlama [oluşturma HALUK uç noktası anahtarı](#create-luis-endpoint-key) bölümü. Bu makalenin sonraki bölümlerinde bu değerleri kullanmak için kodu değiştirmeniz gerekir. 

  Bu değerler tüm alt kısmındaki uç nokta URL'si dahil edilen **Yayımla** oluşturduğunuz anahtarı için sayfa. 
  
  Yapmak **değil** bu alıştırma için ücretsiz başlangıç anahtarı kullanın. Yalnızca bir **dil anlama** Azure Portal'da oluşturulan anahtarı bu alıştırma için çalışır. 

  https://**bölge**.api.cognitive.microsoft.com/luis/v2.0/apps/**APPID**? abonelik anahtarı =**LUISKEY**& q =

## <a name="audio-device"></a>Ses aygıtı
Bu makalede, bilgisayarınızda ses aygıtını kullanır. Kulaklık mikrofon veya yerleşik bir ses aygıtı sahip olabilir. Ses aygıt tarafından algılanan konuşmanızı sağlamak için normalde verilenden daha yüksek sesle konuşurken varsa görmek için ses giriş düzeylerini denetleyin. 

## <a name="download-the-luis-sample-project"></a>HALUK örnek projenizi indirin
 Kopyalama veya indirme [HALUK-Samples](https://github.com/Microsoft/LUIS-Samples) deposu. Açık [hedefi projesine konuşma](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/tutorial-speech-intent-recognition) Visual Studio ile ve NuGet paketleri geri yükleyin. VS çözüm.\LUIS-Samples-master\documentation-samples\tutorial-speech-intent-recognition\csharp\csharp_samples.sln dosyasıdır.

Konuşma SDK'sı zaten bir başvuru olarak dahil edilir. 

[![](./media/luis-tutorial-speech-to-intent/nuget-package.png "Visual Studio 2017 ekran görüntüsü görüntüleme Microsoft.CognitiveServices.Speech NuGet paketi")](./media/luis-tutorial-speech-to-intent/nuget-package.png#lightbox)

## <a name="modify-the-c-code"></a>C# kodunu değiştirme
Açık **LUIS_samples.cs** dosya ve aşağıdaki değişkenleri değiştirin:

|Değişken adı|Amaç|
|--|--|
|luisSubscriptionKey|Uç nokta URL'SİNİN abonelik anahtarı değerine Yayımla sayfasından karşılık gelir.|
|luisRegion|Uç nokta URL'SİNİN ilk alt etki alanı için karşılık gelen|
|luisAppId|Uç nokta URL'SİNİN yol aşağıdaki karşılık gelen **apps /**|

[![](./media/luis-tutorial-speech-to-intent/change-variables.png "Ekran görüntüsü, Visual Studio LUIS_samples.cs değişkenleri görüntüleme 2017")](./media/luis-tutorial-speech-to-intent/change-variables.png#lightbox)

Eşlenen İnsan Kaynakları hedefleri dosya zaten var.

[![](./media/luis-tutorial-speech-to-intent/intents.png "Ekran görüntüsü, Visual Studio LUIS_samples.cs hedefleri görüntüleme 2017")](./media/luis-tutorial-speech-to-intent/intents.png#lightbox)

Derleme ve uygulamayı çalıştırın. 

## <a name="test-code-with-utterance"></a>Kodu utterance ile test
Seçin **1** ve mikrofon konuşun "Kim Hasan Aydın Yöneticisi".

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

Doğru hedefi **GetEmployeeOrgChart**, % 61'i güvenle bulundu. Anahtar cümlesi varlık döndürüldü. 

Konuşma SDK'sı tüm HALUK yanıtı döndürür. 

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerektiğinde HALUK İnsanKaynakları uygulamayı silin. Bunu yapmak için uygulama listesinde uygulama adının yanındaki üç nokta menüsüne (...) tıklayıp **Delete** (Sil) öğesini seçin. Açılan **Delete app?** (Uygulama silinsin mi?) iletişim kutusunda **Ok** (Tamam) öğesini seçin.

İşiniz bittiğinde HALUK örnekleri dizinini silmeye unutmayın örnek kodu kullanarak.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [HALUK BOT ile tümleştirme](luis-csharp-tutorial-build-bot-framework-sample.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website