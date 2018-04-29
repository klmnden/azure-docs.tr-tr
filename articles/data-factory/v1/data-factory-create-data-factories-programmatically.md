---
title: Azure .NET SDK kullanarak veri ardışık düzen oluşturun | Microsoft Docs
description: Program aracılığıyla oluşturmak, izlemek ve Data Factory SDK'yı kullanarak Azure data factory'leri yönetme hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: b0a357be-3040-4789-831e-0d0a32a0bda5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 5de6c401ddbefbe66e23abb99f389e505d9b5120
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="create-monitor-and-manage-azure-data-factories-using-azure-data-factory-net-sdk"></a>Oluşturma, izlemek ve Azure Data Factory .NET SDK kullanarak Azure data factory'leri yönetme
> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Data Factory hizmetinin önizleme aşamasında olan 2. sürümünü kullanıyorsanız [sürüm 2’de kopyalama etkinliği öğreticisi belgeleri](../quickstart-create-data-factory-dot-net.md) konusunu inceleyin. 

## <a name="overview"></a>Genel Bakış
Oluşturun, izlemek ve program aracılığıyla Data Factory .NET SDK kullanarak Azure data factory'leri yönetin. Bu makalede, oluşturur ve bir veri fabrikası izler örnek bir .NET konsol uygulaması oluşturmak için izleyebileceğiniz bir kılavuz içerir. 

> [!NOTE]
> Bu makale, Data Factory .NET API’nin tamamını kapsamaz. Bkz: [Data Factory .NET API Başvurusu](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1) hakkında kapsamlı bilgi için .NET API veri fabrikası için. 

