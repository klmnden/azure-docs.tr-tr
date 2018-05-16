---
title: Azure web uygulamaları için uygulama performansı ile ilgili SSS | Microsoft Docs
description: Azure App Service Web Apps özelliğini kullanılabilirliği, performansı ve uygulama sorunları hakkında sık sorulan soruların yanıtlarını alın.
services: app-service\web
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: 9c8eda1c4a65465a93b9c3f9caf23a6663073f26
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="application-performance-faqs-for-web-apps-in-azure"></a>Azure Web uygulamaları için uygulama performansı ile ilgili SSS

Bu makale için uygulama performans sorunları hakkında sık sorulan sorular (SSS) yanıtlarını sahip [Azure App Service Web Apps özelliğini](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-is-my-app-slow"></a>Neden Uygulamam yavaş mı?

Birden çok faktörünü uygulama performansı yavaş katkıda bulunabilir. Ayrıntılı sorun giderme adımları için bkz [sorun giderme yavaş web uygulaması performans](app-service-web-troubleshoot-performance-degradation.md).

## <a name="how-do-i-troubleshoot-a-high-cpu-consumption-scenario"></a>Yüksek CPU tüketimi senaryo nasıl sorun giderme?

Bazı yüksek CPU tüketimi senaryolarda uygulamanızı gerçekten daha fazla bilgi işlem kaynakları gerektirebilir. Bu durumda, uygulama gerekli tüm kaynakları alır için daha yüksek bir hizmet katmanı için ölçeklendirme göz önünde bulundurun. Diğer durumlarda, hatalı bir döngü veya kodlama bir uygulama tarafından yüksek CPU tüketimi kaynaklanabilir. Daha yüksek CPU kullanımına neden olan içine Insight alma iki parçalı bir işlemdir. İlk olarak, işlem dökümü oluşturun ve işlem dökümü analiz edin. Daha fazla bilgi için bkz: [yakalamak ve Web uygulamaları için yüksek CPU tüketimi için döküm dosyasını çözümlemek](https://blogs.msdn.microsoft.com/asiatech/2016/01/20/how-to-capture-dump-when-intermittent-high-cpu-happens-on-azure-web-app/).

## <a name="how-do-i-troubleshoot-a-high-memory-consumption-scenario"></a>Yüksek bellek tüketimine senaryo nasıl sorun giderme?

Bazı yüksek bellek tüketimine senaryolarda uygulamanızı gerçekten daha fazla bilgi işlem kaynakları gerektirebilir. Bu durumda, uygulama gerekli tüm kaynakları alır için daha yüksek bir hizmet katmanı için ölçeklendirme göz önünde bulundurun. Diğer durumlarda, bir bellek sızıntısı kodda bir hata neden olabilir. Bir kodlama yöntemi de bellek tüketimi artırabilir. Ne yüksek bellek tüketimi iki parçalı bir işlemdir tetikleme içine Insight alınıyor. İlk olarak, işlem dökümü oluşturun ve işlem dökümü analiz edin. Azure Site uzantısı galerisinden kilitlenme tanılayıcı verimli bir şekilde hem bu adımları gerçekleştirebilir. Daha fazla bilgi için bkz: [yakalamak ve Web uygulamaları için bir döküm dosyası aralıklı yüksek bellek için analiz](https://blogs.msdn.microsoft.com/asiatech/2016/02/02/how-to-capture-and-analyze-dump-for-intermittent-high-memory-on-azure-web-app/).

## <a name="how-do-i-automate-app-service-web-apps-by-using-powershell"></a>App Service web apps PowerShell kullanarak nasıl otomatikleştirmek?

Yönetmek ve App Service web apps korumak için PowerShell cmdlet'lerini kullanabilirsiniz. Bizim blog yayını içinde [PowerShell kullanarak Azure App Service içinde barındırılan web uygulamaları otomatikleştirmek](https://blogs.msdn.microsoft.com/puneetgupta/2016/03/21/automating-webapps-hosted-in-azure-app-service-through-powershell-arm-way/), biz Azure Resource Manager tabanlı PowerShell cmdlet'leri ortak görevleri otomatik hale getirmek için nasıl kullanılacağını açıklar. Blog postası çeşitli web apps yönetim görevleri için örnek kod da sahiptir. Açıklamaları ve tüm App Service web apps cmdlet'leri için sözdizimi için bkz: [AzureRM.Websites](https://docs.microsoft.com/powershell/module/azurerm.websites/?view=azurermps-4.0.0).

## <a name="how-do-i-view-my-web-apps-event-logs"></a>My web uygulamanızın olay günlüklerini nasıl görüntüleyebilirim?

Web uygulamanızın olay günlüklerini görüntülemek için:

1. Oturum açın, [Kudu Web sitesi](https://*yourwebsitename*.scm.azurewebsites.net).
2. Menüde seçin **hata ayıklama Konsolu'nda** > **CMD**.
3. Seçin **LogFiles** klasör.
4. Olay günlüklerini görüntülemek için Kurşun Kalem simgesini seçin **eventlog.xml**.
5. Günlükleri indirmek için PowerShell cmdlet'ini çalıştırın `Save-AzureWebSiteLog -Name webappname`.

## <a name="how-do-i-capture-a-user-mode-memory-dump-of-my-web-app"></a>My web uygulaması, bir kullanıcı modu bellek dökümü nasıl yakalama?

Kullanıcı modu bellek dökümü, web uygulamanızın yakalamak için:

1. Oturum açın, [Kudu Web sitesi](https://*yourwebsitename*.scm.azurewebsites.net).
2. Seçin **işlem Gezgini'ni** menüsü.
3. Sağ **w3wp.exe** işlem veya Web işi işlem.
4. Seçin **karşıdan bellek dökümü** > **tam döküm**.

## <a name="how-do-i-view-process-level-info-for-my-web-app"></a>Web Uygulamam için nasıl işlem düzeyinde bilgisi görüntüleyebilirim?

Web uygulamanız için işlem düzeyinde bilgilerini görüntülemek için iki seçeneğiniz vardır:

*   Azure portalında:
    1. Açık **işlem Gezgini'ni** web uygulaması için.
    2. Ayrıntıları görmek için seçin **w3wp.exe** işlemi.
*   Kudu konsolunda:
    1. Oturum açın, [Kudu Web sitesi](https://*yourwebsitename*.scm.azurewebsites.net).
    2. Seçin **işlem Gezgini'ni** menüsü.
    3. İçin **w3wp.exe** işlemi, select **özellikleri**.

## <a name="when-i-browse-to-my-app-i-see-error-403---this-web-app-is-stopped-how-do-i-resolve-this"></a>Uygulamama göz attığınızda, "Hatası 403 - bu web uygulaması durduruldu." görüyorum Bu nasıl giderebilirim?

Üç koşul bu hataya neden olabilir:

* Web uygulaması bir fatura sınırına ulaştı ve sitenizi devre dışı bırakıldı.
* Web uygulaması portalda durduruldu.
* Web uygulaması serbest için geçerli olabilecek veya ölçek hizmet planı paylaşılan bir kaynak kota sınırına ulaştı.

Hataya neden olan bakın ve sorunu çözmek için adımları [Web Apps: "Hatası 403 – bu web uygulaması durdurulduğunda"](https://blogs.msdn.microsoft.com/waws/2016/01/05/azure-web-apps-error-403-this-web-app-is-stopped/).

## <a name="where-can-i-learn-more-about-quotas-and-limits-for-various-app-service-plans"></a>Burada çeşitli uygulama hizmeti planları için kotalar ve sınırlar hakkında daha fazla bilgi edinebilirsiniz?

Kotalar ve sınırlar hakkında daha fazla bilgi için bkz: [App Service sınırları](../azure-subscription-service-limits.md#app-service-limits). 

## <a name="how-do-i-decrease-the-response-time-for-the-first-request-after-idle-time"></a>Boşta kalma süresi sonra ilk istek için yanıt süresi nasıl azaltmak?

Varsayılan olarak ayarlanmış bir süre için boşta olmaları durumunda web uygulamaları kaldırılır. Bu şekilde, sistem kaynakların tasarrufu yapabilirsiniz. Dezavantajı, web uygulaması kaldırılmadan sonra yapılan ilk istek yanıtı yüklenip yanıtlara hizmet başlangıç web uygulaması izin vermek için uzun olmasıdır. Temel ve standart hizmeti planlarında, açabilirsiniz **her zaman açık** her zaman yüklenen uygulama tutmak için ayarı. Bu uygulama boşta kaldıktan sonra uzun yükleme süreleri ortadan kaldırır. Değiştirmek için **her zaman açık** ayarı:

1. Azure portalında web uygulamanıza gidin.
2. Seçin **uygulama ayarları**.
3. İçin **her zaman açık**seçin **üzerinde**.

## <a name="how-do-i-turn-on-failed-request-tracing"></a>Başarısız istek izlemeyi nasıl kapatırım?

Başarısız istek izleme özelliğini açmak için:

1. Azure portalında web uygulamanıza gidin.
3. Seçin **tüm ayarları** > **tanılama günlükleri**.
4. İçin **başarısız istek izleme**seçin **üzerinde**.
5. **Kaydet**’i seçin.
6. Web uygulaması dikey penceresinde, seçin **Araçları**.
7. Seçin **Visual Studio Online**.
8. Ayar değilse **üzerinde**seçin **üzerinde**.
9. Seçin **Git**.
10. Seçin **Web.config**.
11. System.webServer içinde bu yapılandırma (belirli bir URL yakalamak için) ekleyin:

    ```
    <system.webServer>
    <tracing> <traceFailedRequests>
    <remove path="*api*" />
    <add path="*api*">
    <traceAreas>
    <add provider="ASP" verbosity="Verbose" />
    <add provider="ASPNET" areas="Infrastructure,Module,Page,AppServices" verbosity="Verbose" />
    <add provider="ISAPI Extension" verbosity="Verbose" />
    <add provider="WWW Server" areas="Authentication,Security,Filter,StaticFile,CGI,Compression, Cache,RequestNotifications,Module,FastCGI" verbosity="Verbose" />
    </traceAreas>
    <failureDefinitions statusCodes="200-999" />
    </add> </traceFailedRequests>
    </tracing>
    ```
12. Yavaş performans sorunlarını gidermek için bu yapılandırma (Yakalama isteği 30 saniyeden fazla sürüyorsa) ekleyin:
    ```
    <system.webServer>
    <tracing> <traceFailedRequests>
    <remove path="*" />
    <add path="*">
    <traceAreas> <add provider="ASP" verbosity="Verbose" />
    <add provider="ASPNET" areas="Infrastructure,Module,Page,AppServices" verbosity="Verbose" />
    <add provider="ISAPI Extension" verbosity="Verbose" />
    <add provider="WWW Server" areas="Authentication,Security,Filter,StaticFile,CGI,Compression, Cache,RequestNotifications,Module,FastCGI" verbosity="Verbose" />
    </traceAreas>
    <failureDefinitions timeTaken="00:00:30" statusCodes="200-999" />
    </add> </traceFailedRequests>
    </tracing>
    ```
13. Başarısız istek izlemelerin de indirmek için [portal](https://portal.azure.com)Web sitesine gidin.
15. Seçin **Araçları** > **Kudu** > **Git**.
18. Menüde seçin **hata ayıklama Konsolu'nda** > **CMD**.
19. Seçin **LogFiles** klasörünü ve ardından ile başlayan bir ada sahip klasör **W3SVC**.
20. XML dosyasını görmek için Kurşun Kalem simgesini seçin.

## <a name="i-see-the-message-worker-process-requested-recycle-due-to-percent-memory-limit-how-do-i-address-this-issue"></a>"Çalışan işlem 'Yüzde bellek' sınırı nedeniyle geri dönüşüm isteğinde." iletisini görüyorum Bu sorunu nasıl ele?

En fazla kullanılabilir bellek miktarını 32 bitlik bir işlem için (hatta bir 64-bit işletim sisteminde) 2 GB'dir. Varsayılan olarak, çalışan işlem için 32-bit ayarlanır (eski web uygulamalarıyla uyumluluk için) App Service'te.

64 bit işlemleri ek belleğe Web çalışanı rolünüz yararlanabilir şekilde değiştirmeyi düşünün. Bu web uygulamasını yeniden başlatır, böylece buna uygun şekilde zamanlayın.

Ayrıca bir 64-bit ortamı bir temel veya standart hizmet planı gerektirdiğini unutmayın. Ücretsiz ve paylaşılan planları her zaman bir 32 bit ortamda çalıştırılabilir.

Daha fazla bilgi için bkz: [App Service'te web uygulamalarını yapılandırma](web-sites-configure.md).

## <a name="why-does-my-request-time-out-after-240-seconds"></a>Neden my isteği zaman aşımına 240 saniye sonra?

Azure yük dengeleyici, dört dakikalık varsayılan boşta kalma zaman aşımı ayarının. Bu genellikle bir web isteği için makul yanıt zaman sınırı içindir. Web uygulamanızın arka plan işleme gerektiriyorsa, Azure Web işleri kullanmanızı öneririz. Azure web uygulaması Web işleri çağırabilir ve arka plan işlemi tamamlandığında bildirim. Web işleri, kuyruklar ve Tetikleyicileri dahil olmak üzere birden çok yöntemler arasından seçim yapabilirsiniz.

Web işleri arka plan işleme için tasarlanmıştır. WebJob içinde istediğiniz kadar arka plan işlemi yapabilirsiniz. Web işleri hakkında daha fazla bilgi için bkz: [arka plan görevleri ile Web işleri çalıştırma](web-sites-create-web-jobs.md).

## <a name="aspnet-core-applications-that-are-hosted-in-app-service-sometimes-stop-responding-how-do-i-fix-this-issue"></a>App Service içinde bazen barındırılan ASP.NET Core uygulamaları yanıt vermiyor. Bu sorunu nasıl düzeltirim?

Daha önceki bir bilinen bir sorun [Kestrel sürüm](https://github.com/aspnet/KestrelHttpServer/issues/1182) barındırılan ASP.NET Core 1.0 uygulama zaman zaman yanıt vermemesine App Service'te neden olabilir. Ayrıca, bu iletiyi görebilirsiniz: "belirtilen CGI uygulaması hatayla karşılaştı ve sunucu işlemi sonlandırdı."

Bu sorun Kestrel sürüm 1.0.2 düzeltilmiştir. Bu sürüm bulunur ASP.NET Core 1.0.3 güncelleştirin. Bu sorunu çözmek için Kestrel 1.0.2 kullanmak için uygulama bağımlılıkları güncelleştirdiğinizden emin olun. Blog gönderisinde açıklandığı iki geçici çözümlerden birini kullanmanız alternatif olarak, [ASP.NET Core 1.0 yavaş performans sorunlarını App Service web apps](https://blogs.msdn.microsoft.com/waws/2016/12/11/asp-net-core-slow-perf-issues-on-azure-websites).


## <a name="i-cant-find-my-log-files-in-the-file-structure-of-my-web-app-how-can-i-find-them"></a>Web Uygulamam dosya yapısı içinde my günlük dosyalarını bulmak olamaz. Bunları nasıl bulabilirim?

Uygulama hizmeti, yerel önbellek özelliğini kullanırsanız, uygulama hizmet örneğiniz LogFiles ve veri klasörünün klasör yapısını etkilenir. Yerel önbelleği kullanıldığında, alt klasörler depolama LogFiles ve veri klasörleri oluşturulur. Alt klasörleri adlandırma deseni "benzersiz tanımlayıcı" + zaman damgası kullanın. Her alt web uygulaması çalıştıran veya çalışan bir VM örneğine karşılık gelir.

Yerel önbelleği kullanarak olup olmadığını belirlemek için uygulama hizmetiniz denetleyin **uygulama ayarları** sekmesi. Uygulama ayarı yerel önbelleği kullanılıyorsa, `WEBSITE_LOCAL_CACHE_OPTION` ayarlanır `Always`.

Yerel önbelleği kullanmıyorsanız ve bu sorunu yaşıyorsanız, bir destek isteği gönderin.

## <a name="i-see-the-message-an-attempt-was-made-to-access-a-socket-in-a-way-forbidden-by-its-access-permissions-how-do-i-resolve-this"></a>"Bir yuva erişim izinlerini tarafından yasaklanmış bir şekilde erişmek için girişimde bulunuldu." iletisini görüyorum Bu nasıl giderebilirim?

VM örneği üzerinde giden TCP bağlantılarını azalırsa genellikle bu hata oluşur. App Service'te her bir VM örneği için yapılan giden bağlantılar, en fazla sayısını sınırlar. Daha fazla bilgi için bkz: [arası VM sayısal sınırlar](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#cross-vm-numerical-limits).

Uygulamadan yerel adres erişmeye çalıştığınızda bu hatayı de oluşabilir. Daha fazla bilgi için bkz: [yerel adres istekleri](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#local-address-requests).

Web uygulamanızda giden bağlantılar hakkında daha fazla bilgi için ilgili postasına bakın [Azure Web sitelerine bağlantılar giden](http://www.freekpaans.nl/2015/08/starving-outgoing-connections-on-windows-azure-web-sites/).

## <a name="how-do-i-use-visual-studio-to-remote-debug-my-app-service-web-app"></a>Uzaktan için Visual Studio nasıl kullanırım App Service web Uygulamam hata ayıklama?

Visual Studio kullanarak web uygulamanızın hatalarını ayıklama nasıl oluşturulduğunu gösteren ayrıntılı bilgi için bkz: [uzaktan hata ayıklama, App Service web uygulaması](https://blogs.msdn.microsoft.com/benjaminperkins/2016/09/22/remote-debug-your-azure-app-service-web-app/).
