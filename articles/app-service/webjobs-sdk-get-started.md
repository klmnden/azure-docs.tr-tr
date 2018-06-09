---
title: Azure WebJobs SDK Kullanmaya Başlarken
description: Olay kaynaklı arka plan işlemesi için WebJobs SDK'sı giriş. Azure ve üçüncü taraf hizmetleri verilerine erişmek öğrenin.
services: app-service\web, storage
documentationcenter: .net
author: tdykstra
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/27/2018
ms.author: tdykstra
ms.openlocfilehash: d0afc8b6f8e0b7ef73e5d1c3cbabf2e1542f47f4
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234536"
---
# <a name="get-started-with-the-azure-webjobs-sdk-for-event-driven-background-processing"></a>Olay denetimli arka plan işlemesi için Azure WebJobs SDK'sını kullanmaya başlama

Bu makalede, bir Azure WebJobs SDK projesi oluşturun, yerel olarak çalıştırın ve Azure App Service'e dağıtma gösterilmektedir.

Yönergeler için olan [Visual Studio 2017](https://www.visualstudio.com/vs/), ancak diğer araçları ile aynı görevleri gibi gerçekleştirilebilir [Visual Studio Code](https://code.visualstudio.com/).

## <a name="what-is-the-azure-webjobs-sdk"></a>Azure WebJobs SDK nedir?

Azure Web işleri SDK'si, Azure Hizmetleri verilere eriştiğinde arka plan işleme kod yazma görevini basitleştirir bir çerçevedir. SDK bir sıraya eklenen yeni bir ileti gibi bir işlev tetiklemesi gereken olayları belirten bir bildirim temelli söz dizimi özellikleri. Benzer Tanımlayıcı Sözdizimi okuma ve bir işlev tetiklenen sonra veri yazma denetler. Bu sistem Tetikleyicileri ve bağlamaları Azure ve üçüncü taraf hizmetlerine erişme ile ilişkili alt düzey kodlama görevlerin çoğunu ilgilenir.

### <a name="functions-triggers-and-bindings"></a>İşlevler, Tetikleyicileri ve bağlamaları

Bir veya daha fazla Web işleri SDK'si proje tanımlar *işlevler*. Bir işlev yöntemi imzası tetikleyici özniteliğine sahip bir yöntemdir. Bir işlevi çağırmak için koşullar Tetikleyicileri ve bağlamaları ne okumak ve yazmak belirtin. Örneğin, aşağıdaki işlev tetikleyici özniteliğinde bir kuyruk iletisi görünür her işlev çağrısı için çalışma zamanı söyler `items` sırası. `Blob` Özniteliği söyleyen bir blob'a okumak için kuyruk iletisini kullanılacak çalışma zamanı *iş öğeleri* kapsayıcı. Kuyruk iletisini içeriğini &mdash; sağlanan `queueTrigger` parametresi &mdash; blob adıdır.

```cs
public static void Run(
    [QueueTrigger("items")] string myQueueItem,
    [Blob("workitems/{queueTrigger}", FileAccess.Read)] Stream myBlob,
    TraceWriter log)
{
    log.Info($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}
```

### <a name="versions-2x-and-3x"></a>Sürümleri 2.x ve 3.x

Yönergeleri 3.x (önizlemede) için farklı nedir hakkında notlar ile bir Web işleri SDK'si sürüm 2.x projesi oluşturmak nasıl bildirir. 3.x tarafından sunulan ana değişiklik .NET Core .NET Framework yerine kullanılır.

### <a name="azure-functions"></a>Azure İşlevleri

[Azure işlevleri](../azure-functions/functions-overview.md) WebJobs SDK temel alır ve WebJobs SDK doğrudan kullanmanız gerekmez zamanki bir seçenektir. Azure işlevleri 1.x kullanan Web işleri SDK'si 2.x. Daha fazla bilgi için bkz: [Azure işlevleri ve WebJobs SDK arasında karşılaştırma](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md#compare-functions-and-webjobs).

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, sahip olduğunuz varsayılmaktadır [bir Azure hesabı](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) ile deneyimi [Azure uygulama hizmetinde uygulamalar](app-service-web-overview.md). Bu makaledeki adımları tamamlamak için:

* [Visual Studio 2017 yükleme](https://docs.microsoft.com/visualstudio/install/) ile **Azure geliştirme** iş yükü. Zaten Visual Studio olan ancak iş yükü yok, iş yükü seçerek eklemek **Araçlar > alma araçları ve özelliklerinin**.
* [Bir App Service uygulaması oluşturma](app-service-web-get-started-dotnet-framework.md). Zaten bir Webjob'un dağıtabileceğiniz varsa, yeni bir tane oluşturmak yerine bunu kullanabilirsiniz.

## <a name="create-a-project"></a>Proje oluşturma

1. Visual Studio'da seçin **Dosya > Yeni proje**.

2. Seçin **Windows Klasik Masaüstü > konsol uygulaması (.NET Framework)**.

   3.x projesi oluşturmak için seçin **.NET Core > konsol uygulaması (.NET Core)**.

3. Proje adı *WebJobsSDKSample*ve ardından **Tamam**.

   ![Yeni Proje iletişim kutusu](./media/webjobs-sdk-get-started/new-project.png)

## <a name="add-webjobs-nuget-package"></a>Web işleri NuGet paketi ekleme

1. NuGet paketi en son kararlı 2.x sürümünü yükleme `Microsoft.Azure.WebJobs`. (Web işleri SDK'si için 3.x, en son 3.x sürümünü seçin.)
 
   Burada **Paket Yöneticisi Konsolu** sürüm 2.2.0 komutunu:

   ```powershell
   Install-Package Microsoft.Azure.WebJobs -version 2.2.0
   ``` 

## <a name="create-the-jobhost"></a>JobHost oluşturma

`JobHost` Nesnesidir işlevler için çalışma zamanı kapsayıcısı: tetikleyiciler ve çağrıları işlevleri için dinler. 

1. İçinde *Program.cs*, ekleme bir `using` deyimi:

   ```cs
   using Microsoft.Azure.WebJobs;
   ```

1. `Main` yöntemini aşağıdaki kod ile değiştirin:

   ```cs
   static void Main()
   {
       var config = new JobHostConfiguration();
       var host = new JobHost(config);
       host.RunAndBlock();
   }
   ```

## <a name="enable-console-logging"></a>Konsol günlük kaydını etkinleştir

Web işleri SDK'si projesinde günlüğü için birkaç seçenek vardır. Bir tavsiye ederiz [ASP.NET Core için geliştirilmiş günlük framework](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging). Bu çerçeve, daha iyi performans ve depolama medyası ve filtreleme daha fazla esneklik sunar. 

Bu bölümde, yeni framework kullanan konsol günlüğünü ayarlama.

1. Aşağıdaki NuGet paketleri en son kararlı sürümü yükleyin:

   * `Microsoft.Extensions.Logging` -Günlük kaydı çerçevesi.
   * `Microsoft.Extensions.Logging.Console` -Konsol *sağlayıcı*. Bir sağlayıcı belirli bir hedefe günlükleri, bu durumda konsola gönderir. 
 
   Burada **Paket Yöneticisi Konsolu** komutları sürüm 2.0.1 için:

   ```powershell
   Install-Package Microsoft.Extensions.Logging -version 2.0.1
   ``` 

   ```powershell
   Install-Package Microsoft.Extensions.Logging.Console -version 2.0.1
   ``` 

2. İçinde *Program.cs*, ekleme bir `using` deyimi:

   ```cs
   using Microsoft.Extensions.Logging;
   ```

3. İçinde `Main` yöntemi, güncelleştirme için kodu ekleyin `JobHostConfiguration` oluşturmadan önce `JobHost`:
 
   ```
   config.DashboardConnectionString = "";
   var loggerFactory = new LoggerFactory();
   config.LoggerFactory = loggerFactory
       .AddConsole();
   ```

   Bu kod, aşağıdaki değişiklikleri yapar:

   * Devre dışı bırakır [Pano günlük](https://github.com/Azure/azure-webjobs-sdk/wiki/Queues#logs). Pano izleme aracı eski olduğundan ve Pano günlük yüksek verimlilik üretim senaryoları için önerilmez.
   * Varsayılan konsol sağlayıcısıyla ekler [filtreleme](webjobs-sdk-how-to.md#log-filtering). 

   `Main` Yöntemi şimdi aşağıdaki gibi görünür:

   ```
   var config = new JobHostConfiguration();
   config.DashboardConnectionString = "";
   var loggerFactory = new LoggerFactory();
   config.LoggerFactory = loggerFactory
       .AddConsole();
   var host = new JobHost(config);
   host.RunAndBlock();
   ```
   
## <a name="create-a-function"></a>İşlev oluşturma

1. Oluşturma *Functions.cs* proje klasöründe ve şablonu kodu bu kod ile değiştirin:

   ```cs
   using Microsoft.Azure.WebJobs;
   using Microsoft.Azure.WebJobs.Host;
   using Microsoft.Extensions.Logging;

   namespace WebJobsSDKSample
   {
       public class Functions
       {
           public static void ProcessQueueMessage([QueueTrigger("queue")] string message, ILogger logger)
           {
               logger.LogInformation(message);
           }
       }
   }
   ```

   `QueueTrigger` Özniteliği söyler adlı bir Azure depolama kuyruğu üzerinde yeni bir ileti yazılırken bu işlevi çağırmak için çalışma zamanı `queue`. Kuyruk iletinin içeriğini yöntemi kodunda sağlanır `message` parametresi. Yönteminin gövdesi, tetikleyici verileri işlemek burada ' dir. Bu örnekte, kod yalnızca iletiyi günlüğe kaydeder.

   `message` Parametresi bir dize olması gerekmez. Ayrıca bir JSON nesnesi bir bayt dizisi bağlayabilirsiniz veya [CloudQueueMessage](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage) nesnesi. [Sıra tetikleyici kullanımını görmek](../azure-functions/functions-bindings-storage-queue.md#trigger---usage). Bağlama türlerinin (örneğin, kuyruklar, BLOB'lar veya tabloları) bağlayabilirsiniz parametre türleri farklı bir dizi vardır.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Yerel olarak çalışan Azure Storage öykünücüsü WebJobs SDK gereken tüm özelliklere sahip değil. Bu nedenle bu bölümde bir depolama hesabı oluşturmak ve kullanmak için proje yapılandırın.

1. Açık **Sunucu Gezgini** ve Azure oturumu açın. Sağ **Azure** düğümünü ve ardından **Microsoft Azure aboneliğine bağlanma**.

   ![Azure'da oturum açma](./media/webjobs-sdk-get-started/sign-in.png)

1. Altında **Azure** düğümünde **Sunucu Gezgini**, sağ **depolama**ve ardından **oluşturma depolama hesabı**.

   ![Depolama hesabı menü oluşturma](./media/webjobs-sdk-get-started/create-storage-account-menu.png)

2. İçinde **depolama hesabı oluştur** iletişim kutusunda, depolama hesabı için benzersiz bir ad girin.

3. Aynı seçin **bölge** uygulama hizmeti uygulamanızı oluşturulan veya bir bölge için kapatın.

1. **Oluştur**’u seçin.

   ![Depolama hesabı oluşturma](./media/webjobs-sdk-get-started/create-storage-account.png)

1. Altında **depolama** düğümünde **Sunucu Gezgini**, yeni depolama hesabı seçin. İçinde **özellikleri** penceresinde, üç nokta seçin (**...** ) sağ tarafındaki **bağlantı dizesi** değeri alanı.

   ![Bağlantı dizesi üç nokta](./media/webjobs-sdk-get-started/conn-string-ellipsis.png)

2. Bağlantı dizesini kopyalayın ve bu değer herhangi bir yerde yeniden kullanıma kopyalayabilirsiniz olduğunu kaydedin.

   ![Bağlantı dizesini kopyalayın](./media/webjobs-sdk-get-started/copy-key.png)

## <a name="configure-storage-for-running-locally"></a>Yerel olarak çalıştırmak için depolama yapılandırma

WebJobs SDK uygulama ayarları koleksiyondaki depolama bağlantı dizesi arar. Yerel olarak çalıştırdığınızda, bu değeri arar *App.config* dosya ya da ortam değişkenleri.

1. Aşağıdaki XML eklemek *App.config* açtıktan hemen sonra dosya `<configuration>` etiketi.

   ```xml
   <connectionStrings>
     <add name="AzureWebJobsStorage" connectionString="{storage connection string}" />
   </connectionStrings>
   ```

2. Değiştir *{depolama bağlantı dizesi}* daha önce kopyaladığınız bağlantı dizesine sahip.

   Daha sonra Azure App Service uygulaması yapılandırdığınızda, bağlantı dizesi yeniden kullanacaksınız.

## <a name="test-locally"></a>Yerel olarak test etme

Bu bölümde, projeyi yerel olarak çalıştırın ve bir kuyruk iletisi oluşturarak Tetik işlevi oluşturun.

1. Projeyi çalıştırmak için CTRL + F5 tuşuna basın.

   Konsol çalışma işlevinizde bulundu ve onu tetiklemek bir kuyruk iletisi bekleyen gösterir.

   ```console
   Found the following functions:
   WebJobsSDKSample.Functions.ProcessQueueMessage
   info: Host.Startup[0]
         Found the following functions:
         WebJobsSDKSample.Functions.ProcessQueueMessage
   Job host started
   info: Host.Startup[0]
         Job host started
   ```

   Bir uyarı iletisi görebilirsiniz bir `ServicePointManager` ayarı. Bu proje ile yapılması test etmek için bu uyarıyı yoksayabilirsiniz. Uyarı hakkında daha fazla bilgi için bkz: [WebJobs SDK kullanmayı](webjobs-sdk-how-to.md#jobhost-servicepointmanager-settings).

2. Konsol penceresini kapatın.

1. İçinde **Sunucu Gezgini**, yeni depolama hesabınız için düğümünü genişletin ve ardından sağ **sıraları**. 

2. Seçin **kuyruk oluşturma**. 

3. Girin *sıra* sıra ve ardından adı olarak **Tamam**.

   ![Kuyruk oluştur](./media/webjobs-sdk-get-started/create-queue.png)

4. Yeni sıra düğümünü sağ tıklatın ve ardından **sırayı görüntüle**.

5. Seçin **ekleme ileti** simgesi.

   ![Kuyruk oluştur](./media/webjobs-sdk-get-started/create-queue-message.png)

6. İçinde **ekleme ileti** iletişim kutusunda, girin *Merhaba Dünya!* olarak **ileti metni**ve ardından **Tamam**.

   ![Kuyruk oluştur](./media/webjobs-sdk-get-started/hello-world-text.png)

7. Projeyi tekrar çalıştırın.

   Kullandığınız olduğundan `QueueTrigger` özniteliğini `ProcessQueueMessage` işlevi, WeJobs SDK çalışma zamanı dinleyen bir kuyruk iletisi başladığında. Sıranın adlı yeni bir kuyruk iletisi bulduğu *sıra* ve işlevi çağırır.

   Nedeniyle [üstel geri alma yoklama sıra](../azure-functions/functions-bindings-storage-queue.md#trigger---polling-algorithm), iletiyi bulun ve işlevin çalıştırılabilmesi çalışma zamanı için 2 dakika olarak uzun sürebilir. Bu bekleme süresi çalıştırarak azaltılabilir [geliştirme modunu](webjobs-sdk-how-to.md#jobhost-development-settings).

  Konsol çıktısı şu şekildedir:

   ```console
   Found the following functions:
   WebJobsSDKSample.Functions.ProcessQueueMessage
   info: Host.Startup[0]
         Found the following functions:
         WebJobsSDKSample.Functions.ProcessQueueMessage
   Job host started
   info: Host.Startup[0]
         Job host started
   Executing 'Functions.ProcessQueueMessage' (Reason='New queue message detected on 'queue'.', Id=ebcb275d-0d7c-4293-a1af-93e0804b9e49)
   info: Function[0]
         Hello World!
   info: Host.Results[0]
         Executed 'Functions.ProcessQueueMessage' (Succeeded, Id=ebcb275d-0d7c-4293-a1af-93e0804b9e49)
   Executed 'Functions.ProcessQueueMessage' (Succeeded, Id=ebcb275d-0d7c-4293-a1af-93e0804b9e49)
   ```

8. Konsol penceresini kapatın.

## <a name="add-application-insights-logging"></a>Application Insights günlüğü ekleme

Projeyi Azure içinde çalıştığında, konsol çıktısı görüntüleyerek işlevi yürütme izleyemez. Tavsiye ederiz izleme çözümü [Application Insights](../application-insights/app-insights-overview.md). Daha fazla bilgi için bkz: [İzleyici Azure işlevleri](../azure-functions/functions-monitoring.md).

Bu bölümde, Azure'a dağıtmadan önce Application Insights günlük ayarlama için aşağıdaki görevleri yapın:

* Bir uygulama hizmeti uygulama ve çalışmak için Application Insights örneği olduğundan emin olun.
* Application Insights örneği ve daha önce oluşturduğunuz depolama hesabı kullanmak için App Service uygulaması yapılandırın.
* Proje için Application Insights için günlük ayarlayın.

### <a name="create-app-service-app-and-application-insights-instance"></a>App Service uygulaması ve Application Insights örneği oluşturma

1. Kullanabileceğiniz, bir uygulama hizmeti uygulaması zaten yoksa [oluşturmak](app-service-web-get-started-dotnet-framework.md).

2. Kullanabileceğiniz, Application Insights kaynağı zaten yoksa [oluşturmak](../application-insights/app-insights-create-new-resource.md). Ayarlama **uygulama türü** için **genel**ve bölümlerde atla **izleme anahtarını kopyalama**.

3. Kullanmak istediğiniz bir Application Insights kaynağı zaten varsa [izleme anahtarını kopyalama](../application-insights/app-insights-create-new-resource.md#copy-the-instrumentation-key).

### <a name="configure-app-settings"></a>Uygulama ayarlarını yapılandırma 

1. İçinde **Sunucu Gezgini**, genişletin **uygulama hizmeti** düğümü altında **Azure**.

1. Uygulama hizmeti uygulamanızı bulunduğu kaynak grubunu genişletin ve ardından uygulama hizmeti uygulamanızı sağ tıklatın.

3. Seçin **görünüm ayarları**.

4. İçinde **bağlantı dizeleri** kutusunda, aşağıdaki girişi ekleyin.

   |Ad  |Bağlantı dizesi  |Veritabanı Türü|
   |---------|---------|------|
   |AzureWebJobsStorage | {daha önce kopyaladığınız depolama bağlantı dizesi}|Özel|
   
6. Varsa **uygulama ayarları** kutusunu anahtar, daha önce kopyaladığınız bir ekleme Application Insights araçları yok. (İzleme anahtarı zaten vardır, App Service uygulaması nasıl oluşturulacağını bağlı olabilir.)

   |Ad  |Değer  |
   |---------|---------|
   |APPINSIGHTS_INSTRUMENTATIONKEY | {İzleme anahtarını} |

2. Değiştir *{izleme anahtarını}* kullanmakta olduğunuz Application Insights kaynağı araçları anahtarla.

2. **Kaydet**’i seçin.

1. Aşağıdaki XML eklemek *App.config* bağlantı dizeleri koleksiyonu hemen sonra dosya.

   ```xml
   <appSettings>
     <add key="APPINSIGHTS_INSTRUMENTATIONKEY" value="{instrumentation key}" />
   </appSettings>
   ```

2. Değiştir *{izleme anahtarını}* kullanmakta olduğunuz Application Insights kaynağı araçları anahtarla.

   Bu verilerde ekleme *App.config* dosya projeyi yerel olarak çalıştırdığınızda test Application Insights bağlantısı olanak sağlar. 

3. Yaptığınız değişiklikleri kaydedin.

### <a name="add-application-insights-logging-provider"></a>Application Insights oturum açma sağlayıcısı Ekle

1. Application Insights oturum açma sağlayıcısı için NuGet paketi en son kararlı 2.x sürümünü yükleyin: `Microsoft.Azure.WebJobs.Logging.ApplicationInsights`. (Web işleri SDK'si için 3.x, paketin en son 3.x sürümünü seçin.)

   Burada **Paket Yöneticisi Konsolu** sürüm 2.2.0 komutunu:

   ```powershell
   Install-Package Microsoft.Azure.WebJobs.Logging.ApplicationInsights -version 2.2.0
   ``` 

1. .NET configuration manager için NuGet paketi en son kararlı 4.x sürümünü yükleyin: `System.Configuration.ConfigurationManager`.

   Burada **Paket Yöneticisi Konsolu** sürüm 4.4.1 komutunu:

   ```powershell
   Install-Package System.Configuration.ConfigurationManager -version 4.4.1
   ``` 

2. Açık *Program.cs* ve ekleme bir `using` deyimi configuration manager için:

   ```csharp
   using System.Configuration;
   ```

2. Kodla `Main` aşağıdaki kod ile yöntemi:

   ```csharp
   using (var loggerFactory = new LoggerFactory())
   {
       var config = new JobHostConfiguration();
       var instrumentationKey =
           ConfigurationManager.AppSettings["APPINSIGHTS_INSTRUMENTATIONKEY"];
       config.DashboardConnectionString = "";
       config.LoggerFactory = loggerFactory
           .AddApplicationInsights(instrumentationKey, null)
           .AddConsole();
       var host = new JobHost(config);
       host.RunAndBlock();
   }
   ```

   Bu kod, aşağıdaki değişiklikleri yapar:

   * Varsayılan bir Application Insights günlüğü sağlayıcısından ekler [filtreleme](webjobs-sdk-how-to.md#log-filtering); yerel olarak çalıştırırken tüm bilgiler ve daha yüksek düzey günlükleri artık hem Konsol hem de Application Insights için geçer. 
   * Koyar `LoggerFactory` nesnesinde bir `using` konak çıktığında, günlük çıktısı Temizlenen emin olmak için blok. 

## <a name="test-application-insights-logging"></a>Test Application Insights günlüğe kaydetme

Bu bölümde, yerel olarak yeniden konsol konusunda da Application Insights'a, günlük verilerini şimdi gittiği doğrulamak için çalıştırın.

1. Kullanım **Sunucu Gezgini** bir kuyruk iletisi oluşturmak için aynı şekilde yaptığınız [önceki](#trigger-the-function), girin dışında *Hello App Insights!* ileti metni.

1. Projeyi çalıştırın.

   WebJobs SDK kuyruk iletisini işler ve konsol penceresinde günlüklerine bakın.

1. Konsol penceresini kapatın.

1. Açık [Azure portal](https://portal.azure.com/)ve Application Insights kaynağınıza gidin.

2. Seçin **arama**.

   ![Ara'yı seçin](./media/webjobs-sdk-get-started/select-search.png)

1. Görmüyorsanız *Hello App Insights!* ileti, select **yenileme** birkaç dakika için düzenli aralıklarla. (Günlükleri temizlemek Application Insights istemcisi için biraz zaman alır çünkü işler hemen günlükleri görünmüyor.)

   ![Application Insights günlükleri](./media/webjobs-sdk-get-started/logs-in-ai.png)

3. Konsol penceresini kapatın.

## <a name="deploy-as-a-webjob"></a>Bir Web işi dağıtma

Bu bölümde bir Web işi projeyi dağıtın. Bir App Service uygulamasına dağıtmak, [daha önce oluşturduğunuz](#create-app-service-app-and-application-insights-instance). Azure üzerinde çalıştığı sırada kodunuzu test etmek için bir kuyruk iletisi oluşturarak bir işlev çağrısını tetiklemek.

1. İçinde **Çözüm Gezgini**projeye sağ tıklayın ve ardından **Azure Web işi olarak Yayımla**.

1. İçinde **Azure Web işine Ekle** iletişim kutusunda **Tamam**.

   ![Azure Web işi ekleme](./media/webjobs-sdk-get-started/add-azure-webjob.png)

   Visual Studio Web işi yayımlama NuGet paketi otomatik olarak yükler.

1. İçinde **profil** adımında **Yayımla** seçin **Microsoft Azure App Service**.

   ![Yayımlama iletişim kutusu](./media/webjobs-sdk-get-started/publish-dialog.png)

1. İçinde **uygulama hizmeti** iletişim kutusunda **kaynak grubunuz > uygulama hizmeti uygulamanızı**ve ardından **Tamam**.

   ![App Service iletişim](./media/webjobs-sdk-get-started/app-service-dialog.png)

1. İçinde **bağlantı** sihirbazın, select **Yayımla**.

## <a name="trigger-the-function-in-azure"></a>Tetikleyici Azure işlevi

1. Değil çalıştırdığınız yerel olarak emin olun (konsol penceresi hala açıksa kapatın). Aksi takdirde yerel örneği oluşturduğunuz herhangi bir kuyruk iletisi işleyemedi ilk olabilir.

1. Kullanım **Sunucu Gezgini** bir kuyruk iletisi oluşturmak için aynı şekilde yaptığınız [önceki](#trigger-the-function), girin dışında *Hello Azure!*.

7. Yenileme **sıra** sayfa Visual Studio ve yeni ileti Azure App Service içinde çalışan işlevi işlendiği çünkü kayboldu.

   > [!TIP]
   > Azure'da test ederken kullanmak [geliştirme modunu](webjobs-sdk-how-to.md#jobhost-development-settings) sıra Tetik işlevi hemen başlatıldığından emin olun ve nedeniyle gecikmelerden kaçınmak için [üstel geri alma yoklama sıra](../azure-functions/functions-bindings-storage-queue.md#trigger---polling-algorithm).

### <a name="view-logs-in-application-insights"></a>Application ınsights'ta günlüklerini görüntüle

1. Açık [Azure portal](https://portal.azure.com/)ve Application Insights kaynağınıza gidin.

2. Seçin **arama**.

1. Görmüyorsanız *Hello Azure!* ileti, select **yenileme** birkaç dakika için düzenli aralıklarla.

   WebJob içinde çalışan işlevi günlüklerinden gördüğünüz dahil olmak üzere *Hello Azure!* önceki bölümde girilen metin.

## <a name="add-an-input-binding"></a>Bir giriş bağlaması ekleme

Giriş bağlamaları veri okuyan kod basitleştirin. Bu örnek için kuyruk iletisini blob adı olacaktır ve bulmak ve Azure storage'da bir blob okumak için blob adı kullanacaksınız.

1. İçinde *Functions.cs*, yerine `ProcessQueueMessage` aşağıdaki kod ile yöntemi:

   ```cs
   public static void ProcessQueueMessage(
       [QueueTrigger("queue")] string message,
       [Blob("container/{queueTrigger}", FileAccess.Read)] Stream myBlob,
       ILogger logger)
   {
       logger.LogInformation($"Blob name:{message} \n Size: {myBlob.Length} bytes");
   }
   ```

   Bu kodda, `queueTrigger` olan bir [ifade bağlama](../azure-functions/functions-triggers-bindings.md#binding-expressions-and-patterns), yani çalışma zamanında farklı bir değer çözümler.  Çalışma zamanında, kuyruk iletinin içeriğini içeriyor.

2. Ekleme bir `using`:

   ```cs
   using System.IO;
   ```

3. Depolama hesabınızda blob kapsayıcı oluşturun.

   a. İçinde **Sunucu Gezgini**, depolama hesabınız için düğümünü genişletin, sağ **BLOB'lar**ve ardından **Blob kapsayıcısı oluşturmak**.

   b. İçinde **Blob kapsayıcısı oluşturmak** iletişim kutusunda, girin *kapsayıcı* kapsayıcı adı ve ardından olarak **Tamam**.

4. Karşıya yükleme *Program.cs* dosya blob kapsayıcısı. (Bu dosya burada kullanılır; Örneğin, herhangi bir metin dosyasını karşıya yükleyin ve dosya adında bir kuyruk iletisi oluşturun.)

   a. İçinde **Sunucu Gezgini**, az önce oluşturduğunuz kapsayıcısı için düğüme çift tıklayın.

   b. İçinde **kapsayıcı** penceresinde, seçin **karşıya** düğmesi.

   ![BLOB Karşıya Yükle düğmesi](./media/webjobs-sdk-get-started/blob-upload-button.png)

   c. Bulmak ve seçmek *Program.cs*ve ardından **Tamam**.

5. Bir kuyruk iletisi daha önce oluşturduğunuz kuyruk oluşturma ile *Program.cs* ileti metni olarak.

   ![Kuyruk iletisi Program.cs](./media/webjobs-sdk-get-started/queue-msg-program-cs.png)

6. Projeyi çalıştırın.

   Kuyruk iletisini sonra blob okur ve uzunluğu günlüklerini işlevi tetikler. Konsol çıktısı şu şekildedir:

   ```console
   Found the following functions:
   ConsoleApp1.Functions.ProcessQueueMessage
   Job host started
   Executing 'Functions.ProcessQueueMessage' (Reason='New queue message detected on 'queue'.', Id=5a2ac479-de13-4f41-aae9-1361f291ff88)
   Blob name:Program.cs
   Size: 532 bytes
   Executed 'Functions.ProcessQueueMessage' (Succeeded, Id=5a2ac479-de13-4f41-aae9-1361f291ff88)
   ```

## <a name="add-an-output-binding"></a>Çıktı bağlaması ekleme

Çıktı bağlamaları veri yazan kodu basitleştirin. Bu örnek blob boyutuna günlük kaydı yerine bir kopyasını yazarak öncekinin değiştirir.

1. `ProcessQueueMessage` yöntemini aşağıdaki kod ile değiştirin:

   ```cs
   public static void ProcessQueueMessage(
       [QueueTrigger("queue")] string message,
       [Blob("container/{queueTrigger}", FileAccess.Read)] Stream myBlob,
       [Blob("container/copy-{queueTrigger}", FileAccess.Write)] Stream outputBlob,
       ILogger logger)
   {
       logger.LogInformation($"Blob name:{message} \n Size: {myBlob.Length} bytes");
       myBlob.CopyTo(outputBlob);
   }
   ```

5. Başka bir kuyruk iletisi *Program.cs* ileti metni olarak.

6. Projeyi çalıştırın.

   Kuyruk iletisini sonra blob okur, uzunluğu günlüğe kaydeder ve yeni blob oluşturur işlevi tetikler. Konsol çıktısı, aynıdır ancak zaman blob kapsayıcı penceresine gidin ve seçin **yenileme**, adlı yeni bir blob gördüğünüz *kopyalama Program.cs.*

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuz, oluşturmak, çalıştırmak ve WebJobs SDK projesi dağıtma göstermiştir.

Her şeyi içeren bir Web işleri SDK'si projeye gider Göster, yönergeleri, sıfırdan bir proje oluşturmanız gerekirdi. Sonraki projenizi oluşturduğunuzda, ancak kullanmayı **Azure WebJob** şablonunda **bulut** kategorisi. Bu şablon, NuGet paketlerini ve örnek kod daha önce ayarlamış bir proje oluşturur. Not örnek kodunu yeni günlük framework kullanmak için değiştirilmesi gerekebilir.

Daha fazla bilgi için bkz: [WebJobs SDK kullanmayı](webjobs-sdk-how-to.md).
