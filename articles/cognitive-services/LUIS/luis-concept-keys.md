---
title: HALUK anahtarlarınızı - Azure anlama | Microsoft Docs
description: Uygulamanızı yazmak ve, endpoing sorgulamak için dil anlama (HALUK) tuşlarını kullanın.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/23/2018
ms.author: v-geberr
ms.openlocfilehash: 70bca3b181e02f42da50e827154193936544131a
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36263827"
---
# <a name="keys-in-luis"></a>HALUK anahtarlarında
HALUK iki anahtar kullanır: [yazma](#programmatic-key) ve [endpoint](#endpoint-key). Geliştirme anahtarı HALUK hesabınızı oluşturduğunuzda, sizin için otomatik olarak oluşturulur. HALUK uygulamanızı yayımlamak hazır olduğunuzda, gerek [uç noktası anahtarı oluşturma](luis-how-to-azure-subscription.md#create-luis-endpoint-key), [onu atayın](Manage-keys.md#assign-endpoint-key) HALUK uygulamanıza ve [endpoint sorgu ile birlikte kullanmak](#use-endpoint-key-in-query). 

|Anahtar|Amaç|
|--|--|
|[Anahtar yazma](#programmatic-key)|Geliştirme, ortak çalışanların yönetme yayımlama, sürüm oluşturma|
|[Uç noktası anahtarı](#endpoint-key)| Sorgulama|

HALUK uygulamalarında Yazar önemlidir [bölgeleri](luis-reference-regions.md#publishing-regions) , ayrıca istediğiniz yayımlama ve sorgu.

<a name="programmatic-key" ></a>
## <a name="authoring-key"></a>Anahtar yazma

Bir başlangıç anahtarı olarak da bilinen bir geliştirme anahtarı HALUK hesabı oluşturun ve boş olduğunda otomatik olarak oluşturulur. Her yazma tüm HALUK uygulama arasında bir geliştirme anahtara sahip [bölge](luis-reference-regions.md). Geliştirme anahtar HALUK uygulamanızı yazar veya uç nokta sorguları test etmek için sağlanır. 

Geliştirme anahtarı bulmak için oturum [HALUK] [ LUIS] ve açmak için sağ üst gezinti çubuğundaki hesap adını tıklatın **hesap ayarlarını**.

![Anahtar yazma](./media/luis-concept-keys/programatic-key.png)

Yapmak istediğiniz zaman **üretim uç noktası sorguları**, Azure oluşturma [HALUK abonelik](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/). 

> [!CAUTION]
> Birkaç uç nokta çağrılarında sağlar kolaylık sağlamak için birçok örnekleri yazma anahtar kullanamadı kendi [kota](luis-boundaries.md#key-limits).  

## <a name="endpoint-key"></a>Uç noktası anahtarı
 Gerektiğinde **üretim uç noktası sorguları**, oluşturma bir [HALUK anahtar](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/) Azure portalında. Anahtar oluşturmak için kullanılan ad unutmayın, uygulamaya anahtar eklediğinizde ihtiyacınız..

HALUK abonelik işlemi tamamlandıktan sonra [anahtarı eklemek](Manage-keys.md#assign-endpoint-key) üzerinde uygulamaya **Yayımla** sayfası. 

Uç noktası anahtarı anahtar oluşturulurken belirtilen kullanım plana göre uç noktası isabet kotası sağlar. Bkz: [Bilişsel hizmetler fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/?v=17.23h) fiyatlandırma bilgileri için.

Uç noktası anahtarı tüm HALUK uygulamalar için veya belirli HALUK uygulamalar için kullanılabilir. 

Uç noktası anahtarı HALUK uygulamaları yazmak için kullanmayın. 

## <a name="use-endpoint-key-in-query"></a>Sorguda uç noktası anahtarı kullanma
Sorgu iki stillerini HALUK uç nokta kabul eder, her ikisi de uç nokta anahtar, ancak farklı yerlerde kullanın:

|Fiil|Örnek URL'si ve anahtar konumu|
|--|--|
|[GET](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78)|https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/98998dcf-66d2-468e-840a-7c7c57549b5a?subscription-key=your-endpoint-key-here&verbose=true&timezoneOffset=0&q=turn üzerinde ışık<br><br>Sorgu dizesi değeri `subscription-key`<br><br>Uç nokta sorgu değerini değiştirmek `subscription-key` HALUK uç nokta anahtar kota hızı kullanmak için yeni uç nokta anahtarına yazma (Başlangıç) anahtarı. Anahtar oluşturma ve anahtar atamak ancak abonelik anahtar uç nokta sorgu değeri değiştirmeyin ', uç nokta anahtar kota kullanmıyorsanız.|
|[POST](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee79)| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/98998dcf-66d2-468e-840a-7c7c57549b5a<br><br> üstbilgi değeri `Ocp-Apim-Subscription-Key`<br><br>Uç nokta sorgu değerini değiştirmek `Ocp-Apim-Subscription-Key` HALUK uç nokta anahtar kota hızı kullanmak için yeni uç nokta anahtarına yazma (Başlangıç) anahtarı. Anahtar oluşturma ve anahtar atarsanız ancak uç nokta sorgu değeri değiştirmeyin `Ocp-Apim-Subscription-Key`, uç nokta anahtar kota kullanmıyorsanız.|

## <a name="api-usage-of-ocp-apim-subscription-key"></a>Ocp Apim abonelik anahtar API kullanımı
HALUK API'ları kullanmak üst `Ocp-Apim-Subscription-Key`. Üstbilgi adı hangi anahtar ve API kümesi kullandığınız göre değişmez. Üstbilgi geliştirme anahtarına yazma API'ler ayarlayın. Uç noktası kullanıyorsanız, üstbilgi için uç nokta anahtarını ayarlayın. 

API'ları yazmak için uç nokta anahtar geçilemez. Bunu yaparsanız, 401 hatası - erişim reddedildi nedeniyle geçersiz abonelik anahtar alın. 

## <a name="key-limits"></a>Anahtar sınırları
Bkz: [anahtar sınırları](luis-boundaries.md#key-limits) ve [Azure bölgeleri](luis-reference-regions.md). Geliştirme boş ve yazmak için kullanılan anahtardır. HALUK abonelik anahtarı ücretsiz katmanına sahip, ancak sizin tarafınızdan oluşturulmalı ve HALUK uygulamanızla ilişkili **Yayımla** sayfası. Yazma, ancak yalnızca son nokta sorgular için kullanılamaz.

Yayımlama bölgeler bölgeler yazma farklıdır. Uygulama geliştirme bölge istediğiniz yayımlama bölge ilgili oluşturduğunuzdan emin olun.

## <a name="key-limit-errors"></a>Anahtar sınırı hataları
Aşarsanız, ikinci kota bir HTTP 429 hatasını alıyorsunuz. Aşarsanız, ay kota bir HTTP 403 hatası alırsınız. Bir HALUK alarak bu hataları düzeltin [endpoint](#endpoint-key) anahtar [atama](Manage-keys.md#assign-endpoint-key) uygulamasını anahtarına **Yayımla** sayfasında [HALUK] [ LUIS] Web sitesi.

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi [kavramları](Manage-Keys.md#assign-endpoint-key) yazma ve uç nokta anahtarları hakkında.

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
