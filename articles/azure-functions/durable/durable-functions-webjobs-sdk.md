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
ms.openlocfilehash: df12639aaafaf3df7ae2b755d635d4fba83d846e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60648661"
---
# <a name="how-to-run-durable-functions-as-webjobs"></a>Dayanıklı işlevler Web işleri çalıştırma

Varsayılan olarak, Azure işlevleri çalışma zamanı ana bilgisayarı düzenlemeleri için dayanıklı işlevler kullanır. Ancak, daha olayları dinleyen kod üzerinde denetlediğiniz belirli senaryolar olabilir. Bu makalede nasıl Web işleri SDK'sını kullanarak, orchestration uygulanacağı gösterilmektedir. İşlevleri ve WebJobs arasında daha ayrıntılı bir karşılaştırması için bkz [karşılaştırma işlevleri ve WebJobs](../functions-compare-logic-apps-ms-flow-webjobs.md#compare-functions-and-webjobs).

[Azure işlevleri](../functions-overview.md) ve [dayanıklı işlevler](durable-functions-overview.md) uzantı üzerinde oluşturulan [WebJobs SDK](../../app-service/webjobs-sdk-how-to.md). WebJobs SDK proje konak, Azure işlevleri'nde çalışma zamanıdır. Azure işlevleri'nde mümkün olmayan yöntemlerden davranışını denetlemek gerekiyorsa, geliştirin ve dayanıklı işlevler kendiniz WebJobs SDK kullanarak çalıştırın.

Sürümünde 3.x Web işleri SDK'sının konak uygulaması olduğu `IHost`ve sürüm 2.x kullanmanız `JobHost` nesne.

