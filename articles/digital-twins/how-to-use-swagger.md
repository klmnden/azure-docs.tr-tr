---
title: Swagger Azure dijital İkizlerini başvurusunun nasıl kullanılacağını anlama | Microsoft Docs
description: Azure dijital İkizlerini Swagger başvuru belgeleri kullanmayı anlama.
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 06/29/2019
ms.author: v-adgera
ms.custom: seodec18
ms.openlocfilehash: 0b8c2b50e00c8e9727b09a454504d214a3060fe4
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67502697"
---
# <a name="azure-digital-twins-swagger-reference-documentation"></a>Azure dijital İkizlerini Swagger başvuru belgeleri

Sağlanan her bir Azure dijital İkizlerini örneği otomatik olarak oluşturulan kendi Swagger başvuru belgelerini içerir.

[Swagger](https://swagger.io/), veya [Openapı](https://www.openapis.org/), karmaşık bir etkileşimli ve dilden başvuru kaynağı API bilgilere sahip. Swagger hangi JSON yükü, HTTP yöntemleri ve API üzerinde işlemler gerçekleştirmek için kullanılacak özel uç noktaları ile ilgili kritik başvuru malzemesi sağlar.

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

[![Swagger üst](media/how-to-use-swagger/swagger_management_top.PNG)](media/how-to-use-swagger/swagger_management_top.PNG#lightbox)

Yönetim API'si nesne modelini de listelenir.

[![Swagger modelleri](media/how-to-use-swagger/swagger_management_models.PNG)](media/how-to-use-swagger/swagger_management_models.PNG#lightbox)

Daha ayrıntılı bir özetinin varlıkların önemli özniteliklerinin her listelenen nesne modeli seçebilirsiniz.

[![Swagger modeli](media/how-to-use-swagger/swagger_management_model.PNG)](media/how-to-use-swagger/swagger_management_model.PNG#lightbox)

Tüm kullanılabilir Azure dijital İkizlerini görmek oluşturulan Swagger nesne modellerini kullanışlıdır [nesneleri ve API'leri](./concepts-objectmodel-spatialgraph.md). Azure üzerinde dijital İkizlerini çözümleri derlediğinizde, geliştiriciler bu kaynak kullanabilir.

## <a name="endpoint-summary"></a>Uç nokta özeti

Swagger, ayrıca Yönetim API'leri oluşturan tüm uç noktalar kapsamlı bir genel bakış sağlar.

Listelenen her endpoint gerekli istek bilgileri gibi de içerir:

* Gerekli Parametreler.
* Gerekli parametre veri türleri.
* Kaynağa erişmek için HTTP yöntemi.

[![Swagger uç noktaları](media/how-to-use-swagger/swagger_management_endpoints.PNG)](media/how-to-use-swagger/swagger_management_endpoints.PNG#lightbox)

Daha ayrıntılı bir genel bakış için her bir kaynak seçin.

## <a name="use-swagger-to-test-endpoints"></a>Swagger uç noktaları test etmek için kullanın

Swagger sağlayan güçlü işlevlerini bir API uç noktası doğrudan belgeleri kullanıcı Arabirimi aracılığıyla test etme olanağı biridir.

Belirli bir uç noktası seçtikten sonra gördüğünüz **denemek**.

[![Swagger deneyin](media/how-to-use-swagger/swagger_management_try.PNG)](media/how-to-use-swagger/swagger_management_try.PNG#lightbox)

Gerekli ve isteğe bağlı her parametre için giriş alanlarını getirmek için bu bölümü genişletin. Doğru değerleri girin ve seçin **yürütme**.

[![Swagger çalıştı](media/how-to-use-swagger/swagger_management_tried.PNG)](media/how-to-use-swagger/swagger_management_tried.PNG#lightbox)

Test yürüttükten sonra yanıt verileri doğrulayabilirsiniz.

## <a name="swagger-response-data"></a>Swagger yanıt verileri

Listelenen her endpoint, geliştirme ve test doğrulamak için yanıt gövdesi verileri de içerir. Bu, durum kodları ve başarılı HTTP istekleri için görmek istediğiniz JSON verilebilir.

[![Swagger yanıt](media/how-to-use-swagger/swagger_management_response.PNG)](media/how-to-use-swagger/swagger_management_response.PNG#lightbox)

Örnekler de hata ayıklama veya başarısız olan testler geliştirmenize yardımcı olacak hata kodlarını içerir.

## <a name="swagger-oauth-20-authorization"></a>Swagger OAuth 2.0 yetkilendirme

> [!NOTE]
> * Azure dijital İkizlerini kaynak oluşturan kullanıcı asıl alan Yönetici rolü atama olacaktır ve diğer kullanıcılar için ek rol atamaları oluşturmak mümkün olacaktır. API'leri çağırmak için bu kullanıcıları ve rollerini yetkilendirilebilir.

1. Bağlantısındaki [Bu hızlı başlangıçta](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad) veya [ile Azure Active Directory eski Azure dijital İkizlerini uygulamanızı kaydetmeniz](./how-to-use-legacy-aad.md) oluşturmak ve bir Azure AD uygulaması yapılandırmak için. Alternatif olarak, var olan bir uygulama kaydı yeniden kullanabilirsiniz.

1. Aşağıdaki yanıt URL'si için uygulama kaydı ekleyin:

    ```plaintext
    https://YOUR_SWAGGER_URL/ui/oauth2-redirect-html
    ```
    | Ad  | Şununla değiştir | Örnek |
    |---------|---------|---------|
    | YOUR_SWAGGER_URL | Yönetim REST API'si belgeleri URL'nizi portalda bulunamadı  | `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/swagger` |

1. Azure AD uygulamanızın Kimliğini kopyalayın.

Azure Active Directory kaydı tamamladıktan sonra:

1. Seçin **Authorize** swagger sayfanızda düğmesi.

    [![Düğme seçin Swagger Yetkilendir](media/how-to-use-swagger/swagger-select-authorize-btn.png)](media/how-to-use-swagger/swagger-select-authorize-btn.png#lightbox)

1. Uygulama Kimliği yapıştırın **client_id** alan.

    [![Swagger client_id alan](media/how-to-use-swagger/swagger-auth-form.png)](media/how-to-use-swagger/swagger-auth-form.png#lightbox)

1. Ardından kalıcı aşağıdaki başarısı için yönlendirilirsiniz.

    [![Swagger kalıcı yeniden yönlendirme](media/how-to-use-swagger/swagger_auth_redirect.png)](media/how-to-use-swagger/swagger_auth_redirect.png#lightbox)

OAuth 2.0 tarafından korunan istekleri etkileşimli olarak test etme hakkında daha fazla bilgi için bkz. [resmi belgelerine](https://swagger.io/docs/specification/authentication/oauth2/).

## <a name="next-steps"></a>Sonraki adımlar

- Azure dijital İkizlerini nesne modelleri hakkında daha fazlasını okuyun ve okuma uzamsal zeka grafiğin [Azure dijital İkizlerini anlama modelleri nesne](./concepts-objectmodel-spatialgraph.md).

- Yönetim API'si ile kimlik doğrulaması yapmayı öğrenmek için [API'leri ile kimlik doğrulama](./security-authenticating-apis.md).