---
title: Dil anlama (HALUK) bölgeleri | Microsoft Docs
titleSuffix: Azure
description: Bu makalede HALUK Web sitesi, Azure abonelikleri ve world bölgeler için HALUK bölgelerin listesini içerir.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/19/2018
ms.author: v-geberr
ms.openlocfilehash: 86a20770178707f72cf2991ca08b6b98eaeaf0cf
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36237289"
---
# <a name="regions-and-keys"></a>Bölgeler ve anahtarları

HALUK uygulamanızı yayınlama bölge, bölge veya Azure HALUK uç noktası anahtarı oluşturduğunuzda, Azure Portalı'nda belirttiğiniz bir konuma karşılık gelir. Olduğunda, [uygulama yayımlama](./PublishApp.md), HALUK anahtar ile ilişkili bölge için bir uç nokta URL'sini otomatik olarak oluşturur. Birden fazla bölgeye HALUK uygulama yayımlamak için bölge başına en az bir anahtar gerekir. 

## <a name="luis-website"></a>HALUK Web sitesi
Bölgeye göre üç HALUK Web siteleri, vardır. Yazar ve aynı bölgede yayımlayın. 

|LUIS|Bölge|
|--|--|
|[www.luis.ai][www.luis.ai]|ABD<br>değil Avrupa<br>Avustralya için değil|
|[AU.luis.ai][au.luis.ai]|Avustralya|
|[EU.luis.ai][eu.luis.ai]|Avrupa|


## <a name="publishing-regions"></a>Yayımlama bölgeleri

Oluşturulan HALUK uygulamaları https://www.luis.ai tüm uç noktaları için yayımlanan [Avrupa](#publishing-to-europe) ve [Avustralya](#publishing-to-australia) bölgeleri. 

Geliştirme bölge uygulama yalnızca bir karşılık gelen yayımlanabilir bölge yayımlayın. Uygulamanız şu anda yanlış yazma bölgeye ise, uygulama verme ve yayımlama bölgeniz için doğru geliştirme bölge aktarın. 

 Genel bölge | Bölge yazma | Yayımlama & bölge sorgulama   |   HALUK Web sitesi | Uç noktası URL'si biçimi   |
|-----|------|------|------|------|
| Asya | Batı ABD| Doğu Asya     | [www.luis.ai][www.luis.ai] |  https://eastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Asya | Batı ABD| Güneydoğu Asya     | [www.luis.ai][www.luis.ai] |   https://southeastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| *[Avustralya](#publishing-to-australia) | Avustralya Doğu| Avustralya Doğu     |   [AU.luis.ai][au.luis.ai] | https://australiaeast.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| *[Avrupa](#publishing-to-europe)| Batı Avrupa| Kuzey Avrupa     | [EU.luis.ai][eu.luis.ai]|  https://northeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| *[Avrupa](#publishing-to-europe) | Batı Avrupa| Batı Avrupa     | [EU.luis.ai][eu.luis.ai]|  https://westeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| Kuzey Amerika | Batı ABD | Doğu ABD      |[www.luis.ai][www.luis.ai] |   https://eastus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Kuzey Amerika | Batı ABD | Doğu ABD 2     | [www.luis.ai][www.luis.ai] |  https://eastus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Kuzey Amerika | Batı ABD | Orta Güney ABD     | [www.luis.ai][www.luis.ai] |  https://southcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| Kuzey Amerika | Batı ABD | Batı Orta ABD     |[www.luis.ai][www.luis.ai] |  https://westcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Kuzey Amerika | Batı ABD | Batı ABD |  [www.luis.ai][www.luis.ai] | https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY  |
| Kuzey Amerika | Batı ABD | Batı ABD 2    | [www.luis.ai][www.luis.ai] |  https://westus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY  |
| Güney Amerika | Batı ABD | Güney Brezilya     | [www.luis.ai][www.luis.ai] |  https://brazilsouth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |

## <a name="publishing-to-europe"></a>Avrupa için yayımlama

Uygulamaların HALUK oluşturduğunuz Avrupa bölgeler yayımlamak için https://eu.luis.ai yalnızca. Avrupa bölgede başka bir anahtar kullanarak her yerden yayımlama girişiminde HALUK bir uyarı iletisi görüntüler. Bunun yerine, kullanın https://eu.luis.ai. Oluşturulma HALUK uygulamaları [ https://eu.luis.ai ] [ eu.luis.ai] diğer bölgeler için otomatik olarak geçirme. Dışarı aktarma ve geçirmek için HALUK uygulama alın.

## <a name="publishing-to-australia"></a>Avustralya için yayımlama

Uygulamaların HALUK oluşturduğunuz Avustralya bölgeler yayımlamak için https://au.luis.ai yalnızca. Avustralya bölgede başka bir anahtar kullanarak her yerden yayımlama girişiminde HALUK bir uyarı iletisi görüntüler. Bunun yerine, kullanın https://au.luis.ai. Oluşturulma HALUK uygulamaları [ https://au.luis.ai ] [ au.luis.ai] diğer bölgeler için otomatik olarak geçirme. Dışarı aktarma ve geçirmek için HALUK uygulama alın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Önceden oluşturulmuş varlık başvurusu](./luis-reference-prebuilt-entities.md)

 [www.luis.ai]: https://www.luis.ai
 [au.luis.ai]: https://au.luis.ai
 [eu.luis.ai]: https://eu.luis.ai