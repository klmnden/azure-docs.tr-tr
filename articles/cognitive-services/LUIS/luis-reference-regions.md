---
title: Yayımlama bölgeleri & uç noktaları
titleSuffix: Azure Cognitive Services
description: LUIS uygulamanızı yayımladığınız bölge, bölge veya Azure portalında bir Azure LUIS uç noktası anahtarı oluştururken belirttiğiniz konuma karşılık gelir. LUIS, otomatik olarak bir uygulama yayımladığınızda, bir anahtar ile ilişkili bölge için uç nokta URL'si oluşturur.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/07/2018
ms.author: diberry
ms.openlocfilehash: 5d8d3d4d55d4d03eb6d6a62898823158812d7c1f
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53135840"
---
# <a name="regions-and-keys"></a>Bölgeler ve anahtarlar

LUIS uygulamanızı yayımladığınız bölge, bölge veya Azure portalında bir Azure LUIS uç noktası anahtarı oluştururken belirttiğiniz konuma karşılık gelir. Olduğunda, [uygulama yayımlama](./luis-how-to-publish-app.md), LUIS otomatik olarak bir anahtar ile ilişkili bölge için uç nokta URL'si oluşturur. Birden fazla bölgeye LUIS uygulaması yayımlamak için bölge başına en az bir anahtar gerekir. 

## <a name="luis-website"></a>LUIS Web sitesi
Bölgeye göre üç LUIS Web siteleri, vardır. Yazar ve aynı bölgede yayımlama gerekir. 

|LUIS|Bölge|
|--|--|
|[www.luis.ai][www.luis.ai]|ABD<br>Avrupa'değil<br>Avustralya'değil|
|[AU.luis.ai][au.luis.ai]|Avustralya|
|[EU.luis.ai][eu.luis.ai]|Avrupa|

## <a name="regions-and-azure-resources"></a>Bölgeler ve Azure kaynakları
Uygulama LUIS Portalı'nda eklenen LUIS kaynaklarla ilişkili tüm bölgeler için yayımlanır. Örneğin, üzerinde oluşturulan bir uygulama için [www.luis.ai][www.luis.ai]LUIS kaynak oluşturma, **westus** ve uygulamaya bir kaynak olarak eklemek için uygulama o bölgenin yayımlanır. 

## <a name="public-apps"></a>Genel uygulamalar
LUIS kaynak bölge tabanlı anahtarı bir kullanıcıyla hangi bölge kendi kaynak anahtarı ile ilişkili uygulama erişebilmesi için tüm bölgelerde genel bir uygulama yayımlanır.

## <a name="publishing-regions"></a>Yayımlama bölgeleri

LUIS uygulamalar üzerinde oluşturulan https://www.luis.ai tüm uç noktalar için yayımlanan [Avrupa](#publishing-to-europe) ve [Avustralya](#publishing-to-australia) bölgeleri. 

Yazma bölgesi uygulama yalnızca bir karşılık gelen yayımlanabilir bölge yayımlayın. Uygulamanız şu anda geliştirme yanlış bölgede ise, uygulamayı dışarı aktarma ve yayımlama bölgeniz için doğru yazma bölgesini almak. 

 Genel bölge | Yazma bölgesi<br>`API region name` | Yayımlama & bölgesi sorgulama<br>`API region name`   |   LUIS Web sitesi | Uç nokta URL'si biçimi   |
