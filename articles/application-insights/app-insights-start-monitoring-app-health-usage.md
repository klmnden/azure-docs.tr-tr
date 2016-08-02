<properties
    pageTitle="ASP.NET uygulamanızı izlemek için Application Insights SDK’sı ekleme | Microsoft Azure"
    description="Application Insights ile şirket içi veya Microsoft Azure web uygulamanızın kullanımını, kullanılabilirliğini ve performansını analiz edin."
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
    ms.date="02/04/2016"
    ms.author="awills"/>


# ASP.NET uygulamanızı izlemek için Application Insights SDK’sı ekleme

*Application Insights önizlemededir.*

[AZURE.INCLUDE [app-insights-selector-get-started](../../includes/app-insights-selector-get-started.md)]


Visual Studio Application Insights, [performans sorunlarını ve özel durumlarını saptayıp tanılamanıza][detect] ve [uygulamanızın nasıl kullanıldığını keşfetmenize][knowUsers] yardımcı olmak amacıyla canlı uygulamanızı izler. Çok çeşitli uygulama türleri ile birlikte kullanılabilir. Azure web uygulamalarının yanı sıra şirket içi kendi IIS sunucularınızda veya Azure sanal makinelerinde barındırılan uygulamalar için çalışır.



![Örnek performans izleme grafikleri](./media/app-insights-start-monitoring-app-health-usage/10-perf.png)

*Ayrıca bkz:*

* [ASP.NET 5](app-insights-asp-net-five.md)
* [Cihaz uygulamaları ve Java sunucuları][platformlar]

#### Başlamadan önce

