---
title: Dayanıklı işlevleri - Azure WebJobs olarak çalıştırma
description: Kod ve WebJobs Web işleri SDK'si kullanarak çalıştırmak için dayanıklı işlevleri yapılandırma hakkında bilgi edinin.
services: functions
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/25/2018
ms.author: azfuncdf
ms.openlocfilehash: 3fc84d1492d2855ffa3bb5538226da049a928339
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="how-to-run-durable-functions-as-webjobs"></a>Dayanıklı işlevleri WebJobs olarak çalıştırma

[Azure işlevleri](functions-overview.md) ve [dayanıklı işlevleri](durable-functions-overview.md) uzantısı üzerinde oluşturulan [WebJobs SDK](../app-service/web-sites-create-web-jobs.md). `JobHost` WebJobs SDK'ın Azure işlevleri çalışma zamanı kapsamı. Denetime ihtiyacınız varsa `JobHost` davranışı yolla Azure işlevlerinde mümkün değildir, geliştirmek ve dayanıklı işlevleri kendiniz WebJobs SDK kullanarak çalıştırın. Bir Azure WebJob içinde dayanıklı işlevlerinizi çalıştırabilirsiniz veya herhangi bir yerden bir konsol uygulaması çalıştırır.

Zincirleme dayanıklı işlevleri örnek bir Web işleri SDK'si sürümde kullanılabilir: indirme veya kopyalama [dayanıklı işlevleri deposu](https://github.com/azure/azure-functions-durable-extension/) gidin *örnekleri\\webjobssdk\\Zincirleme* klasör.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, C# sınıf kitaplığı geliştirme için Azure işlevleri ve dayanıklı işlevleri WebJobs SDK temelleri alışık olduğunuz varsayılır. Bu konularda giriş gerekirse, aşağıdaki kaynaklara bakın:

* [WebJobs SDK ile çalışmaya başlama](../app-service/webjobs-sdk-get-started.md)
* [Visual Studio kullanarak ilk işlevinizi oluşturma](functions-create-your-first-function-visual-studio.md)
* [Dayanıklı işlevleri](durable-functions-sequence.md)

Bu makaledeki adımları tamamlamak için:

