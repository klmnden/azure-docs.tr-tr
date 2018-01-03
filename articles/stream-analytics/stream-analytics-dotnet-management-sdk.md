---
title: "Azure Stream Analytics yönetimi .NET SDK'sı | Microsoft Docs"
description: "Stream Analytics yönetimi .NET SDK ile çalışmaya başlayın. Ayarlama ve analytics işleri çalıştırma hakkında bilgi edinin. Proje, giriş, çıkış ve dönüşümleri oluşturun."
keywords: .NET SDK, API analizi
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 5e93de87-0c6f-4f4b-be98-08d63f832897
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/06/2017
ms.author: samacha
ms.openlocfilehash: f17225d92fc35a6da9f6aa3cb0397569665e95e7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="management-net-sdk-set-up-and-run-analytics-jobs-using-the-azure-stream-analytics-api-for-net"></a>.NET SDK'sı Yönetimi: Ayarlama ve .NET için Azure Stream Analytics API'sini kullanarak analytics işleri çalıştırma
Ayarlamak ve yönetim .NET SDK kullanarak .NET için Stream Analytics API kullanarak analytics işleri çalıştırma hakkında bilgi edinin. Bir projesi ayarlayın, giriş ve çıkış kaynakları, dönüştürme ve başlangıç oluşturun ve işleri durdurur. Analytics işleriniz için Blob depolama biriminden veya event hub'ındaki veri akışını sağlayabilirsiniz.

Bkz: [.NET Stream Analytics API'si için yönetim başvuru belgelerine](https://msdn.microsoft.com/library/azure/dn889315.aspx).

Azure Stream Analytics akış verileri bulutta üzerinden düşük gecikmeli, yüksek oranda kullanılabilir, ölçeklenebilir, karmaşık olay işleme sağlayan tam olarak yönetilen bir hizmettir. Akış analizi veri akışlarını çözümlemek için akış işlerinin ayarlamak müşteriler etkinleştirir ve gerçek zamanlı analiz sürücü almalarını sağlar.  

> [!NOTE]
> Azure Stream Analytics yönetimi .NET SDK'sı v2.x sürümüyle Biz bu makaledeki örnek kod güncelleştirdiniz. Kullanır lagecy (1.x) SDK sürümü kullanan örnek kod için lütfen bkz [için Stream Analytics yönetimi .NET SDK'sı v1.x kullanmak](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-dotnet-management-sdk-v1).

## <a name="prerequisites"></a>Ön koşullar
Bu makaleye başlamadan önce aşağıdakilere sahip olmanız ve aşağıdaki işlemleri yapmış olmanız gerekir:

