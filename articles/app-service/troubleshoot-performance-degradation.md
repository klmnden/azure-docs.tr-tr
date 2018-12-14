---
title: Performans düşüşü - Azure App Service'e giderme | Microsoft Docs
description: Bu makale, Azure App Service'te yavaş web uygulaması performans sorunlarını gidermenize yardımcı olur.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
tags: top-support-issue
keywords: Web uygulaması performansını yavaş uygulama uygulaması yavaş
ms.assetid: b8783c10-3a4a-4dd6-af8c-856baafbdde5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2016
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: c0642b24fc8eefd210d5b6b58e7d0c772408166a
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53389109"
---
# <a name="troubleshoot-slow-web-app-performance-issues-in-azure-app-service"></a>Azure App Service'te yavaş web uygulaması performans sorunlarını giderme
Bu makale, yavaş bir web uygulaması performans sorunlarını gidermenize yardımcı olur. [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714).

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) tıklayın **destek al**.

## <a name="symptom"></a>Belirti
Web uygulaması, sayfa yükü yavaş göz atarken ve bazen zaman aşımı.

## <a name="cause"></a>Nedeni
Bu sorun genellikle gibi uygulama düzeyinde sorunlarını tarafından kaynaklanır:

* ağ isteklerine uzun sürüyor
* verimli olan uygulama kodu veya veritabanı sorguları
* yüksek bellek/CPU kullanarak uygulama
* bir özel durum nedeniyle kilitlenme uygulama

## <a name="troubleshooting-steps"></a>Sorun giderme adımları
Sorun giderme sırayla üç ayrı görevlere ayrılabilir:

