---
title: "İzleme ve tanılama Azure Service Fabric ASP.NET Core hizmetler için | Microsoft Docs"
description: "İzleme ve tanılama Azure Service Fabric ASP.NET Core uygulama için nasıl ayarlanacağını öğrenin."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/14/2017
ms.author: dekapur
ms.custom: mvc
ms.openlocfilehash: 68788efffd27edf2813cf455490b651c2c7106a8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-and-diagnose-an-aspnet-core-application-on-service-fabric"></a>İzleme ve Service Fabric bir ASP.NET Core uygulama tanılama
Bu öğretici dört serinin bir parçasıdır. İzleme ve tanılama Application Insights kullanarak bir Service Fabric kümesi üzerinde çalışan bir ASP.NET Core uygulama için ayarlamak için adımları gider. Telemetri öğreticinin ilk bölümü geliştirilen uygulamasından toplarız [.NET Service Fabric uygulaması derleme](service-fabric-tutorial-create-dotnet-app.md). 

Bölümünde öğretici serisinin dört öğrenin nasıl yapılır:
> [!div class="checklist"]
> * Application Insights, uygulamanız için yapılandırma
> * Yanıt telemetri izleme HTTP tabanlı iletişim hizmetleri arasında Topla
> * Application ınsights'ta uygulama eşleme özelliğini kullanma
> * Application Insights API'si kullanarak özel olaylar ekleme

