---
title: Azure Logic Apps erişimin güvenliğini sağlama | Microsoft Docs
description: Güvenlik tetikleyicileri, girdileri ve çıktıları, parametreleri ve diğer hizmetleri de dahil olmak üzere Azure Logic Apps için ekleyin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
ms.assetid: 9fab1050-cfbc-4a8b-b1b3-5531bee92856
ms.topic: article
ms.date: 02/05/2019
ms.openlocfilehash: 6baeb27855381ca03862f2632d31c628a088af39
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58620623"
---
# <a name="secure-access-in-azure-logic-apps"></a>Azure Logic apps'te güvenli erişim

Mantıksal uygulamanızı nerede erişim güvenliğini sağlayabilirsiniz öğeleri şunlardır:

* [İstek veya Web kancası Tetikleyicileri](#secure-triggers)
* [Yönetme, düzenleme veya görüntüleme gibi işlemler](#secure-operations) mantıksal uygulamanızı
* [Giriş ve çıkışları](#secure-run-history) mantıksal uygulamanızdan ait çalıştırma geçmişi
* [Eylem parametreleri ve girişleri](#secure-action-parameters)
* [Get istekleri Hizmetleri](#secure-requests) mantıksal uygulamanızdan

<a name="secure-triggers"></a>

## <a name="secure-access-to-request-triggers"></a>Tetikleyiciler istemek için güvenli erişim

Mantıksal uygulamanızı kullandığında talep tabanlı bir HTTP tetikleyicisi gibi [isteği](../connectors/connectors-native-reqres.md) veya [Web kancası](../connectors/connectors-native-webhook.md) tetikleyici kısıtlayabileceğiniz erişimi yalnızca yetkili istemcilerin mantıksal uygulamanızı başlatabilmeniz. Bir mantıksal uygulama tarafından alınan tüm istekler şifrelenir ve Güvenli Yuva Katmanı (SSL) protokolü ile güvenli hale getirilmiş. Bu tetikleyici türü için erişim güvenliğini sağlayabilirsiniz farklı yolu vardır:

* [Paylaşılan erişim imzaları oluşturma](#sas)
* [Gelen IP adreslerini kısıtlamak](#restrict-incoming-ip-addresses)
* [Azure Active Directory, OAuth veya diğer güvenlik ekleme](#add-authentication)

<a name="sas"></a>

### <a name="generate-shared-access-signatures"></a>Paylaşılan erişim imzaları oluşturma

Her istek uç noktasında bir mantıksal uygulama içeren bir [paylaşılan erişim imzası (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) uç noktanın URL. Her URL içeren bir `sp`, `sv`, ve `sig` sorgu parametresi:

* `sp` izin verilen HTTP yöntemleri kullanmak için eşleyen izinleri belirtir.
* `sv` imza oluşturmak için kullanılan sürümünü belirtir.
* `sig` Tetikleyici erişimi kimlik doğrulaması için kullanılır.

İmza, tüm özellikleri ve URL yolu bir gizli erişim anahtarı ile SHA256 algoritmasını kullanarak oluşturulur. Gizli anahtar hiçbir zaman kullanıma sunulan veya yayımlanan ve şifrelenmiş ve mantıksal uygulama ile saklı tutulur. Mantıksal uygulamanızı, gizli anahtar hatalı oluşturulmuş geçerli bir imzaya sahip Tetikleyiciler yetkisi verir. 

Paylaşılan erişim imzası ile erişim güvenliğini sağlama hakkında daha fazla bilgi aşağıda verilmiştir:

* [Erişim anahtarlarını yeniden oluştur](#access-keys)
* [Süresi dolan geri çağırma URL'ler oluşturma](#expiring-urls)
* [Birincil veya ikincil anahtarıyla URL'ler oluşturma](#primary-secondary-key)

<a name="access-keys"></a>

#### <a name="regenerate-access-keys"></a>Erişim anahtarlarını yeniden oluştur

Dilediğiniz zaman yeni bir güvenli erişim anahtarı yeniden oluşturmak için Azure portalı ve Azure REST API'si kullanın. Daha önce oluşturulan tüm mantıksal uygulama tetikleyicisi için eski anahtarı geçersiz kılınır ve artık yetkili URL'lerini. Anahtarınızın yeniden oluşturulması oturumunuz sonra yeni bir erişim anahtarı ile aldığınız URL'leri.

1. Azure portalında yeniden oluşturmak istediğinizden anahtara sahip mantıksal uygulamayı açın.

1. Mantıksal uygulama menüsünde, altında **ayarları**seçin **erişim anahtarlarını**.

1. Yeniden oluşturun ve işlemi tamamlamak için istediğiniz anahtarı seçin.

<a name="expiring-urls"></a>

#### <a name="create-callback-urls-with-expiration-dates"></a>Geri çağırma URL'leri ile sona erme tarihleri oluşturma

Bir tetikleyici istek tabanlı uç noktasının URL'sini diğer kişilerle paylaşmak, geri çağırma URL'leri özel anahtarlar ve sona erme tarihleri gerektiği gibi oluşturabilirsiniz. Daha sonra sorunsuz bir şekilde anahtarları alma veya erişimi belirli bir zaman aralığı için mantıksal uygulamanız tetikleme. Bir URL kullanarak bir sona erme tarihi belirtebilirsiniz [Logic Apps REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers), örneğin:

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

Gövdesinde dahil `NotAfter`kullanan bir JSON özellik tarih dizesi. Bu özellik yalnızca kadar geçerli bir geri çağırma URL'sini döndürür `NotAfter` tarih ve saat.

<a name="primary-secondary-key"></a>

#### <a name="create-urls-with-primary-or-secondary-secret-key"></a>Birincil veya ikincil gizli anahtar ile URL'ler oluşturma

Ne zaman oluşturduğunuz veya listesi geri çağırma URL'leri istek tabanlı tetikleyiciler için URL imzalama için kullanılacak anahtarı da belirtebilirsiniz. Belirli bir anahtar kullanılarak imzalanmış bir URL oluşturabilirsiniz [Logic Apps REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers), örneğin:

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

Gövdesinde dahil `KeyType` özelliği olarak ya da `Primary` veya `Secondary`. Bu özellik belirtilen güvenli anahtarı ile imzalanmış bir URL döndürür.

<a name="restrict-incoming-ip"></a>

### <a name="restrict-incoming-ip-addresses"></a>Gelen IP adreslerini kısıtlamak

Paylaşılan erişim imzası yanı sıra, mantıksal uygulamayı çağırabilir, belirli istemcileri sınırlamak isteyebilirsiniz.  
Örneğin, Azure API Management ile istek uç noktanızı yönetiyorsanız, mantıksal uygulamanız yalnızca API Management örneğinin IP adresinden gelen istekleri kabul edecek şekilde kısıtlayabilirsiniz. 

#### <a name="set-ip-ranges---azure-portal"></a>IP aralıklarını ayarlama - Azure portalı

Azure portalında bu kısıtlama ayarlamak için mantıksal uygulamanızın ayarlarına gidin: 

1. Azure portalında mantıksal Uygulama Tasarımcısı'nda mantıksal uygulamanızı açın. 

1. Mantıksal uygulama menüsünde, altında **ayarları**seçin **iş akışı ayarları**.

1. Altında **erişim denetimi Yapılandırması** > 
**izin verilen gelen IP adresleri**seçin **belirli IP aralıkları**.

1. Altında **Tetikleyiciler için IP aralıkları**, tetikleyici kabul eden IP adresi aralıklarını belirtin. Geçerli bir IP aralığı Bu biçimler kullanır: *x.x.x.x/x* veya *x.x.x.x-x.x.x.x* 

Mantıksal uygulamanız yalnızca bir iç içe geçmiş mantıksal uygulama, gelen ateşlenmesine istiyorsanız **izin verilen gelen IP adresleri** listesinden **yalnızca diğer mantıksal uygulamalar**. Bu seçenek, Logic Apps hizmetinin (üst mantıksal uygulamalar) yalnızca çağrılarından iç içe geçmiş mantıksal uygulamayı tetikleyebilirsiniz. Bu nedenle, mantıksal uygulama kaynağı için boş bir dizi yazar.

> [!NOTE]
> IP adresi bağımsız olarak kullanarak talep tabanlı tetikleyicisine sahip bir mantıksal uygulama yine de çalıştırabilirsiniz `/triggers/{triggerName}/run` Azure REST API'sini veya API Management aracılığıyla. Ancak, bu senaryo yine de Azure REST API'sine karşı kimlik doğrulaması gerektirir ve tüm olaylar Azure denetim günlüğünde görünür. Erişim denetimi ilkeleri buna göre ayarladığınızdan emin olun.

#### <a name="set-ip-ranges---logic-app-deployment-template"></a>IP aralıklarını ayarlama - mantıksal uygulama dağıtım şablonu

Mantıksal uygulama dağıtımlarını kullanarak otomatikleştiriyorsanız bir [Azure Resource Manager dağıtım şablonu](../logic-apps/logic-apps-create-deploy-template.md), şablon içinde örneğin IP aralıklarını ayarlayabilirsiniz:

``` json
{
   "properties": {
      "definition": {},
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
   "type": "Microsoft.Logic/workflows",
}
```

<a name="add-authentication"></a>

### <a name="add-azure-active-directory-oauth-or-other-security"></a>Azure Active Directory, OAuth veya diğer güvenlik ekleme

Daha fazla yetkilendirme protokolleri mantıksal uygulamanıza eklemek için kullanmayı [Azure API Management](https://azure.microsoft.com/services/api-management/). Bu hizmet, zengin izleme, güvenlik, ilke ve herhangi bir uç nokta için belgeler sağlar ve mantıksal uygulamanızı API olarak açığa yapma olanağı tanır. API Management, Azure Active Directory, OAuth, sertifika veya diğer güvenlik standartlarını kullanabilirsiniz sonra mantıksal uygulamanız için bir genel veya özel uç nokta üzerinden kullanıma sunabilirsiniz. API yönetimi bir istek aldığında, hizmet tüm gerekli dönüştürmeleri veya yol üzerindeki kısıtlamaları da yaparak mantıksal uygulamanızı isteği gönderir. Mantıksal uygulamanızı tetikleyecek yalnızca API Management izin vermek için mantıksal uygulamanızın gelen IP aralığı ayarları kullanabilirsiniz. 

<a name="secure-operations"></a>

## <a name="secure-access-to-logic-app-operations"></a>Mantıksal uygulama işlemlerini güvenli erişim

Yalnızca belirli kullanıcılar veya mantıksal uygulamanızı işlemleri çalıştırma gruplarına izin vermek için yönetmek, düzenleme ve görüntüleme gibi görevleri erişimi kısıtlayabilirsiniz. Logic Apps destekler [Azure rol tabanlı Access Control (RBAC)](../role-based-access-control/role-assignments-portal.md), özelleştirme veya örneğin, aboneliğinizde yerleşik roller üyelerine atayın:

* **Mantıksal uygulama katkıda bulunanı**: Kullanıcılar görüntülemek, düzenlemek ve mantıksal uygulamanızı güncelleştirin. Bu rol, mantıksal uygulamanızı silme veya yönetici işlemleri çalıştırın.
* **Mantıksal uygulama operatörü**: Kullanıcıların mantıksal uygulamanız ve çalıştırma geçmişini görüntülemek ve etkinleştirebilir veya mantıksal uygulama devre dışı bırakın. Bu rol, düzenlemek veya mantıksal uygulamanızı güncelleştirin.

Mantıksal uygulamanızı silme veya değiştirme diğerlerinden önlemek için kullanabileceğiniz [Azure kaynak kilidi](../azure-resource-manager/resource-group-lock-resources.md). Bu özellik, değiştirme veya silme üretim kaynakları diğerlerinden engellemenize yardımcı olur.

<a name="secure-run-history"></a>

## <a name="secure-access-to-logic-app-run-history"></a>Mantıksal uygulama çalıştırma geçmişi güvenli erişim

Giriş veya çıkış olarak önceki mantıksal uygulama çalıştırmaları ' geçirilen içeriği korumak için belirli IP adresi aralıkları için erişimi kısıtlayabilirsiniz. Bu özellik, daha fazla erişim denetimi olanağı sunuyor. Tüm verileri bir mantıksal uygulamanın çalışma sırasında aktarım ve bekleme sırasında şifrelenir. Bir mantıksal uygulamanın çalıştırma geçmişi istediğinde, Logic Apps bu isteğin kimliğini doğrular ve girişleri bağlantılar sağlar ve istekleri ve yanıtları mantıksal uygulamanızın iş akışında çıkarır. Bu içerik yalnızca belirli bir IP adresi isteklerinden döndürülmesi için bu bağlantıları koruyabilirsiniz. Örneğin, hatta bir IP adresi gibi belirtebilirsiniz `0.0.0.0-0.0.0.0` hiç giriş ve çıkışları erişebilmek için. Yalnızca yönetici izinlerine sahip bir kişi, mantıksal uygulamanızın içeriğini "just-in-time" erişim olanağı sağlayarak bu kısıtlama, kaldırabilirsiniz.

### <a name="set-ip-ranges---azure-portal"></a>IP aralıklarını ayarlama - Azure portalı

Azure portalında bu kısıtlama ayarlamak için mantıksal uygulamanızın ayarlarına gidin:

1. Azure portalında mantıksal Uygulama Tasarımcısı'nda mantıksal uygulamanızı açın. 

1. Mantıksal uygulama menüsünde, altında **ayarları**seçin **iş akışı ayarları**.

1. Altında **erişim denetimi Yapılandırması** > 
    **izin verilen gelen IP adresleri**seçin **belirli IP aralıkları**.

1. Altında **içerikler için IP aralıkları**, girdileri ve çıktıları içeriğe erişebilir IP adresi aralıklarını belirtin. 
   Geçerli bir IP aralığı Bu biçimler kullanır: *x.x.x.x/x* veya *x.x.x.x-x.x.x.x* 

### <a name="set-ip-ranges---logic-app-deployment-template"></a>IP aralıklarını ayarlama - mantıksal uygulama dağıtım şablonu

Mantıksal uygulama dağıtımlarını kullanarak otomatikleştiriyorsanız bir [Azure Resource Manager dağıtım şablonu](../logic-apps/logic-apps-create-deploy-template.md), şablon içinde örneğin IP aralıklarını ayarlayabilirsiniz:

``` json
{
   "properties": {
      "definition": {},
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
   "type": "Microsoft.Logic/workflows",
}
```

<a name="secure-action-parameters"></a>

## <a name="secure-action-parameters-and-inputs"></a>Eylem parametreleri ve girişleri güvenliğini sağlama

Çeşitli ortamlar genelinde dağıtırken, mantıksal uygulamanızın iş akışı tanımı belirli öğeleri isteyebileceğiniz. Bu şekilde kullanmak ve hassas bilgileri korumak ortamları tabanlı girişleri sağlayabilir. Örneğin, HTTP eylemleri ile kimlik doğrulaması [Azure Active Directory](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication), tanımlamak ve istemci Kimliğini ve kimlik doğrulaması için kullanılan istemci gizli anahtarı kabul parametreleri güvenli. Bu parametreler için mantıksal uygulama tanımınızı kendi bölümüne sahiptir `parameters` bölümü.
Çalışma zamanı sırasında parametre değerlerini erişmek için kullanabileceğiniz `@parameters('parameterName')` tarafından sağlanan ifadenin [iş akışı tanımlama dili](https://aka.ms/logicappsdocs). 

Parametreler ve değerler istemediğiniz gösterilen mantıksal uygulama veya izleme çalıştırma geçmişi düzenlerken korumak için parametrelerle tanımlayabilirsiniz `securestring` yazın ve gerekirse kodlamayı kullanır. Olan bu tür parametreler kaynak tanımıyla döndürülen olmayan ve dağıtımdan sonra kaynak görüntülerken erişilemez.

> [!NOTE]
> Bir isteğin üstbilgileri veya gövdesi bir parametreyi kullanırsanız, bu parametre, mantıksal uygulamanızın çalıştırma geçmişi ve giden HTTP istek erişirken görünür olabilir. İçerik erişim ilkelerinizi de uygun şekilde ayarladığınızdan emin olun.
> Yetkilendirme üst bilgileri hiçbir zaman giriş veya çıkış görülebilir. Bu nedenle bir gizli dizi var. kullandıysanız, bu gizli dizi alınabilir değil.

Parametreleri mantıksal uygulama tanımları güvenliğini sağlama hakkında daha fazla bilgi için bkz. [parametrelerinde mantıksal uygulama tanımları güvenli](#secure-parameters-workflow) daha sonra bu sayfayı.

Dağıtımları ile otomatikleştiriyorsanız [Azure Resource Manager dağıtım şablonlarını](../azure-resource-manager/resource-group-authoring-templates.md#parameters), güvenli parametreleri bu şablonları kullanabilirsiniz. Örneğin, mantıksal uygulamanızı oluştururken KeyVault gizli dizileri almak için parametreleri kullanabilirsiniz. Dağıtım şablonu tanımınızı kendi bölümüne sahiptir `parameters` bölümünde, mantıksal uygulamanızın ayrı `parameters` bölümü. Dağıtım şablonları parametrelerinde güvenliğini sağlama hakkında daha fazla bilgi için bkz. [güvenli dağıtım şablonları parametrelerinde](#secure-parameters-deployment-template) daha sonra bu sayfayı.

<a name="secure-parameters-workflow"></a>

### <a name="secure-parameters-in-logic-app-definitions"></a>Parametreleri mantıksal uygulama tanımları güvenliğini sağlama

Mantıksal uygulama iş akışı tanımınızı hassas bilgileri korumak için mantıksal uygulamanızı kaydettikten sonra bu bilgiyi görünmez şekilde güvenli parametrelerini kullanın. Örneğin, kullanmakta olduğunuz varsayalım `Basic` bir HTTP eylem tanımındaki kimlik doğrulaması. Bu örnek içerir bir `parameters` eylem tanımı parametrelerini tanımlayan bölümü artı bir `authentication` kabul eden bölüm `username` ve `password` parametre değerleri. Bu parametrelerin değerlerini sağlamak için örneğin bir ayrı parametre dosyasını kullanabilirsiniz:

```json
"definition": {
   "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
   "actions": {
      "HTTP": {
         "type": "Http",
         "inputs": {
            "method": "GET",
            "uri": "https://www.microsoft.com",
            "authentication": {
               "type": "Basic",
               "username": "@parameters('usernameParam')",
               "password": "@parameters('passwordParam')"
            }
         },
         "runAfter": {}
      }
   },
   "parameters": {
      "passwordParam": {
         "type": "securestring"
      },
      "userNameParam": {
         "type": "securestring"
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
}
```

Gizli anahtarları kullanıyorsanız, bu gizli dizileri dağıtım sırasında kullanarak alabileceğiniz [Azure Resource Manager KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md).

<a name="secure-parameters-deployment-template"></a>

### <a name="secure-parameters-in-azure-resource-manager-deployment-templates"></a>Azure Resource Manager dağıtım şablonlarını güvenli parametreleri

Bu örnek, birden fazla çalışma zamanı parametresiyle birlikte kullanan bir Resource Manager dağıtım şablonu gösterir `securestring` türü:

* `armTemplatePasswordParam`, mantıksal uygulama tanımının için giriş `logicAppWfParam` parametresi

* `logicAppWfParam`, temel kimlik doğrulaması kullanarak HTTP eylemi için giriş

Bu örnek, bir iç içerir `parameters` mantıksal uygulamanızın iş akışı tanımı ve bir dış ait olduğu bölüm `parameters` dağıtım şablonunuza ait olduğu bölüm. Ortam parametrelerinin değerlerini belirtmek için ayrı parametre dosyasını kullanabilirsiniz. 

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {
      "logicAppName": {
         "type": "string",
         "minLength": 1,
         "maxLength": 80,
         "metadata": {
            "description": "Name of the Logic App."
         }
      },
      "armTemplatePasswordParam": {
         "type": "securestring"
      },
      "logicAppLocation": {
         "type": "string",
         "defaultValue": "[resourceGroup().location]",
         "allowedValues": [
            "[resourceGroup().location]",
            "eastasia",
            "southeastasia",
            "centralus",
            "eastus",
            "eastus2",
            "westus",
            "northcentralus",
            "southcentralus",
            "northeurope",
            "westeurope",
            "japanwest",
            "japaneast",
            "brazilsouth",
            "australiaeast",
            "australiasoutheast",
            "southindia",
            "centralindia",
            "westindia",
            "canadacentral",
            "canadaeast",
            "uksouth",
            "ukwest",
            "westcentralus",
            "westus2"
         ],
         "metadata": {
            "description": "Location of the Logic App."
         }
      }
   },
   "variables": {},
   "resources": [
      {
         "name": "[parameters('logicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('logicAppLocation')]",
         "tags": {
            "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "properties": {
            "definition": {
               "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-0601/workflowdefinition.json#",
               "actions": {
                  "HTTP": {
                     "type": "Http",
                     "inputs": {
                        "method": "GET",
                        "uri": "https://www.microsoft.com",
                        "authentication": {
                           "type": "Basic",
                           "username": "@parameters('usernameParam')",
                           "password": "@parameters('logicAppWfParam')"
                        }
                     },
                  "runAfter": {}
                  }
               },
               "parameters": {
                  "logicAppWfParam": {
                     "type": "securestring"
                  },
                  "userNameParam": {
                     "type": "securestring"
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
               "logicAppWfParam": {
                  "value": "[parameters('armTemplatePasswordParam')]"
               }
            }
         }
      }
   ],
   "outputs": {}
}
```

Gizli anahtarları kullanıyorsanız, bu gizli dizileri dağıtım sırasında kullanarak alabileceğiniz [Azure Resource Manager KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md).

<a name="secure-requests"></a>

## <a name="secure-access-to-services-receiving-requests"></a>İstekleri hizmetlerine güvenli erişim

Mantıksal uygulamanızı nerede erişmesi ve istekleri gönderir herhangi bir uç nokta güvenliğini sağlamak için bazı yollar şunlardır.

### <a name="add-authentication-on-outbound-requests"></a>Giden istekler için kimlik doğrulaması ekleme

Bir HTTP, HTTP + Swagger (açık API) veya Web kancası eylemi ile çalışırken, mantıksal uygulamanız tarafından gönderilen istek için kimlik doğrulama ekleyebilirsiniz. Örneğin, temel kimlik doğrulaması, sertifika kimlik doğrulaması veya Azure Active Directory kimlik doğrulaması kullanabilirsiniz. Daha fazla bilgi için [Tetikleyiciler veya Eylemler kimlik doğrulaması](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

### <a name="restrict-access-to-logic-app-ip-addresses"></a>Mantıksal uygulama IP adreslerine erişimi kısıtlama

Mantıksal uygulamalardan tüm çağrıları atanmış özel IP adresleri bölgeye göre gelir. Bu IP adreslerinden yalnızca isteklerini kabul eden filtreleme ekleyebilirsiniz. Bu IP adresleri için bkz: [limitler ve yapılandırma için Azure Logic Apps](logic-apps-limits-and-config.md#configuration).

### <a name="secure-on-premises-connectivity"></a>Güvenli şirket içi bağlantı

Azure Logic Apps, güvenli ve güvenilir için bu hizmetleri ile tümleştirme şirket iletişimi sağlar.

#### <a name="on-premises-data-gateway"></a>Şirket içi veri ağ geçidi

Azure Logic Apps için birçok yönetilen bağlayıcılar, şirket içi sistemler, dosya sistemi, SQL, SharePoint, DB2 ve diğerleri gibi güvenli bağlantılar sağlar. Ağ geçidi, şifrelenmiş kanallarda Azure Service Bus aracılığıyla şirket içi kaynaklardan verileri gönderir. Tüm trafiği, ağ geçidi aracının giden trafiği güvenli olarak kaynaklanır. Bilgi [şirket içi veri ağ geçidi nasıl çalıştığını](logic-apps-gateway-install.md#gateway-cloud-service).

#### <a name="azure-api-management"></a>Azure API Management

[Azure API Management](https://azure.microsoft.com/services/api-management/) siteden siteye sanal özel ağ ve ExpressRoute tümleştirme güvenli proxy ve şirket içi sistemler ile iletişim gibi şirket içi bağlantı seçenekleri sunar. Logic Apps Tasarımcısı'nda, şirket içi sistemlere hızlı erişim sağlayarak mantıksal uygulamanızın iş akışından API Management tarafından sunulan bir API seçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Dağıtım şablonu oluşturma](logic-apps-create-deploy-template.md)  
* [Özel durum işleme](logic-apps-exception-handling.md)  
* [Mantıksal uygulamalarınızı izleyin](logic-apps-monitor-your-logic-apps.md)  
* [Mantıksal uygulama hatalarını ve sorunlarını tanılayın](logic-apps-diagnosing-failures.md)  