## <a name="prerequisites"></a>Önkoşullar
* Visual Studio 2012 veya 2013 veya 2015
* İndirme ve yükleme [Azure .NET SDK'sı](http://azure.microsoft.com/downloads/).
* Azure PowerShell. Bilgisayarınıza Azure PowerShell’i yüklemek için [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview) makalesindeki yönergeleri izleyin. Azure PowerShell’i kullanarak bir Azure Active Directory uygulaması oluşturursunuz.

### <a name="create-an-application-in-azure-active-directory"></a>Azure Active Directory’de uygulama oluşturma
Bir Azure Active Directory uygulaması oluşturun, uygulama için bir hizmet sorumlusu oluşturun ve bunu **Data Factory Katılımcısı** rolüne atayın.

1. **PowerShell**’i başlatın.
2. Aşağıdaki komutu çalıştırın ve Azure portalda oturum açmak için kullandığınız kullanıcı adı ve parolayı girin.

    ```PowerShell
    Connect-AzureRmAccount
    ```
3. Bu hesapla ilgili tüm abonelikleri görmek için aşağıdaki komutu çalıştırın.

    ```PowerShell
    Get-AzureRmSubscription
    ```
4. Çalışmak isteğiniz aboneliği seçmek için aşağıdaki komutu çalıştırın. **&lt;NameOfAzureSubscription**&gt; değerini Azure aboneliğinizin adıyla değiştirin.

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```

   > [!IMPORTANT]
   > Bu komutun çıktısından **SubscriptionId** ve **TenantId** değerlerin not alın.

5. PowerShell’de aşağıdaki komutu çalıştırarak **ADFTutorialResourceGroup** adlı bir Azure kaynak grubu oluşturun.

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```

    Kaynak grubu zaten varsa bunun güncelleştirileceğini mi (Y) yoksa (N) olarak tutulacağını mı belirtirsiniz.

    Farklı bir kaynak grubu kullanıyorsanız, bu öğreticide kullanılan ADFTutorialResourceGroup yerine kaynak grubunuzun adını kullanmanız gerekir.
6. Bir Azure Active Directory uygulaması oluşturun.

    ```PowerShell
    $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFDotNetWalkthroughApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfdotnetwalkthroughapp.org/example" -Password "Pass@word1"
    ```

    Aşağıdaki hatayı alırsanız farklı bir URL belirtip komutu yeniden çalıştırın.
    
    ```PowerShell
    Another object with the same value for property identifierUris already exists.
    ```
7. AD hizmet sorumlusunu oluşturun.

    ```PowerShell
    New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    ```
8. Hizmet sorumlusunu **Data Factory Katılımcısı** rolüne ekleyin.

    ```PowerShell
    New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    ```
9. Uygulama kimliğini alın.

    ```PowerShell
    $azureAdApplication 
    ```
    Çıktıdaki uygulama kimliğini (applicationID) not alın.

Bu adımlardan sonra aşağıdaki dört değere sahip olmanız gerekir:

* Kiracı Kimliği
* Abonelik Kimliği
* Uygulama Kimliği
* Parola (ik komutta belirtilir)

## <a name="walkthrough"></a>Kılavuz
Kılavuzda data factory kopyalama etkinliği içeren sahip işlem hattı oluşturun. Kopyalama etkinliği verileri Azure blob depolama alanınızın bir klasörde aynı blob depolama başka bir klasöre kopyalar. 

Kopyalama Etkinliği, Azure Data Factory’de veri hareketini gerçekleştirir. Etkinlik, çeşitli veri depolama alanları arasında güvenli, güvenilir ve ölçeklenebilir bir yolla veri kopyalayabilen genel olarak kullanılabilir bir hizmet tarafından desteklenir. Kopyalama etkinliği hakkında ayrıntılı bilgi için [Veri Taşıma Etkinlikleri](data-factory-data-movement-activities.md) makalesine bakın.

1. Visual Studio 2012/2013/2015'i kullanarak bir C# .NET konsol uygulaması oluşturun.
   1. **Visual Studio** 2012/2013/2015’i başlatın.
   2. **Dosya**’ya tıklayın, **Yeni**’nin üzerine gelin ve **Proje**’ye tıklayın.
   3. **Şablonlar**’ı genişletin ve **Visual C#** seçeneğini belirleyin. Bu kılavuzda C# kullanıyor olsanız da dilediğiniz .NET dilini kullanabilirsiniz.
   4. Sağ taraftaki proje türleri listesinden **Konsol Uygulaması**’nı seçin.
   5. Ad için **DataFactoryAPITestApp** değerini girin.
   6. Konum için **C:\ADFGetStarted** yolunu seçin.
   7. Projeyi oluşturmak için **Tamam**'a tıklayın.
2. **Araçlar**'a tıklayın, **NuGet Paket Yöneticisi**'nin üzerine gelin ve ardından **Paket Yöneticisi Konsolu**'na tıklayın.
3. **Paket Yöneticisi Konsolu**'nda şu adımları uygulayın:
   1. Data Factory paketini yüklemek için şu komutu çalıştırın: `Install-Package Microsoft.Azure.Management.DataFactories`
   2. Azure Active Directory paketini yüklemek için şu komutu çalıştırın (kodda Active Directory API'sini kullanırsınız): `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`
4. Değiştir **App.config** aşağıdaki içeriğe sahip proje dosyasında: 
    
    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="ActiveDirectoryEndpoint" value="https://login.microsoftonline.com/" />
            <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
            <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

            <add key="ApplicationId" value="your application ID" />
            <add key="Password" value="Password you used while creating the AAD application" />
            <add key="SubscriptionId" value= "Subscription ID" />
            <add key="ActiveDirectoryTenantId" value="Tenant ID" />
        </appSettings>
    </configuration>
    ```
5. App.Config dosyasında değerlerini güncelleştirin  **&lt;uygulama kimliği&gt;**,  **&lt;parola&gt;**,  **&lt;abonelik kimliği&gt;**, ve **&lt;kimliği Kiracı&gt;** kendi değerlere sahip.
6. Aşağıdakileri ekleyin **kullanarak** deyimlerini **Program.cs** proje dosyasında.

    ```csharp
    using System.Configuration;
    using System.Collections.ObjectModel;
    using System.Threading;
    using System.Threading.Tasks;

    using Microsoft.Azure;
    using Microsoft.Azure.Management.DataFactories;
    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Common.Models;

    using Microsoft.IdentityModel.Clients.ActiveDirectory;

    ```
6. **DataPipelineManagementClient** sınıfının bir örneğini oluşturan aşağıdaki kodu **Main** yöntemine ekleyin. Bir veri fabrikası, bağlı hizmet, girdi ve çıktı veri kümeleri ve işlem hattı oluşturmak için bu nesneyi kullanırsınız. Çalışma zamanında bir veri kümesinin dilimlerini izlemek için de bu nesneyi kullanırsınız.

    ```csharp
    // create data factory management client

    //IMPORTANT: specify the name of Azure resource group here
    string resourceGroupName = "ADFTutorialResourceGroup";

    //IMPORTANT: the name of the data factory must be globally unique.
    // Therefore, update this value. For example:APITutorialFactory05122017
    string dataFactoryName = "APITutorialFactory";

    TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader().Result);

    Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);
    ```

   > [!IMPORTANT]
   > **resourceGroupName** değerini Azure kaynak grubunuzun adıyla değiştirin. Kullanarak bir kaynak grubu oluşturabilirsiniz [New-AzureResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet'i.
   >
   > Veri fabrikasının adını (dataFactoryName) benzersiz olacak şekilde güncelleştirin. Veri fabrikasının adı genel olarak benzersiz olmalıdır. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](data-factory-naming-rules.md) konusuna bakın.
7. Bir **veri fabrikası** oluşturan aşağıdaki kodu **Main** yöntemine ekleyin.

    ```csharp
    // create a data factory
    Console.WriteLine("Creating a data factory");
    client.DataFactories.CreateOrUpdate(resourceGroupName,
        new DataFactoryCreateOrUpdateParameters()
        {
            DataFactory = new DataFactory()
            {
                Name = dataFactoryName,
                Location = "westus",
                Properties = new DataFactoryProperties()
            }
        }
    );
    ```
8. Bir **Azure Depolama bağlı hizmeti** oluşturan aşağıdaki kodu **Main** yöntemine ekleyin.

   > [!IMPORTANT]
   > **storageaccountname** ve **accountkey** sözcüklerini Azure Depolama hesabınızın adı ve anahtarıyla değiştirin.

    ```csharp
    // create a linked service for input data store: Azure Storage
    Console.WriteLine("Creating Azure Storage linked service");
    client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new LinkedServiceCreateOrUpdateParameters()
        {
            LinkedService = new LinkedService()
            {
                Name = "AzureStorageLinkedService",
                Properties = new LinkedServiceProperties
                (
                    new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<accountkey>")
                )
            }
        }
    );
    ```
9. **Girdi ve çıktı veri kümeleri** oluşturan aşağıdaki kodu **Main** yöntemine ekleyin.

    **FolderPath** giriş blob ayarlanmıştır **adftutorial /** nerede **adftutorial** blob depolama alanınızın kapsayıcısında adıdır. Bu kapsayıcı, Azure blob depolamada mevcut değilse, bu ada sahip bir kapsayıcı oluşturmak: **adftutorial** ve kapsayıcıya bir metin dosyasını karşıya yükleyin.

    Çıktı blob FolderPath ayarlamak: **adftutorial/apifactoryoutput / {dilim}** nerede **dilim** dinamik olarak değeri temel alınarak hesaplanır **SliceStart** (tarih-saat her dilimin başlatın.)

    ```csharp
    // create input and output datasets
    Console.WriteLine("Creating input and output datasets");
    string Dataset_Source = "DatasetBlobSource";
    string Dataset_Destination = "DatasetBlobDestination";
    
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
    new DatasetCreateOrUpdateParameters()
    {
        Dataset = new Dataset()
        {
            Name = Dataset_Source,
            Properties = new DatasetProperties()
            {
                LinkedServiceName = "AzureStorageLinkedService",
                TypeProperties = new AzureBlobDataset()
                {
                    FolderPath = "adftutorial/",
                    FileName = "emp.txt"
                },
                External = true,
                Availability = new Availability()
                {
                    Frequency = SchedulePeriod.Hour,
                    Interval = 1,
                },
    
                Policy = new Policy()
                {
                    Validation = new ValidationPolicy()
                    {
                        MinimumRows = 1
                    }
                }
            }
        }
    });
    
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
    new DatasetCreateOrUpdateParameters()
    {
        Dataset = new Dataset()
        {
            Name = Dataset_Destination,
            Properties = new DatasetProperties()
            {
    
                LinkedServiceName = "AzureStorageLinkedService",
                TypeProperties = new AzureBlobDataset()
                {
                    FolderPath = "adftutorial/apifactoryoutput/{Slice}",
                    PartitionedBy = new Collection<Partition>()
                    {
                        new Partition()
                        {
                            Name = "Slice",
                            Value = new DateTimePartitionValue()
                            {
                                Date = "SliceStart",
                                Format = "yyyyMMdd-HH"
                            }
                        }
                    }
                },
    
                Availability = new Availability()
                {
                    Frequency = SchedulePeriod.Hour,
                    Interval = 1,
                },
            }
        }
    });
    ```
10. **Bir işlem hattı oluşturan ve işlem hattını etkinleştiren** aşağıdaki kodu **Main** yöntemine ekleyin. Bu işlem hattının **BlobSource**’u bir kaynak olarak, **BlobSink**’i ise bir havuz olarak alan bir **CopyActivity** etkinliği vardır.

    Kopyalama Etkinliği, Azure Data Factory’de veri hareketini gerçekleştirir. Etkinlik, çeşitli veri depolama alanları arasında güvenli, güvenilir ve ölçeklenebilir bir yolla veri kopyalayabilen genel olarak kullanılabilir bir hizmet tarafından desteklenir. Kopyalama etkinliği hakkında ayrıntılı bilgi için [Veri Taşıma Etkinlikleri](data-factory-data-movement-activities.md) makalesine bakın.

    ```csharp
    // create a pipeline
    Console.WriteLine("Creating a pipeline");
    DateTime PipelineActivePeriodStartTime = new DateTime(2014, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
    DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
    string PipelineName = "PipelineBlobSample";
    
    client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
    new PipelineCreateOrUpdateParameters()
    {
        Pipeline = new Pipeline()
        {
            Name = PipelineName,
            Properties = new PipelineProperties()
            {
                Description = "Demo Pipeline for data transfer between blobs",
    
                // Initial value for pipeline's active period. With this, you won't need to set slice status
                Start = PipelineActivePeriodStartTime,
                End = PipelineActivePeriodEndTime,
    
                Activities = new List<Activity>()
                {
                    new Activity()
                    {
                        Name = "BlobToBlob",
                        Inputs = new List<ActivityInput>()
                        {
                            new ActivityInput()
                {
                                Name = Dataset_Source
                            }
                        },
                        Outputs = new List<ActivityOutput>()
                        {
                            new ActivityOutput()
                            {
                                Name = Dataset_Destination
                            }
                        },
                        TypeProperties = new CopyActivity()
                        {
                            Source = new BlobSource(),
                            Sink = new BlobSink()
                            {
                                WriteBatchSize = 10000,
                                WriteBatchTimeout = TimeSpan.FromMinutes(10)
                            }
                        }
                    }
    
                },
            }
        }
    });
    ```
12. Çıktı veri kümesinin veri diliminin durumunu almak için aşağıdaki kodu **Main** yöntemine ekleyin. Bu örnekte beklenen yalnızca bir dilim yoktur.

    ```csharp
    // Pulling status within a timeout threshold
    DateTime start = DateTime.Now;
    bool done = false;
    
    while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
    {
        Console.WriteLine("Pulling the slice status");
        // wait before the next status check
        Thread.Sleep(1000 * 12);
    
        var datalistResponse = client.DataSlices.List(resourceGroupName, dataFactoryName, Dataset_Destination,
            new DataSliceListParameters()
            {
                DataSliceRangeStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString(),
                DataSliceRangeEndTime = PipelineActivePeriodEndTime.ConvertToISO8601DateTimeString()
            });
    
        foreach (DataSlice slice in datalistResponse.DataSlices)
        {
            if (slice.State == DataSliceState.Failed || slice.State == DataSliceState.Ready)
            {
                Console.WriteLine("Slice execution is done with status: {0}", slice.State);
                done = true;
                break;
            }
            else
            {
                Console.WriteLine("Slice status is: {0}", slice.State);
            }
        }
    }
    ```
13. **(isteğe bağlı)**  Bir veri dilimi ayrıntılarını çalıştırmak için aşağıdaki kodu ekleyin **ana** yöntemi.

    ```csharp
    Console.WriteLine("Getting run details of a data slice");
    
    // give it a few minutes for the output slice to be ready
    Console.WriteLine("\nGive it a few minutes for the output slice to be ready and press any key.");
    Console.ReadKey();
    
    var datasliceRunListResponse = client.DataSliceRuns.List(
        resourceGroupName,
        dataFactoryName,
        Dataset_Destination,
        new DataSliceRunListParameters()
        {
            DataSliceStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString()
        });
    
    foreach (DataSliceRun run in datasliceRunListResponse.DataSliceRuns)
    {
        Console.WriteLine("Status: \t\t{0}", run.Status);
        Console.WriteLine("DataSliceStart: \t{0}", run.DataSliceStart);
        Console.WriteLine("DataSliceEnd: \t\t{0}", run.DataSliceEnd);
        Console.WriteLine("ActivityId: \t\t{0}", run.ActivityName);
        Console.WriteLine("ProcessingStartTime: \t{0}", run.ProcessingStartTime);
        Console.WriteLine("ProcessingEndTime: \t{0}", run.ProcessingEndTime);
        Console.WriteLine("ErrorMessage: \t{0}", run.ErrorMessage);
    }
    
    Console.WriteLine("\nPress any key to exit.");
    Console.ReadKey();
    ```
14. **Main** yöntemi tarafından kullanılan aşağıdaki yardımcı yöntemini **Program** sınıfına ekleyin. Bu yöntem sağlamanıza olanak tanıyan bir iletişim kutusu açılır **kullanıcı adı** ve **parola** , Azure portalında oturum açmak için kullanın.

    ```csharp
    public static async Task<string> GetAuthorizationHeader()
    {
        AuthenticationContext context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
        ClientCredential credential = new ClientCredential(
            ConfigurationManager.AppSettings["ApplicationId"],
            ConfigurationManager.AppSettings["Password"]);
        AuthenticationResult result = await context.AcquireTokenAsync(
            resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
            clientCredential: credential);

        if (result != null)
            return result.AccessToken;

        throw new InvalidOperationException("Failed to acquire token");
    }
    ```

15. Çözüm Gezgini'nde projenizi genişletin: **DataFactoryAPITestApp**, sağ **başvuruları**, tıklatıp **Başvuru Ekle**. İçin bu onay kutusunu işaretleyin `System.Configuration` derleme ve tıklatın **Tamam**.
15. Konsol uygulamasını derleyin. Menüde **Derle**’ye tıklayın ve **Çözümü Derle**’ye tıklayın.
16. Olduğunu en az bir dosya adftutorial kapsayıcısında Azure blob depolama alanınızın onaylayın. Aksi durumda, Emp.txt dosyasını Not Defteri'nde aşağıdaki içerik ile oluşturun ve adftutorial kapsayıcıya yükleyin.

    ```
    John, Doe
    Jane, Doe
    ```
17. Menüden **Hata Ayıkla** -> **Hata Ayıklamayı Başlat**’a tıklayarak örneği çalıştırın. **Getting run details of a data slice** iletisini gördüğünüzde birkaç dakika bekleyin ve **ENTER** tuşuna basın.
18. Azure portalı kullanarak **APITutorialFactory** veri fabrikasının aşağıdaki yapıtlarla birlikte oluşturulduğunu doğrulayın:
    * Bağlantılı hizmeti: **AzureStorageLinkedService**
    * Veri kümesi: **DatasetBlobSource** ve **DatasetBlobDestination**.
    * İşlem hattı: **PipelineBlobSample**
19. Bir çıkış dosyası oluşturulur doğrulayın **apifactoryoutput** klasöründe **adftutorial** kapsayıcı.

## <a name="get-a-list-of-failed-data-slices"></a>Başarısız olan veri dilimlerinin listesini al 

```csharp
// Parse the resource path
var ResourceGroupName = "ADFTutorialResourceGroup";
var DataFactoryName = "DataFactoryAPITestApp";

