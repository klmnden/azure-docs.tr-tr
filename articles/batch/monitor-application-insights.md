---
title: Azure Application Insights ile toplu izleme | Microsoft Docs
description: Azure Application Insights kitaplığını kullanarak bir Azure Batch .NET uygulama izleme öğrenin.
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: .NET
ms.topic: article
ms.workload: na
ms.date: 04/05/2018
ms.author: danlep
ms.openlocfilehash: 9f989ada01a2ffced509b42df9e46aa001386ab6
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
ms.locfileid: "34077403"
---
# <a name="monitor-and-debug-an-azure-batch-net-application-with-application-insights"></a>İzleme ve Application Insights ile bir Azure Batch .NET uygulama hatalarını ayıklama

[Application Insights](../application-insights/app-insights-overview.md) Azure hizmetlerine dağıtılan izleme ve hata ayıklama uygulamaları için geliştiricilere yönelik zarif ve güçlü bir yol sağlar. Application Insights izleme performans sayaçları ve özel durumları yanı sıra gereç özel ölçümleri ve izleme kodunuzu kullanın. Application Insights Azure Batch uygulamanızla tümleştirmek davranışları ayrıntılı Öngörüler elde ve yakın gerçek zamanlı olarak sorunları araştırmak sağlar.

Bu makalede, Ekle, Application Insights kitaplığı Azure Batch .NET çözümünüze yapılandırmak ve uygulama kodunuz izleme gösterilmektedir. Ayrıca, uygulamanızı Azure Portalı aracılığıyla izlemek ve özel panolar derleme yolu gösterir. Application Insights diğer dillerde desteği için bakmak [diller, platformlar ve tümleştirmeler belgeleri](../application-insights/app-insights-platforms.md).

Örnek C# çözümü bu makalede eşlik koduyla kullanılabilir [GitHub](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ApplicationInsights). Bu örnek için Application Insights araçları kod ekler [TopNWords](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords) örnek. Bu örnekle bilmiyorsanız, oluşturma ve çalıştırma TopNWords ilk deneyin. Bunun yapılması, bir dizi giriş BLOB'ları birden çok işlem düğümlerinde paralel işleme temel toplu iş akışı anlamanıza yardımcı olur. 

