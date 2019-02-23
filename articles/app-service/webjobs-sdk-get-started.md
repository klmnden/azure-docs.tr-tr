---
title: WebJobs SDK - Azure ile çalışmaya başlama
description: WebJobs SDK olay odaklı arka plan işlemesi için giriş. Azure Hizmetleri ve üçüncü taraf hizmetleri verilere erişmek öğrenin.
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/15/2019
ms.author: glenga
ms.openlocfilehash: 390d9dcda18e10c21db8165653faec7880863ba1
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2019
ms.locfileid: "56734481"
---
# <a name="get-started-with-the-azure-webjobs-sdk-for-event-driven-background-processing"></a>Olay temelli arka planda işleme için Azure WebJobs SDK ile çalışmaya başlama

Bu makalede, Azure WebJobs SDK projesi oluşturun, yerel olarak çalıştırın ve Azure App Service'e dağıtma gösterilmektedir.

Yönergeler için olan [Visual Studio 2017](https://www.visualstudio.com/vs/), ancak diğer araçları ile gerçekleştirdiğiniz görevlerin gibi gerçekleştirilebilir [Visual Studio Code](https://code.visualstudio.com/).

## <a name="what-is-the-azure-webjobs-sdk"></a>Azure WebJobs SDK nedir?

Azure WebJobs SDK, arka plan işleme Azure hizmetlerindeki verilere erişen bir kod yazma görevini kolaylaştıran bir çerçevedir. Sıraya eklenen yeni bir ileti gibi bir işlevi tetiklemesi gereken olayları belirtmek için bir bildirim temelli söz dizimi SDK'sı sunar. Tetiklenen bir işlev sonra veri yazma ve okuma benzer bildirim temelli söz dizimini denetler. Bu sistem, Tetikleyicileri ve bağlamaları, Azure ve üçüncü taraf hizmetleri erişim ile ilişkili alt düzey kodlama görevlerin çoğunu üstlenir.

### <a name="functions-triggers-and-bindings"></a>İşlevler, tetikleyiciler ve bağlamalar

WebJobs SDK projesi, bir veya daha fazla tanımlar *işlevleri*. Bir işlev, Yöntem imzasında tetikleyici özniteliği olan bir yöntemdir. Bir işlevi çağırmak için koşullar Tetikleyicileri ve bağlamaları ne okumak ve yazmak belirtin. Örneğin, aşağıdaki işlev tetikleyici özniteliğinde bir kuyruk iletisi göründüğünde işlevi çağırmak için çalışma zamanı söyler `items` kuyruk. `Blob` Özniteliği söyleyen bir blobun okunabilmesi için kuyruk iletisi kullanılacak çalışma zamanı *iş öğeleri* kapsayıcı. Kuyruk iletisinin içeriği &mdash; sağlanan `queueTrigger` parametre &mdash; blob adıdır.

```cs
public static void Run(
    [QueueTrigger("items")] string myQueueItem,
    [Blob("workitems/{queueTrigger}", FileAccess.Read)] Stream myBlob,
    TraceWriter log)
{
    log.Info($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}
```

### <a name="versions-2x-and-3x"></a>Sürüm 2.x ve 3.x

Web işleri SDK'sı sürüm 3.x proje oluşturma yönergeleri söyleyin. Tanıtılan ana değişiklik sürümü tarafından 3.x .NET Core, .NET Framework yerine kullanılır. ' Nın v2.x v3.x arasındaki farklar adlı bu makaledeki genişletme.

### <a name="azure-functions"></a>Azure İşlevleri

[Azure işlevleri](../azure-functions/functions-overview.md) işlevleriniz için sunucusuz bir seçenek sağlar. İşlevleri WebJobs SDK'da temel alır ve Web işleri SDK'sını doğrudan kullanmak zorunda kalmazsınız zamanki bir seçenektir. Azure işlevleri 2.x kullanan Web işleri SDK'sı 3.x. Daha fazla bilgi için [Azure işlevleri ve WebJobs SDK karşılaştırması](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md#compare-functions-and-webjobs).

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, sahip olduğunuz varsayılır [bir Azure hesabı](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) ve deneyimini [Azure App Service'te uygulamaları](overview.md). Bu makaledeki adımları tamamlayabilmeniz için:

* [Visual Studio 2017'yi](/visualstudio/install/) ile **Azure geliştirme** iş yükü. Zaten Visual Studio yüklü ancak bu iş yükü yüklü değilse, iş yükü seçerek ekleyin **Araçlar > araçları ve özellikleri Al**.
* [Bir App Service uygulaması oluşturma](app-service-web-get-started-dotnet-framework.md). Webjob'a dağıtabileceğiniz bir hesabınız varsa, yeni bir tane oluşturmak yerine bunu kullanabilirsiniz.

## <a name="create-a-project"></a>Proje oluşturma

1. Visual Studio'da **Dosya > Yeni > Proje**.

2. Seçin **Windows Klasik Masaüstü > konsol uygulaması (.NET Framework)**.

3. Projeyi adlandırın *WebJobsSDKSample*ve ardından **Tamam**.

   ![Yeni Proje iletişim kutusu](./media/webjobs-sdk-get-started/new-project.png)

## <a name="webjobs-nuget-packages"></a>Web işleri NuGet paketleri

Web işleri SDK'sını yükleyin NuGet paketlerini,'ın v2.x ve v3.x biraz farklıdır.

### <a name="install-sdk-version-3x-packages"></a>SDK sürümü 3.x paketlerini yükleme

1. Aşağıdaki NuGet paketlerini en son kararlı 3.x sürümlerini yükleyin:

    * `Microsoft.Azure.WebJobs`
    * `Microsoft.Azure.WebJobs.Extensions`

    İşte **Paket Yöneticisi Konsolu** komutları 3.0.3 sürümü için:

    ```powershell
    Install-Package Microsoft.Azure.WebJobs -version 3.0.2
    Install-Package Microsoft.Azure.WebJobs.Extensions -version 3.0.1
    ```

### <a name="install-the-sdk-version-2x-package"></a>SDK sürüm 2.x paketini yükleme

1. En son kararlı sürümü olan 2.x sürümüne NuGet paketini yükleyin `Microsoft.Azure.WebJobs`.

    İşte **Paket Yöneticisi Konsolu** komutu 2.2.0 sürümü için:

    ```powershell
    Install-Package Microsoft.Azure.WebJobs -version 2.2.0
    ```

## <a name="create-the-host"></a>Konağı oluşturma

Konak işlevler çalışma zamanı kapsayıcıdır: tetikleyiciler ve çağrıları işlevleri için dinler. Oluşturduğunuz ana SDK sürümüne bağlıdır.

### <a name="version-3x"></a>Sürüm 3.x

Aşağıdaki uygulayan bir ana bilgisayar oluşturur [ `IHost` ](/dotnet/api/microsoft.extensions.hosting.ihost), ASP.NET Core genel ana bilgisayar olduğu. 

1. İçinde *Program.cs*, ekleme bir `using` deyimi:

    ```cs
    using Microsoft.Extensions.Hosting;
    ```

1. `Main` yöntemini aşağıdaki kod ile değiştirin:

    ```cs
    static void Main(string[] args)
    {
        var builder = new HostBuilder();
        builder.ConfigureWebJobs(b =>
                {
                    b.AddAzureStorageCoreServices();
                });
        var host = builder.Build();
        using (host)
        {
            host.Run();
        }
    }
    ```

ASP.NET Core, ana bilgisayar yapılandırmalarını üzerinde yöntemleri çağırarak ayarlanır [ `HostBuilder` ](/dotnet/api/microsoft.extensions.hosting.hostbuilder) örneği. Daha fazla bilgi için [.NET genel ana bilgisayar](/aspnet/core/fundamentals/host/generic-host). `ConfigureWebJobs` Genişletme yöntemi WebJobs ana bilgisayarı başlatır.

### <a name="version-2x"></a>Sürüm 2.x

Aşağıdaki kod oluşturur **JobHost** nesne.

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

Ana bilgisayar yapılandırmalarını ayarlanır `JobHostConfiguration` örneği oluşturmak için kullanılan `JobHost`.

## <a name="enable-console-logging"></a>Konsol günlüğü kaydetmeyi etkinleştir

ASP.NET Core için her iki sürümü önerilir geliştirilmiş günlüğe kaydetme çerçevesi 2.x ve Web işleri SDK'sının 3.x. Bu çerçeve, daha iyi performans ve depolama ortamı ve filtreleme konusunda daha fazla esneklik sunar. Daha fazla bilgi için bkz. [ASP.NET Core günlüğü](/aspnet/core/fundamentals/logging).

Bu bölümde, bu framework kullanan konsol günlüğünü ayarlama.

1. Aşağıdaki NuGet paketlerini en son kararlı sürümünü yükleyin:

   * `Microsoft.Extensions.Logging` -Günlüğe kaydetme çerçevesi.
   * `Microsoft.Extensions.Logging.Console` -Konsol *sağlayıcısı*. Bir sağlayıcı günlükleri belirli bir hedefe, bu durumda konsola gönderir.

   İşte **Paket Yöneticisi Konsolu** komutları 2.2.0 sürümü için:

   ```powershell
   Install-Package Microsoft.Extensions.Logging -version 2.2.0
   ```

   ```powershell
   Install-Package Microsoft.Extensions.Logging.Console -version 2.2.0
   ```

1. İçinde *Program.cs*, ekleme bir `using` deyimi:

   ```cs
   using Microsoft.Extensions.Logging;
   ```

1. Günlük kod güncelleştirmelerini SDK sürümünüzde bağlıdır:

    **Sürüm 3.x**

    Çağrı [ `ConfigureLogging` ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging) metodunda [ `HostBuilder` ](/dotnet/api/microsoft.extensions.hosting.hostbuilder). [ `AddConsole` ](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions.addconsole) Yöntemi konsol günlüğü yapılandırmaya ekler.

    ```cs
    builder.ConfigureLogging((context, b) =>
    {
        b.AddConsole();
    });
    ```

    `Main` Metodu artık şuna benzer:

    ```cs
    static void Main(string[] args)
    {
        var builder = new HostBuilder();
        builder.ConfigureWebJobs(b =>
                {
                    b.AddAzureStorageCoreServices();
                });
        builder.ConfigureLogging((context, b) =>
                {
                    b.AddConsole();
                });
        var host = builder.Build();
        using (host)
        {
            host.Run();
        }
    }
    ```

    **Sürüm 2.x**

    İçinde `Main` yöntemini güncelleştirmek için kodu ekleyin `JobHostConfiguration` oluşturmadan önce `JobHost`:

    ```cs
    config.DashboardConnectionString = "";
    var loggerFactory = new LoggerFactory();
    config.LoggerFactory = loggerFactory
        .AddConsole();
    ```

    `Main` Metodu artık şuna benzer:

    ```cs
    var config = new JobHostConfiguration();
    config.DashboardConnectionString = "";
    var loggerFactory = new LoggerFactory();
    config.LoggerFactory = loggerFactory
        .AddConsole();
    var host = new JobHost(config);
    host.RunAndBlock();
    ```

    Bu güncelleştirmeler, aşağıdakileri yapın:

    * Devre dışı bırakır [Pano günlük](https://github.com/Azure/azure-webjobs-sdk/wiki/Queues#logs). İzleme aracı eski panodur ve yüksek performanslı üretim senaryoları için panoyu günlük önerilmez.
    * Varsayılan konsol sağlayıcısıyla ekler [filtreleme](webjobs-sdk-how-to.md#log-filtering).

Şimdi, gelen iletileri tarafından tetiklenen bir işlev ekleyebilirsiniz. bir [Azure depolama kuyruğu](../azure-functions/functions-bindings-storage-queue.md).

## <a name="install-binding-extensions"></a>Bağlama uzantılarını yükleme

Sürümünden başlayarak 3.x işlevlerinizi içinde kullandığınız WebJobs SDK bağlama özniteliklerini için uzantıları açıkça yüklemeniz gerekir. Yalnızca özel durumlar [Zamanlayıcı tetikleyicisi](../azure-functions/functions-bindings-timer.md) ve [HTTP tetikleyicisi](../azure-functions/functions-bindings-http-webhook.md), uzantı gerektirmez. Sürüm extensions WebJobs SDK 2.x kullanmaz, bunlar SDK'da eklenir. Sürümünü kullanıyorsanız 2.x, önceden sonraki bölüme atlayın.

1. En son kararlı sürümünü [Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage) NuGet paketi sürüm 3.x. 

    İşte **Paket Yöneticisi Konsolu** komutunu 3.0.2 sürümü için:

    ```powershell
    Install-Package Microsoft.Azure.WebJobs.Extensions.Storage -Version 3.0.2
    ```

2. İçinde `ConfigureWebJobs` genişletme yöntemi, çağrı `AddAzureStorage` metodunda [ `HostBuilder` ](/dotnet/api/microsoft.extensions.hosting.hostbuilder) Örnek Depolama uzantı başlatılamadı. Bu noktada, `ConfigureWebJobs` yöntemi aşağıdaki örnekteki gibi görünür:

    ```cs
    builder.ConfigureWebJobs(b =>
                    {
                        b.AddAzureStorageCoreServices();
                        b.AddAzureStorage();
                    });
    ```

## <a name="create-a-function"></a>İşlev oluşturma

1. Projeye sağ tıklayın, **Ekle** > **yeni öğe...** ve yeni adı C# sınıf dosyası *Functions.cs*.

1. Functions.cs içinde oluşturulan şablon aşağıdaki kodla değiştirin:

   ```cs
   using Microsoft.Azure.WebJobs;
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

   `QueueTrigger` Özniteliği söyler adlı bir Azure depolama kuyruğuna üzerinde yeni bir mesaj yazıldığında bu işlevi çağırmak için çalışma zamanı `queue`. Kuyruk iletisi içeriğini yöntemi kodunda sağlanır `message` parametresi. Tetikleyici verileri işlem nerede Yöntemin gövdesi olur. Bu örnekte, kod yalnızca iletiyi günlüğe kaydeder.

   `message` Parametresi bir dize olması gerekmez. Ayrıca bir JSON nesnesi bir bayt dizisi bağlayabilir veya [CloudQueueMessage](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage) nesne. [Kuyruk tetikleyicisi kullanımını görmek](../azure-functions/functions-bindings-storage-queue.md#trigger---usage). Her bağlama türü (örneğin, kuyruklar, blobları veya tabloları) bağlayabilirsiniz parametre türleri farklı bir kümesi vardır.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Yerel olarak çalışan Azure depolama öykünücüsü WebJobs SDK gereken tüm özelliklere sahip değil. Bu nedenle bu bölümde Azure depolama hesabı oluşturma ve bunu kullanmak için projeyi yapılandırın.

1. Açık **Sunucu Gezgini** Visual studio ve azure'da oturum açın. Sağ **Azure** düğümüne tıklayın ve ardından **Microsoft Azure aboneliğine bağlanma**.

   ![Azure'da oturum açma](./media/webjobs-sdk-get-started/sign-in.png)

1. Altında **Azure** düğümünde **Sunucu Gezgini**, sağ **depolama**ve ardından **oluşturma depolama hesabı**.

   ![Depolama hesabı menü oluşturma](./media/webjobs-sdk-get-started/create-storage-account-menu.png)

1. İçinde **depolama hesabı oluştur** iletişim kutusunda, depolama hesabı için benzersiz bir ad girin.

1. Aynı seçin **bölge** App Service uygulamanızı oluşturduğunuz veya size bir bölge kapatın.

1. **Oluştur**’u seçin.

   ![Depolama hesabı oluşturma](./media/webjobs-sdk-get-started/create-storage-account.png)

1. Altında **depolama** düğümünde **Sunucu Gezgini**, yeni depolama hesabını seçin. İçinde **özellikleri** penceresinde öğesinin üç noktasını (**...** ) sağ tarafında **bağlantı dizesi** değeri alanı.

   ![Bağlantı dizesi üç nokta](./media/webjobs-sdk-get-started/conn-string-ellipsis.png)

1. Bağlantı dizesini kopyalayın ve yeniden bir kolayca kopyalayabilir, bu değer bir yere kaydedin.

   ![Bağlantı dizesini kopyalayın](./media/webjobs-sdk-get-started/copy-key.png)

## <a name="configure-storage-to-run-locally"></a>Yerel olarak çalıştırmak için depolamayı yapılandırma

WebJobs SDK, uygulama ayarlarında, Azure depolama bağlantı dizesini arar. Yerel olarak çalıştırdığınızda, bu değer, yerel yapılandırma dosyasında veya ortam değişkenlerini arar.

### <a name="appsettingsjson-sdk-version-3x"></a>appSettings.JSON (SDK sürümü 3.x)

1. Oluşturma bir *appsettings.json* dosya ya da ekleme bir `AzureWebJobsStorage` aşağıdaki örnekteki gibi alan:

    ```json
    {
        "AzureWebJobsStorage": "{storage connection string}"
    }
    ```

1. Değiştirin *{depolama bağlantı dizesi}* daha önce kopyaladığınız bağlantı dizesi.

### <a name="appconfig-sdk-version-2x"></a>App.config (SDK sürüm 2.x)

1. Eklemek için aşağıdaki XML *App.config* açtıktan hemen sonra dosya `<configuration>` etiketi.

   ```xml
   <connectionStrings>
     <add name="AzureWebJobsStorage" connectionString="{storage connection string}" />
   </connectionStrings>
   ```

1. Değiştirin *{depolama bağlantı dizesi}* daha önce kopyaladığınız bağlantı dizesi.

Daha sonra uygulamanızı Azure App Service'te aynı bağlantı dizesi uygulama ayarı ekleyeceksiniz.

## <a name="test-locally"></a>Yerel olarak test etme

Bu bölümde, projeyi yerel olarak çalıştırın ve bir kuyruk iletisi oluşturarak işlevi tetikleyin oluşturun.

1. Projeyi çalıştırmak için CTRL + F5 tuşlarına basın.

   Konsol, çalışma zamanının işlevinizi bulundu ve bunu tetiklemek kuyruk iletileri için bekliyor gösterir. Aşağıdaki çıktı v3.x ana bilgisayar tarafından oluşturulur:

   ```console
    info: Microsoft.Azure.WebJobs.Hosting.JobHostService[0]
          Starting JobHost
    info: Host.Startup[0]
          Found the following functions:
          WebJobsSDKSample.Functions.ProcessQueueMessage

    info: Host.Startup[0]
          Job host started
    Application started. Press Ctrl+C to shut down.
    Hosting environment: Development
    Content root path: C:\WebJobsSDKSample\WebJobsSDKSample\bin\Debug\netcoreapp2.1\
   ```

1. Konsol penceresini kapatın.

1. İçinde **Sunucu Gezgini** Visual Studio'da, yeni depolama hesabınız için düğümü genişletin ve ardından sağ tıklayarak **kuyrukları**. 

1. Seçin **kuyruk oluşturma**.

1. Girin *kuyruk* seçin ve kuyruk adı olarak **Tamam**.

   ![Kuyruk oluştur](./media/webjobs-sdk-get-started/create-queue.png)

1. Yeni Kuyruk düğümünü sağ tıklatın ve ardından **sırayı görüntüle**.

1. Seçin **ekleme ileti** simgesi.

   ![Kuyruk oluştur](./media/webjobs-sdk-get-started/create-queue-message.png)

1. İçinde **ekleme ileti** iletişim kutusunda girin *Merhaba Dünya!* olarak **ileti metni**ve ardından **Tamam**.

   ![Kuyruk oluştur](./media/webjobs-sdk-get-started/hello-world-text.png)

1. Projeyi tekrar çalıştırın.

   Nedeniyle `QueueTrigger` özniteliğini `ProcessQueueMessage` işlevi WeJobs SDK çalışma zamanı dinler kuyruk iletileri için açıldığında. Sıranın adlı yeni bir kuyruk iletisi bulduğu *kuyruk* ve işlevini çağırır.

   Şu nedenle [üstel geri alma yoklama kuyruk](../azure-functions/functions-bindings-storage-queue.md#trigger---polling-algorithm), iletiyi bulun ve işlevin çalıştırılabilmesi çalışma zamanı için 2 dakika kadar sürebilir. Bu bekleme süresi çalıştırarak azaltılabilir [geliştirme modu](webjobs-sdk-how-to.md#host-development-settings).

  Sürüm için 3.x, konsol çıktısı şuna benzer:

   ```console
    info: Function.ProcessQueueMessage[0]
          Executing 'Functions.ProcessQueueMessage' (Reason='New queue message detected on 'queue'.', Id=2c319369-d381-43f3-aedf-ff538a4209b8)
    info: Function.ProcessQueueMessage[0]
          Trigger Details: MessageId: b00a86dc-298d-4cd2-811f-98ec39545539, DequeueCount: 1, InsertionTime: 1/18/2019 3:28:51 AM +00:00
    info: Function.ProcessQueueMessage.User[0]
          Hello World!
    info: Function.ProcessQueueMessage[0]
          Executed 'Functions.ProcessQueueMessage' (Succeeded, Id=2c319369-d381-43f3-aedf-ff538a4209b8)
   ```

1. Konsol penceresini kapatın.

## <a name="add-application-insights-logging"></a>Application Insights günlük Ekle

Projeyi Azure içinde çalıştığında, işlevi yürütme konsol çıktısı görüntüleyerek izleyemez. Tavsiye ederiz izleme çözümü [Application Insights](../azure-monitor/app/app-insights-overview.md). Daha fazla bilgi için [İzleyici Azure işlevleri](../azure-functions/functions-monitoring.md).

Bu bölümde, Azure'a dağıtmadan önce Application Insights günlük ayarlama için şu görevleri yapın:

* Bir App Service uygulaması ve çalışmak için Application Insights örneği olduğundan emin olun.
* Application Insights örneği ve daha önce oluşturduğunuz depolama hesabını kullanmak için App Service uygulamasını yapılandırın.
* Proje için Application Insights için günlük kaydı ayarlayın.

### <a name="create-app-service-app-and-application-insights-instance"></a>App Service uygulaması ve Application Insights örneği oluşturma

1. Kullanabileceğiniz, App Service uygulaması zaten yoksa [oluşturmak](app-service-web-get-started-dotnet-framework.md). Uygulamanızı oluşturduğunuzda, bağlı bir Application Insights kaynağı da oluşturabilirsiniz. Bunu yaptığınızda `APPINSIGHTS_INSTRUMENTATIONKEY` sizin için uygulamanızı ayarlayın.

1. Kullanabileceğiniz, Application Insights kaynağı henüz yoksa [oluşturmak](../azure-monitor/app/create-new-resource.md ). Ayarlama **uygulama türü** için **genel**ve aşağıdaki bölümlerde atla **izleme anahtarını kopyalama**.

1. Kullanmak istediğiniz bir Application Insights kaynağı zaten varsa [izleme anahtarını kopyalama](../azure-monitor/app/create-new-resource.md#copy-the-instrumentation-key).

### <a name="configure-app-settings"></a>Uygulama ayarlarını yapılandırma 

1. İçinde **Sunucu Gezgini** Visual Studio'da genişletme **App Service** düğümünde **Azure**.

1. App Service uygulamanızı olan kaynak grubunu genişletin ve ardından App Service uygulamanıza sağ tıklayın.

1. Seçin **görünüm ayarları**.

1. İçinde **bağlantı dizeleri** kutusunda, şu girişi ekleyin.

   |Ad  |bağlantı dizesi  |Veritabanı Türü|
   |---------|---------|------|
   |AzureWebJobsStorage | {daha önce kopyaladığınız depolama bağlantı dizesi}|Özel|

1. Varsa **uygulama ayarları** kutusu, bir Application Insights izleme anahtarı, daha önce kopyaladığınız eklemesini yok. (İzleme anahtarını zaten Burada, App Service uygulamasını nasıl oluşturduğunuza bağlı olabilir.)

   |Ad  |Değer  |
   |---------|---------|
   |APPINSIGHTS_INSTRUMENTATIONKEY | {İzleme anahtarını} |

1. Değiştirin *{izleme anahtarını}* kullanmakta olduğunuz Application Insights kaynağı izleme anahtarını ile.

1. **Kaydet**’i seçin.

1. Application Insights bağlantı projeye ekleyin, yerel olarak çalıştırabilirsiniz:

    **Sürüm 3.x**

    İçinde *appsettings.json* ekleyin bir `APPINSIGHTS_INSTRUMENTATIONKEY` aşağıdaki örnekteki gibi alan:

    ```json
    {
        "AzureWebJobsStorage": "{storage connection string}",
        "APPINSIGHTS_INSTRUMENTATIONKEY": "{instrumentation key}"
    }
    ```

    **Sürüm 2.x**

    Eklemek için aşağıdaki XML *App.config* dosya, bağlantı dizeleri koleksiyona hemen sonra.

    ```xml
    <appSettings>
        <add key="APPINSIGHTS_INSTRUMENTATIONKEY" value="{instrumentation key}" />
    </appSettings>
    ```

    Her iki sürümde değiştirin *{izleme anahtarını}* kullanmakta olduğunuz Application Insights kaynağı izleme anahtarını ile.

1. Yaptığınız değişiklikleri kaydedin.

### <a name="add-application-insights-logging-provider"></a>Application Insights oturum açma sağlayıcısı Ekle

Yararlanmak için [Application Insights](../azure-monitor/app/app-insights-overview.md) günlüğü, aşağıdakileri yapmak için günlük kodunuzu güncelleştirin:

* Varsayılan bir Application Insights günlük sağlayıcısını ekleyin [filtreleme](webjobs-sdk-how-to.md#log-filtering); tüm bilgiler ve daha üst düzey günlükleri gider hem Konsol hem de Application Insights için yerel olarak çalıştırırken.
* PUT `LoggerFactory` nesnesine bir `using` konak çıktığında, günlük çıktısını Temizlenen emin olmak için blok.

#### <a name="version-3x"></a>Sürüm 3.x

1. Application Insights oturum açma sağlayıcısı için NuGet paketini en son kararlı 3.x sürümünü yükleyin: `Microsoft.Azure.WebJobs.Logging.ApplicationInsights`.

   İşte **Paket Yöneticisi Konsolu** komutunu 3.0.2 sürümü için:

   ```powershell
   Install-Package Microsoft.Azure.WebJobs.Logging.ApplicationInsights -Version 3.0.2
   ```

1. Açık *Program.cs* ve kodu değiştirin `Main` yöntemini aşağıdaki kod ile:

    ```cs
    static void Main(string[] args)
    {
        var builder = new HostBuilder();
        builder.UseEnvironment(EnvironmentName.Development);
        builder.ConfigureWebJobs(b =>
                {
                    b.AddAzureStorageCoreServices();
                    b.AddAzureStorage();
                });
        builder.ConfigureLogging((context, b) =>
                {
                    b.AddConsole();

                    // If the key exists in settings, use it to enable Application Insights.
                    string instrumentationKey = context.Configuration["APPINSIGHTS_INSTRUMENTATIONKEY"];
                    if (!string.IsNullOrEmpty(instrumentationKey))
                    {
                        b.AddApplicationInsights(o => o.InstrumentationKey = instrumentationKey);
                    }
                });
        var host = builder.Build();
        using (host)
        {
            host.Run();
        }
    }
    ```

#### <a name="version-2x"></a>Sürüm 2.x

1. En son kararlı sürümü olan 2.x sürümüne NuGet paketini yüklemek için Application Insights günlük sağlayıcısını: `Microsoft.Azure.WebJobs.Logging.ApplicationInsights`.

   İşte **Paket Yöneticisi Konsolu** komutu 2.2.0 sürümü için:

   ```powershell
   Install-Package Microsoft.Azure.WebJobs.Logging.ApplicationInsights -version 2.2.0
   ``` 

1. .NET configuration manager için NuGet paketini en son kararlı 4.x sürümünü yükleyin: `System.Configuration.ConfigurationManager`.

   İşte **Paket Yöneticisi Konsolu** komutunu 4.4.1 sürümü için:

   ```powershell
   Install-Package System.Configuration.ConfigurationManager -version 4.4.1
   ``` 

1. Açık *Program.cs* ve ekleme bir `using` deyimi configuration manager için:

   ```csharp
   using System.Configuration;
   ```

1. Değiştirin `Main` yöntemini aşağıdaki kod ile:

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

## <a name="test-application-insights-logging"></a>Test Application Insights günlüğe kaydetme

Bu bölümde, yerel olarak yeniden Konsolu için de Application Insights, günlük verilerini artık geçiyor doğrulamak için çalıştırın.

1. Kullanım **Sunucu Gezgini** aynı şekilde Visual bir kuyruk iletisi oluşturmak üzere Studio'da yaptığınız [önceki](#trigger-the-function-in-azure)girin dışında *Hello App Insights!* ileti metni.

1. Projeyi çalıştırın.

   WebJobs SDK kuyruk iletisini işler ve konsol penceresinde günlüklere bakın.

1. Konsol penceresini kapatın.

1. Açık [Azure portalında](https://portal.azure.com/)ve Application Insights kaynağınıza gidin.

1. Seçin **arama**.

   ![Ara'yı seçin](./media/webjobs-sdk-get-started/select-search.png)

1. Görmüyorsanız *Hello App Insights!* ileti, select **Yenile** birkaç dakika için düzenli aralıklarla. (Günlüklerini temizlemek Application Insights istemci bir süredir aldığından işlediğinden hemen günlükleri görünmez.)

   ![Application ınsights'ta günlükleri](./media/webjobs-sdk-get-started/logs-in-ai.png)

1. Konsol penceresini kapatın.

## <a name="deploy-as-a-webjob"></a>Bir Web işi olarak dağıtmak

Bu bölümde, projeyi bir WebJob olarak dağıtın. Bir App Service uygulamasına dağıtın, [daha önce oluşturduğunuz](#create-app-service-app-and-application-insights-instance). Azure'da çalıştığı sırada kodunuzu test etmek için bir kuyruk iletisi oluşturarak bir işlev çağrısını tetikler.

1. İçinde **Çözüm Gezgini**projeye sağ tıklayın ve ardından **Azure WebJob olarak Yayımla**.

1. İçinde **Azure WebJob Ekle** iletişim kutusunda **Tamam**.

   ![Azure Web işi Ekle](./media/webjobs-sdk-get-started/add-azure-webjob.png)

   Visual Studio, NuGet paketi WebJob yayımlama için otomatik olarak yükler.

1. İçinde **profili** adımında **Yayımla** seçin **Microsoft Azure App Service**.

   ![Yayımla iletişim kutusu](./media/webjobs-sdk-get-started/publish-dialog.png)

1. İçinde **App Service** iletişim kutusunda **kaynak grubunuzun > App Service uygulamanızı**ve ardından **Tamam**.

   ![App Service iletişim kutusu](./media/webjobs-sdk-get-started/app-service-dialog.png)

1. Yayımlama profili oluşturulduktan sonra seçin **Yayımla**.

## <a name="trigger-the-function-in-azure"></a>Azure işlevi tetikleyin

1. Değil çalıştırdığınız yerel olarak emin olun (hala açık değilse konsol penceresini kapatın). Aksi takdirde yerel örneğini oluşturduğunuz herhangi bir kuyruk iletileri işlemek için ilk olabilir.

1. İçinde **kuyruk** sayfasında Visual Studio'da, kuyruğa bir ileti ekleyin, önceki gibi.

1. Yenileme **kuyruk** sayfası ve yeni iletinin, Azure'da çalışan işlev tarafından işlenmemiş olduğundan kaybolur.

   > [!TIP]
   > Azure'da test ederken kullanmak [geliştirme modu](webjobs-sdk-how-to.md#host-development-settings) kuyruğu tetikleme işlevi hemen başlatıldığından emin olun ve son gecikmeleri önlemek için [üstel geri alma yoklama kuyruk](../azure-functions/functions-bindings-storage-queue.md#trigger---polling-algorithm).

### <a name="view-logs-in-application-insights"></a>Application ınsights'ta günlüklerini görüntüle

1. Açık [Azure portalında](https://portal.azure.com/)ve Application Insights kaynağınıza gidin.

1. Seçin **arama**.

1. Görmüyorsanız *Merhaba Azure!* ileti, select **Yenile** birkaç dakika için düzenli aralıklarla.

   Bir WebJob içinde çalışan işlevi günlüklerinden gördüğünüz gibi *Merhaba Azure!* önceki bölümde girdiğiniz metin.

## <a name="add-an-input-binding"></a>Bir giriş bağlaması Ekle

Giriş bağlamaları veri okuyan kod basitleştirin. Bu örnekte, kuyruk iletisi blob adı olacak ve blob adı bulmak ve Azure Depolama'daki bir blobun okunabilmesi için kullanacaksınız.

1. İçinde *Functions.cs*, değiştirin `ProcessQueueMessage` yöntemini aşağıdaki kod ile:

   ```cs
   public static void ProcessQueueMessage(
       [QueueTrigger("queue")] string message,
       [Blob("container/{queueTrigger}", FileAccess.Read)] Stream myBlob,
       ILogger logger)
   {
       logger.LogInformation($"Blob name:{message} \n Size: {myBlob.Length} bytes");
   }
   ```

   Bu kodda, `queueTrigger` olduğu bir [ifade bağlama](../azure-functions/functions-bindings-expressions-patterns.md), çalışma zamanında farklı bir değer çözümler anlamına gelir.  Çalışma zamanında, kuyruk iletisi içeriğini sahiptir.

1. Ekleme bir `using`:

   ```cs
   using System.IO;
   ```

1. Depolama hesabınızdaki blob kapsayıcı oluşturun.

   a. İçinde **Sunucu Gezgini** Visual Studio'da, depolama hesabınızın düğümünü genişletin, sağ **Blobları**ve ardından **Blob kapsayıcısı Oluştur**.

   b. İçinde **Blob kapsayıcısı Oluştur** iletişim kutusunda girin *kapsayıcı* kapsayıcı adı ve ardından olarak **Tamam**.

1. Karşıya yükleme *Program.cs* blob kapsayıcısına dosya. (Bu dosya burada kullanılan örnek olarak; size herhangi bir metin dosyasını karşıya yükleme ve dosya adında bir kuyruk iletisi oluşturun.)

   a. İçinde **Sunucu Gezgini**, oluşturduğunuz kapsayıcıya düğümünü çift tıklayın.

   b. İçinde **kapsayıcı** penceresinde **karşıya** düğmesi.

   ![Blobu karşıya yükleme düğmesini](./media/webjobs-sdk-get-started/blob-upload-button.png)

   c. Bulmak ve seçmek *Program.cs*ve ardından **Tamam**.

1. Daha önce oluşturduğunuz sırada bir kuyruk iletisi oluşturmak ile *Program.cs* iletisinin metni.

   ![Kuyruk iletisi Program.cs](./media/webjobs-sdk-get-started/queue-msg-program-cs.png)

1. Projeyi çalıştırın.

   Kuyruk iletisi ardından blobu okur ve uzunluğunu günlükleri işlevi tetikler. Konsol çıktısı şuna benzer:

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

Çıkış bağlamaları veri yazan kodu basitleştirin. Bu örnek blob boyutu günlüğü yerine bir kopyasını yazarak Öncekine değiştirir. BLOB Depolama bağlamaları, daha önce yüklediğimiz Azure depolama uzantısı paketinde dahil edilir.

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

1. Başka bir kuyruk iletisi *Program.cs* iletisinin metni.

1. Projeyi çalıştırın.

   Kuyruk iletisi ardından blobu okur, uzunluğu günlüğe kaydeder ve yeni bir blob oluşturur işlevini tetikler. Konsol çıktısı, aynıdır ancak ne zaman blob kapsayıcı penceresine gidin ve seçin **Yenile**, adlı yeni bir blob gördüğünüz *Program.cs kopyalama.*

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuz, oluşturma, çalıştırma ve Web işleri SDK'sı projeyi dağıtmak nasıl göstermiştir.

WebJobs SDK'si projeye gider her şeyi göstermek için sıfırdan bir proje oluşturma yönergeleri vardı. Bir sonraki projenizi oluşturduğunuzda, ancak kullanmayı **Azure WebJob** şablonunda **bulut** kategorisi. Bu şablon, NuGet paketlerini ve örnek kodu zaten ayarlanmış olan bir proje oluşturur. Örnek kod, yeni günlük altyapısının kullanılacağını değiştirilmesi gerekebilir.

> [!div class="nextstepaction"]
> [Web işleri SDK'sı hakkında daha fazla bilgi edinin](webjobs-sdk-how-to.md)
