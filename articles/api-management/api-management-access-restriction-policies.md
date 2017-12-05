---
title: "Azure API Management erişimi kısıtlama ilkeleri | Microsoft Docs"
description: "Azure API Management'te kullanıma erişim kısıtlama ilkeleri hakkında bilgi edinin."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 034febe3-465f-4840-9fc6-c448ef520b0f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: apimpm
ms.openlocfilehash: f9872ee033d8c0bed215f8b37d64395e5dcd534c
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="api-management-access-restriction-policies"></a>API yönetim erişim kısıtlama ilkeleri
Bu konu aşağıdaki API Management ilkeleri bir başvuru sağlar. Ekleme ve ilkeleri yapılandırma hakkında daha fazla bilgi için bkz: [API Management ilkeleri](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="AccessRestrictionPolicies"></a>Erişimi kısıtlama ilkeleri  
  
-   [Onay HTTP üstbilgisi](api-management-access-restriction-policies.md#CheckHTTPHeader) -varlığı ve/veya bir HTTP üstbilgisi değerini zorlar.  
-   [Abonelik tarafından çağrı hızını sınırla](api-management-access-restriction-policies.md#LimitCallRate) -engeller API kullanımı ani başına abonelik başına çağrı hızını sınırlayarak.  
-   [Anahtara göre çağrı hızını sınırla](#LimitCallRateByKey) -engeller API kullanımı ani başına anahtar başına çağrı hızını sınırlayarak.  
-   [Arayan IP'leri kısıtlamak](api-management-access-restriction-policies.md#RestrictCallerIPs) -filtreleri (verir/engellediği) çağrılarından belirli IP adreslerini ve/veya adres aralığı.  
-   [Abonelik tarafından kullanım kotası ayarla](api-management-access-restriction-policies.md#SetUsageQuota) -yenilenebilir veya ömrü çağrısı birim ve/veya bant genişliği kota, abonelik başına temelinde zorunlu tutmanıza olanak tanır.  
-   [Anahtara göre kullanım kotası ayarla](#SetUsageQuotaByKey) -yenilenebilir veya ömrü çağrısı birim ve/veya bant genişliği kota anahtarı başına temelinde zorunlu tutmanıza olanak tanır.  
-   [JWT doğrulama](api-management-access-restriction-policies.md#ValidateJWT) -varlığı ve belirtilen bir HTTP üstbilgisi veya belirtilen sorgu parametresi ayıklanan JWT geçerliliğini zorlar.  
  
##  <a name="CheckHTTPHeader"></a>HTTP üstbilgisi denetleyin  
 Kullanım `check-header` bir istek belirtilen bir HTTP üstbilgisi olduğunu zorlamak için ilke. İsteğe bağlı olarak üstbilgi belirli bir değer veya izin verilen değer aralığı için onay olup olmadığını görmek için kontrol edebilirsiniz. Denetimi başarısız olursa, ilke isteği işleme sonlandırır ve ilke tarafından belirtilen HTTP durum kodu ve hata iletisi döndürür.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<check-header name="header name" failed-check-httpcode="code" failed-check-error-message="message" ignore-case="True">  
    <value>Value1</value>  
    <value>Value2</value>  
</check-header>  
```  
  
### <a name="example"></a>Örnek  
  
```xml  
<check-header name="Authorization" failed-check-httpcode="401" failed-check-error-message="Not authorized" ignore-case="false">  
    <value>f6dc69a089844cf6b2019bae6d36fac8</value>  
</check-header>  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Ad|Açıklama|Gerekli|  
|----------|-----------------|--------------|  
|onay üstbilgisi|Kök öğesi.|Evet|  
|değer|İzin verilen HTTP üstbilgisi değeri. Birden çok değer öğesi belirtildiğinde değerlerden herhangi biri bir eşleşme varsa onay başarı olarak kabul edilir.|Hayır|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Ad|Açıklama|Gerekli|Varsayılan|  
|----------|-----------------|--------------|-------------|  
|başarısız oldu-onay-hata iletisi|HTTP yanıt gövdesinde başlığı yok veya geçersiz bir değere sahip ise döndürülecek hata iletisi. Bu ileti, tüm özel karakterleri düzgün şekilde çıkıldığından olması gerekir.|Evet|Yok|  
|başarısız oldu-onay-httpcode|Üstbilgi yok veya geçersiz bir değere sahip olursa döndürmek için HTTP durum kodu.|Evet|Yok|  
|üstbilgi adı|Denetlenecek HTTP üstbilgisinin adı.|Evet|Yok|  
|Yoksay durumu|TRUE veya False ayarlanabilir. Üstbilgi değeri kabul edilebilir değerler kümesi karşı karşılaştırıldığında doğru servis talebi kümesine göz ardı gerekiyorsa.|Evet|Yok|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** gelen, giden  
  
-   **İlke kapsamları:** genel, ürün, API işlemi  
  
##  <a name="LimitCallRate"></a>Abonelik tarafından çağrı hızını sınırla  
 `rate-limit` İlke sayıyı belirtilen sayıda belirli bir süre başına çağrı hızını sınırlayarak API kullanımındaki ani bir abonelik başına temelinde engeller. Bu ilke tetiklendiğinde arayan alan bir `429 Too Many Requests` yanıt durum kodu.  
  
> [!IMPORTANT]
>  Bu ilke, ilke belge başına yalnızca bir kez kullanılabilir.  
>   
>  [İlke ifadeleri](api-management-policy-expressions.md) İlkesi özniteliklerini hiçbirinde Bu ilke için kullanılamaz.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<rate-limit calls="number" renewal-period="seconds">  
    <api name="name" calls="number" renewal-period="seconds">  
        <operation name="name" calls="number" renewal-period="seconds" />  
    </api>  
</rate-limit>  
```  
  
### <a name="example"></a>Örnek  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <rate-limit calls="20" renewal-period="90" />  
    </inbound>  
    <outbound>  
        <base />          
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Ad|Açıklama|Gerekli|  
|----------|-----------------|--------------|  
|sınırını ayarlama|Kök öğesi.|Evet|  
|api|Bir veya daha fazla ürün içinde çağrı hızı sınırı üzerinde API'leri zorunlu tuttukları için bu öğeleri ekleyin. Ürün ve API sınırları bağımsız olarak uygulanır oranı arayın.|Hayır|  
|işlemi|Bir veya daha fazla bir API işlemlerini bir çağrı hızı sınırı koymak için bu öğeleri ekleyin. Ürün, API ve işlem sınırları bağımsız olarak uygulanır oranı çağırın.|Hayır|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Ad|Açıklama|Gerekli|Varsayılan|  
|----------|-----------------|--------------|-------------|  
|ad|Hız sınırı uygulanacağı için API adı.|Evet|Yok|  
|çağrı|Çağrı belirtilen zaman aralığı boyunca izin verilen en fazla toplam sayısı `renewal-period`.|Evet|Yok|  
|yenileme dönemi|Zaman aralığını saniye olarak geçmesi kota sıfırlar.|Evet|Yok|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** gelen  
  
-   **İlke kapsamları:** ürün  
  
##  <a name="LimitCallRateByKey"></a>Anahtara göre çağrı hızını sınırla  
 `rate-limit-by-key` İlke sayıyı belirtilen sayıda belirli bir süre başına çağrı hızını sınırlayarak API kullanımındaki ani bir anahtar başına temelinde engeller. Anahtar isteğe bağlı bir dize değerine sahip olabilir ve genellikle bir ilke ifade kullanılarak sağlanır. İsteğe bağlı increment koşulu, hangi isteklerinin sınırında sayılır belirtmek için eklenebilir. Bu ilke tetiklendiğinde arayan alan bir `429 Too Many Requests` yanıt durum kodu.  
  
 Daha fazla bilgi ve bu ilkeyi örnekleri için bkz: [Gelişmiş istek azaltma Azure API Management ile](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).  
  
> [!IMPORTANT]
>  Bu ilke, ilke belge başına yalnızca bir kez kullanılabilir.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<rate-limit-by-key calls="number"  
                   renewal-period="seconds"   
                   increment-condition="condition"   
                   counter-key="key value" />  
  
```  
  
### <a name="example"></a>Örnek  
 Aşağıdaki örnekte, hız sınırı arayan IP adresine göre anahtarlanır.  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <rate-limit-by-key  calls="10"  
              renewal-period="60"  
              increment-condition="@(context.Response.StatusCode == 200)"  
              counter-key="@(context.Request.IpAddress)"/>  
    </inbound>  
    <outbound>  
        <base />          
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Ad|Açıklama|Gerekli|  
|----------|-----------------|--------------|  
|sınırını ayarlama|Kök öğesi.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Ad|Açıklama|Gerekli|Varsayılan|  
|----------|-----------------|--------------|-------------|  
|çağrı|Çağrı belirtilen zaman aralığı boyunca izin verilen en fazla toplam sayısı `renewal-period`.|Evet|Yok|  
|Tamamlayıcı anahtarı|Hız sınırı ilkesi için kullanılacak anahtarı.|Evet|Yok|  
|Koşul artırma|İstek kota sayılan varsa belirten boolean ifadesi (`true`).|Hayır|Yok|  
|yenileme dönemi|Zaman aralığını saniye olarak geçmesi kota sıfırlar.|Evet|Yok|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** gelen  
  
-   **İlke kapsamları:** genel, ürün, API işlemi  
  
##  <a name="RestrictCallerIPs"></a>Arayan IP'leri kısıtla  
 `ip-filter` İlke filtrelerini belirli IP adreslerini ve/veya adres aralıklarını gelen çağrıları (verir/engeller).  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="example"></a>Örnek  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Ad|Açıklama|Gerekli|  
|----------|-----------------|--------------|  
|IP Filtresi|Kök öğesi.|Evet|  
|Adres|Filtre uygulamak tek bir IP adresini belirtir.|En az bir `address` veya `address-range` öğesi gereklidir.|  
|adres aralığı adresinden ="" için "Adres" =|Bir IP adresi aralığı filtrelemek için belirtir.|En az bir `address` veya `address-range` öğesi gereklidir.|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Ad|Açıklama|Gerekli|Varsayılan|  
|----------|-----------------|--------------|-------------|  
|adres aralığı adresinden ="" için "Adres" =|Bir dizi IP adreslerini izin vermek veya erişimi reddetmek için.|Ne zaman gerekli `address-range` öğe kullanılır.|Yok|  
|IP filtre eylemi = "izin &#124; yasaklamaz"|Çağrıları verilip verilmeyeceğini veya belirtilen IP adresleri ve aralıkları için belirtir.|Evet|Yok|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** gelen  
-   **İlke kapsamları:** genel, ürün, API işlemi  
  
##  <a name="SetUsageQuota"></a>Abonelik tarafından kullanım kotası ayarla  
 `quota` İlke yenilenebilir veya ömrü çağrısı birim ve/veya bant genişliği kota, abonelik başına temelinde zorunlu tutar.  
  
> [!IMPORTANT]
>  Bu ilke, ilke belge başına yalnızca bir kez kullanılabilir.  
>   
>  [İlke ifadeleri](api-management-policy-expressions.md) İlkesi özniteliklerini hiçbirinde Bu ilke için kullanılamaz.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">  
    <api name="name" calls="number" bandwidth="kilobytes">  
        <operation name="name" calls="number" bandwidth="kilobytes" />  
    </api>  
</quota>  
```  
  
### <a name="example"></a>Örnek  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <quota calls="10000" bandwidth="40000" renewal-period="3600" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Ad|Açıklama|Gerekli|  
|----------|-----------------|--------------|  
|Kota|Kök öğesi.|Evet|  
|api|Bir veya daha fazla API'lerde kota ürün içinde koymak için bu öğeleri ekleyin. Ürün ve API kotaları bağımsız olarak uygulanır.|Hayır|  
|işlemi|Bir veya daha fazla bir API işlemlerini bir kota koymak için bu öğeleri ekleyin. Ürün, API ve işlem kotaları bağımsız olarak uygulanır.|Hayır|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Ad|Açıklama|Gerekli|Varsayılan|  
|----------|-----------------|--------------|-------------|  
|ad|API veya adını kota uygulandığı işlemi.|Evet|Yok|  
|Bant genişliği|Kilobayt belirtilen zaman aralığı boyunca izin verilen en fazla toplam sayısı `renewal-period`.|Her iki `calls`, `bandwidth`, veya her ikisini de birlikte belirtilmesi gerekir.|Yok|  
|çağrı|Çağrı belirtilen zaman aralığı boyunca izin verilen en fazla toplam sayısı `renewal-period`.|Her iki `calls`, `bandwidth`, veya her ikisini de birlikte belirtilmesi gerekir.|Yok|  
|yenileme dönemi|Zaman aralığını saniye olarak geçmesi kota sıfırlar.|Evet|Yok|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** gelen  
-   **İlke kapsamları:** ürün  
  
##  <a name="SetUsageQuotaByKey"></a>Anahtara göre kullanım kotası ayarla  
 `quota-by-key` İlke yenilenebilir veya ömrü çağrısı birim ve/veya bant genişliği kota anahtarı başına temelinde zorunlu tutar. Anahtar isteğe bağlı bir dize değerine sahip olabilir ve genellikle bir ilke ifade kullanılarak sağlanır. İsteğe bağlı increment koşulu, hangi istekler kota sayılması belirtmek için eklenebilir.  
  
 Daha fazla bilgi ve bu ilkeyi örnekleri için bkz: [Gelişmiş istek azaltma Azure API Management ile](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).  
  
> [!IMPORTANT]
>  Bu ilke, ilke belge başına yalnızca bir kez kullanılabilir.  
>   
>  [İlke ifadeleri](api-management-policy-expressions.md) İlkesi özniteliklerini hiçbirinde Bu ilke için kullanılamaz.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<quota-by-key calls="number"   
              bandwidth="kilobytes"   
              renewal-period="seconds"  
              increment-condition="condition"   
              counter-key="key value" />  
  
```  
  
### <a name="example"></a>Örnek  
 Aşağıdaki örnekte, kota arayan IP adresine göre anahtarlanır.  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <quota-by-key calls="10000" bandwidth="40000" renewal-period="3600"  
                      increment-condition="@(context.Response.StatusCode >= 200 && context.Response.StatusCode < 400)"  
                      counter-key="@(context.Request.IpAddress)" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Ad|Açıklama|Gerekli|  
|----------|-----------------|--------------|  
|Kota|Kök öğesi.|Evet|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Ad|Açıklama|Gerekli|Varsayılan|  
|----------|-----------------|--------------|-------------|  
|Bant genişliği|Kilobayt belirtilen zaman aralığı boyunca izin verilen en fazla toplam sayısı `renewal-period`.|Her iki `calls`, `bandwidth`, veya her ikisini de birlikte belirtilmesi gerekir.|Yok|  
|çağrı|Çağrı belirtilen zaman aralığı boyunca izin verilen en fazla toplam sayısı `renewal-period`.|Her iki `calls`, `bandwidth`, veya her ikisini de birlikte belirtilmesi gerekir.|Yok|  
|Tamamlayıcı anahtarı|Kota ilkesi için kullanılacak anahtarı.|Evet|Yok|  
|Koşul artırma|İstek kota sayılan varsa belirten boolean ifadesi (`true`)|Hayır|Yok|  
|yenileme dönemi|Zaman aralığını saniye olarak geçmesi kota sıfırlar.|Evet|Yok|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** gelen  
-   **İlke kapsamları:** genel, ürün, API işlemi  
  
##  <a name="ValidateJWT"></a>JWT doğrula  
 `validate-jwt` İlkesi varlığı zorunlu kılan ve HTTP üstbilgisi veya belirtilen sorgu parametresi JWT geçerliliğini ya da belirtilen bir ayıklanır.  
  
> [!IMPORTANT]
>  `validate-jwt` İlkesi gerektiriyor `exp` kayıtlı talep olan JWT belirteci inlcuded sürece `require-expiration-time` özniteliği belirtilen ve kümesine `false`.  
> `validate-jwt` İlkesi HS256 ve RS256 imza algoritmaları destekler. HS256 için anahtar base64 ile kodlanmış form ilkesinde içi sağlanmalıdır. RS256 için anahtar olmasını sağlamak bir Open ID yapılandırma uç noktası aracılığıyla sahiptir.  
  
### <a name="policy-statement"></a>İlke bildirimi  
  
```xml  
<validate-jwt   
    header-name="name of http header containing the token (use query-parameter-name attribute if the token is passed in the URL)"   
    failed-validation-httpcode="http status code to return on failure"   
    failed-validation-error-message="error message to return on failure"   
    require-expiration-time="true|false"
    require-scheme="scheme"
    require-signed-tokens="true|false"   
    clock-skew="allowed clock skew in seconds">  
  <issuer-signing-keys>  
    <key>base64 encoded signing key</key>  
    <!-- if there are multiple keys, then add additional key elements -->  
  </issuer-signing-keys>  
  <audiences>  
    <audience>audience string</audience>  
    <!-- if there are multiple possible audiences, then add additional audience elements -->  
  </audiences>  
  <issuers>  
    <issuer>issuer string</issuer>  
    <!-- if there are multiple possible issuers, then add additional issuer elements -->  
  </issuers>  
  <required-claims>  
    <claim name="name of the claim as it appears in the token" match="all|any" separator="separator character in a multi-valued claim">
      <value>claim value as it is expected to appear in the token</value>  
      <!-- if there is more than one allowed values, then add additional value elements -->  
    </claim>  
    <!-- if there are multiple possible allowed values, then add additional value elements -->  
  </required-claims>  
  <openid-config url="full URL of the configuration endpoint, e.g. https://login.constoso.com/openid-configuration" />  
  <zumo-master-key id="key identifier">key value</zumo-master-key>  
</validate-jwt>  
  
```  
  
### <a name="examples"></a>Örnekler  
  
#### <a name="azure-mobile-services-token-validation"></a>Azure Mobile Services belirteci doğrulama  
  
```xml  
<validate-jwt header-name="x-zumo-auth" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Supplied access token is invalid.">  
    <issuers>  
        <issuer>urn:microsoft:windows-azure:zumo</issuer>  
    </issuers>  
    <audiences>  
        <audience>Facebook</audience>  
    </audiences>  
    <issuer-signing-keys>  
        <zumo-master-key id="0">insert key here</zumo-master-key>  
    </issuer-signing-keys>  
</validate-jwt>  
```  
  
#### <a name="azure-active-directory-token-validation"></a>Azure Active Directory token doğrulama  
  
```xml  
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">  
    <openid-config url="https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration" />  
    <audiences>
        <audience>25eef6e4-c905-4a07-8eb4-0d08d5df8b3f</audience>
    </audiences>
    <required-claims>  
        <claim name="id" match="all">  
            <value>insert claim here</value>  
        </claim>  
    </required-claims>  
</validate-jwt>  
```  

  
#### <a name="azure-active-directory-b2c-token-validation"></a>Azure Active Directory B2C belirteç doğrulama  
  
```xml  
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">  
    <openid-config url="https://login.microsoftonline.com/tfp/contoso.onmicrosoft.com/b2c_1_signin/v2.0/.well-known/openid-configuration" />
    <audiences>
        <audience>d313c4e4-de5f-4197-9470-e509a2f0b806</audience>
    </audiences>
    <required-claims>  
        <claim name="id" match="all">  
            <value>insert claim here</value>  
        </claim>  
    </required-claims>  
</validate-jwt>  
```  
  
#### <a name="authorize-access-to-operations-based-on-token-claims"></a>Belirteç talepleri temelinde işlemleri erişim yetkisi  
 Bu örnek nasıl kullanılacağını gösterir [doğrulamak JWT](api-management-access-restriction-policies.md#ValidateJWT) işlemlerine erişim önceden yetkilendirmek için ilke tabanlı belirteç talep. Yapılandırma ve bu ilkeyi kullanan bir örnek için bkz: [bulut kapak bölüm 177: daha Vlad Vinogradsky ile yönetim özelliklerini API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) ve ileri sarma 13:50. İlke Düzenleyicisi'nde yapılandırılmış ilkeler görmek için 15:00 ve 18:50 hem ile hem de gerekli yetkilendirme belirteci olmadan Geliştirici portalından bir işlem arama tanıtımı için ileri sarma.  
  
```xml  
<!-- Copy the following snippet into the inbound section at the api (or higher) level to pre-authorize access to operations based on token claims -->  
<set-variable name="signingKey" value="insert signing key here" />  
<choose>  
  <when condition="@(context.Request.Method.Equals("patch",StringComparison.OrdinalIgnoreCase))">  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
      <required-claims>  
        <claim name="edit">  
          <value>true</value>  
        </claim>  
      </required-claims>  
    </validate-jwt>  
  </when>  
  <when condition="@(new [] {"post", "put"}.Contains(context.Request.Method,StringComparer.OrdinalIgnoreCase))">  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
      <required-claims>  
        <claim name="create">  
          <value>true</value>  
        </claim>  
      </required-claims>  
    </validate-jwt>  
  </when>  
  <otherwise>  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
    </validate-jwt>  
  </otherwise>  
</choose>  
```  
  
### <a name="elements"></a>Öğeleri  
  
|Öğe|Açıklama|Gerekli|  
|-------------|-----------------|--------------|  
|doğrulama jwt|Kök öğesi.|Evet|  
|Hedef kitle|Belirteçte mevcut olabilecek kabul edilebilir İzleyici talep listesini içerir. Birden fazla hedef kitle değer mevcut sonra her değer denenir kadar tüm (Bu durumda doğrulama başarısız) tükendiği veya biri başarılı olana kadar. En az bir hedef kitle belirtilmelidir.|Hayır|  
|verici İmzalama anahtarları|İmzalanmış belirteçleri doğrulamak için kullanılan güvenlik Base64 ile kodlanmış anahtarları listesi. Birden çok güvenlik anahtarları varsa sonra her anahtar denenir kadar tüm (Bu durumda doğrulama başarısız) tükendiği veya biri (belirteci geçiş işlemi için yararlıdır) başarılı olana kadar. Anahtar öğeleri sahip isteğe bağlı bir `id` karşı eşleştirmek için kullanılan öznitelik `kid` talep.|Hayır|  
|verenler|Belirtecin kabul edilebilir sorumluların listesini. Birden çok veren değer mevcut sonra her değer denenir kadar tüm (Bu durumda doğrulama başarısız) tükendiği veya biri başarılı olana kadar.|Hayır|  
|openıd-config|İçinden İmzalama anahtarları ve veren alınabilir uyumlu bir Open ID yapılandırma endpoint belirtmek için kullanılan öğe.|Hayır|  
|gerekli talep|Talep belirteci için geçerli kabul edilebilmesi için mevcut olması beklenen listesini içerir. Zaman `match` özniteliği `all` her talep değeri ilkesinde başarılı olması doğrulama belirteci mevcut olması gerekir. Zaman `match` özniteliği `any` en az bir talep belirteci başarılı olması doğrulama için mevcut olmalıdır.|Hayır|  
|zumo ana anahtarı|Ana anahtar için Azure Mobile Services tarafından yayınlanan belirteçleri|Hayır|  
  
### <a name="attributes"></a>Öznitelikler  
  
|Ad|Açıklama|Gerekli|Varsayılan|  
|----------|-----------------|--------------|-------------|  
|saat eğriltme|TimeSpan. Belirtecin sona erme talep belirteçte ve geçerli tarihi durumda bazı küçük leeway sağlar / saat.|Hayır|0 saniye|  
|başarısız oldu-doğrulama-hata iletisi|JWT doğrulamayı geçemezse HTTP yanıt gövdesinde döndürmek için hata iletisi. Bu ileti, tüm özel karakterleri düzgün şekilde çıkıldığından olması gerekir.|Hayır|Varsayılan hata iletisini doğrulama sorunu, örneğin "JWT mevcut değil." bağlıdır|  
|başarısız oldu-doğrulama-httpcode|JWT doğrulama geçmiyor ise döndürülecek HTTP durum kodu.|Hayır|401|  
|üstbilgi adı|Belirteç bulunduran HTTP üstbilgisinin adı.|Ya da `header-name` veya `query-paremeter-name` belirtilmelidir; ancak ikisi birden değil.|Yok|  
|id|`id` Özniteliği `key` öğesi karşı eşleşen dize belirtmenize olanak verir `kid` imza doğrulama için kullanılacak uygun anahtarını bulmak (varsa) belirteç talep.|Hayır|Yok|  
|eşleşme|`match` Özniteliği `claim` öğesi ilkedeki her talep değeri başarılı olması doğrulama belirteçte mevcut olup olmadığını belirtir. Olası değerler şunlardır:<br /><br /> -                          `all`-her talep değeri ilkesinde başarılı olması doğrulama belirteci mevcut olması gerekir.<br /><br /> -                          `any`-en az bir talep değeri başarılı olması doğrulama belirteci mevcut olmalıdır.|Hayır|tümü|  
|Sorgu paremeter adı|Adını belirteci bulunduran sorgu parametresi.|Ya da `header-name` veya `query-paremeter-name` belirtilmelidir; ancak ikisi birden değil.|Yok|  
|gerekli-sona erme-time|Boole değeri. Bir süre sonu talep belirteçte gerekip gerekmediğini belirtir.|Hayır|true|
|gerektiren düzeni|Belirteç adını düzen, örn. "Bearer". Bu öznitelik ayarlandığında, ilke yetkilendirme üst bilgisi değeri, belirtilen düzeni bulunduğundan emin olun.|Hayır|Yok|
|gerekli-oturum-belirteçleri|Boole değeri. Bir belirteç imzalanmasını gerekli olup olmadığını belirtir.|Hayır|true|  
|ayırıcı|Dize. Ayırıcı belirtir (örneğin ",") birden çok değerli bir talep değerleri kümesi ayıklanması için kullanılacak.|Hayır|Yok| 
|url|Burada Open ID yapılandırma meta verilerini elde edilebilir gelen kimliği yapılandırma uç noktasının URL'sini açın. Azure Active Directory için şu URL'yi kullanın: `https://login.microsoftonline.com/{tenant-name}/.well-known/openid-configuration` directory Kiracı adınız, örneğin değiştirerek `contoso.onmicrosoft.com`.|Evet|Yok|  
  
### <a name="usage"></a>Kullanım  
 Bu ilke aşağıdaki ilkesi kullanılabilir [bölümleri](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **İlke bölümleri:** gelen  
-   **İlke kapsamları:** genel, ürün, API işlemi  
  
## <a name="next-steps"></a>Sonraki adımlar

İlkeleriyle çalışma daha fazla bilgi için bkz:

+ [API Management ilkeleri](api-management-howto-policies.md)
+ [API dönüştürme](transform-api.md)
+ [Grup İlkesi başvurusu](api-management-policy-reference.md) ilke deyimleri ve ayarlarının tam listesi için
+ [İlke örnekleri](policy-samples.md)   