|-----|------|------|------|------|
| Asya | Batı ABD<br>`westus`| Doğu Asya<br>`eastasia`     | [www.luis.ai][www.luis.ai] |  https://eastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Asya | Batı ABD<br>`westus`| Güneydoğu Asya<br>`souteastasia`     | [www.luis.ai][www.luis.ai] |   https://southeastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| *[Avustralya](#publishing-to-australia) | Avustralya Doğu<br>`australiaeast`| Avustralya Doğu<br>`australiaeast`     |   [AU.luis.ai][au.luis.ai] | https://australiaeast.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| *[Avrupa](#publishing-to-europe)| Batı Avrupa<br>`westeurope`| Kuzey Avrupa<br>`northeurope`     | [EU.luis.ai][eu.luis.ai]|  https://northeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| *[Avrupa](#publishing-to-europe) | Batı Avrupa<br>`westeurope`| Batı Avrupa<br>`westeurope`     | [EU.luis.ai][eu.luis.ai]|  https://westeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| Kuzey Amerika | Batı ABD<br>`westus` | Doğu ABD<br>`eastus`      |[www.luis.ai][www.luis.ai] |   https://eastus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Kuzey Amerika | Batı ABD<br>`westus` | Doğu ABD 2<br>`eastus2`     | [www.luis.ai][www.luis.ai] |  https://eastus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Kuzey Amerika | Batı ABD<br>`westus` | Orta Güney ABD<br>`southcentralus`     | [www.luis.ai][www.luis.ai] |  https://southcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   | 
| Kuzey Amerika | Batı ABD<br>`westus` | Batı Orta ABD<br>`westcentralus`     |[www.luis.ai][www.luis.ai] |  https://westcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |
| Kuzey Amerika | Batı ABD<br>`westus` | Batı ABD<br>`westus`  |  [www.luis.ai][www.luis.ai] | https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY  |
| Kuzey Amerika | Batı ABD<br>`westus` | Batı ABD 2<br>`westus2`    | [www.luis.ai][www.luis.ai] |  https://westus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY  |
| Güney Amerika | Batı ABD<br>`westus` | Güney Brezilya<br>`brazilsouth`     | [www.luis.ai][www.luis.ai] |  https://brazilsouth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY   |

## <a name="publishing-to-europe"></a>Avrupa yayımlama

Uygulamaları LUIS oluşturduğunuz Avrupa bölgelerine yayımlamak için https://eu.luis.ai yalnızca. LUIS, herhangi bir başka Avrupa bölgesinde bir anahtar kullanarak yayımlamak çalışırsanız, bir uyarı iletisi görüntüler. Bunun yerine, https://eu.luis.ai. LUIS uygulama oluşturulma [ https://eu.luis.ai ] [ eu.luis.ai] diğer bölgelere otomatik olarak geçirilmez. Dışarı aktarma ve ardından geçirmek için LUIS uygulaması alın.

## <a name="publishing-to-australia"></a>Avustralya yayımlama

Avustralya bölgeleri için yayımlamak için LUIS uygulamaları oluşturmanız https://au.luis.ai yalnızca. LUIS, herhangi bir başka Avustralya bölgesinde bir anahtar kullanarak yayımlamak çalışırsanız, bir uyarı iletisi görüntüler. Bunun yerine, https://au.luis.ai. LUIS uygulama oluşturulma [ https://au.luis.ai ] [ au.luis.ai] diğer bölgelere otomatik olarak geçirilmez. Dışarı aktarma ve ardından geçirmek için LUIS uygulaması alın.

## <a name="endpoints"></a>Uç Noktalar

LUIS, şu anda 2 uç noktalar vardır: biri geliştirme ve metin analizi için bir tane.

|Amaç|URL'si|
|--|--|
|Yazma|`https://{region}.api.cognitive.microsoft.com/luis/api/v2.0/apps/{appID}/`|
|Metin analizi (sorgu tahmin)|`https://{region}.api.cognitive.microsoft.com/luis/v2.0/apps/{appId}?q={q}[&timezoneOffset][&verbose][&spellCheck][&staging][&bing-spell-check-subscription-key][&log]`|

Küme ayracı ile belirtilen parametreler, aşağıdaki tabloda açıklanmaktadır `{}`, önceki tabloda.

|Parametre|Amaç|
|--|--|
|bölge|Farklı bölgelerdeki Azure bölgesi - yazma ve yayımlama sahip|
|Uygulama Kimliği|URL rotasına kullanılan ve uygulama panosunda bulunan LUIS uygulama kimliği|
|q|Sohbet Robotu gibi istemci uygulamasından gönderilen utterance metin|


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Önceden oluşturulmuş varlıklar başvurusu](./luis-reference-prebuilt-entities.md)

 [www.luis.ai]: https://www.luis.ai
 [au.luis.ai]: https://au.luis.ai
 [eu.luis.ai]: https://eu.luis.ai