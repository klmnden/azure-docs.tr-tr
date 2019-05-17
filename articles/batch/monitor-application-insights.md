---
title: Batch Azure Application Insights ile izleme | Microsoft Docs
description: Azure Application Insights Kitaplığı'nı kullanarak bir Azure Batch .NET uygulama izleme hakkında bilgi edinin.
services: batch
author: laurenhughes
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: .NET
ms.topic: article
ms.workload: na
ms.date: 04/05/2018
ms.author: lahugh
ms.openlocfilehash: c527b0b10a2b9a351b242d0858fdbe64687970a7
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65595295"
---
# <a name="monitor-and-debug-an-azure-batch-net-application-with-application-insights"></a>İzleme ve Application Insights ile bir Azure Batch .NET uygulama hatalarını ayıklama

[Application Insights](../azure-monitor/app/app-insights-overview.md) geliştiriciler Azure Hizmetleri için dağıtılan izleme ve hata ayıklama uygulamaları için zarif ve güçlü bir yol sağlar. Application Insights izleme performans sayaçları ve özel durumların yanı sıra gereç özel Ölçümler ve izleme ile kodunuzu kullanın. Application Insights ile Azure toplu işlem uygulamanızı tümleştirme davranışları derin Öngörüler elde edin ve sorunları neredeyse gerçek zamanlı olarak araştırmak sağlar.

Bu makalede, ekleme, Azure Batch .NET çözümünüze Application Insights Kitaplığı'nı yapılandırma ve uygulama kodunuz izleme gösterilmektedir. Ayrıca, uygulamanızı Azure portal aracılığıyla izlemek ve özel panolar oluşturmak için yol gösterir. Application Insights'ı diğer dillerde desteği sağlamak için bakmak [dilleri, platformlar ve tümleştirmeler belgeleri](../azure-monitor/app/platforms.md).

Bu makalede eşlik eden kodunu içeren bir örnek C# çözüm edinilebilir [GitHub](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ApplicationInsights). Bu örnek için Application Insights izleme kodu ekler [TopNWords](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords) örnek. Bu örnek oluşturma ve TopNWords ilk çalıştırma deneyin bilmiyorsanız. Bunun yapılması, bir dizi bir giriş bloblarını birden çok işlem düğümlerinde paralel işleme temel bir Batch workflow anlamanıza yardımcı olur. 

