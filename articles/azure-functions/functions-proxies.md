---
title: Azure işlevleri proxy'leri ile çalışma | Microsoft Docs
description: Azure işlev proxy'lerini kullanmaya nasıl genel bakış
services: functions
author: alexkarcher-msft
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: alkarche
ms.openlocfilehash: 2fbf29385b9a14cf5d4a9df621f0767a32079587
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61021005"
---
# <a name="work-with-azure-functions-proxies"></a>Azure işlev proxy'leri ile çalışma

Bu makalede, yapılandırma ve Azure işlev proxy'lerini ile çalışmak açıklanmaktadır. Bu özellik ile başka bir kaynak tarafından uygulanan işlev uygulamanız üzerindeki uç noktaları belirtebilirsiniz. İstemciler için hala tek bir API yüzeyi sunarken büyük API birden fazla işlev uygulaması (olduğu gibi bir mikro Hizmet Mimarisi) uygulamasına ayırmak için bu proxy'ler kullanabilirsiniz.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE] 
> Faturalama standart işlevleri, proxy yürütme için geçerlidir. Daha fazla bilgi için [Azure işlevleri fiyatlandırması](https://azure.microsoft.com/pricing/details/functions/).

## <a name="create"></a>Bir ara sunucu oluşturma

Bu bölümde bir ara sunucu işlevleri Portalı'nda oluşturma işlemini gösterir.

1. Açık [Azure portal]ve ardından işlev uygulamanıza gidin.
2. Sol bölmede seçin **yeni proxy**.
3. Ara sunucunuz için bir ad belirtin.
4. Sunulan uç noktası belirterek bu işlev uygulamasını yapılandırma **rota şablonu** ve **HTTP yöntemleri**. Bu parametreler için kurallara göre davranır [HTTP Tetikleyicileri].
5. Ayarlama **arka uç URL'si** başka bir uç nokta için. Bu uç nokta, başka bir işlev uygulaması bir işlevde olabilir veya başka bir API olabilir. Değer statik olması gerekmez ve ona başvuran [uygulama ayarları] ve [Özgün istemci İstek parametreleri].
6. **Oluştur**’a tıklayın.

İşlev uygulamanız üzerinde yeni bir uç nokta olarak ara sunucunuz artık yok. İstemci açısından bakıldığında, Azure işlevleri'nde bir HttpTrigger eşdeğerdir. Proxy URL'si kopyalayarak ve sık kullanılan HTTP istemci ile test, yeni proxy deneyebilirsiniz.

## <a name="modify-requests-responses"></a>İsteklerin ve yanıtların değiştirme

Azure işlev proxy'lerini ile isteklerini ve arka uç alınan yanıtları değiştirebilirsiniz. Bu dönüştürmeler sınıfında tanımlandığı gibi değişkenleri kullanabilirsiniz [değişkenlerini kullanma].

### <a name="modify-backend-request"></a>Arka uç istek değiştirme

Varsayılan olarak, özgün istek bir kopyası olarak arka uç isteği başlatıldı. Arka uç URL'si ayarlamanın yanı sıra HTTP yöntemi, üst bilgiler ve sorgu dizesi parametreleri değişiklik yapabilirsiniz. Değiştirilmiş değerlere başvurabilirsiniz [uygulama ayarları] ve [Özgün istemci İstek parametreleri].

Arka uç istekleri değiştirilebilir portalda genişleterek *geçersiz kılma isteği* bölümünü proxy Ayrıntıları sayfası. 

### <a name="modify-response"></a>Yanıtı değiştirebilir

Varsayılan olarak, istemci yanıtı arka uç yanıtı bir kopyası olarak başlatılır. Yanıtın durum kodu, neden ifadesini, üstbilgi ve gövde değişiklik yapabilirsiniz. Değiştirilmiş değerlere başvurabilirsiniz [uygulama ayarları], [Özgün istemci İstek parametreleri], ve [arka uç yanıtı Parametreler].

Arka uç istekleri değiştirilebilir portalda genişleterek *geçersiz kılmayı Yanıtla* bölümünü proxy Ayrıntıları sayfası. 

## <a name="using-variables"></a>Değişkenlerini kullanma

Bir ara sunucu yapılandırmasını statik olması gerekmez. Değişkenler özgün istemci isteği, arka uç yanıtı ya da uygulama ayarları kullanacak şekilde koşul.

### <a name="reference-localhost"></a>Başvuru yerel işlevler
Kullanabileceğiniz `localhost` bir işlev aynı işlev uygulamasında bir gidiş dönüş proxy isteği doğrudan başvurmak için.

`"backendurl": "https://localhost/api/httptriggerC#1"` bir yerel yol HTTP ile tetiklenen işlevi başvurur. `/api/httptriggerC#1`

 
>[!Note]  
>İşlevinizi kullanıyorsa *işlevi, yönetici veya sys* yetkilendirme düzeyleri, kod ve ClientID, orijinal işlev URL'sini göre sağlamak gerekir. Bu durumda başvuru şöyle görünmelidir: `"backendurl": "https://localhost/api/httptriggerC#1?code=<keyvalue>&clientId=<keyname>"`

### <a name="request-parameters"></a>Başvuru İstek parametreleri

İstek parametreleri, giriş olarak arka uç URL'si özelliği veya isteklerini ve yanıtlarını değiştirme işleminin parçası olarak kullanabilirsiniz. Temel Ara sunucu yapılandırmasında belirtilen rota şablondaki bazı parametreler bağlanabilir ve diğerleri gelen istek özelliklerinden gelebilir.

#### <a name="route-template-parameters"></a>Şablon parametreleri yolu
Yol şablonunda kullanılan parametreleri adı tarafından başvurulan kullanılabilir. Parametre adları ayraç içine alınır ({}).

Örneğin, bir proxy gibi bir rota şablonuna sahip `/pets/{petId}`, arka uç URL'si değerini içerebilir `{petId}`, olarak `https://<AnotherApp>.azurewebsites.net/api/pets/{petId}`. Rota şablonu bir joker karakter olarak gibi kesilip kesilmediğini `/api/{*restOfPath}`, değer `{restOfPath}` kalan yol kesimleri gelen istek dize gösterimidir.

#### <a name="additional-request-parameters"></a>Ek İstek parametreleri
Rota şablonu parametrelerine ek olarak, yapılandırma değerleri aşağıdaki değerler kullanılabilir:

* **{request.method}** : Özgün istekte kullanılan HTTP yöntemi.
* **{request.headers. \<HeaderName\>}** : Özgün istekteki okunabilir üstbilgisi. Değiştirin *\<HeaderName\>* okumak istediğiniz üst bilgi adı. İstek üst bilgisi dahil edilmemişse, değer boş dize olacaktır.
* **{request.querystring. \<ParameterName\>}** : Özgün istekteki okunabilir bir sorgu dizesi parametresi. Değiştirin *\<ParameterName\>* ile okumak istediğiniz parametrenin adı. İstek parametre dahil edilmezse, boş bir dize değeri olacaktır.

### <a name="response-parameters"></a>Başvuru arka uç yanıtı parametreleri

Yanıt parametrelerinin, yanıtı istemciye değiştirme işleminin parçası olarak kullanılabilir. Yapılandırma değerleri aşağıdaki değerler kullanılabilir:

* **{backend.response.statusCode}** : Arka uç yanıtta döndürülen HTTP durum kodu.
* **{backend.response.statusReason}** : Arka uç yanıtta döndürülen HTTP neden deyimi.
* **{backend.response.headers. \<HeaderName\>}** : Arka uç yanıtı okuyabilen bir üst bilgisi. Değiştirin *\<HeaderName\>* okumak istediğiniz üst bilgi adı. Yanıt üst bilgisi dahil edilmemişse değeri boş dize olacaktır.

### <a name="use-appsettings"></a>Uygulama ayarları başvurusu

Ayrıca başvurabilirsiniz [uygulama ayarları işlev uygulaması için tanımlanan](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings) yüzde işaretleri (%) ayarı adıyla çevreleyen tarafından.

Örneğin, bir arka uç URL'si *https://%ORDER_PROCESSING_HOST%/api/orders* "% ORDER_PROCESSING_HOST % ORDER_PROCESSING_HOST ayarının değeri ile değiştirilmiştir" gerekir.

> [!TIP] 
> Birden çok dağıtım olduğunda uygulama ayarları için arka uç ana bilgisayarları kullanın veya test ortamları. Bu şekilde, her zaman bu ortam için doğru arka uca varsayılır, emin olabilirsiniz.

## <a name="debugProxies"></a>Proxy'leri sorunlarını giderme

Bayrağı ekleyerek `"debug":true` herhangi proxy'sine, `proxies.json` hata ayıklama günlük kaydını etkinleştirir. Günlükleri depolanır `D:\home\LogFiles\Application\Proxies\DetailedTrace` ve Gelişmiş araçlar (kudu) üzerinden erişilebilir. HTTP yanıtları de içeren bir `Proxy-Trace-Location` üstbilgi günlük dosyasına erişmek için bir URL.

İstemci tarafında bir ara sunucuya ekleyerek ayıklayabilirsiniz bir `Proxy-Trace-Enabled` başlığı ayarlayın `true`. Bu da dosya sistemine bir izleme günlüğü ve yanıt üst bilgi olarak izleme URL'sini döndürür.

### <a name="block-proxy-traces"></a>Blok proxy izlemeleri

Güvenlik nedenleriyle herkesin bir izleme oluşturmak için hizmetinizi çağırmadan istemeyebilirsiniz. Bunlar olmadan oturum açma kimlik bilgilerinizi izleme içeriğine erişmek mümkün olmayacaktır, ancak izleme oluşturma kaynaklarını tüketir ve işlev proxy'lerini kullandığınızı gösterir.

Ekleyerek izlemeleri tamamen devre dışı `"debug":false` herhangi belirli proxy'sine, `proxies.json`.

## <a name="advanced-configuration"></a>Gelişmiş yapılandırma

Yapılandırdığınız proxy depolanan bir *proxies.json* bir işlev uygulaması dizin kök dizininde bulunan dosya. El ile bu dosyasını düzenleyin ve herhangi birini kullandığınızda, uygulamanızın bir parçası olarak dağıtma [dağıtım yöntemleri](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment) bu işlevleri destekler. 

> [!TIP] 
> Dağıtım yöntemlerinden birini ayarlamadıysanız, ayrıca çalışabileceğiniz *proxies.json* Portalı'nda dosya. İşlevi uygulamanızı seçin Git **Platform özellikleri**ve ardından **App Service Düzenleyicisi**. Bunu yaptığınızda, işlev uygulamanızın tüm dosya yapısı görüntüleyebilir ve ardından değişiklikleri yapın.

*Proxies.JSON* adlandırılmış proxy ve bunların tanımlarının oluşan bir proxy nesnesi tarafından tanımlanır. İsteğe bağlı olarak, düzenleyici destekliyorsa, başvurabileceğiniz bir [JSON şeması](http://json.schemastore.org/proxies) için kod tamamlama. Bir örnek dosyası aşağıdakine benzeyebilir:

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

Her bir proxy gibi bir kolay ad sahip *proxy1* önceki örnekte. Karşılık gelen proxy tanım nesnesi, aşağıdaki özellikleri tarafından tanımlanır:

* **matchCondition**: --Gerekli bu proxy yürütülmesini tetiklemek istekleri tanımlayan nesne. İle paylaşılan iki özellik içeriyor [HTTP Tetikleyicileri]:
    * _yöntemleri_: Yanıt Ara sunucu HTTP yöntemleri dizisi. Belirtilmezse, proxy tüm HTTP yöntemleri rotadaki yanıt verir.
    * _Rota_: Gerekli--proxy, istek URL'lerini denetleme için rota şablonu tanımlar yanıt verir. Aksine HTTP tetikleyicileri, varsayılan değer yoktur.
* **backendUri**: İstek proxy edileceği için arka uç kaynak URL'si. Bu değer, uygulama ayarları ve parametreleri özgün istemci istekten başvurabilirsiniz. Bu özellik dahil edilmezse, Azure işlevleri, bir HTTP 200 OK ile yanıt verir.
* **requestOverrides**: Arka uç isteği dönüştürmeleri tanımlayan nesne. Bkz: [requestOverrides nesnesi tanımlayın].
* **responseOverrides**: İstemci yanıtı dönüştürmeleri tanımlayan nesne. Bkz: [responseOverrides nesnesi tanımlayın].

> [!NOTE] 
> *Rota* Azure işlev proxy'lerini bir özellik değil dikkate *routeprefix öğesi* işlevi uygulama konağı Yapılandırma özelliği. Bir önek gibi dahil etmek istiyorsanız `/api`, içinde eklenmelidir *rota* özelliği.

### <a name="disableProxies"></a> Bireysel proxy'leri devre dışı bırak

Ekleyerek bireysel proxy'leri devre dışı bırakabilirsiniz `"disabled": true` proxy'sine `proxies.json` dosya. Bu, 404 döndüren matchCondition toplantı tüm istekleri neden olur.
```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "Root": {
            "disabled":true,
            "matchCondition": {
                "route": "/example"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>"
        }
    }
}
```

### <a name="applicationSettings"></a> Uygulama ayarları

Birçok uygulama ayarları tarafından proxy davranışı denetlenebilir. Bunların tümü özetlenen [işlevler uygulama ayarları başvurusu](./functions-app-settings.md)

* [AZURE_FUNCTION_PROXY_DISABLE_LOCAL_CALL](./functions-app-settings.md#azure_function_proxy_disable_local_call)
* [AZURE_FUNCTION_PROXY_BACKEND_URL_DECODE_SLASHES](./functions-app-settings.md#azure_function_proxy_backend_url_decode_slashes)

### <a name="reservedChars"></a> Ayrılmış karakterleri (biçimlendirme dizesi)

Proxy'leri oku tüm dizeler dışında bir JSON dosyası, kullanarak \ kaçış sembol olarak. Proxy'leri, küme ayraçları ayrıca yorumlayın. Eksiksiz bir örneklere bakın.

|Karakter|Kaçan karakter|Örnek|
|-|-|-|
|{veya}|{{veya}}|`{{ example }}` --> `{ example }`
| \ | \\\\ | `example.com\\text.html` --> `example.com\text.html`
|"|\\\"| `\"example\"` --> `"example"`

### <a name="requestOverrides"></a>RequestOverrides nesnesi tanımlayın

RequestOverrides nesne isteği arka uç kaynağa çağrıldığında yapılan değişiklikleri tanımlar. Nesne, aşağıdaki özellikleri tarafından tanımlanır:

* **backend.Request.Method**: Arka uç çağırmak için kullanılan HTTP yöntemi.
* **backend.Request.QueryString. \<ParameterName\>** : Arka uca çağrısı için ayarlanabilir bir sorgu dizesi parametresi. Değiştirin *\<ParameterName\>* ile belirlemek istediğiniz parametrenin adı. Boş dize sağlanmazsa, parametre arka uç isteği dahil edilmez.
* **backend.Request.Headers. \<HeaderName\>** : Arka uca çağrısı için ayarlanabilir üstbilgisi. Değiştirin *\<HeaderName\>* ayarlamak istediğiniz üst bilgi adı. Boş bir dize sağlayın, üst bilgi arka uç isteği dahil edilmez.

Değerleri uygulama ayarları ve parametreleri özgün istemci istekten başvurabilirsiniz.

Örnek bir yapılandırma aşağıdaki gibi görünebilir:

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

İstemciye geri geçirilen yanıta yapılan değişiklikleri requestOverrides nesneyi tanımlar. Nesne, aşağıdaki özellikleri tarafından tanımlanır:

* **response.statusCode**: İstemciye döndürülecek HTTP durum kodu.
* **response.statusReason**: İstemciye döndürülecek HTTP neden deyimi.
* **Response.body**: İstemciye döndürülecek gövdesini dize gösterimi.
* **Response.Headers. \<HeaderName\>** : Yanıtı istemciye için ayarlanabilir üstbilgisi. Değiştirin *\<HeaderName\>* ayarlamak istediğiniz üst bilgi adı. Boş bir dize sağlayın, üst bilgi yanıtta dahil edilmez.

Uygulama ayarları, özgün istemci İstek parametreleri ve parametre değerlerini arka uç yanıtı başvuruda bulunabilir.

Örnek bir yapılandırma aşağıdaki gibi görünebilir:

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
> Bu örnekte, yanıt gövdesi doğrudan, dolayısıyla no ayarlanmış `backendUri` özelliği gereklidir. Örnek API sahte işlem için Azure işlevleri proxy'leri nasıl kullanacağınızı gösterir.

[Azure portal]: https://portal.azure.com
[HTTP Tetikleyicileri]: https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook
[Modify the back-end request]: #modify-backend-request
[Modify the response]: #modify-response
[RequestOverrides nesnesi tanımlayın]: #requestOverrides
[ResponseOverrides nesnesi tanımlayın]: #responseOverrides
[Uygulama ayarları]: #use-appsettings
[Değişkenlerini kullanma]: #using-variables
[Özgün istemci İstek parametreleri]: #request-parameters
[arka uç yanıtı Parametreler]: #response-parameters