Birçok uygulama türü için [Visual Studio sizin haberiniz bile olmadan uygulamanıza Application Insights ekleyebilir](#ide). Ancak neler olduğunu daha iyi anlamak üzere bu makaleyi okuduğunuz için bu adımları el ile yapmanıza kılavuzluk edeceğiz.


Gerekenler:

* Bir [Microsoft Azure](http://azure.com) aboneliği. Ekibinizin ve kuruluşunuzun Azure aboneliği varsa, sahibi [Microsoft hesabınızı](http://live.com) kullanarak sizi buna ekleyebilir.
* Visual Studio 2013 veya üstü.

## <a name="add"></a> 1. Application Insights kaynağı oluşturma

[Azure portalında][portal] oturum açın ve yeni bir Application Insights kaynağı oluşturun. Uygulama türü olarak ASP.NET’i seçin.

![Yeni, Application Insights öğesine tıklayın](./media/app-insights-start-monitoring-app-health-usage/01-new-asp.png)

Azure’da [kaynak][roles] bir hizmetin örneğidir. Bu kaynak, uygulamanızdan alınan telemetri verilerinin analiz edilip size sunulacağı yerdir.

Uygulama türü seçimi, kaynak dikey pencerelerinin varsayılan içeriğini ve [Ölçüm Gezgini][metrics] içinde görünen özellikleri belirler.

#### İzleme Anahtarını Kopyalama

Kaynağı tanımlayan bu anahtarı kısa bir süre sonra verileri kaynağa yönlendirmek için SDK’ya yükleyeceksiniz.

![Özellikler'e tıklayın, anahtarı seçin ve ctrl + C tuşlarına basın](./media/app-insights-start-monitoring-app-health-usage/02-props-asp.png)

Yeni bir kaynak oluşturmak üzere az önce uyguladığınız adımlar herhangi bir uygulamada izlemeyi başlatmanın iyi bir yoludur. Şimdi uygulamaya veri gönderebilirsiniz.

## <a name="sdk"></a> 2. Uygulamanıza SDK yükleme

Application Insights SDK'sının yüklenmesi ve yapılandırılması üzerinde çalıştığınız platforma bağlı olarak değişir. ASP.NET uygulamaları için kolaydır.

1. Visual Studio'da web uygulaması projenizin NuGet paketlerini düzenleyin.

    ![Projeye sağ tıklayın ve Nuget Paketlerini Yönet’i seçin](./media/app-insights-start-monitoring-app-health-usage/03-nuget.png)

2. Web Apps için Application Insights SDK’sı yükleme

    !["Application Insights" araması yapın](./media/app-insights-start-monitoring-app-health-usage/04-ai-nuget.png)

3. ApplicationInsights.config dosyasını (NuGet yüklemesinde eklenen dosya) düzenleyin. Kapanış etiketinin hemen önüne şunu ekleyin:

    `<InstrumentationKey>` *kopyaladığınız izleme anahtarı* `</InstrumentationKey>`

    (Dilerseniz uygulamanıza [bazı kodlar yazarak anahtarı ayarlayabilirsiniz][apikey].)

#### Gelecekteki SDK sürümlerine yükseltmek için

SDK’nın yeni sürümü zaman zaman yayınlanmaktadır.

[SDK'nın yeni sürümüne](app-insights-release-notes-dotnet.md) yükseltme yapmak için NuGet paket yöneticisini yeniden açın ve yüklü paketleri filtreleyin. **Microsoft.ApplicationInsights.Web** ve **Yükselt** öğelerini seçin.

ApplicationInsights.config dosyasında herhangi bir özelleştirme yaptıysanız yükseltmeden önce bir kopyasını kaydedin ve daha sonra değişikliklerinizi yeni sürümle birleştirin.


## <a name="run"></a> 3. Projenizi çalıştırma

**F5** ile uygulamanızı çalıştırın ve şunu deneyin: birkaç telemetri oluşturmak için farklı sayfalar açın.

Visual Studio'da gönderilmiş etkinliklerin sayısını görürsünüz.

![](./media/app-insights-start-monitoring-app-health-usage/appinsights-09eventcount.png)

## <a name="monitor"></a> 4. Telemetrinizi görüntüleme

[Azure portalı][portal]’na geri dönün Application Insights kaynağınıza göz atın.


Genel Bakış grafiklerinde veri arayın. İlk olarak yalnızca bir veya iki nokta görürsünüz. Örneğin:

![Daha fazla veri için tıklayın](./media/app-insights-start-monitoring-app-health-usage/12-first-perf.png)

Daha ayrıntılı ölçümler görmek için herhangi bir grafiğe tıklayın. [Ölçümler hakkında daha fazla bilgi edinin.][perf]

#### Veri yok mu?

* Birkaç telemetri oluşturması için farklı sayfaları açarak uygulamayı kullanın.
* Olayları tek tek görmek için [Arama][diagnostic] kutucuğunu açın. Bazı durumlarda olayların ölçüm ardışık düzenine ulaşması biraz daha uzun sürer.
* Birkaç saniye bekleyin ve **Yenile**’ye tıklayın. Grafikler kendilerini düzenli olarak yeniler, ancak bazı verilerin görüntülenmesini bekliyorsanız el ile yenileyebilirsiniz.
* Bkz. [Sorun giderme][qna].

## Uygulamanızı yayımlama

Şimdi uygulamanızı IIS veya Azure’a dağıtın ve verilerin birikmesini izleyin.

![Visual Studio kullanarak uygulamanızı yayımlama](./media/app-insights-start-monitoring-app-health-usage/15-publish.png)

Hata ayıklama modunda çalıştırdığınızda telemetri ardışık düzen üzerinden gönderilir, böylece görüntülenen verileri saniyeler içinde görebilirsiniz. Uygulamanızı Sürüm yapılandırmasında dağıttığınızda veriler daha yavaş birikir.

#### Sunucunuza yayımladıktan sonra veri yok mu?

Sunucunuzun güvenlik duvarında giden trafik için şu bağlantı noktalarını açın:

+ `dc.services.visualstudio.com:443`
+ `f5.services.visualstudio.com:443`


#### Derleme sunucunuzda sorun mu yaşıyorsunuz?

Lütfen [bu Sorun Giderme maddesine](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild) bakın.

> [AZURE.NOTE] Uygulamanız çok sayıda telemetri oluşturuyorsa (ve ASP.NET SDK sürüm 2.0.0-beta3 ya da üstünü kullanıyorsanız), uyarlamalı örnekleme modülü olayların yalnızca bir temsilci fraksiyonunu göndererek portala gönderilen hacmi otomatik olarak azaltır. Ancak, aynı istekle ilişkili olaylar grup olarak seçilir ya da seçimi kaldırılır, böylece ilgili olaylar arasında gezinebilirsiniz. 
> [Örnekleme hakkında bilgi edinin](app-insights-sampling.md).


## 5. Bağımlılık izleme (ve IIS performans sayaçları) ekleme

SDK’nın bazı verilere erişmesi için biraz yardım gereklidir. Özellikle, uygulamanızdan veritabanlarına, REST API'lerine ve diğer dış bileşenlere gönderilen çağrıları otomatik olarak ölçmek üzere bu ek adım gerekli olabilir. Bu bağımlılık ölçümleri performans sorunlarını tanılamanıza yardımcı olmada çok yararlı olabilir.

Kendi IIS sunucunuzu çalıştırıyorsanız bu adım ayrıca sistem performans sayaçlarının [ölçüm gezgininde](app-insights-metrics-explorer.md) görünmesine imkan tanır.

#### Uygulamanız IIS sunucunuzda çalışıyorsa

Sunucunuzda yönetici haklarıyla oturum açın ve [Application Insights Durum İzleyicisi](http://go.microsoft.com/fwlink/?LinkId=506648)’ni yükleyin.

[Güvenlik duvarınızda ek giden bağlantı noktaları açmanız](app-insights-monitor-performance-live-website-now.md#troubleshooting) gerekebilir.

Bu adım ayrıca CPU, bellek, ağ kapasitesi gibi konularda [performans sayaçlarını raporlamayı](app-insights-web-monitor-performance.md#system-performance-counters) sağlar.

#### Uygulamanız bir Azure Web Uygulaması ise

Azure Web Uygulamanızın denetim masasında Application Insights uzantısını ekleyin.

![Web uygulamanızda Ayarlar, Uzantılar, Ekle, Application Insights](./media/app-insights-start-monitoring-app-health-usage/05-extend.png)


#### Bir Azure cloud services projesiyse

[Web ve çalışan rollerine komut dosyaları ekleyin](app-insights-cloudservices.md).



## 6. İstemci tarafı izleme ekleme

Uygulamanızın sunucusundan (arka uç) telemetri verileri gönderen SDK’yı yüklediniz. İstemci tarafı izlemeyi artık ekleyebilirsiniz. Bu özellik kullanıcılar, oturumlar, sayfa görünümleri ve tarayıcıda oluşan her türlü özel durum ya da kilitlenme hakkında veriler sağlar. Ayrıca, kullanıcılarınızın uygulamanızla nasıl çalıştığını izlemek üzere, tıklama ve tuş darbelerinin ayrıntılarına inerek kendi kodunuzu yazabilirsiniz.


Her sayfaya JavaScript kod parçacığı ekleyin. Application Insights kaynağınızdan kodu alın:

![Web uygulamanızda Hızlı Başlangıç’ı açın ve 'Web sayfalarımı izlemeyi sağlayan kodu al' seçeneğine tıklayın](./media/app-insights-start-monitoring-app-health-usage/02-monitor-web-page.png)

Kodun uygulama kaynağınızı tanımlayan izleme anahtarını içerdiğinden emin olun.

[Web sayfası izleme hakkında daha fazla bilgi edinin.](app-insights-web-track-usage.md)


## Uygulama sürümünü izleme

`buildinfo.config` öğesinin MSBuild işleminiz tarafından oluşturulduğundan emin olun. .csproj dosyanızda şunları ekleyin:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup> 
```

Yapı bilgisi mevcut olduğunda Application Insights web modülü **Uygulama sürümünü** telemetrinin her bir öğesine bir özellik olarak ekler. Bunun yapılması [tanılama aramaları][diagnostic] gerçekleştirirken veya [ölçümleri keşfederken][metrics] sürüme göre filtrelemenize imkan tanır. 

Ancak, derleme sürüm numarasının Visual Studio’daki geliştirici derlemesi tarafından değil, yalnızca MS Build tarafından oluşturulduğuna dikkat edin.

## 7. Yüklemenizi tamamlama

Uygulamanızın 360 derecelik tam görünümünü almak için yapabileceğiniz birkaç şey daha vardır:

* Uygulamanızın canlı ve duyarlı kaldığından emin olmak için [web testleri oluşturma][availability].
* Sık kullandığınız günlük altyapısından [günlük izlemelerini yakalama][netlogs]
* Uygulamanızın nasıl kullanıldığı hakkında daha fazla bilgi edinmek için istemcide veya sunucuda ya da her ikisinde [özel olayları ve ölçümleri izleme][api].

## <a name="ide"></a> Otomatik yol

Bu makalenin başlangıcında bir Application Insights kaynağını el ile oluşturma ve ardından SDK’yı yükleme işlemini göstereceğimizi söylemiştik. Bu yordamın iki bölümden oluştuğunu anlamak önemlidir. Ancak ASP.NET uygulamaları (ve diğer birçoğu) için daha da hızlı bir otomatik yöntem mevcuttur.

Bunun için [Visual Studio](http://go.microsoft.com/fwlink/?linkid=397827&clcid=0x409) (2013 güncelleştirme 3 veya üstü) ve [Microsoft Azure](http://azure.com)’da bir hesap gereklidir.

#### Yeni bir projeyse...

Visual Studio'da yeni bir proje oluşturduğunuzda **Application Insights Ekle** seçeneğinin işaretli olduğundan emin olun.


![ASP.NET projesi oluşturma](./media/app-insights-start-monitoring-app-health-usage/appinsights-01-vsnewp1.png)

Visual Studio, Application Insights'ta bir kaynak oluşturur, SDK’yı projenize ekler ve anahtarı `.config` dosyasına yerleştirir.

Projenizde web sayfaları varsa [JavaScript SDK’sını][client] da web sayfasına ekler.

#### ... veya mevcut bir projeyse

Çözüm Gezgini'nde projeye sağ tıklayın ve **Application Insights Ekle**’yi seçin.

![Application Insights Ekle’yi seçme](./media/app-insights-start-monitoring-app-health-usage/appinsights-03-addExisting.png)

Visual Studio, Application Insights'ta bir kaynak oluşturur, SDK’yı projenize ekler ve anahtarı `.config` dosyasına yerleştirir.

Bu durumda [JavaScript SDK’sı][client] web sayfalarınıza eklenmez; bunu sonraki adımda yapmanız önerilir.

#### Kurulum seçenekleri

İlk kez kurulum yapıyorsanız Microsoft Azure Preview’e kaydolmanız ya da oturum açmanız istenir. 

Bu uygulama daha büyük bir uygulamanın parçası ise diğer bileşenlerle aynı kaynak grubuna yerleştirmek için **Ayarları yapılandır**’ı kullanmak isteyebilirsiniz.

*Application Insights seçeneği yok mu? Visual Studio 2013 Güncelleştirme 3 veya üstünü kullandığınızdan ve Uzantılar ve Güncelleştirmeler menüsünde Application Insights Araçları’nın etkinleştirildiğinden emin olun.*

#### Projenizden Application Insights’ı açın

![Projenize sağ tıklayıp Azure portalını açın](./media/app-insights-start-monitoring-app-health-usage/appinsights-04-openPortal.png)




## <a name="video"></a>Video

> [AZURE.VIDEO getting-started-with-application-insights]


<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apikey]: app-insights-api-custom-events-metrics.md#ikey
[availability]: app-insights-monitor-web-app-availability.md
[azure]: ../insights-perf-analytics.md
[client]: app-insights-javascript.md
[detect]: app-insights-detect-triage-diagnose.md
[diagnostic]: app-insights-diagnostic-search.md
[knowUsers]: app-insights-overview-usage.md
[metrics]: app-insights-metrics-explorer.md
[netlogs]: app-insights-asp-net-trace-logs.md
[perf]: app-insights-web-monitor-performance.md
[platformlar]: app-insights-platforms.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[roles]: app-insights-resources-roles-access-control.md
[start]: app-insights-overview.md



<!--HONumber=Jun16_HO2-->


