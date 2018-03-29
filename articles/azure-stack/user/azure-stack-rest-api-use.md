---
title: Azure yığını API'si kullanın | Microsoft Docs
description: Azure API istekleri için Azure yığın yapmak için bir kimlik doğrulaması almak öğrenin.
services: azure-stack
documentationcenter: ''
author: cblackuk
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2018
ms.author: mabrigg
ms.reviewer: sijuman
ms.openlocfilehash: 3523f86791a3bf437cbd21ba4df5afc0cd1955fd
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
<!--  cblackuk and charliejllewellyn -->

# <a name="use-the-azure-stack-api"></a>Azure yığını API'si kullanın

Azure yığını API'si Market öğesi syndicating gibi işlemleri otomatikleştirmek için kullanabilirsiniz.

İşlem gerektiren, istemci kimlik doğrulamaları Microsoft Azure oturum açma uç noktası. Uç nokta bir belirteci döndürür. Azure yığın API'sine gönderilen her istek üstbilgisi belirteçte sağlar. Azure Oauth2.0 kullanır.

Bu makalede, curl yardımcı programını kullanarak, Azure yığınına belirli basit örnekler sağlar. Bu konu Azure yığını API'si erişmek için bir belirteç alma işleminde size kılavuzluk eder. Çoğu diller, bu tür belirtecini yenilemeyi sağlam belirteci yönetim ve görevleri işleme sahip Oauth 2.0 kitaplıkları sağlar.

Genel bir REST istemcisiyle Azure yığın REST API'sini kullanarak tüm işleminden bakarak curl gibi temel alınan istek ve yanıt yükünde almayı bekleyebileceğiniz anlamanıza yardımcı olabilir.

