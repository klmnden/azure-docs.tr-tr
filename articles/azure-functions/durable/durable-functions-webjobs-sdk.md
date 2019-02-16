---
title: Dayanıklı işlevler - Azure WebJobs çalıştırmayı öğrenin
description: Kod ve dayanıklı işlevler Web işleri Web işleri SDK'sı çalıştırmak amacıyla kullanmak üzere yapılandırma hakkında bilgi edinin.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/25/2018
ms.author: azfuncdf
ms.openlocfilehash: 282b07a384ac6db5bfbc144ca06440f3a8f01a6a
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56301205"
---
# <a name="how-to-run-durable-functions-as-webjobs"></a>Dayanıklı işlevler Web işleri çalıştırma

[Azure işlevleri](../functions-overview.md) ve [dayanıklı işlevler](durable-functions-overview.md) uzantı üzerinde oluşturulan [WebJobs SDK](../../app-service/webjobs-create.md). `JobHost` WebJobs SDK Azure işlevleri çalışma zamanı olan. Denetime ihtiyacınız varsa `JobHost` davranışı Azure işlevleri'nde mümkün olmayan yöntemlerden geliştirin ve dayanıklı işlevler Web işleri SDK'sını kullanarak kendiniz çalıştırın. Bir Azure WebJob içinde dayanıklı işlevler daha sonra çalıştırabilir veya bir konsol uygulaması her yerde çalışır.

Zincirleme dayanıklı işlevler örnek bir Web işleri SDK'sı sürümü olarak kullanılabilir: indirin veya kopyalayın [dayanıklı işlevler depo](https://github.com/azure/azure-functions-durable-extension/) gidin *örnekleri\\webjobssdk\\Zincirleme* klasör.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, C# sınıf kitaplığı geliştirme için Azure işlevleri ve dayanıklı işlevler Web işleri SDK'ın temel alışık olduğunuz varsayılır. Bu konulara bir giriş gerekiyorsa, aşağıdaki kaynaklara bakın:

* [WebJobs SDK ile çalışmaya başlama](../../app-service/webjobs-sdk-get-started.md)
* [Visual Studio kullanarak ilk işlevinizi oluşturma](../functions-create-your-first-function-visual-studio.md)
* [Dayanıklı İşlevler](durable-functions-sequence.md)

Bu makaledeki adımları tamamlayabilmeniz için:

