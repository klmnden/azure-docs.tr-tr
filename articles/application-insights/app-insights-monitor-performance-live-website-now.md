---
title: "Çalışan IIS web sitesinde performans sorunlarını tanılama | Microsoft Belgeleri"
description: "Yeniden dağıtmadan web sitesinin performansını izleme. Bağımlılık telemetrisi almak için Application Insights SDK’yi tek başına veya birlikte kullanın."
services: application-insights
documentationcenter: .net
author: alancameronwills
manager: douge
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/24/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: b70c8baab03703bc00b75c2c611f69e3b71d6cd7
ms.openlocfilehash: 5159e7fc47d320d52eb7b94b5775158a3f09c769


---
# <a name="instrument-web-apps-at-runtime-with-application-insights"></a>Application Insights ile çalışma zamanında web uygulamalarını izleme
*Application Insights önizlemededir.*

Kodunuzu değiştirmeye veya yeniden dağıtmaya gerek olmadan canlı bir web uygulamasını Visual Studio Application Insights ile izleyebilirsiniz. Uygulamalarınız şirket içi bir IIS sunucusunda barındırılıyorsa Durum İzleyicisi’ni indirin veya Azure web uygulamaları ise ya da bir Azure sanal makinesinde çalışıyorsa Application Insights uzantısını yükleyebilirsiniz. ([Canlı J2EE web uygulamaları](app-insights-java-live.md) ve [Azure Cloud Services](app-insights-cloudservices.md) izleme hakkında ayrı makaleler de vardır.)

![örnek grafikler](./media/app-insights-monitor-performance-live-website-now/10-intro.png)

.NET web uygulamalarınıza Application Insights uygulamak için şu üç yoldan birini tercih edebilirsiniz:

