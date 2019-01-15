---
title: Azure Stack API kullanma | Microsoft Docs
description: Azure Stack için API isteği yapmak için Azure kimlik doğrulaması almak öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/14/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: fe516d1d34496d190ae45e00893deb646fc08408
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2019
ms.locfileid: "54306565"
---
<!--  cblackuk and charliejllewellyn. This is a community contribution by cblackuk-->

# <a name="use-the-azure-stack-api"></a>Azure Stack API kullanın

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Uygulama programlama arabirimi (API), Azure Stack bulutunuza bir VM ekleme gibi işlemleri otomatikleştirmek için kullanabilirsiniz.

API, Microsoft Azure oturum açma uç noktası için kimlik doğrulaması yapmak istemcinizi gerektirir. Uç nokta, Azure Stack API'ye gönderilen her istek üstbilgisindeki kullanmak için bir belirteç döndürür. Microsoft Azure, Oauth 2.0 kullanır.

Bu makalede kullanan örnekler **cURL** Azure Stack istekleri oluşturmak için yardımcı program. CURL, uygulama, veri aktarmak için bir kitaplığı ile bir komut satırı aracıdır. Bu örnekler, Azure Stack API'sine erişmek için bir belirteç alma işleminde size kılavuzluk. Çoğu programlama dilinden böyle belirteç yenileme belirteci sağlam yönetim ve tutamacı görevleri olan Oauth 2.0 kitaplıkları sağlar.

Azure Stack REST API'si gibi genel bir REST istemcisi ile kullanarak tüm işlemi gözden **cURL**, arka plandaki istek ve yanıt yükünde almayı bekleyebilirsiniz gösterir anlamanıza yardımcı olacak.

