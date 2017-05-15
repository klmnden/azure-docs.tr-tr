---
title: "Azure Cloud Services için Application Insights | Microsoft Docs"
description: "Application Insights ile web ve çalışan rollerinizi etkili bir şekilde izleyin"
services: application-insights
documentationcenter: 
author: alancameronwills
manager: carmonm
editor: alancameronwills
ms.assetid: 5c7a5b34-329e-42b7-9330-9dcbb9ff1f88
ms.service: application-insights
ms.devlang: na
ms.tgt_pltfrm: ibiza
ms.topic: get-started-article
ms.workload: tbd
ms.date: 05/05/2017
ms.author: awills
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: bfae0fcf992c38d7afef6140fdd79d87ab0ecb4f
ms.contentlocale: tr-tr
ms.lasthandoff: 05/10/2017


---
# <a name="application-insights-for-azure-cloud-services"></a>Azure Cloud Services için Application Insights
[Microsoft Azure Cloud hizmeti uygulamalarının](https://azure.microsoft.com/services/cloud-services/) kullanılabilirliği, performansı, hataları ve kullanımı [Application Insights][start] tarafından izlenebilir. Uygulamanızın gerçek hayattaki performansı ve etkinliğine ilişkin aldığınız geri bildirimlerden yararlanarak her geliştirme yaşam döngüsünde tasarımın yönü konusunda bilinçli kararlar alabilirsiniz.

![Örnek](./media/app-insights-cloudservices/sample.png)

## <a name="before-you-start"></a>Başlamadan önce
Gerekenler:

* [Microsoft Azure](http://azure.com) içeren bir abonelik. Windows, XBox Live veya diğer Microsoft bulut hizmetlerinde kullanıyor olabileceğiniz bir Microsoft hesabıyla oturum açın. 
* Microsoft Azure araçları 2.9 veya üzeri
* Developer Analytics Tools 7.10 veya üzeri

## <a name="quick-start"></a>Hızlı başlangıç
Bulut hizmetinizi Application Insights ile izlemenin en hızlı ve kolay yolu, uygulamanızı Azure’da yayımlarken bu seçeneği belirlemektir.

![Örnek](./media/app-insights-cloudservices/azure-cloud-application-insights.png)

Bu seçenek çalışma zamanında uygulamanızı izleyerek web rolünüzdeki istekleri, özel durumları ve bağımlılıkları izlemek için ihtiyacınız olan tüm telemetri öğelerinin yanı sıra çalışan rollerinizden performans sayaçlarını izlemenize imkan tanır. Uygulamanız tarafından oluşturulan tüm tanılama izlemeleri Application Insights’a da gönderilir.

Tek ihtiyacınız olan buysa başka bir şey yapmanız gerekmez! Sonraki adımlar [uygulamanızdan alınan ölçümleri görüntüleme](app-insights-metrics-explorer.md), [Analytics ile verilerinizi sorgulama](app-insights-analytics.md) ve isteğe bağlı olarak bir [pano](app-insights-dashboards.md) ayarlama üzerinedir. Tarayıcıda performansı izlemek için [kullanılabilirlik testleri](app-insights-monitor-web-app-availability.md) ayarlamak ve [web sayfalarınıza kod eklemek](app-insights-javascript.md) isteyebilirsiniz.

Bununla birlikte, daha fazla seçeneğe de sahip olabilirsiniz:

* Farklı bileşenlerden veri gönderin ve kaynakları ayırmak için çeşitli yapılandırmalar oluşturun.
* Uygulamanızdan özel telemetri ekleyin.

Bu seçenekler ilginizi çekiyorsa okumaya devam edin.

## <a name="sample-application-instrumented-with-application-insights"></a>Application Insights ile izlenen Örnek Uygulama
Azure’da barındırılan iki çalışan rolüne sahip bir bulut hizmetine Application Insights’ın eklendiği bu [örnek uygulamayı](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService) inceleyin. 

Aşağıda, kendi bulut hizmeti projenizi aynı şekilde nasıl uyarlayabileceğiniz açıklanmıştır.

## <a name="plan-resources-and-resource-groups"></a>Kaynakları ve kaynak gruplarını planlama
Uygulamanızdan alınan telemetri, Application Insights türündeki bir Azure kaynağında depolanır, analiz edilir ve görüntülenir. 

Her kaynak bir kaynak grubuna aittir. Kaynak grupları maliyetleri yönetmek, takım üyelerine erişim izni vermek ve güncelleştirmeleri tek bir eşgüdümlü işlemle dağıtmak için kullanılır. Örneğin, bir Azure Cloud Service hizmetini ve bunun Application Insights izleme kaynaklarını tek bir işlemde [dağıtmak için bir betik yazabilirsiniz](../azure-resource-manager/resource-group-template-deploy.md).

### <a name="resources-for-components"></a>Bileşenler için kaynaklar
Önerilen şema, uygulamanızın her bir bileşeni (yani, her bir web rolü ve çalışan rolü) için ayrı bir kaynak oluşturmaktır. Her bir bileşeni ayrı ayrı analiz edebileceğiniz gibi, tüm bileşenlerden toplanan önemli grafikleri bir araya getiren bir [pano](app-insights-dashboards.md) oluşturarak bunları birlikte karşılaştırma ve izleme olanağından da yararlanabilirsiniz. 

Alternatif bir şema, birden fazla rolden alınan telemetrinin aynı kaynağa gönderilmesi, ancak kaynak kodunu tanımlamak için [her telemetri öğesine bir boyut özelliği eklenmesidir](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer). Bu şemada, özel durumlar gibi ölçüm grafikleri normalde farklı rollerden alınan sayımların toplamını gösterir, ancak gerektiğinde grafiği rol tanımlayıcısına göre bölümlere ayırabilirsiniz. Aramalar da aynı boyuta göre filtrelenebilir. Bu alternatif yöntem her şeyin aynı anda görüntülenmesi biraz daha kolaylaştırır, ancak rollerin ayırt edilmesi konusunda biraz karışıklığa yol açma ihtimali de vardır.

Tarayıcı telemetrisi genellikle ait olduğu sunucu tarafı web rolüyle aynı kaynağa dahil edilir.

Farklı bileşenlere yönelik Application Insights kaynaklarını aynı kaynak grubuna ekleyin. Bu yöntem, kaynakların birlikte yönetilmesini kolaylaştırır. 

### <a name="separating-development-test-and-production"></a>Ayırma, geliştirme, test ve üretim
Bir önceki sürümünüz yayındayken yen özelliğiniz için özel olaylar geliştiriyorsanız, geliştirme telemetrisini ayrı bir Application Insights kaynağına göndermeniz mantıklı olur. Aksi takdirde, canlı siteden gelen yoğun trafik arasında test telemetrinizi bulmakta zorlanabilirsiniz.

Bu durumdan kaçınmak istiyorsanız her bir derleme yapılandırması veya sisteminizin her bir ‘damgası’ (geliştirme, test, üretim, vs.) için ayrı kaynaklar oluşturun. Her derleme yapılandırmasına ait kaynakları ayrı bir kaynak grubuna ekleyin. 

Uygun kaynaklara telemetri göndermek için Application Insights SDK’sını derleme yapılandırmasına göre farklı bir izleme anahtarı alacak şekilde yapılandırabilirsiniz. 

## <a name="create-an-application-insights-resource-for-each-role"></a>Her rol için bir Application Insights kaynağı oluşturma
Her rol için ayrı bir kaynak oluşturmaya, hatta her derleme yapılandırması için ayrı bir küme oluşturmaya karar verdiyseniz, bunların tümünü Application Insights portalında oluşturmak en kolay yöntemdir. (Çok kaynak oluşturuyorsanız [işlemi otomatikleştirebilirsiniz](app-insights-powershell.md).

1. [Azure portalında][portal] yeni bir Application Insights kaynağı oluşturun. Uygulama türü olarak ASP.NET uygulamasını seçin. 

    ![Yeni, Application Insights öğesine tıklayın](./media/app-insights-cloudservices/01-new.png)
2. Her kaynağın bir İzleme Anahtarı ile tanımlandığını unutmayın. Daha sonra SDK’yı el ile yapılandırmak veya SDK yapılandırmasını doğrulamak isterseniz buna ihtiyacınız olabilir.

    ![Özellikler'e tıklayın, anahtarı seçin ve ctrl + C tuşlarına basın](./media/app-insights-cloudservices/02-props.png) 

## <a name="set-up-azure-diagnostics-for-each-role"></a>Her rol için Azure Tanılama ayarlama
Uygulamanızı Application Insights ile izlemek için bu seçeneği ayarlayın. Bu seçenek, web rolleri için performans izleme, uyarılar ve tanılamanın yanı sıra kullanım analizi sağlar. Diğer roller için yeniden başlatma, performans sayaçları ve System.Diagnostics.Trace çağrıları gibi Azure tanılamalarını arayabilir ve izleyebilirsiniz. 

1. Visual Studio Çözüm Gezgini'nde, &lt;Bulut Hizmetinizin&gt; Roller bölümünden her bir rolün özelliklerini açın.
2. **Yapılandırma** bölümünde **Tanılama verilerini Application Insights'a gönder** seçeneğini ayarlayın ve daha önce oluşturduğunuz uygun Application Insights kaynağını seçin.

Her derleme yapılandırması için ayrı bir Application Insights kaynağı kullanmaya karar verdiyseniz önce yapılandırmayı seçin.

![Her Azure rolünün özelliklerinde Application Insights’ı ayarlayın](./media/app-insights-cloudservices/configure-azure-diagnostics.png)

Bu, Application Insights izleme anahtarlarınızın `ServiceConfiguration.*.cscfg` adlı dosyalara eklenmesini sağlar. ([Örnek kod](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/AzureEmailService/ServiceConfiguration.Cloud.cscfg)).

Application Insights’a gönderilen tanılama bilgilerinin düzeyini değiştirmek isterseniz [`.cscfg` dosyalarını doğrudan düzenleyerek](app-insights-azure-diagnostics.md) bunu gerçekleştirebilirsiniz.

## <a name="sdk"></a>Her projede SDK’yı yükleyin
Bu seçenek, uygulamanızın nasıl kullanıldığını ve performansını daha ayrıntılı olarak analiz etmek için herhangi bir role özel iş telemetrisi ekleme özelliğini ekler.

Visual Studio’da her bulut uygulaması projesi için Application Insights SDK’sını yapılandırın.

1. **Web rolleri**: Projeye sağ tıklayıp **Application Insights’ı Yapılandır**’ı veya **Ekle > Application Insights telemetrisi** seçeneğini belirleyin.

2. **Çalışan rolleri**: 
 * Projeye sağ tıklayıp **Nuget Paketlerini Yönet**’i seçin.
 * [Windows Sunucuları için Application Insights](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/)’ı ekleyin.

    !["Application Insights" araması yapın](./media/app-insights-cloudservices/04-ai-nuget.png)

3. SDK’yı Application Insights kaynağına veri gönderecek biçimde yapılandırın.

    Uygun bir başlangıç işlevinde, .cscfg dosyasındaki yapılandırma ayarından izleme anahtarını ayarlayın:
 
    ```C#
   
     TelemetryConfiguration.Active.InstrumentationKey = RoleEnvironment.GetConfigurationSettingValue("APPINSIGHTS_INSTRUMENTATIONKEY");
    ```
   
    Bunu uygulamanızdaki her rol için gerçekleştirin. Örneklere bakın:
   
   * [Web rolü](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Global.asax.cs#L27)
   * [Çalışan rolü](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L232)
   * [Web sayfaları için](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Views/Shared/_Layout.cshtml#L13) 
4. ApplicationInsights.config dosyasını her zaman çıkış dizinine kopyalanacak şekilde ayarlayın. 
   
    (.config dosyasında, izleme anahtarını oraya eklemenizi isteyen mesajlar görürsünüz. Ancak, bulut uygulamaları için bunun .cscfg dosyasından ayarlanması daha iyidir. Bu, portalda rolün doğru tanımlanmasını sağlar.)

#### <a name="run-and-publish-the-app"></a>Uygulamayı çalıştırma ve yayımlama
Uygulamanızı çalıştırın ve Azure'da oturum açın. Oluşturduğunuz Application Insights kaynaklarını açtığınızda, [Ara](app-insights-diagnostic-search.md) kutucuğunda tek tek veri noktalarının, [Ölçüm Gezgini](app-insights-metrics-explorer.md)’nde ise toplu verilerin göründüğünü görebilirsiniz. 

Daha fazla telemetri ekleyin (aşağıdaki bölümlere bakın) ve sonra canlı tanılama ve kullanım geri bildirimi almak için uygulamanızı yayımlayın. 

#### <a name="no-data"></a>Veri yok mu?
* Olayları tek tek görmek için [Ara][diagnostic] kutucuğunu açın.
* Birkaç telemetri oluşturması için farklı sayfaları açarak uygulamayı kullanın.
* Birkaç saniye bekleyin ve Yenile’ye tıklayın.
* Bkz. [Sorun giderme][qna].

## <a name="view-azure-diagnostic-events"></a>Azure tanılama olaylarını görüntüleme
Tanılamalar nerede bulunur:

* Performans sayaçları özel ölçümler olarak görüntülenir. 
* Windows olay günlükleri izlemeler ve özel olaylar olarak gösterilir.
* Uygulama günlükleri, ETW günlükleri ve varsa tanılama altyapısı günlükleri izlemeler olarak görünür.

Performans sayaçlarını ve olay sayısını görmek için [Ölçüm Gezgini](app-insights-metrics-explorer.md)’ni açıp yeni bir grafik ekleyin:

![Azure tanılama verileri](./media/app-insights-cloudservices/23-wad.png)

Azure Tanılama tarafından gönderilen çeşitli izleme günlüklerinde arama yapmak için [Ara](app-insights-diagnostic-search.md) seçeneğini veya bir [Analiz sorgusunu](app-insights-analytics-tour.md) kullanın. Örneğin, bir Rolün kilitlenmesine ve geri dönüştürülmesine neden olan bir işlenmeyen özel durumunuz olduğunu varsayalım. Bu bilgi, Windows Olay Günlüğü’nün Uygulama kanalında görünür. Ara’yı kullanarak Windows Olay Günlüğü hatasına bakabilir ve özel duruma ilişkin tam yığın izlemesini edinebilirsiniz. Bu, sorunun kök nedenini bulmanıza yardımcı olacaktır.

![Azure tanılama araması](./media/app-insights-cloudservices/25-wad.png)

## <a name="more-telemetry"></a>Daha fazla telemetri
Aşağıdaki bölümlerde, uygulamanızın farklı boyutlarından nasıl ek telemetri toplayabileceğiniz gösterilmiştir.

## <a name="track-requests-from-worker-roles"></a>Çalışan rollerinden gelen İstekleri izleme
Web rollerinde, istek modülü otomatik olarak HTTP istekleriyle ilgili verileri toplar. Varsayılan toplama davranışını nasıl geçersiz kılabileceğinize ilişkin örnekler için bkz. [örnek MVCWebRole](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole). 

Çalışan rollerine yapılan çağrıları HTTP istekleriyle aynı yöntemle izleyerek bunların performansını yakalayabilirsiniz. Application Insights’ta İstek telemetri türü, zamanlanabilen ve bağımsız olarak başarılı ya da başarısız olabilen adlandırılmış sunucu tarafı işin bir birimini ölçer. HTTP istekleri SDK tarafından otomatik olarak yakalansa da çalışan rollerine yapılan istekleri izlemek üzere kendi kodunuzu ekleyebilirsiniz.

İstekleri raporlamak için izlenen iki örnek çalışan rolüne bakın: [WorkerRoleA](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/WorkerRoleA) ve [WorkerRoleB](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/WorkerRoleB)

## <a name="exceptions"></a>Özel durumlar
Farklı web uygulaması türlerinden işlenmeyen özel durumları nasıl toplayabileceğiniz konusunda bilgi edinmek için bkz. [Application Insights’ta Özel Durumları İzleme](app-insights-asp-net-exceptions.md).

Örnek web rolü, MVC5 ve Web API 2 denetleyicilerine sahiptir. Bu ikisinden toplanan işlenmemiş özel durumlar aşağıdaki işleyicilerle yakalanır:

* MVC5 denetleyicileri için [burada](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/App_Start/FilterConfig.cs#L12) ayarlanmış [AiHandleErrorAttribute](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Telemetry/AiHandleErrorAttribute.cs)
* Web API 2 denetleyicileri için [burada](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/App_Start/WebApiConfig.cs#L25) ayarlanmış [AiWebApiExceptionLogger](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Telemetry/AiWebApiExceptionLogger.cs)

Çalışan rolleri için özel durumları izlemenin iki yolu vardır:

* TrackException(ex)
* Application Insights izleme dinleyicisi NuGet paketini eklediyseniz, özel durumları günlüğe kaydetmek için **System.Diagnostics.Trace** öğesini kullanabilirsiniz. [Kod örneği.](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L107)

## <a name="performance-counters"></a>Performans Sayaçları
Aşağıdaki sayaçlar varsayılan olarak toplanır:

    * \Process(??APP_WIN32_PROC??)\% İşlemci Süresi
    * \Memory\Available Bytes
    * \.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec
    * \Process(??APP_WIN32_PROC??)\Private Bytes
    * \Process(??APP_WIN32_PROC??)\IO Data Bytes/sec
    * \Processor(_Total)\% Processor Time

Web rolleri için aşağıdaki sayaçlar da toplanır:

    * \ASP.NET Applications(??APP_W3SVC_PROC??)\Requests/Sec
    * \ASP.NET Applications(??APP_W3SVC_PROC??)\Request Execution Time
    * \ASP.NET Applications(??APP_W3SVC_PROC??)\Requests In Application Queue

ApplicationInsights.config dosyasını [bu örnekte gösterildiği gibi](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/ApplicationInsights.config#L14) düzenleyerek ek özel sayaçlar veya başka Windows performans sayaçları belirtebilirsiniz.

  ![Performans sayaçları](./media/app-insights-cloudservices/OLfMo2f.png)

## <a name="correlated-telemetry-for-worker-roles"></a>Çalışan Rolleri için Bağıntılı Telemetri
Başarısız veya gecikme süresi yüksek olan bir isteğe neyin neden olduğunu görebilmek, zengin bir tanılama deneyimi sağlar. Web rolleri için SDK otomatik olarak ilgili telemetri öğeleri arasında bağıntı ayarlar. Çalışan rollerinde bunu yapmak istiyorsanız, özel bir telemetri başlatıcısı kullanarak tüm telemetri öğeleri için ortak bir Operation.Id bağlam özniteliği ayarlayın. Bu sayede, gecikme süresinin veya hatanın bir bağımlılıktan mı yoksa kodunuzdan mı kaynaklandığını bir bakışta görebilirsiniz! 

Bunu yapmak için:

* [Burada](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L36) gösterildiği gibi bağıntı kimliğini bir CallContext’e ayarlayın. Bu örnekte, bağıntı kimliği olarak İstek Kimliği değerini kullanıyoruz
* Operation.Id değerini yukarıda belirlenen correlationId değerine ayarlamak için, özel bir TelemetryInitializer uygulaması ekleyin. Örnek: [ItemCorrelationTelemetryInitializer](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/Telemetry/ItemCorrelationTelemetryInitializer.cs#L13)
* Özel telemetri başlatıcısını ekleyin. Bunu ApplicationInsights.config dosyasında ya da [burada](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L233) gösterildiği gibi kodda yapabilirsiniz

İşte bu kadar! Portal deneyimi zaten tüm ilişkili telemetri öğelerini tek bakışta görmenize yardımcı olacak şekilde hazırlanmıştır:

![Bağıntılı telemetri](./media/app-insights-cloudservices/bHxuUhd.png)

## <a name="client-telemetry"></a>İstemci telemetrisi
Sayfa görüntüleme sayıları, sayfa yüklenme süreleri, betik özel durumları gibi tarayıcı tabanlı telemetri öğelerini almak ve sayfa betiklerinizde özel telemetri yazma imkanına sahip olmak için [web sayfalarınıza JavaScript SDK’sını ekleyin][client].

## <a name="availability-tests"></a>Kullanılabilirlik testleri
Uygulamanızın canlı ve duyarlı kaldığından emin olmak için [web testleri oluşturun][availability].

## <a name="display-everything-together"></a>Her şeyi birlikte görüntüleme
Sisteminizin genel bir görünümüne sahip olmak için temel izleme grafiklerini bir [panoda](app-insights-dashboards.md) birleştirebilirsiniz. Örneğin, her rolün istek ve hata sayılarını sabitleyebilirsiniz. 

Sisteminiz tarafından Stream Analytics gibi diğer Azure hizmetleri kullanılıyorsa bunların izleme grafiklerini de dahil edin. 

İstemci mobil uygulamanız varsa önemli kullanıcı işlemlerinde özel olaylar gönderilmesi için bir kod ekleyin ve bir [HockeyApp köprüsü](app-insights-hockeyapp-bridge-app.md) oluşturun. [Analiz](app-insights-analytics.md)’de olay sayılarını görüntüleyecek sorgular oluşturun ve bunları panoya sabitleyin.

## <a name="example"></a>Örnek
[Örnek](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService), bir web rolü ve iki çalışan rolüne sahip bir hizmeti izler.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Azure Cloud Services’da çalıştırma üzerine “yöntem bulunamadı” özel durumu
.NET 4.6 için mi oluşturdunuz? 4.6 sürümü Azure Cloud Services rollerinde otomatik olarak desteklenmez. Uygulamanızı çalıştırmadan önce [her role 4.6 sürümünü yükleyin](../cloud-services/cloud-services-dotnet-install-dotnet.md).

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Tanılama verilerinin Application Insights’a gönderimini yapılandırma](app-insights-azure-diagnostics.md)
* [Application Insights kaynakları oluşturmayı otomatikleştirme](app-insights-powershell.md)
* [Azure tanılamayı otomatikleştirme](app-insights-powershell-azure-diagnostics.md)
* [Azure İşlevleri](https://github.com/christopheranderson/azure-functions-app-insights-sample)

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[azure]: app-insights-azure.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[netlogs]: app-insights-asp-net-trace-logs.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md 

