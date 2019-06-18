---
title: Azure API Management'te özel önbelleğe alma
description: Azure API Management'ta anahtara göre öğeleri önbelleğe alınacağını öğrenin
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
ms.openlocfilehash: 922ab731ccd76e6a1336d61abe4b0251e358beb7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60780830"
---
# <a name="custom-caching-in-azure-api-management"></a>Azure API Management'te özel önbelleğe alma
Azure API Management hizmeti için yerleşik desteği vardır [HTTP yanıt önbelleğe alma](api-management-howto-cache.md) kaynak URL anahtar olarak kullanma. Anahtarı kullanarak istek üst bilgileri tarafından değiştirilebilir `vary-by` özellikleri. Tüm HTTP yanıtlarını (diğer adıyla gösterimleri) önbelleğe alma işlemi için yararlıdır, ancak bazen yalnızca önbelleğe gösterimi bir kısmını yararlı olur. Yeni [önbellek arama değeri](/azure/api-management/api-management-caching-policies#GetFromCacheByKey) ve [önbellek deposu değeri](/azure/api-management/api-management-caching-policies#StoreToCacheByKey) ilkeleri ilke tanımları içindeki verileri rastgele parçalarını depolanıp olanağı sağlar. Bu özellik ayrıca değeri daha önce sunulan ekler [gönderme isteği](/azure/api-management/api-management-advanced-policies#SendRequest) ilke çünkü artık dış hizmetlerden yanıtları önbelleğe alabilir.

## <a name="architecture"></a>Mimari
API Management hizmeti kullanan bir kiracı başına paylaşılan veri önbelleği, ölçeği birden çok birimi olarak hala aynı erişim elde etmeniz, verileri önbelleğe. Ancak, çok bölgeli dağıtımlar ile çalışırken, her bölge içinde bağımsız önbellekleri vardır. Önbellek yalnızca kaynağı bilgi parçasının olduğu bir veri deposu olarak davranmaya olmayan önemlidir. Yaptık ve çok bölgeli dağıtım yararlanmak daha sonra karar, müşterilerin yolculuk kullanıcılarla önbelleğe alınan verilerdeki erişimlerini kaybedebilir.

## <a name="fragment-caching"></a>Parça önbelleğe alma
Burada verilen yanıtları belirlemek pahalı ve makul bir süre için henüz yeni kalan verileri kısmı içeren bazı durumlar vardır. Örneğin, uçuş ayırmalar, uçuş durumu ile ilgili bilgi sağlayan bir havayolu tarafından oluşturulan bir hizmet göz önünde bulundurun. Kullanıcı airlines noktaları programının bir üyesi ise, geçerli durumlarını ilgili bilgileri de gerekir ve mesafe toplanır. Bu kullanıcı ile ilgili bilgileri, farklı bir sistem depolanabilir, ancak uçuş durumu ve rezervasyonları döndürülen yanıtların dahil istenebilir. Bu yapılabilir parça olarak adlandırılan bir işlem kullanarak. Birincil gösterimi tür belirtecinin kullanıcı ile ilgili bilgiler eklenecek olduğu belirtmek için kullanarak kaynak sunucudan döndürülebilir. 

Arka uç API'sini aşağıdaki JSON yanıtı göz önünde bulundurun.

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

Uygun kullanıcı bilgilerini dahil etmek için API Management belirlemek için son kullanıcının kim olduğunu belirlemek gerekir. Bu mekanizma uygulamaya bağlıdır. Örneğin, kullanıyorum `Subject` , talep bir `JWT` belirteci. 

```xml
<set-variable
  name="enduserid"
  value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />
```

API Management depoları `enduserid` daha sonra kullanmak için bir bağlam değişkeni değeri. Sonraki adım, önceki bir istek zaten kullanıcı bilgileri alınır ve önbellekte depolanır, belirlemektir. Bunun için API Yönetimi kullanan `cache-lookup-value` ilkesi.

```xml
<cache-lookup-value
key="@("userprofile-" + context.Variables["enduserid"])"
variable-name="userprofile" />
```

Anahtar değeri ve ardından Hayır karşılık gelen önbellek girişi yoksa `userprofile` bağlam değişkeni oluşturulur. API Management kullanarak arama başarısını denetler `choose` denetim akış ilkesi.

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("userprofile"))">
        <!-- If the userprofile context variable doesn’t exist, make an HTTP request to retrieve it.  -->
    </when>
</choose>
```

Varsa `userprofile` bağlam değişkeni yok ve API Management, almak için bir HTTP isteği yapmak zorunda geçmeye.

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

API Management kullanan `enduserid` kullanıcı profili kaynağı URL'sini oluşturmak için. API Management yanıtı aldığında yanıt dışında gövde metni çeker ve bir bağlam değişkeni depolar.

```xml
<set-variable
    name="userprofile"
    value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />
```

API Management aynı kullanıcının başka bir istekte bulunduğunda bu HTTP isteğinin yeniden yapmasını önlemek için önbelleğe kullanıcı profilini depolamak için belirtebilirsiniz.

```xml
<cache-store-value
    key="@("userprofile-" + context.Variables["enduserid"])"
    value="@((string)context.Variables["userprofile"])" duration="100000" />
