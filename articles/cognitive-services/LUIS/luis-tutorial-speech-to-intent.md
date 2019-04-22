---
title: Konuşarak C# SDK'sı
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti tek bir istek kullanarak ses almanızı ve JSON nesneleriyle LUIS tahmini döndürmenizi sağlar. Bu makalede bir C# projesi indirip Visual Studio'da kullanarak mikrofona konuşacak ve LUIS tahmin bilgilerini alacaksınız.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 04/08/2018
ms.author: diberry
ms.openlocfilehash: 9d6173ee25f28aa884513d126c06a8a7c722098d
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59273847"
---
# <a name="integrate-speech-service-with-your-language-understanding-app"></a>Konuşma hizmeti Language Understanding uygulamanızla tümleştirin
[Konuşma hizmeti](https://docs.microsoft.com/azure/cognitive-services/Speech-Service/) tek bir istek kullanarak ses almanızı ve JSON nesneleriyle LUIS tahmini döndürmenizi sağlar. Bu makalede bir C# projesi indirip Visual Studio'da kullanarak mikrofona konuşacak ve LUIS tahmin bilgilerini alacaksınız. Bu projede Konuşma [NuGet](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech/) paketi kullanılmaktadır ve bu paket başvuru olarak projeye eklenmiştir. 

Bu makalede uygulamayı içeri aktarmak için ücretsiz bir [LUIS][LUIS] web sitesi hesabına ihtiyacınız olacak.

## <a name="create-luis-endpoint-key"></a>LUIS uç nokta anahtarı oluşturma
Azure portalında [oluşturma](luis-how-to-azure-subscription.md) bir **Bilişsel hizmet** LUIS uygulamanızı (LUIS) anahtarı.  

## <a name="import-human-resources-luis-app"></a>İnsan Kaynakları LUIS uygulamasını içeri aktarma
İnsan Kaynakları LUIS uygulaması kullanılabilir amacı ve bu makalede konuşma arasındadır [Azure-Samples](https://github.com/Azure-Samples/cognitive-services-language-understanding) GitHub deposu. [HumanResources.json](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/HumanResources.json) dosyasını indirin, `.json` uzantısıyla kaydedin ve LUIS'e [aktarın](luis-how-to-start-new-app.md#import-new-app). 

Bu uygulamada İnsan Kaynakları alanıyla ilgili amaçlar, varlıklar ve konuşmalar bulunur. Örnek konuşmalar şunlardır:

|Örnek konuşmalar|
|--|
|Who is John Smith's manager? (John Smith'in yöneticisi kim?)|
|Who does John Smith manage? (John Smith kimleri yönetiyor?)|
|Where is Form 123456? (Form 123456 nerede?)|
|Do I have any paid time off? (Ücretli iznim var mı?)|


## <a name="add-keyphrase-prebuilt-entity"></a>Önceden oluşturulan KeyPhrase varlığını ekleme
Uygulamayı içeri aktardıktan sonra **Entities** (Varlıklar) ve ardından **Add prebuilt entities** (Önceden oluşturulan varlık ekle) öğesini seçin. **KeyPhrase** varlığını ekleyin. KeyPhrase varlığı konuşmadaki önemli konuları ayıklar.

## <a name="train-and-publish-the-app"></a>Uygulamayı eğitme ve yayımlama
1. Yukarıda, sağ taraftaki gezinti çubuğunda bulunan **Train** (Eğit) düğmesini seçerek LUIS uygulamasını eğitin.

2. Sağ üst çubuktan **Manage** (Yönet) ve ardından sol taraftan **Keys and endpoints** (Anahtarlar ve uç noktalar) öğesini seçin. 

3. **Keys and endpoints** (Anahtarlar ve uç noktalar) sayfasında [LUIS uç nokta anahtarı oluşturma](#create-luis-endpoint-key) bölümünde oluşturulan LUIS anahtarını atayın.

   Bu sayfada [LUIS uç nokta anahtarı oluşturma](#create-luis-endpoint-key) bölümünde oluşturulan LUIS anahtarının uygulama kimliği, yayımlama bölgesi ve abonelik kimliğini alın. Makalenin ilerleyen bölümlerinde kodu değiştirerek bu değerleri kullanacaksınız. 
  
   Bu alıştırma için ücretsiz başlangıç anahtarını **kullanmayın**. Bu alıştırmada yalnızca Azure portalda oluşturulan bir **Language Understanding** anahtarı kullanılabilir. 

   https://**BÖLGE**.api.cognitive.microsoft.com/luis/v2.0/apps/**UYGULAMAKİMLİĞİ**?subscription-key=**LUISANAHTARI**&q=


4. Sağ üst bölümde bulunan **Publish** (Yayımla) düğmesini seçerek LUIS uygulamasını yayımlayın. 

## <a name="audio-device"></a>Ses cihazı
Bu makalede bilgisayarınızdaki ses cihazı kullanılmaktadır. Bu mikrofonlu kulaklık veya yerleşik ses cihazı olabilir. Ses giriş düzeylerini inceleyin ve konuşmanızın ses cihazı tarafından algılanması için gerekirse normalden yüksek sesle konuşun. 

## <a name="download-the-luis-sample-project"></a>LUIS Örnek projesini indirme
 Kopyala veya indir [Azure-Samples](https://github.com/Azure-Samples/cognitive-services-language-understanding) depo. [Speech to intent project](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/tutorial-speech-intent-recognition) projesini Visual Studio ile açın ve NuGet paketlerini geri yükleyin. VS çözüm documentation-samples\tutorial-speech-intent-recognition\csharp\csharp_samples.sln dosyasıdır.

Konuşma SDK'sı başvuru olarak eklenmiştir. 

[![Visual Studio 2017 ekran görüntüleme Microsoft.CognitiveServices.Speech NuGet paketini](./media/luis-tutorial-speech-to-intent/nuget-package.png "görüntüleme Microsoft.CognitiveServices.Speech NuGet paketini Visual Studio 2017 ekran görüntüsü")](./media/luis-tutorial-speech-to-intent/nuget-package.png#lightbox)

## <a name="modify-the-c-code"></a>C# kodunu değiştirme
`Program.cs` dosyasını açın ve aşağıdaki değişkenleri değiştirin:

|Değişken adı|Amaç|
|--|--|
|LUIS_assigned_endpoint_key|Publish (Yayımla) sayfasındaki uç nokta URL'sine atanan abonelik anahtarı değerini gösterir|
|LUIS_endpoint_key_region|URL'nin ilk alt etki alanını gösterir, örneğin: `westus`|
|LUIS_app_ID|Uç nokta URL'si yolunun **apps/** sonrasını gösterir|

`Program.cs` dosyasına İnsan Kaynakları amaçları eşlenmiştir.

Uygulamayı derleyin ve çalıştırın. 

## <a name="test-code-with-utterance"></a>Kodu konuşmayla test etme
Mikrofona "Who are the approved dentists in Redmond?" (Redmond'daki onaylı diş hekimleri hangileri?) deyin.

[!code-console[Command line response from spoken utterance](~/samples-luis/documentation-samples/tutorial-speech-intent-recognition/console-output.txt "Command line response from spoken utterance")]

Doğru amaç olan **GetEmployeeBenefits**, %85 güvenilirlik puanıyla bulundu. keyPhrase varlığı döndürüldü. 

Konuşma SDK'sı, LUIS yanıtının tamamını döndürür. 

## <a name="clean-up-resources"></a>Kaynakları temizleme
İhtiyacınız kalmadıysa LUIS HumanResources uygulamasını silebilirsiniz. Bunu yapmak için uygulamayı seçin ve listenin üzerindeki bağlam araç çubuğunda **Delete** (Sil) öğesini seçin. Açılan **Delete app?** (Uygulama silinsin mi?) iletişim kutusunda **Ok** (Tamam) öğesini seçin.

İşiniz bittiğinde, dizin silmeyi unutmayın örnek kodu kullanarak.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [LUIS’i bir BOT ile tümleştirme](luis-csharp-tutorial-build-bot-framework-sample.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
