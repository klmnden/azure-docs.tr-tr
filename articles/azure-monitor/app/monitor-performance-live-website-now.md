---
title: Azure Application Insights ile canlı bir ASP.NET web uygulamasını izleme | Microsoft Docs
description: Yeniden dağıtmadan web sitesinin performansını izleme. ASP.NET web uygulamaları barındırılan şirket içi veya sanal makinelerinde çalışır.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: mbullwin
ms.openlocfilehash: 3f4ef7f333525d7408d0345b917102cddb295386
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66255478"
---
# <a name="instrument-web-apps-at-runtime-with-application-insights-status-monitor"></a>Application Insights Durum İzleyicisi ile çalışma zamanında web uygulamalarını izleme

Kodunuzu değiştirmeye veya yeniden dağıtmaya gerek olmadan canlı bir web uygulamasını Azure Application Insights ile izleyebilirsiniz. [Microsoft Azure](https://azure.com) aboneliğiniz olması gerekir.

Durum İzleyicisi IIS'de barındırılan bir .NET uygulaması izleme için kullanılan şirket içinde veya bir VM.

- Uygulamanızı Azure app Services'e dağıtılması durumunda izleyin [bu yönergeleri](azure-web-apps.md).
- Uygulamanızı Azure VM'deki dağıttıysanız, Azure denetim masasından Application Insights izlemeyi geçiş yapabilirsiniz.
- (İzleme hakkında ayrı makaleler de vardır [Azure Cloud Services](../../azure-monitor/app/cloudservices.md).)


![Başarısız istekler, sunucu yanıt süresi ve sunucu istekleri hakkında bilgi içeren App Insights ekran genel bakış grafikleri](./media/monitor-performance-live-website-now/overview-graphs.png)

.NET web uygulamalarınıza Application Insights uygulamak için iki yol vardır:

* **Derleme zamanı:** [Application Insights SDK'sı ekleme] [ greenbrown] web uygulama kodunuzda.
* **Çalışma zamanı:** Web uygulamanızı sunucu üzerinde yeniden oluşturma ve kod yeniden dağıtmaya gerek olmadan, aşağıda açıklandığı gibi izleyin.

> [!NOTE]
> Derleme zamanında izleme kullanırsanız, çalışma zamanı açık olsa bile alet çalışmaz.

Burada, her yöntemle kazanacaklarınızın bir özeti verilmiştir:

|  | Derleme zamanı | Çalışma zamanı |
| --- | --- | --- |
| İstekler ve özel durumlar |Evet |Evet |
| [Daha ayrıntılı özel durumlar](../../azure-monitor/app/asp-net-exceptions.md) | |Yes |
| [Bağımlılık tanılaması](../../azure-monitor/app/asp-net-dependencies.md) |.NET 4.6+ üzerinde ancak daha az ayrıntılı |Evet, tam ayrıntılı: sonuç kodları, SQL komut metni, HTTP fiili|
| [Sistem performans sayaçları](../../azure-monitor/app/performance-counters.md) |Evet |Evet |
| [Özel telemetri için API][api] |Evet |Hayır |
| [İzleme günlüğü tümleştirmesi](../../azure-monitor/app/asp-net-trace-logs.md) |Evet |Hayır |
| [Sayfa görünümü ve kullanıcı verileri](../../azure-monitor/app/javascript.md) |Evet |Hayır |
| Kodu yeniden derlemeniz gerekir |Evet | Hayır |



## <a name="monitor-a-live-iis-web-app"></a>Canlı bir IIS web uygulamasını izleme

Uygulamanız bir IIS sunucusunda barındırılıyorsa, Durum İzleyici’yi kullanarak Application Insights’ı etkinleştirin.

1. IIS web sunucunuzda yönetici kimlik bilgileriyle oturum açın.
2. Application Insights Durum İzleyicisi zaten yüklü değilse, [indirin ve yükleyiciyi çalıştırın](#download)
3. Durum İzleyicisi'nde yüklü web uygulamasını veya izlemek istediğiniz web sitesini seçin. Azure kimlik bilgilerinizle oturum açın.

    Sonuçlarını Application Insights portalında görmek istediğiniz kaynağı yapılandırın. (Normalde, yeni bir kaynak oluşturmak en iyi yöntemdir. Bu uygulama için zaten [web testleriniz][availability] veya [istemci izleme][client] özelliği varsa mevcut bir kaynağı seçin.) 

    ![Uygulama ve kaynak seçin.](./media/monitor-performance-live-website-now/appinsights-036-configAIC.png)

4. IIS’yi yeniden başlatın.

    ![iletişim kutusunun üstündeki Yeniden Başlat’ı seçin.](./media/monitor-performance-live-website-now/appinsights-036-restart.png)

    Web hizmetiniz kısa bir süre kesintiye uğrar.

## <a name="customize-monitoring-options"></a>İzlemeyi özelleştirme seçenekleri

Application Insights’ın etkinleştirilmesi, web uygulamanıza DLL ve ApplicationInsights.config dosyasını ekler. Bazı seçenekleri değiştirmek için [.config dosyasını düzenleyebilirsiniz](../../azure-monitor/app/configuration-with-applicationinsights-config.md).

## <a name="when-you-re-publish-your-app-re-enable-application-insights"></a>Uygulamanızı yeniden yayımladığınızda, Application Insights’ı yeniden etkinleştirin

Uygulamanızı yeniden yayımlamadan önce [Visual Studio’daki koda Application Insights'ı eklemeyi][greenbrown] düşünün. Bu şekilde daha ayrıntılı telemetri alabilir ve özel telemetri yazabilirsiniz.

Koda Application Insights eklemeden yeniden yayımlamak istiyorsanız, dağıtım işleminin yayımlanmış web sitesinden DLL ve ApplicationInsights.config dosyasını silebileceğini unutmayın. Bu nedenle:

1. ApplicationInsights.config dosyasını düzenlediyseniz, uygulamanızı yeniden yayımlamadan önce bir kopyasını alın.
2. Uygulamanızı yeniden yayımlayın.
3. Application Insights izlemeyi yeniden etkinleştirin. (Uygun yöntemi kullanın: Azure web uygulaması denetim masası veya bir IIS konağındaki Durum İzleyicisi.)
4. .config dosyası üzerinde yaptığınız tüm düzenlemeleri yeniden devreye sokun.


## <a name="troubleshoot"></a>Sorun giderme

### <a name="confirm-a-valid-installation"></a>Geçerli bir yükleme işlemini onaylayın 

Bu, yüklemenin başarılı olduğunu onaylamak için gerçekleştirebileceğiniz bazı adımlardır.

- Applicationınsights.config dosyasını hedef uygulama dizinde mevcut olduğundan ve ikey içeren onaylayın.

- Veriler eksik olduğundan şüpheleniyorsanız, basit bir sorgu çalıştırabilirsiniz [Analytics](../log-query/get-started-portal.md) şu anda telemetri gönderdiği tüm bulut rollerini listelemek için.
  ```Kusto
  union * | summarize count() by cloud_RoleName, cloud_RoleInstance
  ```

- Application Insights olduğunu onaylamak gerekiyorsa başarıyla kullanıma açıldı çalıştırabileceğiniz [Sysinternals tanıtıcı](https://docs.microsoft.com/sysinternals/downloads/handle) bir komut penceresi bu applicationinsights.dll onaylamak için IIS tarafından yüklendi.
  ```cmd
  handle.exe /p w3wp.exe
  ```


### <a name="cant-connect-no-telemetry"></a>Bağlanamıyor musunuz? Telemetri yok mu?

* Durum İzleyicisi’nin çalışmasına izin vermek için sunucunuzun güvenlik duvarında [gerekli giden bağlantı noktalarını](../../azure-monitor/app/ip-addresses.md#outgoing-ports) açın.

### <a name="unable-to-login"></a>Oturum açılamıyor

* Durum İzleyicisi için oturum açamıyor, yoksa bir komut satırı yükleme yerine. Durum İzleyicisi, ikey toplamak için oturum açma girişiminde, ancak bu komutu kullanılarak el ile sağlayın:

```powershell
Import-Module 'C:\Program Files\Microsoft Application Insights\Status Monitor\PowerShell\Microsoft.Diagnostics.Agent.StatusMonitor.PowerShell.dll'
Start-ApplicationInsightsMonitoring -Name appName -InstrumentationKey 00000000-000-000-000-0000000
```

### <a name="could-not-load-file-or-assembly-systemdiagnosticsdiagnosticsource"></a>Dosya veya 'System.Diagnostics.DiagnosticSource' derlemesi yüklenemedi

Application Insights'ı etkinleştirdikten sonra bu hatayı alabilirsiniz. Yükleyici bu dll bin dizinindeki değiştirdiğinden budur.
Düzeltmek için web.config güncelleştirin:

```xml
<dependentAssembly>
    <assemblyIdentity name="System.Diagnostics.DiagnosticSource" publicKeyToken="cc7b13ffcd2ddd51"/>
    <bindingRedirect oldVersion="0.0.0.0-4.*.*.*" newVersion="4.0.2.1"/>
</dependentAssembly>
```

Bu sorunu takip ettiğimiz [burada](https://github.com/Microsoft/ApplicationInsights-Home/issues/301).


### <a name="application-diagnostic-messages"></a>Uygulama tanılama iletileri

* Durum İzleyicisi’ni açın ve sol bölmede uygulamanızı seçin. "Yapılandırma bildirimleri" bölümünde bu uygulamayla ilgili herhangi bir tanılama iletisi olup olmadığını denetleyin:

  ![İstek, yanıt süresi, bağımlılık ve diğer verileri görmek için Performans dikey penceresini açın](./media/monitor-performance-live-website-now/appinsights-status-monitor-diagnostics-message.png)
  
### <a name="detailed-logs"></a>Ayrıntılı günlükler

* Varsayılan olarak, Durum İzleyicisi tanılama günlüklerine çıkarır: `C:\Program Files\Microsoft Application Insights\Status Monitor\diagnostics.log`

* Ayrıntılı günlükleri çıktısını almak için yapılandırma dosyasını değiştirme: `C:\Program Files\Microsoft Application Insights\Status Monitor\Microsoft.Diagnostics.Agent.StatusMonitor.exe.config` ve ekleme `<add key="TraceLevel" value="All" />` için `appsettings`.
Ardından, Durum İzleyicisi'ni yeniden başlatın.

* Durum İzleyicisi, bir .NET uygulaması olduğundan da etkinleştirebilirsiniz [uygun tanılama yapılandırma dosyasına ekleyerek .net izleme](https://docs.microsoft.com/dotnet/framework/configure-apps/file-schema/trace-debug/system-diagnostics-element). Örneğin, bazı senaryolarda bu tarafından ağ düzeyinde neler olduğunu görmek yararlı olabilir [ağ izlemeyi yapılandırma](https://docs.microsoft.com/dotnet/framework/network-programming/how-to-configure-network-tracing)

### <a name="insufficient-permissions"></a>İzinler yetersiz
  
* "Yetersiz izinler" hakkında bir ileti görürseniz, sunucuda aşağıdakileri deneyin:
  * IIS Yöneticisi'nde, uygulama havuzunuzu seçin, **Gelişmiş Ayarlar**’ı açın ve **işlem modeli** altında kimliğini not edin.
  * Bilgisayar yönetimi denetim masasında, bu kimliği Performans İzleyicisi Kullanıcıları grubuna ekleyin.

### <a name="conflict-with-systems-center-operations-manager"></a>Systems Center Operations Manager ile çakışma

* Sunucunuza yüklenmiş MMA/SCOM (System Center Operations Manager) varsa bazı sürümler çakışabilir. Hem SCOM’u, hem de Durum İzleyicisi’ni kaldırın ve en son sürümleri yeniden yükleyin.

### <a name="failed-or-incomplete-installation"></a>Başarısız veya tamamlanmamış yüklemesi

Durum İzleyicisi yükleme sırasında başarısız olursa, Durum İzleyicisi, kurtarılır silemiyor tamamlanmamış bir yükleme ile kalabilir. Bu elle sıfırlama gerektirir.

Bu dosyalar, uygulama dizininizde bulunan silin:
- Ya da "Microsoft.AI." Başlangıç bin dizinindeki tüm DLL'ler veya "Microsoft.applicationınsights".
- Bu DLL bin dizinindeki "Microsoft.Web.Infrastructure.dll"
- Bu DLL bin dizinindeki "System.Diagnostics.DiagnosticSource.dll"
- "App_Data\packages" uygulama dizininizde Kaldır
- "Applicationınsights.config" uygulama dizininizde Kaldır


### <a name="additional-troubleshooting"></a>Ek Sorun Giderme Seçenekleri

* Bkz: ek [sorun giderme][qna].

## <a name="system-requirements"></a>Sistem Gereksinimleri
Sunucuda Application Insights Durum İzleyicisi için işletim sistemi desteği:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows server 2012 R2
* Windows Server 2016

en son SP ve .NET Framework 4.5 (framework'ün bu sürümü Durum izleyicisini yerleşik)

İstemci tarafında: Windows 7, 8, 8.1 ve 10, .NET Framework 4.5 ile

IIS desteği şöyledir: IIS 7, 7.5, 8, 8.5 (IIS gereklidir)

## <a name="automation-with-powershell"></a>PowerShell ile Automation
IIS sunucunuzda PowerShell'i kullanarak izlemeyi başlatabilir ve durdurabilirsiniz.

İlk olarak Application Insights modülünü içeri aktarın:

`Import-Module 'C:\Program Files\Microsoft Application Insights\Status Monitor\PowerShell\Microsoft.Diagnostics.Agent.StatusMonitor.PowerShell.dll'`

Hangi uygulamaların izlenmekte olduğunu öğrenin:

`Get-ApplicationInsightsMonitoringStatus [-Name appName]`

* `-Name` (İsteğe bağlı) Bir web uygulaması adı.
* Bu IIS sunucusundaki her web uygulaması için (veya adlandırılmış uygulama) Application Insights izleme durumunu görüntüler.
* Her uygulama için `ApplicationInsightsApplication` döndürür:

  * `SdkState==EnabledAfterDeployment`: Uygulama izlenmektedir ve Durum İzleyicisi aracıyla ya da çalışma zamanında işaretlenmiş olan `Start-ApplicationInsightsMonitoring`.
  * `SdkState==Disabled`: Uygulamayı Application Insights için izlenmemektedir. Hiçbir zaman izlenmeyecek veya Durum İzleyicisi aracıyla veya `Stop-ApplicationInsightsMonitoring` ile çalışma zamanı izlemesi devre dışı bırakılmayacaktır.
  * `SdkState==EnabledByCodeInstrumentation`: Kaynak koduna SDK eklenerek uygulama izlendi. SDK’si güncelleştirilemez veya durdurulamaz.
  * `SdkVersion`, bu uygulamanın izlenmesi için kullanılan sürümü gösterir.
  * `LatestAvailableSdkVersion`, şu anda NuGet galerisinde mevcut olan sürümü gösterir. Uygulama bu sürüme yükseltmek için `Update-ApplicationInsightsMonitoring` kullanın.

`Start-ApplicationInsightsMonitoring -Name appName -InstrumentationKey 00000000-000-000-000-0000000`

* `-Name` Uygulamanın IIS'deki adı
* `-InstrumentationKey` Sonuçların görüntülenmesini istediğiniz Application Insights kaynağına ait iKey.
* Bu cmdlet yalnızca henüz izlenmemiş uygulamaları etkiler; başka bir deyişle zaten işaretlendiğine değil - diğer bir deyişle, SdkState==NotInstrumented.

    Cmdlet, zaten izleme eklenmiş olan uygulamayı etkilemez. Uygulamanın koda SDK eklenerek derleme zamanında izlenmesi veya cmdlet’in önceki bir kullanımıyla çalışma zamanında izlenmesi farklılık yaratmaz.

    Uygulamayı izlemek için kullanılan SDK sürümü sunucuya en son indirilen sürümdür.

    En son sürümü indirmek için Update-ApplicationInsightsVersion kullanın.
* Başarılı bir şekilde `ApplicationInsightsApplication` döndürür. Başarısız olursa, stderr’e bir izleme kaydeder.

          Name                      : Default Web Site/WebApp1
          InstrumentationKey        : 00000000-0000-0000-0000-000000000000
          ProfilerState             : ApplicationInsights
          SdkState                  : EnabledAfterDeployment
          SdkVersion                : 1.2.1
          LatestAvailableSdkVersion : 1.2.3

`Stop-ApplicationInsightsMonitoring [-Name appName | -All]`

* `-Name` Bir uygulamanın IIS'deki adı
* `-All` Bu IIS sunucusunda, şu koşulu sağlayan tüm uygulamaları izlemeyi durdurur: `SdkState==EnabledAfterDeployment`
* Belirtilen uygulamaları izlemeyi durdurur ve izlemeyi kaldırır. Yalnızca, Durum İzleme aracını veya Start-ApplicationInsightsApplication kullanarak çalışma zamanında izleme eklenmiş uygulamalar için çalışır. (`SdkState==EnabledAfterDeployment`)
* ApplicationInsightsApplication döndürür.

`Update-ApplicationInsightsMonitoring -Name appName [-InstrumentationKey "0000000-0000-000-000-0000"`]

* `-Name`: Bir web uygulamasının IIS'deki adı.
* `-InstrumentationKey` (İsteğe bağlı.) Uygulamanın telemetri gönderildiği kaynağı değiştirmek için kullanın.
* Bu cmdlet:
  * Adlandırılmış uygulamaları SDK'nin bu makineye en son indirilen sürümüne yükseltir. (Yalnızca `SdkState==EnabledAfterDeployment` ise çalışır)
  * İzleme anahtarını sağlarsanız, adlandırılmış uygulama, telemetriyi bu anahtarla kaynağa göndermek için yeniden yapılandırılır. (`SdkState != Disabled` ise çalışır)

`Update-ApplicationInsightsVersion`

* En son Application Insights SDK’sini sunucuya indirir.

## <a name="questions"></a>Durum İzleyicisi hakkında sorular

### <a name="what-is-status-monitor"></a>Durum İzleyicisi nedir?

IIS web sunucunuza yüklediğiniz bir masaüstü uygulamasıdır. Web uygulamalarını izleyip yapılandırmanıza yardımcı olur. 

### <a name="when-do-i-use-status-monitor"></a>Durum İzleyicisi’ni ne zaman kullanırım?

* Halihazırda çalışıyor olsa bile, IIS sunucunuzda çalışan herhangi bir web uygulamasını izleme.
* Derleme zamanında [Application Insights SDK'sı ile derlenmiş](../../azure-monitor/app/asp-net.md) web uygulamaları için ek telemetriyi etkinleştirme. 

### <a name="can-i-close-it-after-it-runs"></a>Çalıştıktan sonra kapatabilir miyim?

Evet. Seçtiğiniz web sitelerini izledikten sonra kapatabilirsiniz.

Telemetriyi tek başına toplamaz. Yalnızca web uygulamalarını yapılandırır ve bazı izinleri ayarlar.

### <a name="what-does-status-monitor-do"></a>Durum İzleyicisi ne yapar?

Durum İzleyicisi’nin izlemesi için bir web uygulaması seçtiğinizde:

* İndirir ve Application Insights derlemelerini ve Applicationınsights.config dosyasını web uygulamasının ikili dosyalar klasörüne yerleştirir.
* Bağımlılık çağrılarını toplamak için CLR profil oluşturma özelliğini etkinleştirir.

### <a name="what-version-of-application-insights-sdk-does-status-monitor-install"></a>Application Insights SDK'sı sürümünü, Durum İzleyicisi yükleme?

Şu anda, Durum İzleyicisi, yalnızca Application Insights SDK'sı sürüm 2.3 veya 2.4 yükleyebilirsiniz. 

Application Insights SDK sürüm 2.4 olan [destek .NET 4.0 için son sürüm](https://github.com/microsoft/ApplicationInsights-dotnet/releases/tag/v2.5.0-beta1) olduğu [EOL'ye Ocak 2016](https://devblogs.microsoft.com/dotnet/support-ending-for-the-net-framework-4-4-5-and-4-5-1/). Bu nedenle, şu andan itibaren Durum İzleyicisi, bir .NET 4.0 uygulamasını izleme için kullanılabilir. 

### <a name="do-i-need-to-run-status-monitor-whenever-i-update-the-app"></a>Uygulamayı her güncelleştirdiğimde Durum İzleyicisi’ni çalıştırmam gerekiyor mu?

Artımlı olarak dağıtmanız durumunda gerekli değildir. 

Yayımlama işleminde 'mevcut dosyaları sil' seçeneğini belirlerseniz, Application Insights’ı yapılandırmak için Durum İzleyicisi’ni yeniden çalıştırmanız gerekir.

### <a name="what-telemetry-is-collected"></a>Hangi telemetri toplanır?

Durum İzleyicisi'ni kullanarak yalnızca çalışma zamanında izlediğiniz uygulamalar için:

* HTTP istekleri
* Bağımlılık çağrıları
* Özel durumlar
* Performans sayaçları

Derleme zamanında zaten izlenmekte olan uygulamalar için:

 * İşlem sayaçları.
 * Bağımlılık çağrıları (.NET 4.5); bağımlılık çağrılarındaki dönüş değerleri (.NET 4.6).
 * Özel durum yığın izleme değerleri.

[Daha fazla bilgi](https://apmtips.com/blog/2016/11/18/how-application-insights-status-monitor-not-monitors-dependencies/)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="download"></a>Durum İzleyicisi'ni indirin

- İndirme ve çalıştırma [Durum İzleyicisi yükleyici](https://go.microsoft.com/fwlink/?LinkId=506648)
- Veya çalıştırmak [Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx) ve Application Insights Durum İzleyicisi da arayın.

## <a name="next"></a>Sonraki adımlar

Telemetrinizi görüntüleyin:

* Performans ve kullanımı izlemek için [ölçümleri keşfedin](../../azure-monitor/app/metrics-explorer.md)
* Sorunların tanımlanması için [Olayları ve günlükleri arayın][diagnostic]
* Daha gelişmiş sorgular için [analiz](../../azure-monitor/app/analytics.md)

Daha fazla telemetri ekleyin:

* Sitenizin canlı kalması için [web testleri oluşturun][availability].
* Web sayfası koduna ait özel durumları görmek ve izleme çağrıları eklemenize izin vermek için [web istemcisi telemetrisini ekleyin][usage].
* İzleme ve günlük çağrıları ekleyebilmek için [kodunuza Application Insights SDK’sı ekleyin][greenbrown]

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[availability]: monitor-web-app-availability.md
[client]: ../../azure-monitor/app/javascript.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[greenbrown]: ../../azure-monitor/app/asp-net.md
[qna]: ../../azure-monitor/app/troubleshoot-faq.md
[roles]: ../../azure-monitor/app/resources-roles-access-control.md
[usage]: ../../azure-monitor/app/javascript.md
