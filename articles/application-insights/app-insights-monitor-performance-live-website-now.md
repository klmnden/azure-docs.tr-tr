---
title: "Azure Application Insights ile canlı bir ASP.NET web uygulamasını izleme | Microsoft Docs"
description: "Yeniden dağıtmadan web sitesinin performansını izleme. Şirket içinde, sanal makinelerde veya Azure üzerinde ASP.NET web uygulamaları ile çalışır."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/05/2017
ms.author: bwren
ms.translationtype: HT
ms.sourcegitcommit: c3ea7cfba9fbf1064e2bd58344a7a00dc81eb148
ms.openlocfilehash: 457ba9c9f74bc9d88800607a2f78a3c3c96cea07
ms.contentlocale: tr-tr
ms.lasthandoff: 07/19/2017

---
# <a name="instrument-web-apps-at-runtime-with-application-insights"></a>Application Insights ile çalışma zamanında web uygulamalarını izleme


Kodunuzu değiştirmeye veya yeniden dağıtmaya gerek olmadan canlı bir web uygulamasını Azure Application Insights ile izleyebilirsiniz. Uygulamalarınız şirket içi IIS sunucusu tarafından barındırılıyorsa, Durum İzleyicisi’ni yükleyin. Uygulamalarınız Azure web uygulamaları ise veya bir Azure VM ortamında çalışıyorsa, Azure denetim masasından Application Insights izlemeyi etkinleştirebilirsiniz. ([Canlı J2EE web uygulamaları](app-insights-java-live.md) ve [Azure Cloud Services](app-insights-cloudservices.md) izleme hakkında ayrı makaleler de vardır.) [Microsoft Azure](http://azure.com) aboneliğiniz olması gerekir.

![örnek grafikler](./media/app-insights-monitor-performance-live-website-now/10-intro.png)

.NET web uygulamalarınıza Application Insights uygulamak için şu üç yoldan birini tercih edebilirsiniz:

* **Derleme süresi:** Web uygulaması kodunuza [Application Insights SDK'sını ekleyin][greenbrown].
* **Çalışma zamanı:** Kodu yeniden derlemeden ve yeniden dağıtmadan web uygulamanızı sunucu üzerinde izleyin.
* **Her ikisi:** SDK’yı web uygulama kodunuzda derleyin ve ayrıca çalışma zamanı uzantılarını uygulayın. Her iki seçeneğin en iyisini edinin.

Burada, her yöntemle kazanacaklarınızın bir özeti verilmiştir:

|  | Derleme zamanı | Çalışma zamanı |
| --- | --- | --- |
| İstekler ve özel durumlar |Evet |Evet |
| [Daha ayrıntılı özel durumlar](app-insights-asp-net-exceptions.md) | |Yes |
| [Bağımlılık tanılaması](app-insights-asp-net-dependencies.md) |.NET 4.6+ üzerinde ancak daha az ayrıntılı |Evet, tam ayrıntılı: sonuç kodları, SQL komut metni, HTTP fiili|
| [Sistem performans sayaçları](app-insights-performance-counters.md) |Evet |Evet |
| [Özel telemetri için API][api] |Evet |Hayır |
| [İzleme günlüğü tümleştirmesi](app-insights-asp-net-trace-logs.md) |Evet |Hayır |
| [Sayfa görünümü ve kullanıcı verileri](app-insights-javascript.md) |Evet |Hayır |
| Kodu yeniden derlemeniz gerekir |Evet | Hayır |


## <a name="monitor-a-live-azure-web-app"></a>Canlı bir Azure web uygulamasını izleme

Uygulamanız bir Azure web hizmeti olarak çalışıyorsa, izlemeyi açma işlemi şu şekilde yapılır:

* Azure'da, uygulamanın denetim masasında bulunan Application Insights'ı seçin.

    ![Bir Azure web uygulaması için Application Insights’ı ayarlama](./media/app-insights-monitor-performance-live-website-now/azure-web-setup.png)
* Application Insights özet sayfası açıldığında, tam Application Insights kaynağını açmak için alttaki bağlantıya tıklayın.

    ![Application Insights'a tıklayın](./media/app-insights-monitor-performance-live-website-now/azure-web-view-more.png)

[Bulut ve VM uygulamalarını izleme](app-insights-azure.md).

### <a name="enable-client-side-monitoring-in-azure"></a>Azure'da istemci tarafı izlemeyi etkinleştirme

Azure'da Application Insights'ı etkinleştirdiyseniz sayfa görüntülemesi ve kullanıcı telemetrisi ekleyebilirsiniz.

1. Ayarlar > Uygulama Ayarları'nı seçin.
2.  Uygulama Ayarları'nın altında yeni bir anahtar değer çifti ekleyin: 
   
    Anahtar: `APPINSIGHTS_JAVASCRIPT_ENABLED` 
    
    Değer:`true`
3. Ayarları **Kaydedin** ve uygulamanızı **Yeniden başlatın**.

Bu işlemle Application Insights JavaScript SDK tüm web sayfalarına eklenmiş olur.

## <a name="monitor-a-live-iis-web-app"></a>Canlı bir IIS web uygulamasını izleme

Uygulamanız bir IIS sunucusunda barındırılıyorsa, Durum İzleyici’yi kullanarak Application Insights’ı etkinleştirin.

1. IIS web sunucunuzda yönetici kimlik bilgileriyle oturum açın.
2. Application Insights Durum İzleyicisi henüz yüklü değilse, [Durum İzleyicisi yükleyicisini](http://go.microsoft.com/fwlink/?LinkId=506648) indirip çalıştırın (veya [Web Platformu Yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)’ni çalıştırıp Application Insights Durum İzleyicisi için arama yapın).
3. Durum İzleyicisi'nde yüklü web uygulamasını veya izlemek istediğiniz web sitesini seçin. Azure kimlik bilgilerinizle oturum açın.

    Sonuçlarını Application Insights portalında görmek istediğiniz kaynağı yapılandırın. (Normalde, yeni bir kaynak oluşturmak en iyi yöntemdir. Bu uygulama için zaten [web testleriniz][availability] veya [istemci izleme][client] özelliği varsa mevcut bir kaynağı seçin.) 

    ![Uygulama ve kaynak seçin.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-configAIC.png)

4. IIS’yi yeniden başlatın.

    ![iletişim kutusunun üstündeki Yeniden Başlat’ı seçin.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-restart.png)

    Web hizmetiniz kısa bir süre kesintiye uğrar.

## <a name="customize-monitoring-options"></a>İzlemeyi özelleştirme seçenekleri

Application Insights’ın etkinleştirilmesi, web uygulamanıza DLL ve ApplicationInsights.config dosyasını ekler. Bazı seçenekleri değiştirmek için [.config dosyasını düzenleyebilirsiniz](app-insights-configuration-with-applicationinsights-config.md).

## <a name="when-you-re-publish-your-app-re-enable-application-insights"></a>Uygulamanızı yeniden yayımladığınızda, Application Insights’ı yeniden etkinleştirin

Uygulamanızı yeniden yayımlamadan önce [Visual Studio’daki koda Application Insights'ı eklemeyi][greenbrown] düşünün. Bu şekilde daha ayrıntılı telemetri alabilir ve özel telemetri yazabilirsiniz.

Koda Application Insights eklemeden yeniden yayımlamak istiyorsanız, dağıtım işleminin yayımlanmış web sitesinden DLL ve ApplicationInsights.config dosyasını silebileceğini unutmayın. Bu nedenle:

1. ApplicationInsights.config dosyasını düzenlediyseniz, uygulamanızı yeniden yayımlamadan önce bir kopyasını alın.
2. Uygulamanızı yeniden yayımlayın.
3. Application Insights izlemeyi yeniden etkinleştirin. (Uygun yöntemi kullanın: Azure web uygulaması denetim masası veya bir IIS konağındaki Durum İzleyicisi.)
4. .config dosyası üzerinde yaptığınız tüm düzenlemeleri yeniden devreye sokun.


## <a name="troubleshooting-runtime-configuration-of-application-insights"></a>Application Insights çalışma zamanı yapılandırma sorunlarını giderme

### <a name="cant-connect-no-telemetry"></a>Bağlanamıyor musunuz? Telemetri yok mu?

* Durum İzleyicisi’nin çalışmasına izin vermek için sunucunuzun güvenlik duvarında [gerekli giden bağlantı noktalarını](app-insights-ip-addresses.md#outgoing-ports) açın.

* Durum İzleyicisi’ni açın ve sol bölmede uygulamanızı seçin. "Yapılandırma bildirimleri" bölümünde bu uygulamayla ilgili herhangi bir tanılama iletisi olup olmadığını denetleyin:

  ![İstek, yanıt süresi, bağımlılık ve diğer verileri görmek için Performans dikey penceresini açın](./media/app-insights-monitor-performance-live-website-now/appinsights-status-monitor-diagnostics-message.png)
* "Yetersiz izinler" hakkında bir ileti görürseniz, sunucuda aşağıdakileri deneyin:
  * IIS Yöneticisi'nde, uygulama havuzunuzu seçin, **Gelişmiş Ayarlar**’ı açın ve **işlem modeli** altında kimliğini not edin.
  * Bilgisayar yönetimi denetim masasında, bu kimliği Performans İzleyicisi Kullanıcıları grubuna ekleyin.
* Sunucunuza yüklenmiş MMA/SCOM (System Center Operations Manager) varsa bazı sürümler çakışabilir. Hem SCOM’u, hem de Durum İzleyicisi’ni kaldırın ve en son sürümleri yeniden yükleyin.
* Bkz. [Sorun giderme][qna].

## <a name="system-requirements"></a>Sistem Gereksinimleri
Sunucuda Application Insights Durum İzleyicisi için işletim sistemi desteği:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows server 2012 R2
* Windows Server 2016

en son SP ve .NET Framework 4.5 ile

İstemci tarafında Windows 7, 8, 8.1 ve 10, .NET Framework 4.5 ile

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
* Derleme zamanında [Application Insights SDK'sı ile derlenmiş](app-insights-asp-net.md) web uygulamaları için ek telemetriyi etkinleştirme. 

### <a name="can-i-close-it-after-it-runs"></a>Çalıştıktan sonra kapatabilir miyim?

Evet. Seçtiğiniz web sitelerini izledikten sonra kapatabilirsiniz.

Telemetriyi tek başına toplamaz. Yalnızca web uygulamalarını yapılandırır ve bazı izinleri ayarlar.

### <a name="what-does-status-monitor-do"></a>Durum İzleyicisi ne yapar?

Durum İzleyicisi’nin izlemesi için bir web uygulaması seçtiğinizde:

* Application Insights derlemelerini ve .config dosyasını indirip web uygulamasının ikili dosyalar klasörüne yerleştirir.
* Application Insights HTTP izleme modülünü eklemek üzere `web.config` üzerinde değişiklik yapar.
* Bağımlılık çağrılarını toplamak için CLR profil oluşturma özelliğini etkinleştirir.

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

[Daha fazla bilgi](http://apmtips.com/blog/2016/11/18/how-application-insights-status-monitor-not-monitors-dependencies/)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next"></a>Sonraki adımlar

Telemetrinizi görüntüleyin:

* Performans ve kullanımı izlemek için [ölçümleri keşfedin](app-insights-metrics-explorer.md)
* Sorunların tanımlanması için [Olayları ve günlükleri arayın][diagnostic]
* Daha gelişmiş sorgular için [analiz](app-insights-analytics.md)
* [Panolar oluşturun](app-insights-dashboards.md)

Daha fazla telemetri ekleyin:

* Sitenizin canlı kalması için [web testleri oluşturun][availability].
* Web sayfası koduna ait özel durumları görmek ve izleme çağrıları eklemenize izin vermek için [web istemcisi telemetrisini ekleyin][usage].
* İzleme ve günlük çağrıları ekleyebilmek için [kodunuza Application Insights SDK’sı ekleyin][greenbrown]

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[roles]: app-insights-resources-roles-access-control.md
[usage]: app-insights-javascript.md

