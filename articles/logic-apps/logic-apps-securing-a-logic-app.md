---
title: "Güvenli erişim Azure Logic Apps | Microsoft Docs"
description: "Tetikleyiciler, girişleri ve çıkışları, eylem parametrelerini ve Azure mantıksal uygulamaları'nda iş akışları ile kullanılan hizmetler erişimi korumaya yönelik güvenlik ekleyin."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 9fab1050-cfbc-4a8b-b1b3-5531bee92856
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 11/22/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 45a4e476f930e0f5f6633dc5b3b35b66dc6dfa20
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="secure-access-to-your-logic-apps"></a>Mantıksal uygulamalarınızı güvenli erişim

Mantıksal uygulamanızı güvenliğini sağlamanıza yardımcı olabilecek birçok araç vardır.

* Bir mantıksal uygulama (HTTP isteği tetikleyici) tetiklemek için erişim güvenliğini sağlama
* Yönetme, düzenleme veya bir mantıksal uygulama okuma erişimi güvenli hale getirme
* Bir çalıştırma için girişleri ve çıkışları içeriğini erişimi güvenli hale getirme
* Parametreleri ya da bir iş akışında Eylemler içinde girişleri güvenliğini sağlama
* Bir iş akışından isteklerini alacak hizmetlerine erişim güvenliğini sağlama

## <a name="secure-access-to-trigger"></a>Tetiklemek için güvenli erişim

Bir HTTP isteğiyle tetiklenen bir mantıksal uygulama ile çalışırken ([isteği](../connectors/connectors-native-reqres.md) veya [Web kancası](../connectors/connectors-native-webhook.md)), böylece yalnızca yetkili istemcilerin mantıksal uygulama tetikleyebilir erişimi kısıtlayabilirsiniz. Bir mantıksal uygulama içinde tüm istekleri şifrelenir ve SSL güvenli.

### <a name="shared-access-signature"></a>Paylaşılan erişim imzası

