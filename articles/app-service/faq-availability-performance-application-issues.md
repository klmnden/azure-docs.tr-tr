---
title: Uygulama performansı ile ilgili SSS - Azure App Service | Microsoft Docs
description: Azure App Service'in Web Apps özelliği, kullanılabilirlik, performans ve uygulama sorunları hakkında sık sorulan soruların yanıtlarını alın.
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
ms.date: 10/31/2018
ms.author: genli
ms.custom: seodec18
ms.openlocfilehash: f455985d2a7d05f45100d4a88b43c688fe1a7767
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65955759"
---
# <a name="application-performance-faqs-for-web-apps-in-azure"></a>Azure Web Apps için uygulama performansı ile ilgili SSS

> [!NOTE]
> Bazı yönergeleri yalnızca Windows veya Linux uygulama hizmetleri üzerinde çalışabilir. Örneğin, Linux uygulama Hizmetleri çalıştıran 64 bit modunda varsayılan olarak.
>

Bu makalede, uygulama performans sorunları hakkında sık sorulan sorular (SSS) yanıtlarını bulunur. [Azure App Service'in Web Apps özelliği](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-is-my-app-slow"></a>Uygulamam yavaş neden oluyor?

Çoklu faktörlerle uygulama performansı yavaş katkıda bulunabilir. Ayrıntılı sorun giderme adımları için bkz: [sorun giderme yavaş web uygulaması performansını](troubleshoot-performance-degradation.md).

## <a name="how-do-i-troubleshoot-a-high-cpu-consumption-scenario"></a>Yüksek CPU tüketimi senaryo sorunlarını nasıl giderebilirim?

Bazı yüksek CPU tüketimi senaryolarda, uygulamanızı daha fazla bilgi işlem kaynakları gerçekten gerektirebilir. Bu durumda, uygulama ihtiyaç duyduğu tüm kaynakları alır. Bu nedenle daha yüksek bir hizmet katmanına ölçeklendirmeyi düşünün. Diğer durumlarda, yüksek CPU tüketimi hatalı bir döngü veya kodlama bir uygulama tarafından kaynaklanıyor olabilir. Daha yüksek CPU tüketimi işlemi tetikleniyor Öngörüler alma iki bölümden oluşur. İlk olarak, bir işlem dökümü oluşturur ve sonra işlem dökümü analiz edin. Daha fazla bilgi için [yakalamak ve Web uygulamaları için yüksek CPU tüketimi için bir döküm dosyası çözümleme](https://blogs.msdn.microsoft.com/asiatech/2016/01/20/how-to-capture-dump-when-intermittent-high-cpu-happens-on-azure-web-app/).

## <a name="how-do-i-troubleshoot-a-high-memory-consumption-scenario"></a>Yüksek bellek tüketimi senaryosu sorunlarını nasıl giderebilirim?

Yüksek bellek tüketimi bazı senaryolarda, uygulamanızı daha fazla bilgi işlem kaynakları gerçekten gerektirebilir. Bu durumda, uygulama ihtiyaç duyduğu tüm kaynakları alır. Bu nedenle daha yüksek bir hizmet katmanına ölçeklendirmeyi düşünün. Diğer durumlarda, bir bellek sızıntısı koddaki bir hata neden olabilir. Bir kodlama uygulama bellek tüketimini da artırabilir. Ne tüketim iki bölümden oluşur, yüksek bellek harekete Öngörüler alınıyor. İlk olarak, bir işlem dökümü oluşturur ve sonra işlem dökümü analiz edin. Azure Site uzantı Galerisi kilitlenme tanılayıcı verimli bir şekilde hem de bu adımları gerçekleştirebilir. Daha fazla bilgi için [yakalamak ve Web uygulamaları için aralıklı yüksek bellek için bir döküm dosyası çözümleme](https://blogs.msdn.microsoft.com/asiatech/2016/02/02/how-to-capture-and-analyze-dump-for-intermittent-high-memory-on-azure-web-app/).

## <a name="how-do-i-automate-app-service-web-apps-by-using-powershell"></a>App Service web apps PowerShell kullanarak nasıl otomatikleştirebilirim?

App Service web apps korumak ve yönetmek için PowerShell cmdlet'lerini kullanabilirsiniz. Blog gönderimizi içinde [PowerShell kullanarak Azure App Service'te barındırılan web uygulamalarını otomatikleştirmek](https://blogs.msdn.microsoft.com/puneetgupta/2016/03/21/automating-webapps-hosted-in-azure-app-service-through-powershell-arm-way/), biz ortak görevleri otomatik hale getirmek için Azure Resource Manager tabanlı PowerShell cmdlet'lerini kullanmak nasıl açıklar. Blog gönderisinde, çeşitli web apps yönetim görevleri için örnek kod de vardır. Açıklamaları ve tüm App Service web uygulamaları cmdlet'leri için söz dizimi için bkz: [Az.Websites](/powershell/module/az.websites).

## <a name="how-do-i-view-my-web-apps-event-logs"></a>My web uygulamasının olay günlüklerini nasıl görüntüleyebilirim?

Web uygulamanızın olay günlüklerini görüntülemek için:

1. Oturum açın, [Kudu Web sitesi](https://*yourwebsitename*.scm.azurewebsites.net).
2. Menüde **hata ayıklama konsolunu** > **CMD**.
3. Seçin **LogFiles** klasör.
4. Olay günlüklerini görüntüleyin, yanındaki kalem simgesini seçin **eventlog.xml**.
5. Günlükleri indirmek için PowerShell cmdlet'ini çalıştırın `Save-AzureWebSiteLog -Name webappname`.

## <a name="how-do-i-capture-a-user-mode-memory-dump-of-my-web-app"></a>Bir kullanıcı modu bellek dökümü web uygulamamı nasıl yakalama?

Web uygulamanızın kullanıcı modu bellek dökümü yakalamak için:

1. Oturum açın, [Kudu Web sitesi](https://*yourwebsitename*.scm.azurewebsites.net).
2. Seçin **Process Explorer** menüsü.
3. Sağ **w3wp.exe** işlem veya WebJob işleminizi.
4. Seçin **bellek dökümünü indirin** > **tam döküm**.

## <a name="how-do-i-view-process-level-info-for-my-web-app"></a>İşlem düzeyi bilgileri web uygulamamı nasıl görüntüleyebilirim?

Web uygulamanız için işlem düzeyinde bilgileri görüntülemek için iki seçeneğiniz vardır:

*   Azure portalında:
    1. Açık **Process Explorer** web uygulaması için.
    2. Ayrıntıları görmek için seçin **w3wp.exe** işlem.
*   Kudu konsolunda:
    1. Oturum açın, [Kudu Web sitesi](https://*yourwebsitename*.scm.azurewebsites.net).
    2. Seçin **Process Explorer** menüsü.
    3. İçin **w3wp.exe** select işlemi **özellikleri**.

## <a name="when-i-browse-to-my-app-i-see-error-403---this-web-app-is-stopped-how-do-i-resolve-this"></a>Ben uygulamama göz attığınızda, "Hata 403 - bu web uygulaması durduruldu." görüyorum Bu nasıl giderebilirim?

Üç koşul bu hataya neden olabilir:

* Web uygulaması, bir faturalama sınırına ulaşmıştır ve siteniz devre dışı bırakıldı.
* Portalda web uygulaması durduruldu.
* Web uygulamasını ücretsiz olarak geçerli olabilecek veya ölçek hizmet planı paylaştığı bir kaynak kotası sınırına ulaştı.

Hataya neden olan bakın ve sorunu gidermek için adımları [Web uygulamaları: "Hata 403 – bu web uygulaması durduruldu"](https://blogs.msdn.microsoft.com/waws/2016/01/05/azure-web-apps-error-403-this-web-app-is-stopped/).

## <a name="where-can-i-learn-more-about-quotas-and-limits-for-various-app-service-plans"></a>Çeşitli App Service planları için kotalar ve sınırlar hakkında nereden bilgi edinebilirim?

Kotalar ve sınırlar hakkında daha fazla bilgi için bkz. [App Service limitleri](../azure-subscription-service-limits.md#app-service-limits). 

## <a name="how-do-i-decrease-the-response-time-for-the-first-request-after-idle-time"></a>İlk istek için yanıt süresi boşta kalma süresi sonrasında nasıl azaltma?

Belirli bir süre için boşta olmaları durumunda varsayılan olarak, web uygulamaları kaldırılır. Bu şekilde, sistem kaynakları tasarruf edebilirsiniz. Olumsuz tarafı, web uygulaması kaldırılmadan sonra ilk isteğin yanıtını yük ve yanıt tanıtıcısıyla başlatmak web uygulamasına izin vermek için uzun olmasıdır. Temel ve standart hizmet planlarında etkinleştirebilirsiniz **Always On** uygulama her zaman tutmak ayarı. Bu, uygulamanın boşta kaldıktan sonra uzun yükleme sürelerini ortadan kaldırır. Değiştirilecek **Always On** ayarı:

1. Azure portalında web uygulamanıza gidin.
2. Seçin **uygulama ayarları**.
3. İçin **Always On**seçin **üzerinde**.

## <a name="how-do-i-turn-on-failed-request-tracing"></a>Başarısız istek izlemeyi nasıl kapatırım?

Başarısız istek izlemeyi etkinleştirmek için:

1. Azure portalında web uygulamanıza gidin.
3. Seçin **tüm ayarlar** > **tanılama günlükleri**.
4. İçin **başarısız istek izleme**seçin **üzerinde**.
5. **Kaydet**’i seçin.
6. Web uygulaması dikey penceresinde seçin **Araçları**.
7. Seçin **Visual Studio Team Services**.
8. Ayar değilse **üzerinde**seçin **üzerinde**.
9. Seçin **Git**.
10. Seçin **Web.config**.
11. System.webServer bu yapılandırma (belirli bir URL'ye yakalamak için) ekleyin:

    ```xml
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
12. Yavaş performans sorunlarını gidermek için bu yapılandırma (Yakalama isteği 30 saniyeden uzun sürerse) ekleyin:
    ```xml
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
13. Başarısız istek izlemelerin de indirmek için [portalı](https://portal.azure.com)Web sitenize gidin.
15. Seçin **Araçları** > **Kudu** > **Git**.
18. Menüde **hata ayıklama konsolunu** > **CMD**.
19. Seçin **LogFiles** klasörü ve ardından çizgi ile başlayan bir ada sahip klasör **W3SVC**.
20. XML dosyasını görmek için kalem simgesini seçin.

## <a name="i-see-the-message-worker-process-requested-recycle-due-to-percent-memory-limit-how-do-i-address-this-issue"></a>"Çalışan işlemi 'Yüzde bellek' sınırı nedeniyle geri dönüşüm istedi." iletisini görüyorum Bu sorunu nasıl ele?

En fazla kullanılabilir bellek miktarı için 32 bitlik bir işlem (bile bir 64-bit işletim sistemi) 2 GB'dir. Varsayılan olarak, 32 bit çalışan işlemi ayarlanır (eski web uygulamalarıyla uyumluluk için) App Service'te.

Web çalışanı rolünüz ek bellek yararlanabilir, böylece 64-bit işlemlere göz önünde bulundurun. Bu web uygulamasını yeniden başlatma tetikler, bu nedenle uygun şekilde zamanlayın.

Ayrıca, bir 64-bit ortamından bir temel veya standart hizmet planı gerektiğini unutmayın. Ücretsiz ve paylaşılan planları, her zaman bir 32-bit ortamında çalıştırın.

Daha fazla bilgi için [App Service'te web uygulamalarını yapılandırma](configure-common.md).

## <a name="why-does-my-request-time-out-after-230-seconds"></a>Neden benim isteği zaman aşımına 230 saniye sonra mu?

Azure Load Balancer dört dakikalık varsayılan bir boşta kalma zaman aşımı ayarı vardır. Genellikle bir web isteği için makul yanıt zaman sınırı budur. Web uygulamanıza arka plan işlemesi gerekiyorsa, Azure WebJobs'ı kullanmanızı öneririz. Azure web uygulaması Web işleri çağırabilir ve arka plan işlemi tamamlandığında bildirim. WebJobs, kuyruklar ve Tetikleyicileri dahil olmak üzere kullanmak için birden çok yöntem arasından seçim yapabilirsiniz.

Web işleri, arka plan işlemleri için tasarlanmıştır. WebJob içinde istediğiniz kadar arka plan işlemi yapabilirsiniz. Web işleri hakkında daha fazla bilgi için bkz. [WebJobs ile arka plan görevleri çalıştırma](webjobs-create.md).

## <a name="aspnet-core-applications-that-are-hosted-in-app-service-sometimes-stop-responding-how-do-i-fix-this-issue"></a>Bazen App Service'te barındırılan ASP.NET Core uygulamaları, yanıt vermeyi durdurur. Bu sorunu nasıl düzeltebilirim?

Bir önceki bilinen bir sorun [Kestrel sürüm](https://github.com/aspnet/KestrelHttpServer/issues/1182) barındırılan bir ASP.NET Core 1.0 uygulamasını App Service aralıklı olarak yanıt vermeyi durdurmasına neden olabilir. Ayrıca, bu iletiyi görebilirsiniz: "Belirtilen CGI uygulaması bir hatayla karşılaştı ve sunucu işlemi sonlandırıldı."

Bu sorun sürüm 1.0.2 Kestrel içinde düzeltilmiştir. Bu sürüm bulunur ASP.NET Core 1.0.3 güncelleştirme. Bu sorunu çözmek için Kestrel 1.0.2 kullanılacak uygulama bağımlılıklarınızı güncelleştirdiğinizden emin olun. Alternatif olarak, blog gönderisinde açıklanan iki geçici çözümlerden birini kullanabilirsiniz [ASP.NET Core 1.0 yavaş performans sorunları App Service web apps'te](https://blogs.msdn.microsoft.com/waws/2016/12/11/asp-net-core-slow-perf-issues-on-azure-websites).


## <a name="i-cant-find-my-log-files-in-the-file-structure-of-my-web-app-how-can-i-find-them"></a>Web Uygulamam dosya yapısını günlük dosyalarımı bulamıyorum. Bunları nasıl bulabilirim?

App Service yerel önbellek özelliğini kullanırsanız, App Service örneğinizin LogFiles ve veri klasörleri klasör yapısını etkilenir. Yerel önbellek kullanıldığında, alt klasörler LogFiles depolama ve veri klasörleri oluşturulur. Alt klasörleri adlandırma deseni "benzersiz tanımlayıcı" + zaman damgasını kullanın. Her bir alt web uygulamasını çalıştıran veya çalışan bir VM örneğine karşılık gelir.

App Service yerel önbellek kullanıp kullanmadığını belirleyin denetleyin **uygulama ayarları** sekmesi. Yerel önbellek, uygulama ayarı kullanılıyorsa `WEBSITE_LOCAL_CACHE_OPTION` ayarlanır `Always`.

Yerel önbellek kullanmıyorsanız ve bu sorunu yaşıyorsanız, bir destek isteği gönderin.

## <a name="i-see-the-message-an-attempt-was-made-to-access-a-socket-in-a-way-forbidden-by-its-access-permissions-how-do-i-resolve-this"></a>"Bir yuva, erişim izinleri tarafından yasaklanmış bir şekilde erişmek için girişimde bulunuldu." iletisini görüyorum Bu nasıl giderebilirim?

Bu hata genellikle sanal makine örneğinde giden TCP bağlantılarına tüketilirse oluşur. App Service'te sayısı için her bir VM örneği yapılabilir giden bağlantılar için sınırlar uygulanır. Daha fazla bilgi için [sayısal sınırlar arası VM](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#cross-vm-numerical-limits).

Bu hata, uygulamadan yerel adres erişmeyi denerseniz de oluşabilir. Daha fazla bilgi için [yerel adres istekleri](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#local-address-requests).

Web uygulamanızda giden bağlantılar hakkında daha fazla bilgi için ilgili blog gönderisine bakın [giden bağlantılar Azure Web siteleri'ne](https://www.freekpaans.nl/2015/08/starving-outgoing-connections-on-windows-azure-web-sites/).

## <a name="how-do-i-use-visual-studio-to-remote-debug-my-app-service-web-app"></a>Uzak Visual Studio nasıl kullanırım my App Service web uygulamasında hata ayıklama?

Visual Studio kullanarak web uygulamanızda hata ayıklama gösteren ayrıntılı bir kılavuz için bkz. [uzaktan hata ayıklama, App Service web uygulamanıza](https://blogs.msdn.microsoft.com/benjaminperkins/2016/09/22/remote-debug-your-azure-app-service-web-app/).