Bu makalede, etkileşimli oturum açma gibi belirteçleri almak veya ayrılmış uygulama kimlikleri oluşturma için kullanılabilir tüm seçenekleri keşfedin değil. Bu konular hakkında bilgi edinmek için [Azure REST API Başvurusu](https://docs.microsoft.com/rest/api/).

## <a name="get-a-token-from-azure"></a>Azure'dan bir belirteç Al

İçerik türü x-www-form-urlencoded ile biten bir erişim belirteci almak için kullanmak biçimlendirilmiş istek gövdesi oluşturun. Azure REST kimlik doğrulama ve oturum açma uç noktası isteğinizi gönderin.

### <a name="uri"></a>URI

```bash  
POST https://login.microsoftonline.com/{tenant id}/oauth2/token
```

**Kiracı kimliği** ya da:

 - Kiracı etki alanınızın gibi `fabrikam.onmicrosoft.com`
 - Örneğin, Kiracı kimliği `8eaed023-2b34-4da1-9baa-8bc8c9d6a491`
 - Bağımsız Kiracı anahtarları için varsayılan değer: `common`

### <a name="post-body"></a>POST gövdesini

```bash  
grant_type=password
&client_id=1950a258-227b-4e31-a9cf-717495945fc2
&resource=https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155
&username=admin@fabrikam.onmicrosoft.com
&password=Password123
&scope=openid
```

Her bir değer için:

 - **grant_type değeri**  
    Kimlik doğrulama düzeni türünü kullanarak yapar. Bu örnekte, değerdir `password`

 - **Kaynak**  
    Kaynak belirteci erişir. Azure Stack yönetim meta veri uç noktasını sorgulayarak kaynak bulabilirsiniz. Bakmak **izleyiciler** bölümü

 - **Azure Stack yönetim uç noktası**  
    ```
    https://management.{region}.{Azure Stack domain}/metadata/endpoints?api-version=2015-01-01
    ```

  > [!NOTE]  
  > Kiracı API'si erişmeye bir yöneticiyseniz, daha sonra Kiracı uç noktası, örneğin kullanılacak emin olmanız gerekir: `https://adminmanagement.{region}.{Azure Stack domain}/metadata/endpoints?api-version=2015-01-011`  

  Örneğin, Azure Stack geliştirme Seti'ni bir uç nokta olarak:

    ```bash
    curl 'https://management.local.azurestack.external/metadata/endpoints?api-version=2015-01-01'
    ```

  Yanıt:

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

  Bu değeri, sabit kodlanmış bir varsayılan değer şudur:

  ```
  1950a258-227b-4e31-a9cf-717495945fc2
  ```

  Belirli senaryolar için diğer seçenekler kullanılabilir:

  
  | Uygulama | ApplicationId |
  | --------------------------------------- |:-------------------------------------------------------------:|
  | LegacyPowerShell | 0a7bdc5c-7b57-40be-9939-d4c5fc7cd417 |
  | PowerShell | 1950a258-227b-4e31-a9cf-717495945fc2 |
  | WindowsAzureActiveDirectory | 00000002-0000-0000-c000-000000000000 |
  | VisualStudio | 872cd9fa-d31f-45e0-9eab-6e460a02d1f1 |
  | AzureCLI | 04b07795-8ddb-461a-bbee-02f9e1bf7b46 |

  **Kullanıcı adı**

  Örneğin, Azure Stack AAD hesabı:

  ```
  azurestackadmin@fabrikam.onmicrosoft.com
  ```

  **Parola**

  Azure Stack AAD yönetici parolası.

### <a name="example"></a>Örnek

İstek:

```
curl -X "POST" "https://login.windows.net/fabrikam.onmicrosoft.com/oauth2/token" \
-H "Content-Type: application/x-www-form-urlencoded" \
--data-urlencode "client_id=1950a258-227b-4e31-a9cf-717495945fc2" \
--data-urlencode "grant_type=password" \
--data-urlencode "username=admin@fabrikam.onmicrosoft.com" \
--data-urlencode 'password=Password12345' \
--data-urlencode "resource=https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155"
```

Yanıt:

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

## <a name="api-queries"></a>API sorguları

Erişim belirtecinizi aldıktan sonra üst bilgi olarak bir her API isteklerinizin eklemeniz gerekir. Bunu yapmak için bir başlık oluşturmanız gerekir **yetkilendirme** değerle: `Bearer <access token>`. Örneğin:

İstek:

```bash  
curl -H "Authorization: Bearer eyJ0eXAiOi...truncated for readability..." 'https://adminmanagement.local.azurestack.external/subscriptions?api-version=2016-05-01'
```

Yanıt:

```bash  
offerId : /delegatedProviders/default/offers/92F30E5D-F163-4C58-8F02-F31CFE66C21B
id : /subscriptions/800c4168-3eb1-406b-a4ca-919fe7ee42e8
subscriptionId : 800c4168-3eb1-406b-a4ca-919fe7ee42e8
tenantId : 9fea4606-7c07-4518-9f3f-8de9c52ab628
displayName : Default Provider Subscription
state : Enabled
subscriptionPolicies : @{locationPlacementId=AzureStack}
```

### <a name="url-structure-and-query-syntax"></a>URL yapısı ve sorgu söz dizimi

Genel istek URI oluşur: {URI scheme} :// {URI ana bilgisayarı} / {kaynak-path}? {Sorgu dizesi}

- **URI şeması**:  
URI isteği göndermek için kullanılan protokolü belirtir. Örneğin, `http` veya `https`.
- **URI ana bilgisayarı**:  
Ana bilgisayar etki alanı adı ya da REST Hizmeti uç noktası barındırıldığı, gibi sunucusunun IP adresini belirtir `graph.microsoft.com` veya `adminmanagement.local.azurestack.external`.
- **Kaynak yolu**:  
Yol kaynak veya hizmetin kaynak seçimine belirlemede tarafından kullanılan birden fazla bölüm içerebilen kaynak koleksiyonunu belirtir. Örneğin: `beta/applications/00003f25-7e1f-4278-9488-efc7bac53c4a/owners` uygulamaları koleksiyonundaki belirli bir uygulamanın sahipleri listesinde sorgulamak için kullanılabilir.
- **Sorgu dizesi**:  
Dize, API sürümü veya kaynak seçim ölçütü gibi basit ek parametreler sunar.

## <a name="azure-stack-request-uri-construct"></a>Azure Stack istek URI yapısı

```
{URI-scheme} :// {URI-host} / {subscription id} / {resource group} / {provider} / {resource-path} ? {OPTIONAL: filter-expression} {MANDATORY: api-version}
```

### <a name="uri-syntax"></a>URI söz dizimi

```
https://adminmanagement.local.azurestack.external/{subscription id}/resourcegroups/{resource group}/providers/{provider}/{resource-path}?{api-version}
```

### <a name="query-uri-example"></a>Sorgu URI örneği

```
https://adminmanagement.local.azurestack.external/subscriptions/800c4168-3eb1-406b-a4ca-919fe7ee42e8/resourcegroups/system.local/providers/microsoft.infrastructureinsights.admin/regionhealths/local/Alerts?$filter=(Properties/State eq 'Active') and (Properties/Severity eq 'Critical')&$orderby=Properties/CreatedTimestamp desc&api-version=2016-05-01"
```

## <a name="next-steps"></a>Sonraki adımlar

Azure RESTful uç noktaları kullanma hakkında daha fazla bilgi için bkz. [Azure REST API Başvurusu](https://docs.microsoft.com/rest/api/).
