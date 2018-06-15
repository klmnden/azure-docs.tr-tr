---
title: Yönetim .NET SDK v1.x Azure akış analizi için
description: Stream Analytics yönetimi .NET SDK ile çalışmaya başlayın. Ayarlama ve analytics işleri çalıştırma hakkında bilgi edinin. Proje, giriş, çıkış ve dönüşümleri oluşturun.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/06/2017
ms.openlocfilehash: a8f272403a0c6903521acc7cc45935c4e5bc419c
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
ms.locfileid: "31528112"
---
# <a name="management-net-sdk-v1x-set-up-and-run-analytics-jobs-using-the-azure-stream-analytics-api-for-net"></a>Yönetim .NET SDK'sı v1.x: ayarlamak ayarlama ve .NET için Azure Stream Analytics API'sini kullanarak analytics işleri çalıştırma
Ayarlamak ve yönetim .NET SDK kullanarak .NET için Stream Analytics API kullanarak analytics işleri çalıştırma hakkında bilgi edinin. Bir projesi ayarlayın, giriş ve çıkış kaynakları, dönüştürme ve başlangıç oluşturun ve işleri durdurur. Analytics işleriniz için Blob depolama biriminden veya event hub'ındaki veri akışını sağlayabilirsiniz.

Bkz: [.NET Stream Analytics API'si için yönetim başvuru belgelerine](https://msdn.microsoft.com/library/azure/dn889315.aspx).

Azure Stream Analytics akış verileri bulutta üzerinden düşük gecikmeli, yüksek oranda kullanılabilir, ölçeklenebilir, karmaşık olay işleme sağlayan tam olarak yönetilen bir hizmettir. Akış analizi veri akışlarını çözümlemek için akış işlerinin ayarlamak müşteriler etkinleştirir ve gerçek zamanlı analiz sürücü almalarını sağlar.  

> [!NOTE]
> Bu makaledeki örnek kod, yine Azure Stream Analytics yönetimi .NET SDK'ın eski (1.x) sürümünü kullanır. Güncel SDK sürümü kullanan örnek kod için lütfen bkz [akış analizi için yönetim .NET SDK'sını kullanma](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-dotnet-management-sdk).

## <a name="prerequisites"></a>Önkoşullar
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


* Bir giriş ve çıkış hedefi bağlanmak iş için ayarlayın.

## <a name="set-up-a-project"></a>Bir projesi ayarlayın
Bir analiz işi oluşturmak için .NET, ilk projenizi ayarlamak için Stream Analytics API kullanın.

1. Visual Studio C# .NET konsol uygulaması oluşturun.
2. Paket Yöneticisi Konsolu'nda NuGet paketlerini yüklemek için aşağıdaki komutları çalıştırın. İlk Azure Stream Analytics yönetimi .NET SDK ' dir. İkinci bir kimlik doğrulaması için kullanılacak Azure Active Directory istemcidir.
   
        Install-Package Microsoft.Azure.Management.StreamAnalytics -Version 1.8.3
        Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.28.4
3. Aşağıdakileri ekleyin **appSettings** App.config dosyasını bölümüne:
   
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

    İçin değerleri **Subscriptionıd** ve **ActiveDirectoryTenantId** Azure aboneliği ve Kiracı kimlikleri. Aşağıdaki Azure PowerShell cmdlet'ini çalıştırarak bu değerleri alabilirsiniz:

        Get-AzureAccount

4. Aşağıdaki başvuru .csproj dosyanızda ekleyin:

        <Reference Include="System.Configuration" />

1. Aşağıdakileri ekleyin **kullanarak** deyimleri projenin kaynak dosyasında (Program.cs):
   
        using System;
        using System.Configuration;
        using System.Threading.Tasks;
        
        using Microsoft.Azure;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
