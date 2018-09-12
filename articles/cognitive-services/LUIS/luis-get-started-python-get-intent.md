---
title: 'Hızlı Başlangıç: Python kullanarak bir Language Understanding (LUIS) uygulamasını çağırma | Microsoft Docs'
description: Bu hızlı başlangıçta Python kullanarak bir LUIS uygulamasını çağırmayı öğreneceksiniz.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 06/27/2018
ms.author: diberry
ms.openlocfilehash: bc7ae912d762a98c34b9a1b2d6a82d5630c4794b
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "43772554"
---
# <a name="quickstart-call-a-luis-endpoint-using-python"></a>Öğretici: Python kullanarak bir LUIS uç noktasını çağırma
Bu hızlı başlangıçta, amaç ve varlıkları döndürmek için bir LUIS uç noktasına konuşma iletin.

<!-- green checkmark -->
<!--
> [!div class="checklist"]
> * Create LUIS subscription and copy key value for later use
> * View LUIS endpoint results from browser to public sample IoT app
> * Create Visual Studio C# console app to make HTTPS call to LUIS endpoint
-->

Bu makale için kendi LUIS uygulamanızı yazma amacıyla ücretsiz bir [LUIS](luis-reference-regions.md#luis-website) hesabına ihtiyacınız olacak.

<a name="create-luis-subscription-key"></a>
## <a name="create-luis-endpoint-key"></a>LUIS uç nokta anahtarı oluşturma
Bu kılavuzda kullanılan örnek LUIS uygulamasına çağrı yapmak için Bilişsel Hizmetler API'si anahtarına ihtiyacınız vardır. 

API anahtarı almak için aşağıdaki adımları izleyin: 

1. Öncelikle Azure portalında bir [Bilişsel Hizmetler API'si hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oluşturmanız gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

2. https://portal.azure.com adresinden Azure portalında oturum açın. 

3. Anahtar almak için [Azure'da Uç Nokta Anahtarı Oluşturma](./luis-how-to-azure-subscription.md) sayfasındaki adımları inceleyin.

4. [LUIS](luis-reference-regions.md) web sitesine dönün ve Azure hesabınızı kullanarak oturum açın. 

    [![](media/luis-get-started-node-get-intent/app-list.png "Uygulama listesinin ekran görüntüsü")](media/luis-get-started-node-get-intent/app-list.png)

## <a name="understand-what-luis-returns"></a>LUIS uygulaması tarafından döndürülen verileri anlama

Bir LUIS uygulaması tarafından döndürülen verileri anlamak için örnek LUIS uygulamasının URL'sini bir tarayıcı penceresine yapıştırabilirsiniz. Örnek uygulama, kullanıcının lambaları açma veya kapatma isteğini belirleyen bir IoT uygulamasıdır.

1. Örnek uygulamanın uç noktası şu biçimdedir: `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=<YOUR_API_KEY>&verbose=false&q=turn%20on%20the%20bedroom%20light` URL'yi kopyalayın ve `subscription-key` alanının değerini kendi uç nokta anahtarınızla değiştirin.
2. URL'yi bir tarayıcı penceresine yapıştırıp Enter tuşuna basın. Tarayıcıda LUIS uygulamasının `HomeAutomation.TurnOn` amacını ve `HomeAutomation.Room` varlığını `bedroom` değeriyle algıladığını belirten bir JSON sonucu görüntülenir.

    ![JSON sonucu, TurnOn amacını algılar](./media/luis-get-started-node-get-intent/turn-on-bedroom.png)
3. URL'deki `q=` parametresinin değerini `turn off the living room light` olarak değiştirip Enter tuşuna basın. Sonuç şimdi LUIS uygulamasının `HomeAutomation.TurnOff` amacını ve `HomeAutomation.Room` varlığını `living room` değeriyle algıladığını belirtir. 

    ![JSON sonucu, TurnOff amacını algılar](./media/luis-get-started-node-get-intent/turn-off-living-room.png)


## <a name="consume-a-luis-result-using-the-endpoint-api-with-python"></a>Python ile Uç Nokta API'sini kullanarak bir LUIS sonucundan faydalanma

Python kullanarak bir önceki adımda tarayıcı penceresinde gördüğünüz sonuçlara ulaşabilirsiniz.

1. Aşağıdaki kod parçacıklarından birini `quickstart-call-endpoint.py` adlı bir dosyaya kopyalayın:

   [!code-python[Console app code that calls a LUIS endpoint for Python 2.7](~/samples-luis/documentation-samples/quickstarts/analyze-text/python/2.x/quickstart-call-endpoint-2-7.py)]

   [!code-python[Console app code that calls a LUIS endpoint for Python 3.6](~/samples-luis/documentation-samples/quickstarts/analyze-text/python/3.x/quickstart-call-endpoint-3-6.py)]

2. `Ocp-Apim-Subscription-Key` alanının değerini LUIS uç nokta anahtarınızla değiştirin.

3. `pip install requests` ile bağımlılıkları yükleyin.

4. `python ./quickstart-call-endpoint.py` ile betiği çalıştırın. Tarayıcı penceresinde daha önce gördüğünüz JSON kodunu görüntüler.
<!-- 
![Console window displays JSON result from LUIS](./media/luis-get-started-python-get-intent/console-turn-on.png)
-->

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu öğreticide oluşturulan iki kaynak, LUIS uç nokta anahtarı ve C# projesidir. LUIS uç nokta anahtarını Azure portalından kaldırın. Visual Studio projesini kapatın ve dizini dosya sisteminden kaldırın. 

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Konuşma ekleme](luis-get-started-python-add-utterance.md)