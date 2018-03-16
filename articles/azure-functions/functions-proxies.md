---
title: "Azure işlevleri proxy'leri ile çalışırsınız | Microsoft Docs"
description: "Azure işlevleri proxy'leri kullanma genel bakış"
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: cfowler
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/22/2018
ms.author: alkarche
ms.openlocfilehash: 0e7fe474c3b247baa6550770c661af62e83b3737
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="work-with-azure-functions-proxies"></a>Azure işlevleri proxy ile çalışma

Bu makalede, yapılandırma ve Azure işlevleri proxy'leri iş açıklanmaktadır. Bu özellik ile başka bir kaynak tarafından uygulanan işlevi uygulamanızdan uç noktalarını belirtebilirsiniz. İstemciler için tek bir API yüzeyi hala sunarken büyük bir API (olduğu gibi bir mikro Hizmet Mimarisi), birden çok işlev uygulamalarının bölüneceği bu proxy'leri kullanabilirsiniz.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE] 
> Faturalama standart işlevleri proxy yürütmeleri için geçerlidir. Daha fazla bilgi için bkz: [Azure işlevleri fiyatlandırma](https://azure.microsoft.com/pricing/details/functions/).

## <a name="create"></a>Bir proxy sunucu oluşturma

Bu bölümde işlevleri Portalı'nda bir proxy oluşturulacağını gösterir.

1. Açık [Azure portal]ve ardından işlevi uygulamanıza gidin.
2. Sol bölmede seçin **yeni proxy**.
3. Proxy için bir ad sağlayın.
4. Sunulan uç noktası belirterek bu işlev uygulaması Yapılandır **rota şablonu** ve **HTTP yöntemleri**. Bu parametreler için kurallara uygun davranır [HTTP Tetikleyicileri].
5. Ayarlama **arka uç URL'si** başka bir uç noktası. Bu uç noktaya başka bir işlev uygulaması işlevinde olabilir veya diğer herhangi bir API'yi olabilir. Değer statik olması gerekmez ve başvurusu yapabilir [uygulama ayarları] ve [özgün istemci istek parametrelerinden].
6. **Oluştur**’a tıklayın.

Proxy şimdi işlevi uygulamanızdan yeni bir uç noktası olarak bulunmaktadır. İstemci açısından bakıldığında, Azure işlevlerinde bir HttpTrigger eşdeğerdir. Proxy URL'si kopyalayarak ve sık kullanılan HTTP istemci ile test yeni proxy deneyebilirsiniz.

## <a name="modify-requests-responses"></a>İsteklerin ve yanıtların değiştirme

Azure işlevleri proxy'leriyle isteklerini ve yanıtlarını arka ucundan değiştirebilirsiniz. Bu dönüşümleri tanımlandığı gibi değişkenleri kullanabilirsiniz [değişkenlerini kullanın].

### <a name="modify-backend-request"></a>Arka uç isteği değiştirme

Varsayılan olarak, arka uç isteği özgün istek bir kopyası olarak başlatılır. Arka uç URL'si ayarlamaya ek olarak, HTTP yöntemi, üst bilgiler ve sorgu dizesi parametreleri değişiklik yapabilirsiniz. Değiştirilen değerleri başvurabilir [uygulama ayarları] ve [özgün istemci istek parametrelerinden].

Arka uç istekleri değiştirilebilir portalda tarafından expading *geçersiz kılma isteği* proxy ayrıntı sayfasının bölümünde. 

### <a name="modify-response"></a>Yanıtı değiştirebilir

Varsayılan olarak, istemci yanıt arka uç yanıtının kopya olarak başlatılır. Yanıtın durum kodu, neden ifadesini, üstbilgiler ve gövde değişiklik yapabilirsiniz. Değiştirilen değerleri başvurabilir [uygulama ayarları], [özgün istemci istek parametrelerinden], ve [arka uç yanıt parametrelerinden].

Arka uç istekleri değiştirilebilir portalda tarafından expading *yanıt geçersiz kılma* proxy ayrıntı sayfasının bölümünde. 

## <a name="using-variables"></a>Değişkenleri kullanma

Bir proxy sunucu yapılandırmasını statik olması gerekmez. Değişkenleri özgün istemci isteği, arka uç yanıt ya da uygulama ayarları kullanmak üzere koşulu.

### <a name="reference-localhost"></a>Başvuru yerel işlevler
Kullanabileceğiniz `localhost` aynı işlev uygulaması işlevinde bir gidiş dönüş proxy isteği bu olmadan doğrudan başvurmak için.

`"backendurl": "https://localhost/api/httptriggerC#1"` bir yerel yol tetiklenen HTTP işlevini başvurur. `/api/httptriggerC#1`

 
>[!Note]  
>İşlevinizi kullanıyorsa *işlevi, yönetici veya sys* yetkilendirme düzeyleri, kod ve ClientID, özgün işlevi URL'nin başına sağlamak gerekir. Bu durumda başvurusu gibi görünür: `"backendurl": "https://localhost/api/httptriggerC#1?code=<keyvalue>&clientId=<keyname>"`

### <a name="request-parameters"></a>Başvuru İstek parametreleri

İstek parametreleri giriş uç URL'si özelliği olarak veya isteklerin ve yanıtların değiştirerek bir parçası olarak kullanabilirsiniz. Bazı parametreler temel proxy yapılandırma dosyasında belirtilen rota şablondan bağlı olabilir ve diğerleri gelen istek özelliklerinden gelebilir.

#### <a name="route-template-parameters"></a>Rota şablon parametreleri
Rota şablonda kullanılan parametreleri adıyla başvurulması kullanılabilir. Parametre adı kaşlı ({}) içine alınır.

Örneğin, bir proxy gibi bir rota şablonu varsa `/pets/{petId}`, arka uç URL'si değerini içerebilir `{petId}`, olarak `https://<AnotherApp>.azurewebsites.net/api/pets/{petId}`. Rota şablonu bir joker karakter olarak gibi kesilip kesilmediğini `/api/{*restOfPath}`, değer `{restOfPath}` gelen istek kalan yol kesimleri dize gösterimidir.

#### <a name="additional-request-parameters"></a>Ek İstek parametreleri
Rota şablonu parametrelerine ek olarak, aşağıdaki değerleri yapılandırma değerleri kullanılabilir:

* **{request.method}** : Özgün istekte kullanılan HTTP yöntemi.
* **{request.headers. \<HeaderName\>}**: özgün istekteki veriye okunabilir bir üstbilgi. Değiştir  *\<HeaderName\>*  okumak istediğiniz üstbilgi adı. Üstbilgi isteğinde bulunmuyorsa, değer boş dize olacaktır.
* **{request.querystring.\<ParameterName\>}**: A query string parameter that can be read from the original request. Değiştir  *\<ParameterName\>*  okumak istediğiniz parametre adı. Parametre isteğinde bulunmuyorsa, değer boş dize olacaktır.

### <a name="response-parameters"></a>Başvuru arka uç yanıt parametreleri

Yanıt parametrelerinin istemciye yanıtına değiştirerek bir parçası olarak kullanılabilir. Aşağıdaki değerleri yapılandırma değerleri kullanılabilir:

* **{backend.response.statusCode}** : Arka uç yanıtta döndürülen HTTP durum kodu.
* **{backend.response.statusReason}** : Arka uç yanıtta döndürülen HTTP neden ifadesini.
* **{backend.response.headers. \<HeaderName\>}**: arka uç yanıttan okunabilir bir üstbilgi. Değiştir  *\<HeaderName\>*  okumak istediğiniz üstbilgi adı. Üstbilgi yanıtta dahil edilmezse, değer boş dize olacaktır.

### <a name="use-appsettings"></a>Başvuru uygulama ayarları

Ayrıca başvurabilir [işlev uygulaması için tanımlanan uygulama ayarları](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#develop) yüzde işaretleri (%) ayarı adı çevreleyen tarafından.

Örneğin, arka uç URL'sini *https://%ORDER_PROCESSING_HOST%/api/orders* "ORDER_PROCESSING_HOST ayarı değerle değiştirilen ORDER_PROCESSING_HOST %" olması gerekir.

> [!TIP] 
> Birden çok dağıtım varsa, arka uç ana bilgisayarlar için uygulama ayarları kullanın veya test ortamları. Bu şekilde, her zaman bu ortam için uygun arka uç için varsayılır olduğundan emin olabilirsiniz.

## <a name="debugProxies"></a>Proxy sorun giderme

Bayrak ekleyerek `"debug":true` herhangi proxy'sine, `proxies.json` hata ayıklama günlüğünü etkinleştirir. Günlükleri depolanır `D:\home\LogFiles\Application\Proxies\DetailedTrace` ve Gelişmiş araçlar (kudu) üzerinden erişilebilir. HTTP yanıtları de içerecek bir `Proxy-Trace-Location` günlük dosyasına erişmek için bir URL ile üstbilgisi.

İstemci tarafı proxy'sindeki ekleyerek ayıklayabilirsiniz bir `Proxy-Trace-Enabled` üstbilgi kümesine `true`. Bu da dosya sistemine bir izleme günlüğü ve yanıt üst bilgisi olarak dönüş izleme URL'si.

### <a name="block-proxy-traces"></a>Blok proxy izlemeleri

Güvenlik nedenleriyle herkesin bir izleme oluşturmak için hizmet çağırma istemeyebilirsiniz. Oturum açma kimlik bilgilerinizi olmadan izleme içeriğine erişmek seçebilecekler değil, ancak izleme oluşturma kaynağı tüketir ve işlevi proxy'leri kullandığınızı gösterir.

Ekleyerek izlemeleri tamamen devre dışı `"debug":false` herhangi belirli proxy'sine, `proxies.json`.

## <a name="advanced-configuration"></a>Gelişmiş yapılandırma

Yapılandırdığınız proxy depolanmış bir *proxies.json* bir işlev uygulaması dizin kökünde bulunan dosya. El ile bu dosyasını düzenleyin ve herhangi birini kullandığınızda, uygulamanızın bir parçası olarak dağıtmanız [dağıtım yöntemleri](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment) bu işlevleri destekler. 

> [!TIP] 
> Bir dağıtım yöntemleri ayarlamadıysanız ayrıca çalışabileceğiniz *proxies.json* portal dosyasında. Select, işlev uygulaması Git **Platform özellikleri**ve ardından **App Service Düzenleyicisi**. Bunu yaparak, tüm dosya yapısı işlevi uygulamanızın görüntülemek ve ardından değişiklikleri yapın.

*Proxies.JSON* adlandırılmış proxy'leri ve tanımlarını oluşan bir proxy nesnesi tarafından tanımlanır. İsteğe bağlı olarak, düzenleyicinizi destekliyorsa, başvurabileceğiniz bir [JSON şeması](http://json.schemastore.org/proxies) için kod tamamlama. Bir örnek dosyası aşağıdakine benzeyebilir:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>"
        }
    }
}
```

Her proxy gibi kolay bir ada sahip *proxy1* önceki örnekte. Karşılık gelen proxy tanımı nesnesi aşağıdaki özellikleri tarafından tanımlanır:

* **matchCondition**:--Bu proxy yürütülmesini tetiklemek istekleri tanımlayan bir nesne gerekli. İle paylaşılan iki özellikler içeren [HTTP Tetikleyicileri]:
    * _yöntemleri_: proxy yanıtlaması HTTP yöntemleri dizisi. Belirtilmezse, proxy tüm HTTP yöntemleri rotadaki yanıt verir.
    * _Rota_: gerekli--hangi URL'lerin proxy isteği denetlemek için rota şablonu tanımlar yanıt verir. Farklı olarak HTTP tetikleyicileri, varsayılan değer yoktur.
* **backendUri**: için istek olacağı yönlendirilirken arka uç kaynak URL'si. Bu değer, uygulama ayarları ve parametreleri özgün istemci istekten başvurabilirsiniz. Bu özellik dahil edilmezse, Azure işlevlerinin bir HTTP 200 Tamam ile yanıt verir.
* **requestOverrides**: dönüşümleri arka uç isteğine tanımlayan bir nesne. Bkz: [requestOverrides nesnesi tanımlayın].
* **responseOverrides**: dönüşümleri istemci yanıt olarak tanımlayan bir nesne. Bkz: [responseOverrides nesnesi tanımlayın].

> [!NOTE] 
> *Rota* Azure işlevleri proxy'leri özelliğinde dikkate değil *routeprefix öğesi* işlevi uygulama ana bilgisayar yapılandırma özelliği. Bir önek gibi dahil etmek istiyorsanız `/api`, içinde eklenmelidir *rota* özelliği.

### <a name="disableProxies"></a>Tek tek proxy'leri devre dışı bırak

Ekleyerek tek tek proxy'leri devre dışı bırakabilirsiniz `"disabled": true` proxy'sine `proxies.json` dosya. Bu, 404 döndürmek üzere matchCondidtion toplantı tüm istekleri neden olur.
```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "Root": {
            "disabled":true,
            "matchCondition": {
                "route": "/example"
            },
            "backendUri": "www.example.com"
        }
    }
}
```

### <a name="requestOverrides"></a>RequestOverrides nesnesi tanımlayın

RequestOverrides nesnesi isteği arka uç kaynak çağrıldığında yapılan değişiklikleri tanımlar. Nesne aşağıdaki özellikleri tarafından tanımlanır:

* **backend.Request.Method**: arka uç çağırmak için kullanılan HTTP yöntemi.
* **backend.request.querystring.\<ParameterName\>**: A query string parameter that can be set for the call to the back-end. Değiştir  *\<ParameterName\>*  ayarlamak istediğiniz parametre adı. Boş dize sağlanırsa, parametre arka uç isteği dahil edilmez.
* **backend.request.headers.\<HeaderName\>**: A header that can be set for the call to the back-end. Değiştir  *\<HeaderName\>*  ayarlamak istediğiniz üstbilgi adı. Boş dize sağlarsanız, üstbilgi arka uç isteği dahil edilmez.

Uygulama ayarları ve parametreleri değerleri özgün istemci istekten başvuruda bulunabilir.

Örnek bir yapılandırma aşağıdakine benzeyebilir:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>",
            "requestOverrides": {
                "backend.request.headers.Accept": "application/xml",
                "backend.request.headers.x-functions-key": "%ANOTHERAPP_API_KEY%"
            }
        }
    }
}
```