var parameters = new ActivityWindowsByDataFactoryListParameters(ResourceGroupName, DataFactoryName);
parameters.WindowState = "Failed";
var response = dataFactoryManagementClient.ActivityWindows.List(parameters);
do
{
    foreach (var activityWindow in response.ActivityWindowListResponseValue.ActivityWindows)
    {
        var row = string.Join(
            "\t",
            activityWindow.WindowStart.ToString(),
            activityWindow.WindowEnd.ToString(),
            activityWindow.RunStart.ToString(),
            activityWindow.RunEnd.ToString(),
            activityWindow.DataFactoryName,
            activityWindow.PipelineName,
            activityWindow.ActivityName,
            string.Join(",", activityWindow.OutputDatasets));
        Console.WriteLine(row);
    }

    if (response.NextLink != null)
    {
        response = dataFactoryManagementClient.ActivityWindows.ListNext(response.NextLink, parameters);
    }
    else
    {
        response = null;
    }
}
while (response != null);
```

## <a name="next-steps"></a>Sonraki adımlar
Verileri Azure blob depolama alanından Azure SQL veritabanına kopyalar .NET SDK kullanarak bir işlem hattı oluşturmak için aşağıdaki örneğe bakın: 

- [Blob depolama alanından SQL veritabanına veri kopyalamak için bir işlem hattı oluşturma](data-factory-copy-activity-tutorial-using-dotnet-api.md)