Zincirleme dayanıklı işlevler örnek bir Web işleri SDK 2.x sürümde kullanılabilir: indirin veya kopyalayın [dayanıklı işlevler depo](https://github.com/azure/azure-functions-durable-extension/)ve Git *örnekleri\\webjobssdk\\Zincirleme* klasör.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, C# sınıf kitaplığı geliştirme için Azure işlevleri ve dayanıklı işlevler Web işleri SDK'ın temel alışık olduğunuz varsayılır. Bu konulara bir giriş gerekiyorsa, aşağıdaki kaynaklara bakın:

* [WebJobs SDK ile çalışmaya başlama](../../app-service/webjobs-sdk-get-started.md)
* [Visual Studio kullanarak ilk işlevinizi oluşturma](../functions-create-your-first-function-visual-studio.md)
* [Dayanıklı İşlevler](durable-functions-sequence.md)

Bu makaledeki adımları tamamlayabilmeniz için:

* [Visual Studio 2017 sürüm 15.6 veya üzerini yükleyin](https://docs.microsoft.com/visualstudio/install/) ile **Azure geliştirme** iş yükü.

  Zaten Visual Studio yüklü ancak bu iş yükü yüklü değilse, iş yükü seçerek ekleyin **Araçları** > **araçları ve özellikleri Al**.

  (Kullanabilirsiniz [Visual Studio Code](https://code.visualstudio.com/) bunun yerine, ancak alan yönergelerden bazılarını Visual Studio'ya özgüdür.)

* Yükleme ve çalıştırma [Azure Storage öykünücüsü](../../storage/common/storage-use-emulator.md) 5.2 veya sonraki bir sürümü. Güncelleştirilecek alternatiftir *App.config* bir Azure depolama bağlantı dizesi içeren dosya.

## <a name="webjobs-sdk-versions"></a>WebJobs SDK sürümleri

Bu makalede, WebJobs SDK 2.x projeyi geliştirmek üzere açıklanmaktadır (Azure işlevleri sürüm eşdeğer 1.x). Sürümü hakkında daha fazla bilgi için 3.x bkz [WebJobs SDK 3.x](#webjobs-sdk-3x) bu makalenin ilerleyen bölümlerinde.

## <a name="create-a-console-app"></a>Konsol uygulaması oluşturma

Dayanıklı işlevler Web işleri çalıştırmak için bir konsol uygulaması oluşturmanız gerekir. Yeni bir konsol uygulama projesi yüklü uygun NuGet paketleri ile bir Web işleri SDK'sı projesidir.

Visual Studio **yeni proje** iletişim kutusunda **Windows Klasik Masaüstü** > **konsol uygulaması (.NET Framework)**. Proje dosyasında `TargetFrameworkVersion` olmalıdır `v4.6.1`.

Visual Studio seçerek kullanabileceğiniz bir WebJob proje şablonunu da olan **bulut** > **Azure webjob'ı (.NET Framework)**. Bu şablon, bazıları gerekmeyen birçok paketi yükler.

## <a name="install-nuget-packages"></a>NuGet paketlerini yükleme

WebJobs SDK, çekirdek bağlamaları, günlüğe kaydetme çerçevesi ve dayanıklı görev uzantısı için NuGet paketleri gerekir. İşte **Paket Yöneticisi Konsolu** bu paketleri, bu makalenin yazıldığı tarih itibarıyla en son kararlı sürüm numaraları için komutları:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions -version 2.2.0
Install-Package Microsoft.Extensions.Logging -version 2.0.1
Install-Package Microsoft.Azure.WebJobs.Extensions.DurableTask -version 1.4.0
```

Ayrıca günlük sağlayıcıları gerekir. Aşağıdaki komutlar Azure Application Insights sağlayıcıyı yükleyin ve `ConfigurationManager`. `ConfigurationManager` Application Insights izleme anahtarı uygulama ayarlarından alabilmenizi sağlar.

```powershell
Install-Package Microsoft.Azure.WebJobs.Logging.ApplicationInsights -version 2.2.0
Install-Package System.Configuration.ConfigurationManager -version 4.4.1
```

Aşağıdaki komut, konsol sağlayıcısını yükler:

```powershell
Install-Package Microsoft.Extensions.Logging.Console -version 2.0.1
```

## <a name="jobhost-code"></a>JobHost kod

Konsol uygulaması oluşturan ve NuGet paketleri yüklü gereksinim, dayanıklı işlevler kullanmaya hazırsınız. JobHost kod kullanarak bunu yapın.

Dayanıklı işlevler uzantısını kullanmak için çağrı `UseDurableTask` üzerinde `JobHostConfiguration` nesnesine, `Main` yöntemi:

```cs
var config = new JobHostConfiguration();
config.UseDurableTask(new DurableTaskExtension
{
    HubName = "MyTaskHub",
};
```

İçinde ayarlayabildiğiniz özellikleri listesini `DurableTaskExtension` nesne, bkz: [host.json](../functions-host-json.md#durabletask).

`Main` Yöntemi yerdir de günlüğe kaydetme sağlayıcılarını kurma ayarlamak için. Aşağıdaki örnek konsol ve Application Insights sağlayıcılarını yapılandırır.

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

Dayanıklı işlevler Web işleri bağlamında farklı dayanıklı işlevler davranışından Azure işlevleri bağlamında. Kodunuzu yazarken gözlemlenen farkları bilmeniz önemlidir.

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

WebJobs SDK projesinde, HTTP isteği göndererek yerine düzenleme istemci nesnesi üzerinde yöntemleri çağırabilir. Aşağıdaki yöntemlerden HTTP yönetim API'si ile yapabileceğiniz üç görev karşılık gelir:

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

Dayanıklı işlevler bir WebJob olarak çalıştırmak üzere ayarlanan sahte kendinizi ve artık bu Azure işlevleri bağımsız dayanıklı işlevler çalışmasını nasıl değişir anlamak vardır. Bu noktada, örnek olarak çalışması görmeye yardımcı olabilir.

Bu bölüm, nasıl çalıştırılacağı hakkında genel bakış sağlar. [kodunuzla](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/webjobssdk/chaining). WebJobs SDK proje yerel olarak çalıştırma ve için bir Azure WebJob dağıtma açıklayan ayrıntılı yönergeler için bkz. [WebJobs SDK ile çalışmaya başlama](../../app-service/webjobs-sdk-get-started.md#deploy-as-a-webjob).

### <a name="run-locally"></a>Yerel olarak çalıştırma

1. Depolama öykünücüsü çalıştığından emin olun (bkz [önkoşulları](#prerequisites)).

1. Projeyi yerel olarak çalıştırdığınızda, Application Insights günlüklerine görmek istiyorsanız:

    a. Application Insights kaynağı oluşturun ve kullanın **genel** onun için uygulama türü.

    b. İzleme anahtarını kaydetmek *App.config* dosya.

1. Projeyi çalıştırın.

### <a name="run-in-azure"></a>Azure'da çalıştırın

1. Bir web uygulaması ve depolama hesabı oluşturun.

1. Web uygulamasında adlı ayar uygulama depolama bağlantı dizesini kaydedin `AzureWebJobsStorage`.

1. Application Insights kaynağı oluşturun ve kullanın **genel** onun için uygulama türü.

1. İzleme anahtarını adlı ayar bir uygulamada Kaydet `APPINSIGHTS_INSTRUMENTATIONKEY`.

1. Bir WebJob olarak dağıtın.

## <a name="webjobs-sdk-3x"></a>Web işleri SDK'sı 3.x

Bu makalede, WebJobs SDK 2.x projeyi geliştirmek açıklanmaktadır. Geliştiriyorsanız bir [WebJobs SDK 3.x](../../app-service/webjobs-sdk-get-started.md) proje, bu bölümde, farkları olur.

Tanıtılan ana değişiklik .NET Core, .NET Framework yerine kullanılır. WebJobs SDK 3.x projesi oluşturmak için yönergeler bu özel durumları ile aynıdır:

1. .NET Core konsol uygulaması oluşturacaksınız. Visual Studio **yeni proje** iletişim kutusunda **.NET Core** > **konsol uygulaması (.NET Core)**. Proje dosyasını belirten `TargetFramework` olduğu `netcoreapp2.x`.

1. Web işleri SDK'sı sürümünü seçin şu paketlerin 3.x:

    * `Microsoft.Azure.WebJobs.Extensions`
    * `Microsoft.Azure.WebJobs.Extensions.Storage`
    * `Microsoft.Azure.WebJobs.Logging.ApplicationInsights`

1. Depolama bağlantı dizesini ve Application Insights izleme anahtarını ayarlayın bir *appsettings.json* kullanarak .NET Core framework yapılandırma dosyası. Bir örneği aşağıda verilmiştir:

    ```json
        {
            "AzureWebJobsStorage": "<replace with storage connection string>",
            "APPINSIGHTS_INSTRUMENTATIONKEY": "<replace with Application Insights instrumentation key>"
        }
    ```

1. Değişiklik `Main` Bunu yapmak için yöntemi kodu. Bir örneği aşağıda verilmiştir:

   ```cs
   static void Main(string[] args)
   {
        var hostBuilder = new HostBuilder()
            .ConfigureWebJobs(config =>
            {
                config.AddAzureStorageCoreServices();
                config.AddAzureStorage();
                config.AddTimers();
                config.AddDurableTask(options =>
                {
                    options.HubName = "MyTaskHub";
                    options.AzureStorageConnectionStringName = "AzureWebJobsStorage";
                });
            })
            .ConfigureLogging((context, logging) =>
            {
                logging.AddConsole();
                logging.AddApplicationInsights(config =>
                {
                    config.InstrumentationKey = context.Configuration["APPINSIGHTS_INSTRUMENTATIONKEY"];
                });
            })
            .UseConsoleLifetime();

        var host = hostBuilder.Build();

        using (host)
        {
            host.Run();
        }
   }
   ```

## <a name="next-steps"></a>Sonraki adımlar

Web işleri SDK'sı hakkında daha fazla bilgi için bkz. [Web işleri SDK'sını kullanmayı](../../app-service/webjobs-sdk-how-to.md).
