<properties
    pageTitle="Çalışan IIS web sitesinde performans sorunlarını tanılama | Microsoft Azure"
    description="Yeniden dağıtmadan web sitesinin performansını izleme. Bağımlılık telemetrisi almak için Application Insights SDK’yi tek başına veya birlikte kullanın."
    services="application-insights"
    documentationCenter=".net"
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="03/09/2016"
    ms.author="awills"/>


# Web sitesi performansını izlemek için Application Insights Durum İzleyicisi’ni yükleme

*Application Insights önizlemededir.*

Visual Studio Application Insights’ın Durum İzleyicisi, ASP.NET uygulamalarında özel durumları ve performans sorunlarını tanılamanızı sağlar. 

![örnek grafikler](./media/app-insights-monitor-performance-live-website-now/10-intro.png)

> [AZURE.TIP] [Canlı J2EE web uygulamaları](app-insights-java-live.md) ve [Azure Cloud Services](app-insights-cloudservices.md) izleme hakkında ayrı makaleler vardır.


IIS web uygulamalarınıza Application Insights uygulamak için üç yolunuz vardır:

* **Derleme süresi:** Web uygulaması kodunuza [Application Insights SDK'sini ekleyin][greenbrown]. Şunları alırsınız:
 * Bir dizi standart tanılama ve kullanım telemetrisi.
 * [Application Insights API’si][api] ayrıntılı kullanımı izlemek veya sorunları tanılamak için kendi telemetrinizi yazmanızı sağlar.
* **Çalışma zamanı:** Sunucuda web uygulamanızı izlemek isterseniz Durum İzleyicisi’ni kullanın.
 * Zaten çalışan web uygulamalarını izleme: bunları yeniden oluşturmanız veya yeniden yayımlamanız gerekmez.
 * Bir dizi standart tanılama ve kullanım telemetrisi.
 * Bağımlılık tanılama &#151; uygulamanızın veritabanları, REST API'leri veya diğer hizmetler gibi başka bileşenleri kullandığı yerde hataları veya yetersiz performansı bulun.
 * Telemetriyle ilgili sorunları giderin.
* **Her ikisi:** SDK’yi web uygulama kodunuzda derleyip Durum izleyicisi’ni web sunucunuzda çalıştırın.  Her iki dünyanın da en iyisi:
 * Standart tanılama ve kullanım telemetrisi.
 * Bağımlılık tanılama.
 * API özel telemetri yazmanızı sağlar.
 * SDK ve telemetriyle ilgili sorunları giderin.


## Application Insights Durum İzleyicisi yükleme

[Microsoft Azure](http://azure.com) aboneliğiniz olması gerekir.

### Uygulamanız IIS sunucunuzda çalışıyorsa

1. IIS web sunucunuzda yönetici kimlik bilgileriyle oturum açın.
2. [Durum İzleyicisi yükleyici](http://go.microsoft.com/fwlink/?LinkId=506648)’yi indirip çalıştırın.
4. Yükleme Sihirbazı'ndaki Microsoft Azure'da oturum açın.

    ![Microsoft hesabı kimlik bilgilerinizle Azure’de oturum açın](./media/app-insights-monitor-performance-live-website-now/appinsights-035-signin.png)

    *Bağlantı hatası mı var? Bkz. [Sorun giderme](#troubleshooting).*

5. İzlemek istediğiniz yüklü web uygulamasını veya web sitesini seçip sonuçlarını Application Insights portalında görmek istediğiniz kaynağı yapılandırın.

    ![Uygulama ve kaynak seçin.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-configAIC.png)

    Normalde, yapılandırmak üzere yeni bir kaynak ve [kaynak grubu][roles] seçersiniz.

    bunun yerine, siteniz için [web testleri][availability] veya [web istemcisi izleme][client] ayarladıysanız var olan kaynağı kullanın.

6. IIS’yi yeniden başlatın.

    ![iletişim kutusunun üstündeki Yeniden Başlat’ı seçin.](./media/app-insights-monitor-performance-live-website-now/appinsights-036-restart.png)

    Web hizmetiniz kısa bir süre kesintiye uğrar.

6. İzlemek istediğiniz web uygulamalarına ApplicationInsights.config eklendiğinden emin olun.

    ![Web uygulamasının kod dosyalarında .config dosyasını bulun.](./media/app-insights-monitor-performance-live-website-now/appinsights-034-aiconfig.png)

   web.config dosyasında da bazı değişiklikler vardır.

#### Daha sonra (yeniden) yapılandırmak istiyor musunuz?

Sihirbazı tamamladıktan sonra istediğiniz zaman aracıyı yeniden yapılandırabilirsiniz. Bu aracıyı yüklediyseniz, ancak ilk kurulumda bazı sorunlar oluştuysa da kullanabilirsiniz.

![Görev çubuğunda Application Insights simgesine tıklama](./media/app-insights-monitor-performance-live-website-now/appinsights-033-aicRunning.png)


### Uygulamanız Azure Web Uygulaması çalıştırıyorsa

Azure Web Uygulamanızın denetim masasında Application Insights uzantısını ekleyin.

![Web uygulamanızda Ayarlar, Uzantılar, Ekle, Application Insights](./media/app-insights-monitor-performance-live-website-now/05-extend.png)


### Bir Azure cloud services projesiyse

[Web ve çalışan rollerine komut dosyaları ekleyin](app-insights-cloudservices.md).


## Performans telemetrisini görüntüleme

[Azure portalında](https://portal.azure.com) oturum açın, Application Insights’a gidip oluşturduğunuz kaynağı açın.

![Gözat’ı, Application Insights'ı, sonra da uygulamanızı seçme](./media/app-insights-monitor-performance-live-website-now/appinsights-08openApp.png)

İstek, yanıt süresi, bağımlılık ve diğer verileri görmek için Performans dikey penceresini açın.

![Performans](./media/app-insights-monitor-performance-live-website-now/21-perf.png)

Görüntülenenlerin ayrıntılarını ayarlamak için tıklayın veya yeni bir grafik ekleyin.


![](./media/app-insights-monitor-performance-live-website-now/appinsights-038-dependencies.png)

## Bağımlılıklar

Bağımlılık Süresi grafiği uygulamanızdan veritabanları, REST API’leri veya Azure blob depolamaya yapılan çağrıların ne kadar süre aldığını gösterir.

Grafiği farklı bağımlılıklara çağrıya göre bölümlere ayırmak için grafiği seçin, Gruplandırma’yı açın, sonra da Bağımlılık, Bağımlılık Türü veya Bağımlılık Performansı’nı seçin.

Belirli bir bağımlılığa, türe veya performans demetine bakmak için de grafiğe filtre uygulayabilirsiniz. Filtreler'e tıklayın.

## Performans sayaçları

(Azure web uygulamaları için değil.) CPU doluluğu ve bellek kullanımı gibi sunucu performans sayacı grafiklerini görmek için genel bakış dikey penceresinde Sunucular’a tıklayın.

Yeni bir grafik ekleyin veya gösterdiklerini değiştirmek için herhangi bir grafiğe tıklayın. 

Ayrıca, [SDK tarafından bildirilen performans sayaçları kümesini değiştirebilirsiniz](app-insights-configuration-with-applicationinsights-config.md#nuget-package-3). 

## Özel durumlar

![Sunucu özel durumları grafiğine tıklayın](./media/app-insights-monitor-performance-live-website-now/appinsights-039-1exceptions.png)

Belirli özel durumların ayrıntısına gidebilir (son yedi gün) ve yığın izlemeleri ve bağlam verileri alabilirsiniz.

## Örnekleme

Uygulamanız çok miktarda veri gönderiyorsa ve ASP.NET sürüm 2.0.0-beta3 veya daha sonraki uygulamalar için Application Insights SDK kullanıyorsanız, uyarlamalı örnekleme özelliği telemetrenizin yalnızca yüzdesini çalıştırır ve gönderir. [Örnekleme hakkında daha fazla bilgi edinin.](app-insights-sampling.md)


## Sorun giderme

### Bağlantı hataları

Durum İzleyicisi’ne izin vermek için sunucunuzun güvenlik duvarında bazı giden bağlantı noktalarını açmanız gerekir:

+ Telemetri - bunlar her zaman gereklidir:
 +  `dc.services.visualstudio.com:80`
 +  `dc.services.visualstudio.com:443`
 +  `dc.applicationinsights.microsoft.com`
+ Yapılandırma - yalnızca değişiklik yaparken gerekir:
 -  `management.core.windows.net:443`
 -  `management.azure.com:443`
 -  `login.windows.net:443`
 -  `login.microsoftonline.com:443`
 -  `secure.aadcdn.microsoftonline-p.com:443`
 -  `auth.gfx.ms:443`
 -  `login.live.com:443`
+ Yükleme:
 +  `packages.nuget.org:443`

Bu liste zaman zaman değiştirilebilir.

### Telemetri yok mu?

  * Bazı verileri oluşturmak için sitenizi kullanın.
  * Verilerin gelmesi için birkaç dakika bekleyin ve **Yenile**’ye tıklayın.
  * Olayları tek tek görmek için Tanılama Arama’yı (Ara kutucuğu) açın. Veri toplama grafiklerde görünmeden önce olaylar genellikle Tanılama Arama’da görülebilir.
  * Durum İzleyicisi’ni açın ve sol bölmede uygulamanızı seçin. "Yapılandırma bildirimleri" bölümünde bu uygulamayla ilgili herhangi bir tanılama iletisi olup olmadığını denetleyin:

  ![](./media/app-insights-monitor-performance-live-website-now/appinsights-status-monitor-diagnostics-message.png)

  * Sunucu güvenlik duvarının yukarıda listelenen bağlantı noktalarında giden trafiğe izin verdiğinden emin olun.
  * "Yetersiz izinler" hakkında bir ileti görürseniz, sunucuda aşağıdakileri deneyin:
    * IIS Yöneticisi'nde, uygulama havuzunuzu seçin, **Gelişmiş Ayarlar**’ı açın ve **işlem modeli** altında kimliğini not edin.
    * Bilgisayar yönetimi denetim masasında, bu kimliği Performans İzleyicisi Kullanıcıları grubuna ekleyin.
  * Sunucunuza yüklenmiş MMA/SCOM varsa bazı sürümler çakışabilir. Hem SCOM’u, hem de Durum İzleyicisi’ni kaldırın ve en son sürümleri yeniden yükleyin.
  * Bkz. [Sorun giderme][qna].

## Sistem Gereksinimleri

Sunucuda Application Insights Durum İzleyicisi için işletim sistemi desteği:

- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows server 2012 R2

en son SP ve .NET Framework 4.0 ve 4.5 ile

İstemci tarafında Windows 7, 8 ve 8.1, .NET Framework 4.0 ve 4.5 ile yeniden

IIS desteği: IIS 7, 7.5, 8, 8.5 (IIS gereklidir)

## PowerShell ile Automation

PowerShell kullanarak izlemeyi başlatabilir ve durdurabilirsiniz.

`Get-ApplicationInsightsMonitoringStatus [-Name appName]`

* `-Name` (İsteğe bağlı) Web uygulamasının adı.
* Bu IIS sunucusundaki her web uygulaması için (veya adlandırılmış uygulama) Application Insights izleme durumunu görüntüler.

* Her uygulama için `ApplicationInsightsApplication` döndürür:
 * `SdkState==EnabledAfterDeployment`: Uygulama izlenmektedir ve Durum İzleyicisi aracıyla veya `Start-ApplicationInsightsMonitoring` ile çalışma zamanında izlenmektedir.
 * `SdkState==Disabled`: Bu uygulama Application Insights için izlenmemektedir. Hiçbir zaman izlenmeyecek veya Durum İzleyicisi aracıyla veya `Stop-ApplicationInsightsMonitoring` ile çalışma zamanı izlemesi devre dışı bırakılmayacaktır.
 * `SdkState==EnabledByCodeInstrumentation`: Kaynak koduna SDK eklenerek uygulama izlendi. SDK’si güncelleştirilemez veya durdurulamaz.
 * `SdkVersion` bu uygulamanın izlenmesi için kullanılan sürümünü gösterir.
 * `LatestAvailableSdkVersion`şu anda NuGet galerisinde uygun sürümü gösterir. Uygulama bu sürüme yükseltmek için `Update-ApplicationInsightsMonitoring` kullanın.

`Start-ApplicationInsightsMonitoring -Name appName -InstrumentationKey 00000000-000-000-000-0000000`

* `-Name` IIS’de uygulamanın adı
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

* `-Name` IIS’de bir uygulamanın adı
* `-All` Bu IIS sunucusunun tüm uygulamaları için izlemeyi durdurur: `SdkState==EnabledAfterDeployment`

* Belirtilen uygulamaları izlemeyi durdurur ve izlemeyi kaldırır. Yalnızca, Durum İzleme aracını veya Start-ApplicationInsightsApplication kullanarak çalışma zamanında izleme eklenmiş uygulamalar için çalışır. (`SdkState==EnabledAfterDeployment`)

* ApplicationInsightsApplication döndürür.

`Update-ApplicationInsightsMonitoring -Name appName [-InstrumentationKey "0000000-0000-000-000-0000"`]

* `-Name`: IIS’de web uygulamasının adı.
* `-InstrumentationKey` (İsteğe bağlı.) Uygulamanın telemetri gönderildiği kaynağı değiştirmek için kullanın.
* Bu cmdlet:
 * Adlandırılmış uygulamaları SDK'nin bu makineye en son indirilen sürümüne yükseltir. (Yalnızca `SdkState==EnabledAfterDeployment` ise çalışır)
 * İzleme anahtarını sağlarsanız, adlandırılmış uygulama, telemetriyi bu anahtarla kaynağa göndermek için yeniden yapılandırılır. (`SdkState != Disabled` ise çalışır)

`Update-ApplicationInsightsVersion`

* En son Application Insights SDK’sini sunucuya indirir.

## Azure şablonu

web uygulaması Azure’deyse ve Azure Resource Manager şablonu kullanarak kaynaklarınızı oluşturuyorsanız, bunu kaynak düğümüne ekleyerek Application Insights’ı yapılandırabilirsiniz:

    {
      resources: [
        /* Create Application Insights resource */
        {
          "apiVersion": "2015-05-01",
          "type": "microsoft.insights/components",
          "name": "nameOfAIAppResource",
          "location": "centralus",
          "kind": "web",
          "properties": { "ApplicationId": "nameOfAIAppResource" },
          "dependsOn": [
            "[concat('Microsoft.Web/sites/', myWebAppName)]"
          ]
        }
       ]
     } 

* `nameOfAIAppResource` - Application Insights kaynağı için bir ad
* `myWebAppName` - web uygulaması kimliği

## <a name="next"></a>Sonraki adımlar

* Sitenizin canlı kalması için [web testleri oluşturun][availability].
* Sorunların tanımlanması için [Olayları ve günlükleri arayın][diagnostic].
* Web sayfası koduna ait özel durumları görmek ve izleme çağrıları eklemenize izin vermek için [web istemcisi telemetrisini ekleyin][usage].
* [Web hizmeti kodunuza Application Insights SDK ekleyin][greenbrown]; böylece, izleme ekleyebilir ve sunucu koduna çağrı kaydedebilirsiniz.

## Video

#### Performans izleme

[AZURE.VIDEO app-insights-performance-monitoring]

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[roles]: app-insights-resources-roles-access-control.md
[usage]: app-insights-web-track-usage.md



<!--HONumber=Jun16_HO2-->


