---
title: Azure API Management'te özel önbelleğe alma
description: Azure API Management'te anahtarı tarafından öğeleri önbelleğe öğrenin
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 772bc8dd-5cda-41c4-95bf-b9f6f052bc85
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 838850d38c9df51fabcf620831371bed401e9492
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
ms.locfileid: "29376040"
---
# <a name="custom-caching-in-azure-api-management"></a>Azure API Management'te özel önbelleğe alma
Azure API Management hizmeti için yerleşik destek sahip [HTTP yanıt önbelleğe alma](api-management-howto-cache.md) kaynak URL anahtar olarak kullanma. Anahtar kullanarak istek üstbilgileri tarafından değiştirilebilir `vary-by` özellikleri. Tüm HTTP yanıtlarını (diğer adıyla Beyanları) önbelleğe alma işlemi için yararlıdır, ancak bazen yalnızca önbellek temsili bir kısmı için yararlıdır. Yeni [önbellek arama değeri](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) ve [önbellek deposu değeri](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) ilkeleri, depolama ve ilke tanımları içindeki verileri rasgele parçalarını alma olanağı sağlar. Bu özellik ayrıca değeri önceden sunulan ekler [gönderme isteği](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) İlkesi çünkü dış hizmetler yanıtlarının şimdi önbelleğe alabilir.

## <a name="architecture"></a>Mimari
API Management hizmeti kullanan Kiracı başına paylaşılan veri önbelleğe alma, Ölçek birden çok birimi kadar olarak hala aynı erişim sağlamak için verileri önbelleğe. Ancak, bölgeli dağıtımı ile çalışırken, her bölge içinde bağımsız önbellekleri vardır. Önbellek yalnızca kaynak bilgileri parçasının olduğu bir veri deposu olarak davran değil önemlidir. Vermedi ve daha sonra bölgeli dağıtım yararlanmak karar, seyahat kullanıcılarla müşteriler bu önbelleğe alınmış verileri erişimlerini kaybedebilir.

## <a name="fragment-caching"></a>Parça önbelleğe alma
Burada verilen yanıtları belirlemek pahalıdır ve makul bir süre için henüz yeni kalan verileri kısmı içeren bazı durumlar vardır. Örnek olarak, Uçuş Rezervasyonları, uçuş durumu ile ilgili bilgi sağlayan bir uçak tarafından oluşturulmuş bir hizmeti kullanmayı düşünün. Kullanıcı airlines noktaları program üyesi ise, geçerli durumlarıyla ilgili bilgileri de var ve mesafe toplanır. Bu kullanıcı ile ilgili bilgiler farklı depolanabilir, ancak uçuş durumu ve ayırmaları hakkında döndürülen yanıtlar dahil istenebilir. Bu yapılabilir parça önbelleğe alma adlı bir işlem kullanılarak. Birincil gösterimi kullanıcı ile ilgili bilgiler eklenecek nerede belirtmek için bir tür belirteci kullanarak kaynak sunucudan döndürülebilir. 

Arka uç API'si aşağıdaki JSON yanıtı göz önünde bulundurun.

```json
{
  "airline" : "Air Canada",
  "flightno" : "871",
  "status" : "ontime",
  "gate" : "B40",
  "terminal" : "2A",
  "userprofile" : "$userprofile$"
}  
```

Ve ikincil kaynakta `/userprofile/{userid}` olduğunu gibi görünüyor,

```json
{ "username" : "Bob Smith", "Status" : "Gold" }
```

Dahil etmek için API Management uygun kullanıcı bilgileri belirlemek için son kullanıcının kim olduğunu belirlemek gerekir. Bu mekanizma uygulama bağımlıdır. Örnek olarak, kullanıyorum `Subject` , talep bir `JWT` belirteci. 

```xml
<set-variable
  name="enduserid"
  value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />
```

API Management depoları `enduserid` daha sonra kullanmak için bir bağlam değişken değeri. Sonraki adım, önceki bir istek daha önce kullanıcı bilgileri alınır ve önbellekte depolanan varsa belirlemektir. Bunun için API Management kullanır `cache-lookup-value` ilkesi.

```xml
<cache-lookup-value
key="@("userprofile-" + context.Variables["enduserid"])"
variable-name="userprofile" />
```

Anahtar değeri sonra Hayır karşılık gelen önbellek girişi yoksa `userprofile` bağlamının oluşturulur. API Management denetler arama kullanma başarısını `choose` kontrol akışı ilkesi.

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("userprofile"))">
        <!-- If the userprofile context variable doesn’t exist, make an HTTP request to retrieve it.  -->
    </when>