## <a name="prerequisites"></a>Önkoşullar
* [Visual Studio IDE](https://www.visualstudio.com/vs) (Visual Studio 2015 veya daha yeni bir sürüm)

* [Toplu işlem hesabı ile bağlantılı depolama hesabı](batch-account-create-portal.md)

* [Uygulama Insights kaynağı](../application-insights/app-insights-create-new-resource.md)
  
   * Application Insights oluşturmak için Azure portal'ı kullanmanızı *kaynak*. Seçin *genel* **uygulama türü**.

   * Kopya [izleme anahtarını](../application-insights/app-insights-create-new-resource.md#copy-the-instrumentation-key) portalından. Bu makalenin sonraki bölümlerinde gereklidir.
  
  > [!NOTE]
  > Olabilir [ücret](https://azure.microsoft.com/pricing/details/application-insights/) Application Insights'ta depolanan veriler için. Bu tanılama ve izleme bu makalede ele alınan verileri içerir.
  > 

## <a name="add-application-insights-to-your-project"></a>Projenize Application Insights ekleyin

**Microsoft.ApplicationInsights.WindowsServer** NuGet paketi ve bağımlılıklarını projeniz için gerekli. Ekleyin veya onları uygulamanızın projeye geri yükleyin. Paketi yüklemek için kullandığınız `Install-Package` komut veya NuGet Paket Yöneticisi.

```powershell
Install-Package Microsoft.ApplicationInsights.WindowsServer
```
Başvuru Application Insights .NET uygulamanızdan kullanarak **Microsoft.ApplicationInsights** ad alanı.

## <a name="instrument-your-code"></a>Kodunuzu izleme

Kodunuzu izleme için çözümünüzün bir Application Insights oluşturmak için gerek duyduğu [TelemetryClient](/dotnet/api/microsoft.applicationinsights.telemetryclient). Örnekte, TelemetryClient yapılandırmasını yükler [Applicationınsights.config](../application-insights/app-insights-configuration-with-applicationinsights-config.md) dosya. Applicationınsights.config aşağıdaki projelerde Application Insights araçları anahtarınızla güncelleştirdiğinizden emin olun: Microsoft.Azure.Batch.Samples.TelemetryStartTask ve TopNWordsSample.

```xml
<InstrumentationKey>YOUR-IKEY-GOES-HERE</InstrumentationKey>
```
Ayrıca izleme anahtarını TopNWords.cs dosyasında ekleyin.

Aşağıdaki örnekte, TopNWords.cs kullanır [araçları çağrıları](../application-insights/app-insights-api-custom-events-metrics.md) Application Insights API'si gelen:
* `TrackMetric()` -Ne kadar süreyle, ortalama olarak bir işlem düğümünde gerekli metin dosyasını karşıdan yüklemek için gereken izler.
* `TrackTrace()` -Kodunuzu hata ayıklama çağrıları ekler.
* `TrackEvent()` -Olaylarını yakalamak için ilginç izler.

Bu örnek, özel durum işleme çıkışı kasıtlı olarak bırakır. Bunun yerine, Application Insights raporlarını otomatik olarak işlenmeyen özel durumlar, hata ayıklama deneyimini önemli ölçüde iyileştirir. 

Aşağıdaki kod parçacığında bu yöntemlerinin nasıl kullanılacağını gösterir.

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

### <a name="azure-batch-telemetry-initializer-helper"></a>Azure Batch telemetri Başlatıcı Yardımcısı
Verilen sunucu ve örnek için telemetri raporlama Application Insights Azure VM rolü ve VM adı varsayılan değerleri kullanır. Azure Batch bağlamında örnek havuz adı ve düğüm adı bunun yerine işlem nasıl kullanılacağı gösterilir. Kullanım bir [telemetri Başlatıcı](../application-insights/app-insights-api-filtering-sampling.md#add-properties) varsayılan değerleri geçersiz kılmak için. 

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

Telemetri Başlatıcısı etkinleştirmek için TopNWordsSample proje Applicationınsights.config dosyasında aşağıdakileri içerir:

```xml
<TelemetryInitializers>
    <Add Type="Microsoft.Azure.Batch.Samples.TelemetryInitializer.AzureBatchNodeTelemetryInitializer, Microsoft.Azure.Batch.Samples.TelemetryInitializer"/>
</TelemetryInitializers>
``` 

## <a name="update-the-job-and-tasks-to-include-application-insights-binaries"></a>Güncelleştirme işi ve görevleri Application Insights ikili dosyaları eklemek için

Application Insights'ın doğru işlem düğümlerinde çalışması için sırayla ikili dosyaları doğru yerleştirilir emin olun. Böylece göreviniz yürütür zaman yükleneceğini gerekli ikili dosyaları, görevin kaynak dosyaları koleksiyona ekleyin. Aşağıdaki kod parçacıkları Job.cs kodda benzerdir.

İlk olarak, statik karşıya yüklemek için Application Insights dosyaların listesini oluşturun.

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

Ardından, görev tarafından kullanılan basamak dosyaları oluşturun.
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

`FileToStage` Yardımcı işlevini kolayca bir dosyaya yerel diskten bir Azure Storage blobu karşıya yüklemenize olanak sağlayan kod örneğinde bir yöntemdir. Her dosya daha sonra bir işlem düğümünde indirilir ve bir görev tarafından başvurulan.

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

## <a name="view-data-in-the-azure-portal"></a>Görünüm verilerini Azure portalında

Application Insights kullanmak için görevleri ve iş yapılandırdıktan, örnek işi havuzunuzdaki çalıştırın. Azure Portalı'na gidin ve sağladığınız Application Insights kaynağı açın. Havuz sağlandıktan sonra veri akışının ve günlüğe görmeye başlamanız gerekir. Bu makalenin geri kalanında yalnızca birkaç Application Insights özellikleri dokunur, ancak tam özellik kümesi keşfetmek çekinmeyin.

### <a name="view-live-stream-data"></a>Canlı akış verilerini görüntüleme

Uygulamaları Insights kaynağınıza izleme günlükleri görüntülemek için **canlı akış**. Aşağıdaki ekran görüntüsünde, örneğin işlem düğümü başına CPU kullanımı havuzdaki işlem düğümleri'ten gelen canlı verileri görüntülemek gösterilmiştir.

![Canlı akış işlem düğümü veri](./media/monitor-application-insights/applicationinsightslivestream.png)

### <a name="view-trace-logs"></a>İzleme günlüklerini görüntüle

Uygulamaları Insights kaynağınıza izleme günlükleri görüntülemek için **arama**. Bu görünüm izlemelerini, olaylar ve özel durumlar dahil olmak üzere Application Insights tarafından yakalanan Tanılama verileri listesini gösterir. 

Aşağıdaki ekran görüntüsünde, nasıl bir görev için tek bir izleme oturumu ve hata ayıklama amacıyla daha sonra sorgulanan gösterir.

![İzleme günlükleri görüntüsü](./media/monitor-application-insights/tracelogsfortask.png)

### <a name="view-unhandled-exceptions"></a>İşlenmeyen özel durumlarını görüntülemek

Aşağıdaki ekran görüntüleri, Application Insights uygulamanızdan oluşturulan özel durumları nasıl günlüğe yazacağını gösterir. Bu durumda, özel durum atma uygulama saniye içinde belirli bir özel durum incelemek ve sorunu tanılamak.

![İşlenmeyen özel durumlar](./media/monitor-application-insights/exception.png)

### <a name="measure-blob-download-time"></a>Ölçü blob karşıdan yükleme süresi

Özel ölçümleri ayrıca portalında değerli bir araç değildir. Örneğin, her işlem düğümü işlemekte olan gerekli metin dosyasını karşıdan yüklemek için geçen ortalama süre görüntüleyebilirsiniz.

Bir örnek grafik oluşturmak için:
1. Application Insights kaynağınıza tıklatın **ölçüm Gezgini** > **Ekle grafik**.
2. Tıklatın **Düzenle** eklendi grafik.
2. Grafik ayrıntıları şu şekilde güncelleştirin:
   * Ayarlama **grafik türü** için **kılavuz**.
   * Ayarlama **toplama** için **ortalama**.
   * Ayarlama **Group by** için **nodeId**.
   * İçinde **ölçümleri**seçin **özel** > **Blob yükleme saniye cinsinden**.
   * Görüntü ayarlama **renk paletini** tercih ettiğiniz için. 

![Düğüm başına BLOB karşıdan yükleme süresi](./media/monitor-application-insights/blobdownloadtime.png)


## <a name="monitor-compute-nodes-continuously"></a>Sürekli olarak İzleyici işlem düğümleri

Görevler çalışırken performans sayaçları dahil olmak üzere tüm ölçümleri yalnızca kaydedilir fark etmiş olabilirsiniz. Bu davranış, Application Insights günlüklerini veri miktarını sınırlar için yararlıdır. Bununla birlikte, her zaman işlem düğümleri izlemek istediğiniz zaman durumlar vardır. Örneğin, bunlar değil zamanlanan arka plan çalışması Batch hizmeti ile çalışıyor olabilir. Bu durumda, bir izleme işlemini işlem düğümü ömrü için çalışacak şekilde ayarlayın. 

Bu davranış elde etmek için bir Application Insights kitaplığı yükleyen ve arka planda çalışan bir işlem oluşturma yoludur. Örnekte, başlangıç görevi makinedeki ikilileri yükler ve süresiz olarak çalışan bir işlemi tutar. Bu işlem, performans sayaçları gibi ilgilendiğiniz ek veri yayma için Application Insights yapılandırma dosyası yapılandırın.

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
    virtualMachineSize: "small",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));
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
> Çözümünüzü yönetilebilirliğini artırmak için bütünleştirilmiş kodunda paketleyebilirsiniz bir [uygulama paketi](./batch-application-packages.md). Ardından, uygulama paketi otomatik olarak, havuzlarınızı dağıtmak için bir uygulama paketi başvurusu havuzu yapılandırması için ekleyin.
>

## <a name="throttle-and-sample-data"></a>Azaltma ve örnek veri 

Azure Batch uygulamalar üretimde çalışan büyük ölçekli yapısı nedeniyle, maliyetleri yönetmek için Application Insights tarafından toplanan veri miktarını sınırlamak isteyebilirsiniz. Bkz: [Application Insights'ta örnekleme](../application-insights/app-insights-sampling.md) Bunu başarmak bazı mekanizmalar için.


## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [Application Insights](../application-insights/app-insights-overview.md).

* Application Insights diğer dillerde desteği için bakmak [diller, platformlar ve tümleştirmeler belgeleri](../application-insights/app-insights-platforms.md).


