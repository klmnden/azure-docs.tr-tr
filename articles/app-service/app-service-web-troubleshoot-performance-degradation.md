---
title: "Yavaş App Service'te web uygulaması performans | Microsoft Docs"
description: "Bu makale, Azure App Service'te yavaş web uygulaması performans sorunlarını gidermenize yardımcı olur."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: top-support-issue
keywords: "Web uygulama performansı, yavaş uygulama, uygulama yavaş"
ms.assetid: b8783c10-3a4a-4dd6-af8c-856baafbdde5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2016
ms.author: cephalin
ms.openlocfilehash: 6b71aa004095a94bea84623fd2b5dbdfc1f81af0
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2018
---
# <a name="troubleshoot-slow-web-app-performance-issues-in-azure-app-service"></a>Azure App Service'te yavaş web uygulaması performans sorunlarını giderme
Bu makalede yavaş web uygulaması performans sorunları gidermenize yardımcı [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumlar](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) ve tıklayın **destek alın**.

## <a name="symptom"></a>Belirti
Web uygulaması, sayfa yükü yavaş göz atarken ve bazen zaman aşımı.

## <a name="cause"></a>Nedeni
Bu sorun sık sık uygulama düzeyi sorunları tarafından gibi neden olur:

* ağ isteklerine uzun sürüyor
* uygulama kodu veya veritabanı sorguları verimsiz bırakılıyor
* yüksek bellek/CPU kullanarak uygulama
* bir özel durum nedeniyle kilitlenen uygulama

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
Sorun giderme sıralı bir düzende üç farklı görevler ayrılabilir:

