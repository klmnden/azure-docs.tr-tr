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
ms.date: 11/09/2017
ms.author: ranjithr
ms.openlocfilehash: f7f3186ba93670e566a10f97dde86a977101e8fc
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2017
---
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-web-apps"></a>En iyi yöntemler ve Azure Web uygulamaları üzerinde düğümü uygulamalar için sorun giderme kılavuzu
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Bu makalede, en iyi yöntemler ve sorun giderme adımları için bilgi [düğümü uygulamaları](app-service-web-get-started-nodejs.md) Azure Web uygulamaları çalıştıran (ile [iisnode](https://github.com/azure/iisnode)).

> [!WARNING]
> Sorun giderme adımlarını üretim sitenizde kullanırken dikkatli olun. Uygulamanızı bir üretim dışı kurulumunu örneğin hazırlama yuvası sorun giderme ve sorun düzeltildiğinde, hazırlama yuvası, üretim yuvası ile takas önerilir.
> 
> 

## <a name="iisnode-configuration"></a>IISNODE yapılandırma
Bu [şema dosyası](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) için ıısnode yapılandırabileceğiniz tüm ayarları gösterilmektedir. Uygulamanız için yararlı olan ayarlardan bazıları:

* nodeProcessCountPerApplication
  
    Bu ayar IIS uygulaması başlatılan düğüm işlem sayısını denetler. Varsayılan değer 1'dir. Bu ayarlayarak VM vCPU sayınız sayıda node.exes başlatabilirsiniz 0. Tüm Vcpu'lar makinenizde kullanabilmeniz için önerilen değeri çoğu uygulama için 0'dır. Node.exe tek parçacıklı en fazla 1 vCPU bir node.exe kullanır. Düğüm uygulamanızı dışında en yüksek performans almak için tüm Vcpu'lar kullanmak istediğiniz.
* nodeProcessCommandLine
  
    Bu ayar, node.exe yolunu denetler. Bu değer, node.exe sürümüne işaret edecek şekilde ayarlayabilirsiniz.
* maxConcurrentRequestsPerProcess
  
    Bu ayar her node.exe iisnode tarafından gönderilen eşzamanlı istek sayısını denetler. Azure Web uygulamaları, bunun için varsayılan değer sonsuzdur. Bu ayar hakkında endişelenmeniz gerekmez. Azure Web Apps dışında varsayılan değer 1024'tür. Bu, kaç tane uygulamanızı alır isteyen ve ne kadar hızlı uygulamanızı her isteğini işler bağlı olarak yapılandırabilirsiniz.
* maxNamedPipeConnectionRetry
  
    Bu ayar, en fazla kaç kez denetler iisnode yeniden deneme için node.exe üzerinden isteği göndermek için adlandırılmış kanal üzerinde bağlantısı yapma. Bu ayar namedPipeConnectionRetryDelay birlikte her istek iisnode içinde toplam zaman aşımı belirler. Azure Web uygulamaları üzerinde 200 varsayılan değerdir. Zaman aşımı saniye cinsinden toplam = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000
* namedPipeConnectionRetryDelay
  
    Bu ayar isteği node.exe için adlandırılmış kanal üzerinden göndermek için denemeler arasındaki süre (ms) iisnode bekler miktarını denetler. Varsayılan değer 250 ms ' dir.
    Zaman aşımı saniye cinsinden toplam = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000
  
    Varsayılan olarak, Azure Web uygulamaları üzerinde iisnode toplam zaman aşımı 200'dür \* 250 ms = 50 saniye.
* logDirectory
  
    Bu ayar, burada iisnode stdout/stderr günlüklerini dizin denetler. Ana kod dizini (ana server.js mevcut olduğu dizin) göre iisnode varsayılan değer:
* debuggerExtensionDll
  
    Bu ayar, düğüm uygulamanızın hatalarını ayıklama node-Inspector iisnode hangi sürümünün kullanır denetler. Şu anda iisnode denetçisi 0.7.3.dll ve iisnode inspector.dll Bu ayar için yalnızca iki geçerli değerler şunlardır. İisnode denetçisi 0.7.3.dll varsayılan değerdir. İisnode denetçisi 0.7.3.dll sürüm düğümü denetçisi 0.7.3 ve websockets kullanır. Bu sürümü kullanmak için Azure webapp websockets etkinleştirmeniz gerekir. Bkz: <http://www.ranjithr.com/?p=98> iisnode yeni node-Inspector kullanacak şekilde yapılandırma hakkında daha fazla ayrıntı için.
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
  
    Yanıt Varlık gövdesi her parçasında temizleme etkinleştirilmesi ekler ~ %5 (itibariyle v0.1.13) tarafından sistemin verimliliğini azaltır performansa yanıt akış gerektiren uç noktaları için bu ayarı kapsamı en iyisidir (örneğin, kullanma <location> web.config öğesinde).
  
    Buna ek olarak, uygulamalar, akış için de iisnode işleyicinizi responseBufferLimit 0 olarak ayarlamanız gerekir.
  
    ```
    <handlers>    
        <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>    
    </handlers>
    ```
* watchedFiles
  
    Bu değişiklikleri izlenen dosyaların noktalı virgülle ayrılmış listesidir. Bir dosyayı yapılan bir değişikliği geri dönüştürmek uygulamanın neden olur. Her giriş isteğe bağlı dizin adı ve ana uygulama giriş noktası bulunduğu dizini ile ilişkilidir gerekli dosya adına sahiptir. Joker karakterler yalnızca dosya adı bölümünde izin verilir. Varsayılan değer "\*. js;web.config"
* recycleSignalEnabled
  
    Varsayılan değer false. Etkinleştirilirse, düğüm uygulamanız için bir adlandırılmış kanal bağlanabilir (ortam değişkeni IISNODE\_denetim\_kanal) ve "Geri Dönüşüm" iletisi gönderin. Bu, düzgün biçimde geri dönüştürmek w3wp neden olur.
* idlePageOutTimePeriod
  
    Bu özellik devre dışı anlamına gelir 0 varsayılan değerdir. Bazı değeri 0'dan büyük olarak ayarlandığında, iisnode tüm alt işlemleri milisaniye cinsinden her 'idlePageOutTimePeriod' sayfa. Bkz: [belgelerine](https://msdn.microsoft.com/library/windows/desktop/ms682606.aspx) ne anlamına gelir sayfasında anlamak için. Bu ayar, çok fazla bellek kullanmasına ve bazen bazı RAM boşaltmak için disk belleği çıkışı sayfasında istediğiniz uygulamalar için kullanışlıdır.

> [!WARNING]
> Aşağıdaki yapılandırma ayarları üretim uygulamaları etkinleştirirken dikkatli olun. Bunları dinamik üretim uygulamaları etkinleştirmemeniz önerilir.
> 
> 

* debugHeaderEnabled
  
    Varsayılan değer false. True, iisnode kümesine her HTTP yanıt olarak bir HTTP yanıt üstbilgisi iisnode-debug eklerse iisnode-debug üstbilgi değeri bir URL'dir gönderir. Tanılama bilgileri tek tek parçaları URL parçası bakarak konusunda, ancak daha iyi görselleştirme URL tarayıcıda açarak elde edilir.
* loggingEnabled
  
    Bu ayar tarafından iisnode stdout ve stderr günlüğe kaydedilmesini denetler. Iisnode stdout/stderr başlatır ve 'logDirectory' ayarında belirtilen dizine yazan düğümü işlemlerden yakalar. Bu etkinleştirildikten sonra dosya sistemi ve uygulama tarafından gerçekleştirilen günlük miktarına bağlı olarak, uygulamanızın günlükler yazar, performans etkileri olabilir.
* devErrorsEnabled
  
   Varsayılan değer false. Ne zaman true iisnode kümesine tarayıcınızdaki HTTP durum kodu ve Win32 hata kodu görüntüler. Win32 kodu belirli türde bir sorunları hata ayıklamaya yardımcı olur.
* debuggingEnabled (Canlı üretim sitesinde etkinleştirmeyin.)
  
    Bu ayar, hata ayıklama özelliği denetler. Iisnode node-Inspector ile tümleşiktir. Bu ayarın etkinleştirilmesi, düğüm uygulamanızın veya hata ayıklamayı etkinleştir. Bu ayar etkinleştirildiğinde, iisnode düğümü uygulamanıza ilk hata ayıklama isteği 'debuggerVirtualDir' dizinindeki gerekli node-Inspector dosyalarda düzenleme. Node-Inspector http://yoursite/server.js/debug için bir istek göndererek yükleyebilirsiniz. Hata ayıklama URL kesimi 'debuggerPathSegment' ayarıyla denetleyebilirsiniz. Varsayılan olarak, debuggerPathSegment 'debug' =. Başkaları tarafından bulunmasına daha da zor olan şekilde, bu bir GUID, örneğin, ayarlayabilirsiniz.
  
    Bu kontrol [bağlantı](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) hata ayıklama hakkında daha fazla bilgi.

## <a name="scenarios-and-recommendationstroubleshooting"></a>Senaryolar ve öneriler ve giderme
### <a name="my-node-application-is-making-too-many-outbound-calls"></a>Düğüm Uygulamam çok fazla giden çağrıları yapma.
Birçok uygulama, normal işlemlerinin bir parçası olarak giden bağlantılar oluşturmak istersiniz. Örneğin, bir istek geldiğinde, başka bir yerde bir REST API başvurun ve isteğini işlemek için bazı bilgiler almak düğüm uygulamanızı istersiniz. Http veya https çağrıları yapılırken canlı tutma aracı kullanmak isteyebilirsiniz. Örneğin, bu giden çağrıları yapılırken canlı tutma aracısı olarak agentkeepalive modülü kullanabilirsiniz. Bu yuva, Azure webapp VM yeniden kullanıldığı emin olur ve giden her istek için yeni yuva oluşturma yükünü azaltma. Ayrıca, bu sayıda giden istek yapmak için daha az sayıda yuva kullanıyorsanız ve bu nedenle VM başına ayrılmış maxSockets aşmayan emin olur. Azure Web uygulamaları üzerinde agentKeepAlive maxSockets değeri 160 yuva VM başına toplam ayarlamak için önerilir. Bu VM'de çalıştırılan dört node.exe varsa, 160 VM başına toplam olduğu agentKeepAlive maxSockets node.exe, başına 40 ayarlamak istediğinizi anlamına gelir.

Örnek [agentKeepALive](https://www.npmjs.com/package/agentkeepalive) yapılandırma:

```
var keepaliveAgent = new Agent({    
    maxSockets: 40,    
    maxFreeSockets: 10,    
    timeout: 60000,    
    keepAliveTimeout: 300000    
});
```

Bu örnek, VM'de çalıştırılan 4 node.exe olduğunu varsayar. VM çalıştıran node.exe farklı sayıda varsa, uygun şekilde ayarlama maxSockets değiştirmeniz gerekir.

### <a name="my-node-application-is-consuming-too-much-cpu"></a>Düğüm Uygulamam çok fazla CPU tüketilmesine neden olabilir.
Yüksek cpu tüketimi hakkında portalınızdaki Azure Web uygulamaları bir öneri büyük olasılıkla alırsınız. Belirli izlemek için izleyicilerinin ayarlayabilirsiniz [ölçümleri](web-sites-monitor.md). CPU kullanımı üzerinde denetlerken [Azure portalı panosunun](../application-insights/app-insights-web-monitor-performance.md), en yüksek değerleri kaçırmamak için en büyük değerleri için CPU kontrol edin.
Burada, uygulamanızın çok fazla CPU kullanan ve nedenini açıklayan olamaz düşündüğünüz durumlarda öğrenmek için düğüm uygulamanıza profil.

### 
#### <a name="profiling-your-node-application-on-azure-web-apps-with-v8-profiler"></a>Düğüm uygulamanızı Azure Web Apps V8 Profil Oluşturucu ile profil oluşturma
Örneğin, aşağıdaki gibi profil istediğiniz bir hello world uygulamanız varsayalım:

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

Aşağıda gösterildiği gibi bir komut istemi görürsünüz. Site/wwwroot dizinine gidin.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

"Npm yükleme v8 profil oluşturucu" komutunu çalıştırın.

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

Önceki kod profilleri WriteConsoleLog işlev ve site wwwroot altında 'profile.cpuprofile' dosyasına profili çıktısı yazar. Uygulamanız için bir istek gönderin. Site wwwroot altında oluşturulan bir 'profile.cpuprofile' dosyasına bakın.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

Bu dosyayı indirin ve Chrome F12 araçları ile açın. Chrome üzerinde F12 tuşuna basın ve ardından **profilleri** sekmesi. Seçin **yük** düğmesi. İndirdiğiniz profile.cpuprofile dosyanızı seçin. Yeni yüklenen profilinde'ı tıklatın.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

Süre % 95 WriteConsoleLog işlevi tarafından tüketilen görebilirsiniz. Bu ayrıca tam satır numaralarını ve soruna neden kaynak dosyaları gösterilmektedir.

### <a name="my-node-application-is-consuming-too-much-memory"></a>Düğüm Uygulamam çok fazla bellek tükettikten
Uygulamanız çok fazla bellek tükettikten, yüksek bellek tüketimi hakkında portalınızdaki Azure Web uygulamalarından bildiren bir duyuru bakın. Belirli izlemek için izleyicilerinin ayarlayabilirsiniz [ölçümleri](web-sites-monitor.md). Bellek kullanımı üzerinde denetlerken [Azure portalı panosunun](../application-insights/app-insights-web-monitor-performance.md), en yüksek değerleri kaçırmamak için bellek için en büyük değerleri kontrol ettiğinizden emin olun.

#### <a name="leak-detection-and-heap-diffing-for-nodejs"></a>Sızıntısı algılama ve node.js için yığın farkı alınıyor
Kullanabileceğinizi [düğümü memwatch](https://github.com/lloyd/node-memwatch) bellek tanımlamanıza yardımcı olması için sızdırıyor.
Memwatch v8 profil oluşturucu gibi yükleyin ve kodunuzu yakalama ve fark Düzenle bellek tanımlamak için yığın sızdırıyor uygulamanızda.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>My node.exe's rastgele sonlandırıldı
Neden bu geçekleşmiş birkaç nedeni vardır:

1. Uygulamanızı onay d: Yakalanmayan Özel durumlar – atma\\ev\\LogFiles\\uygulama\\günlük errors.txt dosyada Ayrıntılar için özel durum belirtildi. Bu dosya yığın izleme sahiptir, bu nedenle bunu temel alarak, uygulamanızın düzeltebilirsiniz.
2. Uygulamanızın hangi Başlarken gelen diğer işlemleri etkileyen çok fazla bellek tükettikten. Toplam VM bellek yakın % 100 ise, node.exe's biraz çalışmanız şansınız diğer işlemlere izin vermek için işlem yöneticisi tarafından sonlandırıldı. Bu sorunu gidermek için uygulama bellek sızıntısına yol değil emin olun veya daha büyük bir VM ile çok daha fazla RAM, uygulamanızın önemli miktarda bellek kullanması gerekirse, ölçeği.

### <a name="my-node-application-does-not-start"></a>Düğüm Uygulamam başlamıyor
Başlatıldığında, uygulamanızın 500 hataları döndürüyor birkaç nedeni olabilir:

1. Node.exe doğru konumda mevcut değil. NodeProcessCommandLine ayarını denetleyin.
2. Ana komut dosyası doğru konumda mevcut değil. Web.config denetleyin ve işleyiciler bölümü ana komut dosyası ana komut dosyası adıyla eşleştiğinden emin olun.
3. Web.config yapılandırması doğru değil-ayarları adları/değerlerini denetleyin.
4. Soğuk başlangıç – uygulamanızı başlatmak için çok uzun sürüyor. Uygulamanızı daha uzun sürerse (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000 saniye iisnode 500 hata döndürür. Zaman aşımına uğrama ve 500 hata döndürüyor iisnode önlemek için uygulama başlangıç zamanı eşleştirmek için bu ayarları değerini artırın.

### <a name="my-node-application-crashed"></a>Düğüm Uygulamam kilitlendi
Uygulamanızı Lütfen onay d: Yakalanmayan Özel durumlar – atma\\ev\\LogFiles\\uygulama\\günlük errors.txt dosyada Ayrıntılar için özel durum belirtildi. Bu dosya yığın izleme sahiptir, bu nedenle bunu temel alarak, uygulamanızın düzeltebilirsiniz.

### <a name="my-node-application-takes-too-much-time-to-start-cold-start"></a>Düğüm Uygulamam (Cold Start) başlatmak için çok fazla zaman alır
Başlatmak için uzun süren bir uygulama için en yaygın nedeni düğümündeki dosyaları yüksek sayısıdır\_modüller. Uygulama başlatılırken bu dosyaların çoğu, yüklemeye çalışır. Azure Web uygulamaları üzerinde ağ paylaşımında dosyalarınızın bulunduğu varsayılan olarak, birçok dosyalarının yüklenmesi uzun sürebilir.
Bu işlemi hızlandırmak için bazı çözümleri şunlardır:

1. Düz bağımlılık yapısı ve yinelenen bağımlılık modüllerinizi yüklemek için npm3 kullanılarak sahip olduğundan emin olun.
2. Yavaş deneyin düğümünüz yük\_modülleri ve tüm uygulama başlangıcında modüllerin yük değil. Bu, aslında işlev Modülü'nü kullanırken deneyin içinde ihtiyacınız olduğunda require('module') çağrısı yapılmalıdır anlamına gelir.
3. Azure Web Apps yerel önbelleği olarak adlandırılan bir özellik sunar. Bu özellik, içeriğinizin ağ paylaşımından VM yerel diskte kopyalar. Dosyaları yükleme zamanını düğümünün yerel olduğundan\_modülleri çok daha hızlı.

## <a name="iisnode-http-status-and-substatus"></a>IISNODE http durumu ve alt durumu
Bu [kaynak dosyası](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) tüm olası durum substatus birleşimleri iisnode bir hata durumunda dönebilirsiniz listeler.

Win32 hata kodu görmek, uygulamanız için FREB etkinleştir (performans nedenleriyle üretim dışı sitelerindeki yalnızca FREB etkinleştirdiğinizden emin olun).

| Http durumu | HTTP alt durum | Olası bir nedeni? |
| --- | --- | --- |
| 500 |1000 |IISNODE isteği gönderme bazı sorun oluştu – node.exe başlatılıp başlatılmadığını denetleyin. Node.exe başlatırken çökme. Hatalar için web.config yapılandırmanızı denetleyin. |
| 500 |1001 |-Win32Error 0x2 - uygulama, URL'ye yanıt vermiyor. Express uygulamanızı tanımlanan doğru yol varsa URL yeniden yazma kuralları veya onay denetleyin. -Win32Error 0x6d – adlandırılmış kanal meşgul – kanal meşgul olduğundan Node.exe isteklerini kabul etmiyor. Yüksek cpu kullanımını denetleyin. -Diğer hataları – denetleyin, node.exe kilitlendi. |
| 500 |1002 |Node.exe kilitlendi – d: denetleyin\\ev\\LogFiles\\yığın izlemesi için günlük errors.txt. |
| 500 |1003 |Yapılandırma sorunu kanal – hiçbir zaman bu görürsünüz, ancak bunu yaparsanız, adlandırılmış kanal yapılandırması doğru değil. |
| 500 |1004-1018 |Bu isteği göndermek veya grafikten node.exe yanıt işleme sırasında bir hata vardı. Node.exe kilitlendi olmadığını denetleyin. d: denetleyin\\ev\\LogFiles\\yığın izlemesi için günlük errors.txt. |
| 503 |1000 |Daha fazla adlandırılmış kanal bağlantılarına ayırmak için yeterli bellek yok. Uygulamanız çok bellek neden tükettikten denetleyin. MaxConcurrentRequestsPerProcess ayarının değerini denetleyin. Sonsuz değildir ve birçok istek varsa, bu hatayı önlemek için bu değeri artırın. |
| 503 |1001 |Uygulamasını geri dönüşüme olduğundan isteği için node.exe dağıtılamadı. Uygulama geri dönüşüme sonra istekleri normalde hizmet. |
| 503 |1002 |Onay win32 hata kodu gerçek nedeni – isteği için bir node.exe dağıtılamadı. |
| 503 |1003 |Adlandırılmış kanal çok meşgul – düğüm aşırı CPU kullanmadığına denetleyin |

DÜĞÜM olarak adlandırılır NODE.exe içinde bir ayar yoktur\_BEKLEYEN\_kanal\_örnekleri. Azure Web Apps dışında varsayılan olarak bu değer 4 olur. Başka bir deyişle, bu node.exe bir adlandırılmış kanal zaman dört istekleri yalnızca kabul edebilir. Azure Web uygulamalarında, bu değer 5000 olarak ayarlanır. Bu değer Azure Web uygulamaları üzerinde çalışan çoğu düğümü uygulamalar için yeterince iyi olmalıdır. 503.1003 Azure Web uygulamalarında nedeniyle yüksek bir değer DÜĞÜMÜNÜ görülmemelidir\_BEKLEYEN\_kanal\_örnekleri.  |

## <a name="more-resources"></a>Diğer kaynaklar
Azure App Service node.js uygulamaları hakkında daha fazla bilgi için aşağıdaki bağlantıları izleyin.

* [Azure App Service'te Node.js Web uygulamalarını kullanmaya başlama](app-service-web-get-started-nodejs.md)
* [Azure Uygulama Hizmeti’ndeki bir Node.js web uygulamasına hata ayıklama](app-service-web-tutorial-nodejs-mongodb-app.md)
* [Azure uygulamalarıyla Node.js Modüllerini kullanma](../nodejs-use-node-modules-azure-apps.md)
* [Azure Uygulama Hizmeti Web Apps: Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Node.js Geliştirici Merkezi](../nodejs-use-node-modules-azure-apps.md)
* [Süper Gizli Kudu Hata Ayıklama Konsolunu keşfetme](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)