* [Visual Studio 2017 sürüm 15.6 veya üzerini yükleyin](https://docs.microsoft.com/visualstudio/install/) ile **Azure geliştirme** iş yükü.

  Zaten Visual Studio yüklü ancak bu iş yükü yüklü değilse, iş yükü seçerek ekleyin **Araçlar > araçları ve özellikleri Al**.

  (Kullanabilirsiniz [Visual Studio Code](https://code.visualstudio.com/) bunun yerine, ancak alan yönergelerden bazılarını Visual Studio'ya özgüdür.)

* Yükleme ve çalıştırma [Azure Storage öykünücüsü](../../storage/common/storage-use-emulator.md) 5.2 veya sonraki bir sürümü. Güncelleştirilecek alternatiftir *App.config* bir Azure depolama bağlantı dizesi içeren dosya.

## <a name="webjobs-sdk-versions"></a>WebJobs SDK sürümleri

Bu makalede, WebJobs SDK 2.x projeyi geliştirmek üzere açıklanmaktadır (Azure işlevleri sürüm eşdeğer 1.x). Sürümü hakkında daha fazla bilgi için 3.x bkz [WebJobs SDK 3.x](#webjobs-sdk-3x) bu makalenin ilerleyen bölümlerinde.

## <a name="create-console-app"></a>Konsol uygulaması oluşturma

Yeni bir konsol uygulama projesi yüklü uygun NuGet paketleri ile bir Web işleri SDK'sı projesidir.

Visual Studio **yeni proje** iletişim kutusunda **Windows Klasik Masaüstü > konsol uygulaması (.NET Framework)**. Proje dosyasında `TargetFrameworkVersion` olmalıdır `v4.6.1`.

Visual Studio seçerek kullanabileceğiniz bir WebJob proje şablonunu da olan **bulut > Azure Web işi (.NET Framework)**. Bu şablon, bazıları gerekmeyen birçok paketi yükler.

## <a name="install-nuget-packages"></a>NuGet paketlerini yükleme

WebJobs SDK, çekirdek bağlamaları, günlüğe kaydetme çerçevesi ve dayanıklı görev uzantısı için NuGet paketleri gerekir. İşte **Paket Yöneticisi Konsolu** bu paketleri, bu makalenin yazıldığı tarih itibarıyla en son kararlı sürüm numaraları için komutları:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions -version 2.2.0
Install-Package Microsoft.Extensions.Logging -version 2.0.1
Install-Package Microsoft.Azure.WebJobs.Extensions.DurableTask -version 1.4.0
```

Ayrıca günlük sağlayıcıları gerekir. Aşağıdaki komutlar Application Insights sağlayıcıyı yükleyin ve `ConfigurationManager`. `ConfigurationManager` Application Insights izleme anahtarı uygulama ayarlarından alabilmenizi sağlar.

```powershell
Install-Package Microsoft.Azure.WebJobs.Logging.ApplicationInsights -version 2.2.0
Install-Package System.Configuration.ConfigurationManager -version 4.4.1
```

Aşağıdaki komut, konsol sağlayıcısını yükler:

```powershell
Install-Package Microsoft.Extensions.Logging.Console -version 2.0.1
```

## <a name="jobhost-code"></a>JobHost kod

Dayanıklı işlevler uzantısını kullanmak için çağrı `UseDurableTask` üzerinde `JobHostConfiguration` nesnesine, `Main` yöntemi:

```cs
var config = new JobHostConfiguration();
config.UseDurableTask(new DurableTaskExtension
{
    HubName = "MyTaskHub",
};
```

İçinde ayarlayabildiğiniz özellikleri listesini `DurableTaskExtension` nesne, bkz: [host.json](../functions-host-json.md#durabletask).

`Main` Yöntemi yerdir de günlüğe kaydetme sağlayıcılarını kurma ayarlamak için. Aşağıdaki örnek, konsol ve Application Insights sağlayıcıları yapılandırır.

```cs
static void Main(string[] args)
{
    using (var loggerFactory = new LoggerFactory())
    {
        var config = new JobHostConfiguration();

        config.DashboardConnectionString = "";

        var instrumentationKey =
            ConfigurationManager.AppSettings["APPINSIGHTS_INSTRUMENTATIONKEY"];

        config.LoggerFactory = loggerFactory
            .AddApplicationInsights(instrumentationKey, null)
            .AddConsole();

        config.UseTimers();
        config.UseDurableTask(new DurableTaskExtension
        {
            HubName = "MyTaskHub",
        });
        var host = new JobHost(config);
        host.RunAndBlock();
    }
}
```

## <a name="functions"></a>İşlevler

Hizmet için Azure işlevleri yazmak için karşılaştırıldığında WebJobs SDK işlevleri için yazdığınız kodda bazı farklar vardır.

Web işleri SDK'sı, aşağıdaki Azure işlevleri özelliklerini desteklemez:

* [FunctionName özniteliği](#functionname-attribute)
* [HTTP tetikleyicisi](#http-trigger)
* [Dayanıklı işlevler HTTP yönetim API'si](#http-management-api)

### <a name="functionname-attribute"></a>FunctionName özniteliği

WebJobs SDK projede, bir işlevin yöntemi adı işlev adıdır. `FunctionName` Özniteliği, yalnızca Azure işlevleri'nde kullanılır.

### <a name="http-trigger"></a>HTTP tetikleyicisi

WebJobs SDK, bir HTTP tetikleyici yok. Örnek Proje düzenleme istemcisi zamanlama tetikleyicisini kullanır:

```cs
public static async Task CronJob(
    [TimerTrigger("0 */2 * * * *")] TimerInfo timer,
    [OrchestrationClient] DurableOrchestrationClient client,
    ILogger logger)
{
  ...
}
```

### <a name="http-management-api"></a>HTTP yönetim API'si

HTTP tetikleyici yok olduğundan, Web işleri SDK'sı yoktur [HTTP yönetim API'si](durable-functions-http-api.md).

WebJobs SDK projesinde, HTTP istekleri göndermek yerine düzenleme istemci nesnesi üzerinde yöntemleri çağırabilir. Aşağıdaki yöntemlerden HTTP yönetim API'si ile yapabileceğiniz üç görev karşılık gelir:

* `GetStatusAsync`
* `RaiseEventAsync`
* `TerminateAsync`

Örnek Proje düzenleme istemci işlevinde orchestrator işlevi başlar ve ardından çağıran bir döngüye giriyor `GetStatusAsync` her 2 saniyede:

```cs
string instanceId = await client.StartNewAsync(nameof(HelloSequence), input: null);
logger.LogInformation($"Started new instance with ID = {instanceId}.");

DurableOrchestrationStatus status;
while (true)
{
    status = await client.GetStatusAsync(instanceId);
    logger.LogInformation($"Status: {status.RuntimeStatus}, Last update: {status.LastUpdatedTime}.");

    if (status.RuntimeStatus == OrchestrationRuntimeStatus.Completed ||
        status.RuntimeStatus == OrchestrationRuntimeStatus.Failed ||
        status.RuntimeStatus == OrchestrationRuntimeStatus.Terminated)
    {
        break;
    }

    await Task.Delay(TimeSpan.FromSeconds(2));
}
```

## <a name="run-the-sample"></a>Örneği çalıştırma

Bu bölüm, nasıl çalıştırılacağı hakkında genel bakış sağlar. [kodunuzla](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/webjobssdk/chaining). WebJobs SDK proje yerel olarak çalıştırma ve için bir Azure WebJob dağıtma açıklayan ayrıntılı yönergeler için bkz. [WebJobs SDK ile çalışmaya başlama](../../app-service/webjobs-sdk-get-started.md#deploy-as-a-webjob).

### <a name="run-locally"></a>Yerel olarak çalıştırma

1. Depolama öykünücüsü çalıştığından emin olun (bkz [önkoşulları](#prerequisites)).

1. Yerel olarak çalıştırdığınızda, Application Insights günlüklerine görmek istiyorsanız:

    a. Uygulama türü bir Application Insights kaynağı oluşturma **genel**.

    b. İzleme anahtarını kaydetmek *App.config* dosya.

1. Projeyi çalıştırın.

### <a name="run-in-azure"></a>Azure'da çalıştırın

1. Bir web uygulaması ve depolama hesabı oluşturun.

1. Web uygulamasında AzureWebJobsStorage adlı bir uygulama ayarı depolama bağlantı dizesini kaydedin.

1. Uygulama türü bir Application Insights kaynağı oluşturma **genel**.

1. İzleme anahtarı appınsıghts_ınstrumentatıonkey adlı bir uygulama ayarı kaydedin.

1. Bir WebJob olarak dağıtın.

## <a name="webjobs-sdk-3x"></a>Web işleri SDK'sı 3.x

3.x tarafından sunulan ana değişiklik .NET Core, .NET Framework yerine kullanılır. 3.x projesi oluşturmak için yönergeler, bu özel durumları ile aynıdır:

1. .NET Core konsol uygulaması oluşturacaksınız. Visual Studio **yeni proje** iletişim kutusunda **.NET Core > konsol uygulaması (.NET Core)**. Proje dosyasını belirten `TargetFramework` olduğu `netcoreapp2.0`.

1. Yayın öncesi sürümü seçin şu paketlerin 3.x:

    * `Microsoft.Azure.WebJobs.Extensions`
    * `Microsoft.Azure.WebJobs.Logging.ApplicationInsights`

1. Değişiklik `Main` yöntemi kod depolama bağlantı dizesini ve Application Insights izleme anahtarını almak için bir *appsettings.json* dosya, .NET Core yapılandırma çerçevesini kullanarak.  Bir örneği aşağıda verilmiştir:

   ```cs
   static void Main(string[] args)
   {
       var builder = new ConfigurationBuilder()
           .SetBasePath(Directory.GetCurrentDirectory())
           .AddJsonFile("appsettings.json");

       var appSettingsConfig = builder.Build();

       using (var loggerFactory = new LoggerFactory())
       {
           var config = new JobHostConfiguration();

           config.DashboardConnectionString = "";
           config.StorageConnectionString =
               appSettingsConfig.GetConnectionString("AzureWebJobsStorage");
           var instrumentationKey =
               appSettingsConfig["APPINSIGHTS_INSTRUMENTATIONKEY"];

           config.LoggerFactory = loggerFactory
               .AddApplicationInsights(instrumentationKey, null)
               .AddConsole();

           config.UseTimers();
           config.UseDurableTask(new DurableTaskExtension
           {
               HubName = "MyTaskHub",
           });
           var host = new JobHost(config);
           host.RunAndBlock();
       }
   }
   ```

## <a name="next-steps"></a>Sonraki adımlar

Web işleri SDK'sı hakkında daha fazla bilgi için bkz. [Web işleri SDK'sını kullanmayı](../../app-service/webjobs-sdk-how-to.md).