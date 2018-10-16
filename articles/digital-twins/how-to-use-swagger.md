---
title: Azure dijital İkizlerini Swagger kullanmayı anlama | Microsoft Docs
description: Azure dijital İkizlerini Swagger'ı kullanma
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: adgera
ms.openlocfilehash: dbadc90a206937d4c9f1d7b75a872d93b1a8a587
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324351"
---
# <a name="how-to-use-azure-digital-twins-swagger"></a>Azure dijital İkizlerini Swagger'ı kullanma

Sağlanan her bir Azure dijital İkizlerini örneği otomatik olarak oluşturulan kendi Swagger başvuru belgelerini içerir.

[Swagger](https://swagger.io/) (veya [Openapı](https://www.openapis.org/)) karmaşık API bilgi etkileşimli ve dilden başvurusu kaynağına sahip. Özellikle, Swagger hangi JSON yükü, HTTP yöntemleri ve API üzerinde işlemler gerçekleştirmek için kullanılacak özel uç noktaları ile ilgili kritik başvuru malzemesi sağlar.

## <a name="swagger-summary"></a>Swagger özeti

Swagger API dahil, etkileşimli bir Özet sağlar:

* API ve nesne modeli bilgileri.
* REST API uç noktaları belirleme, istek yükü, üst bilgiler, parametreleri, içerik yolları ve HTTP yöntemleri gereklidir.
* API işlevlerini test etme.
* Doğrulama ve HTTP yanıtlarını onaylama için örnek yanıt bilgileri.
* Hata kod bilgisini.

Swagger bu nedenle geliştirme ve yönetim API'sine yapılan test çağrıları yardımcı olmak için kullanışlı bir araçtır.

> [!TIP]
> Başvuru için API özelliği göstermek için bir Swagger önizlemesi sağlanır.
> Konumunda barındırılan [docs.westcentralus.azuresmartspaces.net/management/swagger](https://docs.westcentralus.azuresmartspaces.net/management/swagger).

Oluşturulan, kendi yönetim API'si Swagger belgelerimize erişebilirsiniz:

```plaintext
https://yourInstanceName.yourLocation.azuresmartspaces.net/management/swagger
```

| Özel öznitelik adı | Değiştirin |
| --- | --- |
| `yourInstanceName` | Azure dijital İkizlerini örneğinizin adı |
| `yourLocation` | Örneğiniz üzerinde barındırılıyorsa hangi sunucu bölge |

## <a name="reference-material"></a>Başvuru kaynakları

Otomatik olarak oluşturulan başvuru malzemesi önemli kavramları ve nesne modellerini açıklar.

Kısa bir Özet API açıklanmaktadır:

![Swagger üst][1]

Core API'si nesne modelini de listelenir:

![Swagger modelleri][2]

Daha ayrıntılı bir özetinin varlıkların önemli özniteliklerinin her listelenen nesne modeline tıklayabilirsiniz:

![Swagger modeli][3]

Tüm kullanılabilir Azure dijital İkizlerini görmek oluşturulan Swagger nesne modellerini kullanışlıdır [nesneleri ve API'leri](./concepts-objectmodel-spatialgraph.md). Çözüm, Azure üzerinde dijital İkizlerini oluştururken kullanılacak geliştiriciler için harika bir kaynaktır.

## <a name="endpoint-summary"></a>Uç nokta özeti

Swagger API oluşturan tüm uç noktalar kapsamlı bir bakış da sağlar.

Listelenen her uç nokta da gibi gerekli istek bilgileri içerir:

* Gerekli Parametreler.
* Gerekli parametre veri türleri.
* Kaynağa erişmek için HTTP yöntemi.

![Swagger uç noktaları][4]

Daha ayrıntılı bir genel bakış için her kaynak tıklanabilecek.

## <a name="using-swagger-to-test-endpoints"></a>Uç noktalar test etmek için Swagger'ı kullanma

Swagger sağlayan güçlü işlevlerini biri yeteneği **denemek** veya bir API uç noktası doğrudan belgeleri kullanıcı Arabirimi aracılığıyla test edin.

Göreceğiniz belirli bir uç noktaya tıklandıktan sonra bir **denemek** düğmesi:

![Swagger deneyin][5]

Bu bölümü genişleterek her gerekli ve isteğe bağlı parametre için giriş alanlarını getirir. Uygun değerleri girin ve tıklatın **yürütme**:

![Swagger çalıştı][6]

Test yürütüldükten sonra yanıt verileri doğrulayabilirsiniz.

## <a name="swagger-response-data"></a>Swagger yanıt verileri

Listelenen her endpoint, geliştirme ve test doğrulamak için yanıt gövdesi verileri de içerir. Bu örnek, istenen durum kodları ve JSON başarılı HTTP istekleri için verilebilir.

![Swagger yanıt][7]

Örnekler de hata ayıklama veya başarısız olan testler geliştirmenize yardımcı olacak hata kodlarını içerir.

## <a name="swagger-oauth-20-authorization"></a>Swagger OAuth 2.0 yetkilendirme

Etkileşimli olarak istekleri OAuth 2.0 tarafından korunan API'si kaynaklarına karşı test etmek için bkz: [resmi belgelerine](https://swagger.io/docs/specification/authentication/oauth2/).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için Azure dijital İkizlerini nesne modelleri ve uzamsal zeka graph hakkında bilgi edinin [bu makalede](./concepts-objectmodel-spatialgraph.md).

Yönetim API'si ile kimlik doğrulaması yapmayı öğrenmek için [API'leri ile kimlik doğrulaması](./security-authenticating-apis.md).

<!-- Images -->
[1]: media/how-to-use-swagger/swagger_management_top.png
[2]: media/how-to-use-swagger/swagger_management_models.png
[3]: media/how-to-use-swagger/swagger_management_model.png
[4]: media/how-to-use-swagger/swagger_management_endpoints.png
[5]: media/how-to-use-swagger/swagger_management_try.png
[6]: media/how-to-use-swagger/swagger_management_tried.png
[7]: media/how-to-use-swagger/swagger_management_response.png