</choose>
```

Varsa `userprofile` bağlamının yoksa, ardından API Management bunu almak için bir HTTP isteği yapmak zorunda geçmeye.

```xml
<send-request
  mode="new"
  response-variable-name="userprofileresponse"
  timeout="10"
  ignore-error="true">

  <!-- Build a URL that points to the profile for the current end-user -->
  <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),
      (string)context.Variables["enduserid"]).AbsoluteUri)
  </set-url>
  <set-method>GET</set-method>
</send-request>
```

API Management kullanır `enduserid` kullanıcı profili kaynağı URL'si oluşturulamadı. API Management yanıt olduğunda, gövde metni yanıt dışında çeker ve bir bağlam değişkende depolar.

```xml
<set-variable
    name="userprofile"
    value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />
```

API Management aynı kullanıcı başka bir istekte bulunduğunda bu HTTP isteği yeniden yapmasını önlemek için bir kullanıcı profili önbellekte depolamak için belirtebilirsiniz.

```xml
<cache-store-value
    key="@("userprofile-" + context.Variables["enduserid"])"
    value="@((string)context.Variables["userprofile"])" duration="100000" />
```

API Management değeri API Management ile almak için ilk olarak çalıştı tam aynı anahtarı kullanarak önbelleğinde saklar. API Management değeri depolamak için seçtiği süresi hakkında temel gereken genellikle bilgi değişikliklerini ve nasıl dayanıklı kullanıcılar için güncel bilgilerdir. 

Önbellekten hala olduğundan bir işlem dışı, ağ isteği ve potansiyel olarak milisaniye onlarca isteği eklemeye devam edebilirsiniz, hayata geçirmek önemlidir. Kullanıcı profili bilgilerini, sorgu veya toplama bilgilerinden birden fazla arka uç veritabanı gerek nedeniyle uzun sürdüğünde belirlerken avantajları gelir.

Son işlem döndürülen yanıt kullanıcı profili bilgilerini güncelleştirmek için adımdır.

```xml
<!-- Update response body with user profile-->
<find-and-replace
    from='"$userprofile$"'
    to="@((string)context.Variables["userprofile"])" />
```

Seçtiğiniz Değiştir bile gerçekleşmezse, yanıt hala geçerli bir JSON olmasını sağlamak için tırnak işaretleri belirtecinin bir parçası dahil etmek için kullanabilirsiniz.  

Tüm adımları birleştirmek sonra sonuç aşağıdaki biri gibi görünen bir ilkedir.

```xml
<policies>
    <inbound>
        <!-- How you determine user identity is application dependent -->
        <set-variable
          name="enduserid"
          value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

        <!--Look for userprofile for this user in the cache -->
        <cache-lookup-value
          key="@("userprofile-" + context.Variables["enduserid"])"
          variable-name="userprofile" />

        <!-- If API Management doesn’t find it in the cache, make a request for it and store it -->
        <choose>
            <when condition="@(!context.Variables.ContainsKey("userprofile"))">
                <!-- Make HTTP request to get user profile -->
                <send-request
                  mode="new"
                  response-variable-name="userprofileresponse"
                  timeout="10"
                  ignore-error="true">

                   <!-- Build a URL that points to the profile for the current end-user -->
                    <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),(string)context.Variables["enduserid"]).AbsoluteUri)</set-url>
                    <set-method>GET</set-method>
                </send-request>

                <!-- Store response body in context variable -->
                <set-variable
                  name="userprofile"
                  value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

                <!-- Store result in cache -->
                <cache-store-value
                  key="@("userprofile-" + context.Variables["enduserid"])"
                  value="@((string)context.Variables["userprofile"])"
                  duration="100000" />
            </when>
        </choose>
        <base />
    </inbound>
    <outbound>
        <!-- Update response body with user profile-->
        <find-and-replace
              from='"$userprofile$"'
              to="@((string)context.Variables["userprofile"])" />
        <base />
    </outbound>
