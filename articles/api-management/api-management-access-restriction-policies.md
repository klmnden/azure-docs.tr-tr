---
title: Azure API Management erişim kısıtlama ilkeleri | Microsoft Docs
description: Azure API Yönetimi'nde kullanıma erişimi kısıtlama ilkeleri hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 034febe3-465f-4840-9fc6-c448ef520b0f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: apimpm
ms.openlocfilehash: 41f9ce38124cdee2166b5a573c4ab91a26c5fb8a
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58402431"
---
# <a name="api-management-access-restriction-policies"></a>API Management erişim kısıtlama ilkeleri

Bu konu aşağıdaki API Management ilkeleri bir başvuru sağlar. Ekleme ve ilkeleri yapılandırma hakkında daha fazla bilgi için bkz: [API Management ilkeleri](https://go.microsoft.com/fwlink/?LinkID=398186).

## <a name="AccessRestrictionPolicies"></a> Erişimi kısıtlama ilkeleri

-   [Onay HTTP üst bilgisi](api-management-access-restriction-policies.md#CheckHTTPHeader) -varlığı ve/veya HTTP üstbilgisi değerini zorunlu kılar.
-   [Abonelik yoluyla çağrı hızını sınırla](api-management-access-restriction-policies.md#LimitCallRate) -engeller API kullanımı ani olarak abonelik başına çağrı hızını sınırlama.
-   [Anahtara göre çağrı hızını sınırla](#LimitCallRateByKey) -engeller API kullanımı ani başına anahtar başına çağrı hızını sınırlama tarafından.
-   [Arayan IP kısıtlama](api-management-access-restriction-policies.md#RestrictCallerIPs) -belirli IP adreslerinin ve/veya adres aralıkları filtreleri (sağlayan/engeller) çağırır.
-   [Abonelik tarafından kullanım kotası ayarla](api-management-access-restriction-policies.md#SetUsageQuota) -yenilenebilir veya ömrü çağrı birim ve/veya bant genişliği kota, abonelik başına temelinde zorunlu tutmanıza olanak tanır.
-   [Anahtara göre kullanım kotası ayarla](#SetUsageQuotaByKey) -yenilenebilir veya ömrü çağrı birim ve/veya bant genişliği kota anahtarı başına temelinde zorunlu tutmanıza olanak tanır.
-   [JWT doğrulama](api-management-access-restriction-policies.md#ValidateJWT) -varlığı ve belirtilen bir HTTP üstbilgisi veya belirtilen sorgu parametresi ayıklanan JWT'nin geçerliliğini zorunlu kılar.

## <a name="CheckHTTPHeader"></a> HTTP üst bilgisini denetleyin

Kullanım `check-header` bir istek belirtilen bir HTTP üstbilgisi olduğunu zorlamak için ilke. İsteğe bağlı olarak belirli bir değer veya izin verilen değer aralığı için onay üstbilgi olup olmadığını görmek için kontrol edebilirsiniz. Denetimi başarısız olursa, ilke isteği işlemini sonlandırır ve ilke tarafından belirtilen HTTP durum kodu ve hata iletisi döndürür.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<check-header name="header name" failed-check-httpcode="code" failed-check-error-message="message" ignore-case="true">
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

### <a name="elements"></a>Öğeler

| Ad         | Açıklama                                                                                                                                   | Gerekli |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| Üstbilgi denetimi | Kök öğe.                                                                                                                                 | Evet      |
| değer        | İzin verilen HTTP üstbilgisi değeri. Birden çok değer öğesi belirtildiğinde değerlerden herhangi biri bir eşleşme varsa onay başarı olarak kabul edilir. | Hayır       |

### <a name="attributes"></a>Öznitelikler

| Ad                       | Açıklama                                                                                                                                                            | Gerekli | Varsayılan |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------- |
| başarısız oldu-onay-hata iletisi | Üst bilgi yok veya geçersiz bir değere sahip olursa HTTP yanıt gövdesinde döndürülecek hata iletisi. Bu ileti için doğru kaçış karakterleri dışında özel karakter olmalıdır. | Evet      | Yok     |
| başarısız oldu-onay-httpcode      | Üst bilgisi yok veya geçersiz bir değere sahip ise döndürülecek HTTP durum kodu.                                                                                        | Evet      | Yok     |
| üst bilgi adı                | Denetlenecek HTTP üstbilgisinin adı.                                                                                                                                  | Evet      | Yok     |
| Yoksay örneği                | TRUE veya False olarak ayarlanabilir. Üst bilgi değeri kabul edilebilir değerler kümesini karşı karşılaştırıldığında kümesi doğru çalışması için göz ardı edilip.                                    | Evet      | Yok     |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen, giden

-   **İlke kapsamları:** genel, ürün, API, işlemi

## <a name="LimitCallRate"></a> Abonelik yoluyla çağrı hızını sınırla

`rate-limit` İlke API ani bir abonelik başına temelinde belirtilen sayıda belirtilen zaman süresi başına çağrı hızını sınırlama tarafından engeller. Bu ilke tetiklenir çağıranın alır bir `429 Too Many Requests` yanıt durum kodu.

> [!IMPORTANT]
> Bu ilke, ilke belge başına yalnızca bir kez kullanılabilir.
>
> [İlke ifadeleri](api-management-policy-expressions.md) herhangi bir ilke özniteliği bu ilke için kullanılamaz.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<rate-limit calls="number" renewal-period="seconds">
    <api name="API name" id="API id" calls="number" renewal-period="seconds" />
        <operation name="operation name" id="operation id" calls="number" renewal-period="seconds" />
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

### <a name="elements"></a>Öğeler

| Ad      | Açıklama                                                                                                                                                                                                                                                                                              | Gerekli |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| sınırını ayarlama | Kök öğe.                                                                                                                                                                                                                                                                                            | Evet      |
| api       | Bir veya daha fazla ürün içinde çağrı hızı sınırını API'lerini dayatmak için bu öğeleri ekleyin. Oran sınırları bağımsız olarak uygulanır, ürün ve API çağırın. API olabilir yoluyla başvurulan `name` veya `id`. Her iki öznitelik sağlanırsa, `id` kullanılacak ve `name` göz ardı edilir.                    | Hayır       |
| işlem | Bir veya daha fazla API işlemlerini bir çağrı hızı sınırı dayatmak için bu öğeleri ekleyin. Oran sınırları bağımsız olarak uygulanır, ürün, API ve işleme çağırın. İşlem olabilir ya da aracılığıyla başvurulan `name` veya `id`. Her iki öznitelik sağlanırsa, `id` kullanılacak ve `name` göz ardı edilir. | Hayır       |

### <a name="attributes"></a>Öznitelikler

| Ad           | Açıklama                                                                                           | Gerekli | Varsayılan |
| -------------- | ----------------------------------------------------------------------------------------------------- | -------- | ------- |
| ad           | Hız sınırını uygulanacağı API adı.                                                | Evet      | Yok     |
| çağrı          | Çağrı belirtilen zaman aralığı boyunca izin verilen en fazla toplam sayısı `renewal-period`. | Evet      | Yok     |
| yenileme dönemi | Kota sonra sıfırlayan saniye cinsinden süre.                                              | Evet      | Yok     |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen

-   **İlke kapsamları:** ürün

## <a name="LimitCallRateByKey"></a> Anahtara göre çağrı hızını sınırla

> [!IMPORTANT]
> Bu özellik kullanılamıyor **tüketim** API Yönetimi katmanı.

`rate-limit-by-key` İlke, belirtilen bir sayıya belirtilen zaman süresi başına çağrı hızını sınırlama tarafından API anahtarı başına temelinde ani engeller. Anahtar, rastgele bir dize olabilir ve genellikle bir ilke ifadesi kullanılarak sağlanır. Hangi istekleri sınırında sayılması belirtmek için isteğe bağlı increment koşul eklenebilir. Bu ilke tetiklenir çağıranın alır bir `429 Too Many Requests` yanıt durum kodu.

Daha fazla bilgi ve işbu politikaya ilişkin örnekler için bkz. [Gelişmiş istek azaltma ile Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).

### <a name="policy-statement"></a>İlke bildirimi

```xml
<rate-limit-by-key calls="number"
                   renewal-period="seconds"
                   increment-condition="condition"
                   counter-key="key value" />

```

### <a name="example"></a>Örnek

Aşağıdaki örnekte, Hız sınırını çağıran IP adresine göre anahtarlanır.

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

### <a name="elements"></a>Öğeler

| Ad      | Açıklama   | Gerekli |
| --------- | ------------- | -------- |
| sınırını ayarlama | Kök öğe. | Evet      |

### <a name="attributes"></a>Öznitelikler

| Ad                | Açıklama                                                                                           | Gerekli | Varsayılan |
| ------------------- | ----------------------------------------------------------------------------------------------------- | -------- | ------- |
| çağrı               | Çağrı belirtilen zaman aralığı boyunca izin verilen en fazla toplam sayısı `renewal-period`. | Evet      | Yok     |
| Tamamlayıcı anahtarı         | Hız sınırı ilkesi için kullanılacak anahtarı.                                                             | Evet      | Yok     |
| Koşul artırma | İstek kota hesaplamanıza dahil sayılan olmadığını belirten bir Boole ifadesi (`true`).        | Hayır       | Yok     |
| yenileme dönemi      | Kota sonra sıfırlayan saniye cinsinden süre.                                              | Evet      | Yok     |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen

-   **İlke kapsamları:** genel, ürün, API, işlemi

## <a name="RestrictCallerIPs"></a> Arayan IP kısıtlama

`ip-filter` İlke filtrelerini belirli IP adreslerinin ve/veya adres aralıkları gelen çağrıları (sağlayan/vermez).

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

### <a name="elements"></a>Öğeler

| Ad                                      | Açıklama                                         | Gerekli                                                       |
| ----------------------------------------- | --------------------------------------------------- | -------------------------------------------------------------- |
| IP Filtresi                                 | Kök öğe.                                       | Evet                                                            |
| adres                                   | Filtre uygulamak tek bir IP adresini belirtir.   | En az bir `address` veya `address-range` öğesi gereklidir. |
| adres aralığı adresinden ="" için "address" = | Filtre uygulamak bir dizi IP adresini belirtir. | En az bir `address` veya `address-range` öğesi gereklidir. |

### <a name="attributes"></a>Öznitelikler

| Ad                                      | Açıklama                                                                                 | Gerekli                                           | Varsayılan |
| ----------------------------------------- | ------------------------------------------------------------------------------------------- | -------------------------------------------------- | ------- |
| adres aralığı adresinden ="" için "address" = | İzin vermek veya erişimi reddetmek için bir dizi IP adresleri.                                        | Ne zaman gerekli `address-range` öğe kullanılır. | Yok     |
| IP Filtresi eylem = "izin ver &#124; yasaklayabilme"    | Çağrıları verilip verilmeyeceğini veya belirtilen IP adresleri ve aralıkları için belirtir. | Evet                                                | Yok     |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen
-   **İlke kapsamları:** genel, ürün, API, işlemi

## <a name="SetUsageQuota"></a> Abonelik tarafından kullanım kotası ayarla

`quota` Yenilenebilir veya ömrü çağrı birim ve/veya bant genişliği kota, abonelik başına temelinde ilkeleri uygular.

> [!IMPORTANT]
> Bu ilke, ilke belge başına yalnızca bir kez kullanılabilir.
>
> [İlke ifadeleri](api-management-policy-expressions.md) herhangi bir ilke özniteliği bu ilke için kullanılamaz.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    <api name="API name" id="API id" calls="number" renewal-period="seconds" />
        <operation name="operation name" id="operation id" calls="number" renewal-period="seconds" />
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

### <a name="elements"></a>Öğeler

| Ad      | Açıklama                                                                                                                                                                                                                                                                                  | Gerekli |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| kota     | Kök öğe.                                                                                                                                                                                                                                                                                | Evet      |
| api       | Bir veya daha fazla API çağrısı kotasını ürün içinde dayatmak için bu öğeleri ekleyin. Ürün ve API çağrısı kotalar bağımsız olarak uygulanır. API olabilir yoluyla başvurulan `name` veya `id`. Her iki öznitelik sağlanırsa, `id` kullanılacak ve `name` göz ardı edilir.                    | Hayır       |
| işlem | Bir veya daha fazla API işlemlerini çağrı kota koymak için bu öğeleri ekleyin. Ürün, API ve işleme çağrı kotalar bağımsız olarak uygulanır. İşlem olabilir ya da aracılığıyla başvurulan `name` veya `id`. Her iki öznitelik sağlanırsa, `id` kullanılacak ve `name` göz ardı edilir. | Hayır       |

### <a name="attributes"></a>Öznitelikler

| Ad           | Açıklama                                                                                               | Gerekli                                                         | Varsayılan |
| -------------- | --------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | ------- |
| ad           | API veya kota geçerli olduğu için işlem adıdır.                                             | Evet                                                              | Yok     |
| bant genişliği      | Kilobayt belirtilen zaman aralığı boyunca izin verilen en fazla toplam sayısı `renewal-period`. | Her iki `calls`, `bandwidth`, veya her ikisini de birlikte belirtilmesi gerekir. | Yok     |
| çağrı          | Çağrı belirtilen zaman aralığı boyunca izin verilen en fazla toplam sayısı `renewal-period`.     | Her iki `calls`, `bandwidth`, veya her ikisini de birlikte belirtilmesi gerekir. | Yok     |
| yenileme dönemi | Kota sonra sıfırlayan saniye cinsinden süre.                                                  | Evet                                                              | Yok     |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen
-   **İlke kapsamları:** ürün

## <a name="SetUsageQuotaByKey"></a> Anahtara göre kullanım kotası ayarla

> [!IMPORTANT]
> Bu özellik kullanılamıyor **tüketim** API Yönetimi katmanı.

`quota-by-key` Yenilenebilir veya ömrü çağrı birim ve/veya bant genişliği kota anahtarı başına temelinde ilkeleri uygular. Anahtar, rastgele bir dize olabilir ve genellikle bir ilke ifadesi kullanılarak sağlanır. Hangi istekler kota hesaplamanıza dahil sayılması belirtmek için isteğe bağlı increment koşul eklenebilir. Birden çok ilke, aynı anahtar değeri gerçekleştirilemediği, istek başına yalnızca bir kez artırılır. Çağrı sınıra ulaşıldığında, çağırana alır bir `403 Forbidden` yanıt durum kodu.

Daha fazla bilgi ve işbu politikaya ilişkin örnekler için bkz. [Gelişmiş istek azaltma ile Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).

> [İlke ifadeleri](api-management-policy-expressions.md) herhangi bir ilke özniteliği bu ilke için kullanılamaz.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<quota-by-key calls="number"
              bandwidth="kilobytes"
              renewal-period="seconds"
              increment-condition="condition"
              counter-key="key value" />

```

### <a name="example"></a>Örnek

Aşağıdaki örnekte, kota çağıran IP adresine göre anahtarlanır.

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

### <a name="elements"></a>Öğeler

| Ad  | Açıklama   | Gerekli |
| ----- | ------------- | -------- |
| kota | Kök öğe. | Evet      |

### <a name="attributes"></a>Öznitelikler

| Ad                | Açıklama                                                                                               | Gerekli                                                         | Varsayılan |
| ------------------- | --------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- | ------- |
| bant genişliği           | Kilobayt belirtilen zaman aralığı boyunca izin verilen en fazla toplam sayısı `renewal-period`. | Her iki `calls`, `bandwidth`, veya her ikisini de birlikte belirtilmesi gerekir. | Yok     |
| çağrı               | Çağrı belirtilen zaman aralığı boyunca izin verilen en fazla toplam sayısı `renewal-period`.     | Her iki `calls`, `bandwidth`, veya her ikisini de birlikte belirtilmesi gerekir. | Yok     |
| Tamamlayıcı anahtarı         | Kota ilkesinde kullanmak üzere anahtarı.                                                                      | Evet                                                              | Yok     |
| Koşul artırma | İstek kota hesaplamanıza dahil sayılan olmadığını belirten bir Boole ifadesi (`true`)             | Hayır                                                               | Yok     |
| yenileme dönemi      | Kota sonra sıfırlayan saniye cinsinden süre.                                                  | Evet                                                              | Yok     |

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen
-   **İlke kapsamları:** genel, ürün, API, işlemi

## <a name="ValidateJWT"></a> JWT doğrula

`validate-jwt` İlke varlığını zorunlu kılar ve HTTP üstbilgisi veya belirtilen sorgu parametresi bir JWT geçerliliğini ya da bir belirtilen ayıklanır.

> [!IMPORTANT]
> `validate-jwt` İlkesi gerektirir `exp` kayıtlı talep sürece JWT belirteç dahil `require-expiration-time` özniteliği belirtilmiş ve kümesine `false`.
> `validate-jwt` İlkesi HS256 ve RS256 imzalama algoritmaları destekler. HS256 için base64 olarak kodlanmış biçiminde ilke içinde satır içi anahtarı belirtilmelidir. RS256 için anahtar olmasını sağlamak bir Open ID yapılandırma uç noktası aracılığıyla sahiptir.
> `validate-jwt` İlkesi kullanarak aşağıdaki şifreleme algoritmalarını A128CBC-HS256 A192CBC-HS384 A256CBC HS512 simetrik anahtarlarla şifrelenir belirteçleri destekler.

### <a name="policy-statement"></a>İlke bildirimi

```xml
<validate-jwt
    header-name="name of http header containing the token (use query-parameter-name attribute if the token is passed in the URL)"
    failed-validation-httpcode="http status code to return on failure"
    failed-validation-error-message="error message to return on failure"
    token-value="expression returning JWT token as a string"
    require-expiration-time="true|false"
    require-scheme="scheme"
    require-signed-tokens="true|false"
    clock-skew="allowed clock skew in seconds"
    output-token-variable-name="name of a variable to receive a JWT object representing successfully validated token">
  <issuer-signing-keys>
    <key>base64 encoded signing key</key>
    <!-- if there are multiple keys, then add additional key elements -->
  </issuer-signing-keys>
  <decryption-keys>
    <key>base64 encoded signing key</key>
    <!-- if there are multiple keys, then add additional key elements -->
  </decryption-keys>
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

#### <a name="simple-token-validation"></a>Basit bir belirteç doğrulama

```xml
<validate-jwt header-name="Authorization" require-scheme="Bearer">
    <issuer-signing-keys>
        <key>{{jwt-signing-key}}</key>  <!-- signing key specified as a named value -->
    </issuer-signing-keys>
    <audiences>
        <audience>@(context.Request.OriginalUrl.Host)</audience>  <!-- audience is set to API Management host name -->
    </audiences>
    <issuers>
        <issuer>http://contoso.com/</issuer>
    </issuers>
</validate-jwt>
```

#### <a name="azure-active-directory-token-validation"></a>Azure Active Directory belirteci doğrulama

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

#### <a name="authorize-access-to-operations-based-on-token-claims"></a>Belirteç taleplere göre işlemleri erişim yetkisi verme

Bu örnek nasıl kullanılacağını gösterir [doğrulamak için JWT](api-management-access-restriction-policies.md#ValidateJWT) işlemleri erişim yetkisi vermek için ilke temelli belirteç talep değeri.

```xml
<validate-jwt header-name="Authorization" require-scheme="Bearer" output-token-variable-name="jwt">
    <issuer-signing-keys>
        <key>{{jwt-signing-key}}</key> <!-- signing key is stored in a named value -->
    </issuer-signing-keys>
    <audiences>
        <audience>@(context.Request.OriginalUrl.Host)</audience>
    </audiences>
    <issuers>
        <issuer>contoso.com</issuer>
    </issuers>
    <required-claims>
        <claim name="group" match="any">
            <value>finance</value>
            <value>logistics</value>
        </claim>
    </required-claims>
</validate-jwt>
<choose>
    <when condition="@(context.Request.Method == "POST" && !((Jwt)context.Variables["jwt"]).Claims["group"].Contains("finance"))">
        <return-response>
            <set-status code="403" reason="Forbidden" />
        </return-response>
    </when>
</choose>
```

#### <a name="azure-mobile-services-token-validation"></a>Azure mobil hizmetler belirteç doğrulama

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

### <a name="elements"></a>Öğeler

| Öğe             | Açıklama                                                                                                                                                                                                                                                                                                                                           | Gerekli |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| validate-jwt        | Kök öğe.                                                                                                                                                                                                                                                                                                                                         | Evet      |
| İzleyiciler           | Belirteçte bulunabilir kabul edilebilir İzleyici talepleri listesini içerir. Birden fazla hedef kitle değerleri yok sonra her bir değeri denenir kadar tüm (Bu durumda doğrulama başarısız) bitirildi veya başarılı olana kadar. En az bir hedef kitle belirtilmesi gerekir.                                                                     | Hayır       |
| verici İmzalama anahtarları | İmzalanmış belirteçleri doğrulamak için kullanılan güvenlik Base64 kodlamalı anahtarları listesi. Birden çok güvenlik anahtarı mevcut sonra her anahtar denenir kadar tüm (Bu durumda doğrulama başarısız) bitirildi veya (belirteç geçişi için yararlıdır) başarılı olana kadar. Temel öğelere sahip isteğe bağlı `id` karşı eşleştirmek için kullanılan öznitelik `kid` talep.               | Hayır       |
| şifre çözme anahtarları     | Base64 kodlamalı belirteçlerin şifresini çözmek için kullanılan anahtarları listesi. Birden çok güvenlik anahtarı varsa, her anahtar (Bu durumda doğrulama başarısız) ya da tüm anahtarları bitti kadar veya bir anahtar başarılı olana kadar denenir. Temel öğelere sahip isteğe bağlı `id` karşı eşleştirmek için kullanılan öznitelik `kid` talep.                                                 | Hayır       |
| verenler             | Belirteci veren kabul edilebilir sorumluların listesini. Veren birden çok değer varsa sonra her bir değeri denenir kadar tüm (Bu durumda doğrulama başarısız) bitirildi veya başarılı olana kadar.                                                                                                                                         | Hayır       |
| openıd yapılandırma       | Öğe, imzalama anahtarları ve veren alınabilir uyumlu bir Open ID yapılandırma uç belirtmek için kullanılır.                                                                                                                                                                                                                        | Hayır       |
| gerekli talepler     | Talep belirteci için geçerli kabul edilmesi için mevcut olması beklenen bir listesini içerir. Zaman `match` özniteliği `all` ilkesindeki her talep değeri için başarılı olması doğrulama belirteci mevcut olmalıdır. Zaman `match` özniteliği `any` en az bir talep belirteci doğrulamanın başarılı olması için mevcut olmalıdır. | Hayır       |
| zumo ana anahtarı     | Azure mobil hizmetler tarafından verilen belirteçlere için ana anahtar                                                                                                                                                                                                                                                                                                 | Hayır       |

### <a name="attributes"></a>Öznitelikler

| Ad                            | Açıklama                                                                                                                                                                                                                                                                                                                                                                                                                                            | Gerekli                                                                         | Varsayılan                                                                           |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| saat eğriltme                      | Zaman aralığı. Belirteci veren tarafın sistem saatleri ve API Management örneği arasındaki farkı beklenen en uzun süreyi belirtmek için kullanın.                                                                                                                                                                                                                                                                                                               | Hayır                                                                               | 0 saniye                                                                         |
| başarısız oldu-doğrulama-hata iletisi | JWT doğrulamayı geçemezse HTTP yanıt gövdesinde döndürülecek hata iletisi. Bu ileti için doğru kaçış karakterleri dışında özel karakter olmalıdır.                                                                                                                                                                                                                                                                                                 | Hayır                                                                               | Varsayılan hata iletisi doğrulama sorunu, örneğin "JWT mevcut değil." bağlıdır. |
| failed-validation-httpcode      | JWT doğrulama başarısız olursa döndürülecek HTTP durum kodu.                                                                                                                                                                                                                                                                                                                                                                                         | Hayır                                                                               | 401                                                                               |
| üst bilgi adı                     | Belirteci tutarak HTTP üstbilgisinin adı.                                                                                                                                                                                                                                                                                                                                                                                                         | Aşağıdakilerden birini `header-name`, `query-parameter-name` veya `token-value` belirtilmesi gerekir. | Yok                                                                               |
| sorgu parametresi adı            | Belirteci tutarak sorgu parametresinin adı.                                                                                                                                                                                                                                                                                                                                                                                                     | Aşağıdakilerden birini `header-name`, `query-parameter-name` veya `token-value` belirtilmesi gerekir. | Yok                                                                               |
| Belirteç değeri                     | JWT belirteci içeren bir dize döndüren bir ifade                                                                                                                                                                                                                                                                                                                                                                                                     | Aşağıdakilerden birini `header-name`, `query-parameter-name` veya `token-value` belirtilmesi gerekir. | Yok                                                                               |
| id                              | `id` Özniteliği `key` öğesi karşı eşleşen dizeyi belirtmenize olanak verir `kid` belirteçteki imza doğrulaması için kullanılacak uygun anahtarı bulmak (varsa) talep.                                                                                                                                                                                                                                           | Hayır                                                                               | Yok                                                                               |
| eşleşme                           | `match` Özniteliği `claim` öğesi ilkesindeki her talep değeri doğrulamanın başarılı olması için belirteçteki mevcut olması gerekip gerekmediğini belirtir. Olası değerler şunlardır:<br /><br /> - `all` -her talep değeri ilkesinde doğrulamanın başarılı olması için belirteci mevcut olmalıdır.<br /><br /> - `any` -en az bir talep değeri başarılı olması doğrulama için belirteçteki mevcut olmalıdır.                                                       | Hayır                                                                               | tümü                                                                               |
| gerekli-sona erme-saati         | Boole değeri. Belirteç süre sonu talebi gerekip gerekmediğini belirtir.                                                                                                                                                                                                                                                                                                                                                                               | Hayır                                                                               | true                                                                              |
| gerekli düzeni                  | Belirteç adı şeması, örneğin "Bearer". Bu öznitelik ayarlandığında, yetkilendirme üst bilgisi değeri, belirtilen şema varsa ilkeyi sağlayacaktır.                                                                                                                                                                                                                                                                                    | Hayır                                                                               | Yok                                                                               |
| gerekli imzalı-belirteçleri           | Boole değeri. Bir belirteç imzalanmasını gerekli olup olmadığını belirtir.                                                                                                                                                                                                                                                                                                                                                                                           | Hayır                                                                               | true                                                                              |
| Ayırıcı                       | dize. Ayırıcı belirtir (örneğin ","), bir dizi birden çok değerli bir talep ayıklanması için kullanılacak.                                                                                                                                                                                                                                                                                                                                          | Hayır                                                                               | Yok                                                                               |
| url                             | Kimliği yapılandırma uç nokta URL'si nerede Open ID yapılandırma meta verilerini elde edilebilir gelen açın. Yanıt özellikleri için URL'de tanımlandığı gibi uygun:`https://openid.net/specs/openid-connect-discovery-1_0.html#ProviderMetadata`. Azure Active Directory için şu URL'yi kullanın: `https://login.microsoftonline.com/{tenant-name}/.well-known/openid-configuration` dizin Kiracı adınızın, örneğin değiştirerek `contoso.onmicrosoft.com`. | Evet                                                                              | Yok                                                                               |
Çıkış belirteci değişken adı|dize. Türünde bir nesne belirteç değeri alacak bağlam değişkeninin adı [ `Jwt` ](api-management-policy-expressions.md) bağlı belirteç doğrulama başarılı|Hayır|Yok

### <a name="usage"></a>Kullanım

Bu ilke aşağıdaki ilkesinde kullanılabilir [bölümleri](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) ve [kapsamları](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **İlke bölümler:** gelen
-   **İlke kapsamları:** genel, ürün, API, işlemi

## <a name="next-steps"></a>Sonraki adımlar

İlkeleriyle çalışma hakkında bilgi için bkz:

-   [API Management ilkeleri](api-management-howto-policies.md)
-   [API'leri dönüştürme](transform-api.md)
-   [İlke başvurusu](api-management-policy-reference.md) ilke bildirimlerine ve ayarlarının tam listesi için
-   [İlke örnekleri](policy-samples.md)