1. [İnceleyin ve uygulama davranışını izleme](#observe)
2. [Veri toplama](#collect)
3. [Sorunu gidermek](#mitigate)

[App Service Web Apps](app-service-web-overview.md) her adımda çeşitli seçenekler sunar.

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a>1. İnceleyin ve uygulama davranışını izleme
#### <a name="track-service-health"></a>Hizmet durumu izleme
Microsoft Azure hizmet kesintisi veya performans düşüşü olduğundan her zaman publicizes. Hizmetinin durumunu takip edebilirsiniz [Azure portalında](https://portal.azure.com/). Daha fazla bilgi için [hizmet durumunu izleme](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>Web uygulamanızı izleme
Bu seçenek, uygulamanızı herhangi bir sorun olması durumunda öğrenmek sağlar. Web uygulamanızın dikey penceresinde **istekler ve hatalar** Döşe. **Ölçüm** dikey ekleyebileceğiniz tüm ölçümleri gösterir.

Bazı web uygulamanızı izlemek için isteyebilirsiniz ölçümleri

* Ortalama bellek çalışma kümesi
* Ortalama yanıt süresi
* CPU süresi
* Bellek çalışma kümesi
* İstekler

![Web uygulaması performansını izleme](./media/app-service-web-troubleshoot-performance-degradation/1-monitor-metrics.png)

Daha fazla bilgi için bkz.

* [Azure App Service'te Web uygulamalarını izleme](web-sites-monitor.md)
* [Uyarı bildirimleri alma](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

#### <a name="monitor-web-endpoint-status"></a>Web uç noktası durumunu izleme
Web uygulamanızı çalıştırıyorsanız, **standart** fiyatlandırma katmanı, Web Apps iki uç nokta üç coğrafi konumlardan izlemenize izin verir.

Uç nokta izleme web testleri test yanıt süresi ve web URL'leri süresinin coğrafi olarak dağıtılmış konumlardan yapılandırır. Test çalışma süresi ve yanıt süresi her konumdan belirlemek için web URL'si bir HTTP GET işlemi gerçekleştirir. Yapılandırılmış her yere beş dakikada bir test çalıştırır.

HTTP yanıt kodları kullanılarak izlenen çalışma süresi ve yanıt süresi, milisaniye cinsinden ölçülür. HTTP yanıt kodu 400'e eşit veya daha büyük ise ya da yanıtın 30 saniyeden uzun sürerse izleme testi başarısız. Bir uç nokta izleme testlerinin belirtilen tüm konumlarda başarılı olması için kullanılabilir olarak kabul edilir.

Bunu ayarlamak için bkz: [Azure App Service'te uygulamaları izleme](web-sites-monitor.md).

Ayrıca bkz [yanı sıra uç nokta izleme - Stefan Schackow ile Azure Web siteleri tutma](https://channel9.msdn.com/Shows/Azure-Friday/Keeping-Azure-Web-Sites-up-plus-Endpoint-Monitoring-with-Stefan-Schackow) uç nokta izleme bir video için.

#### <a name="application-performance-monitoring-using-extensions"></a>Uzantıları kullanarak uygulama performansı izleme
Kullanarak da uygulama performansınızı izleyebilirsiniz bir *site uzantısı*.

Her App Service web uygulaması, site uzantısı dağıtılan araçları güçlü bir dizi kullanmanıza olanak sağlayan bir genişletilebilen yönetim uç noktası sağlar. Uzantılara şunlar dahildir: 

- Kaynak kod düzenleyicilerinden ister [Azure DevOps](https://www.visualstudio.com/products/what-is-visual-studio-online-vs.aspx). 
- Yönetim Araçları gibi bir MySQL veritabanına bağlı kaynaklar için bir web uygulamasına bağlı.

[Azure Application Insights](https://azure.microsoft.com/services/application-insights/) olduğu bir performans izleme de kullanılabilir olan site uzantısı. Application Insights'ı kullanmak için bir SDK'sı ile kodunuzu yeniden oluşturun. Ayrıca, ek verilere erişim sağlayan bir uzantı yükleyebilirsiniz. SDK'sı, uygulamanızın daha ayrıntılı performans ve kullanımı izlemek için kod yazmanıza olanak sağlar. Daha fazla bilgi için [web uygulamalarının performansını izleme](../application-insights/app-insights-web-monitor-performance.md).

<a name="collect" />

### <a name="2-collect-data"></a>2. Veri toplama
Web Apps ortamı, günlük bilgilerini hem web sunucusu, hem de web uygulaması için tanılama işlevi sağlar. Bilgiler, web sunucusu tanılama ve uygulama tanılama olarak ayrılmıştır.

#### <a name="enable-web-server-diagnostics"></a>Web sunucusu tanılamayı etkinleştirme
Etkinleştirmek veya günlükleri aşağıdaki türde devre dışı bırakabilirsiniz:

* **Ayrıntılı hata günlüğü** -ayrıntılı hata bilgileri belirten bir hata (durum kodu 400 veya üzeri) HTTP durum kodları için. Bu sunucunun döndürülen hata kodu neden belirlemek yardımcı olabilecek bilgiler içerebilir.
* **Başarısız istek izleme** -ayrıntılı bir izleme isteği ve her bir bileşende geçen süre işlemek için kullanılan IIS bileşenlerini de dahil olmak üzere, başarısız isteklerle bilgileri. Bu, web uygulaması performansını artırmak veya belirli bir HTTP hatası neden olan yalıtmak çalışıyorsanız yararlı olabilir.
* **Web sunucusu günlüğe kaydetme** -HTTP işlemlerini W3C Genişletilmiş günlük dosyası biçimini kullanarak hakkında bilgi. Bu, işlenen isteklerin veya özel bir IP adresinden kaç isteklerdir sayısı gibi genel web uygulaması ölçümleri belirlerken kullanışlıdır.

#### <a name="enable-application-diagnostics"></a>Uygulama tanılamayı etkinleştirme
Web uygulamaları, uygulamanızı Visual Studio'dan Canlı ya da daha fazla bilgi ve izlemeleri günlüğe kaydetmek için uygulama kodunu değiştirmeniz profili uygulama performans verilerini toplamak için birkaç seçenek vardır. Uygulama ve izlemeden gözlemlenen zorunda ne kadar erişim aracı tabanlı seçenekleri seçebilirsiniz.

##### <a name="use-application-insights-profiler"></a>Application Insights Profiler ' ı kullanın
Ayrıntılı performans izlemeleri yakalama başlatmak Application Insights Profiler etkinleştirebilirsiniz. En fazla yakalanan izlere erişebilirsiniz beş sorunları araştırmak için gerektiğinde gün önce gerçekleşmiş geçmiş. Azure Portal'da web uygulamasının Application Insights kaynağına erişimi olduğu sürece, bu seçeneği seçebilirsiniz.

Application Insights Profiler, hangi kod satırının yavaş yanıtlar neden gösteren yanıt süresi içinde her bir web çağrısı ve izlemeler için istatistikler sağlar. App Service uygulaması belirli kod içinde bir yüksek performanslı yazılmaz çünkü bazen yavaş yolu. Kilit çakışması veritabanı paralel ve istenmeyen çalıştırılabilir sıralı kod örnekleri içerir. Bu performans sorunlarını kodda kaldırma uygulamanın performansı artırır ancak ayrıntılı izlemelerini ve günlüklerini ayarlama olmadan algılanması zor olur. Application Insights Profiler ' ı tarafından toplanan izlemeleri, uygulamayı yavaşlatan kod satırlarını tanımlama yardımcı olur ve App Service uygulamaları için bu zorluğun üstesinden gelmek.

 Daha fazla bilgi için [profil oluşturma Application Insights ile canlı Azure web apps](../application-insights/app-insights-profiler.md).

##### <a name="use-remote-profiling"></a>Uzaktan profil oluşturma kullanın
Azure App Service'te Web Apps, API Apps ve WebJobs uzaktan profil oluşturulabilir. Web uygulaması kaynağına erişiminiz ve sorunu yeniden oluşturmak nasıl bildiğiniz veya performans sorunu tam zaman aralığını biliyorsanız olur, bu seçeneği belirleyin.

Uzak profil oluşturma işleminin CPU kullanımını yüksek olduğunda, işlem beklenenden daha yavaş çalışıyor veya HTTP isteklerin gecikme süresi Normalden yüksek yaparsanız uzaktan işleminizi profil ve işlem çözümlemek için CPU örnekleme çağrı yığınlarını alma yararlıdır etkinlik ve kod etkin yolları.

Daha fazla bilgi için [Azure App Service'te uzaktan profil oluşturma desteği](https://azure.microsoft.com/blog/remote-profiling-support-in-azure-app-service).

##### <a name="set-up-diagnostic-traces-manually"></a>Tanılama izlemeleri el ile ayarlama
Web uygulama kaynak koduna erişiminiz varsa, uygulama Tanılama web uygulaması tarafından üretilen bilgileri yakalamanıza olanak sağlar. ASP.NET uygulamalarında kullanabileceğiniz `System.Diagnostics.Trace` uygulama tanılama günlüğüne bilgileri günlüğe kaydetmek için sınıf. Ancak, kodu değiştirin ve uygulamanızı yeniden dağıtmanız gerekir. Uygulamanızı bir sınama ortamında çalışıyorsa bu yöntem tavsiye edilir.

Günlüğe kaydetme için uygulamanızı yapılandırma hakkında ayrıntılı yönergeler için bkz. [Azure App Service'te web apps için tanılama günlüğünü etkinleştirme](web-sites-enable-diagnostic-log.md).

#### <a name="use-the-diagnostics-tool"></a>Tanılama aracını kullanma
App Service, web uygulamanızı yapılandırma gerektirmeden gidermenize yardımcı olması için akıllı ve etkileşimli bir deneyim sağlar. Web uygulamanızla bir sorunla karşılaşırsanız çalıştırdığınızda, Tanılama aracını daha kolay ve hızlı bir şekilde ve sorunu gidermek için doğru bilgileri size yol göstermesi sorunun ne olduğunu gösterir.

App Service tanılamasını erişmek için App Service uygulamanızı veya App Service Ortamı'nda gidin [Azure portalında](https://portal.azure.com). Sol gezinti bölmesinde, tıklayarak **tanılayın ve sorunlarını çözmek**.

#### <a name="use-the-kudu-debug-console"></a>Kudu hata ayıklama konsolunu kullanma
Web Apps, hata ayıklama için keşfetmek, ortamınız hakkında bilgi almak için JSON uç noktaları yanı sıra, dosyaları karşıya yükleme için kullanabileceğiniz bir hata ayıklama konsoluna ile birlikte gelir. Bu konsolu olarak adlandırılan *Kudu konsolunu* veya *SCM Pano* web uygulamanız için.

Bağlantı giderek bu panoya erişebilirsiniz **https://&lt;uygulama adınız >.scm.azurewebsites.net/**.

Kudu sağlayan şeylerden bazıları şunlardır:

* Uygulamanız için ortam ayarları
* günlük akışı
* Tanılama dökümü
* hata ayıklama konsolunu Powershell cmdlet'leri ve temel DOS komutlarını çalıştırabilirsiniz.

Başka bir kullanışlı Kudu durumunda, uygulamanız, ilk şans özel durumları atma Kudu kullanabilirsiniz ve bellek oluşturmak için Procdump SysInternals aracı dökümleri, özelliğidir. Bu bellek dökümlerini işleminin anlık görüntüleri ve genellikle web uygulamanızla daha karmaşık sorunları gidermenize yardımcı olabilir.

Kudu içinde kullanılabilen özellikler hakkında daha fazla bilgi için bkz. [bilmeniz gereken Azure DevOps Araçları](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />

### <a name="3-mitigate-the-issue"></a>3. Sorunu gidermek
#### <a name="scale-the-web-app"></a>Web uygulamasını ölçeklendirme
Azure App Service'te daha yüksek performans ve aktarım hızı, uygulamanızı çalıştıran ölçeği ayarlayabilirsiniz. Bir web uygulamasını ölçeklendirme iki ilgili eylemleri içerir: App Service planınızın değiştirilmesi daha yüksek bir fiyatlandırma katmanına ve daha yüksek bir fiyatlandırma katmanına geçirildikten sonra belirli ayarları yapılandırma.

Ölçeklendirme hakkında daha fazla bilgi için bkz. [web uygulamasını Azure App Service'te ölçeklendirme](web-sites-scale.md).

Ayrıca, birden fazla örneğinde uygulamanızı çalıştırmak seçebilirsiniz. Ölçeği genişletme yalnızca ile daha fazla işleme yeteneği sağlar, ancak Ayrıca, belirli bir miktarda hataya dayanıklılık sağlar. İşlemin bir örneği üzerinde kalırsa diğer örneklerin isteklerine hizmet vermeye devam edin.

El ile veya otomatik olarak ölçeklendirme ayarlayabilirsiniz.

#### <a name="use-autoheal"></a>AutoHeal kullanın
AutoHeal (yapılandırma değişiklikleri, istekler, bellek tabanlı sınırları veya bir isteğin yürütülmesi için gereken süre gibi) seçtiğiniz ayarlara bağlı uygulamanız için çalışan işlemi geri dönüştürür. Çoğu zaman, Geri Dönüşüm işlemi bir sorundan en hızlı yoludur. Her zaman doğrudan Azure portalındaki web uygulamasından yeniden başlatabilirsiniz, ancak AutoHeal bunu otomatik olarak sizin için halleder. Tek yapmak için ihtiyacınız olan kök Web.config dosyasında web uygulamanız için bazı tetikleyiciler ekleyin. Uygulamanızı .net uygulaması olsa bile, bu ayarları aynı şekilde çalışır.

Daha fazla bilgi için [Azure Web siteleri otomatik onarım](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).

#### <a name="restart-the-web-app"></a>Web uygulamasını yeniden başlatın
Yeniden başlatma genellikle tek seferlik sorunları en basit yoludur. Üzerinde [Azure portalında](https://portal.azure.com/), web uygulamanızın dikey penceresinde, durdurmak veya uygulamanızı yeniden seçeneğiniz vardır.

 ![Performans sorunlarını çözmek için web uygulamasını yeniden başlatın](./media/app-service-web-troubleshoot-performance-degradation/2-restart.png)

Web uygulamanızı Azure Powershell kullanarak da yönetebilirsiniz. Daha fazla bilgi için bkz. [Azure PowerShell'i Azure Resource Manager ile kullanma](../powershell-azure-resource-manager.md).