</policies>
```

Önbelleğe alma bu yaklaşım, tek bir sayfa olarak işlenebilecek böylece burada HTML sunucu tarafında oluşur web siteleri, öncelikle kullanılır. Ayrıca, istemcilerin HTTP istemci tarafı önbelleğe alma işlemi yapamazsınız veya bu sorumluluğu istemcide değil yerleştirilecek tercih edilir API'lerde yararlı olabilir.

Parça önbelleğe alma bu aynı tür, arka uç web sunucularında sunucu önbelleği Redis kullanılarak yapılabilir, önbelleğe alınan parçaları birincil daha farklı arka uçları'ten gelen ancak API Management hizmeti kullanarak bu çalışmayı gerçekleştirmek için faydalıdır yanıtlar.

## <a name="transparent-versioning"></a>Saydam sürüm oluşturma
Herhangi bir zamanda desteklenmesi için bir API birden çok farklı uygulama sürümlerini yaygın bir uygulamadır. Örneğin, farklı ortamlarda (geliştirme, test, üretim, vb.) desteklemek için ya da daha yeni sürümüne geçirmek API Tüketiciler için zaman vermek için API eski sürümleri desteklemek için. 

Bu, URL'lerden değiştirmek istemci geliştiriciler istemek yerine işleme bir yaklaşım `/v1/customers` için `/v2/customers` şu anda istedikleri kullanın ve uygun arka uç URL'si çağırmak için API'ın hangi sürümü tüketicinin profil verileri depolamak için. Belirli bir istemci için çağırmak için doğru arka uç URL'si belirlemek için bazı yapılandırma verileri sorgulamak gereklidir. Bu yapılandırma verileri önbelleğe alarak API Management yaşanan bu Arama yapmanın performans sorunları en aza indirebilirsiniz.

İlk adım, istenen sürüm yapılandırmak için kullanılan tanımlayıcı belirlemektir. Bu örnekte, ürün abonelik anahtarı sürüme ilişkilendirilecek seçtiğim. 

```xml
<set-variable name="clientid" value="@(context.Subscription.Key)" />
```

API Management sonra önceden istenen istemci sürümü alınan olup olmadığını görmek için önbellek araması yapar.

```xml
<cache-lookup-value
key="@("clientversion-" + context.Variables["clientid"])"
variable-name="clientversion" />
```

Ardından, API Management, onu önbellekte bulamadı, olmadığını denetler.

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("clientversion"))">
```

API Management, bulamadıysanız, API Management alır.

```xml
<send-request
    mode="new"
    response-variable-name="clientconfiguresponse"
    timeout="10"
    ignore-error="true">
            <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
            <set-method>GET</set-method>
</send-request>
```

Yanıt gövdesi metni yanıttan ayıklayın.

```xml
<set-variable
      name="clientversion"
      value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
```

Gelecekte kullanım için önbelleğindeki depolar.

```xml
<cache-store-value
      key="@("clientversion-" + context.Variables["clientid"])"
      value="@((string)context.Variables["clientversion"])"
      duration="100000" />
```

Ve son olarak istemci tarafından istenen hizmetin sürümünü seçmek için arka uç URL'si güncelleştirin.

```xml
<set-backend-service
      base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
```

İlkenin tamamını aşağıdaki gibidir:

```xml
<inbound>
    <base />
    <set-variable name="clientid" value="@(context.Subscription.Key)" />
    <cache-lookup-value key="@("clientversion-" + context.Variables["clientid"])" variable-name="clientversion" />

    <!-- If API Management doesn’t find it in the cache, make a request for it and store it -->
    <choose>
        <when condition="@(!context.Variables.ContainsKey("clientversion"))">
            <send-request mode="new" response-variable-name="clientconfiguresponse" timeout="10" ignore-error="true">
                <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                <set-method>GET</set-method>
            </send-request>
            <!-- Store response body in context variable -->
            <set-variable name="clientversion" value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
            <!-- Store result in cache -->
            <cache-store-value key="@("clientversion-" + context.Variables["clientid"])" value="@((string)context.Variables["clientversion"])" duration="100000" />
        </when>
    </choose>
    <set-backend-service base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
</inbound>
```

Saydam güncelleştirin ve istemcilerin yeniden dağıtmak zorunda kalmadan, hangi arka uç sürümü istemciler tarafından erişiliyor denetlemek API tüketicileri etkinleştirme birçok API sürüm oluşturma sorunları ele zarif bir çözümdür.

## <a name="tenant-isolation"></a>Kiracı yalıtımı
Daha büyük, çok kiracılı dağıtımlarda bazı şirketler, arka uç donanım ayrı dağıtımlarına kiracılar ayrı grupları oluşturun. Bu arka uç üzerinde donanım sorundan etkilenen müşteriler sayısını en aza indirir. Ayrıca, yeni yazılım sürümleri aşamada alınması sağlar. İdeal olarak bu arka uç mimarisi API tüketicilere saydam olmalıdır. API anahtarı başına yapılandırma durumu kullanarak arka uç URL'si düzenleme aynı tekniği bağlı olduğu bu saydam sürüm benzer bir şekilde sağlanabilir.  

API her abonelik anahtarı için tercih edilen bir sürümünü döndürme yerine atanan donanım grubu için bir kiracı ilişkili tanımlayıcı döndürecektir. Bu tanımlayıcı, uygun arka uç URL'si oluşturmak için kullanılabilir.

## <a name="summary"></a>Özet
Her türlü veri depolamak için Azure API management önbellek kullanma özgürlüğü verimli bir gelen talep işlenen biçimini etkileyebilir yapılandırma verilerine erişim sağlar. Ayrıca, bir arka API döndürülen yanıt genişletebilirsiniz veri parçaları depolamak için de kullanılabilir.
