---
title: "Azure API Management etki alanları arası ilkeler | Microsoft Docs"
description: "Azure API Management'te kullanıma etki alanları arası ilkeler hakkında bilgi edinin."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 7689d277-8abe-472a-a78c-e6d4bd43455d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 05b25ffad4a91859932cd53475d82b11bf3e43e5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="api-management-cross-domain-policies"></a>API Management etki alanları arası ilkeler
Bu konu aşağıdaki API Management ilkeleri bir başvuru sağlar. Ekleme ve ilkeleri yapılandırma hakkında daha fazla bilgi için bkz: [API Management ilkeleri](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="CrossDomainPolicies"></a>Etki alanı ilkelerini arası  
  
-   [Etki alanları arası çağrılar izin](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -API Adobe Flash ve Microsoft Silverlight tarayıcı tabanlı istemcilerden erişilebilir hale getirir.  
  
-   [CORS](api-management-cross-domain-policies.md#CORS) -çıkış noktaları arası kaynak paylaşımı (CORS) desteklemek için bir işlem veya tarayıcı tabanlı istemcilerden etki alanları arası çağrılarına izin vermek için bir API ekler.  
  
-   [JSONP](api-management-cross-domain-policies.md#JSONP) -bir işlem veya tarayıcı tabanlı JavaScript istemcilerden etki alanları arası çağrılarına izin vermek için bir API (JSONP) doldurma desteğiyle JSON ekler.  
  
##  <a name="AllowCrossDomainCalls"></a>Etki alanları arası çağrılar izin ver  
 Kullanım `cross-domain` API Adobe Flash ve Microsoft Silverlight tarayıcı tabanlı istemcilerden erişilebilir olması için ilke.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<cross-domain>  
   <!-Policy configuration is in the Adobe cross-domain policy file format,   
      see http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html-->  
</cross-domain>  
```  
  
### <a name="example"></a>Örnek  
  
```xml  
<cross-domain>  
    <cross-domain-policy>  
        <allow-http-request-headers-from domain='*' headers='*' />  
    </cross-domain-policy>  
</cross-domain>  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Ad|Açıklama|Gerekli|  
|----------|-----------------|--------------|  
|etki alanları arası|Kök öğesi. Alt öğeler için uygun olmalıdır [Adobe etki alanları arası ilke dosya belirtimi](http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html).|Evet|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** gelen  
  
-   **İlke kapsamları:** genel  
  
##  <a name="CORS"></a>CORS  
 `cors` İlkesi çıkış noktaları arası kaynak paylaşımı (CORS) desteklemek için bir işlem veya tarayıcı tabanlı istemcilerden etki alanları arası çağrılarına izin vermek için bir API ekler.  
  
 CORS bir tarayıcı ile etkileşim kurmanızı ve belirli cross-origin istekleri (JavaScript'ten bir web sayfasında diğer etki alanlarına yapılan yani XMLHttpRequests çağrıları) izin verilip verilmeyeceğini belirlemek için sunucusu sağlar. Bu, yalnızca kaynak aynı isteklerine izin'den daha fazla esneklik sağlar, ancak tüm çıkış noktaları arası istekleri izin vererek değerinden daha güvenlidir.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<cors allow-credentials="false|true">  
    <allowed-origins>  
        <origin>origin uri</origin>  
    </allowed-origins>  
    <allowed-methods preflight-result-max-age="number of seconds">  
        <method>http verb</method>  
    </allowed-methods>  
    <allowed-headers>  
        <header>header name</header>  
    </allowed-headers>  
    <expose-headers>  
        <header>header name</header>  
    </expose-headers>  
</cors>  
```  
  
### <a name="example"></a>Örnek  
 Bu örnek özel üstbilgi veya dışında GET ve POST yöntemleri olanlar gibi ön uçuş isteklerini destekleyecek şekilde nasıl gösterir. Özel üstbilgi ve ek HTTP fiilleri desteklemek için kullanma `allowed-methods` ve `allowed-headers` bölümler aşağıdaki örnekte gösterildiği gibi.  
  
```xml  
<cors allow-credentials="true">  
    <allowed-origins>  
        <!-- Localhost useful for development -->  
        <origin>http://localhost:8080/</origin>  
        <origin>http://example.com/</origin>  
    </allowed-origins>  
    <allowed-methods preflight-result-max-age="300">  
        <method>GET</method>  
        <method>POST</method>  
        <method>PATCH</method>  
        <method>DELETE</method>  
    </allowed-methods>  
    <allowed-headers>  
        <!-- Examples below show Azure Mobile Services headers -->  
        <header>x-zumo-installation-id</header>  
        <header>x-zumo-application</header>  
        <header>x-zumo-version</header>  
        <header>x-zumo-auth</header>  
        <header>content-type</header>  
        <header>accept</header>  
    </allowed-headers>  
    <expose-headers>  
        <!-- Examples below show Azure Mobile Services headers -->  
        <header>x-zumo-installation-id</header>  
        <header>x-zumo-application</header>  
    </expose-headers>  
</cors>  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Ad|Açıklama|Gerekli|Varsayılan|  
|----------|-----------------|--------------|-------------|  
|cors|Kök öğesi.|Evet|Yok|  
|izin verilen çıkış|İçeren `origin` etki alanları arası istekleri için izin verilen çıkış noktası açıklayan öğeler. `allowed-origins`ya da tek bir içerebilir `origin` belirtir öğesi `*` her türlü kaynağa veya bir veya daha fazla izin vermek için `origin` bir URI içeren öğeler.|Evet|Yok|  
|Kaynak|Değer, ya da olabilir `*` tüm kaynaklara ya da tek bir kaynak belirten bir URI izin vermek için. URI şeması, ana bilgisayar ve bağlantı noktası eklemeniz gerekir.|Evet|Bağlantı noktası bir URI atlanırsa, HTTP için 80 numaralı bağlantı noktası kullanılır ve HTTPS için 443 numaralı bağlantı noktası kullanılır.|  
|izin verilen yöntemleri|Yöntemleri dışında GET veya POST izin verilir, bu öğe gereklidir. İçeren `method` desteklenen HTTP fiilleri öğeleri.|Hayır|Bu bölümde mevcut değilse, GET ve POST desteklenir.|  
|Yöntemi|Bir HTTP fiili belirtir.|En az bir `method` öğesi, varsa gereklidir `allowed-methods` bölüm mevcut.|Yok|  
|izin verilen üstbilgileri|Bu öğeyi içeren `header` öğeleri isteğine dahil başlıklarının adlarını belirtme.|Hayır|Yok|  
|sunmaya üstbilgileri|Bu öğeyi içeren `header` öğeleri istemci tarafından erişilebilecek başlıklarının adlarını belirtme.|Hayır|Yok|  
|üst bilgi|Bir üstbilgi adı belirtir.|En az bir `header` öğesi gerekli `allowed-headers` veya `expose-headers` bölüm mevcut değilse.|Yok|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Ad|Açıklama|Gerekli|Varsayılan|  
|----------|-----------------|--------------|-------------|  
|izin ver-kimlik bilgileri|`Access-Control-Allow-Credentials` Ön yanıt üstbilgisi bu özniteliğin değerini ayarlayın ve istemci kimlik bilgileri etki alanları arası istek gönderme olanağı etkiler.|Hayır|False|  
|Ön-sonuç-max-age|`Access-Control-Max-Age` Ön yanıt üstbilgisi bu özniteliğin değerini ayarlayın ve önbellek öncesi uçuş yanıtı kullanıcı aracısının yeteneği etkiler.|Hayır|0|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** gelen  
  
-   **İlke kapsamları:** API, işlemi  
  
##  <a name="JSONP"></a>JSONP  
 `jsonp` İlkesi bir işlem veya tarayıcı tabanlı JavaScript istemcilerden etki alanları arası çağrılarına izin vermek için bir API (JSONP) doldurma desteğiyle JSON ekler. JSONP JavaScript programlarda farklı bir etki alanındaki bir sunucudan istek verileri için kullanılan bir yöntemdir. JSONP burada web sayfalarına erişimi aynı etki alanında olmalıdır çoğu web tarayıcısı tarafından uygulanan sınırlama atlar.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<jsonp callback-parameter-name="callback function name" />  
```  
  
### <a name="example"></a>Örnek  
  
```xml  
<jsonp callback-parameter-name="cb" />  
```  
  
 Geri çağırma parametresi olmadan yöntemini çağırırsanız? cb = XXX (olmadan işlev çağrısı sarmalayıcı) düz JSON döndürecektir.  
  
 Geri çağırma parametresi eklerseniz `?cb=XXX` bir JSONP sonuç döndürür ve özgün JSON kaydırma geri çağırma işlevi geçici Sonuçlardan memnun`XYZ('<json result goes here>');`  
  
### <a name="elements"></a>Öğeleri  
  
|Ad|Açıklama|Gerekli|  
|----------|-----------------|--------------|  
|jsonp|Kök öğesi.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Ad|Açıklama|Gerekli|Varsayılan|  
|----------|-----------------|--------------|-------------|  
|geri çağırma parametre adı|İşlevin yer tam etki alanı adıyla önek etki alanları arası JavaScript işlevi çağrısı.|Evet|Yok|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** giden  
  
-   **İlke kapsamları:** genel, ürün, API işlemi  
  
## <a name="next-steps"></a>Sonraki adımlar
İlkeleriyle çalışma daha fazla bilgi için bkz: [API Management ilkeleri](api-management-howto-policies.md).  