* [Visual Studio 2017 15.6 veya sonraki bir sürümü yüklemeniz](https://docs.microsoft.com/visualstudio/install/) ile **Azure geliştirme** iş yükü.

  Zaten Visual Studio olan ancak iş yükü yok, iş yükü seçerek eklemek **Araçlar > alma araçları ve özelliklerinin**. 

  (Kullanabileceğiniz [Visual Studio Code](https://code.visualstudio.com/) bunun yerine, ancak bazı yönergeler için Visual Studio belirli.)

* Yükleme ve çalıştırma [Azure Storage öykünücüsü](../storage/common/storage-use-emulator.md) 5.2 veya sonraki bir sürümü. Güncelleştirilecek alternatiftir *App.config* Azure depolama bağlantı dizesini içeren bir dosya.

## <a name="webjobs-sdk-versions"></a>WebJobs SDK sürümleri

Bu makalede, bir Web işleri SDK'si 2.x proje geliştirmek açıklanmaktadır (Azure işlevleri sürüm eşdeğer 1.x). Sürüm hakkında bilgi için 3.x bkz [WebJobs SDK 3.x](#webjobs-sdk-3x) bu makalenin ilerisinde yer. 

## <a name="create-console-app"></a>Konsol uygulaması oluşturma

Yeni bir konsol uygulama projesi yüklü uygun NuGet paketleri ile Web işleri SDK'si projesidir.

Visual Studio'da **yeni proje** iletişim kutusunda **Windows Klasik Masaüstü > konsol uygulaması (.NET Framework)**. Proje dosyasında `TargetFrameworkVersion` olmalıdır `v4.6.1`.

Visual Studio ayrıca seçerek kullanabileceğiniz bir Web işi proje şablonu olan **bulut > Azure WebJob (.NET Framework)**. Bu şablon bazıları ihtiyacınız olmayan birçok paketleri yükler.

## <a name="install-nuget-packages"></a>NuGet paketlerini yükleme

WebJobs SDK, çekirdek bağlamaları, günlük framework ve dayanıklı görev uzantısı için NuGet paketlerini gerekir. Burada **Paket Yöneticisi Konsolu** komutları bu makalenin yazıldığı tarih itibariyle en yeni kararlı sürüm numaralarıyla bu paketleri için:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions -version 2.2.0
Install-Package Microsoft.Extensions.Logging -version 2.0.1
Install-Package Microsoft.Azure.WebJobs.Extensions.DurableTask -version 1.4.0
```

Ayrıca günlüğe kaydetme sağlayıcıları gerekir. Aşağıdaki komutları Application Insights Sağlayıcısı'nı yüklemek ve `ConfigurationManager`. `ConfigurationManager` Uygulama ayarlarından Application Insights izleme anahtarı alabilmenizi sağlar.

```powershell
Install-Package Microsoft.Azure.WebJobs.Logging.ApplicationInsights -version 2.2.0
Install-Package System.Configuration.ConfigurationManager -version 4.4.1
```

Aşağıdaki komutu Konsolu sağlayıcısı yükler:

```powershell
Install-Package Microsoft.Extensions.Logging.Console -version 2.0.1
```

## <a name="jobhost-code"></a>JobHost kodu

Dayanıklı işlevleri uzantısını kullanmak için arama `UseDurableTask` üzerinde `JobHostConfiguration` nesnesinde, `Main` yöntemi:

```cs
var config = new JobHostConfiguration();
config.UseDurableTask(new DurableTaskExtension
{
    HubName = "MyTaskHub",
};
```

Ayarlayabilirsiniz özelliklerinin bir listesi için `DurableTaskExtension` nesne için bkz: [host.json](functions-host-json.md#durabletask).

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

Karşılaştırılan WebJobs SDK işlevleri için Azure işlevleri hizmeti için yazmak üzere yazdığınız kodları bazı farklılıklar vardır.

WebJobs SDK aşağıdaki Azure işlevleri özellikleri desteklemez:

* [FunctionName özniteliği](#functionname-attribute)
* [HTTP tetikleyici](#http-trigger)
* [Dayanıklı işlevleri HTTP yönetim API'si](#http-management-api)

### <a name="functionname-attribute"></a>FunctionName özniteliği

Bir Web işleri SDK'si projesinde yöntemi işlevinin işlev adı addır. `FunctionName` Özniteliği yalnızca Azure işlevlerinde kullanılır.

### <a name="http-trigger"></a>HTTP tetikleyicisi

WebJobs SDK, bir HTTP tetikleyicisi yok. Örnek projenin orchestration istemci Zamanlayıcı tetikleyicisi kullanır:

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

WebJobs SDK sahip olmayan bir HTTP tetikleyicisi içerdiğinden, [HTTP yönetim API'si](durable-functions-http-api.md).

Bir Web işleri SDK'si projesinde HTTP istekleri göndermek yerine orchestration istemci nesnesindeki yöntemleri çağırabilir. Aşağıdaki yöntemlerden HTTP yönetim API'si ile gerçekleştirebileceğiniz üç görevleri karşılık gelir:

* `GetStatusAsync`
* `RaiseEventAsync`
* `TerminateAsync`

Örnek Proje orchestration istemci işlevinde orchestrator işlevi başlar ve ardından çağıran bir döngüye girer `GetStatusAsync` 2 saniyede:

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

Bu bölümde nasıl çalıştırılacağını genel bakış [örnek proje](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/webjobssdk/chaining). Web işleri SDK'si projeyi yerel olarak çalıştırın ve bir Azure Web işine dağıtma açıklayan ayrıntılı yönergeler için bkz: [WebJobs SDK ile çalışmaya başlama](../app-service/webjobs-sdk-get-started.md#deploy-as-a-webjob).

### <a name="run-locally"></a>Yerel olarak çalıştırma

1. Depolama öykünücüsü çalıştığından emin olun (bkz [Önkoşullar](#prerequisites)).

1. Yerel olarak çalıştırdığınızda, Application Insights günlüklerinde görmek istiyorsanız:

  a. Uygulama türü bir Application Insights kaynağı oluşturma **genel**.

  b. İzleme anahtarı Kaydet *App.config* dosya.

1. Projeyi çalıştırın.

### <a name="run-in-azure"></a>Azure'da çalıştırma

1. Bir web uygulaması ve bir depolama hesabı oluşturun.

1. Web uygulamasında AzureWebJobsStorage adlı bir uygulama ayarı depolama bağlantı dizesini kaydedin.

1. Uygulama türü bir Application Insights kaynağı oluşturma **genel**.

1. İzleme anahtarı appınsıghts_ınstrumentatıonkey adlı bir uygulama ayarı kaydedin.

1. Bir Web işi dağıtın.

## <a name="webjobs-sdk-3x"></a>Web işleri SDK'si 3.x

3.x tarafından sunulan ana değişiklik .NET Core .NET Framework yerine kullanılır. 3.x projesi oluşturmak için yönergeler, bu özel durumları ile aynıdır:

1. Bir .NET Core konsol uygulaması oluşturun. Visual Studio'da **yeni proje** iletişim kutusunda **.NET Core > konsol uygulaması (.NET Core)**. Proje dosyası belirleyen `TargetFramework` olan `netcoreapp2.0`.

1. Sürüm öncesi sürümünü seçin 3.x aşağıdaki paketleri:

  * `Microsoft.Azure.WebJobs.Extensions`
  * `Microsoft.Azure.WebJobs.Logging.ApplicationInsights`

2. Değişiklik `Main` yöntemi kodu depolama bağlantı dizesini ve Application Insights izleme anahtarı almak için bir *appsettings.json* dosya, .NET Core yapılandırma framework kullanarak.  Bir örneği aşağıda verilmiştir:

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

WebJobs SDK hakkında daha fazla bilgi için bkz: [WebJobs SDK kullanmayı](../app-service/webjobs-sdk-how-to.md).

