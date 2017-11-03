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
ms.date: 04/11/2017
ms.author: alkarche
ms.openlocfilehash: d201c8395adf47fa3d9f790b77b1d29dda5a0aeb
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2017
---
# <a name="work-with-azure-functions-proxies-preview"></a>Azure işlevleri proxy'leri (Önizleme) ile çalışma

> [!NOTE] 
> Azure işlevleri proxy'leri şu anda önizlemede değil. Önizleme, ancak standart işlevleri faturalama proxy yürütmeleri uygularken ücretsizdir. Daha fazla bilgi için bkz: [Azure işlevleri fiyatlandırma](https://azure.microsoft.com/pricing/details/functions/).

Bu makalede, yapılandırma ve Azure işlevleri proxy'leri iş açıklanmaktadır. Bu özellik ile başka bir kaynak tarafından uygulanan işlevi uygulamanızdan uç noktalarını belirtebilirsiniz. İstemciler için tek bir API yüzeyi hala sunarken büyük bir API (olduğu gibi bir mikro Hizmet Mimarisi), birden çok işlev uygulamalarının bölüneceği bu proxy'leri kullanabilirsiniz.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="create"></a>Bir proxy sunucu oluşturma

Bu bölümde işlevleri Portalı'nda bir proxy oluşturulacağını gösterir.

1. Açık [Azure portal]ve ardından işlevi uygulamanıza gidin.
2. Sol bölmede seçin **yeni proxy**.
3. Proxy için bir ad sağlayın.
4. Sunulan uç noktası belirterek bu işlev uygulaması Yapılandır **rota şablonu** ve **HTTP yöntemleri**. Bu parametreler için kurallara uygun davranır [HTTP Tetikleyicileri].
5. Ayarlama **arka uç URL'si** başka bir uç noktası. Bu uç noktaya başka bir işlev uygulaması işlevinde olabilir veya diğer herhangi bir API'yi olabilir. Değer statik olması gerekmez ve başvurusu yapabilir [uygulama ayarları] ve [özgün istemci istek parametrelerinden].
6. **Oluştur**'a tıklayın.

Proxy şimdi işlevi uygulamanızdan yeni bir uç noktası olarak bulunmaktadır. İstemci açısından bakıldığında, Azure işlevlerinde bir HttpTrigger eşdeğerdir. Proxy URL'si kopyalayarak ve sık kullanılan HTTP istemci ile test yeni proxy deneyebilirsiniz.

## <a name="modify-requests-responses"></a>İsteklerin ve yanıtların değiştirme

Azure işlevleri proxy'leriyle isteklerini ve yanıtlarını arka uçtan değiştirebilirsiniz. Bu dönüşümleri tanımlandığı gibi değişkenleri kullanabilirsiniz [değişkenlerini kullanın].

### <a name="modify-backend-request"></a>Arka uç isteği değiştirme

Varsayılan olarak, arka uç isteği özgün istek bir kopyası olarak başlatılır. Arka uç URL'si ayarlamaya ek olarak, HTTP yöntemi, üst bilgiler ve sorgu dizesi parametreleri değişiklik yapabilirsiniz. Değiştirilen değerleri başvurabilir [uygulama ayarları] ve [özgün istemci istek parametrelerinden].

Şu anda arka uç istekleri değiştirmek için hiçbir portal deneyimi yoktur. Bu yetenek proxies.json uygulamak öğrenmek için bkz: [requestOverrides nesnesi tanımlayın].

### <a name="modify-response"></a>Yanıtı değiştirebilir

Varsayılan olarak, istemci yanıt arka uç yanıtının kopya olarak başlatılır. Yanıtın durum kodu, neden ifadesini, üstbilgiler ve gövde değişiklik yapabilirsiniz. Değiştirilen değerleri başvurabilir [uygulama ayarları], [özgün istemci istek parametrelerinden], ve [arka uç yanıt parametrelerinden].

Şu anda, yanıtları değiştirmek için hiçbir portal deneyimi yoktur. Bu yetenek proxies.json uygulamak öğrenmek için bkz: [responseOverrides nesnesi tanımlayın].

## <a name="using-variables"></a>Değişkenleri kullanma

Bir proxy sunucu yapılandırmasını statik olması gerekmez. İlk istek, arka uç yanıt ya da uygulama ayarları değişkenleri kullanmak üzere koşulu.

### <a name="request-parameters"></a>Başvuru İstek parametreleri

İstek parametreleri giriş uç URL'si özelliği olarak veya isteklerin ve yanıtların değiştirerek bir parçası olarak kullanabilirsiniz. Bazı parametreler temel proxy yapılandırma dosyasında belirtilen rota şablondan bağlı olabilir ve diğerleri gelen istek özelliklerinden gelebilir.

#### <a name="route-template-parameters"></a>Rota şablon parametreleri
Rota şablonda kullanılan parametreleri adıyla başvurulması kullanılabilir. Parametre adı kaşlı ({}) içine alınır.

Örneğin, bir proxy gibi bir rota şablonu varsa `/pets/{petId}`, arka uç URL'si değerini içerebilir `{petId}`, olarak `https://<AnotherApp>.azurewebsites.net/api/pets/{petId}`. Rota şablonu bir joker karakter olarak gibi kesilip kesilmediğini `/api/{*restOfPath}`, değer `{restOfPath}` gelen istek kalan yol kesimleri dize gösterimidir.

#### <a name="additional-request-parameters"></a>Ek İstek parametreleri
Rota şablonu parametrelerine ek olarak, aşağıdaki değerleri yapılandırma değerleri kullanılabilir:

