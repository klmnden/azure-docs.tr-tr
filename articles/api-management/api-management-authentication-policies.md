---
title: "Azure API Management kimlik doğrulama ilkeleri | Microsoft Docs"
description: "Azure API Management'te kullanım için uygun kimlik doğrulama ilkeleri hakkında bilgi edinin."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 061702a7-3a78-472b-a54a-f3b1e332490d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: 4d13d9dbea9da9db5bfe9a9af85fdbf9eab1ae84
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="api-management-authentication-policies"></a>API Management kimlik doğrulama ilkeleri
Bu konu aşağıdaki API Management ilkeleri bir başvuru sağlar. Ekleme ve ilkeleri yapılandırma hakkında daha fazla bilgi için bkz: [API Management ilkeleri](http://go.microsoft.com/fwlink/?LinkID=398186).  

##  <a name="AuthenticationPolicies"></a>Kimlik doğrulama ilkeleri  
  
-   [Basic ile kimlik doğrulaması](api-management-authentication-policies.md#Basic) -temel kimlik doğrulaması kullanarak arka uç hizmeti ile kimlik doğrulaması.  
  
-   [İstemci sertifikası ile kimlik doğrulaması](api-management-authentication-policies.md#ClientCertificate) -istemci sertifikalarını kullanan bir arka uç hizmeti ile kimlik doğrulaması.  
  
##  <a name="Basic"></a>Basic ile kimlik doğrulaması  
 Kullanım `authentication-basic` temel kimlik doğrulaması kullanarak arka uç hizmeti ile kimlik doğrulaması ilkesi. Bu ilke HTTP Authorization Üstbilgisi ilkede sağlanan kimlik bilgileri karşılık gelen değer için etkili bir şekilde ayarlar.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<authentication-basic username="username" password="password" />  
```  
  
### <a name="example"></a>Örnek  
  
```xml  
<authentication-basic username="testuser" password="testpassword" />  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Ad|Açıklama|Gerekli|  
|----------|-----------------|--------------|  
|kimlik doğrulama-temel|Kök öğesi.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Ad|Açıklama|Gerekli|Varsayılan|  
|----------|-----------------|--------------|-------------|  
|kullanıcı adı|Temel kimlik bilgileri kullanıcı adı belirtir.|Evet|Yok|  
|password|Temel kimlik bilgisinin parolasını belirtir.|Evet|Yok|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** gelen  
  
-   **İlke kapsamları:** API  
  
##  <a name="ClientCertificate"></a>İstemci sertifikası ile kimlik doğrulaması  
 Kullanım `authentication-certificate` istemci sertifikasını kullanarak bir arka uç hizmeti ile kimlik doğrulaması ilkesi. Sertifika olması gereken [API yönetime yüklü](http://go.microsoft.com/fwlink/?LinkID=511599) ilk ve kendi parmak izi tarafından tanımlanır.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<authentication-certificate thumbprint="thumbprint" />  
```  
  
### <a name="example"></a>Örnek  
  
```xml  
<authentication-certificate thumbprint="....." />  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Ad|Açıklama|Gerekli|  
|----------|-----------------|--------------|  
|kimlik doğrulama sertifikası|Kök öğesi.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Ad|Açıklama|Gerekli|Varsayılan|  
|----------|-----------------|--------------|-------------|  
|parmak izi|İstemci sertifikasının parmak izi.|Evet|Yok|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** gelen  
  
-   **İlke kapsamları:** API  
  

## <a name="next-steps"></a>Sonraki adımlar
İlkeleriyle çalışma daha fazla bilgi için bkz:

+ [API Management ilkeleri](api-management-howto-policies.md)
+ [API dönüştürme](transform-api.md)
+ [Grup İlkesi başvurusu](api-management-policy-reference.md) ilke deyimleri ve ayarlarının tam listesi için
+ [İlke örnekleri](policy-samples.md)   
