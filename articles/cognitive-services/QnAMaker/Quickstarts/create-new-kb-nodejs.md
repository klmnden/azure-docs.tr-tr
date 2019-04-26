---
title: 'Hızlı Başlangıç: Bilgi Bankası - REST, Node.js - soru-cevap Oluşturucu Oluşturma'
description: Bu REST tabanlı hızlı başlangıçta Bilişsel Hizmetler API hesabınızdaki Azure Panonuzda görünecek olan örnek bir Soru-Cevap Oluşturma bilgi bankasını programlamayla oluşturma adımları gösterilmektedir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 02/04/2019
ms.author: diberry
ms.openlocfilehash: 2d199960b18bcab5179b7b433586492487eedc58
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60309326"
---
# <a name="quickstart-create-a-knowledge-base-in-qna-maker-using-nodejs"></a>Hızlı Başlangıç: Node.js kullanarak soru-cevap Oluşturucu Bilgi Bankası oluşturma

Bu hızlı başlangıçta program aracılığıyla örnek bir Soru-Cevap Oluşturma bilgi bankası (KB) oluşturma ve yayımlama adımlarında yol gösterilir. Soru-Cevap Oluşturma, [veri kaynaklarından](../Concepts/data-sources-supported.md) ve SSS gibi yarı yapılandırılmış içerikten soru ve cevapları otomatik olarak ayıklar. JSON ile tanımlanan bilgi bankası modeli API isteğinin gövdesinde gönderilir. 

Bu hızlı başlangıç şu Soru-Cevap Oluşturma API'lerini çağırır:
* [KB Oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
* [İşlem Ayrıntılarını Alma](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/operations_getoperationdetails)

## <a name="prerequisites"></a>Önkoşullar

* [Node.js 6+](https://nodejs.org/en/download/)
* [Soru-Cevap Oluşturma hizmetine](../How-To/set-up-qnamaker-service-azure.md) sahip olmanız gerekir. Anahtarınızı almak için, panonuzda **Kaynak Yönetimi** altında **Anahtarlar** öğesini seçin. 

[!INCLUDE [Code is available in Azure-Samples GitHub repo](../../../../includes/cognitive-services-qnamaker-nodejs-repo-note.md)]

## <a name="create-a-knowledge-base-nodejs-file"></a>Bilgi bankası Node.js dosyası oluşturma

`create-new-knowledge-base.js` adlı bir dosya oluşturun.

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

Aşağıdaki satırları `create-new-knowledge-base.js` adlı dosyanın en üstüne ekleyerek projeye gerekli bağımlılıkları dahil edin:

[!code-nodejs[Add the dependencies](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.js?range=1-4 "Add the dependencies")]

## <a name="add-the-required-constants"></a>Gerekli sabitleri ekleme
Yukarıdaki gerekli bağımlılıklardan sonra Soru-Cevap Oluşturma hizmetine erişmek için gerekli sabitleri ekleyin. `subscriptionKey` değişkeninin değerini kendi Soru-Cevap Oluşturma anahtarınızla değiştirin.

[!code-nodejs[Add the required constants](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.js?range=10-19 "Add the required constants")]

## <a name="add-the-kb-model-definition"></a>KB modeli tanımını ekleme

Sabitlerden sonra aşağıdaki KB modeli tanımını ekleyin. Model tanımdan sonra bir dizeye dönüştürülür.

[!code-nodejs[Add the KB model definition](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.js?range=21-51 "Add the KB model definition")]

## <a name="add-supporting-functions"></a>Destekleyici işlevleri ekleme

Bir sonraki adımda aşağıdaki destekleyici işlevleri ekleyin.

1. JSON sonucunu okunabilir biçimde yazdırmak için aşağıdaki işlevi ekleyin:

   [!code-nodejs[Add supporting functions, step 1](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.js?range=53-56 "Add supporting functions, step 1")]

2. HTTP yanıtını yönetmek için aşağıdaki işlevi ekleyin:

   [!code-nodejs[Add supporting functions, step 2](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.js?range=58-80 "Add supporting functions, step 2")]

## <a name="add-functions-to-create-kb"></a>KB oluşturma işlevlerini ekleme

Bilgi bankasını oluşturma amacıyla bir HTTP POST isteğinde bulunmak için aşağıdaki işlevleri ekleyin. `Ocp-Apim-Subscription-Key`, Soru-Cevap Oluşturma hizmeti anahtarıdır ve kimlik doğrulaması için kullanılır. 

[!code-nodejs[POST Request to API](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.js?range=82-109 "POST Request to API")]

Bu API çağrısı, işlem kimliğini içeren bir JSON yanıtı döndürür. İşlem kimliğini KB'nin başarıyla oluşturulup oluşturulmadığını belirlemek için kullanın. 

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:19:01Z",
  "lastActionTimestamp": "2018-09-26T05:19:01Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "8dfb6a82-ae58-4bcb-95b7-d1239ae25681"
}
```

## <a name="add-functions-to-determine-creation-status"></a>Oluşturma durumunu belirleme işlevlerini ekleme

İşlem durumunu denetleme amacıyla bir HTTP GET isteğinde bulunmak için aşağıdaki işlevi ekleyin. `Ocp-Apim-Subscription-Key`, Soru-Cevap Oluşturma hizmeti anahtarıdır ve kimlik doğrulaması için kullanılır. 

[!code-nodejs[Determine creation status](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.js?range=112-135 "Determine creation status")]

Başarılı veya başarısız bir sonuç alana kadar çağrıyı tekrarlayın: 

```JSON
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:23:08Z",
  "resourceLocation": "/knowledgebases/XXX7892b-10cf-47e2-a3ae-e40683adb714",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```


## <a name="add-create-kb-function"></a>create-kb işlevi ekleme

Aşağıdaki işlev ana işlevdir ve KB'yi oluşturup durum denetimini tekrarlar. _create_ **Operation ID**, POST yanıtı üst bilgisinin **Location** alanında döndürülür ve GET isteğindeki yolun bir parçası olarak kullanılır. KB oluşturma işlemi zaman alabileceğinden başarılı veya başarısız bir sonuç alana kadar durum denetimi çağrılarını tekrarlamanız gerekir.

[!code-nodejs[Add create-kb function](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.js?range=137-167 "Add create-kb function")]

## <a name="run-the-program"></a>Programı çalıştırma

Programı çalıştırmak için aşağıdaki komutu bir komut satırına yazın. Soru-Cevap Oluşturma API'sine KB oluşturma isteği gönderir ve 30 saniyede bir sonucu yoklar. Her yanıt konsol penceresine yazdırılır.

```bash
node create-new-knowledge-base.js
```

Bilgi bankanız oluşturulduktan sonra Soru-Cevap Oluşturma Portalı’nızdaki [Bilgi bankalarım](https://www.qnamaker.ai/Home/MyServices) sayfasından görüntüleyebilirsiniz. 

[!INCLUDE [Clean up files and KB](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