* **Derleme süresi:** Web uygulaması kodunuza [Application Insights SDK'sini ekleyin][greenbrown].
* **Çalışma zamanı:** Kodu yeniden derlemeden ve yeniden dağıtmadan web uygulamanızı sunucu üzerinde izleyin.
* **Her ikisi:** SDK’yı web uygulama kodunuzda derleyin ve ayrıca çalışma zamanı uzantılarını uygulayın. Her iki seçeneğin en iyisini edinin.

Burada, her yöntemle kazanacaklarınızın bir özeti verilmiştir:

|  | Derleme zamanı | Çalışma zamanı |
| --- | --- | --- |
| İstekler ve özel durumlar |Evet |Evet |
| [Daha ayrıntılı özel durumlar](app-insights-asp-net-exceptions.md) | |Yes |
| [Bağımlılık tanılaması](app-insights-asp-net-dependencies.md) |.NET 4.6+ üzerinde |Evet |
| [Sistem performans sayaçları](app-insights-performance-counters.md) | |IIS veya Azure bulut hizmeti, Azure web uygulaması değil |
| [Özel telemetri için API][api] |Evet | |
| [İzleme günlüğü tümleştirmesi](app-insights-asp-net-trace-logs.md) |Evet | |
| [Sayfa görünümü ve kullanıcı verileri](app-insights-javascript.md) |Evet | |
| Kodu yeniden derlemeniz gerekmez |Hayır | |

## <a name="instrument-your-web-app-at-run-time"></a>Çalışma zamanında web uygulamanızı izleme
[Microsoft Azure](http://azure.com) aboneliğiniz olması gerekir.

### <a name="if-your-app-is-an-azure-web-app-or-cloud-service"></a>Uygulamanız bir Azure Web uygulaması veya Bulut Hizmetiyse
* Azure'da, uygulamanın denetim masasında bulunan Application Insights'ı seçin.

    [Daha fazla bilgi edinin](app-insights-azure.md).

### <a name="if-your-app-is-hosted-on-your-iis-server"></a>Uygulamanız IIS sunucunuzda barındırılıyorsa
1. IIS web sunucunuzda yönetici kimlik bilgileriyle oturum açın.
2. [Durum İzleyicisi yükleyici](http://go.microsoft.com/fwlink/?LinkId=506648)’yi indirip çalıştırın.
3. Yükleme Sihirbazı'ndaki Microsoft Azure'da oturum açın.

    ![Microsoft hesabı kimlik bilgilerinizle Azure’de oturum açın](./media/app-insights-monitor-performance-live-website-now/appinsights-035-signin.png)

    *Bağlantı hataları mı var? Bkz. [Sorun giderme](#troubleshooting).*
4. İzlemek istediğiniz yüklü web uygulamasını veya web sitesini seçip sonuçlarını Application Insights portalında görmek istediğiniz kaynağı yapılandırın.

    ![Uygulama ve kaynak seçin.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-configAIC.png)

    Normalde, yapılandırmak üzere yeni bir kaynak ve [kaynak grubu][roles] seçersiniz.

    bunun yerine, siteniz için [web testleri][availability] veya [web istemcisi izleme][client] ayarladıysanız var olan kaynağı kullanın.
5. IIS’yi yeniden başlatın.

    ![iletişim kutusunun üstündeki Yeniden Başlat’ı seçin.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-restart.png)

    Web hizmetiniz kısa bir süre kesintiye uğrar.
6. İzlemek istediğiniz web uygulamalarına ApplicationInsights.config eklendiğinden emin olun.

    ![Web uygulamasının kod dosyalarında .config dosyasını bulun.](./media/app-insights-monitor-performance-live-website-now/appinsights-034-aiconfig.png)

   web.config dosyasında da bazı değişiklikler vardır.

#### <a name="want-to-reconfigure-later"></a>Daha sonra (yeniden) yapılandırmak istiyor musunuz?
Sihirbazı tamamladıktan sonra istediğiniz zaman aracıyı yeniden yapılandırabilirsiniz. Bu aracıyı yüklediyseniz, ancak ilk kurulumda bazı sorunlar oluştuysa da kullanabilirsiniz.

![Görev çubuğunda Application Insights simgesine tıklama](./media/app-insights-monitor-performance-live-website-now/appinsights-033-aicRunning.png)

## <a name="view-performance-telemetry"></a>Performans telemetrisini görüntüleme
[Azure portalında](https://portal.azure.com) oturum açın, Application Insights’a gidip oluşturduğunuz kaynağı açın.

![Gözat’ı, Application Insights'ı, sonra da uygulamanızı seçme](./media/app-insights-monitor-performance-live-website-now/appinsights-08openApp.png)

İstek, yanıt süresi, bağımlılık ve diğer verileri görmek için Performans dikey penceresini açın.

![Performans](./media/app-insights-monitor-performance-live-website-now/21-perf.png)

Daha ayrıntılı bir görünüm açmak için herhangi bir grafiğe tıklayın.

Grafikleri [düzenleyebilir, yeniden ayarlayabilir, kaydedebilir](app-insights-metrics-explorer.md) ve grafikleri ya da tüm dikey pencereyi bir [panoya](app-insights-dashboards.md) sabitleyebilirsiniz.

## <a name="dependencies"></a>Bağımlılıklar
Bağımlılık Süresi grafiği uygulamanızdan veritabanları, REST API’leri veya Azure blob depolamaya yapılan çağrıların ne kadar süre aldığını gösterir.

Grafiği farklı bağımlılıklara çağrıya göre bölümlere ayırmak için: Grafiği düzenleyin, Gruplandırma’yı açın, sonra Bağımlılık, Bağımlılık Türü veya Bağımlılık Performansına göre gruplandırın.

![Bağımlılık](./media/app-insights-monitor-performance-live-website-now/23-dep.png)

## <a name="performance-counters"></a>Performans sayaçları
(Azure web uygulamaları için değil.) CPU doluluğu ve bellek kullanımı gibi sunucu performans sayacı grafiklerini görmek için genel bakış dikey penceresinde Sunucular’a tıklayın.

Birden çok sunucu örneğiniz varsa Rol örneğine göre gruplandırmak üzere grafikleri düzenlemek isteyebilirsiniz.

![Sunucular](./media/app-insights-monitor-performance-live-website-now/22-servers.png)

Ayrıca, SDK tarafından bildirilen performans sayaçları kümesini değiştirebilirsiniz. 

## <a name="exceptions"></a>Özel durumlar
![Sunucu özel durumları grafiğine tıklayın](./media/app-insights-monitor-performance-live-website-now/appinsights-039-1exceptions.png)

Belirli özel durumların ayrıntısına gidebilir (son yedi gün) ve yığın izlemeleri ve bağlam verileri alabilirsiniz.

## <a name="sampling"></a>Örnekleme
Uygulamanız çok miktarda veri gönderiyorsa ve ASP.NET sürüm 2.0.0-beta3 veya daha sonraki uygulamalar için Application Insights SDK kullanıyorsanız, uyarlamalı örnekleme özelliği telemetrenizin yalnızca yüzdesini çalıştırır ve gönderir. [Örnekleme hakkında daha fazla bilgi edinin.](app-insights-sampling.md)

## <a name="troubleshooting"></a>Sorun giderme
### <a name="connection-errors"></a>Bağlantı hataları
Durum İzleyicisi’ne izin vermek için sunucunuzun güvenlik duvarında [bazı giden bağlantı noktalarını](app-insights-ip-addresses.md#outgoing-ports) açmanız gerekir.

### <a name="no-telemetry"></a>Telemetri yok mu?
* Bazı verileri oluşturmak için sitenizi kullanın.
* Verilerin gelmesi için birkaç dakika bekleyin ve **Yenile**’ye tıklayın.
* Olayları tek tek görmek için Tanılama Arama’yı (Ara kutucuğu) açın. Veri toplama grafiklerde görünmeden önce olaylar genellikle Tanılama Arama’da görülebilir.
* Durum İzleyicisi’ni açın ve sol bölmede uygulamanızı seçin. "Yapılandırma bildirimleri" bölümünde bu uygulamayla ilgili herhangi bir tanılama iletisi olup olmadığını denetleyin:

  ![İstek, yanıt süresi, bağımlılık ve diğer verileri görmek için Performans dikey penceresini açın](./media/app-insights-monitor-performance-live-website-now/appinsights-status-monitor-diagnostics-message.png)
* Sunucu güvenlik duvarının yukarıda listelenen bağlantı noktalarında giden trafiğe izin verdiğinden emin olun.
* "Yetersiz izinler" hakkında bir ileti görürseniz, sunucuda aşağıdakileri deneyin:
  * IIS Yöneticisi'nde, uygulama havuzunuzu seçin, **Gelişmiş Ayarlar**’ı açın ve **işlem modeli** altında kimliğini not edin.
  * Bilgisayar yönetimi denetim masasında, bu kimliği Performans İzleyicisi Kullanıcıları grubuna ekleyin.
* Sunucunuza yüklenmiş MMA/SCOM varsa bazı sürümler çakışabilir. Hem SCOM’u, hem de Durum İzleyicisi’ni kaldırın ve en son sürümleri yeniden yükleyin.
* Bkz. [Sorun giderme][qna].

## <a name="system-requirements"></a>Sistem Gereksinimleri
Sunucuda Application Insights Durum İzleyicisi için işletim sistemi desteği:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows server 2012 R2

en son SP ve .NET Framework 4.0 ve 4.5 ile

İstemci tarafında Windows 7, 8 ve 8.1, .NET Framework 4.0 ve 4.5 ile yeniden

IIS desteği: IIS 7, 7.5, 8, 8.5 (IIS gereklidir)

## <a name="automation-with-powershell"></a>PowerShell ile Automation
IIS sunucunuzda PowerShell'i kullanarak izlemeyi başlatabilir ve durdurabilirsiniz.

İlk olarak Application Insights modülünü içeri aktarın:

`Import-Module 'C:\Program Files\Microsoft Application Insights\Status Monitor\PowerShell\Microsoft.Diagnostics.Agent.StatusMonitor.PowerShell.dll'`

Hangi uygulamaların izlenmekte olduğunu öğrenin:

`Get-ApplicationInsightsMonitoringStatus [-Name appName]`

* `-Name` (İsteğe bağlı) Bir web uygulaması adı.
* Bu IIS sunucusundaki her web uygulaması için (veya adlandırılmış uygulama) Application Insights izleme durumunu görüntüler.
* Her uygulama için `ApplicationInsightsApplication` döndürür:

  * `SdkState==EnabledAfterDeployment`: Uygulama izleniyor ve çalışma zamanında, Durum İzleyicisi aracıyla ya da `Start-ApplicationInsightsMonitoring` ile izleme eklenmiş.
  * `SdkState==Disabled`: Uygulama, Application Insights için izleme eklenmemiş. Hiçbir zaman izlenmeyecek veya Durum İzleyicisi aracıyla veya `Stop-ApplicationInsightsMonitoring` ile çalışma zamanı izlemesi devre dışı bırakılmayacaktır.
  * `SdkState==EnabledByCodeInstrumentation`: Uygulama, kaynak koduna SDK eklenerek izleme eklenmiş. SDK’si güncelleştirilemez veya durdurulamaz.
  * `SdkVersion`, bu uygulamanın izlenmesi için kullanılan sürümü gösterir.
  * `LatestAvailableSdkVersion`, şu anda NuGet galerisinde mevcut olan sürümü gösterir. Uygulama bu sürüme yükseltmek için `Update-ApplicationInsightsMonitoring` kullanın.

`Start-ApplicationInsightsMonitoring -Name appName -InstrumentationKey 00000000-000-000-000-0000000`

* `-Name` Uygulamanın IIS'deki adı
* `-InstrumentationKey` Sonuçların görüntülenmesini istediğiniz Application Insights kaynağına ait iKey.
* Bu cmdlet yalnızca henüz izlenmemiş uygulamaları etkiler; başka bir deyişle zaten işaretlendiğine değil - diğer bir deyişle, SdkState==NotInstrumented.

    Cmdlet, koda SDK ekleyerek derleme zamanında ya da cmdlet’in önceki bir kullanımıyla çalışma zamanında zaten izlenmiş olan uygulamayı etkilemez.

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

## <a name="a-namenextanext-steps"></a><a name="next"></a>Sonraki adımlar
* Sitenizin canlı kalması için [web testleri oluşturun][availability].
* Sorunların tanımlanması için [Olayları ve günlükleri arayın][diagnostic].
* Web sayfası koduna ait özel durumları görmek ve izleme çağrıları eklemenize izin vermek için [web istemcisi telemetrisini ekleyin][usage].
* [Web hizmeti kodunuza Application Insights SDK ekleyin][greenbrown]; böylece, izleme ekleyebilir ve sunucu koduna çağrı kaydedebilirsiniz.

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[roles]: app-insights-resources-roles-access-control.md
[usage]: app-insights-web-track-usage.md



<!--HONumber=Nov16_HO2-->


