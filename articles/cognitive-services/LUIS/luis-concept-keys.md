---
title: LUIS anahtarlarınızı - Azure'ı Anlama | Microsoft Docs
description: Uygulamanızı yazmak ve, endpoing sorgulamak için Language Understanding (LUIS) tuşlarını kullanın.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/23/2018
ms.author: v-geberr
ms.openlocfilehash: 083169b300cc2714da3921c3abeee68d52444b9b
ms.sourcegitcommit: 727a0d5b3301fe20f20b7de698e5225633191b06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39145715"
---
# <a name="keys-in-luis"></a>LUIS anahtarları
LUIS, iki anahtar kullanır: [yazma](#programmatic-key) ve [uç nokta](#endpoint-key). LUIS hesabınızı oluşturduğunuzda yazma anahtar sizin için otomatik olarak oluşturulur. LUIS uygulamanızı yayımlamaya hazır olduğunuzda yapmanız [uç nokta oluşturma](luis-how-to-azure-subscription.md#create-luis-endpoint-key), [atayabilirsiniz](luis-how-to-manage-keys.md#assign-endpoint-key) LUIS uygulamanıza ve [ile uç nokta sorgu kullanın](#use-endpoint-key-in-query). 

|Anahtar|Amaç|
|--|--|
|[Anahtar yazma](#programmatic-key)|Yazma, yayımlama ortak çalışanlar, yönetme, sürüm oluşturma|
|[Uç noktası anahtarı](#endpoint-key)| Sorgulama|

LUIS uygulamaları yazmak önemlidir [bölgeleri](luis-reference-regions.md#publishing-regions) ayrıca istediğiniz yere yayımlayın ve sorgu.

<a name="programmatic-key" ></a>
## <a name="authoring-key"></a>Anahtar yazma

Bir başlangıç anahtarı olarak da bilinen bir yazma anahtar otomatik olarak bir LUIS hesabı oluşturun ve ücretsiz olarak oluşturulur. Her yazma tüm uygulamalarınızda LUIS bir yazma anahtar sahip [bölge](luis-reference-regions.md). LUIS uygulamanızı yazar veya test uç noktası sorguları yazma anahtar sağlanır. 

Yazma anahtarını bulmak için oturum [LUIS](luis-reference-regions.md#luis-website) ve hesap adı açmak için sağ Gezinti çubuğuna tıklayarak **hesap ayarları**.

![Anahtar yazma](./media/luis-concept-keys/programatic-key.png)

Yapmak istediğinizde **üretim uç noktası sorguları**, Azure oluşturma [LUIS abonelik](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/). 

> [!CAUTION]
> Çünkü birkaç uç nokta çağrılarında sağlar kolaylık sağlamak için birçok örnekleri yazma anahtar kullanım kendi [kota](luis-boundaries.md#key-limits).  

## <a name="endpoint-key"></a>Uç noktası anahtarı
 Gerektiğinde **üretim uç noktası sorguları**, oluşturun bir [LUIS anahtar](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/) Azure portalında. Anahtar oluşturmak için kullanılan ad unutmayın, uygulamaya anahtar eklerken ihtiyacınız..

LUIS abonelik işlemi tamamlandığında [anahtarı Ekle](luis-how-to-manage-keys.md#assign-endpoint-key) uygulamasında **Yayımla** sayfası. 

Uç nokta, uç noktası isabet anahtarı oluştururken belirttiğiniz kullanım planına dayanarak bir kota sağlar. Bkz: [Bilişsel hizmetler fiyatlandırması](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/?v=17.23h) fiyatlandırma bilgileri için.

Uç nokta, tüm LUIS uygulamalar için veya belirli LUIS uygulamalar için kullanılabilir. 

LUIS uygulamaları yazmaya yönelik uç nokta kullanmayın. 

## <a name="use-endpoint-key-in-query"></a>Sorguda uç noktası anahtarı kullanma
İki stili sorgu LUIS uç nokta kabul eder, hem uç noktası anahtarı, ancak farklı yerlerde kullanın:

|Fiili|Örnek URL'sini ve anahtarını konum|
|--|--|
|[GET](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78)|`https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=your-endpoint-key-here&verbose=true&timezoneOffset=0&q=turn%20on%20the%20lights`<br><br>Sorgu dizesi değeri için `subscription-key`<br><br>Uç nokta sorgu değerini `subscription-key` yazma (Başlangıç) anahtarından LUIS uç noktası anahtarı kota oranı kullanmak için yeni uç noktası anahtarı. Anahtarı oluşturduğunuz ve tuşu atama ancak abonelik anahtarı için uç nokta sorgu değerini değiştirmeyin, ', uç nokta anahtar kotanızı kullanmıyorsunuz demektir.|
|[POST](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee79)| `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2`<br><br> üstbilgi değeri `Ocp-Apim-Subscription-Key`<br><br>Uç nokta sorgu değerini `Ocp-Apim-Subscription-Key` yazma (Başlangıç) anahtarından LUIS uç noktası anahtarı kota oranı kullanmak için yeni uç noktası anahtarı. Anahtarı oluşturun ve tuşu atama ancak uç noktası sorgu değerini değiştirmeyin `Ocp-Apim-Subscription-Key`, uç nokta anahtar kotanızı kullanmıyorsunuz demektir.|

Önceki URL'lerinde kullanılan uygulama kimliği `df67dcdb-c37d-46af-88e1-8b97951ca1c2`, genel IOT uygulaması için kullanılan [etkileşimli tanıtım](https://azure.microsoft.com/en-us/services/cognitive-services/language-understanding-intelligent-service/). 

## <a name="api-usage-of-ocp-apim-subscription-key"></a>Ocp-Apim-Subscription-Key API kullanımı
LUIS API'leri kullanan başlık `Ocp-Apim-Subscription-Key`. Üst bilgi adı, hangi anahtarı ve API kümesi kullanarak göre değişmez. Üst bilgisi için yazma API'leri geliştirme anahtarı ayarlayın. Uç noktası kullanıyorsanız, üst bilgisini uç noktası anahtarı ayarlayın. 

Yazma API'leri için uç noktası anahtarı geçiremezsiniz. Bunu yaparsanız, 401 hatası - erişim reddedildi nedeniyle geçersiz uç noktası anahtarı alın. 

## <a name="key-limits"></a>Anahtar sınırları
Bkz: [anahtar sınırları](luis-boundaries.md#key-limits) ve [Azure bölgeleri](luis-reference-regions.md). Yazma ücretsiz ve yazmak için kullanılan anahtardır. LUIS uç noktası anahtarı ücretsiz katmanda vardır ancak sizin tarafınızdan oluşturulmalı ve LUIS uygulamanızla ilişkili **Yayımla** sayfası. Yazma ancak yalnızca uç nokta sorgular için kullanılamaz.

Yayımlama bölgeler bölge geliştirme farklıdır. Yazma bölgesi istediğiniz yayımlama bölgesiyle ilgili uygulama oluşturduğunuzdan emin olun.

## <a name="key-limit-errors"></a>Anahtar sınırı hataları
Aşarsanız, ikinci kota bir HTTP 429 hatasını alıyorsunuz. Aşarsanız, aylık kota bir HTTP 403 hatası alırsınız. Bir LUIS alarak bu hataları düzeltin [uç nokta](#endpoint-key) anahtar [atama](luis-how-to-manage-keys.md#assign-endpoint-key) uygulamasında anahtar **Yayımla** sayfasının [LUIS](luis-reference-regions.md#luis-website) Web sitesi.

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi [kavramları](luis-how-to-manage-keys.md#assign-endpoint-key) yazma ve uç nokta anahtarları hakkında.