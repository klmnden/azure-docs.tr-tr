---
title: Resource Manager REST API'leri | Microsoft Docs
description: "Resource Manager REST API'leri kimlik doğrulama ve kullanım örnekleri genel bakış"
services: azure-resource-manager
documentationcenter: na
author: navalev
manager: timlt
editor: 
ms.assetid: e8d7a1d2-1e82-4212-8288-8697341408c5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/13/2017
ms.author: navale;tomfitz;
ms.openlocfilehash: 2f7ba23775545637de865f9ef63680ae22c62164
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="resource-manager-rest-apis"></a>Resource Manager REST API'leri
> [!div class="op_single_selector"]
> * [Azure PowerShell](powershell-azure-resource-manager.md)
> * [Azure CLI](xplat-cli-azure-resource-manager.md)
> * [Portal](resource-group-portal.md) 
> * [REST API](resource-manager-rest-api.md)
> 
> 

Yapılan her çağrı için Azure Resource Manager dağıtılan her şablon arkasında arkasında her yapılandırılan depolama hesabı var. bir veya daha fazla Azure Resource Manager'ın RESTful API çağrıları Bu konu, bu API'leri ve nasıl bunları herhangi SDK kullanmadan çağırabilirsiniz geçer. Bu yaklaşım, tam denetim Azure isteklerinin istediğiniz veya tercih ettiğiniz dili için SDK'sı kullanılabilir değilse veya işlemlerini desteklemediğinden, ihtiyacınız varsa yararlıdır.

Bu makalede, Azure'da gösterilir, ancak yerine bunları nasıl bağlanacağını örnekleri olarak bazı işlemler kullanan her bir API aracılığıyla geçmez. Temel kavramları öğrendikten sonra okuyabilirsiniz [Azure Resource Manager REST API Başvurusu](https://docs.microsoft.com/rest/api/resources/) rest API'leri kullanma hakkında ayrıntılı bilgi için.

## <a name="authentication"></a>Kimlik Doğrulaması
Kimlik doğrulama kaynak yöneticisi için Azure Active Directory (AD tarafından) ele alınır. Herhangi bir API'yi bağlanmak için önce her istek için geçmesi kimlik doğrulama belirtecini almak için Azure AD ile kimlik doğrulaması gerekir. Saf çağrısı REST API'lerini doğrudan tanımlamakta olduğunuz gibi bir kullanıcı adı ve parola istendiğinde tarafından kimlik doğrulaması istemediğiniz varsayalım. İki faktörlü kimlik doğrulama mekanizmaları kullanmıyorsanız varsayıyoruz. Bu nedenle, hangi Azure AD uygulaması ve hizmet asıl oturum açmak için kullanılan adlandırılır oluşturun. Ancak Azure AD destekleyen birden fazla kimlik doğrulama yordamları ve bunların tümünün sonraki API istekleri için ihtiyacımız bu kimlik doğrulama belirtecini almak için kullanılabilecek unutmayın.
İzleyin [oluşturma Azure AD uygulaması ve hizmet sorumlusu](resource-group-create-service-principal-portal.md) adım adım yönergeler için.

### <a name="generating-an-access-token"></a>Bir erişim belirteci oluşturma
Azure AD kimlik doğrulaması, login.microsoftonline.com bulunan Azure AD ile çağırarak yapılır. Kimlik doğrulaması yapmak için aşağıdaki bilgilere sahip olmanız gerekir:

* Azure AD Kiracı kimliği (genellikle aynı şirketinizin oturum açma kullanarak ancak gerekli değildir, Azure AD adı)
* Uygulama Kimliği (Azure AD uygulama oluşturma adımında taken)
* (Azure AD uygulaması oluştururken seçtiğiniz) parola

Aşağıdaki HTTP istek "Azure AD Kiracı kimliği", "Uygulama kimliği" ve "Parola" doğru değerlerle değiştirdiğinizden emin olun.

**Genel HTTP isteği:**

```HTTP
POST /<Azure AD Tenant ID>/oauth2/token?api-version=1.0 HTTP/1.1 HTTP/1.1
Host: login.microsoftonline.com
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&resource=https%3A%2F%2Fmanagement.core.windows.net%2F&client_id=<Application ID>&client_secret=<Password>
```

... (doğrulama başarılı olursa) olacak aşağıdaki yanıta benzer bir yanıt neden:

