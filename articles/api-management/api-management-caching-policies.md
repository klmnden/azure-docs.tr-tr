---
title: Azure API Management önbelleğe alma ilkeleri | Microsoft Docs
description: Azure API Management'te kullanıma önbelleğe alma ilkeleri hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 8147199c-24d8-439f-b2a9-da28a70a890c
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: 488a4c4b7daf5c07ca5f6b6bb72464279658d372
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
ms.locfileid: "26344831"
---
# <a name="api-management-caching-policies"></a>API Management önbelleğe alma ilkeleri
Bu konu aşağıdaki API Management ilkeleri bir başvuru sağlar. Ekleme ve ilkeleri yapılandırma hakkında daha fazla bilgi için bkz: [API Management ilkeleri](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="CachingPolicies"></a>Önbelleğe alma ilkeleri  
  
-   Yanıt önbelleğe alma ilkeleri  
  
    -   [Önbellekten alma](api-management-caching-policies.md#GetFromCache) -önbellek gerçekleştirmek aramak ve geçerli bir önbelleğe alınan yanıtları kullanılabilir olduğunda döndürür.  
    -   [Önbellek deposuna](api-management-caching-policies.md#StoreToCache) -belirtilen önbellek denetim yapılandırmasında göre yanıtlarını önbelleğe alır.  
  
-   Değer önbelleğe alma ilkeleri  

    -   [Önbelleğe alınan değer alma](#GetFromCacheByKey) -anahtar tarafından önbelleğe alınmış bir öğeyi geri almak. 
    -   [Değer önbellekte depolamak](#StoreToCacheByKey) -bir öğe anahtarı tarafından önbellekte depolamak. 
    -   [Önbelleğe alınan değer kaldırmak](#RemoveCacheByKey) -öğenin önbellekte anahtarının kaldırın.  
  
##  <a name="GetFromCache"></a>Önbellekten alma  
 Kullanım `cache-lookup` önbellek gerçekleştirilecek İlkesi aramak ve geçerli bir önbelleğe alınan yanıt kullanılabilir olduğunda döndürür. Bu ilke, yanıt içeriği bir süre boyunca statik kalır olduğu durumlarda uygulanabilir. Yanıt önbelleğe alma bant genişliğini azaltır ve işleme gereksinimlerini gecikmede API tüketiciler tarafından algılanan arka uç web sunucusu ve düşürmeye uygulanmaz.  
  
> [!NOTE]
>  Bu ilke karşılık gelen olmalıdır [önbellek deposuna](api-management-caching-policies.md#StoreToCache) ilkesi.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<cache-lookup vary-by-developer="true | false" vary-by-developer-groups="true | false" downstream-caching-type="none | private | public" must-revalidate="true | false" allow-private-response-caching="@(expression to evaluate)">  
  <vary-by-header>Accept</vary-by-header>  
  <!-- should be present in most cases -->  
  <vary-by-header>Accept-Charset</vary-by-header>  
  <!-- should be present in most cases -->  
  <vary-by-header>Authorization</vary-by-header>  
  <!-- should be present when allow-private-response-caching is "true"-->  
  <vary-by-header>header name</vary-by-header>  
  <!-- optional, can repeated several times -->  
  <vary-by-query-parameter>parameter name</vary-by-query-parameter>  
  <!-- optional, can repeated several times -->  
</cache-lookup>  
```  
  
### <a name="examples"></a>Örnekler  
  
#### <a name="example"></a>Örnek  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="none" must-revalidate="true">  
            <vary-by-query-parameter>version</vary-by-query-parameter>  
        </cache-lookup>           
    </inbound>  
    <outbound>  
        <cache-store duration="seconds" />  
        <base />          
    </outbound>  
</policies>  
```  
  
#### <a name="example-using-policy-expressions"></a>Örnek ilke ifadeleri kullanma  
 Bu örnek nasıl API Management yanıt önbelleğe alma yanıt arka uç hizmeti olarak önbelleğe almayı eşleşen süresi yapılandırmak için yedeklenmiş hizmetin tarafından belirtilen gösterir `Cache-Control` yönergesi. Yapılandırma ve bu ilkeyi kullanan bir örnek için bkz: [bulut kapak bölüm 177: daha Vlad Vinogradsky ile yönetim özelliklerini API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) ve 25:25 için ileri sarma.  
  
```xml  
<!-- The following cache policy snippets demonstrate how to control API Management reponse cache duration with Cache-Control headers sent by the backend service. -->  
  
<!-- Copy this snippet into the inbound section -->  
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >  
  <vary-by-header>Accept</vary-by-header>  
  <vary-by-header>Accept-Charset</vary-by-header>  
</cache-lookup>  
  
<!-- Copy this snippet into the outbound section. Note that cache duration is set to the max-age value provided in the Cache-Control header received from the backend service or to the deafult value of 5 min if none is found  -->  
<cache-store duration="@{  
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");  
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;  
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;  
  }"  
 />  
```  
  
 Daha fazla bilgi için bkz: [ilke ifadelerini](api-management-policy-expressions.md) ve [bağlamının](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Öğeleri  
  
|Ad|Açıklama|Gerekli|  
|----------|-----------------|--------------|  
|Önbellek araması|Kök öğesi.|Evet|  
|farklılık tarafından-üstbilgisi|Ana bilgisayardan kabul, Accept-Charset, Accept-Encoding, Accept-Language, yetkilendirme gibi Expect, belirtilen üstbilgi değerini başına yanıtlarını önbelleğe alma Başlat IF-Match.|Hayır|  
|farklılık-tarafından-query-parametresi|Her değeri belirtilen sorgu parametrelerinin yanıtlarını önbelleğe alma başlatın. Tek bir veya birden çok parametre girin. Ayırıcı olarak virgül kullanın. Hiçbiri belirtilmezse, tüm sorgu parametreleri kullanılır.|Hayır|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Ad|Açıklama|Gerekli|Varsayılan|  
|----------|-----------------|--------------|-------------|  
|izin ver-özel-yanıt-önbelleğe alma|Ayarlandığında `true`, bir Authorization üstbilgisi içeren istekleri önbelleğe alma sağlar.|Hayır|yanlış|  
|önbelleğe alma aşağı akış türü|Bu öznitelik aşağıdaki değerlerden birine ayarlanmalıdır.<br /><br /> -none - aşağı akış önbelleğe alma izin verilmez.<br />-Özel - aşağı akış özel önbelleğe alma izin verilir.<br />-Genel - özel ve paylaşılan aşağı akış önbelleğe alma izin verilir.|Hayır|yok|  
|gereken revalidate|Aşağı Akış önbelleği etkin olduğunda, bu öznitelik açar veya kapatır `must-revalidate` ağ geçidi yanıtlarındaki önbellek denetimi yönergesi.|Hayır|true|  
|farklılık-tarafından-Geliştirici|Kümesine `true` Geliştirici anahtar başına önbellek yanıtları.|Evet||  
|farklılık tarafından-Geliştirici-grupları|Kümesine `true` kullanıcı rol başına önbellek yanıtları.|Evet||  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** gelen  
-   **İlke kapsamları:** API işlemi, ürün  
  
##  <a name="StoreToCache"></a>Önbelleğine depolamak  
 `cache-store` İlkesi belirtilen önbellek ayarlara göre yanıtlarını önbelleğe alır. Bu ilke, yanıt içeriği bir süre boyunca statik kalır olduğu durumlarda uygulanabilir. Yanıt önbelleğe alma bant genişliğini azaltır ve işleme gereksinimlerini gecikmede API tüketiciler tarafından algılanan arka uç web sunucusu ve düşürmeye uygulanmaz.  
  
> [!NOTE]
>  Bu ilke karşılık gelen olmalıdır [önbellekten alma](api-management-caching-policies.md#GetFromCache) ilkesi.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<cache-store duration="seconds" />  
```  
  
### <a name="examples"></a>Örnekler  
  
#### <a name="example"></a>Örnek  
  
```xml  
<policies>  
    <inbound>  
        <base />  
          <cache-lookup vary-by-developer="true | false" vary-by-developer-groups="true | false" downstream-caching-type="none | private | public" must-revalidate="true | false">  
                <vary-by-query-parameter>parameter name</vary-by-query-parameter> <!-- optional, can repeated several times -->  
        </cache-lookup>  
    </inbound>  
    <outbound>  
        <base />  
            <cache-store duration="3600" />  
    </outbound>  
</policies>  
```  
  
#### <a name="example-using-policy-expressions"></a>Örnek ilke ifadeleri kullanma  
 Bu örnek nasıl API Management yanıt önbelleğe alma yanıt arka uç hizmeti olarak önbelleğe almayı eşleşen süresi yapılandırmak için yedeklenmiş hizmetin tarafından belirtilen gösterir `Cache-Control` yönergesi. Yapılandırma ve bu ilkeyi kullanan bir örnek için bkz: [bulut kapak bölüm 177: daha Vlad Vinogradsky ile yönetim özelliklerini API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) ve 25:25 için ileri sarma.  
  
```xml  
<!-- The following cache policy snippets demonstrate how to control API Management reponse cache duration with Cache-Control headers sent by the backend service. -->  
  
<!-- Copy this snippet into the inbound section -->  
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >  
  <vary-by-header>Accept</vary-by-header>  
  <vary-by-header>Accept-Charset</vary-by-header>  
</cache-lookup>  
  
<!-- Copy this snippet into the outbound section. Note that cache duration is set to the max-age value provided in the Cache-Control header received from the backend service or to the deafult value of 5 min if none is found  -->  
<cache-store duration="@{  
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");  
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;  
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;  
  }"  
 />  
```  
  
 Daha fazla bilgi için bkz: [ilke ifadelerini](api-management-policy-expressions.md) ve [bağlamının](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Öğeleri  
  
|Ad|Açıklama|Gerekli|  
|----------|-----------------|--------------|  
|Önbellek deposu|Kök öğesi.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Ad|Açıklama|Gerekli|Varsayılan|  
|----------|-----------------|--------------|-------------|  
|Süre|Zaman yaşam saniye cinsinden belirtilen önbelleğe alınmış girişlerinin.|Evet|Yok|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** giden    
-   **İlke kapsamları:** API işlemi, ürün  
  
##  <a name="GetFromCacheByKey"></a>Önbelleğe alınan değer alma  
 Kullanım `cache-lookup-value` İlkesi anahtarının önbellek araması gerçekleştirmek ve bir önbelleğe alınan değeri döndürür. Anahtar isteğe bağlı bir dize değerine sahip olabilir ve genellikle bir ilke ifade kullanılarak sağlanır.  
  
> [!NOTE]
>  Bu ilke karşılık gelen olmalıdır [değeri önbellekte depolamak](#StoreToCacheByKey) ilkesi.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<cache-lookup-value key="cache key value"   
    default-value="value to use if cache lookup resulted in a miss"   
    variable-name="name of a variable looked up value is assigned to" />  
```  
  
### <a name="example"></a>Örnek  
 Daha fazla bilgi ve bu ilkeyi örnekleri için bkz: [özel Azure API Management'te önbelleğe alma](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).  
  
```xml  
<cache-lookup-value  
    key="@("userprofile-" + context.Variables["enduserid"])"    
    variable-name="userprofile" />  
  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Ad|Açıklama|Gerekli|  
|----------|-----------------|--------------|  
|Önbellek arama değeri|Kök öğesi.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Ad|Açıklama|Gerekli|Varsayılan|  
|----------|-----------------|--------------|-------------|  
|Varsayılan değer|Önbellek anahtar arama değişken IF atanacak bir değer bir isabetsizliği sonuçlandı. Bu özniteliği belirtilmezse, `null` atanır.|Hayır|`null`|  
|anahtar|Aramada kullanmak için önbellek anahtar değeri.|Evet|Yok|  
|değişken adı|Adını [bağlamının](api-management-policy-expressions.md#ContextVariables) için arama başarılı ise yukarı looked değeri atanır. Bir isabetsizliği arama sonuçları, değişkenin değeri atanacak `default-value` özniteliği veya `null`, `default-value` özniteliği atlanmış.|Evet|Yok|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** gelen, giden arka uç, hata  
-   **İlke kapsamları:** Genel API, işlemi, ürünü  
  
##  <a name="StoreToCacheByKey"></a>Önbellek deposu değeri  
 `cache-store-value` Önbellek depolama anahtarının gerçekleştirir. Anahtar isteğe bağlı bir dize değerine sahip olabilir ve genellikle bir ilke ifade kullanılarak sağlanır.  
  
> [!NOTE]
>  Bu ilke karşılık gelen olmalıdır [değeri önbellekten alma](#GetFromCacheByKey) ilkesi.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<cache-store-value key="cache key value" value="value to cache" duration="seconds" />  
```  
  
### <a name="example"></a>Örnek  
 Daha fazla bilgi ve bu ilkeyi örnekleri için bkz: [özel Azure API Management'te önbelleğe alma](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).  
  
```xml  
<cache-store-value  
    key="@("userprofile-" + context.Variables["enduserid"])"  
    value="@((string)context.Variables["userprofile"])" duration="100000" />  
  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Ad|Açıklama|Gerekli|  
|----------|-----------------|--------------|  
|Önbellek deposu değeri|Kök öğesi.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Ad|Açıklama|Gerekli|Varsayılan|  
|----------|-----------------|--------------|-------------|  
|Süre|Saniye cinsinden belirtilen sağlanan süre değeri için değer önbelleğe alınır.|Evet|Yok|  
|anahtar|Önbellek anahtarı değeri altında depolanır.|Evet|Yok|  
|değer|Önbelleğe alınacak değer.|Evet|Yok|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** gelen, giden arka uç, hata  
-   **İlke kapsamları:** Genel API, işlemi, ürünü  
  
###  <a name="RemoveCacheByKey"></a>Önbelleğe alınan değer Kaldır  
`cache-remove-value` Anahtara göre tanımlanan önbelleğe alınmış bir öğeyi siler. Anahtar isteğe bağlı bir dize değerine sahip olabilir ve genellikle bir ilke ifade kullanılarak sağlanır.  
  
#### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
  
<cache-remove-value key="cache key value"/>  
  
```  
  
#### <a name="example"></a>Örnek  
  
```xml  
  
<cache-remove-value key="@("userprofile-" + context.Variables["enduserid"])"/>  
  
```  
  
#### <a name="elements"></a>Öğeleri  
  
|Ad|Açıklama|Gerekli|  
|----------|-----------------|--------------|  
|Önbellek Kaldır değeri|Kök öğesi.|Evet|  
  
#### <a name="attributes"></a>Öznitelikler  
  
|Ad|Açıklama|Gerekli|Varsayılan|  
|----------|-----------------|--------------|-------------|  
|anahtar|Önbellekten kaldırılacak önceden önbelleğe alınan değerin anahtarı.|Evet|Yok|  
  
#### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .  
  
-   **İlke bölümleri:** gelen, giden arka uç, hata  
-   **İlke kapsamları:** Genel API, işlemi, ürünü  

## <a name="next-steps"></a>Sonraki adımlar

İlkeleriyle çalışma daha fazla bilgi için bkz:

+ [API Management ilkeleri](api-management-howto-policies.md)
+ [API dönüştürme](transform-api.md)
+ [Grup İlkesi başvurusu](api-management-policy-reference.md) ilke deyimleri ve ayarlarının tam listesi için
+ [İlke örnekleri](policy-samples.md)   