* **{request.method}** : Özgün istekte kullanılan HTTP yöntemi.
* **{request.headers. \<HeaderName\>}**: özgün istekteki veriye okunabilir bir üstbilgi. Değiştir  *\<HeaderName\>*  okumak istediğiniz üstbilgi adı. Üstbilgi isteğinde bulunmuyorsa, değer boş dize olacaktır.
* **{request.querystring. \<ParameterName\>}**: özgün istekteki veriye okunabilir bir sorgu dizesi parametresi. Değiştir  *\<ParameterName\>*  okumak istediğiniz parametre adı. Parametre isteğinde bulunmuyorsa, değer boş dize olacaktır.

### <a name="response-parameters"></a>Başvuru arka uç yanıt parametreleri

Yanıt parametrelerinin istemciye yanıtına değiştirerek bir parçası olarak kullanılabilir. Aşağıdaki değerleri yapılandırma değerleri kullanılabilir:

* **{backend.response.statusCode}** : Arka uç yanıtta döndürülen HTTP durum kodu.
* **{backend.response.statusReason}** : Arka uç yanıtta döndürülen HTTP neden ifadesini.
* **{backend.response.headers. \<HeaderName\>}**: arka uç yanıttan okunabilir bir üstbilgi. Değiştir  *\<HeaderName\>*  okumak istediğiniz üstbilgi adı. Üstbilgi isteğinde bulunmuyorsa, değer boş dize olacaktır.

### <a name="use-appsettings"></a>Başvuru uygulama ayarları

Ayrıca başvurabilir [işlev uygulaması için tanımlanan uygulama ayarları](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#develop) yüzde işaretleri (%) ayarı adı çevreleyen tarafından.

Örneğin, arka uç URL'sini *https://%ORDER_PROCESSING_HOST%/api/orders* "ORDER_PROCESSING_HOST ayarı değerle değiştirilen ORDER_PROCESSING_HOST %" olması gerekir.

> [!TIP] 
> Birden çok dağıtım varsa, arka uç ana bilgisayarlar için uygulama ayarları kullanın veya test ortamları. Bu şekilde, her zaman bu ortam için sağ arka ucuna varsayılır olduğundan emin olabilirsiniz.

## <a name="advanced-configuration"></a>Gelişmiş yapılandırma

Yapılandırdığınız proxy işlevi uygulama dizin kökünde bulunan bir proxies.json dosyasında depolanır. El ile bu dosyasını düzenleyin ve herhangi birini kullandığınızda, uygulamanızın bir parçası olarak dağıtmanız [dağıtım yöntemleri](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment) bu işlevleri destekler. Özellik olmalıdır [etkin](#enable) işlenecek dosya için. 

> [!TIP] 
> Bir dağıtım yöntemleri ayarlamadıysanız portalında proxies.json dosyayla çalışabilirsiniz. Select, işlev uygulaması Git **Platform özellikleri**ve ardından **App Service Düzenleyicisi**. Bunu yaparak, tüm dosya yapısı işlevi uygulamanızın görüntülemek ve ardından değişiklikleri yapın.

Proxies.JSON adlandırılmış proxy'leri ve tanımlarını oluşan bir proxy nesnesi tarafından tanımlanır. İsteğe bağlı olarak, düzenleyicinizi destekliyorsa, başvurabileceğiniz bir [JSON şeması](http://json.schemastore.org/proxies) için kod tamamlama. Bir örnek dosyası aşağıdakine benzeyebilir:

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
> Rota Özelliği Azure işlevleri proxy'leri işlevleri ana bilgisayar yapılandırması routeprefix öğesi özelliği dikkate almaz. /Api gibi bir önek dahil etmek istiyorsanız rota özelliğinde eklenmesi gerekir.

### <a name="requestOverrides"></a>RequestOverrides nesnesi tanımlayın

RequestOverrides nesnesi isteği arka uç kaynak çağrıldığında yapılan değişiklikleri tanımlar. Nesne aşağıdaki özellikleri tarafından tanımlanır:

* **backend.Request.Method**: arka uç çağırmak için kullanılan HTTP yöntemi.
* **backend.Request.QueryString. \<ParameterName\>**: çağrısı arka uç için ayarlanmış bir sorgu dizesi parametresi. Değiştir  *\<ParameterName\>*  ayarlamak istediğiniz parametre adı. Boş dize sağlanırsa, parametre arka uç isteği dahil edilmez.
* **backend.Request.Headers. \<HeaderName\>**: çağrısı arka uç için ayarlanabilecek üstbilgi. Değiştir  *\<HeaderName\>*  ayarlamak istediğiniz üstbilgi adı. Boş dize sağlarsanız, üstbilgi arka uç isteği dahil edilmez.

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
> Bu örnekte, gövde doğrudan, bu nedenle Hayır ayarlı `backendUri` özelliği gereklidir. Örnek API mocking için Azure işlevleri proxy'leri nasıl kullanacağınızı gösterir.

## <a name="enable"></a>Azure işlevleri proxy'leri etkinleştir

Proxy'leri artık varsayılan olarak etkin olan! Proxy'leri Önizleme daha eski bir sürümü kullanan ve proxy'ler devre dışı, el ile bir kez çalıştırılacak proxy'leri için sırayla proxy'leri etkinleştirmeniz gerekir.

1. Açık [Azure portal]ve ardından işlevi uygulamanıza gidin.
2. Seçin **işlev uygulaması ayarları**.
3. Anahtar **etkinleştirmek Azure işlevleri proxy'leri (Önizleme)** için **üzerinde**.

Ayrıca burada yeni özellikleri kullanılabilir duruma geldiğinde proxy çalışma zamanı güncelleştirmesi dönebilirsiniz.

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