Her istek uç noktası için bir mantıksal uygulama içeren bir [paylaşılan erişim imzası (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) URL'SİNİN bir parçası olarak. Her URL içeren bir `sp`, `sv`, ve `sig` sorgu parametresi. İzinleri tarafından belirtilen `sp`ve izin verilen, HTTP yöntemleri karşılık `sv` oluşturmak için kullanılan sürümü ve `sig` tetiklemek için erişimde kimlik doğrulaması için kullanılır. İmza, tüm özellikler ve URL yollarını gizli bir anahtar ile SHA256 algoritmasını kullanılarak oluşturulur. Gizli anahtar hiçbir zaman kullanıma sunulan ve yayımlanan ve şifrelenmiş ve mantığı uygulamanın parçası olarak depolanan tutulur. Mantıksal uygulama gizli anahtarı ile oluşturulan geçerli bir imzası içeren Tetikleyicileri yalnızca yetkilendirir.

#### <a name="regenerate-access-keys"></a>Erişim anahtarlarını yeniden oluştur

Yeni güvenli bir anahtar, REST API veya Azure Portal'dan dilediğiniz zaman yeniden oluşturabilirsiniz. Eski anahtarı kullanarak önceden oluşturulan tüm geçerli URL'ler geçersiz ve artık mantıksal uygulama yangın yetkisine.

1. Azure portalında bir anahtarı yeniden oluşturmak istediğiniz mantıksal uygulama açın
1. Tıklatın **erişim tuşları** menü öğesi altında **ayarları**
1. Yeniden oluşturun ve işlemi tamamlamak için anahtarı seçin

Yeniden oluşturma tamamlandıktan sonra alma URL'leri yeni erişim anahtarı ile imzalanmış.

#### <a name="creating-callback-urls-with-an-expiration-date"></a>Geri çağırma URL'leri bir sona erme tarihi ile oluşturma

Diğer kuruluşlarla URL paylaşıyorsanız, özel anahtarları ve gerektiği gibi sona erme tarihleri ile URL'leri oluşturabilir. Sonra sorunsuzca anahtarları alma, veya bir uygulama yangın erişimi belirli bir timespan sınırlı olduğundan emin olun. Bir URL yolu için bir sona erme tarihi belirtebilirsiniz [logic apps REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers):

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

Özellik gövdesinde dahil `NotAfter` bir JSON tarih dizesi döndüren yalnızca geçerliliğinin bir geri çağırma URL'si `NotAfter` tarih ve saat.

#### <a name="creating-urls-with-primary-or-secondary-secret-key"></a>Birincil veya ikincil gizli anahtarla URL'ler oluşturma

Oluşturmak ya da istek tabanlı tetikleyiciler için geri çağırma URL'leri listesinde URL imzalamak için kullanılan hangi anahtar da belirtebilirsiniz.  Belirli bir anahtarı tarafından imzalanan bir URL oluşturabileceğiniz [logic apps REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers) gibi:

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

Özellik gövdesinde dahil `KeyType` olarak `Primary` veya `Secondary`.  Bu, belirtilen güvenli anahtar tarafından imzalanmış bir URL döndürür.

### <a name="restrict-incoming-ip-addresses"></a>Gelen IP adreslerini kısıtlamak

Paylaşılan erişim imzası yanı sıra, yalnızca belirli istemcilerinden bir mantıksal uygulama çağırma sınırlamak isteyebilirsiniz.  Örneğin, uç noktanızı Azure API Management üzerinden yönetiyorsanız, yalnızca isteği API Management örneği IP adresinden geldiğinde isteğini kabul etmek için mantıksal uygulama kısıtlayabilirsiniz.

Bu ayar mantığını uygulaması ayarları içinde yapılandırılabilir:

1. Azure Portal'da, IP adresi sınırlamaları eklemek istediğiniz mantıksal uygulama açın
1. Tıklatın **erişim denetimini yapılandırma** menü öğesi altında **ayarları**
1. Tetik tarafından kabul edilmesi için IP adres aralıklarına listesini belirtin

Geçerli bir IP aralığı biçimini alır `192.168.1.1/255`. Yalnızca bir iç içe geçmiş mantıksal uygulama tetiklenecek mantıksal uygulama istiyorsanız seçin **yalnızca diğer logic apps** seçeneği. Bu seçenek, anlamı yalnızca çağırır hizmetin kendisini (üst mantıksal uygulamalar) kaynak için boş bir dizi Yazar başarıyla tetiklenecek.

> [!NOTE]
> Hala bir mantıksal uygulama isteği tetikleyici ile REST API aracılığıyla çalıştırabilirsiniz / Yönetim `/triggers/{triggerName}/run` IP ne olursa olsun. Bu senaryo Azure REST API'sine karşı kimlik doğrulaması gerektirir ve tüm olayları Azure denetim günlüğünde görünür. Set erişim ilkelerini uygun şekilde denetler.

#### <a name="setting-ip-ranges-on-the-resource-definition"></a>Kaynak tanımı'nda IP aralıklarını ayarlama

Kullanıyorsanız bir [dağıtım şablonu](logic-apps-create-deploy-template.md) dağıtımlarınızı otomatikleştirmek için IP aralığı ayarlarını kaynak şablonu yapılandırılabilir.  

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

Bir mantıksal uygulama üzerinde daha fazla yetkilendirme protokolleri eklemek için [Azure API Management](https://azure.microsoft.com/services/api-management/) zengin izleme, güvenlik, İlkesi ve bir mantıksal uygulama bir API olarak kullanıma sunmak için özelliğine sahip herhangi bir uç nokta için belgeler sağlar. Azure API Management genel veya özel uç noktası için Azure Active Directory, sertifika, OAuth veya diğer güvenlik standartları kullanabilirsiniz mantıksal uygulama getirebilir. Bir istek alındığında, Azure API Management (herhangi bir gerekli dönüşümleri veya kısıtlamaları yürütülen gerçekleştirerek) mantıksal uygulama isteği iletir. Mantıksal uygulama gelen IP aralığı ayarları yalnızca API Yönetimi'nden tetiklenmesi mantıksal uygulama izin vermek için kullanabilirsiniz.

## <a name="secure-access-to-manage-or-edit-logic-apps"></a>Güvenli erişim yönetmek veya logic apps düzenlemek için

Böylece yalnızca belirli kullanıcılara veya gruplara kaynak üzerinde işlem gerçekleştirmek için bir mantıksal uygulama yönetimi işlemleri için erişimi kısıtlayabilirsiniz. Logic apps kullanan Azure [rol tabanlı erişim denetimi (RBAC)](../active-directory/role-based-access-control-configure.md) özellik ve aynı araçları ile özelleştirilebilir.  Aboneliğinize üyeleri de atayabilirsiniz birkaç yerleşik roller vardır:

* **Mantığı uygulamasını katkıda bulunan** -görüntüleme, düzenleme ve bir mantıksal uygulama güncelleştirmek için erişim sağlar.  Kaynak kaldıramaz veya yönetim işlemleri.
* **Mantıksal uygulama işleci** - mantıksal uygulama görüntüleyebilir ve çalıştırma geçmişi ve etkinleştir/devre dışı bırak.  Düzenleyemez veya tanımını güncelleştirin.

Aynı zamanda [Azure kaynak kilidi](../azure-resource-manager/resource-group-lock-resources.md) değiştirme veya silme logic apps önlemek için. Bu özellik, üretim kaynaklardan değişiklikleri ya da silme işlemleri engellemek için faydalıdır.

## <a name="secure-access-to-contents-of-the-run-history"></a>Çalıştırma geçmişi içeriğini güvenli erişim

Belirli IP adresi aralıkları önceki çalışır gelen girişleri veya çıkışları in içeriğine erişimi kısıtlayabilirsiniz.  

Yoldaki ve bekleyen iş akışı çalışması içinde tüm veriler şifrelenir. Geçmiş çalıştırmak için bir çağrı yapıldığında, hizmet isteğin kimliğini doğrular ve isteği ve yanıt girişleri ve çıkışları bağlantılar sağlar. Belirtilen IP adresi aralığından içeriği görüntülemek için yalnızca istekler içeriği döndürür şekilde bu bağlantıyı korunabilir. Ek erişim denetimi için bu özelliği kullanabilirsiniz. Bir IP adresi gibi bile belirtebilirsiniz `0.0.0.0` hiç bir girdi/çıktı erişebilecek şekilde. İş akışı içeriği 'just-in-time' erişimi için olasılığını sağlayan yalnızca yönetici izinlerine sahip olan kişi bu kısıtlama kaldırabilirsiniz.

Bu ayar, Azure portalında kaynak ayarları içinde yapılandırılabilir:

1. Azure Portal'da, IP adresi sınırlamaları eklemek istediğiniz mantıksal uygulama açın
1. Tıklatın **erişim denetimini yapılandırma** menü öğesi altında **ayarları**
1. IP adresi aralıkları için içeriğe erişimi için bir liste belirtin

#### <a name="setting-ip-ranges-on-the-resource-definition"></a>Kaynak tanımı'nda IP aralıklarını ayarlama

Kullanıyorsanız bir [dağıtım şablonu](logic-apps-create-deploy-template.md) dağıtımlarınızı otomatikleştirmek için IP aralığı ayarlarını kaynak şablonu yapılandırılabilir.  

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

## <a name="secure-parameters-and-inputs-within-a-workflow"></a>Güvenli parametreleri ve iş akışı içinde girişleri

Dağıtım için bir iş akışı tanımı bazı yönlerini ortamlar genelinde Parametreleştirme isteyebilirsiniz. Ayrıca, bazı parametreler gibi bir istemci kimliği ve istemci parolası için bir iş akışı düzenlerken görüntülenmesini istemediğiniz güvenli parametreler olabilir [Azure Active Directory kimlik doğrulaması](../connectors/connectors-native-http.md#authentication) bir HTTP eylem.

### <a name="using-parameters-and-secure-parameters"></a>Parametreleri ve güvenli parametrelerini kullanma

Çalışma zamanında kaynak parametresinin değeri erişmek için [iş akışı tanımlama dili](http://aka.ms/logicappsdocs) sağlayan bir `@parameters()` işlemi. Ayrıca, [kaynak dağıtım şablonu parametrelerini belirtin](../azure-resource-manager/resource-group-authoring-templates.md#parameters). Ancak parametre türü olarak belirtirseniz, `securestring`, parametre kaynak tanımı geri kalanı ile döndürülen olmaz ve dağıtımdan sonra kaynak görüntüleyerek erişilebilir olmayacaktır.

> [!NOTE]
> Parametreniz üstbilgileri ya da bir istek gövdesi kullanılırsa, parametre görünür çalıştırma geçmişi ve giden HTTP istek erişerek olabilir. İçerik erişim ilkelerinizi uygun şekilde ayarladığınızdan emin olun.
> Yetkilendirme üstbilgileri hiçbir zaman girişleri veya çıkışları görünür. Bu nedenle gizli var. kullanılıyorsa, gizli alınabilir değil.

#### <a name="resource-deployment-template-with-secrets"></a>Gizli anahtarlarla kaynak dağıtım şablonu

Aşağıdaki örnek, bir secure parametresi başvuruda bulunan bir dağıtım gösterir `secret` çalışma zamanında. Ayrı Parametreler dosyasında ortamı değerini belirtebilirsiniz `secret`, veya kullanmak [Azure Resource Manager KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) almak için zaman adresindeki gizlilik dağıtın.

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
  "resources": [
    {
      "name": "secret-deploy",
      "type": "Microsoft.Logic/workflows",
      "location": "westus",
      "tags": {
        "displayName": "LogicApp"
      },
      "apiVersion": "2016-06-01",
      "properties": {
        "definition": {
          "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "actions": {
            "Call_External_API": {
              "type": "http",
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
    }
  ],
  "outputs": {}
}
```

## <a name="secure-access-to-services-receiving-requests-from-a-workflow"></a>Bir iş akışından isteklerini almak hizmetlerine güvenli erişim

Mantıksal uygulama erişmesi gereken herhangi bir uç nokta güvenliğini sağlamak için birçok yolu vardır.

### <a name="using-authentication-on-outbound-requests"></a>Giden isteklerinde kimlik doğrulaması kullanma

Bir HTTP, HTTP + Swagger (açık API) veya Web kancası eylemi ile çalışırken, gönderilen isteği kimlik doğrulaması ekleyebilirsiniz. Temel kimlik doğrulaması, sertifika kimlik doğrulaması veya Azure Active Directory kimlik doğrulaması dahil olabilir. Bu kimlik doğrulaması yapılandırma hakkında ayrıntılar bulunabilir [bu makalede](../connectors/connectors-native-http.md#authentication).

### <a name="restricting-access-to-logic-app-ip-addresses"></a>Mantıksal uygulama IP adreslerine erişimi kısıtlama

Logic apps gelen tüm çağrıları belirli bir IP adresleri her bölge kümesini gelmektedir. Yalnızca bu atanan IP adreslerinden gelen istekleri kabul edecek şekilde filtreleme ek ekleyebilirsiniz. Bu IP adresleri listesi için bkz: [mantığı uygulama sınırlarını ve yapılandırmasını](logic-apps-limits-and-config.md#configuration).

### <a name="on-premises-connectivity"></a>Şirket içi bağlantı

Logic apps güvenli ve güvenilir sağlamak üzere birkaç hizmetleriyle tümleştirme içi iletişim sağlar.

#### <a name="on-premises-data-gateway"></a>Şirket içi veri ağ geçidi

Birçok yönetilen bağlayıcılar mantıksal uygulamalar için şirket içi sistemlere dosya sistemi, SQL, SharePoint, DB2 ve daha fazlası da dahil olmak üzere, güvenli bağlantı sağlar. Ağ geçidi şifrelenmiş kanalda Azure Service Bus aracılığıyla şirket içi kaynaklardan veri aktarır. Ağ geçidi aracısından güvenli giden trafik olarak tüm trafiğin kaynaklandığı. Daha fazla bilgi edinmek [veri ağ geçidinin nasıl çalıştığını](logic-apps-gateway-install.md#gateway-cloud-service).

#### <a name="azure-api-management"></a>Azure API Management

[Azure API Management](https://azure.microsoft.com/services/api-management/) güvenli proxy'si için siteden siteye VPN ve ExpressRoute tümleştirme ve şirket içi sistemleriyle iletişim dahil olmak üzere şirket içi bağlantı seçenekleri vardır. Mantıksal Uygulama Tasarımcısı'nda, hızlı bir şekilde şirket içi sistemlere hızlı erişim sağlayan Azure API Yönetimi'nden bir iş akışı içinde kullanıma sunulan bir API seçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Bir dağıtım şablonu oluşturma](logic-apps-create-deploy-template.md)  
[Özel durum işleme](logic-apps-exception-handling.md)  
[Mantıksal uygulamalarınızı izleyin](logic-apps-monitor-your-logic-apps.md)  
[Mantıksal uygulama hatalarını ve sorunlarını tanılama](logic-apps-diagnosing-failures.md)  
