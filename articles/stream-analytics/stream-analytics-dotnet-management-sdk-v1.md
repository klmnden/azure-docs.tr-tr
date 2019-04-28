---
title: .NET Yönetim SDK'sı v1.x Azure Stream Analytics için
description: Stream Analytics yönetim .NET SDK'sı ile çalışmaya başlayın. Ayarlama ve analiz işleri çalıştırma hakkında bilgi edinin. Bir proje, girişler, çıkışlar ve dönüştürmeleri oluşturun.
services: stream-analytics
author: jseb225
ms.author: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: 9c4ddb1375154fef3290008a7b981a4e9069ae94
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62104839"
---
# <a name="set-up-and-run-analytics-jobs-using-azure-stream-analytics-api-for-net"></a>Ayarlama ve .NET için Azure Stream Analytics API'si kullanarak analytics işlerini çalıştırın
Ayarlama ve Stream Analytics API'si için .NET Yönetim SDK'sını kullanarak .NET kullanarak analytics işlerini çalıştırma hakkında bilgi edinin. Projesini ayarlarsınız, giriş ve çıkış kaynakları, dönüştürme ve başlangıç oluşturma ve işleri durdur. Analytics işleriniz için Blob depolama alanından veya bir olay hub'ından veri akışını yapabilirsiniz.

Bkz: [Stream Analytics API'si için .NET için yönetim başvuru belgeleri](https://msdn.microsoft.com/library/azure/dn889315.aspx).

Azure Stream Analytics, akış verileri bulutta üzerinden düşük gecikme süreli, yüksek oranda kullanılabilir, ölçeklenebilir, karmaşık olay işleme sağlayan tam olarak yönetilen bir hizmettir. Stream Analytics, müşterilerin veri akışlarını analiz etmek için akış işleri ayarlamanızı sağlar ve gerçek zamanlı analiz sürücü almalarını sağlar.  

> [!NOTE]
> Bu makaledeki örnek kod hala eski (1.x) Azure Stream Analytics yönetim .NET SDK'sı sürümünü kullanır. Güncel SDK sürümünü kullanan örnek kod için lütfen bkz [Stream Analytics için yönetim .NET SDK'yı kullanın](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-dotnet-management-sdk).

## <a name="prerequisites"></a>Önkoşullar
Bu makaleye başlamadan önce aşağıdakilere sahip olmanız ve aşağıdaki işlemleri yapmış olmanız gerekir:

