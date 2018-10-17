---
title: 'Hızlı başlangıç: Bilgi bankası güncelleştirme - REST, Node.js - Soru-Cevap Oluşturma'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta var olan bir Soru-Cevap Oluşturma bilgi bankasını (KB) program aracılığıyla güncelleştirme adımları gösterilmektedir.  Bu JSON kodu yeni veri kaynağı ekleyerek, veri kaynaklarını değiştirerek veya veri kaynaklarını silerek bir KB'yi güncelleştirmenizi sağlar.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 10/02/2018
ms.author: diberry
ms.openlocfilehash: 3bbc55b3bb064b2cf4b140a395e99209b71a5ce1
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816248"
---
# <a name="quickstart-update-a-qna-maker-knowledge-base-in-nodejs"></a>Hızlı başlangıç: Node.js ile Soru-Cevap Oluşturma bilgi bankası güncelleştirme

Bu hızlı başlangıçta var olan bir Soru-Cevap Oluşturma bilgi bankasını (KB) program aracılığıyla güncelleştirme adımları gösterilmektedir.  Bu JSON kodu yeni veri kaynağı ekleyerek, veri kaynaklarını değiştirerek veya veri kaynaklarını silerek bir KB'yi güncelleştirmenizi sağlar.

Bu API Soru-Cevap Oluşturma portalında düzenleme yapıp **Kaydet ve eğit** düğmesini kullanmakla eşdeğerdir.

Bu hızlı başlangıç şu Soru-Cevap Oluşturma API'lerini çağırır:
* [Update](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600): JSON ile tanımlanan bilgi bankası modeli API isteğinin gövdesinde gönderilir. 
* [İşlem Ayrıntılarını Alma](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/operations_getoperationdetails)

## <a name="prerequisites"></a>Ön koşullar

* [Node.js 6+](https://nodejs.org/en/download/)
* [Soru-Cevap Oluşturma hizmetine](../How-To/set-up-qnamaker-service-azure.md) sahip olmanız gerekir. Anahtarınızı almak için, panonuzda **Kaynak Yönetimi** altında **Anahtarlar** öğesini seçin. 
* Soru-Cevap Oluşturma bilgi bankası (KB) kimliği aşağıda gösterildiği gibi URL'nin kbid sorgu dizesi bölümünde bulunur.

    ![Soru-Cevap Oluşturma bilgi bankası kimliği](../media/qnamaker-quickstart-kb/qna-maker-id.png)

Henüz bir bilgi bankanız yoksa, bu hızlı başlangıçta kullanmak için bir örneğini oluşturabilirsiniz: [Yeni bilgi bankası oluşturma](create-new-kb-nodejs.md).

[!INCLUDE [Code is available in Azure-Samples Github repo](../../../../includes/cognitive-services-qnamaker-nodejs-repo-note.md)]

## <a name="create-a-knowledge-base-nodejs-file"></a>Bilgi bankası Node.js dosyası oluşturma

`update-knowledge-base.js` adlı bir dosya oluşturun.

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

Aşağıdaki satırları `update-knowledge-base.js` adlı dosyanın en üstüne ekleyerek projeye gerekli bağımlılıkları dahil edin:

[!code-nodejs[Add the dependencies](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=1-4 "Add the dependencies")]

## <a name="add-required-constants"></a>Gerekli sabitleri ekleme
Yukarıdaki gerekli bağımlılıklardan sonra Soru-Cevap Oluşturma hizmetine erişmek için gerekli sabitleri ekleyin. `subscriptionKey` değişkeninin değerini kendi Soru-Cevap Oluşturma anahtarınızla değiştirin. 

[!code-nodejs[Add required constants](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=10-17 "Add required constants")]

## <a name="add-knowledge-base-id"></a>Bilgi bankası kimliği ekleme

Yukarıdaki sabitlerden sonra bilgi bankası kimliğini ekleyin ve yola dahil edin:

[!code-nodejs[Add knowledge base ID](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=19-23 "Add knowledge base ID")]

## <a name="add-the-kb-update-model-definition"></a>KB güncelleştirme modeli tanımını ekleme

Sabitlerden sonra aşağıdaki KB güncelleştirme tanımını ekleyin. Güncelleştirme tanımı üç bölümden oluşur:

* add
* update
* delete

Tüm bölümler API'ye gönderilen tek bir istekte kullanılabilir. 

[!code-nodejs[Add the KB update definition](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=25-53 "Add the KB update definition")]


## <a name="add-supporting-functions"></a>Destekleyici işlevleri ekleme

Bir sonraki adımda aşağıdaki destekleyici işlevleri ekleyin.

1. JSON sonucunu okunabilir biçimde yazdırmak için aşağıdaki işlevi ekleyin:

   [!code-nodejs[Add supporting functions, step 1](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=55-58 "Add supporting functions, step 1")]

2. Oluşturma işleminin durumunu alma amacıyla HTTP yanıtını yönetmek için aşağıdaki işlevleri ekleyin:

   [!code-nodejs[Add supporting functions, step 2](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=60-82 "Add supporting functions, step 2")]

## <a name="add-patch-request-to-update-kb"></a>KB güncelleştirmesi için PATCH isteğini ekleme

Bilgi bankasını güncelleştirme amacıyla bir HTTP PATCH isteğinde bulunmak için aşağıdaki işlevleri ekleyin. `Ocp-Apim-Subscription-Key`, Soru-Cevap Oluşturma hizmeti anahtarıdır ve kimlik doğrulaması için kullanılır.

[!code-nodejs[Add PATCH request to update KB](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=84-111 "Add PATCH request to update KB")]

Bu API çağrısı, işlem kimliğini içeren bir JSON yanıtı döndürür. İşlem kimliği, işlemin tamamlanmaması halinde durum isteğinde bulunmak için gereklidir.

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-10-02T01:23:00Z",
  "lastActionTimestamp": "2018-10-02T01:23:00Z",
  "userId": "335c3841df0b42cdb00f53a49d51a89c",
  "operationId": "e7be3897-88ff-44e5-a06c-01df0e05b78c"
}
```

## <a name="add-get-request-to-determine-operation-status"></a>İşlem durumunu belirlemek için GET isteği ekleme

İşlemin durumunu denetleyin.
    
[!code-nodejs[Add GET request to determine operation status](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=113-137 "Add GET request to determine operation status")]

Bu API çağrısı, işlem durumunu içeren bir JSON yanıtı döndürür: 

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:22:53Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

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

## <a name="add-updatekb-method"></a>update_kb metodunu ekleme

Aşağıdaki metot, KB'yi güncelleştirir ve durum denetimini tekrarlar. KB oluşturma işlemi zaman alabileceğinden başarılı veya başarısız bir sonuç alana kadar durum denetimi çağrılarını tekrarlamanız gerekir.

[!code-nodejs[Add update_kb method](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=139-169 "Add update_kb method")]

## <a name="run-the-program"></a>Programı çalıştırma

Programı çalıştırmak için aşağıdaki komutu bir komut satırına yazın. Soru-Cevap Oluşturma API'sine KB güncelleştirme isteği gönderir ve 30 saniyede bir sonucu yoklar. Her yanıt konsol penceresine yazdırılır.

```bash
node update-knowledge-base.js
```

Bilgi bankanız güncelleştirildikten sonra Soru-Cevap Oluşturma Portalı’nızdaki [Bilgi bankalarım](https://www.qnamaker.ai/Home/MyServices) sayfasından görüntüleyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)