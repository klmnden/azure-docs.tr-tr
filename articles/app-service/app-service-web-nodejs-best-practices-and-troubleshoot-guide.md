---
title: En iyi yöntemler ve sorun giderme - Azure App Service Node.js için
description: Azure App Service'te node.js uygulamaları için sorun giderme adımları ve en iyi uygulamaları öğrenin.
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
ms.custom: seodec18
ms.openlocfilehash: 321dbf891c77007952f01b32bb509a15c2ac3e6f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60853080"
---
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-app-service-windows"></a>En iyi uygulamalar ve Azure App Service Windows üzerinde node.js uygulamaları için sorun giderme kılavuzu

Bu makalede, en iyi yöntemler ve sorun giderme adımları için bilgi [node uygulamaları](app-service-web-get-started-nodejs.md) Azure App Service üzerinde çalışan (ile [iisnode](https://github.com/azure/iisnode)).

> [!WARNING]
> Sorun giderme adımları üretim sitenizi kullanırken dikkatli olun. Uygulamanızı bir üretim dışı Kurulum Örneğin, hazırlık yuvasına ve sorun düzeltildiğinde, hazırlama yuvasına, üretim yuvası ile takas için önerilir.
>

## <a name="iisnode-configuration"></a>IISNODE yapılandırma

Bu [şema dosyası](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) iisnode için yapılandırabileceğiniz tüm ayarları gösterilir. Uygulamanız için yararlı olan ayarlarını bazıları:

### <a name="nodeprocesscountperapplication"></a>nodeProcessCountPerApplication

Bu ayar, IIS uygulaması başlatılan işlemler düğüm sayısını denetler. Varsayılan değer 1’dir. Değer 0 olarak değiştirerek, VM vCPU sayısı olarak çok node.exes başlatabilirsiniz. Önerilen değer uygulamalarının çoğu için 0 olduğundan tüm vcpu makinenizde kullanabilirsiniz. Node.exe tek parçacıklı en fazla 1 vCPU bir node.exe kullanır. Node.js uygulamanızı dışında en yüksek performansı elde etmek için tüm Vcpu kullanmak istiyorsunuz.

### <a name="nodeprocesscommandline"></a>nodeProcessCommandLine

Bu ayar, node.exe yolu denetler. Bu değer, node.exe sürüme işaret edecek şekilde ayarlayabilirsiniz.

### <a name="maxconcurrentrequestsperprocess"></a>maxConcurrentRequestsPerProcess

Bu ayar, en fazla iisnode tarafından gönderilen her node.exe için eş zamanlı istek sayısını denetler. Azure App Service üzerinde sınırsız varsayılan değerdir. Değer, uygulamanın aldığı kaç istekleri ve uygulamanızı her isteğin ne kadar hızlı işleme bağlı olarak yapılandırabilirsiniz.

### <a name="maxnamedpipeconnectionretry"></a>maxNamedPipeConnectionRetry

Bu ayar, en fazla kaç kez denetler iisnode deneme node.exe için istek göndermek için adlandırılmış kanal üzerindeki bağlantı oluşturma. Bu ayar namedPipeConnectionRetryDelay birlikte her isteğin iisnode içinde toplam zaman aşımı belirler. Azure App Service'te 200 varsayılan değerdir. Zaman aşımı saniye cinsinden toplam = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

### <a name="namedpipeconnectionretrydelay"></a>namedPipeConnectionRetryDelay

Bu ayar adlandırılmış kanal üzerinden node.exe için isteği göndermek için her yeniden deneme arasındaki süre (ms) iisnode bekler miktarını denetler. 250 ms varsayılan değerdir.
Zaman aşımı saniye cinsinden toplam = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

Varsayılan olarak, Azure App Service'te iisnode toplam zaman aşımı 200'dür \* 250 ms = 50 saniye.

### <a name="logdirectory"></a>logDirectory

Bu ayar, burada stdout/stderr iisnode günlüklerini directory denetler. (Ana server.js mevcut olduğu dizin) ana komut dosyası dizinine göreli iisnode, varsayılan değer:

### <a name="debuggerextensiondll"></a>debuggerExtensionDll

Node.js uygulamanızı hata ayıklama sırasında node-Inspector iisnode hangi sürümünü kullanır. Bu ayarı denetler. Şu anda iisnode denetçisi 0.7.3.dll ve iisnode inspector.dll yalnızca iki geçerli Bu ayar için değerler. İisnode denetçisi 0.7.3.dll varsayılan değerdir. İisnode denetçisi 0.7.3.dll sürüm düğümü denetçisi 0.7.3 ve web yuvalarını kullanır. Bu sürümü kullanmak için Azure webapp üzerinde Web yuvalarını etkinleştirin. Bkz: <https://ranjithblogs.azurewebsites.net/?p=98> iisnode yeni node-Inspector'ı kullanmak için yapılandırma hakkında daha fazla ayrıntı için.

### <a name="flushresponse"></a>flushResponse

Yanıt veri arabelleği IIS varsayılan davranış, ayarlamak için önce bir temizlemeye veya yanıt sonuna kadar 4 MB hangisinin önce geldiğine bağlı. iisnode bu davranışı geçersiz kılmak için bir yapılandırma ayarı sunar: iisnode node.exe'yi aldıktan hemen sonra bir parçasını yanıt varlık gövdesini temizlemek için ayarlanacak ihtiyacınız iisnode/@flushResponse özniteliği 'true' web.config dosyasında:

```xml
<configuration>
    <system.webServer>
        <!-- ... -->
        <iisnode flushResponse="true" />
    </system.webServer>
</configuration>
```

Bir temizlemeye etkinleştirme yanıtın her parçasını varlık gövdesini ~ %5 (itibariyle v0.1.13) tarafından sistemin aktarım hızını azaltan performans yükü ekler. Gerektiren yanıt akış uç noktaları için bu ayar kapsamı için en iyi şekilde (örneğin, kullanarak `<location>` web.config öğesinde)

Buna ek olarak, uygulamalar, akış için de responseBufferLimit iisnode işleyicinizi, 0 olarak ayarlamanız gerekir.

```xml
<handlers>
    <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>
</handlers>
```

### <a name="watchedfiles"></a>watchedFiles

Değişiklikleri izlenen dosyaların listesi noktalı virgülle ayrılmış. Bir dosyaya herhangi bir değişikliği geri dönüştürmek uygulamanın neden olur. Ana uygulama giriş noktası bulunduğu dizini ile görelidir gerekli dosya adları yanı sıra, bir isteğe bağlı dizin adı her girdi oluşur. Joker karakterler yalnızca dosya adı bölümünde izin verilir. Varsayılan değer: `*.js;iisnode.yml`

### <a name="recyclesignalenabled"></a>recycleSignalEnabled

Varsayılan değer false'tur. Etkinleştirilirse, node.js uygulamanızı bir adlandırılmış kanala bağlanabilir (ortam değişkeni IISNODE\_denetimi\_kanal) ve bir "geri" ileti gönderin. Bu, yalnızca w3wp düzgün bir şekilde geri dönüştürülmesine neden olur.

### <a name="idlepageouttimeperiod"></a>idlePageOutTimePeriod

Varsayılan değer, bu özellik devre dışı anlamına gelen 0 ' dır. Bir değeri 0'dan büyük olarak ayarlandığında, iisnode tüm alt işlemleri milisaniye cinsinden her 'idlePageOutTimePeriod' sayfa. Bkz: [belgeleri](/windows/desktop/api/psapi/nf-psapi-emptyworkingset) ne anlamına gelir sayfasında anlamak için. Bu ayar, yüksek miktarda bellek kullanır ve bazen RAM boşaltmak için disk belleği kullanıma sayfasında istediğiniz uygulamalar için yararlıdır.

> [!WARNING]
> Üretim uygulamaları aşağıdaki yapılandırma ayarları etkinleştirilirken dikkatli olun. Bunları Canlı üretim uygulamaları etkinleştirmemeniz önerilir.
>

### <a name="debugheaderenabled"></a>debugHeaderEnabled

Varsayılan değer false'tur. Bir HTTP yanıt üst bilgisi doğru iisnode kümeye eklerse `iisnode-debug` , gönderdiği her HTTP yanıtına `iisnode-debug` üst bilgi değeri: bir URL. Tek tek parçalarını tanılama bilgileri URL parçacığı bakarak elde edilebilir, ancak bir görselleştirme URL'sini bir tarayıcıda açarak kullanılabilir.

### <a name="loggingenabled"></a>loggingEnabled

Bu ayar, stdout ve stderr günlük iisnode tarafından denetler. Iisnode stdout/stderr başlatır ve 'logDirectory' ayarında belirtilen dizine yazar düğüm işlemlerden yakalar. Bu etkinleştirildikten sonra uygulamanızın dosya sistemini ve uygulama tarafından gerçekleştirilen günlük miktarına bağlı olarak günlükler yazar, performans etkileri olabilir.

### <a name="deverrorsenabled"></a>devErrorsEnabled

Varsayılan değer false'tur. Ne zaman true iisnode kümesine HTTP durum kodu ve Win32 hata kodu tarayıcınızda görüntülenir. Win32 kodu, hata ayıklama sorunları belirli türlerdeki yararlıdır.

### <a name="debuggingenabled-do-not-enable-on-live-production-site"></a>debuggingEnabled (Canlı üretim sitesini üzerinde etkinleştirmeyin)

Bu ayar, hata ayıklama özelliğini denetler. Iisnode node-Inspector ile tümleşiktir. Bu ayarın etkinleştirilmesi, düğüm uygulamanızı hata ayıklama etkinleştirin. Bu ayarın etkinleştirilmesi üzerine iisnode ilk node.js uygulamanızı hata ayıklama isteği 'debuggerVirtualDir' dizininde node-Inspector dosyaları oluşturur. Bir istek göndererek node-Inspector'ı yükleyebilir `http://yoursite/server.js/debug`. Hata ayıklama URL kesimi 'debuggerPathSegment' ayarıyla denetleyebilirsiniz. Varsayılan olarak, debuggerPathSegment = 'debug'. Ayarlayabileceğiniz `debuggerPathSegment` bir GUID, örneğin, bu nedenle olduğundan başkaları tarafından bulunması daha zor.

Okuma [Windows üzerinde node.js uygulamalarında hata ayıklama](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) hata ayıklama hakkında daha fazla bilgi.

## <a name="scenarios-and-recommendationstroubleshooting"></a>Senaryolar ve öneriler ve giderme

### <a name="my-node-application-is-making-excessive-outbound-calls"></a>Düğüm Uygulamam giden çağrıları yapıyor

Birçok uygulama, normal işlemlerinin bir parçası olarak giden bağlantılar oluşturmak istersiniz. Örneğin, bir istek geldiğinde, başka bir yerde REST API ile iletişime geçin ve isteği işlemek için bazı bilgiler almak düğüm uygulamanızı istersiniz. Http veya https çağrıları yapılırken canlı tutma aracı kullanmak isteyebilirsiniz. Canlı tutma aracısı olarak, bu giden çağrıları yapılırken agentkeepalive modülü kullanabilirsiniz.

Yuvalar, Azure webapp VM yeniden kullanılan agentkeepalive modülü sağlar. Her bir giden talep üzerinde yeni bir yuva oluşturma, uygulamanıza ek yükü ekler. Giden istekler için yuva yeniden uygulamanızın olması, uygulamanızın VM başına ayrılmış maxSockets aşmaması sağlar. Azure App Service'te agentKeepAlive maxSockets değeri ayarlamak için toplam önerilir (node.exe 4 örneklerini \* 40 maxSockets/örnek) VM başına 160 yuva.

Örnek [agentKeepALive](https://www.npmjs.com/package/agentkeepalive) yapılandırma:

```nodejs
let keepaliveAgent = new Agent({
    maxSockets: 40,
    maxFreeSockets: 10,
    timeout: 60000,
    keepAliveTimeout: 300000
});
```

> [!IMPORTANT]
> Bu örnekte, sanal makinenizde çalışan 4 node.exe sahip olduğunuz varsayılır. Node.exe VM'de çalışan farklı bir dizi varsa, buna göre ayarlama maxSockets değiştirmeniz gerekir.
>

#### <a name="my-node-application-is-consuming-too-much-cpu"></a>Çok fazla CPU tükettiğini düğüm Uygulamam

Yüksek cpu tüketimi hakkında portalınızdaki Azure uygulama Hizmeti'nden bir öneri alabilirsiniz. Belirli izlemek için izleyicilerinin ayarlayabilirsiniz [ölçümleri](web-sites-monitor.md). CPU kullanımı üzerinde denetlenirken [Azure portalı Panosu](../azure-monitor/app/web-monitor-performance.md), CPU, en yüksek değerleri kaçırmamak için en büyük değerleri kontrol edin.
Uygulamanız çok fazla CPU kullanan ve neden açıklayamadığınız düşünüyorsanız öğrenmek için node.js uygulamanızı profilini oluşturabilirsiniz.

#### <a name="profiling-your-node-application-on-azure-app-service-with-v8-profiler"></a>Node.js uygulamanızı Azure App Service'te V8 Profiler ile profil oluşturma

Örneğin, aşağıdaki gibi profilini oluşturmak istediğiniz bir Merhaba Dünya uygulaması sahip varsayalım:

```nodejs
const http = require('http');
function WriteConsoleLog() {
    for(let i=0;i<99999;++i) {
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

Hata ayıklama konsolunu sitesine gidin `https://yoursite.scm.azurewebsites.net/DebugConsole`

Site/wwwroot dizinine gidin. Aşağıdaki örnekte gösterildiği gibi bir komut istemi görürsünüz:

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

Komutunu çalıştırın `npm install v8-profiler`.

Bu komut düğümü altında v8 profil oluşturucu yükler\_modülleri dizini ve tüm bağımlılıklarını.
Şimdi, uygulamanızın profilini oluşturmak, server.js düzenleyin.

```nodejs
const http = require('http');
const profiler = require('v8-profiler');
const fs = require('fs');

function WriteConsoleLog() {
    for(let i=0;i<99999;++i) {
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

Önceki kod profilleri WriteConsoleLog işlevi ve ardından, site wwwroot 'profile.cpuprofile' dosyayı profili çıktı yazar. Uygulamanız için bir istek gönderin. 'Profile.cpuprofile' dosya oluşturulduğunda, site wwwroot altında görürsünüz.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

Bu dosyayı indirin ve Chrome F12 araçları ile açın. Chrome'da F12 tuşuna basın ve ardından **profilleri** sekmesi. Seçin **yük** düğmesi. İndirdiğiniz profile.cpuprofile dosyanızı seçin. Yalnızca yüklenen profiline tıklayın.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

%95 zaman WriteConsoleLog işlevi tarafından tüketilen görebilirsiniz. Çıktı Ayrıca, sorunu nedeniyle kaynak dosyaları ve tam satır numaralarını gösterir.

### <a name="my-node-application-is-consuming-too-much-memory"></a>Düğüm Uygulamam çok fazla bellek tüketiyor

Uygulamanız çok fazla bellek kullanıp kullanmadığına portalınızdaki yüksek bellek tüketimi hakkında Azure App Service'in bir bildirim görürsünüz. Belirli izlemek için izleyicilerinin ayarlayabilirsiniz [ölçümleri](web-sites-monitor.md). Bellek kullanımı denetlenirken [Azure portalı Panosu](../azure-monitor/app/web-monitor-performance.md), en yüksek değerleri kaçırmamak için bellek için en büyük değerleri kontrol ettiğinizden emin olun.

#### <a name="leak-detection-and-heap-diff-for-nodejs"></a>Sızıntı algılama ve node.js için yığın farkı

Kullanabileceğinizi [düğüm memwatch](https://github.com/lloyd/node-memwatch) bellek tanımlamanıza yardımcı olması için sızıntıları.
Yükleyebileceğiniz `memwatch` yalnızca v8 profil oluşturucu ve kodunuzu yakalama ve fark yığınlarında tanımlamak için düzenleme gibi uygulamanızda bellek sızıntısı.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>My node.exe rastgele sonlandırılan

Neden node.exe rastgele kapatıldığında birkaç nedeni vardır:

1. Uygulamanızı Yakalanmayan Özel durumların – onay d: yanlamasına ivme kazanmaz\\giriş\\LogFiles\\uygulama\\Ayrıntılar için günlüğe kaydetme errors.txt dosyasını özel durum oluştu. Bu dosya, hata ayıklama ve uygulamanızı gidermek için yığın izlemesi sahiptir.
2. Uygulamanızı Başlarken gelen diğer işlemleri etkilemeden çok fazla bellek tüketiyor. Toplam VM bellek yakın %100 ise, node.exe işlemi Yöneticisi tarafından sonlandırıldı. İşlem Yöneticisi, bazı iş yapma imkanına sahip diğer işlemlere izin vermek için bazı işlemleri sonlandırır. Bu sorunu gidermek için uygulamanız için bellek sızıntılarını profil. Uygulamanız büyük miktarda bellek gerekiyorsa (VM'ye kullanılabilir RAM artıran) daha büyük bir VM ölçeği artırabilirsiniz.

### <a name="my-node-application-does-not-start"></a>Düğüm Uygulamam başlamıyor

Başladığında uygulamanızın 500 hataları döndürüyor, birkaç nedeni olabilir:

1. Node.exe doğru konumda mevcut değil. NodeProcessCommandLine ayarını denetleyin.
2. Ana komut dosyası doğru konumda mevcut değil. Web.config denetleyin ve ana kod dosyasında bulunan işleyiciler bölümündeki ana kod dosyasının adı eşleştiğinden emin olun.
3. Web.config yapılandırması doğru değil-ayarları adları/değerleri kontrol edin.
4. Hazırlıksız başlatma – uygulamanızı başlatmak için çok uzun sürüyor. Uygulamanızı daha uzun sürerse (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000 saniye iisnode 500 hata döndürür. Zaman aşımına uğruyor ve 500 hata döndürüyor iisnode önlemek için uygulama başlangıç zamanı eşleşmesi için bu ayarları değerlerini artırın.

### <a name="my-node-application-crashed"></a>Düğüm Uygulamam kilitlendi

Uygulamanızı Yakalanmayan Özel durumların – onay yanlamasına ivme kazanmaz `d:\\home\\LogFiles\\Application\\logging-errors.txt` Ayrıntılar için dosya üzerinde özel durum oluştu. Bu dosya, uygulamanızı tanılayıp yardımcı olmak için yığın izlemesi sahiptir.

### <a name="my-node-application-takes-too-much-time-to-start-cold-start"></a>Düğüm Uygulamam (Gecikmeli Başlatma) başlatmak için çok fazla zaman alır

Genel uzun uygulama başlangıç zamanlarını çok sayıda düğümü dosyalarında nedeni\_modüller. Uygulama başlatma sırasında bu dosyaları çoğunu yüklemek çalışır. Azure App Service'te, ağ paylaşımında dosyalarınızı depolandığından varsayılan olarak, çok sayıda dosya yüklenirken zaman alabilir.
Bu işlem daha hızlı hale getirmek için bazı çözümler vardır:

1. Düz bağımlılık yapısı ve yinelenen bağımlılıkları yoktur, modüllerini yüklemek için npm3 kullanarak sahip olduğunuzdan emin olun.
2. Yavaş deneyin yük düğümünüzü\_modülleri ve tüm uygulama başlangıcında modüllerini yüklenmeyecek. Modül kod yürütmeyi ilk önce işlev içindeki modül gerçekten ihtiyacınız olduğunda yavaş yük modüllerle require('module') çağrısı yapılmalıdır.
3. Azure App Service yerel önbellek adı verilen bir özellik sunar. Bu özellik, içeriğinizi ağ paylaşımından VM yerel diskte kopyalar. Dosyaları yükleme süresi düğümünün yerel olduğundan\_modülleri çok daha hızlı.

## <a name="iisnode-http-status-and-substatus"></a>IISNODE http durumu ve alt durumu

`cnodeconstants` [Kaynak dosyası](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) liste tüm olası durum substatus birleşimleri iisnode nedeniyle bir hata döndürebilir.

Win32 hata kodu görmek uygulamanızın FREB etkinleştir (performansı artırmak için üretim dışı sitelerindeki yalnızca FREB etkinleştirdiğinizden emin olun).

| Http durumu | HTTP alt durum | Olası nedeni nedir? |
| --- | --- | --- |
| 500 |1000 |IISNODE isteği gönderme sorun oluştu: node.exe başlatılıp başlatılmadığını denetleyin. Node.exe başlatırken kilitlenmiş. Web.config yapılandırma hatalarını denetleyin. |
| 500 |1001 |-Win32Error 0x2 - uygulama URL'si yanıt vermiyor. Express uygulamanızın tanımlanan doğru rotalar varsa URL yeniden yazma kuralları veya onay kontrol edin. -Win32Error 0x6d – adlandırılmış kanal meşgul – kanal meşgul olduğundan Node.exe isteklerini kabul etmiyor. Yüksek cpu kullanımını denetleyin. -Diğer hataları – denetleyin, node.exe kilitlendi. |
| 500 |1002 |Node.exe kilitlendi – d: denetleyin\\giriş\\LogFiles\\günlük errors.txt için yığın izlemesi. |
| 500 |1003 |Kanal yapılandırma sorunu – adlandırılmış kanal yapılandırması doğru değil. |
| 500 |1004-1018 |İstek gönderme veya node.exe/gelen yanıt işleme sırasında hata oluştu. Node.exe kilitlendi olmadığını denetleyin. d: denetleyin\\giriş\\LogFiles\\günlük errors.txt için yığın izlemesi. |
| 503 |1000 |Daha fazla adlandırılmış kanal bağlantılarına ayırmak için yeterli bellek yok. Onay neden uygulamanızı çok fazla bellek tüketiyor. MaxConcurrentRequestsPerProcess ayarının değerini denetleyin. Sonsuz değildir ve çok sayıda istek varsa, bu hatayı önlemek için bu değeri artırın. |
| 503 |1001 |Uygulamasını geri dönüşüme olduğundan isteği için node.exe dağıtılamadı. Uygulama beklemeli sonra istekleri normalde hizmet. |
| 503 |1002 |Gerçek bir nedenle – isteği onay win32 hata kodu için bir node.exe dağıtılamadı. |
| 503 |1003 |Adlandırılmış kanal çok meşgul – node.exe aşırı CPU kullanmadığına doğrulayın |

NODE.exe adlı bir ayar olan `NODE_PENDING_PIPE_INSTANCES`. Azure App Service, bu değer 5000 olarak ayarlanır. Bu node.exe kabul edebilir 5000 istekleri adlandırılmış kanal üzerinde bir seferde anlamına gelir. Bu değer, Azure App Service üzerinde çalışan düğümü uygulamalarının çoğu için yeterince iyi olmalıdır. 503\.1003 üzerinde Azure App Service'te yüksek değeri nedeniyle görmelisiniz değil `NODE_PENDING_PIPE_INSTANCES`

## <a name="more-resources"></a>Daha fazla kaynak

Azure App Service'te node.js uygulamaları hakkında daha fazla bilgi için aşağıdaki bağlantıları izleyin.

* [Azure App Service’te Node.js web uygulamalarını kullanmaya başlama](app-service-web-get-started-nodejs.md)
* [Azure Uygulama Hizmeti’ndeki bir Node.js web uygulamasına hata ayıklama](app-service-web-tutorial-nodejs-mongodb-app.md)
* [Azure uygulamalarıyla Node.js Modüllerini kullanma](../nodejs-use-node-modules-azure-apps.md)
* [Azure App Service Web uygulamaları: Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Node.js Geliştirici Merkezi](../nodejs-use-node-modules-azure-apps.md)
* [Süper Gizli Kudu Hata Ayıklama Konsolunu keşfetme](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)
