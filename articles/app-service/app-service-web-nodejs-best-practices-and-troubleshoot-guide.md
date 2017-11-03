---
title: "En iyi yöntemler ve Azure Web uygulamaları üzerinde düğümü uygulamalar için sorun giderme kılavuzu"
description: "En iyi yöntemler ve sorun giderme adımları düğümü uygulamaları için Azure Web Apps öğrenin."
services: app-service\web
documentationcenter: nodejs
author: ranjithr
manager: wadeh
editor: 
ms.assetid: 387ea217-7910-4468-8987-9a1022a99bef
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 06/06/2016
ms.author: ranjithr
ms.openlocfilehash: 8f39b5e6faf5f9121ec2abe347f50653c5c3e4f9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-web-apps"></a>En iyi yöntemler ve Azure Web uygulamaları üzerinde düğümü uygulamalar için sorun giderme kılavuzu
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Bu makalede, en iyi yöntemler ve sorun giderme adımları için öğreneceksiniz [düğümü uygulamaları](app-service-web-get-started-nodejs.md) Azure Webapps üzerinde çalışan (ile [iisnode](https://github.com/azure/iisnode)).

> [!WARNING]
> Sorun giderme adımlarını üretim sitenizde kullanırken dikkatli olun. Uygulamanızı bir üretim dışı kurulumunu örneğin hazırlama yuvası sorun giderme ve sorun düzeltildiğinde, hazırlama yuvası, üretim yuvası ile takas önerilir.
> 
> 

## <a name="iisnode-configuration"></a>IISNODE yapılandırma
Bu [şema dosyası](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) için ıısnode yapılandırılabilir tüm ayarları gösterilmektedir. Uygulamanız için yararlı olacak ayarlardan bazıları şunlardır:

* nodeProcessCountPerApplication
  
    Bu ayar IIS uygulaması başlatılan düğüm işlem sayısını denetler. Varsayılan değer 1'dir. Bu ayarlayarak, VM çekirdek sayısı sayıda node.exe ait başlatabilirsiniz 0. Tüm çekirdek makinenizde kullanabilir için önerilen değeri çoğu uygulama için 0'dır. Node.exe tek iş parçacıklı bir node.exe 1 çekirdek ve tüm çekirdek kullanmak istediğiniz düğümü uygulamanızı dışında en yüksek performans almak için maksimum kullanacak şekilde.
* nodeProcessCommandLine
  
    Bu ayar, node.exe yolunu denetler. Bu değer, node.exe sürümüne işaret edecek şekilde ayarlayabilirsiniz.
* maxConcurrentRequestsPerProcess
  
    Bu ayar her node.exe iisnode tarafından gönderilen eşzamanlı istek sayısını denetler. Azure webapps üzerinde bu için varsayılan değer sonsuzdur. Bu ayar hakkında endişelenmeniz gerekmez. Azure webapps dışında varsayılan değer 1024'tür. Bu, uygulamanızın alır kaç ister ve ne kadar hızlı uygulamanızı her isteğini işler bağlı olarak yapılandırmak isteyebilirsiniz.
* maxNamedPipeConnectionRetry
  
    Bu ayar, en fazla kaç kez iisnode için node.exe üzerinden isteği göndermek için adlandırılmış kanal yapmayı bağlantıda deneyecek denetler. Bu ayar namedPipeConnectionRetryDelay birlikte her istek iisnode içinde toplam zaman aşımı belirler. Varsayılan değer Azure Webapps üzerinde 200'dür. Zaman aşımı saniye cinsinden toplam = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000
* namedPipeConnectionRetryDelay
  
    Bu ayar denetimleri süresi (ms) iisnode miktarı isteği node.exe için adlandırılmış kanal üzerinden göndermek için her yeniden arasında bekler. 250ms varsayılan değerdir.
    Zaman aşımı saniye cinsinden toplam = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000
  
    Varsayılan olarak azure webapps üzerinde iisnode toplam zaman aşımı 200 olan \* 250ms = 50 saniye.
* logDirectory
  
    Bu ayar, iisnode stdout/stderr burada oturum dizin denetler. Ana komut dosyası dizini (ana server.js mevcut olduğu dizin) göre olan iisnode varsayılan değer:
* debuggerExtensionDll
  
    Bu ayar, node-Inspector iisnode hangi sürümünün düğümü uygulamanızın hatalarını ayıklama kullanacağı denetler. Şu anda iisnode denetçisi 0.7.3.dll ve iisnode inspector.dll Bu ayar için yalnızca 2 geçerli değerler şunlardır. İisnode denetçisi 0.7.3.dll varsayılan değerdir. iisnode denetçisi 0.7.3.dll sürüm düğümü denetçisi 0.7.3 ve bu sürümü kullanmak için azure webapp üzerinde websockets etkinleştirmeniz gerekir böylece websockets, kullanır. Bkz: <http://www.ranjithr.com/?p=98> iisnode yeni node-Inspector kullanacak şekilde yapılandırma hakkında daha fazla ayrıntı için.
* flushResponse
  
    Yanıt veri arabelleği IIS varsayılan davranışını olan yukarı 4 MB temizleme önce ya da yanıt sonuna kadar hangisi önce gelirse. iisnode bu davranışı geçersiz kılmak için bir yapılandırma ayarı sunar: isteğe bağlı olarak iisnode node.exe alır almaz yanıt Varlık gövdesi bir parçasını temizlemek için ayarlamanız gerekir iisnode/@flushResponse özniteliği 'true' web.config dosyasında:
  
    ```
    <configuration>    
        <system.webServer>    
            <!-- ... -->    
            <iisnode flushResponse="true" />    
        </system.webServer>    
    </configuration>
    ```
  
    Yanıt Varlık gövdesi her parçasında temizleme etkinleştirilmesi ekler ~ %5 (itibariyle v0.1.13) tarafından sistemin verimliliğini azaltır performansa yanıt akış gerektiren uç noktaları için bu ayarı kapsamı en iyisidir (örneğin kullanarak<location>web.config öğesinde)
  
    Buna ek olarak, uygulamalar, akış için aynı zamanda iisnode işleyicinizi responseBufferLimit 0 olarak ayarlamak gerekir.
  
    ```
    <handlers>    
        <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>    
    </handlers>
    ```
* watchedFiles
  
    Bu değişiklikleri izlenen dosyaların noktalı virgülle ayrılmış listesidir. Bir dosyayı yapılan bir değişikliği geri dönüştürmek uygulamanın neden olur. Her Giriş bir isteğe bağlı dizin adı ve ana uygulama giriş noktası bulunduğu dizine göre olan gerekli dosya adına sahiptir. Joker karakterler yalnızca dosya adı bölümünde izin verilir. Varsayılan değer "\*. js;web.config"
* recycleSignalEnabled
  
    Varsayılan değer false şeklindedir. Etkinleştirilirse, düğüm uygulamanız için bir adlandırılmış kanal bağlanabilir (ortam değişkeni IISNODE\_denetim\_kanal) ve "Geri Dönüşüm" iletisi gönderin. Bu, düzgün biçimde geri dönüştürmek w3wp neden olur.
* idlePageOutTimePeriod
  
    Varsayılan değer bu özellik devre dışı yani 0'dır. Bazı değeri 0'dan büyük olarak ayarlandığında, iisnode tüm alt işlemleri her 'idlePageOutTimePeriod' milisaniyede sayfa. Ne anlamına gelir sayfasında anlamak için lütfen bu başvurun [belgelerine](https://msdn.microsoft.com/library/windows/desktop/ms682606.aspx). Bu ayar, çok fazla bellek kullanmasına ve diske pageout bellek bazen bazı RAM boşaltmak için istediğiniz uygulamalar için yararlı olacaktır.

> [!WARNING]
> Aşağıdaki yapılandırma ayarları üretim uygulamaları etkinleştirirken dikkatli olun. Bunları dinamik üretim uygulamaları etkinleştirmemeniz önerilir.
> 
> 

* debugHeaderEnabled
  
    Varsayılan değer false. True, iisnode kümesine her HTTP yanıt olarak bir HTTP yanıt üstbilgisi iisnode-debug ekleyeceğinizi iisnode-debug üstbilgi değeri bir URL'dir gönderir. Tanılama bilgileri tek tek parçaları URL parçası bakarak konusunda, ancak daha iyi görselleştirme URL tarayıcıda açarak elde edilir.
* loggingEnabled
  
    Bu ayar tarafından iisnode stdout ve stderr günlüğe kaydedilmesini denetler. Iisnode stdout/stderr başlatılır düğümü işlemlerden yakalamak ve 'logDirectory' ayarında belirtilen dizine yazılamıyor. Etkinleştirme sonra uygulamanızı günlükleri dosya sistemi ve uygulama tarafından gerçekleştirilen günlük miktarına bağlı olarak yazacaksınız, performans etkileri olabilir.
* devErrorsEnabled
  
    Varsayılan değer false şeklindedir. Ne zaman true iisnode kümesine tarayıcınızdaki HTTP durum kodu ve Win32 hata kodu görüntülenir. Win32 kodu belirli türde bir sorunları hata ayıklamaya yardımcı olacaktır.
* debuggingEnabled (Canlı üretim sitesinde etkinleştirmeyin.)
  
    Bu ayar, hata ayıklama özelliği denetler. Iisnode node-Inspector ile tümleşiktir. Bu ayarın etkinleştirilmesi, düğüm uygulamanızın veya hata ayıklamayı etkinleştir. Bu ayar etkinleştirildiğinde, iisnode düğümü uygulamanıza ilk hata ayıklama isteği 'debuggerVirtualDir' dizinindeki gerekli node-Inspector dosyalarda düzeni olur. Node-Inspector http://yoursite/server.js/debug için bir istek göndererek yükleyebilirsiniz. Hata ayıklama URL kesimi 'debuggerPathSegment' ayarıyla denetleyebilirsiniz. Varsayılan debuggerPathSegment tarafından = 'debug'. Böylece başkaları tarafından bulunmak daha zor olduğundan bu örnek için bir GUID ayarlayabilirsiniz.
  
    Bu kontrol [bağlantı](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) hata ayıklama hakkında daha fazla bilgi.

## <a name="scenarios-and-recommendationstroubleshooting"></a>Senaryolar ve öneriler ve giderme
### <a name="my-node-application-is-making-too-many-outbound-calls"></a>Düğüm Uygulamam çok fazla giden çağrıları yapma.
Birçok uygulama, normal işlemlerinin bir parçası olarak giden bağlantılar oluşturmak istersiniz. Örneğin, bir istek geldiğinde, başka bir yerde bir REST API başvurun ve isteğini işlemek için bazı bilgiler almak düğüm uygulamanızı istersiniz. Http veya https çağrıları yapılırken canlı tutma aracı kullanmak isteyebilirsiniz. Örneğin, bu giden çağrıları yapılırken canlı tutma aracısı olarak agentkeepalive modülü kullanabilirsiniz. Bu yuva, azure webapp VM yeniden kullanıldığı emin olur ve giden her istek için yeni yuva oluşturma yükünü azaltma. Ayrıca, bu sayıda giden istek yapmak için daha az sayıda yuva kullanıyorsanız ve bu nedenle VM başına ayrılmış maxSockets aşmayan emin olur. Azure Webapps öneriye agentKeepAlive maxSockets değeri 160 yuva VM başına toplam ayarlamasını olacaktır. Bu VM'de çalıştırılan 4 node.exe varsa, agentKeepAlive maxSockets 160 VM başına toplam olan node.exe başına 40 ayarlamak istediğinizi anlamına gelir.

Örnek [agentKeepALive](https://www.npmjs.com/package/agentkeepalive) yapılandırma:

```
var keepaliveAgent = new Agent({    
    maxSockets: 40,    
    maxFreeSockets: 10,    
    timeout: 60000,    
    keepAliveTimeout: 300000    
});
```

Bu örnek, VM'de çalıştırılan 4 node.exe olduğunu varsayar. VM çalıştıran node.exe farklı sayıda varsa, uygun şekilde ayarlama maxSockets değiştirmek gerekecektir.

### <a name="my-node-application-is-consuming-too-much-cpu"></a>Düğüm Uygulamam çok fazla CPU tüketilmesine neden olabilir.
Yüksek cpu tüketimi hakkında portalınızdaki Azure Webapps bir öneri büyük olasılıkla alırsınız. Belirli izlemek için izleyiciler de ayarlayabilirsiniz [ölçümleri](web-sites-monitor.md). CPU kullanımı üzerinde denetlerken [Azure portalı panosunun](../application-insights/app-insights-web-monitor-performance.md), lütfen yoğun değerlerin kaçırmamak için en büyük değer CPU denetleyin.
Burada, uygulamanızın çok fazla CPU kullanan ve nedenini açıklayan olamaz düşündüğünüz durumda düğüm uygulamanıza profil gerekecektir.

### 
#### <a name="profiling-your-node-application-on-azure-webapps-with-v8-profiler"></a>Düğüm Uygulamanızı azure webapps V8 Profil Oluşturucu ile profil oluşturma
Örneğin, deyin aşağıda gösterildiği gibi profil oluşturmayı istediğiniz bir hello world uygulamanız olanak tanır:

```
var http = require('http');    
function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    WriteConsoleLog();    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Scm site https://yoursite.scm.azurewebsites.net/DebugConsole gidin

Aşağıda gösterildiği gibi bir komut istemi görürsünüz. Site/wwwroot dizinine gidin

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

"Npm yükleme v8 profil oluşturucu" komutunu çalıştırın

Bu profil oluşturucu v8 düğümü altında yüklemelidir\_modülleri dizin ve tüm bağımlılıkları.
Şimdi, uygulamanızın profilini, server.js düzenleyin.

```
var http = require('http');    
var profiler = require('v8-profiler');    
var fs = require('fs');

function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    profiler.startProfiling('HandleRequest');    
    WriteConsoleLog();    
    fs.writeFileSync('profile.cpuprofile', JSON.stringify(profiler.stopProfiling('HandleRequest')));    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Yukarıdaki değişiklikler WriteConsoleLog işlevi profil ve sonra profili çıktıyı 'profile.cpuprofile' sitesinin wwwroot altında yazın. Uygulamanız için bir istek gönderin. Site wwwroot altında oluşturulan 'profile.cpuprofile' dosyasını görürsünüz.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

Bu dosyayı indirin ve bu dosyayı Chrome F12 araçları ile açmanız gerekir. Chrome üzerinde F12 isabet ve "profilleri sekmesinde"'i tıklatın. "Yük" düğmesine tıklayın. Az önce indirdiğiniz profile.cpuprofile dosyanızı seçin. Yeni yüklenen profilinde'ı tıklatın.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

Aşağıda gösterildiği gibi süre % 95 WriteConsoleLog işlevi tarafından tüketilen görürsünüz. Bu ayrıca tam satır numaralarını ve sorunu neden kaynak dosyaları gösterilmektedir.

### <a name="my-node-application-is-consuming-too-much-memory"></a>Düğüm Uygulamam çok fazla bellek tükettikten.
Yüksek bellek tüketimi hakkında portalınızdaki Azure Webapps bir öneri büyük olasılıkla alırsınız. Belirli izlemek için izleyiciler de ayarlayabilirsiniz [ölçümleri](web-sites-monitor.md). Bellek kullanımı üzerinde denetlerken [Azure portalı panosunun](../application-insights/app-insights-web-monitor-performance.md), yoğun değerlerin kaçırmamak için bellek için en büyük değerleri gözden geçirin.

#### <a name="leak-detection-and-heap-diffing-for-nodejs"></a>Sızıntısı algılama ve node.js için yığın farkı alınıyor
Kullanabileceğinizi [düğümü memwatch](https://github.com/lloyd/node-memwatch) bellek tanımlamanıza yardımcı olması için sızdırıyor.
Memwatch v8 profil oluşturucu gibi yükleyin ve kodunuzu yakalama ve fark Düzenle bellek tanımlamak için yığın sızdırıyor uygulamanızda.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>My node.exe's rastgele sonlandırıldı
Neden bu geçekleşmiş birkaç nedeni vardır:

1. Uygulamanızı Lütfen onay d: Yakalanmayan Özel durumlar – atma\\ev\\LogFiles\\uygulama\\günlük errors.txt dosyada Ayrıntılar için özel durum belirtildi. Bu dosya yığın izleme sahiptir, bu nedenle bunu temel alarak, uygulamanızın düzeltebilirsiniz.
2. Uygulamanızın hangi Başlarken gelen diğer işlemleri etkileyen çok fazla bellek tükettikten. Toplam VM bellek yakın % 100 ise, node.exe's biraz çalışmanız şansınız diğer işlemlere izin vermek için işlem yöneticisi tarafından sonlandırıldı. Bu sorunu gidermek için uygulama bellek sızıntısına yol değil emin olun ya da, uygulamanın gerçekten çok fazla bellek kullanması gerekirse, lütfen daha büyük bir VM ile çok daha fazla RAM ölçeği.

### <a name="my-node-application-does-not-start"></a>Düğüm Uygulamam başlamıyor
Uygulamanızı başlangıçta 500 hataları döndürüyor birkaç nedeni olabilir:

1. Node.exe doğru konumda mevcut değil. NodeProcessCommandLine ayarını denetleyin.
2. Ana komut dosyası doğru konumda mevcut değil. Web.config denetleyin ve işleyiciler bölümü ana komut dosyası ana komut dosyası adıyla eşleştiğinden emin olun.
3. Web.config yapılandırması doğru değil-ayarları adları/değerlerini denetleyin.
4. Soğuk Başlangıç – uygulamanız için başlangıç çok uzun sürüyor. Uygulamanızı daha uzun sürerse (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000 saniye iisnode 500 hata döndürür. Zaman aşımına uğrama ve 500 hata döndürüyor iisnode önlemek için uygulama başlangıç zamanı eşleştirmek için bu ayarları değerini artırın.

### <a name="my-node-application-crashed"></a>Düğüm Uygulamam kilitlendi
Uygulamanızı Lütfen onay d: Yakalanmayan Özel durumlar – atma\\ev\\LogFiles\\uygulama\\günlük errors.txt dosyada Ayrıntılar için özel durum belirtildi. Bu dosya yığın izleme sahiptir, bu nedenle bunu temel alarak, uygulamanızın düzeltebilirsiniz.

### <a name="my-node-application-takes-too-much-time-to-startup-cold-start"></a>Düğüm Uygulamam çok fazla zaman başlangıç için (Cold Start) alır.
En yaygın nedeni bu uygulamanın çok sayıda dosya düğümünde olmasıdır\_modülleri ve uygulama bu dosyaların çoğu, başlatma sırasında yük dener. Azure Webapps üzerinde ağ paylaşımında dosyalarınızın bulunduğu varsayılan olarak, çok sayıda dosya yükleme biraz zaman alabilir.
Bu da hızlandırmak için bazı çözümleri şunlardır:

1. Düz bağımlılık yapısı ve yinelenen bağımlılık modüllerinizi yüklemek için npm3 kullanılarak olduğundan emin olun.
2. Yavaş deneyin yük düğümünüz\_modülleri ve tüm modülleri başlangıçta yüklemeyecek. Başka bir deyişle, aslında işlev modülü kullanmayı deneyin içinde ihtiyacınız olduğunda require('module') çağrısı yapılması gerekir.
3. Azure Webapps yerel önbelleği olarak adlandırılan bir özellik sunar. Bu özellik, içeriğinizin ağ paylaşımından VM yerel diskte kopyalar. Dosyaları yükleme zamanını düğümünün yerel olduğundan\_modülleri çok daha hızlı.

## <a name="iisnode-http-status-and-substatus"></a>IISNODE http durumu ve alt durumu
Bu [kaynak dosyası](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) tüm olası durum substatus birleşimi iisnode hatası durumunda dönebilirsiniz listeler.

Win32 hata kodu görmek, uygulamanız için FREB etkinleştir (Lütfen FREB performans nedenleriyle üretim dışı sitelerindeki yalnızca etkinleştirdiğinizden emin olun).

| Http durumu | HTTP alt durum | Olası bir nedeni? |
| --- | --- | --- |
| 500 |1000 |IISNODE isteği gönderme bazı sorun oluştu – node.exe başlatıldığından ise denetleyin. Başlangıçta node.exe çökme. Hatalar için web.config yapılandırmanızı denetleyin. |
| 500 |1001 |-Win32Error 0x2 - uygulama, URL'ye yanıt vermiyor. URL yeniden yazma kuralları veya express uygulamanızı tanımlanan doğru rotalar olup olmadığını denetleyin. -Win32Error 0x6d – adlandırılmış kanal meşgul – kanal meşgul olduğundan Node.exe isteklerini kabul etmiyor. Yüksek cpu kullanımını denetleyin. -Diğer hataları – denetleyin, node.exe kilitlendi. |
| 500 |1002 |Node.exe kilitlendi – d: denetleyin\\ev\\LogFiles\\yığın izlemesi için günlük errors.txt. |
| 500 |1003 |Yapılandırma sorunu kanal – hiçbir zaman bu görürsünüz, ancak bunu yaparsanız, adlandırılmış kanal yapılandırması doğru değil. |
| 500 |1004-1018 |Bu isteği göndermek veya grafikten node.exe yanıt işleme sırasında bir hata vardı. Node.exe kilitlendi olmadığını denetleyin. d: denetleyin\\ev\\LogFiles\\yığın izlemesi için günlük errors.txt. |
| 503 |1000 |Daha fazla adlandırılmış kanal bağlantılarına ayırmak için yeterli bellek yok. Uygulamanız çok bellek neden tükettikten denetleyin. MaxConcurrentRequestsPerProcess ayarının değerini denetleyin. Varsa, not sonsuz ve çok sayıda isteği varsa, bu hatayı önlemek için bu değeri artırın. |
| 503 |1001 |Uygulamasını geri dönüşüme olduğundan isteği için node.exe dağıtılamadı. Uygulama geri dönüşüme sonra istekleri normalde hizmet. |
| 503 |1002 |Onay win32 hata kodu gerçek nedeni – isteği için bir node.exe dağıtılamadı. |
| 503 |1003 |Adlandırılmış kanal çok meşgul – düğüm CPU çok kullanmadığına denetleyin |

DÜĞÜM olarak adlandırılır NODE.exe içinde bir ayar yoktur\_BEKLEYEN\_kanal\_örnekleri. Azure webapps dışında varsayılan olarak bu değer 4 olur. Başka bir deyişle, bu node.exe bir adlandırılmış kanal zaman 4 isteği yalnızca kabul edebilir. Azure Webapps, bu değer 5000 olarak ayarlanır ve bu değer azure webapps üzerinde çalışan çoğu düğümü uygulamalar için yeterince iyi olmalıdır. Biz düğüm için yüksek bir değere sahip olduğundan, 503.1003 üzerinde azure webapps görülmemelidir\_BEKLEYEN\_kanal\_örnekleri.  |

## <a name="more-resources"></a>Diğer kaynaklar
Azure App Service node.js uygulamaları hakkında daha fazla bilgi için aşağıdaki bağlantıları izleyin.

* [Azure App Service'te Node.js web uygulamalarını kullanmaya başlama](app-service-web-get-started-nodejs.md)
* [Azure Uygulama Hizmeti’ndeki bir Node.js web uygulamasına hata ayıklama](app-service-web-tutorial-nodejs-mongodb-app.md)
* [Azure uygulamalarıyla Node.js Modüllerini kullanma](../nodejs-use-node-modules-azure-apps.md)
* [Azure Uygulama Hizmeti Web Apps: Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Node.js Geliştirici Merkezi](../nodejs-use-node-modules-azure-apps.md)
* [Süper Gizli Kudu Hata Ayıklama Konsolunu keşfetme](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)

