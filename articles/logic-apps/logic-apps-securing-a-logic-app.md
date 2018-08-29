---
title: Azure Logic Apps erişimin güvenliğini sağlama | Microsoft Docs
description: Azure Logic Apps için Tetikleyiciler, girişler ve çıkışlar, eylem parametrelerini ve Hizmetleri iş akışlarında erişimi koruma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
ms.assetid: 9fab1050-cfbc-4a8b-b1b3-5531bee92856
ms.topic: article
ms.date: 11/22/2016
ms.openlocfilehash: fc4fdff5080e6ebe13850157e8d560a1d31e7719
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43127488"
---
# <a name="secure-access-in-azure-logic-apps"></a>Azure Logic apps'te güvenli erişim

Mantıksal uygulamanızın farklı bileşenlerine erişimi güvenliğini sağlayabilirsiniz yollar şunlardır:

* HTTP isteği tetikleyicisi olan bir mantıksal uygulama iş akışı tetiklemek için erişimi güvenli hale getirin.
* Güvenli erişimi yönetme, düzenleme veya mantıksal uygulama okuma için.
* Bir mantıksal uygulama çalıştırması için giriş ve çıkışları içine içeriklere erişimi güvenli hale getirin.
* Parametreleri veya bir mantıksal uygulama iş akışı eylemi için girişler güvenli hale getirin.
* Bir mantıksal uygulama iş akışından isteklerini alacak hizmetlerine erişimi güvenli hale getirin.

## <a name="secure-access-to-trigger"></a>Tetiklemek için güvenli erişim

Bir HTTP isteği ile tetiklenen mantıksal uygulama çalışırken ([istek](../connectors/connectors-native-reqres.md) veya [Web kancası](../connectors/connectors-native-webhook.md)), böylece yalnızca yetkili istemcilerin mantıksal uygulama tetikleyebilir erişimi kısıtlayabilirsiniz. Tüm istekleri bir mantıksal uygulama ile şifrelenir ve SSL ile güvenli.

### <a name="shared-access-signature"></a>Paylaşılan erişim imzası

