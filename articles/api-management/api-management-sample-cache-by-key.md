---
title: "Azure API Management'te özel önbelleğe alma"
description: "Azure API Management'te anahtarı tarafından öğeleri önbelleğe öğrenin"
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 772bc8dd-5cda-41c4-95bf-b9f6f052bc85
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 4a41e4e0be44e855ead253ad76fe5a3af52070ec
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="custom-caching-in-azure-api-management"></a>Azure API Management'te özel önbelleğe alma
Azure API Management hizmeti için yerleşik destek sahip [HTTP yanıt önbelleğe alma](api-management-howto-cache.md) kaynak URL anahtar olarak kullanma. Anahtar kullanarak istek üstbilgileri tarafından değiştirilebilir `vary-by` özellikleri. Tüm HTTP yanıtlarını (diğer adıyla Beyanları) önbelleğe alma işlemi için yararlıdır, ancak bazen yalnızca önbellek temsili bir kısmı için yararlıdır. Yeni [önbellek arama değeri](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) ve [önbellek deposu değeri](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) ilkeleri, depolama ve ilke tanımları içindeki verileri rasgele parçalarını alma olanağı sağlar. Bu özellik ayrıca değeri önceden sunulan ekler [gönderme isteği](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) İlkesi çünkü dış hizmetler yanıtlarının şimdi önbelleğe alabilir.

## <a name="architecture"></a>Mimari
En fazla ölçek gibi birden çok birimi hala aynı erişimi alırsınız verileri önbelleğe böylece API Management hizmeti Kiracı başına paylaşılan veri önbelleği kullanır. Ancak, bölgeli dağıtımı ile çalışırken, her bölge içinde bağımsız önbellekleri vardır. Bunun nedeni, önbelleğe yalnızca kaynak bilgileri parçasının olduğu bir veri deposu olarak davranmanız değil önemlidir. Vermedi ve daha sonra bölgeli dağıtım yararlanmak karar, seyahat kullanıcılarla müşteriler bu önbelleğe alınmış verileri erişimlerini kaybedebilir.

## <a name="fragment-caching"></a>Parça önbelleğe alma
Burada verilen yanıtları belirlemek pahalıdır ve makul bir süre için henüz yeni kalan verileri kısmı içeren bazı durumlar vardır. Örnek olarak, Uçuş Rezervasyonları, uçuş durumu ile ilgili bilgi sağlayan bir uçak tarafından oluşturulmuş bir hizmeti kullanmayı düşünün. Kullanıcı airlines noktaları program üyesi ise, ayrıca bunların geçerli durumu ve toplanan mesafe ilgili bilgileri gerekir. Bu kullanıcı ile ilgili bilgiler farklı depolanabilir, ancak uçuş durumu ve ayırmaları hakkında döndürülen yanıtlar dahil istenebilir. Bu yapılabilir parça önbelleğe alma adlı bir işlem kullanılarak. Birincil gösterimi kullanıcı ile ilgili bilgiler eklenecek nerede belirtmek için bir tür belirteci kullanarak kaynak sunucudan döndürülebilir. 

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

Uygun kullanıcı bilgileri içerecek şekilde belirlemek için biz son kullanıcının kim olduğunu tanımlamanız gerekir. Bu mekanizma bağımlı uygulamasıdır. Örnek olarak, kullanıyorum `Subject` , talep bir `JWT` belirteci. 

```xml
<set-variable
  name="enduserid"
  value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />
```

Bu depolarız `enduserid` daha sonra kullanmak için bir bağlam değişken değeri. Sonraki adım, önceki bir istek daha önce kullanıcı bilgileri alınır ve önbellekte depolanan varsa belirlemektir. Bunun için kullandığımız `cache-lookup-value` ilkesi.

```xml
<cache-lookup-value
key="@("userprofile-" + context.Variables["enduserid"])"
variable-name="userprofile" />
```

Anahtar değeri sonra Hayır karşılık gelen önbellek girişi yoksa `userprofile` bağlamının oluşturulur. Biz arama kullanma başarısını denetleyin `choose` kontrol akışı ilkesi.

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("userprofile"))">
        <!-- If the userprofile context variable doesn’t exist, make an HTTP request to retrieve it.  -->
    </when>
</choose>
```

Varsa `userprofile` bağlamının yoksa, ardından bunu almak için bir HTTP isteği yapmak zorunda kalacaklarını.

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

Kullanırız `enduserid` kullanıcı profili kaynağı URL'si oluşturulamadı. Yanıt sahibiz sonra biz yanıt dışında gövde metni çekmek ve bir bağlam değişkende saklayın.

```xml
<set-variable
    name="userprofile"
    value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />
```

Bize aynı kullanıcı başka bir istekte bulunduğunda bu HTTP isteği yeniden sağlamak zorunda önlemek için biz kullanıcı profili önbellekte saklayabilirsiniz.

```xml
<cache-store-value
    key="@("userprofile-" + context.Variables["enduserid"])"
    value="@((string)context.Variables["userprofile"])" duration="100000" />
```

Biz başlangıçta ile almayı denedi tam aynı anahtarı kullanarak önbelleğinde değeri depolarız. Biz değeri depolamak üzere seçtiğiniz süre hakkında temel gereken genellikle bilgi değişikliklerini ve nasıl dayanıklı kullanıcılar için güncel bilgilerdir. 

Önbellekten hala olduğundan bir işlem dışı, ağ isteği ve potansiyel olarak milisaniye onlarca isteği eklemeye devam edebilirsiniz, hayata geçirmek önemlidir. Kullanıcı profili bilgilerini sorgular veya toplama bilgilerinden birden fazla arka uç veritabanı gerek nedeniyle daha önemli ölçüde daha uzun sürer belirlerken avantajları gelir.

Son işlem döndürülen yanıt bizim kullanıcı profili bilgilerle güncelleştirmek için adımdır.

```xml
<!-- Update response body with user profile-->
<find-and-replace
    from='"$userprofile$"'
    to="@((string)context.Variables["userprofile"])" />
```

Tırnak işaretleri belirtecinin bir parçası içermesi Değiştir bile gerçekleşmezse, yanıt hala geçerli JSON zamanki seçtiniz. Öncelikle daha kolay hata ayıklama yapmak için bu.

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

        <!-- If we don’t find it in the cache, make a request for it and store it -->
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

Önbelleğe alma bu yaklaşım, tek bir sayfa olarak işlenebilecek böylece burada HTML sunucu tarafında oluşur web siteleri, öncelikle kullanılır. Ancak, bu da yararlı olabilir burada istemcileri bulunulamaz istemci API'ları yan HTTP önbelleğe alma veya bu sorumluluğu istemcide değil yerleştirilecek istenen bir durumdur.

Parça önbelleğe alma bu aynı tür, arka uç web sunucularında sunucu önbelleği Redis kullanılarak yapılabilir, önbelleğe alınan parçaları birincil daha farklı arka uçları'ten gelen ancak API Management hizmeti kullanarak bu çalışmayı gerçekleştirmek için faydalıdır yanıtlar.

## <a name="transparent-versioning"></a>Saydam sürüm oluşturma
Herhangi bir zamanda desteklenmesi için bir API birden çok farklı uygulama sürümlerini yaygın bir uygulamadır. Bu belki de geliştirme, test, üretim, vb., gibi farklı ortamları desteklemek için veya daha yeni sürümüne geçirmek API Tüketiciler için zaman vermek için API eski sürümleri desteklemek için olabilir. 

Bu URL'lerden değiştirmek istemci geliştiriciler istemek yerine işleme bir yaklaşım `/v1/customers` için `/v2/customers` şu anda istedikleri kullanın ve uygun arka uç URL'si çağırmak için API'ın hangi sürümü tüketicinin profil verileri depolamak için. Belirli bir istemci için çağırmak için doğru arka uç URL'si belirlemek için bazı yapılandırma verileri sorgulamak gereklidir. Bu yapılandırma verileri önbelleğe alarak biz yaşanan bu Arama yapmanın performans sorunları en aza indirebilirsiniz.

İlk adım, istenen sürüm yapılandırmak için kullanılan tanımlayıcı belirlemektir. Bu örnekte, ürün abonelik anahtarı sürüme ilişkilendirilecek seçtiğim. 

```xml
<set-variable name="clientid" value="@(context.Subscription.Key)" />
```

Biz sonra biz zaten istenen istemci sürümü aldıktan olmadığını görmek için önbellek araması yapın.

```xml
<cache-lookup-value
key="@("clientversion-" + context.Variables["clientid"])"
variable-name="clientversion" />
```

Ardından biz biz bunu önbellekte bulamadı olmadığını denetleyin.

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("clientversion"))">
```

Git ve bunu alma sonra biz almadıysanız.

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

Tamamen ilke aşağıdaki gibidir.

```xml
<inbound>
    <base />
    <set-variable name="clientid" value="@(context.Subscription.Key)" />
    <cache-lookup-value key="@("clientversion-" + context.Variables["clientid"])" variable-name="clientversion" />

    <!-- If we don’t find it in the cache, make a request for it and store it -->
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

## <a name="next-steps"></a>Sonraki adımlar
Lütfen bize geri bildirim Disqus iş parçacığı için bu konuda bu ilkeler için etkinleştirdiğiniz diğer senaryolar varsa veya senaryoları varsa elde etmek ister misiniz verin ancak şu anda mümkün olan eşitleyerek değil.