```json
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1448199959",
  "not_before": "1448196059",
  "resource": "https://management.core.windows.net/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhb...86U3JI_0InPUk_lZqWvKiEWsayA"
}
```
(Önceki yanıtta access_token kısaltılmış okunabilirliğini artırmak için)

**Erişim belirteci Bash kullanarak oluşturuluyor:**

```console
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=client_credentials&resource=https://management.core.windows.net/&client_id=<application id>&client_secret=<password you selected for authentication>" https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0
```

**PowerShell kullanarak erişim belirteci oluşturuluyor:**

```powershell
Invoke-RestMethod -Uri https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0 -Method Post
 -Body @{"grant_type" = "client_credentials"; "resource" = "https://management.core.windows.net/"; "client_id" = "<application id>"; "client_secret" = "<password you selected for authentication>" }
```

Yanıt, bir erişim belirteci, ne kadar süreyle bu belirtecin geçerli olduğu hakkında bilgi ve hangi kaynak için bu belirteci kullanabilirsiniz hakkında bilgiler içerir.
Önceki HTTP çağrısında aldığınız erişim belirtecini tüm istekleri için kaynak yöneticisi API'si geçirilmesi gerekir. "Taşıyıcı YOUR_ACCESS_TOKEN" değeriyle "Yetkilendirme" adlı bir üstbilgi değeri olarak geçirin. "Bearer" ve erişim belirtecinizi arasındaki boşluğu dikkat edin.

Belirteç yukarıdaki HTTP sonucundan görebileceğiniz gibi bir belirli süre boyunca önbelleğe ve gerekir, aynı belirteci yeniden için geçerlidir. Her API çağrısı için Azure AD kimlik doğrulaması mümkün olsa bile, yüksek oranda verimsiz olacaktır.

