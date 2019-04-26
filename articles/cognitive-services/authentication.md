---
title: Kimlik Doğrulaması
titleSuffix: Cognitive Services - Azure
description: 'Bir Azure Bilişsel hizmetler kaynağı için bir istek kimliğini doğrulamak için üç yolu vardır: bir abonelik anahtarı, bir taşıyıcı belirteç veya birden çok hizmet bir abonelik. Bu makalede, her yöntemi ve istek yapma hakkında bilgi edineceksiniz.'
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: erhopf
ms.openlocfilehash: 90bc2bf4c207f3bb2727d76c2e6b4fd5597539b1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60336783"
---
# <a name="authenticate-requests-to-azure-cognitive-services"></a>Azure Bilişsel hizmetler isteklerine kimlik doğrulaması

Her bir Azure Bilişsel hizmet isteğine bir kimlik doğrulama üst bilgisi içermesi gerekir. Bir abonelik anahtarı veya erişim bu başlığı geçirir, aboneliğiniz için bir hizmet veya Hizmetleri grubunun doğrulamak için kullanılan belirteç. Bu makalede, bir istek ve her gereksinimlerini kimlik doğrulaması yapmak için yaklaşık üç yol öğreneceksiniz.

* [Bir tek hizmet abonelik anahtarı ile kimlik doğrulaması](#authenticate-with-a-single-service-subscription-key)
* [Bir hizmet birden çok abonelik anahtarı ile kimlik doğrulaması](#authenticate-with-a-multi-service-subscription-key)
* [Bir belirteç ile kimlik doğrulaması](#authenticate-with-an-authentication-token)

## <a name="prerequisites"></a>Önkoşullar

Bir isteği yapmadan önce bir Azure hesabı ve Azure Bilişsel hizmetler abonelik gerekir. Zaten bir hesabınız varsa, devam edin ve sonraki bölüme atlayın. Bir hesabınız yoksa, dakikalar içinde ayarlayın, almak için bir kılavuz sunuyoruz: [Bilişsel Hizmetler hesabı oluşturmak için Azure](cognitive-services-apis-create-account.md).

Abonelik anahtarınızı alabilirsiniz [Azure portalında](cognitive-services-apis-create-account.md#access-your-resource) hesabınızı oluşturma veya etkinleştirdikten sonra bir [ücretsiz deneme sürümü](https://azure.microsoft.com/try/cognitive-services/my-apis).
 
## <a name="authentication-headers"></a>Kimlik doğrulama üst bilgileri

Hızlı bir şekilde Azure Bilişsel hizmetler ile kullanmak için kullanılabilir kimlik doğrulama üst bilgileri gözden geçirelim.

| Üst bilgi | Açıklama |
|--------|-------------|
| Ocp-Apim-Subscription-Key | Belirli bir hizmet için bir abonelik anahtarı veya bir hizmet birden çok abonelik anahtarı ile kimlik doğrulaması için bu üstbilgiyi kullanır. |
| Ocp-Apim-abonelik-bölge | Bu üst bilgi amaçlıdır bir hizmet birden çok abonelik anahtarı ile kullanırken gerekli [Translator Text API](./Translator/reference/v3-0-reference.md). Abonelik bölgeyi belirtmek için bu üstbilgiyi kullanır. |
| Yetkilendirme | Bir kimlik doğrulama belirteci kullanıyorsanız bu üstbilgiyi kullanır. Bir belirteç değişimi gerçekleştirmek için adımlar aşağıdaki bölümlerde ayrıntılı olarak açıklanmaktadır. Sağlanan değer şu biçimdedir: `Bearer <TOKEN>`. |

## <a name="authenticate-with-a-single-service-subscription-key"></a>Bir tek hizmet abonelik anahtarı ile kimlik doğrulaması

Translator metin tanıma gibi belirli bir hizmet için bir abonelik anahtarı ile istek kimliğini doğrulamak için ilk seçenek gelir. Anahtarları, Azure portalında oluşturduğunuz her bir kaynak için kullanılabilir. Bir isteğin kimliğini doğrulamak için bir abonelik anahtarı kullanmak için bunu boyunca olarak geçirilmelidir `Ocp-Apim-Subscription-Key` başlığı.

Bu örnek istekler nasıl kullanılacağını gösteren `Ocp-Apim-Subscription-Key` başlığı. Bu örnek bir geçerli abonelik anahtarı eklemek için ihtiyacınız olacak kullanırken unutmayın.

Bu, Bing Web araması API'si için örnek bir çağrıdır:
```cURL
curl -X GET 'https://api.cognitive.microsoft.com/bing/v7.0/search?q=Welsch%20Pembroke%20Corgis' \
-H 'Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY' | json_pp
```

Bu örnek Translator Text API çağrısı.
```cURL
curl -X POST 'https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de' \
-H 'Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY' \
-H 'Content-Type: application/json' \
--data-raw '[{ "text": "How much for the cup of coffee?" }]' | json_pp
```

Aşağıdaki videoda, Bilişsel hizmetler anahtarı kullanmayı gösterir.

## <a name="authenticate-with-a-multi-service-subscription-key"></a>Bir hizmet birden çok abonelik anahtarı ile kimlik doğrulaması

>[!WARNING]
> Şu anda bu hizmetler **yoksa** çok hizmet anahtarlarını destekler: Soru-cevap Oluşturucu, konuşma Hizmetleri ve özel görüntü işleme.

Bu seçenek ayrıca isteklerinin kimliğini doğrulamak için bir abonelik anahtarı kullanır. İkisi arasındaki temel fark, bir abonelik anahtarı belirli bir hizmete bağlı değildir, bunun yerine, tek bir anahtar için birden çok Bilişsel hizmetler isteklerinin kimliğini doğrulamak için kullanılabilir olan. Bkz: [Bilişsel hizmetler fiyatlandırması](https://azure.microsoft.com/pricing/details/cognitive-services/) bölgesel kullanılabilirliği hakkında daha fazla bilgi için desteklenen özellikler ve fiyatlandırma.

Abonelik anahtarı her istekte sağlanan `Ocp-Apim-Subscription-Key` başlığı.

[![Bilişsel hizmetler için birden çok hizmet aboneliği anahtar Tanıtımı](./media/index/single-key-demonstration-video.png)](https://www.youtube.com/watch?v=psHtA1p7Cas&feature=youtu.be)

### <a name="supported-regions"></a>Desteklenen bölgeler

İsteğin yapılacağı hizmetli abonelik anahtarını kullanırken `api.cognitive.microsoft.com`, URL'de bölge içermelidir. Örneğin: `westus.api.cognitive.microsoft.com`.

Birden çok hizmet bir abonelik anahtarı ile Translator Text API kullanırken, abonelik bölgeyle belirtmelisiniz `Ocp-Apim-Subscription-Region` başlığı.

Birden çok hizmet kimlik doğrulaması bu bölgelerde desteklenir:

| | | |
|-|-|-|
| `australiaeast` | `brazilsouth` | `canadacentral` |
| `centralindia` | `eastasia` | `eastus` |
| `japaneast` | `northeurope` | `southcentralus` |
| `southeastasia` | `uksouth` | `westcentralus` |
| `westeurope` | `westus` | `westus2` |


### <a name="sample-requests"></a>Örnek istekler

Bu, Bing Web araması API'si için örnek bir çağrıdır:

```cURL
curl -X GET 'https://YOUR-REGION.api.cognitive.microsoft.com/bing/v7.0/search?q=Welsch%20Pembroke%20Corgis' \
-H 'Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY' | json_pp
```

Bu örnek Translator Text API çağrısı.

```cURL
curl -X POST 'https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de' \
-H 'Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY' \
-H 'Ocp-Apim-Subscription-Region: YOUR_SUBSCRIPTION_REGION' \
-H 'Content-Type: application/json' \
--data-raw '[{ "text": "How much for the cup of coffee?" }]' | json_pp
```

## <a name="authenticate-with-an-authentication-token"></a>Bir kimlik doğrulama belirteciyle kimlik doğrulaması

Bazı Azure Bilişsel hizmetler kabul edin ve gerektiren bazı durumlarda, bir kimlik doğrulama belirteci. Şu anda kimlik doğrulama belirteçleri bu hizmetleri destekler:

* Metin çevirisi API'si
* Konuşma Hizmetleri: Konuşmayı metne REST API
* Konuşma Hizmetleri: Metin okuma REST API

>[!NOTE]
> Soru-cevap Oluşturucu de yetkilendirme üst bilgisi kullanır, ancak bir uç noktası anahtarı gerektirir. Daha fazla bilgi için [soru-cevap Oluşturucu: Bilgi Bankası cevaplanması](./qnamaker/quickstarts/get-answer-from-kb-using-curl.md).

>[!WARNING]
> Kimlik doğrulama belirteçleri destekleyen hizmetleri zaman içinde lütfen bir hizmet için denetimi API'si başvurusu bu kimlik doğrulama yöntemini kullanmadan önce değişebilir.

Hem de hizmet tek ve çoklu hizmet Abonelik anahtarları için kimlik doğrulama belirteçlerinizi değiştirilebilir. Kimlik doğrulama belirteçlerinizi 10 dakika için geçerlidir.

Kimlik doğrulama belirteçlerinizi istek olarak dahil edilecek `Authorization` başlığı. Tarafından sağlanan belirteç değeri gelmelidir `Bearer`, örneğin: `Bearer YOUR_AUTH_TOKEN`.

### <a name="sample-requests"></a>Örnek istekler

Bir abonelik anahtarı için bir kimlik doğrulama belirteci exchange için bu URL'yi kullanın: `https://YOUR-REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken`.

```cURL
curl -v -X POST \
"https://YOUR-REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken" \
-H "Content-type: application/x-www-form-urlencoded" \
-H "Content-length: 0" \
-H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

Bu çok hizmet bölgeleri belirteç değişimi destekler:

| | | |
|-|-|-|
| `australiaeast` | `brazilsouth` | `canadacentral` |
| `centralindia` | `eastasia` | `eastus` |
| `japaneast` | `northeurope` | `southcentralus` |
| `southeastasia` | `uksouth` | `westcentralus` |
| `westeurope` | `westus` | `westus2` |

Bir kimlik doğrulama belirteci aldıktan sonra her isteği geçmesi gerekir `Authorization` başlığı. Bu örnek Translator Text API çağrısı.

```cURL
curl -X POST 'https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de' \
-H 'Authorization: Bearer YOUR_AUTH_TOKEN' \
-H 'Content-Type: application/json' \
--data-raw '[{ "text": "How much for the cup of coffee?" }]' | json_pp
```

## <a name="see-also"></a>Ayrıca bkz.

* [Bilişsel Hizmetler nedir?](welcome.md)
* [Bilişsel hizmetler fiyatlandırması](https://azure.microsoft.com/pricing/details/cognitive-services/)
* [Hesap oluşturma](cognitive-services-apis-create-account.md)
