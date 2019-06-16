---
title: Abonelik anahtarları
titleSuffix: Language Understanding - Azure Cognitive Services
description: LUIS, tahmin uç noktası kullanıcı Konuşma ile sorgulama için iki anahtar, modelinizi oluşturmak için ücretsiz geliştirme anahtarı ve tarifeli uç noktası anahtarı kullanır.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/18/2019
ms.author: diberry
ms.openlocfilehash: feb4622be14b51cfa72c33cda6c2477f799758c6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66473579"
---
# <a name="authoring-and-query-prediction-endpoint-keys-in-luis"></a>LUIS yazma ve sorgu tahmin uç nokta anahtarları
LUIS, iki anahtar kullanır: [yazma](#programmatic-key) ve [uç nokta](#endpoint-key). LUIS hesabınızı oluşturduğunuzda yazma anahtar sizin için otomatik olarak oluşturulur. LUIS uygulamanızı yayımlamaya hazır olduğunuzda yapmanız [uç nokta oluşturma](luis-how-to-azure-subscription.md), [atayabilirsiniz](luis-how-to-azure-subscription.md) LUIS uygulamanıza ve [ile uç nokta sorgu kullanın](#use-endpoint-key-in-query). 

|Anahtar|Amaç|
|--|--|
|[Anahtar yazma](#programmatic-key)|Yazma, yayımlama ortak çalışanlar, yönetme, sürüm oluşturma|
|[Uç noktası anahtarı](#endpoint-key)| Sorgulama|

LUIS uygulamaları yazmak önemlidir [bölgeleri](luis-reference-regions.md#publishing-regions) ayrıca istediğiniz yere yayımlayın ve sorgu.

<a name="programmatic-key" ></a>
## <a name="authoring-key"></a>Anahtar yazma

Bir başlangıç anahtarı olarak da bilinen bir yazma anahtar otomatik olarak bir LUIS hesabı oluşturun ve ücretsiz olarak oluşturulur. Her yazma tüm uygulamalarınızda LUIS bir yazma anahtar sahip [bölge](luis-reference-regions.md). LUIS uygulamanızı yazar veya test uç noktası sorguları yazma anahtar sağlanır. 

Yazma anahtarını bulmak için oturum açın [LUIS](luis-reference-regions.md#luis-website) ve hesap adı açmak için sağ Gezinti çubuğuna tıklayarak **hesap ayarları**.

![Anahtar yazma](./media/luis-concept-keys/programatic-key.png)

Yapmak istediğinizde **üretim uç noktası sorguları**, Azure'ı oluşturma [LUIS abonelik](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/). 

> [!CAUTION]
> Çünkü birkaç uç nokta çağrılarında sağlar kolaylık sağlamak için birçok örnekleri yazma anahtar kullanım kendi [kota](luis-boundaries.md#key-limits).  

## <a name="endpoint-key"></a>Uç noktası anahtarı
Gerektiğinde **üretim uç noktası sorguları**, bir Azure kaynağı oluşturma sonra LUIS uygulaması için atama. 

[!INCLUDE [Azure resource creation for Language Understanding and Cognitive Service resources](../../../includes/cognitive-services-luis-azure-resource-instructions.md)]

Azure kaynağı oluşturma işlemi tamamlandığında [tuşu atama](luis-how-to-azure-subscription.md) uygulamaya. 

* Uç nokta, uç noktası isabet anahtarı oluştururken belirttiğiniz kullanım planına dayanarak bir kota sağlar. Bkz: [Bilişsel hizmetler fiyatlandırması](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/?v=17.23h) fiyatlandırma bilgileri için.

* Uç nokta, tüm LUIS uygulamalar için veya belirli LUIS uygulamalar için kullanılabilir. 

* LUIS uygulamaları yazmaya yönelik uç nokta kullanmayın. 

## <a name="use-endpoint-key-in-query"></a>Sorguda uç noktası anahtarı kullanma
İki stili sorgu LUIS uç nokta kabul eder, hem uç noktası anahtarı, ancak farklı yerlerde kullanın:

|Fiili|Örnek URL'sini ve anahtarını konum|
|--|--|
|[GET](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78)|`https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=your-endpoint-key-here&verbose=true&timezoneOffset=0&q=turn%20on%20the%20lights`<br><br>Sorgu dizesi değeri için `subscription-key`<br><br>Uç nokta sorgu değerini `subscription-key` yazma (Başlangıç) anahtarından LUIS uç noktası anahtarı kota oranı kullanmak için yeni uç noktası anahtarı. Anahtarı oluşturun ve tuşu atama ancak uç noktası sorgu değerini değiştirmeyin `subscription-key`, uç nokta anahtar kotanızı kullanmıyorsunuz demektir.|
|[POST](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee79)| `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2`<br><br> üstbilgi değeri `Ocp-Apim-Subscription-Key`<br><br>Uç nokta sorgu değerini `Ocp-Apim-Subscription-Key` yazma (Başlangıç) anahtarından LUIS uç noktası anahtarı kota oranı kullanmak için yeni uç noktası anahtarı. Anahtarı oluşturun ve tuşu atama ancak uç noktası sorgu değerini değiştirmeyin `Ocp-Apim-Subscription-Key`, uç nokta anahtar kotanızı kullanmıyorsunuz demektir.|

Önceki URL'lerinde kullanılan uygulama kimliği `df67dcdb-c37d-46af-88e1-8b97951ca1c2`, genel IOT uygulaması için kullanılan [etkileşimli tanıtım](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/). 

## <a name="api-usage-of-ocp-apim-subscription-key"></a>Ocp-Apim-Subscription-Key API kullanımı
LUIS API'leri kullanan başlık `Ocp-Apim-Subscription-Key`. Üst bilgi adı, hangi anahtarı ve API kümesi kullanarak göre değişmez. Üst bilgisi için yazma API'leri geliştirme anahtarı ayarlayın. Uç noktası kullanıyorsanız, üst bilgisini uç noktası anahtarı ayarlayın. 

Yazma API'leri için uç noktası anahtarı geçiremezsiniz. Bunu yaparsanız, 401 hatası - erişim reddedildi nedeniyle geçersiz uç noktası anahtarı alın. 

## <a name="key-limits"></a>Anahtar sınırları
Bkz: [anahtar sınırları](luis-boundaries.md#key-limits) ve [Azure bölgeleri](luis-reference-regions.md). Yazma ücretsiz ve yazmak için kullanılan anahtardır. LUIS uç noktası anahtarı ücretsiz katmanda vardır ancak sizin tarafınızdan oluşturulmalı ve LUIS uygulamanızla ilişkili **Yayımla** sayfası. Yazma ancak yalnızca uç nokta sorgular için kullanılamaz.

Yayımlama bölgeler bölge geliştirme farklıdır. Yazma bölgesi istediğiniz yayımlama bölgesiyle ilgili uygulama oluşturduğunuzdan emin olun.

## <a name="key-limit-errors"></a>Anahtar sınırı hataları
Aşarsanız, ikinci kota bir HTTP 429 hatasını alıyorsunuz. Aşarsanız, aylık kota bir HTTP 403 hatası alırsınız. Bir LUIS alarak bu hataları düzeltin [uç nokta](#endpoint-key) anahtar [atama](luis-how-to-azure-subscription.md) uygulamasında anahtar **Yayımla** sayfasının [LUIS](luis-reference-regions.md#luis-website) Web sitesi.

## <a name="assignment-of-the-endpoint-key"></a>Uç nokta anahtarının atama

Yapabilecekleriniz [atama](luis-how-to-azure-subscription.md) uç noktası anahtarı [LUIS portalı](https://www.luis.ai) veya ilgili API'ler aracılığıyla. 


## <a name="next-steps"></a>Sonraki adımlar

* Bilgi [kavramları](luis-how-to-azure-subscription.md) yazma ve uç nokta anahtarları hakkında.
