---
title: En iyi yöntemler ve Azure Web uygulamaları üzerinde düğümü uygulamalar için sorun giderme kılavuzu
description: En iyi yöntemler ve sorun giderme adımları düğümü uygulamaları için Azure Web Apps öğrenin.
services: app-service\web
documentationcenter: nodejs
author: ranjithr
manager: wadeh
editor: ''
ms.assetid: 387ea217-7910-4468-8987-9a1022a99bef
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 11/09/2017
ms.author: ranjithr
ms.openlocfilehash: 860874ed49056e6b4695c060b06bf061820c390e
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-web-apps"></a>En iyi yöntemler ve Azure Web uygulamaları üzerinde düğümü uygulamalar için sorun giderme kılavuzu

Bu makalede, en iyi yöntemler ve sorun giderme adımları için bilgi [düğümü uygulamaları](app-service-web-get-started-nodejs.md) Azure Web uygulamaları çalıştıran (ile [iisnode](https://github.com/azure/iisnode)).

> [!WARNING]
> Sorun giderme adımlarını üretim sitenizde kullanırken dikkatli olun. Uygulamanızı bir üretim dışı kurulumunu örneğin hazırlama yuvası sorun giderme ve sorun düzeltildiğinde, hazırlama yuvası, üretim yuvası ile takas önerilir.
>

## <a name="iisnode-configuration"></a>IISNODE yapılandırma

Bu [şema dosyası](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) için ıısnode yapılandırabileceğiniz tüm ayarları gösterilmektedir. Uygulamanız için yararlı olan ayarlardan bazıları:

### <a name="nodeprocesscountperapplication"></a>nodeProcessCountPerApplication

Bu ayar IIS uygulaması başlatılan düğüm işlem sayısını denetler. Varsayılan değer 1'dir. Değeri 0 olarak değiştirerek VM vCPU sayınız sayıda node.exes başlatabilirsiniz. Tüm Vcpu'lar makinenizde kullanabilmeniz için önerilen değeri çoğu uygulama için 0'dır. Node.exe tek parçacıklı en fazla 1 vCPU bir node.exe kullanır. Düğüm uygulamanızı dışında en yüksek performans almak için tüm Vcpu'lar kullanmak istediğiniz.

### <a name="nodeprocesscommandline"></a>nodeProcessCommandLine

Bu ayar, node.exe yolunu denetler. Bu değer, node.exe sürümüne işaret edecek şekilde ayarlayabilirsiniz.

### <a name="maxconcurrentrequestsperprocess"></a>maxConcurrentRequestsPerProcess

Bu ayar her node.exe iisnode tarafından gönderilen eşzamanlı istek sayısını denetler. Azure Web uygulamaları, varsayılan değer sonsuzdur. Azure Web uygulamaları üzerinde barındırılan değil, varsayılan değer 1024'tür. Değerin kaç uygulamanızı alır isteyen ve ne kadar hızlı uygulamanızı her istek işleme bağlı olarak yapılandırabilirsiniz.

### <a name="maxnamedpipeconnectionretry"></a>maxNamedPipeConnectionRetry

Bu ayar, en fazla kaç kez denetler node.exe için istekleri göndermek için adlandırılmış kanal üzerinde bağlantısı yaparken iisnode yeniden deneme sayısı. Bu ayar namedPipeConnectionRetryDelay birlikte her istek iisnode içinde toplam zaman aşımı belirler. Azure Web uygulamaları üzerinde 200 varsayılan değerdir. Zaman aşımı saniye cinsinden toplam = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

### <a name="namedpipeconnectionretrydelay"></a>namedPipeConnectionRetryDelay

Bu ayar isteği node.exe için adlandırılmış kanal üzerinden göndermek için denemeler arasındaki süre (ms) iisnode bekler miktarını denetler. Varsayılan değer 250 ms ' dir.
Zaman aşımı saniye cinsinden toplam = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

Varsayılan olarak, Azure Web uygulamaları üzerinde iisnode toplam zaman aşımı 200'dür \* 250 ms = 50 saniye.

### <a name="logdirectory"></a>logDirectory

Bu ayar, burada iisnode stdout/stderr günlüklerini dizin denetler. Ana kod dizini (ana server.js mevcut olduğu dizin) göre iisnode varsayılan değer:

### <a name="debuggerextensiondll"></a>debuggerExtensionDll

Bu ayar, düğüm uygulamanızın hatalarını ayıklama node-Inspector iisnode hangi sürümünün kullanır denetler. Şu anda iisnode denetçisi 0.7.3.dll ve iisnode inspector.dll Bu ayar için yalnızca iki geçerli değerler şunlardır. İisnode denetçisi 0.7.3.dll varsayılan değerdir. İisnode denetçisi 0.7.3.dll sürüm düğümü denetçisi 0.7.3 ve web yuvalarını kullanır. Bu sürümü kullanmak için Azure webapp üzerinde Web yuvalarını etkinleştirin. Bkz: <http://ranjithblogs.azurewebsites.net/?p=98> iisnode yeni node-Inspector kullanacak şekilde yapılandırma hakkında daha fazla ayrıntı için.

### <a name="flushresponse"></a>flushResponse

Yanıt veri arabelleği IIS varsayılan davranışını olan yukarı 4 MB temizleme önce ya da yanıt sonuna kadar hangisi önce gelirse. iisnode bu davranışı geçersiz kılmak için bir yapılandırma ayarı sunar: isteğe bağlı olarak iisnode node.exe alır almaz yanıt Varlık gövdesi bir parçasını temizlemek için ayarlamanız gerekir iisnode/@flushResponse özniteliği 'true' web.config dosyasında:

```xml
<configuration>
    <system.webServer>
        <!-- ... -->
        <iisnode flushResponse="true" />
    </system.webServer>
</configuration>
```

Temizleme etkinleştirmek yanıt her parçasında varlık gövdesini ~ %5 (itibariyle v0.1.13) tarafından sistemin verimliliğini azaltır performansa ekler. Bu ayar yanıt akış gerektiren uç noktalara kapsamı için en iyi (örneğin, kullanarak `<location>` web.config öğesinde)

Buna ek olarak, uygulamalar, akış için de iisnode işleyicinizi responseBufferLimit 0 olarak ayarlamanız gerekir.

```xml
<handlers>
    <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>
</handlers>
```

### <a name="watchedfiles"></a>watchedFiles

Noktalı virgülle ayrılmış değişiklikleri izlenen dosyaları listesi. Bir dosya için herhangi bir değişikliği geri dönüştürmek uygulamanın neden olur. Bir isteğe bağlı dizin adı ve bunun yanı sıra ana uygulama giriş noktası bulunduğu dizini ile ilişkili olan bir gerekli dosya adı, her giriş oluşur. Joker karakterler yalnızca dosya adı bölümünde izin verilir. Varsayılan değer: `*.js;web.config`

### <a name="recyclesignalenabled"></a>recycleSignalEnabled

Varsayılan değer false. Etkinleştirilirse, düğüm uygulamanız için bir adlandırılmış kanal bağlanabilir (ortam değişkeni IISNODE\_denetim\_kanal) ve "Geri Dönüşüm" iletisi gönderin. Bu, düzgün biçimde geri dönüştürmek w3wp neden olur.

### <a name="idlepageouttimeperiod"></a>idlePageOutTimePeriod

Bu özellik devre dışı anlamına gelir 0 varsayılan değerdir. Bazı değeri 0'dan büyük olarak ayarlandığında, iisnode tüm alt işlemleri milisaniye cinsinden her 'idlePageOutTimePeriod' sayfa. Bkz: [belgelerine](https://msdn.microsoft.com/library/windows/desktop/ms682606.aspx) ne anlamına gelir sayfasında anlamak için. Bu ayar, yüksek miktarda bellek kullanmasına ve bazen RAM boşaltmak için disk belleği çıkışı sayfasında istediğiniz uygulamalar için kullanışlıdır.

> [!WARNING]
> Aşağıdaki yapılandırma ayarları üretim uygulamaları etkinleştirirken dikkatli olun. Bunları dinamik üretim uygulamaları etkinleştirmemeniz önerilir.
>

### <a name="debugheaderenabled"></a>debugHeaderEnabled

Varsayılan değer false. Bir HTTP yanıt üstbilgisi true iisnode kümesine eklerse, `iisnode-debug` , gönderdiği her HTTP yanıtına `iisnode-debug` üstbilgi değeri olan bir URL. Tanılama bilgileri tek tek parçaları URL parçası bakarak elde edilebilir, ancak, bir görsel öğe URL'yi bir tarayıcıda açarak kullanılabilir.

### <a name="loggingenabled"></a>loggingEnabled

Bu ayar tarafından iisnode stdout ve stderr günlüğe kaydedilmesini denetler. Iisnode stdout/stderr başlatır ve 'logDirectory' ayarında belirtilen dizine yazan düğümü işlemlerden yakalar. Bu etkinleştirildikten sonra dosya sistemi ve uygulama tarafından gerçekleştirilen günlük miktarına bağlı olarak, uygulamanızın günlükler yazar, performans etkileri olabilir.

### <a name="deverrorsenabled"></a>devErrorsEnabled

Varsayılan değer false. Ne zaman true iisnode kümesine tarayıcınızdaki HTTP durum kodu ve Win32 hata kodu görüntüler. Win32 kodu belirli türde bir sorunları hata ayıklamaya yardımcı olur.

### <a name="debuggingenabled-do-not-enable-on-live-production-site"></a>debuggingEnabled (Canlı üretim sitesinde etkinleştirmeyin.)

Bu ayar, hata ayıklama özelliği denetler. Iisnode node-Inspector ile tümleşiktir. Bu ayarın etkinleştirilmesi, düğüm uygulamanızın veya hata ayıklamayı etkinleştir. Bu ayarın etkinleştirilmesi üzerine iisnode düğümü uygulamanıza ilk hata ayıklama isteği 'debuggerVirtualDir' dizininde node-Inspector dosyaları oluşturur. Bir istek göndererek node-Inspector yükleyebilir http://yoursite/server.js/debug. Hata ayıklama URL kesimi 'debuggerPathSegment' ayarıyla denetleyebilirsiniz. Varsayılan olarak, debuggerPathSegment 'debug' =. Ayarlayabileceğiniz `debuggerPathSegment` bir GUID olarak Örneğin, bu nedenle, BT'nin başkaları tarafından bulunmasına daha zor.

Okuma [Windows node.js uygulamalarında hata ayıklama](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) hata ayıklama hakkında daha fazla bilgi.

## <a name="scenarios-and-recommendationstroubleshooting"></a>Senaryolar ve öneriler ve giderme

### <a name="my-node-application-is-making-excessive-outbound-calls"></a>Düğüm Uygulamam aşırı giden çağrıları yapma

Birçok uygulama, normal işlemlerinin bir parçası olarak giden bağlantılar oluşturmak istersiniz. Örneğin, bir istek geldiğinde, başka bir yerde bir REST API başvurun ve isteğini işlemek için bazı bilgiler almak düğüm uygulamanızı istersiniz. Http veya https çağrıları yapılırken canlı tutma aracı kullanmak isteyebilirsiniz. Bu giden çağrıları yapılırken agentkeepalive modülü canlı tutma aracısı olarak kullanabilirsiniz.

Yuvalar, Azure webapp VM yeniden kullanıldığı agentkeepalive modülü sağlar. Yeni bir yuva giden her istekte oluşturma uygulamanıza ek yükü ekler. Giden istekleri için yuva yeniden uygulamanız sahip uygulamanızı VM başına ayrılmış maxSockets aşmadığından sağlar. Azure Web uygulamalarını için toplam agentKeepAlive maxSockets değeri ayarlamak için önerilir (node.exe 4 örneklerini \* 40 maxSockets/örnek) VM başına 160 yuva.

Örnek [agentKeepALive](https://www.npmjs.com/package/agentkeepalive) yapılandırma:

```nodejs
var keepaliveAgent = new Agent({
    maxSockets: 40,
    maxFreeSockets: 10,
    timeout: 60000,
    keepAliveTimeout: 300000
});
```

> [!IMPORTANT]
> Bu örnek, VM'de çalıştırılan 4 node.exe olduğunu varsayar. VM çalıştıran node.exe farklı sayıda varsa, uygun şekilde ayarlama maxSockets değiştirmeniz gerekir.
>

#### <a name="my-node-application-is-consuming-too-much-cpu"></a>Düğüm Uygulamam çok fazla CPU kullanma

Yüksek cpu tüketimi hakkında portalınızdaki Azure Web uygulamaları bir öneri alabilirsiniz. Belirli izlemek için izleyicilerinin ayarlayabilirsiniz [ölçümleri](web-sites-monitor.md). CPU kullanımı üzerinde denetlerken [Azure portalı panosunun](../application-insights/app-insights-web-monitor-performance.md), en yüksek değerleri kaçırmamak için en büyük değerleri için CPU kontrol edin.
Uygulamanız çok fazla CPU kullanan ve nedenini açıklayan olamaz düşünüyorsanız, öğrenmek için düğüm uygulamanıza profil.

#### <a name="profiling-your-node-application-on-azure-web-apps-with-v8-profiler"></a>Düğüm uygulamanızı Azure Web Apps V8 Profil Oluşturucu ile profil oluşturma

Örneğin, aşağıdaki gibi profil istediğiniz bir hello world uygulamanız varsayalım:

```nodejs
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

Hata Ayıklama Konsolu'nda sitesine git https://yoursite.scm.azurewebsites.net/DebugConsole

Site/wwwroot dizinine gidin. Aşağıdaki örnekte gösterildiği gibi bir komut istemi görürsünüz:

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

Komutu çalıştırın `npm install v8-profiler`.

Bu komut düğümü altında v8 profiler yükler\_modülleri dizin ve tüm bağımlılıkları.
Şimdi, uygulamanızın profilini, server.js düzenleyin.

```nodejs
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

Süre % 95 WriteConsoleLog işlevi tarafından tüketilen görebilirsiniz. Çıktı ayrıca tam satır numaralarını ve soruna neden kaynak dosyaları gösterilmektedir.

### <a name="my-node-application-is-consuming-too-much-memory"></a>Düğüm Uygulamam çok fazla bellek tükettikten

Uygulamanız çok fazla bellek tükettikten, yüksek bellek tüketimi hakkında portalınızdaki Azure Web uygulamalarından bildiren bir duyuru bakın. Belirli izlemek için izleyicilerinin ayarlayabilirsiniz [ölçümleri](web-sites-monitor.md). Bellek kullanımı üzerinde denetlerken [Azure portalı panosunun](../application-insights/app-insights-web-monitor-performance.md), en yüksek değerleri kaçırmamak için bellek için en büyük değerleri kontrol ettiğinizden emin olun.

#### <a name="leak-detection-and-heap-diff-for-nodejs"></a>Sızıntısı algılama ve node.js için yığın fark

Kullanabileceğinizi [düğümü memwatch](https://github.com/lloyd/node-memwatch) bellek tanımlamanıza yardımcı olması için sızdırıyor.
Yükleyebileceğiniz `memwatch` yalnızca v8 profil oluşturucu ve kodunuzu yakalama ve fark yığınlara tanımlamak için düzenleme gibi uygulamanızda bellek sızıntıları.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>My node.exe's rastgele sonlandırıldı

Neden node.exe rastgele kapatılır birkaç nedeni vardır:

1. Uygulamanızı onay d: Yakalanmayan Özel durumlar – atma\\ev\\LogFiles\\uygulama\\günlük errors.txt dosyada Ayrıntılar için özel durum belirtildi. Bu dosya, hata ayıklama ve uygulamanızı gidermek için yığın izlemesi sahiptir.
2. Uygulamanızı Başlarken gelen diğer işlemleri etkileyen çok fazla bellek tükettikten değil. Toplam VM bellek yakın % 100 ise, node.exe's işlem yöneticisi tarafından sonlandırıldı. İşlem Yöneticisi biraz çalışmanız şansınız diğer işlemlere izin vermek için bazı işlemleri sonlandırır. Bu sorunu gidermek için uygulamanız için bellek sızıntıları profil. Uygulamanız büyük miktarlarda bellek gerektiriyorsa (VM kullanılabilir RAM artıran) daha büyük bir VM ölçeği.

### <a name="my-node-application-does-not-start"></a>Düğüm Uygulamam başlamıyor

Başlatıldığında, uygulamanızın 500 hataları döndürüyor birkaç nedeni olabilir:

1. Node.exe doğru konumda mevcut değil. NodeProcessCommandLine ayarını denetleyin.
2. Ana komut dosyası doğru konumda mevcut değil. Web.config denetleyin ve işleyiciler bölümü ana komut dosyası ana komut dosyası adıyla eşleştiğinden emin olun.
3. Web.config yapılandırması doğru değil-ayarları adları/değerlerini denetleyin.
4. Soğuk başlangıç – uygulamanızı başlatmak için çok uzun sürüyor. Uygulamanızı daha uzun sürerse (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000 saniye iisnode 500 hata döndürür. Zaman aşımına uğrama ve 500 hata döndürüyor iisnode önlemek için uygulama başlangıç zamanı eşleştirmek için bu ayarları değerini artırın.

### <a name="my-node-application-crashed"></a>Düğüm Uygulamam kilitlendi

Uygulamanızı onay Yakalanmayan Özel durumlar – atma `d:\\home\\LogFiles\\Application\\logging-errors.txt` dosyasını Ayrıntılar için özel durum belirtildi. Bu dosya tanılamak ve gidermek uygulamanız için yığın izlemesi sahiptir.

### <a name="my-node-application-takes-too-much-time-to-start-cold-start"></a>Düğüm Uygulamam (Cold Start) başlatmak için çok fazla zaman alır

Ortak uzun uygulama başlangıç zamanlarını çok sayıda düğümü dosyalarında nedeni\_modüller. Uygulama başlatılırken bu dosyaların çoğu, yüklemeye çalışır. Dosyalarınızı Azure Web uygulamaları, ağ paylaşımında depolandığından varsayılan olarak, birçok dosyalarının yüklenmesi uzun sürebilir.
Bu işlemi hızlandırmak için bazı çözümleri şunlardır:

1. Düz bağımlılık yapısı ve yinelenen bağımlılık modüllerinizi yüklemek için npm3 kullanılarak sahip olduğundan emin olun.
2. Yavaş deneyin düğümünüz yük\_modülleri ve tüm uygulama başlangıcında modüllerin yük değil. Modül kod ilk yürütülmesini önce işlevin içindeki modül gerçekten gerektiğinde yavaş yük modüllerle require('module') çağrısı yapılması gerekir.
3. Azure Web Apps yerel önbelleği olarak adlandırılan bir özellik sunar. Bu özellik, içeriğinizin ağ paylaşımından VM yerel diskte kopyalar. Dosyaları yükleme zamanını düğümünün yerel olduğundan\_modülleri çok daha hızlı.

## <a name="iisnode-http-status-and-substatus"></a>IISNODE http durumu ve alt durumu

`cnodeconstants` [Kaynak dosyası](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) tüm olası durum substatus birleşimleri iisnode bir hata nedeniyle dönebilirsiniz listeler.

Win32 hata kodu görmek, uygulamanız için FREB etkinleştir (performans nedenleriyle üretim dışı sitelerindeki yalnızca FREB etkinleştirdiğinizden emin olun).

| Http Durumu | HTTP alt durum | Olası bir nedeni? |
| --- | --- | --- |
| 500 |1000 |IISNODE isteği gönderme bazı sorun oluştu – node.exe başlatılıp başlatılmadığını denetleyin. Node.exe başlatırken çökme. Hatalar için web.config yapılandırmanızı denetleyin. |
| 500 |1001 |-Win32Error 0x2 - uygulama, URL'ye yanıt vermiyor. Express uygulamanızı tanımlanan doğru yol varsa URL yeniden yazma kuralları veya onay denetleyin. -Win32Error 0x6d – adlandırılmış kanal meşgul – kanal meşgul olduğundan Node.exe isteklerini kabul etmiyor. Yüksek cpu kullanımını denetleyin. -Diğer hataları – denetleyin, node.exe kilitlendi. |
| 500 |1002 |Node.exe kilitlendi – d: denetleyin\\ev\\LogFiles\\yığın izlemesi için günlük errors.txt. |
| 500 |1003 |Kanal yapılandırma sorunu – adlandırılmış kanal yapılandırması doğru değil. |
| 500 |1004-1018 |Bu isteği göndermek veya grafikten node.exe yanıt işleme sırasında bir hata vardı. Node.exe kilitlendi olmadığını denetleyin. d: denetleyin\\ev\\LogFiles\\yığın izlemesi için günlük errors.txt. |
| 503 |1000 |Daha fazla adlandırılmış kanal bağlantılarına ayırmak için yeterli bellek yok. Uygulamanız çok bellek neden tükettikten denetleyin. MaxConcurrentRequestsPerProcess ayarının değerini denetleyin. Sonsuz değildir ve birçok istek varsa, bu hatayı önlemek için bu değeri artırın. |
| 503 |1001 |Uygulamasını geri dönüşüme olduğundan isteği için node.exe dağıtılamadı. Uygulama geri dönüşüme sonra istekleri normalde hizmet. |
| 503 |1002 |Onay win32 hata kodu gerçek nedeni – isteği için bir node.exe dağıtılamadı. |
| 503 |1003 |Adlandırılmış kanal meşgul – node.exe aşırı CPU kullanmadığına doğrulayın |

NODE.exe sahip adı verilen bir ayarı `NODE_PENDING_PIPE_INSTANCES`. Varsayılan olarak, Azure Web uygulamaları üzerinde değil dağıtıldığında bu değer 4'tür. Bu node.exe yalnızca kabul edebileceği dört isteklerini bir adlandırılmış kanal zaman anlamına gelir. Azure Web uygulamalarında, bu değer 5000 olarak ayarlanır. Bu değer Azure Web uygulamaları üzerinde çalışan çoğu düğümü uygulamalar için yeterince iyi olmalıdır. 503.1003 Azure Web uygulamaları için yüksek değerli nedeniyle görmelisiniz değil `NODE_PENDING_PIPE_INSTANCES`

## <a name="more-resources"></a>Diğer kaynaklar

Azure App Service node.js uygulamaları hakkında daha fazla bilgi için aşağıdaki bağlantıları izleyin.

* [Azure App Service'te Node.js Web uygulamalarını kullanmaya başlama](app-service-web-get-started-nodejs.md)
* [Azure Uygulama Hizmeti’ndeki bir Node.js web uygulamasına hata ayıklama](app-service-web-tutorial-nodejs-mongodb-app.md)
* [Azure uygulamalarıyla Node.js Modüllerini kullanma](../nodejs-use-node-modules-azure-apps.md)
* [Azure Uygulama Hizmeti Web Apps: Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Node.js Geliştirici Merkezi](../nodejs-use-node-modules-azure-apps.md)
* [Süper Gizli Kudu Hata Ayıklama Konsolunu keşfetme](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)