Bu makalede ayrıca belirteçleri etkileşimli oturum açma gibi alma veya özel uygulama kimlikleri oluşturma için kullanabileceğiniz tüm seçenekler keşfedin değil. Daha fazla bilgi için bkz: [Azure REST API Başvurusu](https://docs.microsoft.com/rest/api/).

## <a name="get-a-token-from-azure"></a>Azure'dan belirteç alın

Bir isteği oluşturma *gövde* içerik türü x-www-form-urlencoded bir erişim belirteci almak için kullanılarak biçimlendirilmiş. Azure REST kimlik doğrulama ve oturum açma uç noktası isteğinizi gönderin.
```
POST https://login.microsoftonline.com/{tenant id}/oauth2/token
```

**Kiracı kimliği** da:

- Fabrikam.onmicrosoft.com gibi Kiracı etki alanı
- 8eaed023-2b34-4da1-9baa-8bc8c9d6a491 gibi Kiracı Kimliğinizi
- Varsayılan değer bağımsız Kiracı anahtarları için: Genel

### <a name="post-body"></a>Posta gövdesi
```
grant_type=password
&client_id=1950a258-227b-4e31-a9cf-717495945fc2
&resource=https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155
&username=admin@fabrikam.onmicrosoft.com
&password=Password123
&scope=openid
```

Her bir değer için:

  **grant_type**
  
  Kimlik doğrulama düzeni türünü kullanarak olur. Bu örnekte değeridir:
  ```
  password
  ```

  **Kaynak**

  Kaynak belirteci erişir. Azure yığın yönetim meta veri uç noktasının sorgulayarak kaynak bulabilirsiniz. Bakmak **izleyicileri** bölümü

  Azure yığın yönetim uç noktası:
  ```
  https://management.{region}.{Azure Stack domain}/metadata/endpoints?api-version=2015-01-01
  ```
 > [!NOTE]  
 > Kiracı API'si erişmeye bir yönetici ardından Kiracı uç noktası, örneğin kullandığınızdan emin olmanız gerekir 'https://adminmanagement.{region}.{Azure yığın etki alanı} / meta veri/uç noktaları? api-version = 2015-01-011
  
  Örneğin, Azure yığın Geliştirme Seti:
  ```
  curl 'https://management.local.azurestack.external/metadata/endpoints?api-version=2015-01-01'
  ```
 
  Yanıtı:
  ```
  {
  "galleryEndpoint":"https://adminportal.local.azurestack.external:30015/",
  "graphEndpoint":"https://graph.windows.net/",
  "portalEndpoint":"https://adminportal.local.azurestack.external/",
  "authentication":{
      "loginEndpoint":"https://login.windows.net/",
      "audiences":["https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155"]
      }
  }
  ```

### <a name="example"></a>Örnek
  ```
  https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155
  ```

  **client_id**

  Bu değer, sabit kodlanmış bir varsayılan değere olur:
  ```
  1950a258-227b-4e31-a9cf-717495945fc2
  ```

  Belirli senaryolar için diğer seçenekleri kullanılabilir:
  
  | Uygulama | ApplicationId |
  | --------------------------------------- |:-------------------------------------------------------------:|
  | LegacyPowerShell | 0a7bdc5c-7b57-40be-9939-d4c5fc7cd417 |
  | PowerShell | 1950a258-227b-4e31-a9cf-717495945fc2 |
  | WindowsAzureActiveDirectory | 00000002-0000-0000-c000-000000000000 |
  | VisualStudio | 872cd9fa-d31f-45e0-9eab-6e460a02d1f1 |
  | AzureCLI | 04b07795-8ddb-461a-bbee-02f9e1bf7b46 |

  **Kullanıcı adı**
  
  Azure AAD yığın hesap, örneğin:
  ```
  azurestackadmin@fabrikam.onmicrosoft.com
  ```

  **Parola**

  Azure yığın AAD yönetici parolası.
  
### <a name="example"></a>Örnek

İsteği:
```
curl -X "POST" "https://login.windows.net/fabrikam.onmicrosoft.com/oauth2/token" \
-H "Content-Type: application/x-www-form-urlencoded" \
--data-urlencode "client_id=1950a258-227b-4e31-a9cf-717495945fc2" \
--data-urlencode "grant_type=password" \
--data-urlencode "username=admin@fabrikam.onmicrosoft.com" \
--data-urlencode 'password=Password12345' \
--data-urlencode "resource=https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155"
```

Yanıtı:
```
{
  "token_type": "Bearer",
  "scope": "user_impersonation",
  "expires_in": "3599",
  "ext_expires_in": "0",
  "expires_on": "1512574780",
  "not_before": "1512570880",
  "resource": "https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155",
  "access_token": "eyJ0eXAiOi...truncated for readability..."
}
```

## <a name="api-queries"></a>API'si sorguları

Erişim belirteci edindikten sonra her API isteklerinizin başlık olarak eklemek gerekir. Bunu yapmak için bir başlık oluşturma gerekir **yetkilendirme** değerine sahip: `Bearer <access token>`. Örneğin:

İsteği:
```
curl -H "Authorization: Bearer eyJ0eXAiOi...truncated for readability..." 'https://adminmanagement.local.azurestack.external/subscriptions?api-version=2016-05-01'
```

Yanıtı:
```
offerId : /delegatedProviders/default/offers/92F30E5D-F163-4C58-8F02-F31CFE66C21B
id : /subscriptions/800c4168-3eb1-406b-a4ca-919fe7ee42e8
subscriptionId : 800c4168-3eb1-406b-a4ca-919fe7ee42e8
tenantId : 9fea4606-7c07-4518-9f3f-8de9c52ab628
displayName : Default Provider Subscription
state : Enabled
subscriptionPolicies : @{locationPlacementId=AzureStack}
```

### <a name="url-structure-and-query-syntax"></a>URL yapısı ve sorgu sözdizimi

Genel istek URI oluşur: {URI şeması} :// {URI-host} / {kaynak-path}? {Sorgu dizesi}

- **URI şeması**:  
URI isteği göndermek için kullanılan protokol gösterir. Örneğin, `http` veya `https`.
- **URI ana bilgisayarı**:  
Ana bilgisayar etki alanı adı ya da REST Hizmeti uç noktası barındırıldığı, gibi sunucusunun IP adresini belirtir `graph.microsoft.com` veya `adminmanagement.local.azurestack.external`.
- **Kaynak yolu**:  
Kaynak veya birden çok parçalı kaynaklarla seçimini belirlemede hizmeti tarafından kullanılan içerebilir kaynak koleksiyonu yolunu belirtir. Örneğin: `beta/applications/00003f25-7e1f-4278-9488-efc7bac53c4a/owners` uygulamaları koleksiyonundaki belirli bir uygulamanın sahipler listesini sorgulamak için kullanılır.
- **Sorgu dizesi**:  
Dize API sürümü veya kaynak seçim ölçütü gibi ek basit parametreleri sağlar.

## <a name="azure-stack-request-uri-construct"></a>Azure yığın istek URI yapısı
```
{URI-scheme} :// {URI-host} / {subscription id} / {resource group} / {provider} / {resource-path} ? {OPTIONAL: filter-expression} {MANDATORY: api-version} 
```

### <a name="uri-syntax"></a>URI sözdizimi
```
https://adminmanagement.local.azurestack.external/{subscription id}/resourcegroups/{resource group}/providers/{provider}/{resource-path}?{api-version} 
```

### <a name="query-uri-example"></a>Sorgu URI örneği
```
https://adminmanagement.local.azurestack.external/subscriptions/800c4168-3eb1-406b-a4ca-919fe7ee42e8/resourcegroups/system.local/providers/microsoft.infrastructureinsights.admin/regionhealths/local/Alerts?$filter=(Properties/State eq 'Active') and (Properties/Severity eq 'Critical')&$orderby=Properties/CreatedTimestamp desc&api-version=2016-05-01"
```

## <a name="next-steps"></a>Sonraki adımlar

Azure RESTful uç noktaları kullanma hakkında daha fazla bilgi için bkz: [Azure REST API Başvurusu](https://docs.microsoft.com/rest/api/).