## <a name="calling-resource-manager-rest-apis"></a>Arama Resource Manager REST API'leri
Bu konuda, temel kullanım REST işlemlerinin açıklamak için yalnızca birkaç API'leri kullanır. Tüm işlemleri hakkında daha fazla bilgi için bkz: [Azure Resource Manager REST API'leri](https://docs.microsoft.com/rest/api/resources/).

### <a name="list-all-subscriptions"></a>Tüm abonelikleri listeler
Yapabileceğiniz basit işlemleri erişmek için kullanılabilir abonelikler listesine biridir. Aşağıdaki istekte nasıl erişim belirteci başlık olarak geçirilen bakın:

(YOUR_ACCESS_TOKEN, gerçek erişim belirteci ile değiştirin.)

```HTTP
GET /subscriptions?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

... ve sonuç olarak, bu hizmet sorumlusu erişmesine izin verilip Aboneliklerin listesini alın

(Abonelik kimlikleri okunabilirlik için kısaltılmıştır)

```json
{
  "value": [
    {
      "id": "/subscriptions/3a8555...555995",
      "subscriptionId": "3a8555...555995",
      "displayName": "Azure Subscription",
      "state": "Enabled",
      "subscriptionPolicies": {
        "locationPlacementId": "Internal_2014-09-01",
        "quotaId": "Internal_2014-09-01"
      }
    }
  ]
}
```

### <a name="list-all-resource-groups-in-a-specific-subscription"></a>Belirli bir Abonelikteki tüm kaynak gruplarının listesi
Resource Manager API'leri kullanılarak tüm kaynaklar bir kaynak grubu içinde yuvalanmış. Kaynak Yöneticisi'ni kullanarak aşağıdaki HTTP GET isteği aboneliğinizde için mevcut kaynak grupları sorgulayabilirsiniz. Nasıl abonelik kimliği URL'SİNİN bir parçası bu süre geçirilen dikkat edin.

(YOUR_ACCESS_TOKEN ve ABONELİK_KİMLİĞİ gerçek erişim belirtecini ve abonelik Kimliğinizle değiştirin)

```HTTP
GET /subscriptions/SUBSCRIPTION_ID/resourcegroups?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

Aldığınız yanıt tanımlanmış bir kaynak grubu olup olmadığına bağlıdır ve bu durumda, kaç tane.

(Abonelik kimlikleri okunabilirlik için kısaltılmıştır)

```json
{
    "value": [
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/myfirstresourcegroup",
            "name": "myfirstresourcegroup",
            "location": "eastus",
            "properties": {
                "provisioningState": "Succeeded"
            }
        },
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/mysecondresourcegroup",
            "name": "mysecondresourcegroup",
            "location": "northeurope",
            "tags": {
                "tagname1": "My first tag"
            },
            "properties": {
                "provisioningState": "Succeeded"
            }
        }
    ]
}
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Şu ana kadar biz yalnızca bilgi için Resource Manager API'leri sorgulama. Biz bazı kaynaklar oluşturun ve en basit tümünü, bir kaynak grubu tarafından başlayalım zamanı geldi. Aşağıdaki HTTP isteği bir bölge/konum tercih ettiğiniz bir kaynak grubu oluşturur ve bir etiket ekler.

(YOUR_ACCESS_TOKEN, ABONELİK_KİMLİĞİ, RESOURCE_GROUP_NAME, gerçek erişim belirteci, abonelik kimliği ve oluşturmak istediğiniz kaynak grubunun adıyla değiştirin)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  }
}
```

Başarılı olursa, aşağıdaki yanıta benzer bir yanıt alın:

```json
{
  "id": "/subscriptions/3a8555...555995/resourceGroups/RESOURCE_GROUP_NAME",
  "name": "RESOURCE_GROUP_NAME",
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  },
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

Azure'da bir kaynak grubu başarıyla oluşturdunuz. Tebrikler!

### <a name="deploy-resources-to-a-resource-group-using-a-resource-manager-template"></a>Kaynakları bir Resource Manager şablonu kullanarak bir kaynak grubuna Dağıt
Resource Manager ile şablonları kullanarak kaynaklarınızı dağıtabilirsiniz. Bir şablonu bazı kaynaklar ve bağımlılıklarını tanımlar. Bu bölüm için Resource Manager şablonları hakkında bilginiz ve yalnızca dağıtımı başlatmak için API çağrısı yapma gösteriyoruz varsayıyoruz. Şablonları oluşturma hakkında daha fazla bilgi için bkz: [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).

Dağıtım şablonu, diğer API'leri çağırmak nasıl çok farklı değildir. Önemli bir özelliği, bir şablon dağıtımını oldukça uzun zaman alabilir ' dir. API çağrısı yalnızca döndürür ve dağıtım yapıldığında öğrenmek için geliştirici olarak sorgulamak için dağıtım durumunu olduğunu. Daha fazla bilgi için bkz: [izlemek zaman uyumsuz Azure işlemleri](resource-manager-async-operations.md).

Bu örnekte, kullanılabilir bir genel olarak sunulan şablon kullanırız [GitHub](https://github.com/Azure/azure-quickstart-templates). Kullanırız şablonu, Batı ABD bölgesi için bir Linux VM dağıtır. Bu örnek, GitHub gibi ortak bir havuzda kullanılabilir bir şablonu kullanıyor olsa bile, bunun yerine tam şablon isteğin bir parçası geçiş yapabilir. Dağıtılan şablonu içinde kullanılan parametre değerlerini istekte sağladığımız unutmayın.

(İsteğiniz uygun değerlere ABONELİK_KİMLİĞİ, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD ve DNS_NAME_FOR_PUBLIC_IP değiştirin)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME/providers/microsoft.resources/deployments/DEPLOYMENT_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "properties": {
    "templateLink": {
      "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json",
      "contentVersion": "1.0.0.0",
    },
    "mode": "Incremental",
    "parameters": {
        "newStorageAccountName": {
          "value": "GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME"
        },
        "adminUsername": {
          "value": "ADMIN_USER_NAME"
        },
        "adminPassword": {
          "value": "ADMIN_PASSWORD"
        },
        "dnsNameForPublicIP": {
          "value": "DNS_NAME_FOR_PUBLIC_IP"
        },
        "ubuntuOSVersion": {
          "value": "15.04"
        }
    }
  }
}
```

Bu belgelerin okunabilirliğini artırmak için bu istek için uzun JSON yanıt çıkarıldı. Yanıt, oluşturduğunuz şablonlu dağıtımı hakkında bilgi içerir.

## <a name="next-steps"></a>Sonraki adımlar

- Zaman uyumsuz REST işlemlerini işleme hakkında bilgi edinmek için [izlemek zaman uyumsuz Azure işlemleri](resource-manager-async-operations.md).