### <a name="responseOverrides"></a>ResponseOverrides nesnesi tanımlayın

RequestOverrides nesnesi istemciye geçirilen yanıta yapılan değişiklikleri tanımlar. Nesne aşağıdaki özellikleri tarafından tanımlanır:

* **response.statusCode**: istemciye döndürülecek HTTP durum kodu.
* **response.statusReason**: istemciye döndürülecek HTTP neden ifadesini.
* **Response.body**: istemciye döndürülecek gövde dize gösterimi.
* **Response.Headers. \<HeaderName\>**: istemci yanıtı için ayarlanan üstbilgi. Değiştir  *\<HeaderName\>*  ayarlamak istediğiniz üstbilgi adı. Boş dize sağlarsanız, üstbilgi yanıtta dahil edilmez.

Uygulama ayarları, özgün istemci İstek parametreleri ve parametreleri değerleri arka uç yanıttan başvuruda bulunabilir.

Örnek bir yapılandırma aşağıdakine benzeyebilir:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "responseOverrides": {
                "response.body": "Hello, {test}",
                "response.headers.Content-Type": "text/plain"
            }
        }
    }
}
```
> [!NOTE] 
> Bu örnekte, yanıt gövdesi doğrudan, böylece Hayır ayarlanmış `backendUri` özelliği gereklidir. Örnek API mocking için Azure işlevleri proxy'leri nasıl kullanacağınızı gösterir.

[Azure portal]: https://portal.azure.com
[HTTP Tetikleyicileri]: https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#http-trigger
[Modify the back-end request]: #modify-backend-request
[Modify the response]: #modify-response
[requestOverrides nesnesi tanımlayın]: #requestOverrides
[responseOverrides nesnesi tanımlayın]: #responseOverrides
[uygulama ayarları]: #use-appsettings
[değişkenlerini kullanın]: #using-variables
[özgün istemci istek parametrelerinden]: #request-parameters
[arka uç yanıt parametrelerinden]: #response-parameters