1. [İnceleyin ve uygulama davranışı izleme](#observe)
2. [Veri toplama](#collect)
3. [Bu sorunu azaltmak](#mitigate)

[App Service Web Apps](/services/app-service/web/) her adımda çeşitli seçenekler sunar.

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a>1. İnceleyin ve uygulama davranışı izleme
#### <a name="track-service-health"></a>Hizmet durumu izleme
Microsoft Azure hizmet kesintisi veya performans düşüşü olduğundan her zaman publicizes. Hizmet durumunu takip edebilirsiniz [Azure portal](https://portal.azure.com/). Daha fazla bilgi için bkz: [hizmet durumu izleme](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>Web uygulamanızı izlemek
Bu seçenek, uygulamanız herhangi bir sorun olması durumunda öğrenmek sağlar. Web uygulamanızın dikey penceresinde tıklayın **istekleri ve hataları** döşeme. **Ölçüm** dikey ekleyebilirsiniz tüm ölçümlerini gösterir.

Web uygulamanızı izlemek için isteyebilirsiniz ölçümleri bazıları

* Ortalama bellek çalışma kümesi
* Ortalama yanıt süresi
* CPU süresi
* Bellek çalışma kümesi
* İstekler

![Web uygulaması performans izleme](./media/app-service-web-troubleshoot-performance-degradation/1-monitor-metrics.png)

Daha fazla bilgi için bkz.

* [Azure App Service'te Web uygulamalarını izleme](web-sites-monitor.md)
* [Uyarı bildirimleri alma](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

#### <a name="monitor-web-endpoint-status"></a>Web uç noktası durumunu izleme
Web uygulamanızı çalıştırıyorsanız, **standart** fiyatlandırma katmanı, Web uygulamaları iki uç nokta üç coğrafi konumdan izlemenize izin verir.

Uç nokta izleme web testleri test yanıt süresi ve web URL'leri çalışma süresini coğrafi olarak dağıtılmış konumlardan yapılandırır. Test yanıt süresi ve çalışma süresi her konumdan belirlemek için web URL'si üzerinde bir HTTP GET işlemi gerçekleştirir. Yapılandırılmış her konum bir test beş dakikada bir çalışır.

Açık kalma süresi HTTP yanıt kodları kullanılarak izlenen ve yanıt süresini milisaniye cinsinden ölçülür. HTTP yanıt kodu 400'e eşit veya daha büyük ise veya yanıtın 30 saniyeden uzun sürerse izleme testi başarısız olur. Bir uç nokta izleme testlerinin belirtilen tüm konumlarda başarılı olması kullanılabilir olarak kabul edilir.

Bunu ayarlamak için bkz: [Azure uygulama hizmetinde uygulamaları izleme](web-sites-monitor.md).

Ayrıca bkz [tutma Azure Web siteleri ve uç nokta izleme - Stefan Schackow ile](https://channel9.msdn.com/Shows/Azure-Friday/Keeping-Azure-Web-Sites-up-plus-Endpoint-Monitoring-with-Stefan-Schackow) uç nokta izleme hakkında bir video.

#### <a name="application-performance-monitoring-using-extensions"></a>Uzantıları kullanarak uygulama performansı izleme
Kullanarak, uygulama performansını izleyebilirsiniz bir *site uzantısı*.

Her App Service web uygulaması güçlü birtakım Araçlar dağıtılan site uzantıları olarak kullanmanıza olanak sağlayan bir Genişletilebilir yönetim uç noktası sağlar. Uzantıları aşağıdakileri içerir: 

- Kaynak kodu düzenleyicileri ister [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs.aspx). 
- Yönetim Araçları gibi bir MySQL veritabanının bağlı kaynaklar için bir web uygulamasına bağlandı.

[Azure Application Insights](/services/application-insights/) izleme de kullanılabilir olan site uzantısı performansın. Application Insights kullanmak için kodunuzu bir SDK ile yeniden oluşturun. Ayrıca, ek verilere erişim sağlayan bir uzantı yükleyebilirsiniz. SDK, kullanım ve uygulamanızı daha ayrıntılı performansını izlemek için kod yazma olanak tanır. Daha fazla bilgi için bkz: [web uygulamalarında performansı izleyerek](../application-insights/app-insights-web-monitor-performance.md).

<a name="collect" />

### <a name="2-collect-data"></a>2. Veri toplama
Web Apps ortam web sunucusu ve web uygulamasının içinden bilgileri günlüğe kaydetme için tanılama işlevi sağlar. Bilgiler, web sunucusu tanılama ve uygulama tanılama olarak ayrılır.

#### <a name="enable-web-server-diagnostics"></a>Web sunucusu tanılamayı etkinleştirin
Etkinleştirmek veya günlükleri şu tür devre dışı bırakabilirsiniz:

* **Hata günlüğü ayrıntılı** -ayrıntılı hata bilgileri (durum kodu 400 veya daha büyük) hatası olduğunu gösteren HTTP durum kodları için. Bu neden sunucu döndürülen hata kodu belirlemek yardımcı olabilecek bilgiler içerebilir.
* **Başarısız istek izleme** -ayrıntılı izleme istek ve her bileşenin geçen süre işlemek için kullanılan IIS bileşenlerini de dahil olmak üzere, başarısız istekler hakkında bilgi. Bu web uygulaması performansı veya belirli bir HTTP hata neden olan yalıtmak çalıştığınız durumlarda yararlı olabilir.
* **Web sunucusu günlüğe kaydetme** -HTTP işlemlerini W3C Genişletilmiş günlük dosyası biçimi kullanma hakkında bilgi. Bu, belirli bir IP adresinden işlenen isteklerin ya da kaç istek sayısı gibi genel web uygulaması ölçümleri belirlerken yararlıdır.

#### <a name="enable-application-diagnostics"></a>Uygulama tanılamayı etkinleştirin
Web uygulamaları, uygulamanızı Visual Studio'dan Canlı ya da daha fazla bilgi ve izlemeleri oturum uygulama kodunuz değiştirme profili uygulama performans verilerini toplamak için birkaç seçeneğiniz vardır. Uygulama ve izlemeden gözlenen zorunda ne kadar erişim Araçlar Seçenekler seçebilirsiniz.

##### <a name="use-application-insights-profiler"></a>Uygulama Öngörüler profil oluşturucu kullanın
Ayrıntılı performans izlemelerini yakalama başlatmak uygulama Öngörüler Profiler etkinleştirebilirsiniz. En fazla yakalanan izlere erişebilirsiniz beş sorunları araştırmak için gerektiğinde gün önce geçmişte oldu. Azure Portal'da web uygulamanızın Application Insights kaynağına erişimi olduğu sürece bu seçeneği belirleyebilirsiniz.

Uygulama Öngörüler profil oluşturucu yavaş yanıtlar hangi satırlık bir kod neden gösteren yanıt süresi her bir web araması ve izlemeleri için istatistikler sağlar. Bir kullanıcı belirli kod yazılmaz bazen App Service uygulaması yavaş çünkü yolu. Paralel ve istenmeyen veritabanı kilit çakışmaları çalıştırılabilir sıralı kod örnekleri içerir. Bu performans sorunlarını kodda kaldırmak, uygulamanın performansını artırır, ancak ayrıntılı izlemeleri ve günlükleri ayarlamadan algılamak sabit. Uygulama Öngörüler Profil Oluşturucu tarafından toplanan izlemeleri uygulama hizmeti uygulamalar için bu sorunu gidermek ve uygulama yavaşlatır kod satırlarını tanımlama yardımcı olur.

 Daha fazla bilgi için bkz: [profil oluşturma Canlı Azure web uygulamaları Application Insights ile](../application-insights/app-insights-profiler.md).

##### <a name="use-remote-profiling"></a>Uzaktan profil oluşturmayı kullanma
Azure App Service'te Web Apps, API uygulamaları ve Web işleri uzaktan profili. Web uygulaması kaynak erişimi ve sorunu yeniden biliyorsunuz ya da tam zaman aralığını biliyorsanız, performans sorunu olur gerekirse bu seçeneği belirleyin.

Uzaktan profil oluşturma işleminin CPU kullanımı yüksekse ve işleminizi beklenenden daha yavaş çalışıyor veya HTTP isteklerini gecikme normalden daha yüksek yaparsanız uzaktan işleminizi profil ve işlem etkinlik ve kod etkin yolları çözümlemek için CPU örnekleme çağrı yığınları almak yararlı olur.

Daha fazla bilgi için bkz: [uzaktan profil oluşturma desteği Azure App Service'te](https://azure.microsoft.com/blog/remote-profiling-support-in-azure-app-service).

##### <a name="set-up-diagnostic-traces-manually"></a>Tanılama izlemeleri el ile ayarlayın
Web uygulama kaynak koduna erişiminiz varsa, uygulama Tanılama web uygulama tarafından üretilen bilgileri yakalamanıza olanak sağlar. ASP.NET uygulamaları kullanabileceğiniz `System.Diagnostics.Trace` bilgi uygulama tanılama günlüğüne sınıfı. Ancak, kodu değiştirin ve uygulamanızı dağıtmanız gerekir. Uygulamanızı test bir ortamda çalışıyorsa, bu yöntem önerilir.

Uygulama günlüğü için yapılandırma hakkında ayrıntılı yönergeler için bkz [Azure App Service'te web uygulamalarını için tanılama günlüğünü etkinleştirme](web-sites-enable-diagnostic-log.md).

#### <a name="use-the-azure-app-service-support-portal"></a>Azure uygulama hizmeti destek Portalı'nı kullanın
Web uygulamaları, web uygulamanızın HTTP günlükleri, olay günlükleri, işlem dökümleri ve daha fazla bakarak ilgili sorunları giderme olanağı sağlar. Bizim destek portalında kullanarak tüm bu bilgilere erişebilen **http://&lt;, uygulama adı >.scm.azurewebsites.net/Support**

Azure uygulama hizmeti destek portal yaygın bir sorun giderme senaryo üç adımdan desteklemek için ayrı, üç sekme sağlar:

1. Şu anki davranışı inceleyin
2. Tanılama bilgilerini toplama ve yerleşik çözümleyiciler çalıştıran analiz eder.
3. Azaltma

Sorun şu anda oluşuyorsa tıklatın **Çözümle** > **tanılama** > **şimdi tanılamak** sizin için bir tanılama oturumu oluşturmak için hangi HTTP toplar, Olay Görüntüleyicisi'ni, bellek dökümleri, PHP Hata günlüklerini ve PHP işlem raporu günlükleri.

Veriler toplandıktan sonra destek portal verilerini analiz çalışır ve bir HTML raporu ile sağlar.

Varsayılan olarak verileri indirmek istediğiniz durumda D:\home\data\DaaS klasöründe depolanması.

Azure uygulama hizmeti destek portal hakkında daha fazla bilgi için bkz: [Azure Web siteleri için Site uzantısı desteklemek için yeni güncelleştirmeler](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).

#### <a name="use-the-kudu-debug-console"></a>Kudu hata ayıklama konsolunu kullanın
Web uygulamaları, hata ayıklama, keşfetme, ortamınız hakkında bilgi almak için JSON bitiş noktaları yanı sıra, dosyaları karşıya yükleme için kullanabileceğiniz bir hata ayıklama konsolu ile birlikte gelir. Bu konsolu adlı *Kudu konsol* veya *SCM Pano* web uygulamanız için.

Bağlantısını giderek bu panoya erişebilir **https://&lt;, uygulama adı >.scm.azurewebsites.net/**.

Kudu sağlar şeylerden bazıları şunlardır:

* Uygulamanız için ortam ayarları
* günlük akışı
* Tanılama dökümü
* hata ayıklama konsolunu Powershell cmdlet'leri ve temel DOS komutları çalıştırabilirsiniz.

Başka bir yararlı Kudu uygulamanızı ilk fırsat özel durum atma durumunda, Kudu kullanabilirsiniz ve bellek oluşturmak için Procdump SysInternals aracı dökümleri, özelliğidir. Bu bellek dökümlerini işleminin anlık görüntüleri ve genellikle web uygulamanızı daha karmaşık sorunları gidermenize yardımcı olabilir.

Kudu içinde kullanılabilir özellikler hakkında daha fazla bilgi için bkz: [hakkında bilmeniz Azure Web siteleri Team Services Araçları](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />

### <a name="3-mitigate-the-issue"></a>3. Bu sorunu azaltmak
#### <a name="scale-the-web-app"></a>Web uygulama ölçeklendirme
Azure App Service'te performansı artırmak ve üretilen işi için uygulamanızı çalıştıran ölçeği ayarlayabilirsiniz. Bir web uygulamasını kurup ölçeklendirmeyi kapsar iki eylemlerin: uygulama hizmeti planınızın değiştirilmesi için daha yüksek bir fiyatlandırma katmanı ve daha yüksek fiyatlandırma katmanı geçtikten sonra belirli ayarlarını yapılandırma.

Ölçeklendirme ile ilgili daha fazla bilgi için bkz: [bir web uygulamasını Azure App Service'te ölçeklendirme](web-sites-scale.md).

Ayrıca, birden çok örneğinde, uygulamayı çalıştırmayı seçebilirsiniz. Ölçek genişletme yalnızca ile daha fazla işleme yeteneği sağlar, ancak ayrıca miktar hataya dayanıklılık sağlar. İşlem bir örneğinde devre dışı kalırsa, diğer örnekleri isteklere yanıt devam edin.

El ile veya otomatik olarak ölçeklendirme ayarlayabilirsiniz.

#### <a name="use-autoheal"></a>AutoHeal kullanın
AutoHeal (yapılandırma değişiklikleri, istekleri, bellek tabanlı sınırları veya bir isteğin yürütülmesi için gereken süre gibi) seçtiğiniz ayarlara göre uygulamanız için çalışan işlemi geri dönüştürüldüğünde. Çoğu zaman, Geri Dönüşüm işlemi bir sorundan için en hızlı yoludur. Her zaman doğrudan Azure portalı içinde web uygulamasından yeniden başlatabilirsiniz olsa AutoHeal bunu otomatik olarak sizin için yapar. Yapmanız gereken tek şey, web uygulamanız için kök web.config dosyasında bazı tetikleyicileri ekleyin. Uygulamanız .net uygulaması olsa bile bu ayarları aynı şekilde çalışır.

Daha fazla bilgi için bkz: [Azure Web siteleri otomatik düzeltme](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).

#### <a name="restart-the-web-app"></a>Web uygulaması yeniden
Yeniden başlatmayı çoğunlukla tek seferlik sorunları kurtarmak için kolay yoludur. Üzerinde [Azure portal](https://portal.azure.com/), web uygulamanızın dikey penceresinde, uygulamanızı yeniden başlatmak veya durdurmak için seçeneğiniz vardır.

 ![Performans sorunlarını çözmek için web uygulaması yeniden](./media/app-service-web-troubleshoot-performance-degradation/2-restart.png)

Web uygulamanızı Azure Powershell kullanarak da yönetebilirsiniz. Daha fazla bilgi için bkz. [Azure PowerShell'i Azure Resource Manager ile kullanma](../powershell-azure-resource-manager.md).
