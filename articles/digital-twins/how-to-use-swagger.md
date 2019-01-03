---
title: Swagger Azure dijital İkizlerini başvurusunun nasıl kullanılacağını anlama | Microsoft Docs
description: Azure dijital İkizlerini Swagger başvuru belgeleri kullanmayı anlama.
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 12/31/2018
ms.author: adgera
ms.custom: seodec18
ms.openlocfilehash: 7d079f543f8b564c396560c97225897c12f3cd24
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2019
ms.locfileid: "53975653"
---
# <a name="azure-digital-twins-swagger-reference-documentation"></a>Azure dijital İkizlerini Swagger başvuru belgeleri

Sağlanan her bir Azure dijital İkizlerini örneği otomatik olarak oluşturulan kendi Swagger başvuru belgelerini içerir.

[Swagger](https://swagger.io/), veya [Openapı](https://www.openapis.org/), karmaşık bir etkileşimli ve dilden başvuru kaynağı API bilgilere sahip. Swagger hangi JSON yükü, HTTP yöntemleri ve API üzerinde işlemler gerçekleştirmek için kullanılacak özel uç noktaları ile ilgili kritik başvuru malzemesi sağlar.

> [!IMPORTANT]
> Swagger kimlik doğrulaması desteği genel Önizleme sırasında geçici olarak devre dışı bırakıldı.

## <a name="swagger-summary"></a>Swagger özeti

Swagger içeren API'nizi etkileşimli bir özetini sunar:

* API ve nesne modeli bilgileri.
* Gerekli istek yükü, üst bilgiler, parametreleri, içerik yolları ve HTTP yöntemleri belirtin REST API uç noktaları.
* API işlevlerini test etme.
* HTTP yanıtlarını doğrulamak ve doğrulamak için kullanılan örnek yanıt bilgileri.
* Hata kod bilgisini.

Swagger, geliştirme ve test çağrıların Azure dijital İkizlerini yönetim API'leri için yardımcı olmak için kullanışlı bir araçtır.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

## <a name="reference-material"></a>Başvuru kaynakları

Otomatik olarak oluşturulan Swagger başvuru malzemesi önemli kavramları, kullanılabilir yönetim API uç noktaları ve geliştirme ve test yardımcı olmak için her nesne modeli açıklaması hızlı bir genel bakış sağlar.

Kısa bir Özet API açıklar.

![Swagger üst][1]

Yönetim API'si nesne modelini de listelenir.

![Swagger modelleri][2]

Daha ayrıntılı bir özetinin varlıkların önemli özniteliklerinin her listelenen nesne modeli seçebilirsiniz.

![Swagger modeli][3]

Tüm kullanılabilir Azure dijital İkizlerini görmek oluşturulan Swagger nesne modellerini kullanışlıdır [nesneleri ve API'leri](./concepts-objectmodel-spatialgraph.md). Geliştiriciler yapabilir bu kaynak üzerinde dijital İkizlerini Azure çözümleri derlediğinizde kullanın.

## <a name="endpoint-summary"></a>Uç nokta özeti

Swagger, ayrıca Yönetim API'leri oluşturan tüm uç noktalar kapsamlı bir genel bakış sağlar.

Listelenen her endpoint gerekli istek bilgileri gibi de içerir:

* Gerekli Parametreler.
* Gerekli parametre veri türleri.
* Kaynağa erişmek için HTTP yöntemi.

![Swagger uç noktaları][4]

Daha ayrıntılı bir genel bakış için her bir kaynak seçin.

## <a name="use-swagger-to-test-endpoints"></a>Swagger uç noktaları test etmek için kullanın

Swagger sağlayan güçlü işlevlerini bir API uç noktası doğrudan belgeleri kullanıcı Arabirimi aracılığıyla test etme olanağı biridir.

Belirli bir uç noktası seçtikten sonra gördüğünüz **denemek**.

![Swagger deneyin][5]

Gerekli ve isteğe bağlı her parametre için giriş alanlarını getirmek için bu bölümü genişletin. Uygun değerleri girin ve seçin **yürütme**.

![Swagger çalıştı][6]

Test yürüttükten sonra yanıt verileri doğrulayabilirsiniz.

## <a name="swagger-response-data"></a>Swagger yanıt verileri

Listelenen her endpoint, geliştirme ve test doğrulamak için yanıt gövdesi verileri de içerir. Bu, durum kodları ve başarılı HTTP istekleri için görmek istediğiniz JSON verilebilir.

![Swagger yanıt][7]

Örnekler de hata ayıklama veya başarısız olan testler geliştirmenize yardımcı olacak hata kodlarını içerir.

## <a name="swagger-oauth-20-authorization"></a>Swagger OAuth 2.0 yetkilendirme

OAuth 2.0 tarafından korunan istekleri etkileşimli olarak test etme hakkında daha fazla bilgi için bkz. [resmi belgelerine](https://swagger.io/docs/specification/authentication/oauth2/).

> [!NOTE]
> OAuth 2.0 kimlik doğrulaması desteği genel Önizleme sırasında geçici olarak devre dışı bırakıldı.

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital İkizlerini nesne modelleri hakkında daha fazlasını okuyun ve okuma uzamsal zeka grafiğin [Azure dijital İkizlerini anlama modelleri nesne](./concepts-objectmodel-spatialgraph.md).

Yönetim API'si ile kimlik doğrulaması yapmayı öğrenmek için [API'leri ile kimlik doğrulama](./security-authenticating-apis.md).

<!-- Images -->
[1]: media/how-to-use-swagger/swagger_management_top.PNG
[2]: media/how-to-use-swagger/swagger_management_models.PNG
[3]: media/how-to-use-swagger/swagger_management_model.PNG
[4]: media/how-to-use-swagger/swagger_management_endpoints.PNG
[5]: media/how-to-use-swagger/swagger_management_try.PNG
[6]: media/how-to-use-swagger/swagger_management_tried.PNG
[7]: media/how-to-use-swagger/swagger_management_response.PNG
