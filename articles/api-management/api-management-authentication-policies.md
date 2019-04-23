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
ms.openlocfilehash: 9ee4a9fb5c63061eed32389b5672652aad01208a
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59994964"
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
  
|Name|Açıklama|Gereklidir|  
|----------|-----------------|--------------|  
|Temel kimlik doğrulaması|Kök öğe.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Name|Açıklama|Gereklidir|Varsayılan|  
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
<authentication-certificate thumbprint="thumbprint" />  
```  
  
### <a name="example"></a>Örnek  
  
```xml  
<authentication-certificate thumbprint="....." />  
```  
  
### <a name="elements"></a>Öğeler  
  
|Name|Açıklama|Gereklidir|  
|----------|-----------------|--------------|  
|kimlik doğrulama sertifikası|Kök öğe.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Name|Açıklama|Gereklidir|Varsayılan|  
|----------|-----------------|--------------|-------------|  
|thumbprint|İstemci sertifikası için parmak izi.|Evet|Yok|  
  
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
  
|Name|Açıklama|Gereklidir|  
|----------|-----------------|--------------|  
|kimlik doğrulaması yönetilen kimlik |Kök öğe.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Name|Açıklama|Gereklidir|Varsayılan|  
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