Bu öğretici serisinde öğrenin nasıl yapılır:
> [!div class="checklist"]
> * [Bir .NET Service Fabric uygulaması oluşturma](service-fabric-tutorial-create-dotnet-app.md)
> * [Uzak bir küme için uygulama dağıtma](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [CI/CD Visual Studio Team Services kullanarak yapılandırma](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * İzleme ve tanılama uygulama için ayarlama

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce:
- Bir Azure aboneliğiniz yoksa, oluşturma bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Visual Studio 2017 yükleme](https://www.visualstudio.com/) yükleyip **Azure geliştirme** ve **ASP.NET ve web geliştirme** iş yükleri.
- [Service Fabric SDK yükleme](service-fabric-get-started.md)

## <a name="download-the-voting-sample-application"></a>Oylama örnek uygulamayı indirin
Oylama örnek uygulama yapı içinde değil, [Bu öğretici seri birini Kısım](service-fabric-tutorial-create-dotnet-app.md), yükleyebilirsiniz. Bir komut penceresi veya terminal örnek uygulama depoyu yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="set-up-an-application-insights-resource"></a>Application Insights kaynağı ayarladıysanız
Application Insights Azure'nın uygulama performans yönetim platformu ve Service Fabric'ın platform uygulama izleme ve tanılama için önerilen. Application Insights kaynağı oluşturmak için gidin [Azure portal](https://portal.azure.com). Tıklatın **yeni** Azure Marketi açmak için sol gezinti menüsünde. Tıklayın **izleme + Yönetim** ve ardından **Application Insights**.

![Yeni AI kaynağı oluşturma](./media/service-fabric-tutorial-monitoring-aspnet/new-ai-resource.png)

Artık gerekli bilgileri oluşturulacak kaynağının öznitelikleri hakkında doldurun gerekecektir. Uygun bir girin *adı*, *kaynak grubu*, ve *abonelik*. Ayarlama *konumu* burada Service Fabric kümesi gelecekte dağıtmak. Bu öğreticide, biz uygulamayı yerel kümeye dağıtacak şekilde *konumu* alandır ilgisiz. *Uygulama türü* "ASP.NET web uygulaması." bırakılmalıdır 

![AI kaynak öznitelikleri](./media/service-fabric-tutorial-monitoring-aspnet/new-ai-resource-attrib.png)

Gerekli bilgileri doldurulmuş sonra tıklayın **oluşturma** - kaynak sağlamak için yaklaşık bir dakika sürer. 
<!-- When completed, navigate to the newly deployed resource, and find the "Instrumentation Key" (visible in the "Essentials" drop down section). Copy it to clipboard, since we will need it in the next step. -->

## <a name="add-application-insights-to-the-applications-services"></a>Uygulama hizmetleri için Application Insights Ekle

Visual Studio 2017 yükseltilmiş ayrıcalıklarla başlatın. Başlat menüde Visual Studio simgesine sağ tıklayıp seçerek bunu yapabilirsiniz **yönetici olarak çalıştır**. Tıklatın **dosya** > **açık** > **proje/çözüm** ve oylama uygulaması (ya da bir öğretici veya git parçası oluşturulan gidin Kopyalanan). Açık *Voting.sln* tıklatıp uygulamanın NuGet paketlerini geri yüklemek isteyip istemediğiniz sorulduğunda **Evet**.

Application Insights VotingWeb ve VotingData hizmetleri yapılandırmak için aşağıdaki adımları izleyin:
1. Hizmet adını sağ tıklatın ve **Application Insights Yapılandır...** .

    ![AI yapılandırın](./media/service-fabric-tutorial-monitoring-aspnet/configure-ai.png)

2. Tıklatın **ücretsiz Başlat**.
3. (İle de Azure aboneliğinize ayarlamanız) hesabınızda oturum açın ve Application Insights kaynağı oluşturulan aboneliği seçin. Altında kaynak bulmak *varolan Application Insights kaynağı* "Kaynak" açılır. Tıklatın **kaydetmek** hizmetiniz için Application Insights eklemek için.

    ![AI kaydetme](./media/service-fabric-tutorial-monitoring-aspnet/register-ai.png)

4. Tıklatın **son** açılır iletişim kutusu, eylem tamamlandıktan sonra.
    
Yukarıdaki adımları gerçekleştirdiğinizden emin olun **her ikisi de** Application Insights uygulaması için yapılandırmayı tamamlamak için uygulamada Hizmetleri. Aynı Application Insights kaynağı hem gelen görmeniz için hizmetlerin ve giden istekleri ve Hizmetleri arasındaki iletişim için kullanılır.

## <a name="add-the-microsoftapplicationinsightsservicefabricnative-nuget-to-the-services"></a>Hizmetlere Microsoft.ApplicationInsights.ServiceFabric.Native NuGet ekleme

Application Insights bağlı olarak senaryo kullanılabilecek iki Service Fabric belirli NuGets sahiptir. Bir Service Fabric'ın yerel Hizmetleri ve diğer kapsayıcılar ve Konuk yürütülebilir dosyaları ile birlikte kullanılır. Bu durumda, biz Microsoft.ApplicationInsights.ServiceFabric.Native NuGet taşır Hizmet bağlamı anlayış yararlanmak için kullanırsınız. Daha fazla bilgi için bkz: Application Insights SDK'sı ve Service Fabric belirli NuGets hakkında [Service Fabric için Microsoft Application Insights](https://github.com/Microsoft/ApplicationInsights-ServiceFabric/blob/develop/README.md). 

NuGet ayarlamak için adımlar şunlardır:
1. Sağ **çözüm 'Oylama'** Çözüm Gezgini ve tıklatın üstündeki **çözüm için NuGet paketlerini Yönet...** .
2. Tıklatın **Gözat** "NuGet – Çözüm" penceresi ve onay üst gezinti menüsünde **dahil et** arama çubuğunu yanındaki kutuyu.
3. Arama `Microsoft.ApplicationInsights.ServiceFabric.Native` ve uygun NuGet paketi'ı tıklatın.
4. Sağ tarafta uygulamada iki hizmet yanındaki iki onay kutularını tıklayın **VotingWeb** ve **VotingData** tıklatıp **yükleme**.
    ![AI kaydı tamamlandı](./media/service-fabric-tutorial-monitoring-aspnet/aisdk-sf-nuget.png)
5. Tıklatın **Tamam** üzerinde *değişiklikleri gözden* açılır ve kabul edin iletişim kutusu *lisans kabulünü*. Bu hizmetlere NuGet ekleme işlemini tamamlar.
6. Şimdi iki hizmetlerinde telemetri Başlatıcı ayarlamanız gerekir. Bunun için açık *VotingWeb.cs* ve *VotingData.cs*. Her ikisi de bunları için aşağıdaki iki adımları uygulayın:
    1. Bu iki eklemek *kullanarak* her deyimlerini  *\<ServiceName > .cs*:
    
    ```csharp
    using Microsoft.ApplicationInsights.Extensibility;
    using Microsoft.ApplicationInsights.ServiceFabric;
    ```
    
    2. İç içe içinde *dönmek* deyiminin *CreateServiceInstanceListeners()* veya *CreateServiceReplicaListeners()*altında *ConfigureServices*  >  *Hizmetleri*, bildirilen, iki Singleton hizmetler arasında ekleme: `.AddSingleton<ITelemetryInitializer>((serviceProvider) => FabricTelemetryInitializerExtension.CreateFabricTelemetryInitializer(serviceContext))`.
    Bu ekler *Hizmet bağlamı* telemetrinize için siz telemetrinize Application ınsights'ta kaynak daha iyi anlamak izin verme. İç içe *dönmek* deyiminde *VotingWeb.cs* aşağıdaki gibi görünmelidir:
    
    ```csharp
    return new WebHostBuilder().UseWebListener()
        .ConfigureServices(
            services => services
                .AddSingleton<StatelessServiceContext>(serviceContext)
                .AddSingleton<ITelemetryInitializer>((serviceProvider) => FabricTelemetryInitializerExtension.CreateFabricTelemetryInitializer(serviceContext))
                .AddSingleton<HttpClient>())
        .UseContentRoot(Directory.GetCurrentDirectory())
        .UseStartup<Startup>()
        .UseApplicationInsights()
        .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
        .UseUrls(url)
        .Build();
    ```

    Benzer şekilde, içinde *VotingData.cs*, olması gerekir:

    ```csharp
    return new WebHostBuilder()
        .UseKestrel()
        .ConfigureServices(
            services => services
                .AddSingleton<StatefulServiceContext>(serviceContext)
                .AddSingleton<ITelemetryInitializer>((serviceProvider) => FabricTelemetryInitializerExtension.CreateFabricTelemetryInitializer(serviceContext))
                .AddSingleton<IReliableStateManager>(this.StateManager))
        .UseContentRoot(Directory.GetCurrentDirectory())
        .UseStartup<Startup>()
        .UseApplicationInsights()
        .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.UseUniqueServiceUrl)
        .UseUrls(url)
        .Build();
    ```

Bu noktada, uygulamayı dağıtmak hazırsınız. Tıklatın **Başlat** en üstünde (veya **F5**), ve Visual Studio ve uygulama paketi, yerel küme ayarlamak ve bir uygulamayı kümeye dağıtır. 

Uygulama yaptıktan sonra dağıtma, üzerinden baş [localhost:8080](localhost:8080), burada, oylama örnek tek sayfa uygulaması görüyor olmalısınız. Bazı oluşturmak için tercih ettiğiniz birkaç farklı öğeler örnek veriler ve telemetri - desserts için oluştu için oy verin!

![AI örnek oy](./media/service-fabric-tutorial-monitoring-aspnet/vote-sample.png)

İçin çekinmeyin *kaldırmak* de tamamladığınızda oylama seçenekleri bazıları birkaç oyları ekleme.

## <a name="view-telemetry-and-the-app-map-in-application-insights"></a>Görünüm telemetri ve Application Insights uygulama eşlemesinde 

Azure portalında ve kaynak sol gezinti çubuğunda Application Insights kaynağınıza için üzerinden HEAD tıklayın **önizlemeleri** altında *yapılandırma*. Kapatma **üzerinde** *birden çok rol uygulama eşlemesi* kullanılabilir önizlemeleri listesinde.

![AI etkinleştir AppMap](./media/service-fabric-tutorial-monitoring-aspnet/ai-appmap-enable.png)

Tıklatın **genel bakış** kaynağınız giriş sayfasına geri gidin. Ardından **arama** gelen izlemelerini görmek için üst. Application Insights'ta görünmesi izlemeleri için birkaç dakika sürer. Değil herhangi bakın, bir dakika bekleyin ve isabet, bu durumda **yenileme** üstündeki düğmesi.
![AI bakın izlemeleri](./media/service-fabric-tutorial-monitoring-aspnet/ai-search.png)

Üzerinde aşağı kaydırma *arama* penceresi gösterir, Giden kutusu aldığınız tüm gelen telemetri Application Insights ile. Oylama uygulamada sürdü her bir eylem için olması gerektiğini giden PUT isteğinden *VotingWeb* (PUT oyları/Put [name]), gelen PUT isteğinden *VotingData* (PUT VoteData/Put [adı görüntülenen verileri yenileme GET istekleri çifti arkasından]). Ayrıca olacaktır localhost, HTTP için bir bağımlılık izlemesi HTTP isteklerini bunlar bu yana. İşte bir örnek için nasıl bir oy ne göreceğiniz eklenir: ![AI örnek istek izleme](./media/service-fabric-tutorial-monitoring-aspnet/sample-request.png)

Bunun hakkında daha fazla ayrıntı görüntülemek için izlemeleri birinde tıklatabilirsiniz. Yararlı bilgiler de dahil olmak üzere Application Insights tarafından sağlanan isteğiyle ilgili *yanıt süresi* ve *istek URL'si*. Service Fabric belirli NuGet eklenmiş olduğundan, ayrıca, aynı zamanda veri bağlamında uygulamanız bir Service Fabric kümesi ile ilgili karşılaşırsınız *özel veri* bölümüne bakın. Gördüğünüz bu hizmet bağlamı içerir, böylece *PartitionID* ve *ReplicaID* isteğinin ve daha iyi kaynağı sorunları, uygulamanızın hatalarını tanılamada yerelleştirme.

![AI İzleme Ayrıntıları](./media/service-fabric-tutorial-monitoring-aspnet/trace-details.png)

Ayrıca, beri biz etkin uygulama harita *genel bakış* tıklayarak sayfasında, **uygulama harita** simgesi gösterir, her iki hizmetlerinizi bağlı.

![AI İzleme Ayrıntıları](./media/service-fabric-tutorial-monitoring-aspnet/app-map.png)

Özellikle birlikte çalışan birden çok farklı hizmet ekleme başlangıç olarak uygulama harita uygulama topolojinizi daha iyi anlamanıza yardımcı olabilir. Ayrıca temel veri isteği başarı oranlarına sağlar ve burada şeyler yanlış ilerlemiş anlamak için başarısız istek tanılamanıza yardımcı olabilir. Uygulama eşlemesini kullanma hakkında daha fazla bilgi için bkz: [Application Insights uygulama eşlemesinde](../application-insights/app-insights-app-map.md).

## <a name="add-custom-instrumentation-to-your-application"></a>Özel İzleme uygulamanıza ekleyin

Çok sayıda telemetri kutunun dışında Application Insights sağlar ancak daha fazla özel araçları eklemek isteyebilirsiniz. Bu, iş gereksinimlerinize veya şeyler uygulamanızda ters gittiğinde tanılama artırmak için temel alabilir. Application Insights sahip bir API özel olayları ve daha fazla bilgi edinebilirsiniz ölçümleri alma [burada](../application-insights/app-insights-api-custom-events-metrics.md).

Bazı özel olaylar ekleyelim *VoteDataController.cs* (altında *VotingData* > *denetleyicileri*) oyları yükleniyor ne zaman eklenir ve sildi temel alınan *votesDictionary*. 
1. Ekleme `using Microsoft.ApplicationInsights;` sonunda diğer ifadeler kullanma.
2. Yeni bir bildirme *TelemetryClient* oluşturulmasını altında sınıfı başlangıcında *IReliableStateManager*: `private TelemetryClient telemetry = new TelemetryClient();`.
3. İçinde *Put()* işlev, bir oy onaylar bir olay eklendi ekleyin. Ekleme `telemetry.TrackEvent($"Added a vote for {name}");` işlem tamamlandıktan sonra önce dönüş sağ *OkResult* deyimi.
4. İçinde *Delete()*, var olan bir "IF/başka" tabanlı koşuluyla *votesDictionary* oyların belirli bir oylama seçenek içerir. 
    1. Bir oy silinmesini onaylar bir olay eklemek *varsa* deyimi, sonra *tx.CommitAsync() await*:`telemetry.TrackEvent($"Deleted votes for {name}");`
    2. Silme işlemini gerçekleşmedi olduğunu göstermek için bir olay eklemek *başka* deyim dönüş deyimi önce:`telemetry.TrackEvent($"Unable to delete votes for {name}, voting option not found");`

Örneği ne, *Put()* ve *Delete()* işlevleri göründüğü olayları ekledikten sonra:

```csharp
// PUT api/VoteData/name
[HttpPut("{name}")]
public async Task<IActionResult> Put(string name)
{
    var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

    using (ITransaction tx = this.stateManager.CreateTransaction())
    {
        await votesDictionary.AddOrUpdateAsync(tx, name, 1, (key, oldvalue) => oldvalue + 1);
        await tx.CommitAsync();
    }

    telemetry.TrackEvent($"Added a vote for {name}");
    return new OkResult();
}

// DELETE api/VoteData/name
[HttpDelete("{name}")]
public async Task<IActionResult> Delete(string name)
{
    var votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

    using (ITransaction tx = this.stateManager.CreateTransaction())
    {
        if (await votesDictionary.ContainsKeyAsync(tx, name))
        {
            await votesDictionary.TryRemoveAsync(tx, name);
            await tx.CommitAsync();
            telemetry.TrackEvent($"Deleted votes for {name}");
            return new OkResult();
        }
        else
        {
            telemetry.TrackEvent($"Unable to delete votes for {name}, voting option not found");
            return new NotFoundResult();
        }
    }
}
```

İşiniz bittiğinde bu değişiklikleri yaptıktan **Başlat** uygulama şekilde oluşturur ve en son sürümünü dağıtır. Uygulama yaptıktan sonra dağıtma, üzerinden baş [localhost:8080](localhost:8080), ekleme ve bazı oylama seçenekleri silin. Sonra izleri son çalıştırma için (daha önce izlemeleri 1-2 Application Insights'ta gösterilmesini min sürebilir gibi) görmek için Application Insights kaynağınıza için geri dönün. Eklenen ve silinen tüm oyları için bir "özel olay * yanı sıra tüm yanıt telemetri. görmelisiniz 

![özel olaylar](./media/service-fabric-tutorial-monitoring-aspnet/custom-events.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:
> [!div class="checklist"]
> * Application Insights, uygulamanız için yapılandırma
> * Yanıt telemetri izleme HTTP tabanlı iletişim hizmetleri arasında Topla
> * Application ınsights'ta uygulama eşleme özelliğini kullanma
> * Application Insights API'si kullanarak özel olaylar ekleme

ASP.NET uygulamanız için izleme ve tanılama ayarlama tamamladığınıza göre aşağıdakileri deneyin:
- [İzleme ve tanılama Service Fabric, keşfedin](service-fabric-diagnostics-overview.md)
- [Application Insights ile Service Fabric olay analizi](service-fabric-diagnostics-event-analysis-appinsights.md)
- Application Insights hakkında daha fazla bilgi için bkz: [uygulama Öngörüler belgeleri](https://docs.microsoft.com/en-us/azure/application-insights/)