* Visual Studio 2017 veya 2015'i yükleyin.
* İndirme ve yükleme [Azure .NET SDK'sı](https://azure.microsoft.com/downloads/).
* Aboneliğinizde bir Azure kaynak grubu oluşturun. Örnek Azure PowerShell Betiği verilmiştir. Azure PowerShell için bilgi [yüklemek ve Azure PowerShell yapılandırma](/powershell/azure/overview);  

   ```powershell
   # Log in to your Azure account
   Add-AzureAccount
   # Select the Azure subscription you want to use to create the resource group
   Select-AzureSubscription -SubscriptionName <subscription name>
       # If Stream Analytics has not been registered to the subscription, remove the remark symbol (#) to run the    Register-AzProvider cmdlet to register the provider namespace
       #Register-AzProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'
   # Create an Azure resource group
   New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>
   ```

* Bir giriş kaynağı ve bağlanmak iş için çıktı hedef ayarlayın.

## <a name="set-up-a-project"></a>Projesi kurun
Bir analiz işi oluşturmak için önce projenizi ayarlayın, .NET için Stream Analytics API'si kullanın.

1. Visual Studio C# .NET konsol uygulaması oluşturun.
2. Paket Yöneticisi Konsolu'nda NuGet paketlerini yüklemek için aşağıdaki komutları çalıştırın. Azure Stream Analytics yönetim .NET SDK'sı ilk hesaptır. İkinci bir kimlik doğrulaması için kullanılacak Azure Active Directory istemcisidir.

```powershell
Install-Package Microsoft.Azure.Management.StreamAnalytics -Version 1.8.3
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.28.4
```

1. Aşağıdaki **appSettings** App.config dosyasına bölümü:

   ```csharp
   <appSettings>
     <!--CSM Prod related values-->
     <add key="ActiveDirectoryEndpoint" value="https://login.microsoftonline.com/" />
     <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
     <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
     <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
     <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
     <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION" />
     <add key="ActiveDirectoryTenantId" value="YOU TENANT ID" />
   </appSettings>
   ```

    Değerleri Değiştir **Subscriptionıd** ve **ActiveDirectoryTenantId** Azure abonelik ve Kiracı kimliklerine sahip. Aşağıdaki Azure PowerShell cmdlet'ini çalıştırarak bu değerleri alabilirsiniz:

        Get-AzureAccount

1. .Csproj dosyanızda şu başvuruyu ekleyin:

   ```csharp
   <Reference Include="System.Configuration" />
   ```

1. Aşağıdaki **kullanarak** deyimleri projedeki kaynak dosyasına (Program.cs):

```csharp
using System;
using System.Configuration;
using System.Threading.Tasks;

using Microsoft.Azure;
using Microsoft.Azure.Management.StreamAnalytics;
using Microsoft.Azure.Management.StreamAnalytics.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

1. Bir kimlik doğrulama Yardımcısını yöntemi ekleyin:

   ```csharp
   private static async Task<string> GetAuthorizationHeader()
   {
       var context = new AuthenticationContext(
           ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
           ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

        AuthenticationResult result = await context.AcquireTokenASync(
           resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
           clientId: ConfigurationManager.AppSettings["AsaClientId"],
           redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
           promptBehavior: PromptBehavior.Always);

        if (result != null)
            return result.AccessToken;

       throw new InvalidOperationException("Failed to acquire token");
   }
   ```  

## <a name="create-a-stream-analytics-management-client"></a>Stream Analytics yönetim istemcisi oluşturma
A **StreamAnalyticsManagementClient** nesnesinin, iş ve giriş ve çıkış dönüştürme gibi iş bileşenlerini yönetmenize olanak sağlar.

Aşağıdaki kodu ekleyin başlangıcına **ana** yöntemi:

   ```csharp
   string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
   string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";
   string streamAnalyticsInputName = "<YOUR JOB INPUT NAME>";
   string streamAnalyticsOutputName = "<YOUR JOB OUTPUT NAME>";
   string streamAnalyticsTransformationName = "<YOUR JOB TRANSFORMATION NAME>";
   // Get authentication token
   TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
       ConfigurationManager.AppSettings["SubscriptionId"],
       GetAuthorizationHeader().Result);
   // Create Stream Analytics management client
   StreamAnalyticsManagementClient client = new StreamAnalyticsManagementClient(aadTokenCredentials);
   ```

**ResourceGroupName** değişkenin değeri oluşturulduğunda veya bölümündeki önkoşul adımlarını çekilen kaynak grubunun adıyla aynı olmalıdır.

İş oluşturma kimlik bilgisi sunu yönüyle otomatikleştirmek için başvurmak [Azure Resource Manager ile hizmet sorumlusu kimlik doğrulaması](../active-directory/develop/howto-authenticate-service-principal-powershell.md).

Bu makalenin kalan bölümünde, bu kod başlangıcında olduğunu varsayın **ana** yöntemi.

## <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma
Aşağıdaki kod, tanımladığınız bir kaynak grubundaki bir Stream Analytics işi oluşturur. İş için bir giriş, çıkış ve dönüştürme daha sonra ekleyeceksiniz.

   ```csharp
   // Create a Stream Analytics job
   JobCreateOrUpdateParameters jobCreateParameters = new JobCreateOrUpdateParameters()
   {
       Job = new Job()
       {
           Name = streamAnalyticsJobName,
           Location = "<LOCATION>",
           Properties = new JobProperties()
           {
               EventsOutOfOrderPolicy = EventsOutOfOrderPolicy.Adjust,
               Sku = new Sku()
               {
                   Name = "Standard"
               }
           }
       }
   };
   JobCreateOrUpdateResponse jobCreateResponse = client.StreamingJobs.CreateOrUpdate(resourceGroupName, jobCreateParameters);
   ```

## <a name="create-a-stream-analytics-input-source"></a>Stream Analytics giriş kaynağı oluşturma
Aşağıdaki kod blob giriş kaynağı türü ve CSV serileştirme ile bir Stream Analytics giriş kaynağı oluşturur. Bir olay hub'ı giriş kaynağı oluşturmak için kullanın **EventHubStreamInputDataSource** yerine **BlobStreamInputDataSource**. Benzer şekilde, giriş kaynağı serileştirme türü özelleştirebilirsiniz.

   ```csharp
   // Create a Stream Analytics input source
   InputCreateOrUpdateParameters jobInputCreateParameters = new InputCreateOrUpdateParameters()
   {
       Input = new Input()
       {
           Name = streamAnalyticsInputName,
           Properties = new StreamInputProperties()
           {
               Serialization = new CsvSerialization
               {
                   Properties = new CsvSerializationProperties
                   {
                       Encoding = "UTF8",
                       FieldDelimiter = ","
                   }
               },
               DataSource = new BlobStreamInputDataSource
               {
                   Properties = new BlobStreamInputDataSourceProperties
                   {
                       StorageAccounts = new StorageAccount[]
                       {
                           new StorageAccount()
                           {
                               AccountName = "<YOUR STORAGE ACCOUNT NAME>",
                               AccountKey = "<YOUR STORAGE ACCOUNT KEY>"
                           }
                       },
                       Container = "samples",
                       PathPattern = ""
                   }
               }
           }
       }
   };
   InputCreateOrUpdateResponse inputCreateResponse =
   client.Inputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobInputCreateParameters);
   ```

Giriş kaynakları, varsayılan olarak, belirli bir işin için Blob Depolama veya bir olay hub'ı bağlıdır. Farklı projeler için aynı giriş kaynağı kullanmak için yöntemi yeniden çağırın ve başka bir iş adı belirtmeniz gerekir.

## <a name="test-a-stream-analytics-input-source"></a>Stream Analytics giriş kaynağı test
**TestConnection** yöntemi, Stream Analytics işi giriş kaynağı türüne özel diğer yönleri için giriş kaynağı bağlanabiliyor olup olmadığını sınar. Örneğin, bir önceki adımda oluşturduğunuz blob giriş kaynağı içinde belirtilen kapsayıcının var olup olmadığını denetleyin yanı sıra depolama hesabına bağlanmak için depolama hesabı adı ve anahtar çifti kullanılabilir yöntemi kontrol eder.

   ```csharp
   // Test input source connection
   DataSourceTestConnectionResponse inputTestResponse =
       client.Inputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsInputName);
   ```

## <a name="create-a-stream-analytics-output-target"></a>Stream Analytics Çıkış hedefi oluşturma
Stream Analytics giriş kaynağı oluşturmak için bir çıkış hedefi oluşturma çok benzer. Giriş kaynakları gibi çıkış hedefleri için belirli bir işin bağlıdır. Farklı projeler için aynı çıkış hedefi kullanmak için yöntemi yeniden çağırın ve başka bir iş adı belirtmeniz gerekir.

Aşağıdaki kod, bir çıkış hedefi (Azure SQL veritabanı) oluşturur. Çıkış hedefinin veri türü ve/veya serileştirme türü özelleştirebilirsiniz.

   ```csharp
   // Create a Stream Analytics output target
   OutputCreateOrUpdateParameters jobOutputCreateParameters = new OutputCreateOrUpdateParameters()
   {
       Output = new Output()
       {
           Name = streamAnalyticsOutputName,
           Properties = new OutputProperties()
           {
               DataSource = new SqlAzureOutputDataSource()
               {
                   Properties = new SqlAzureOutputDataSourceProperties()
                   {
                       Server = "<YOUR DATABASE SERVER NAME>",
                       Database = "<YOUR DATABASE NAME>",
                       User = "<YOUR DATABASE LOGIN>",
                       Password = "<YOUR DATABASE LOGIN PASSWORD>",
                       Table = "<YOUR DATABASE TABLE NAME>"
                   }
               }
           }
       }
   };
   OutputCreateOrUpdateResponse outputCreateResponse =
       client.Outputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobOutputCreateParameters);
   ```

## <a name="test-a-stream-analytics-output-target"></a>Stream Analytics çıkış hedef test
Bir Stream Analytics çıkış hedef de sahip **TestConnection** bağlantılarını test etme için yöntemi.

   ```csharp
   // Test output target connection
   DataSourceTestConnectionResponse outputTestResponse =
       client.Outputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsOutputName);
   ```

## <a name="create-a-stream-analytics-transformation"></a>Bir Stream Analytics dönüştürme
Aşağıdaki kod bir Stream Analytics dönüştürme sorgu oluşturur. "seçin * girişten alınan" ve Stream Analytics işi için bir akış birimi ayırmak için belirtir. Akış birimleri ayarlama hakkında daha fazla bilgi için bkz. [ölçek Azure Stream Analytics işleri](stream-analytics-scale-jobs.md).

   ```csharp
   // Create a Stream Analytics transformation
   TransformationCreateOrUpdateParameters transformationCreateParameters = new TransformationCreateOrUpdateParameters()
   {
       Transformation = new Transformation()
       {
           Name = streamAnalyticsTransformationName,
           Properties = new TransformationProperties()
           {
               StreamingUnits = 1,
               Query = "select * from Input"
           }
       }
   };
   var transformationCreateResp =
       client.Transformations.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, transformationCreateParameters);
   ```

Giriş ve çıkış gibi dönüştürme de altında oluşturulan belirli Stream Analytics işi bağlıdır.

## <a name="start-a-stream-analytics-job"></a>Stream Analytics işini başlatın
Bir Stream Analytics işi ve kendi girişlere, çıkışlar ve dönüştürme oluşturduktan sonra iş çağırarak başlatabilirsiniz **Başlat** yöntemi.

Aşağıdaki örnek bir özel çıkış başlangıç saatine sahip bir Stream Analytics işi, 12:12:12 12 Aralık 2012'ye ayarlayın kod başlatır UTC:

   ```csharp
   // Start a Stream Analytics job
   JobStartParameters jobStartParameters = new JobStartParameters
   {
       OutputStartMode = OutputStartMode.CustomTime,
       OutputStartTime = new DateTime(2012, 12, 12, 0, 0, 0, DateTimeKind.Utc)
   };
   
   LongRunningOperationResponse jobStartResponse = client.StreamingJobs.Start(resourceGroupName, streamAnalyticsJobName,    jobStartParameters);
   ```

## <a name="stop-a-stream-analytics-job"></a>Stream Analytics işini durdurma
Çalışan bir Stream Analytics işi çağrı yaparak da durdurabilirsiniz **Durdur** yöntemi.

   ```csharp
   // Stop a Stream Analytics job
   LongRunningOperationResponse jobStopResponse = client.StreamingJobs.Stop(resourceGroupName, streamAnalyticsJobName);
   ```

## <a name="delete-a-stream-analytics-job"></a>Stream Analytics işini sil
**Sil** yöntemi, işi ve bunun yanı sıra iş dönüşümü girişlere ve çıkışlar da dahil olmak üzere temel alınan bir alt kaynakları siler.

   ```csharp
   // Delete a Stream Analytics job
   LongRunningOperationResponse jobDeleteResponse = client.StreamingJobs.Delete(resourceGroupName, streamAnalyticsJobName);
   ```

## <a name="get-support"></a>Destek alın
Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Sonraki adımlar
Oluşturup analytics işlerini çalıştırmak için bir .NET SDK'sı kullanmanın temel adımlarını öğrendiniz. Daha fazla bilgi edinmek için şunlara bakın:

* [Azure Stream analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics yönetim .NET SDK'sini](https://msdn.microsoft.com/library/azure/dn889315.aspx).
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[azure.blob.storage]: https://azure.microsoft.com/documentation/services/storage/
[azure.blob.storage.use]: https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/

[azure.event.hubs]: https://azure.microsoft.com/services/event-hubs/
[azure.event.hubs.developer.guide]: https://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.forum]: https://go.microsoft.com/fwlink/?LinkId=512151

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.developer.guide]: stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301
