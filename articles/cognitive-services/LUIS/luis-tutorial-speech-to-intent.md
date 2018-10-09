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
ms.openlocfilehash: fb17e2d8c0ef1df5a6d4965730d3ddd3764d58f5
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48868760"
---
# <a name="integrate-speech-service"></a>Konuşma hizmeti tümleştirin
[Konuşma hizmeti](https://docs.microsoft.com/azure/cognitive-services/Speech-Service/) ses almak ve JSON nesneleri LUIS tahmin dönmek için tek bir istek kullanmanıza olanak tanır. Bu makalede, indirin ve bir mikrofona bir utterance konuşmak ve LUIS tahmin bilgi almak için Visual Studio'da C# projesi kullanın. Konuşma kullandığından [NuGet](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech/) paket, bir başvuru olarak zaten eklendi. 

Bu makale için ücretsiz bir gereksinim [LUIS] [ LUIS] uygulamayı içeri aktarmak için Web sitesi hesabı.

## <a name="create-luis-endpoint-key"></a>LUIS uç nokta anahtarı oluşturma
Azure portalında [oluşturma](luis-how-to-azure-subscription.md#create-luis-endpoint-key) bir **Language Understanding** (LUIS) anahtarı. 

## <a name="import-human-resources-luis-app"></a>İnsan Kaynakları LUIS alma uygulaması
İnsan Kaynakları LUIS uygulaması kullanılabilir amacı ve bu makalede konuşma arasındadır [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples) Github deposu. İndirme [HumanResources.json](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/HumanResources.json) ile kaydetmek bir dosya `.json` uzantısı ve [alma](luis-how-to-start-new-app.md#import-new-app) LUIS içine. 

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
Açık `Program.cs` dosyasını açıp aşağıdaki değişkenleri değiştirin:

|Değişken adı|Amaç|
|--|--|
|LUIS_assigned_endpoint_key|Uç nokta URL'SİNİN Yayımla sayfasından aboneliği-anahtar değer atanmış karşılık gelir|
|LUIS_endpoint_key_region|Uç nokta URL'SİNİN ilk alt etki alanı için örneğin karşılık gelir `westus`|
|LUIS_app_ID|Uç nokta URL'SİNİN yol aşağıdaki karşılık gelen **uygulamalar /**|

`Program.cs` Dosyası zaten eşlenmiş İnsan Kaynakları hedefleri sahiptir.

Derleme ve uygulamayı çalıştırın. 

## <a name="test-code-with-utterance"></a>Utterance koduyla test
Mikrofona "Redmond'da onaylanan diş hekimi kim?".

[!code-console[Command line response from spoken utterance](~/samples-luis/documentation-samples/tutorial-speech-intent-recognition/console-output.txt "Command line response from spoken utterance")]

Doğru amaç **GetEmployeeBenefits**, % 85'lik güvenle bulundu. Anahtar cümlesi varlık döndürdü. 

Speech SDK'sı LUIS yanıtın tamamını döndürür. 

## <a name="clean-up-resources"></a>Kaynakları temizleme
LUIS İnsanKaynakları uygulamayı artık gerekli değilse silin. Bunu yapmak için uygulamayı seçin ve ardından listesinin üst bağlamsal araç çubuğunda, **Sil**. Açılan **Delete app?** (Uygulama silinsin mi?) iletişim kutusunda **Ok** (Tamam) öğesini seçin.

İşiniz bittiğinde LUIS örnekler dizini silmeyi unutmayın örnek kodu kullanarak.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [LUIS bir BOT ile tümleştirme](luis-csharp-tutorial-build-bot-framework-sample.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website