---
title: 'Hızlı Başlangıç: Küçük resim - REST, cURL oluştur'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, cURL ile Görüntü İşleme API’sini kullanarak bir görüntüden küçük resim oluşturacaksınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 03/27/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: af303544be7b4e7686105b7d57a71c33c0a6a314
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60009070"
---
# <a name="quickstart-generate-a-thumbnail-using-the-rest-api-and-curl-in-computer-vision"></a>Hızlı Başlangıç: REST API kullanarak küçük resim oluşturma ve görüntü işleme cURL

Bu hızlı başlangıçta, görüntü işleme'nın REST API kullanarak görüntüyü küçük resim oluşturma. Giriş görüntüden en/boy oranını değişebilir genişliğini ve istenen yüksekliğini belirtin. Görüntü işleme, akıllı bir şekilde ilgi tanımlamak ve kırpma koordinatları bu bölge geçici bir çözüm oluşturmak için akıllı kırpma kullanır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

- [cURL](https://curl.haxx.se/windows)’niz olmalıdır.
- Görüntü İşleme için bir abonelik anahtarınız olması gerekir. Ücretsiz bir deneme anahtarından alabilirsiniz [Bilişsel Hizmetler'i deneyin](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision). Veya yönergeleri [Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) görüntü işleme için abone ve anahtarınızı alın.

## <a name="get-thumbnail-request"></a>Küçük Resim Alma isteği

[Küçük Resim Alma yöntemi](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) ile, bir görüntünün küçük resmini alabilirsiniz.

Örneği çalıştırmak için aşağıdaki adımları uygulayın:

1. Aşağıdaki kodu bir düzenleyicinin içine kopyalayın.
1. `<Subscription Key>` değerini geçerli abonelik anahtarınızla değiştirin.
1. `<File>` değerini küçük resmin kaydedileceği yol ve dosya adı ile değiştirin.
1. Gerekirse İstek URL’sini (`https://westcentralus.api.cognitive.microsoft.com/vision/v2.0`) abonelik anahtarlarınızı aldığınız konumu kullanacak şekilde değiştirin.
1. İsteğe bağlı olarak, analiz etmek için görüntüyü (`{\"url\":\"...`) değiştirin.
1. cURL yüklü bir bilgisayarda bir komut penceresi açın.
1. Kodu pencereye yapıştırıp komutu çalıştırın.

>[!NOTE]
>REST çağrınızda abonelik anahtarlarınızı almak için kullandığınız konumu kullanmanız gerekir. Örneğin, güvenlik anahtarlarınızı westus konumundan aldıysanız, aşağıdaki URL’de "westcentralus" değerini "westus" olarak değiştirin.

## <a name="create-and-run-the-sample-command"></a>Örnek komutu oluşturma ve çalıştırma

Örneği oluşturup çalıştırmak için aşağıdaki adımları uygulayın:

1. Aşağıdaki komutu bir metin düzenleyicisine kopyalayın.
1. Gerektiğinde komutta aşağıdaki değişiklikleri yapın:
    1. `<subscriptionKey>` değerini abonelik anahtarınızla değiştirin.
    1. `<thumbnailFile>` değerini, küçük resmin kaydedileceği dosyanın yolu ve adıyla değiştirin.
    1. Gerekirse istek URL’sini (`https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail`), abonelik anahtarlarınızı aldığınız Azure bölgesindeki [Küçük Resim Al](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) yönteminin uç nokta URL’si ile değiştirin.
    1. İsteğe bağlı olarak, istek gövdesindeki görüntü URL’sini (`https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\`) içinden küçük resim oluşturulacak farklı bir görüntünün URL’si ile değiştirin.
1. Bir komut istemi penceresi açın.
1. Metin düzenleyicisindeki komutu komut istemi penceresine yapıştırın ve komutu çalıştırın.

    ```console
    curl -H "Ocp-Apim-Subscription-Key: <subscriptionKey>" -o <thumbnailFile> -H "Content-Type: application/json" "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail?width=100&height=100&smartCropping=true" -d "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\"}"
    ```

## <a name="examine-the-response"></a>Yanıtı inceleme

Başarılı bir yanıt, küçük resim görüntüsünü `<thumbnailFile>` içinde belirtilen dosyaya yazar. İstek başarısız olursa yanıt, bir hata kodu ve nelerin yanlış gittiğini belirlemeye yardımcı olması için bir ileti içerir. İstek başarılı olması için gibi görünüyor, ancak oluşturulan küçük resim geçerli bir görüntü dosyası değil, abonelik anahtarınız geçersiz olabilir.

## <a name="next-steps"></a>Sonraki adımlar

Nasıl bir resmi çözümleme, ünlüleri ve önemli yerleri belirleme, küçük resim oluşturma ve basılı ve el yazısı metinleri ayıklamak için görüntü işleme API'sini keşfedin. Görüntü İşleme API'sini hızlı bir şekilde denemeniz için [Open API test konsolu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) konusuna bakın.

> [!div class="nextstepaction"]
> [Görüntü İşleme API’sini keşfetme](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