* Visual Studio 2017 veya 2015 yükleyin.
* İndirme ve yükleme [Azure .NET SDK'sı](https://azure.microsoft.com/downloads/).
* Aboneliğinizde Azure kaynak grubu oluşturun. Örnek bir Azure PowerShell komut dosyası verilmiştir. Azure PowerShell bilgi için bkz: [yükleyin ve Azure PowerShell yapılandırma](/powershell/azure/overview);  

        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group
        Select-AzureSubscription -SubscriptionName <subscription name>

            # If Stream Analytics has not been registered to the subscription, remove the remark symbol (#) to run the Register-AzureRMProvider cmdlet to register the provider namespace
            #Register-AzureRMProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>


* Kullanmak için bir giriş kaynağı ve Çıkış hedefini ayarlayın. Daha ayrıntılı yönergeler için bkz [eklemek girişleri](stream-analytics-add-inputs.md) örnek giriş ayarlamak için ve [eklemek çıkışları](stream-analytics-add-outputs.md) örnek bir çıktı ayarlamak için.

## <a name="set-up-a-project"></a>Bir projesi ayarlayın
Bir analiz işi oluşturmak için .NET, ilk projenizi ayarlamak için Stream Analytics API kullanın.

1. Visual Studio C# .NET konsol uygulaması oluşturun.
2. Paket Yöneticisi Konsolu'nda NuGet paketlerini yüklemek için aşağıdaki komutları çalıştırın. İlk Azure Stream Analytics yönetimi .NET SDK ' dir. İkinci bir Azure istemci kimlik doğrulaması için kullanılır.
   
        Install-Package Microsoft.Azure.Management.StreamAnalytics -Version 2.0.0
        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Version 2.3.1
3. Aşağıdakileri ekleyin **appSettings** App.config dosyasını bölümüne:
   
        <appSettings>
          <add key="ClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
          <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
          <add key="SubscriptionId" value="YOUR SUBSCRIPTION ID" />
          <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
        </appSettings>

    İçin değerleri **Subscriptionıd** ve **ActiveDirectoryTenantId** Azure aboneliği ve Kiracı kimlikleri. Aşağıdaki Azure PowerShell cmdlet'ini çalıştırarak bu değerleri alabilirsiniz:

        Get-AzureAccount

4. Aşağıdaki başvuru .csproj dosyanızda ekleyin:

        <Reference Include="System.Configuration" />

5. Aşağıdakileri ekleyin **kullanarak** deyimleri projenin kaynak dosyasında (Program.cs):
   
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.Threading;
        using System.Threading.Tasks;
        
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Rest;
6. Bir kimlik doğrulama yardımcı yöntemi ekleyin:

   ```
   private static async Task<ServiceClientCredentials> GetCredentials()
   {
       var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(ConfigurationManager.AppSettings["ClientId"], new Uri("urn:ietf:wg:oauth:2.0:oob"));
       ServiceClientCredentials credentials = await UserTokenProvider.LoginWithPromptAsync(ConfigurationManager.AppSettings["ActiveDirectoryTenantId"], activeDirectoryClientSettings);
       
       return credentials;
    }
   ```

## <a name="create-a-stream-analytics-management-client"></a>Stream Analytics Yönetimi istemcisi oluşturma
A **StreamAnalyticsManagementClient** nesnesi, iş ve giriş, çıkış ve dönüştürme gibi iş bileşenlerini yönetmenizi sağlar.

Aşağıdaki kodu başlangıcına ekleyin **ana** yöntemi:

   ```
    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamingJobName = "<YOUR STREAMING JOB NAME>";
    string inputName = "<YOUR JOB INPUT NAME>";
    string transformationName = "<YOUR JOB TRANSFORMATION NAME>";
    string outputName = "<YOUR JOB OUTPUT NAME>";
    
    SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
    
    // Get credentials
    ServiceClientCredentials credentials = GetCredentials().Result;
    
    // Create Stream Analytics management client
    StreamAnalyticsManagementClient streamAnalyticsManagementClient = new StreamAnalyticsManagementClient(credentials)
    {
        SubscriptionId = ConfigurationManager.AppSettings["SubscriptionId"]
    };
   ```

**ResourceGroupName** değişkenin değeri oluşturulduğunda veya bölümündeki önkoşul adımlarını çekilen kaynak grubunun adı ile aynı olması gerekir.

Proje oluşturma kimlik bilgisi sunu yönünü otomatikleştirmek için başvurmak [bir hizmet sorumlusu Azure Resource Manager ile kimlik doğrulaması](../azure-resource-manager/resource-group-authenticate-service-principal.md).

Bu makalenin kalan bölümlerinde Bu kod, başında olduğunu varsayın **ana** yöntemi.

## <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma
Aşağıdaki kod, tanımladığınız kaynak grubu altında Stream Analytics işi oluşturur. İş için bir giriş, çıkış ve dönüştürme daha sonra eklersiniz.

   ```
   // Create a streaming job
   StreamingJob streamingJob = new StreamingJob()
   {
       Tags = new Dictionary<string, string>()
       {
           { "Origin", ".NET SDK" },
           { "ReasonCreated", "Getting started tutorial" }
       },
       Location = "West US",
       EventsOutOfOrderPolicy = EventsOutOfOrderPolicy.Drop,
       EventsOutOfOrderMaxDelayInSeconds = 5,
       EventsLateArrivalMaxDelayInSeconds = 16,
       OutputErrorPolicy = OutputErrorPolicy.Drop,
       DataLocale = "en-US",
       CompatibilityLevel = CompatibilityLevel.OneFullStopZero,
       Sku = new Sku()
       {
           Name = SkuName.Standard
       }
   };
   StreamingJob createStreamingJobResult = streamAnalyticsManagementClient.StreamingJobs.CreateOrReplace(streamingJob, resourceGroupName, streamingJobName);
   ```

## <a name="create-a-stream-analytics-input-source"></a>Stream Analytics giriş kaynağı oluşturma
Aşağıdaki kod blob giriş kaynağı türü ve CSV serileştirme Stream Analytics giriş kaynağı oluşturur. Bir olay hub'ı giriş kaynağı oluşturmak için kullanın **EventHubStreamInputDataSource** yerine **BlobStreamInputDataSource**. Benzer şekilde, giriş kaynağı seri hale getirme türü özelleştirebilirsiniz.

   ```
   // Create an input
   StorageAccount storageAccount = new StorageAccount()
   {
       AccountName = "<YOUR STORAGE ACCOUNT NAME>",
       AccountKey = "<YOUR STORAGE ACCOUNT KEY>"
   };
   Input input = new Input()
   {
       Properties = new StreamInputProperties()
       {
           Serialization = new CsvSerialization()
           {
               FieldDelimiter = ",",
               Encoding = Encoding.UTF8
           },
           Datasource = new BlobStreamInputDataSource()
           {
               StorageAccounts = new[] { storageAccount },
               Container = "<YOUR STORAGE BLOB CONTAINER>",
               PathPattern = "{date}/{time}",
               DateFormat = "yyyy/MM/dd",
               TimeFormat = "HH",
               SourcePartitionCount = 16
           }
       }
   };
   Input createInputResult = streamAnalyticsManagementClient.Inputs.CreateOrReplace(input, resourceGroupName, streamingJobName, inputName);
   ```

Giriş kaynağı, varsayılan olarak, belirli bir iş için Blob Depolama veya bir event hub bağlıdır. Farklı projeler için aynı giriş kaynağı kullanmak için yeniden yöntemini çağırın ve başka bir iş adı belirtin.

## <a name="test-a-stream-analytics-input-source"></a>Stream Analytics giriş kaynağı test
**TestConnection** yöntemi, Stream Analytics işi giriş kaynağına giriş kaynağı türüne özel sair yönlerini bağlanabiliyor olup olmadığını sınar. Örneğin, bir önceki adımda oluşturduğunuz blob giriş kaynağında yanı sıra depolama hesabına bağlanmak için belirtilen kapsayıcının var olup olmadığını denetleyin depolama hesabı adı ve anahtar çifti kullanılabilir yöntem kontrol eder.

   ```
   // Test the connection to the input
   ResourceTestStatus testInputResult = streamAnalyticsManagementClient.Inputs.Test(resourceGroupName, streamingJobName, inputName);
   ```

## <a name="create-a-stream-analytics-output-target"></a>Stream Analytics Çıkış hedefini oluşturma
Bir çıkış hedefini oluşturma, Stream Analytics giriş kaynağı oluşturmak için çok benzer. Giriş kaynaklarıyla gibi belirli bir iş çıktı hedefleri bağlıdır. Aynı çıktı hedefi olarak farklı işler için kullanmak üzere yeniden yöntemini çağırın ve başka bir iş adı belirtin.

Aşağıdaki kod bir çıkış hedefini (Azure SQL veritabanı) oluşturur. Çıktı hedefin veri türüne ve/veya seri hale getirme türü özelleştirebilirsiniz.

   ```
   // Create an output
   Output output = new Output()
   {
       Datasource = new AzureSqlDatabaseOutputDataSource()
       {
           Server = "<YOUR DATABASE SERVER NAME>",
           Database = "<YOUR DATABASE NAME>",
           User = "<YOUR DATABASE LOGIN>",
           Password = "<YOUR DATABASE LOGIN PASSWORD>",
           Table = "<YOUR DATABASE TABLE NAME>"
       }
   };
   Output createOutputResult = streamAnalyticsManagementClient.Outputs.CreateOrReplace(output, resourceGroupName, streamingJobName, outputName);
   ```

## <a name="test-a-stream-analytics-output-target"></a>Stream Analytics Çıkış hedefini test
Stream Analytics Çıkış hedefini de sahip **TestConnection** bağlantıları sınama yöntemi.

   ```
   // Test the connection to the output
   ResourceTestStatus testOutputResult = streamAnalyticsManagementClient.Outputs.Test(resourceGroupName, streamingJobName, outputName);
   ```

## <a name="create-a-stream-analytics-transformation"></a>Stream Analytics dönüşüm oluşturun
Aşağıdaki kod bir akış analizi dönüştürmesi sorguyla oluşturur "seçin * giriş öğesinden" ve akış analizi işi için bir akış birimi ayırmak için belirtir. Akış birimleri ayarlama hakkında daha fazla bilgi için bkz: [ölçek Azure akış analizi işi](stream-analytics-scale-jobs.md).

   ```
   // Create a transformation
   Transformation transformation = new Transformation()
   {
       Query = "Select Id, Name from <your input name>", // '<your input name>' should be replaced with the value you put for the 'inputName' variable above or in a previous step
       StreamingUnits = 1
   };
   Transformation createTransformationResult = streamAnalyticsManagementClient.Transformations.CreateOrReplace(transformation, resourceGroupName, streamingJobName, transformationName);
   ```

Giriş ve çıkış gibi bir dönüşüm de altında oluşturulan belirli Stream Analytics işi bağlıdır.

## <a name="start-a-stream-analytics-job"></a>Akış analizi işi Başlat
Akış analizi işi ve kendi input(s), çıkışa ve dönüştürme oluşturduktan sonra iş çağırarak başlatabilirsiniz **Başlat** yöntemi.

Aşağıdaki örnek kod başlatır Stream Analytics işi özel çıkış başlangıç saati ile 12:12:12 12 Kasım 2012 için ayarlanmış UTC:

   ```
   // Start a streaming job
   StartStreamingJobParameters startStreamingJobParameters = new StartStreamingJobParameters()
   {
       OutputStartMode = OutputStartMode.CustomTime,
       OutputStartTime = new DateTime(2012, 12, 12, 12, 12, 12, DateTimeKind.Utc)
   };
   streamAnalyticsManagementClient.StreamingJobs.Start(resourceGroupName, streamingJobName, startStreamingJobParameters);
   ```

## <a name="stop-a-stream-analytics-job"></a>Stream Analytics işini durdurma
Çağırarak çalışan bir akış analizi işi durdurabilirsiniz **durdurmak** yöntemi.

   ```
   // Stop a streaming job
   streamAnalyticsManagementClient.StreamingJobs.Stop(resourceGroupName, streamingJobName);
   ```

## <a name="delete-a-stream-analytics-job"></a>Akış analizi işi Sil
**Silmek** yöntemi input(s), çıkışa ve dönüşüm işinin dahil olmak üzere temel alınan bir alt kaynaklara yanı sıra iş siler.

   ```
   // Delete a streaming job
   streamAnalyticsManagementClient.StreamingJobs.Delete(resourceGroupName, streamingJobName);
   ```

## <a name="get-support"></a>Destek alın
Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar
Oluşturmak ve analytics işlerini çalıştırmak için .NET SDK kullanarak temelleri öğrendiniz. Daha fazla bilgi edinmek için şunlara bakın:

* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics yönetimi .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[azure.blob.storage]: http://azure.microsoft.com/documentation/services/storage/
[azure.blob.storage.use]: http://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/

[azure.event.hubs]: http://azure.microsoft.com/services/event-hubs/
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.forum]: http://go.microsoft.com/fwlink/?LinkId=512151

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.developer.guide]: stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