2. Bir kimlik doğrulama yardımcı yöntemi ekleyin:

   ```   
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

## <a name="create-a-stream-analytics-management-client"></a>Stream Analytics Yönetimi istemcisi oluşturma
A **StreamAnalyticsManagementClient** nesnesi, iş ve giriş, çıkış ve dönüştürme gibi iş bileşenlerini yönetmenizi sağlar.

Aşağıdaki kodu başlangıcına ekleyin **ana** yöntemi:

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

**ResourceGroupName** değişkenin değeri oluşturulduğunda veya bölümündeki önkoşul adımlarını çekilen kaynak grubunun adı ile aynı olması gerekir.

Proje oluşturma kimlik bilgisi sunu yönünü otomatikleştirmek için başvurmak [bir hizmet sorumlusu Azure Resource Manager ile kimlik doğrulaması](../azure-resource-manager/resource-group-authenticate-service-principal.md).

Bu makalenin kalan bölümlerinde Bu kod, başında olduğunu varsayın **ana** yöntemi.

## <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma
Aşağıdaki kod, tanımladığınız kaynak grubu altında Stream Analytics işi oluşturur. İş için bir giriş, çıkış ve dönüştürme daha sonra eklersiniz.

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


## <a name="create-a-stream-analytics-input-source"></a>Stream Analytics giriş kaynağı oluşturma
Aşağıdaki kod blob giriş kaynağı türü ve CSV serileştirme Stream Analytics giriş kaynağı oluşturur. Bir olay hub'ı giriş kaynağı oluşturmak için kullanın **EventHubStreamInputDataSource** yerine **BlobStreamInputDataSource**. Benzer şekilde, giriş kaynağı seri hale getirme türü özelleştirebilirsiniz.

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

Giriş kaynağı, varsayılan olarak, belirli bir iş için Blob Depolama veya bir event hub bağlıdır. Farklı projeler için aynı giriş kaynağı kullanmak için yeniden yöntemini çağırın ve başka bir iş adı belirtin.

## <a name="test-a-stream-analytics-input-source"></a>Stream Analytics giriş kaynağı test
**TestConnection** yöntemi, Stream Analytics işi giriş kaynağına giriş kaynağı türüne özel sair yönlerini bağlanabiliyor olup olmadığını sınar. Örneğin, bir önceki adımda oluşturduğunuz blob giriş kaynağında yanı sıra depolama hesabına bağlanmak için belirtilen kapsayıcının var olup olmadığını denetleyin depolama hesabı adı ve anahtar çifti kullanılabilir yöntem kontrol eder.

    // Test input source connection
    DataSourceTestConnectionResponse inputTestResponse =
        client.Inputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsInputName);

## <a name="create-a-stream-analytics-output-target"></a>Stream Analytics Çıkış hedefini oluşturma
Bir çıkış hedefini oluşturma, Stream Analytics giriş kaynağı oluşturmak için çok benzer. Giriş kaynaklarıyla gibi belirli bir iş çıktı hedefleri bağlıdır. Aynı çıktı hedefi olarak farklı işler için kullanmak üzere yeniden yöntemini çağırın ve başka bir iş adı belirtin.

Aşağıdaki kod bir çıkış hedefini (Azure SQL veritabanı) oluşturur. Çıktı hedefin veri türüne ve/veya seri hale getirme türü özelleştirebilirsiniz.

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

## <a name="test-a-stream-analytics-output-target"></a>Stream Analytics Çıkış hedefini test
Stream Analytics Çıkış hedefini de sahip **TestConnection** bağlantıları sınama yöntemi.

    // Test output target connection
    DataSourceTestConnectionResponse outputTestResponse =
        client.Outputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsOutputName);

## <a name="create-a-stream-analytics-transformation"></a>Stream Analytics dönüşüm oluşturun
Aşağıdaki kod bir akış analizi dönüştürmesi sorguyla oluşturur "seçin * giriş öğesinden" ve akış analizi işi için bir akış birimi ayırmak için belirtir. Akış birimleri ayarlama hakkında daha fazla bilgi için bkz: [ölçek Azure akış analizi işi](stream-analytics-scale-jobs.md).

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

Giriş ve çıkış gibi bir dönüşüm de altında oluşturulan belirli Stream Analytics işi bağlıdır.

## <a name="start-a-stream-analytics-job"></a>Akış analizi işi Başlat
Akış analizi işi ve kendi input(s), çıkışa ve dönüştürme oluşturduktan sonra iş çağırarak başlatabilirsiniz **Başlat** yöntemi.

Aşağıdaki örnek kod başlatır Stream Analytics işi özel çıkış başlangıç saati ile 12:12:12 12 Kasım 2012 için ayarlanmış UTC:

    // Start a Stream Analytics job
    JobStartParameters jobStartParameters = new JobStartParameters
    {
        OutputStartMode = OutputStartMode.CustomTime,
        OutputStartTime = new DateTime(2012, 12, 12, 0, 0, 0, DateTimeKind.Utc)
    };

    LongRunningOperationResponse jobStartResponse = client.StreamingJobs.Start(resourceGroupName, streamAnalyticsJobName, jobStartParameters);

## <a name="stop-a-stream-analytics-job"></a>Stream Analytics işini durdurma
Çağırarak çalışan bir akış analizi işi durdurabilirsiniz **durdurmak** yöntemi.

    // Stop a Stream Analytics job
    LongRunningOperationResponse jobStopResponse = client.StreamingJobs.Stop(resourceGroupName, streamAnalyticsJobName);

## <a name="delete-a-stream-analytics-job"></a>Akış analizi işi Sil
**Silmek** yöntemi input(s), çıkışa ve dönüşüm işinin dahil olmak üzere temel alınan bir alt kaynaklara yanı sıra iş siler.

    // Delete a Stream Analytics job
    LongRunningOperationResponse jobDeleteResponse = client.StreamingJobs.Delete(resourceGroupName, streamAnalyticsJobName);

## <a name="get-support"></a>Destek alın
Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

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
