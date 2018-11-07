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
ms.openlocfilehash: 4c4c03fffa5786bf3a50f4d2c03511f0a2de0f48
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51250960"
---
# <a name="api-management-authentication-policies"></a>API Management kimlik doğrulama ilkeleri
Bu konu aşağıdaki API Management ilkeleri bir başvuru sağlar. Ekleme ve ilkeleri yapılandırma hakkında daha fazla bilgi için bkz: [API Management ilkeleri](https://go.microsoft.com/fwlink/?LinkID=398186).  

##  <a name="AuthenticationPolicies"></a> Kimlik doğrulama ilkeleri  
  
-   [Temel kimlik doğrulaması](api-management-authentication-policies.md#Basic) -temel kimlik doğrulaması kullanarak arka uç hizmeti ile kimlik doğrulaması.  
  
-   [İstemci sertifikası ile kimlik doğrulaması](api-management-authentication-policies.md#ClientCertificate) -istemci sertifikalarını kullanan bir arka uç hizmeti ile kimlik doğrulaması.  
  
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
<authentication-certificate thumbprint="thumbprint" />  
```  
  
### <a name="example"></a>Örnek  
  
```xml  
<authentication-certificate thumbprint="....." />  
```  
  
### <a name="elements"></a>Öğeler  
  
|Ad|Açıklama|Gerekli|  
|----------|-----------------|--------------|  
|kimlik doğrulama sertifikası|Kök öğe.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Ad|Açıklama|Gerekli|Varsayılan|  
|----------|-----------------|--------------|-------------|  
|parmak izi|İstemci sertifikası için parmak izi.|Evet|Yok|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümler:** gelen  
  
-   **İlke kapsamları:** API  
  

## <a name="next-steps"></a>Sonraki adımlar
İlkeleriyle çalışma hakkında bilgi için bkz:

+ [API Management ilkeleri](api-management-howto-policies.md)
+ [API'leri dönüştürme](transform-api.md)
+ [İlke başvurusu](api-management-policy-reference.md) ilke bildirimlerine ve ayarlarının tam listesi için
+ [İlke örnekleri](policy-samples.md)   
