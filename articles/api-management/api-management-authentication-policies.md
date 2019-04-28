---
title: Azure API Management kimlik doğrulama ilkeleri | Microsoft Docs
description: Azure API Management'ta kullanılabilir kimlik doğrulama ilkeleri hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 061702a7-3a78-472b-a54a-f3b1e332490d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: c0f8da779ca656cf357c418b8766a53307643695
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63764347"
---
# <a name="api-management-authentication-policies"></a>API Management kimlik doğrulama ilkeleri
Bu konu aşağıdaki API Management ilkeleri bir başvuru sağlar. Ekleme ve ilkeleri yapılandırma hakkında daha fazla bilgi için bkz: [API Management ilkeleri](https://go.microsoft.com/fwlink/?LinkID=398186).  

##  <a name="AuthenticationPolicies"></a> Kimlik doğrulama ilkeleri  
  
-   [Temel kimlik doğrulaması](api-management-authentication-policies.md#Basic) -temel kimlik doğrulaması kullanarak arka uç hizmeti ile kimlik doğrulaması.  
  
-   [İstemci sertifikası ile kimlik doğrulaması](api-management-authentication-policies.md#ClientCertificate) -istemci sertifikalarını kullanan bir arka uç hizmeti ile kimlik doğrulaması.  

-   [Yönetilen kimliği ile kimlik doğrulaması](api-management-authentication-policies.md#ManagedIdentity) -kimlik doğrulaması [yönetilen kimliği](../active-directory/managed-identities-azure-resources/overview.md) API Management hizmeti için.  
  
##  <a name="Basic"></a> Temel kimlik doğrulaması  
 Kullanım `authentication-basic` temel kimlik doğrulaması kullanarak arka uç hizmeti ile kimlik doğrulaması ilkesi. Bu ilke etkili bir şekilde HTTP yetkilendirme üst bilgisi ilkede sağlanan kimlik bilgileri için karşılık gelen değere ayarlar.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<authentication-basic username="username" password="password" />  
```  
  
### <a name="example"></a>Örnek  
  
```xml  
<authentication-basic username="testuser" password="testpassword" />  
```  
  
### <a name="elements"></a>Öğeler  
  
|Ad|Açıklama|Gerekli|  
|----------|-----------------|--------------|  
|Temel kimlik doğrulaması|Kök öğe.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Ad|Açıklama|Gerekli|Varsayılan|  
|----------|-----------------|--------------|-------------|  
|kullanıcı adı|Temel kimlik bilgisinin kullanıcı adını belirtir.|Evet|Yok|  
|password|Temel kimlik bilgisinin parolasını belirtir.|Evet|Yok|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümler:** gelen  
  
-   **İlke kapsamları:** API  
  
##  <a name="ClientCertificate"></a> İstemci sertifikası ile kimlik doğrulaması  
 Kullanım `authentication-certificate` istemci sertifikasını kullandığı bir arka uç hizmeti ile kimlik doğrulaması ilkesi. Sertifika olması gereken [API yönetime yüklü](https://go.microsoft.com/fwlink/?LinkID=511599) ilk ve parmak izi tarafından belirlenir.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<authentication-certificate thumbprint="thumbprint" certificate-id="resource name"/>  
```  
  
### <a name="examples"></a>Örnekler  
  
Bu örnekte istemci sertifikası parmak izi tarafından tanımlanır.
```xml  
<authentication-certificate thumbprint="CA06F56B258B7A0D4F2B05470939478651151984" />  
``` 
Bu örnekte istemci sertifikası, kaynak adına göre tanımlanır.
```xml  
<authentication-certificate certificate-id="544fe9ddf3b8f30fb490d90f" />  
```  

### <a name="elements"></a>Öğeler  
  
|Ad|Açıklama|Gerekli|  
|----------|-----------------|--------------|  
|kimlik doğrulama sertifikası|Kök öğe.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Ad|Açıklama|Gerekli|Varsayılan|  
|----------|-----------------|--------------|-------------|  
|thumbprint|İstemci sertifikası için parmak izi.|Ya da `thumbprint` veya `certificate-id` mevcut olması gerekir.|Yok|  
|Sertifika kimliği|Sertifika kaynak adı.|Ya da `thumbprint` veya `certificate-id` mevcut olması gerekir.|Yok|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümler:** gelen  
  
-   **İlke kapsamları:** API  

##  <a name="ManagedIdentity"></a> Yönetilen kimliği ile kimlik doğrulaması  
 Kullanım `authentication-managed-identity` API Management hizmetinin yönetilen kimlik kullanarak bir arka uç hizmeti ile kimlik doğrulaması ilkesi. Bu ilke yönetilen kimlik belirtilen kaynağa erişim sağlamak için Azure Active Directory'den bir erişim belirteci almak için etkili bir şekilde kullanır. 
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<authentication-managed-identity resource="resource" output-token-variable-name="token-variable" ignore-error="true|false"/>  
```  
  
### <a name="example"></a>Örnek  
  
```xml  
<authentication-managed-identity resource="https://graph.windows.net" output-token-variable-name="test-access-token" ignore-error="true" /> 
```  
  
### <a name="elements"></a>Öğeler  
  
|Ad|Açıklama|Gerekli|  
|----------|-----------------|--------------|  
|kimlik doğrulaması yönetilen kimlik |Kök öğe.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Ad|Açıklama|Gerekli|Varsayılan|  
|----------|-----------------|--------------|-------------|  
|kaynak|dize. Hedef web API'sine (güvenli kaynak) Azure Active Directory Uygulama Kimliği URI'si.|Evet|Yok|  
|Çıkış belirteci değişken adı|dize. Bir nesne türü olarak belirteç değeri alacak bağlam değişkeninin adı `string`.|Hayır|Yok|  
|Hatayı Yoksay|Boole değeri. Varsa kümesine `true`, ilke işlem hattı bir erişim belirteci değil elde edilir olsa bile yürütülmeye devam eder.|Hayır|false|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümler:** gelen  
  
-   **İlke kapsamları:** genel, ürün, API, işlemi  

## <a name="next-steps"></a>Sonraki adımlar
İlkeleriyle çalışma hakkında bilgi için bkz:

+ [API Management ilkeleri](api-management-howto-policies.md)
+ [API'leri dönüştürme](transform-api.md)
+ [İlke başvurusu](api-management-policy-reference.md) ilke bildirimlerine ve ayarlarının tam listesi için
+ [İlke örnekleri](policy-samples.md)   