Mantıksal uygulama için her istek uç noktası içeren bir [paylaşılan erişim imzası (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) URL'SİNİN bir parçası olarak. Her URL içeren bir `sp`, `sv`, ve `sig` sorgu parametresi. Tarafından belirtilen izinleri `sp`ve izin verilen, HTTP yöntemleri için karşılık gelen `sv` oluşturmak için kullanılan sürümü ve `sig` tetiklemek için kimlik doğrulaması yapmak için kullanılır. İmza, tüm özellikleri ve URL yolu bir gizli anahtar SHA256 algoritmasını kullanarak oluşturulur. Gizli anahtar hiçbir zaman kullanıma sunulan ve yayımlanan ve şifrelenmiş ve mantıksal uygulamanın parçası olarak saklı tutulur. Mantıksal uygulamanız yalnızca gizli anahtar hatalı oluşturulmuş geçerli bir imzaya sahip Tetikleyiciler yetkisi verir.

#### <a name="regenerate-access-keys"></a>Erişim anahtarlarını yeniden oluştur

Yeni bir güvenli anahtar, REST API'si veya Azure portalından dilediğiniz zaman yeniden oluşturabilirsiniz. Daha önce eski anahtarı kullanılarak oluşturulan tüm geçerli URL'leri geçersiz kılındı ve mantıksal uygulama ateşlenmesine artık yetkili.

1. Bir anahtar oluşturmak istediğiniz mantıksal uygulamayı Azure portalında açın
1. Tıklayın **erişim anahtarlarını** menü öğesi altında **ayarları**
1. Yeniden oluşturun ve işlemi tamamlamak için tuşuna basın

Yeniden oluşturma tamamlandıktan sonra alma URL'leri yeni erişim anahtarı ile imzalanmıştır.

#### <a name="creating-callback-urls-with-an-expiration-date"></a>Geri çağırma URL'leri bir sona erme tarihi ile oluşturma

URL ile diğer taraflara paylaşıyorsanız, özel anahtarları ve gerektiğinde sona erme tarihleri ile URL'leri oluşturabilirsiniz. Daha sonra sorunsuz bir şekilde anahtarları alma veya bir uygulama ateşlenmesine erişim belirli bir timespan sınırlı olduğundan emin olun. Bir URL yolu için bir sona erme tarihi belirtin [logic apps REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers):

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

Özelliğini ve gövde içeren `NotAfter` JSON tarih dizesi yalnızca kadar geçerli olan bir geri çağırma URL'sini döndürür `NotAfter` tarih ve saat.

#### <a name="creating-urls-with-primary-or-secondary-secret-key"></a>URL'ler ile birincil veya ikincil gizli anahtar oluşturma

Oluşturduğunuz veya talep tabanlı Tetikleyicileri için geri çağırma URL'leri listesi, hangi anahtar URL'sini imzalamak için kullanılacak de belirtebilirsiniz.  Belirli bir anahtar ile imzalanmış bir URL oluşturabilirsiniz [logic apps REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers) gibi:

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

Özelliğini ve gövde içeren `KeyType` olarak `Primary` veya `Secondary`.  Bu, belirtilen güvenli anahtar tarafından imzalanmış bir URL döndürür.

### <a name="restrict-incoming-ip-addresses"></a>Gelen IP adreslerini kısıtlamak

Paylaşılan erişim imzası ek olarak, bir mantıksal uygulama yalnızca belirli istemcilerden çağırma kısıtlamak isteyebilirsiniz.  Örneğin, Azure API Management aracılığıyla uç noktanızı yönetiyorsanız, yalnızca API Management örneği IP adresinden istek geldiğinde isteği kabul etmek için mantıksal uygulama kısıtlayabilirsiniz.

Bu ayar, mantıksal uygulama ayarlarında yapılandırılabilir:

1. IP adresi sınırlamaları eklemek istediğiniz mantıksal uygulamayı Azure portalında açın
1. Tıklayın **iş akışı ayarları** menü öğesi altında **ayarları**
1. Tetik tarafından kabul edilmesi için IP adresi aralıkları listesini belirtin

Geçerli bir IP aralığı biçimini alır `192.168.1.1/255`. Mantıksal uygulamanın yalnızca iç içe geçmiş mantıksal uygulama harekete istiyorsanız belirleyin **yalnızca diğer mantıksal uygulamalar** seçeneği. Bu seçenek, anlamı yalnızca hizmetin kendisini (üst mantıksal uygulamalar) çağırır kaynak için boş bir dizi Yazar başarıyla yangın.

> [!NOTE]
> Yine de bir mantıksal uygulama istek tetikleyicisi ile REST API aracılığıyla çalıştırabileceğiniz / Yönetim `/triggers/{triggerName}/run` IP ne olursa olsun. Bu senaryo Azure REST API'sine karşı kimlik doğrulaması gerektirir ve tüm olayları Azure denetim günlüğünde görünür. Kümesi erişimi ilkeleri buna göre denetler.

#### <a name="setting-ip-ranges-on-the-resource-definition"></a>Kaynak tanımında IP aralıklarını ayarlama

Kullanıyorsanız bir [dağıtım şablonu](logic-apps-create-deploy-template.md) dağıtımlarınızı otomatikleştirin kaynak şablonunda IP aralığı ayarları yapılandırılabilir.  

``` json
{
    "properties": {
        "definition": {
        },
        "parameters": {},
        "accessControl": {
            "triggers": {
                "allowedCallerIpAddresses": [
                    {
                        "addressRange": "192.168.12.0/23"
                    },
                    {
                        "addressRange": "2001:0db8::/64"
                    }
                ]
            }
        }
    },
    "type": "Microsoft.Logic/workflows"
}

```

### <a name="adding-azure-active-directory-oauth-or-other-security"></a>Azure Active Directory, OAuth veya diğer güvenlik ekleme

Daha fazla yetkilendirme protokolleri üzerinde bir mantıksal uygulama eklemek için [Azure API Management](https://azure.microsoft.com/services/api-management/) zengin izleme, güvenlik, ilke ve belgeleri için herhangi bir uç noktası ile bir mantıksal uygulama bir API olarak açığa olanağı sunar. Azure API Management, Azure Active Directory, sertifika, OAuth veya diğer güvenlik standartlarını kullanabilirsiniz mantıksal uygulama için bir genel veya özel uç nokta üzerinden kullanıma sunabilirsiniz. Bir istek alındığında, Azure API Yönetimi (tüm gerekli dönüştürmeleri veya kısıtlamaları uçuşan gerçekleştirerek) mantıksal uygulama isteği iletir. Mantıksal uygulama gelen IP aralığı ayarları, yalnızca API Yönetimi'nden tetiklenmesi için mantıksal uygulama izin vermek için kullanabilirsiniz.

## <a name="secure-access-to-manage-or-edit-logic-apps"></a>Güvenli erişimi yönetmek ya da mantıksal uygulamaları düzenleme

Yalnızca belirli kullanıcılar veya gruplar kaynak üzerinde işlemler gerçekleştirmek mümkün olmasını sağlamak için bir mantıksal uygulama yönetim işlemlerini erişimi kısıtlayabilirsiniz. Azure Logic apps kullanın [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/role-assignments-portal.md) özellik ve aynı araçlarla özelleştirilebilir.  Aboneliğinize üyeleri de atayabilirsiniz birkaç yerleşik rolü vardır:

* **Mantıksal uygulama katkıda bulunanı** -görüntüleme, düzenleme ve bir mantıksal uygulama güncelleştirmek için erişim sağlar.  Kaynak kaldıramaz veya yönetim işlemleri gerçekleştirin.
* **Mantıksal uygulama operatörü** - mantıksal uygulama görüntüleyebilir ve çalıştırma geçmişi ve etkinleştirebilir/devre dışı.  Düzenleyemez veya tanımını güncelleştirin.

Ayrıca [Azure kaynak kilidi](../azure-resource-manager/resource-group-lock-resources.md) değiştirme veya logic apps silmeden önlemek için. Bu özellik, üretim kaynaklardan değişiklikleri veya silme işlemlerini önlemek için değerlidir.

## <a name="secure-access-to-contents-of-the-run-history"></a>Çalıştırma geçmişini içeriğini erişimin güvenliğini sağlama

Önceki çalıştırmalardan belirli IP adresi aralıkları için giriş veya çıkış içeriği için erişimi kısıtlayabilirsiniz.  

Bir iş akışı çalıştırması içindeki tüm veriler aktarımda ve bekleme sırasında şifrelenir. Çalıştırma geçmişi için bir çağrı yapıldığında hizmet isteğin kimliğini doğrular ve isteği ve yanıtı giriş ve çıkışları için bağlantılar sağlar. Belirtilen IP adresi aralığındaki içeriği görüntülemek için yalnızca istek içeriği döndürülmesi için bu bağlantıyı korunabilir. Ek erişim denetimi için bu özelliği kullanabilirsiniz. Bir IP adresi gibi bile belirtebilirsiniz `0.0.0.0` hiç girdiler/çıktılar erişim. Yalnızca yönetici izinlerine sahip bir kişi, bu kısıtlama, 'just-in-time' iş akışı içeriklere erişimi için olanağı sağlayarak kaldırabilirsiniz.

Bu ayar, Azure portal'ın kaynak ayarlarında yapılandırılabilir:

1. IP adresi sınırlamaları eklemek istediğiniz mantıksal uygulamayı Azure portalında açın
2. Tıklayın **erişim denetimi Yapılandırması** menü öğesi altında **ayarları**
3. İçeriğe erişim için IP adresi aralıkları listesini belirtin

#### <a name="setting-ip-ranges-on-the-resource-definition"></a>Kaynak tanımında IP aralıklarını ayarlama

Kullanıyorsanız bir [dağıtım şablonu](logic-apps-create-deploy-template.md) dağıtımlarınızı otomatikleştirin kaynak şablonunda IP aralığı ayarları yapılandırılabilir.  

``` json
{
    "properties": {
        "definition": {
        },
        "parameters": {},
        "accessControl": {
            "contents": {
                "allowedCallerIpAddresses": [
                    {
                        "addressRange": "192.168.12.0/23"
                    },
                    {
                        "addressRange": "2001:0db8::/64"
                    }
                ]
            }
        }
    },
    "type": "Microsoft.Logic/workflows"
}
```

## <a name="secure-parameters-and-inputs-within-a-workflow"></a>Güvenli parametreleri ve bir iş akışındaki girişleri

Ortamlar arasında dağıtımı için bir iş akışı tanımı bazı yönlerini Parametreleştirme isteyebilirsiniz. Ayrıca, bazı parametreler güvenli parametreleri gibi bir istemci kimliği ve istemci gizli anahtarı için bir iş akışı düzenlerken görünmesini istemiyorsanız olabilir [Azure Active Directory kimlik doğrulaması](../connectors/connectors-native-http.md#authentication) HTTP eylem.

### <a name="using-parameters-and-secure-parameters"></a>Parametreleri ve güvenli parametreleri kullanma

Çalışma zamanında, bir kaynak parametresinin değeri erişmeye [iş akışı tanımlama dili](http://aka.ms/logicappsdocs) sağlayan bir `@parameters()` işlemi. Ayrıca, [kaynak dağıtım şablonunun parametrelerini belirtin](../azure-resource-manager/resource-group-authoring-templates.md#parameters). Ancak parametre türü olarak belirtirseniz `securestring`, parametre kaynak tanımı geri kalanı ile döndürülen olmaz ve dağıtımdan sonra kaynak görüntüleyerek erişilebilir olmaz.

> [!NOTE]
> Parametreniz üstbilgileri veya bir istek gövdesi kullanılırsa, parametre görünür çalıştırma geçmişi ve giden HTTP istek erişerek olabilir. İçerik erişim ilkelerinizi uygun şekilde ayarladığınızdan emin olun.
> Yetkilendirme üst bilgileri hiçbir zaman giriş veya çıkış görülebilir. Bu nedenle gizli dizi var. kullanılıyorsa, gizli dizi alınabilir değil.

#### <a name="resource-deployment-template-with-secrets"></a>Gizli kaynak dağıtım şablonu

Aşağıdaki örnek, güvenli bir parametre olarak başvuran bir dağıtımı gösterir `secret` zamanında. Ayrı Parametreler dosyasında, ortam değeri belirtebilirsiniz `secret`, veya [Azure Resource Manager KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) almak için zaman, gizli dizileri dağıtma.

``` json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {
      "secretDeploymentParam": {
         "type": "securestring"
      }
   },
   "variables": {},
   "resources": [ {
      "name": "secret-deploy",
      "type": "Microsoft.Logic/workflows",
      "location": "westus",
      "tags": {
         "displayName": "LogicApp"
      },
      "apiVersion": "2016-06-01",
      "properties": {
         "definition": {
            "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
            "actions": {
               "Call_External_API": {
                  "type": "Http",
                  "inputs": {
                     "headers": {
                        "Authorization": "@parameters('secret')"
                     },
                     "body": "This is the request"
                  },
                  "runAfter": {}
               }
            },
            "parameters": {
               "secret": {
                  "type": "SecureString"
               }
            },
            "triggers": {
               "manual": {
                  "type": "Request",
                  "kind": "Http",
                  "inputs": {
                     "schema": {}
                  }
               }
            },
            "contentVersion": "1.0.0.0",
            "outputs": {}
         },
         "parameters": {
            "secret": {
               "value": "[parameters('secretDeploymentParam')]"
            }
         }
      }
   } ],
   "outputs": {}
}
```

## <a name="secure-access-to-services-receiving-requests-from-a-workflow"></a>Bir iş akışından istekleri alma hizmetlerine güvenli erişim

Mantıksal uygulamanın erişmesi gereken herhangi bir uç nokta güvenliğini sağlamak için birçok yolu vardır.

### <a name="using-authentication-on-outbound-requests"></a>Giden istekler için kimlik doğrulaması kullanma

Bir HTTP, HTTP + Swagger (açık API) veya Web kancası eylemi ile çalışırken, gönderilen istek için kimlik doğrulama ekleyebilirsiniz. Temel kimlik doğrulaması, sertifika kimlik doğrulaması veya Azure Active Directory kimlik doğrulaması dahil olabilir. Bu kimlik doğrulaması yapılandırma hakkında daha fazla bilgi bulunabilir [bu makaledeki](../connectors/connectors-native-http.md#authentication).

### <a name="restricting-access-to-logic-app-ip-addresses"></a>Mantıksal uygulama IP adreslerine erişimi kısıtlama

Belirli bir bölge başına IP adresleri kümesini mantıksal uygulamalardan tüm çağrıları gelir. Ek yalnızca belirlenen bu IP adreslerinden gelen istekleri kabul edecek şekilde filtre ekleyebilirsiniz. Bu IP adresleri listesi için bkz. [logic Apps sınırları ve yapılandırma](logic-apps-limits-and-config.md#configuration).

### <a name="on-premises-connectivity"></a>Şirket içi bağlantı

Logic apps, güvenli ve güvenilir sağlamak için çeşitli Hizmetleri ile tümleştirme şirket iletişimi sağlar.

#### <a name="on-premises-data-gateway"></a>Şirket içi veri ağ geçidi

Logic apps için birçok yönetilen bağlayıcılar, şirket içi sistemlere, dosya sistemi, SQL, SharePoint, DB2 ve daha fazlası dahil olmak üzere güvenli bağlantı sağlar. Ağ geçidi, şifrelenmiş kanallarda Azure Service Bus aracılığıyla şirket içi kaynaklardan gelen verileri geçirir. Tüm trafiği, ağ geçidi aracının giden trafiği güvenli olarak kaynaklanır. Daha fazla bilgi edinin [veri ağ geçidi nasıl çalıştığını](logic-apps-gateway-install.md#gateway-cloud-service).

#### <a name="azure-api-management"></a>Azure API Management

[Azure API Management](https://azure.microsoft.com/services/api-management/) güvenli proxy'si için siteden siteye VPN ve ExpressRoute tümleştirme ve şirket içi sistemler ile iletişim dahil olmak üzere şirket içi bağlantı seçenekleri vardır. Logic Apps Tasarımcısı'nda, şirket içi sistemlere hızlı erişim sağlayarak Azure API Management'ı bir iş akışı içinde kullanıma sunulan bir API hızlı bir şekilde seçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Dağıtım şablonu oluşturma](logic-apps-create-deploy-template.md)  
[Özel durum işleme](logic-apps-exception-handling.md)  
[Mantıksal uygulamalarınızı izleyin](logic-apps-monitor-your-logic-apps.md)  
[Mantıksal uygulama hatalarını ve sorunlarını tanılama](logic-apps-diagnosing-failures.md)  