```

API Management API Management ile almak için ilk olarak çalıştı. tam aynı anahtarı kullanarak önbelleğe değeri depolar. Değerini depolamak için API Management'ı seçer süresi hakkında dayanmalıdır genellikle bilgi, değişerek ve nasıl dayanıklı kullanıcılar için güncel bilgilerdir. 

Önbellekten yine de bir işlem dışı, ağ isteği ve potansiyel olarak milisaniye onlarca isteği eklemeye devam edebilirsiniz olduğunu bilmeniz önemlidir. Kullanıcı profili bilgileri, veritabanı sorguları veya birden çok arka uçları toplama bilgilerinden gerek kalmadan nedeniyle daha uzun sürer belirlerken avantajlar sunulur.

Son işlem döndürülen yanıtın kullanıcı profili bilgilerini güncelleştirmek için adımdır.

```xml
<!-- Update response body with user profile-->
<find-and-replace
    from='"$userprofile$"'
    to="@((string)context.Variables["userprofile"])" />
```

Seçtiğiniz Değiştir bile oluşmaz, yanıt hala geçerli bir JSON böylece belirtecinin bir parçası tırnak işaretleri eklemek için kullanabilirsiniz.  

Bu adımların tümünü birleştirmek sonra sonuç aşağıdaki gibi bir görünen bir ilkedir.

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

Önbelleğe alma bu yaklaşım, tek bir sayfa olarak işlenebilecek, HTML sunucu tarafında nereye oluşur web sitelerinde öncelikli olarak kullanılır. Ayrıca burada istemcilerin HTTP istemci tarafı önbelleğe alma yapamaz veya SORUMLULUĞUN istemcide put değil önerilir API'lerindeki yararlı olabilir.

Bu aynı tür parça önbelleğe alma, arka uç web sunucularında bir Redis sunucusunun önbellek kullanılarak yapılabilir, önbelleğe alınan parçaları farklı arka uçları birincil daha geldiğini ancak, bu çalışmayı gerçekleştirmek için API Management hizmeti kullanarak kullanışlıdır yanıtlar.

## <a name="transparent-versioning"></a>Saydam sürüm oluşturma
Herhangi bir zamanda desteklenmesi için bir API birden çok farklı sürümünü yaygın bir uygulamadır. Örneğin, farklı ortamlar (geliştirme, test, üretim, vs.) desteklemek için veya API tüketicilerini yeni sürümlere geçirmek için zaman vermek için API eski sürümlerini destekler. 

Bu, işleme yerine URL'lerden değiştirmek istemci geliştiriciler gerektiren bir yaklaşım `/v1/customers` için `/v2/customers` tüketicinin profil verileri şu anda istedikleri kullanın ve uygun arka uç URL'si çağırmak için API sürümünü depolamaktır. Belirli bir istemci için çağrılacak doğru arka uç URL'si belirlemek için bazı yapılandırma verileri sorgulamak gereklidir. Bu yapılandırma verileri önbelleğe alarak, API Management bu Arama yapmanın performans cezası en aza indirebilirsiniz.

İlk adım, istenen sürüm yapılandırmak için kullanılan tanımlayıcı belirlemektir. Bu örnekte, ürünün abonelik anahtarı sürümüne ilişkilendirilecek seçtim. 

```xml
<set-variable name="clientid" value="@(context.Subscription.Key)" />
```

API Management, ardından istenen istemci sürümü zaten geri olup olmadığını görmek için bir önbellek araması yapar.

```xml
<cache-lookup-value
key="@("clientversion-" + context.Variables["clientid"])"
variable-name="clientversion" />
```

Ardından, API Management, bu önbellekte bulunamadı, olmadığını denetler.

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("clientversion"))">
```

API yönetimi, API Management bulamadıysanız, alır.

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

Gelecekte kullanım için önbelleğindeki Store.

```xml
<cache-store-value
      key="@("clientversion-" + context.Variables["clientid"])"
      value="@((string)context.Variables["clientversion"])"
      duration="100000" />
```

Ve son olarak istemci tarafından istenen hizmetinin sürümü seçmek için arka uç URL'si güncelleştirin.

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

Şeffaf bir şekilde güncelleştirin ve istemcilerin yeniden dağıtmak zorunda kalmadan, hangi arka uç sürümü istemciler tarafından erişiliyor denetlemek API tüketicilerini etkinleştirme birçok API sürümü oluşturma konuları ele alan zarif bir çözümdür.

## <a name="tenant-isolation"></a>Kiracı yalıtımı
Daha büyük, çok kiracılı bir dağıtımda bazı şirketler, arka uç donanım farklı dağıtımlar üzerinde kiracıların ayrı grupları oluşturun. Bu, arka uçtaki bir donanım hatası tarafından etkilenen müşteriler sayısını en aza indirir. Ayrıca, yeni yazılım sürümleri aşamalar halinde alınmasına olanak sağlar. İdeal olarak bu arka uç mimarisi için API tüketicilerini saydam olmalıdır. API anahtarı başına yapılandırma durumu kullanarak arka uç URL'si düzenleme için aynı tekniği bağlı olduğu bu saydam sürüm oluşturma için benzer bir şekilde gerçekleştirilebilir.  

API'nin her bir abonelik anahtarı için tercih edilen bir sürümü döndürmek yerine, bir kiracı gruba atanan donanım ilişkili tanımlayıcı döndürecektir. Bu tanımlayıcı, uygun arka uç URL'si oluşturmak için kullanılabilir.

## <a name="summary"></a>Özet
Her tür veriyi depolamak için Azure API management önbelleği kullanma özgürlüğünü tanıyabilir gelen bir istek işlenmeden şeklini etkileyebilir yapılandırma verileri etkili erişim sağlar. Ayrıca, bir arka uç API döndürülen yanıt genişletebilirsiniz veri parçalarını depolamak için de kullanılabilir.