> [!TIP]
> Alternatif olarak, Batch Gezgini içinde VM performans sayaçları gibi Application Insights verilerini görüntülemek için Batch çözümünüzü yapılandırın. [Batch Explorer](https://github.com/Azure/BatchExplorer) oluşturma, hata ayıklama ve izleme Azure Batch uygulamalarıyla yardımcı olmak için bir ücretsiz ve zengin özellikli tek başına istemci aracıdır. Mac, Linux veya Windows için [yükleme paketi](https://azure.github.io/BatchExplorer/) indirebilirsiniz. Bkz: [batch ınsights depo](https://github.com/Azure/batch-insights) için Application Insights verilerini Batch Explorer'ı etkinleştirmek hızlı adımlar. 
>

## <a name="prerequisites"></a>Önkoşullar
* [Visual Studio 2017 veya üstü](https://www.visualstudio.com/vs)

* [Batch hesabı ve bağlı depolama hesabı](batch-account-create-portal.md)

* [Application Insights kaynağı](../azure-monitor/app/create-new-resource.md )
  
   * Bir Application Insights'ı oluşturmak için Azure portal'ı kullanmanızı *kaynak*. Seçin *genel* **uygulama türü**.

   * Kopyalama [izleme anahtarını](../azure-monitor/app/create-new-resource.md #copy-the-instrumentation-key) portalından. Bu makalenin sonraki bölümlerinde gerekir.
  
  > [!NOTE]
  > Olabilir [ücret](https://azure.microsoft.com/pricing/details/application-insights/) uygulama anlayışları'nda depolanan veriler için. Bu, tanılama ve izleme bu makalede ele alınan verileri içerir.
  > 

## <a name="add-application-insights-to-your-project"></a>Projenize Application Insights ekleyin

**Microsoft.applicationınsights.windowsserver** NuGet paketi ve bağımlılıkları projeniz için gerekli. Ekleme veya bunları, uygulamanızın projesine geri yükleyin. Paketi yüklemek için kullanmak `Install-Package` komut veya NuGet Paket Yöneticisi.

```powershell
Install-Package Microsoft.ApplicationInsights.WindowsServer
```
Başvuru Application Insights .NET uygulamanızı kullanarak **Microsoft.applicationınsights** ad alanı.

## <a name="instrument-your-code"></a>Kodunuzu izleme

Kodunuzu izleme için Application Insights'ı oluşturmak, çözümünüzün gerekir [TelemetryClient](/dotnet/api/microsoft.applicationinsights.telemetryclient). Bu örnekte TelemetryClient yapılandırmasını yükler [Applicationınsights.config](../azure-monitor/app/configuration-with-applicationinsights-config.md) dosya. Applicationınsights.config aşağıdaki projeleri ile Application Insights izleme anahtarınızı güncelleştirdiğinizden emin olun: Microsoft.Azure.Batch.Samples.TelemetryStartTask ve TopNWordsSample.

```xml
<InstrumentationKey>YOUR-IKEY-GOES-HERE</InstrumentationKey>
```
Ayrıca dosyasında TopNWords.cs izleme anahtarını ekleyin.

Aşağıdaki örnekte TopNWords.cs içinde [izleme çağrıları](../azure-monitor/app/api-custom-events-metrics.md) Application Insights API'si:
* `TrackMetric()` -Nasıl uzun, ortalama, gerekli bir metin dosyasını indirmek için bir işlem düğümünde alan izler.
* `TrackTrace()` -Kodunuzda hata ayıklama çağrıları ekler.
* `TrackEvent()` -Olayları yakalamak için ilgi çekici izler.

Bu örnekte, bilerek kullanıma özel durum işleme bırakır. Bunun yerine, Application Insights raporlarını otomatik olarak işlenmeyen özel durumları, hata ayıklama deneyimini önemli ölçüde artırır. 

Aşağıdaki kod parçacığı bu yöntemlerin nasıl kullanılacağını gösterir.

```csharp
public void CountWords(string blobName, int numTopN, string storageAccountName, string storageAccountKey)
{
    // simulate exception for some set of tasks
    Random rand = new Random();
    if (rand.Next(0, 10) % 10 == 0)
    {
        blobName += ".badUrl";
    }

    // log the url we are downloading the file from
    insightsClient.TrackTrace(new TraceTelemetry(string.Format("Task {0}: Download file from: {1}", this.taskId, blobName), SeverityLevel.Verbose));

    // open the cloud blob that contains the book
    var storageCred = new StorageCredentials(storageAccountName, storageAccountKey);
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(blobName), storageCred);
    using (Stream memoryStream = new MemoryStream())
    {
        // calculate blob download time
        DateTime start = DateTime.Now;
        blob.DownloadToStream(memoryStream);
        TimeSpan downloadTime = DateTime.Now.Subtract(start);

        // track how long the blob takes to download on this node
        // this will help debug timing issues or identify poorly performing nodes
        insightsClient.TrackMetric("Blob download in seconds", downloadTime.TotalSeconds, this.CommonProperties);

        memoryStream.Position = 0; //Reset the stream
        var sr = new StreamReader(memoryStream);
        var myStr = sr.ReadToEnd();
        string[] words = myStr.Split(' ');

        // log how many words were found in the text file
        insightsClient.TrackTrace(new TraceTelemetry(string.Format("Task {0}: Found {1} words", this.taskId, words.Length), SeverityLevel.Verbose));
        var topNWords =
            words.
                Where(word => word.Length > 0).
                GroupBy(word => word, (key, group) => new KeyValuePair<String, long>(key, group.LongCount())).
                OrderByDescending(x => x.Value).
                Take(numTopN).
                ToList();
        foreach (var pair in topNWords)
        {
            Console.WriteLine("{0} {1}", pair.Key, pair.Value);
        }

        // emit an event to track the completion of the task
        insightsClient.TrackEvent("Done counting words");
    }
}
```

### <a name="azure-batch-telemetry-initializer-helper"></a>Azure Batch telemetri başlatıcısını Yardımcısı
Verilen sunucu ve örneği için telemetri raporlama, Application Insights Azure VM rolü ve VM adını için varsayılan değerleri kullanır. Azure Batch bağlamında örnek havuz adı ve bunun yerine işlem düğümü adı nasıl kullanılacağını gösterir. Kullanım bir [telemetri başlatıcısını](../azure-monitor/app/api-filtering-sampling.md#add-properties) varsayılan değerlerini geçersiz kılması için. 

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;
using System;
using System.Threading;

namespace Microsoft.Azure.Batch.Samples.TelemetryInitializer
{
    public class AzureBatchNodeTelemetryInitializer : ITelemetryInitializer
    {
        // Azure Batch environment variables
        private const string PoolIdEnvironmentVariable = "AZ_BATCH_POOL_ID";
        private const string NodeIdEnvironmentVariable = "AZ_BATCH_NODE_ID";

        private string roleInstanceName;
        private string roleName;

        public void Initialize(ITelemetry telemetry)
        {
            if (string.IsNullOrEmpty(telemetry.Context.Cloud.RoleName))
            {
                // override the role name with the Azure Batch Pool name
                string name = LazyInitializer.EnsureInitialized(ref this.roleName, this.GetPoolName);
                telemetry.Context.Cloud.RoleName = name;
            }

            if (string.IsNullOrEmpty(telemetry.Context.Cloud.RoleInstance))
            {
                // override the role instance with the Azure Batch Compute Node name
                string name = LazyInitializer.EnsureInitialized(ref this.roleInstanceName, this.GetNodeName);
                telemetry.Context.Cloud.RoleInstance = name;
            }
        }

        private string GetPoolName()
        {
            return Environment.GetEnvironmentVariable(PoolIdEnvironmentVariable) ?? string.Empty;
        }

        private string GetNodeName()
        {
            return Environment.GetEnvironmentVariable(NodeIdEnvironmentVariable) ?? string.Empty;
        }
    }
}
```

Telemetri başlatıcısını etkinleştirileceği TopNWordsSample proje Applicationınsights.config dosyasında aşağıdakileri içerir:

```xml
<TelemetryInitializers>
    <Add Type="Microsoft.Azure.Batch.Samples.TelemetryInitializer.AzureBatchNodeTelemetryInitializer, Microsoft.Azure.Batch.Samples.TelemetryInitializer"/>
</TelemetryInitializers>
``` 

## <a name="update-the-job-and-tasks-to-include-application-insights-binaries"></a>İş ve görevleri Application Insights ikili dosyalarını içerecek şekilde güncelleştirin

Application Insights'ın doğru şekilde, işlem düğümleri üzerinde çalışmak için sırada ikili dosyaları doğru şekilde yerleştirildiğinden emin olun. Göreviniz yürütür anda indirilen, böylece gerekli ikili dosyaları, görevin kaynak dosyaları koleksiyonuna ekleyin. Aşağıdaki kod parçacıklarının Job.cs kodda benzer.

İlk olarak, Application Insights dosyaları karşıya yüklemek için statik bir liste oluşturur.

```csharp
private static readonly List<string> AIFilesToUpload = new List<string>()
{
    // Application Insights config and assemblies
    "ApplicationInsights.config",
    "Microsoft.ApplicationInsights.dll",
    "Microsoft.AI.Agent.Intercept.dll",
    "Microsoft.AI.DependencyCollector.dll",
    "Microsoft.AI.PerfCounterCollector.dll",
    "Microsoft.AI.ServerTelemetryChannel.dll",
    "Microsoft.AI.WindowsServer.dll",
    
    // custom telemetry initializer assemblies
    "Microsoft.Azure.Batch.Samples.TelemetryInitializer.dll",
 };
...
```

Ardından, görev tarafından kullanılan hazırlama dosyalarını oluşturun.
```csharp
...
// create file staging objects that represent the executable and its dependent assembly to run as the task.
// These files are copied to every node before the corresponding task is scheduled to run on that node.
FileToStage topNWordExe = new FileToStage(TopNWordsExeName, stagingStorageAccount);
FileToStage storageDll = new FileToStage(StorageClientDllName, stagingStorageAccount);

// Upload Application Insights assemblies
List<FileToStage> aiStagedFiles = new List<FileToStage>();
foreach (string aiFile in AIFilesToUpload)
{
    aiStagedFiles.Add(new FileToStage(aiFile, stagingStorageAccount));
}
...
```

`FileToStage` Kod örneğinde, bir Azure depolama blobu bir dosyaya yerel diskten kolayca karşıya yüklemenize olanak sağlayan bir yardımcı işlevini yöntemidir. Her dosya daha sonra bir işlem düğümüne yüklenecek ve bir görev tarafından başvurulan.

Son olarak, işe görev ekleme ve gerekli Application Insights ikili dosyalarını içerir.
```csharp
...
// initialize a collection to hold the tasks that will be submitted in their entirety
List<CloudTask> tasksToRun = new List<CloudTask>(topNWordsConfiguration.NumberOfTasks);
for (int i = 1; i <= topNWordsConfiguration.NumberOfTasks; i++)
{
    CloudTask task = new CloudTask("task_no_" + i, String.Format("{0} --Task {1} {2} {3} {4}",
        TopNWordsExeName,
        string.Format("https://{0}.blob.core.windows.net/{1}",
            accountSettings.StorageAccountName,
            documents[i]),
        topNWordsConfiguration.TopWordCount,
        accountSettings.StorageAccountName,
        accountSettings.StorageAccountKey));

    //This is the list of files to stage to a container -- for each job, one container is created and 
    //files all resolve to Azure Blobs by their name (so two tasks with the same named file will create just 1 blob in
    //the container).
    task.FilesToStage = new List<IFileStagingProvider>
                        {
                            // required application binaries
                            topNWordExe,
                            storageDll,
                        };
    foreach (FileToStage stagedFile in aiStagedFiles)
   {
        task.FilesToStage.Add(stagedFile);
   }    
    task.RunElevated = false;
    tasksToRun.Add(task);
}
```

## <a name="view-data-in-the-azure-portal"></a>Azure portalında görünümü verileri

İş ve görevleri Application Insights'ı kullanmak için yapılandırmış olduğunuz, havuzunuzdaki örnek işi çalıştırın. Azure portalına gidin ve sağladığınız Application Insights kaynağını açın. Havuzu sağlandıktan sonra veri akışını ve günlüğe görmeye başlamanız gerekir. Bu makalenin geri kalanında yalnızca birkaç Application Insights özellikleri ele alınır, ancak tüm özellikleri keşfedin çekinmeyin.

### <a name="view-live-stream-data"></a>Canlı akış verileri görüntüle

Uygulamaları Insights kaynağınızın izleme günlüklerini görüntülemek için tıklayın **Canlı Stream**. Aşağıdaki ekran görüntüsünde, örneğin işlem düğümü başına CPU kullanımı havuzdaki işlem düğümlerinden gelen canlı verileri görüntülemek gösterilmektedir.

![Canlı akış işlem düğümünde verileri](./media/monitor-application-insights/applicationinsightslivestream.png)

### <a name="view-trace-logs"></a>İzleme günlüklerini görüntüle

Uygulamaları Insights kaynağınızın izleme günlüklerini görüntülemek için tıklayın **arama**. Bu görünüm, izlemeler, olaylar ve özel durumlar dahil olmak üzere Application Insights tarafından yakalanan Tanılama verileri listesini gösterir. 

Aşağıdaki ekran görüntüsünde, nasıl bir görev için tek bir izleme günlüğe kaydedilir ve daha sonra hata ayıklama amacıyla sorgulanan gösterir.

![İzleme günlükleri görüntüsü](./media/monitor-application-insights/tracelogsfortask.png)

### <a name="view-unhandled-exceptions"></a>İşlenmeyen özel durumları görüntüleyin

Aşağıdaki ekran görüntüleri, Application Insights, uygulamanızdan oluşturulan özel durumları nasıl günlüğe yazacağını gösterir. Bu durumda, özel durumu oluşturan uygulama saniye içinde belirli bir özel durum ayrıntıya ve sorunu tanılayın.

![İşlenmeyen özel durumlar](./media/monitor-application-insights/exception.png)

### <a name="measure-blob-download-time"></a>Ölçü blob karşıdan yükleme süresi

Özel ölçümler de portalında değerli bir araç olan. Örneğin, her işlem düğümü işlemekte olan gerekli bir metin dosyasını indirmek için geçen ortalama süreyi görüntüleyebilirsiniz.

Örnek bir grafik oluşturmak için:
1. Application Insights kaynağınıza tıklayın **ölçüm Gezgini** > **Ekle grafik**.
2. Tıklayın **Düzenle** grafikte eklendi.
2. Grafik ayrıntıları aşağıdaki gibi güncelleştirin:
   * Ayarlama **grafik türü** için **kılavuz**.
   * Ayarlama **toplama** için **ortalama**.
   * Ayarlama **gruplandırma ölçütü** için **nodeId**.
   * İçinde **ölçümleri**seçin **özel** > **saniyeler içinde Blob indirme**.
   * Görüntü ayarlama **renk paletini** seçiminiz için. 

![Düğüm başına BLOB karşıdan yükleme süresi](./media/monitor-application-insights/blobdownloadtime.png)


## <a name="monitor-compute-nodes-continuously"></a>Sürekli izleme işlem düğümleri

Görevler çalıştırılırken performans sayaçları da dahil olmak üzere tüm ölçümleri yalnızca kaydedilir fark etmiş olabilirsiniz. Bu davranış, Application Insights günlükleri veri miktarını sınırlar için yararlıdır. Ancak, her zaman işlem düğümlerine izlemek istediğiniz durumlar vardır. Örneğin, bunlar değil zamanlanan arka plan iş Batch hizmeti ile çalışıyor olabilir. Bu durumda, işlem düğümü ömrü için çalıştırılacak bir izleme işlemini ayarlayın. 

Bu davranışı elde etmek için bir Application Insights kitaplığı yükleyen ve arka planda çalışan bir işlemin üretme yoludur. Örnekte, başlangıç görevi, ikili dosyaları makineye yükler ve süresiz olarak çalışan bir işlemin tutar. Bu işlemin, performans sayaçları gibi ilgilendiğiniz ek veri yaymak Application Insights yapılandırma dosyasını yapılandırın.

```csharp
...
 // Batch start task telemetry runner
private const string BatchStartTaskFolderName = "StartTask";
private const string BatchStartTaskTelemetryRunnerName = "Microsoft.Azure.Batch.Samples.TelemetryStartTask.exe";
private const string BatchStartTaskTelemetryRunnerAIConfig = "ApplicationInsights.config";
...
CloudPool pool = client.PoolOperations.CreatePool(
    topNWordsConfiguration.PoolId,
    targetDedicated: topNWordsConfiguration.PoolNodeCount,
    virtualMachineSize: "standard_d1_v2",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));
...

// Create a start task which will run a dummy exe in background that simply emits performance
// counter data as defined in the relevant ApplicationInsights.config.
// Note that the waitForSuccess on the start task was not set so the Compute Node will be
// available immediately after this command is run.
pool.StartTask = new StartTask()
{
    CommandLine = string.Format("cmd /c {0}", BatchStartTaskTelemetryRunnerName),
    ResourceFiles = resourceFiles
};
...
```

> [!TIP]
> Çözümünüzün yönetilebilirlik artırmak için derleme içinde gruplandırabilirsiniz bir [uygulama paketi](./batch-application-packages.md). Ardından, uygulama paketi havuzlarınızı için otomatik olarak dağıtmak için havuz yapılandırmasına bir uygulama paketi başvurusu ekleyin.
>

## <a name="throttle-and-sample-data"></a>Azaltma ve örnek veriler 

Azure Batch uygulamalarıyla üretimde çalışan büyük ölçekli yapısı nedeniyle, maliyetleri yönetmek için Application Insights tarafından toplanan veri miktarını sınırlamak isteyebilirsiniz. Bkz: [Application Insights'ta örnekleme](../azure-monitor/app/sampling.md) Bunu başarmak bazı mekanizmalar için.


## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [Application Insights](../azure-monitor/app/app-insights-overview.md).

* Application Insights'ı diğer dillerde desteği sağlamak için bakmak [dilleri, platformlar ve tümleştirmeler belgeleri](../azure-monitor/app/platforms.md).


