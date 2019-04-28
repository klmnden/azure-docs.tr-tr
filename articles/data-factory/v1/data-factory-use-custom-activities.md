---
title: Bir Azure Data Factory işlem hattında özel etkinlikler kullanma
description: Özel etkinlikler oluşturur ve bunları bir Azure Data Factory işlem hattında kullanma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
ms.assetid: 8dd7ba14-15d2-4fd9-9ada-0b2c684327e9
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
author: nabhishek
ms.author: abnarain
manager: craigg
robots: noindex
ms.openlocfilehash: 0ddc235064d99e9d6385ab48e78f893952eefa15
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61254827"
---
# <a name="use-custom-activities-in-an-azure-data-factory-pipeline"></a>Bir Azure Data Factory işlem hattında özel etkinlikler kullanma
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](data-factory-use-custom-activities.md)
> * [Sürüm 2 (geçerli sürüm)](../transform-data-using-dotnet-custom-activity.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [V2'de özel etkinlikler](../transform-data-using-dotnet-custom-activity.md).

Kullanabileceğiniz bir Azure Data Factory işlem hattı etkinlikleri iki tür vardır.

- [Veri taşıma etkinlikleri](data-factory-data-movement-activities.md) arasında veri taşımak için [kaynak ve havuz veri deposu desteklenen](data-factory-data-movement-activities.md#supported-data-stores-and-formats).
- [Veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md) verileri dönüştürmek için kullanma gibi işlem hizmetlerini Azure HDInsight, Azure Batch ve Azure Machine Learning.

Data Factory desteklemediği bir veri deposundan/veri taşımak için oluşturma bir **özel etkinlik** veri taşıma mantığı ve bir işlem hattındaki etkinliğin kullanın. Benzer şekilde, dönüştürebilen/Data Factory tarafından desteklenmeyen bir şekilde verileri için özel bir etkinlik ile kendi veri dönüştürme mantığını oluşturun ve bir işlem hattında etkinlik kullanma.

Çalıştırmak için özel bir etkinlik yapılandırabileceğiniz bir **Azure Batch** sanal makine havuzu. Azure Batch kullanırken, mevcut bir Azure Batch havuzu kullanabilirsiniz.

Aşağıdaki yönergeler, özel bir .NET etkinliği oluşturmayı ve bir işlem hattında özel etkinlik kullanmak için adım adım yönergeler sağlar. Bu izlenecek yolda bir **Azure Batch** bağlı hizmeti.

> [!IMPORTANT]
> - Şirket içi veri kaynaklarına erişmek için özel bir etkinlik bir veri yönetimi ağ geçidini kullanmak mümkün değildir. Şu anda [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) veri fabrikasında kopyalama etkinliği, saklı yordam etkinliği yalnızca destekler.

## <a name="walkthrough-create-a-custom-activity"></a>İzlenecek yol: özel etkinlik oluşturma
### <a name="prerequisites"></a>Önkoşullar
* Visual Studio 2012/2013/2015
* [Azure .NET SDK](https://azure.microsoft.com/downloads/)’yı indirip yükleyin

### <a name="azure-batch-prerequisites"></a>Azure Batch önkoşulları
Kılavuzda, Azure Batch bir işlem kaynağı kullanarak, özel .NET etkinlikleri çalıştırın. **Azure Batch**, büyük ölçekli paralel ve yüksek performanslı bilgi işlem (HPC) uygulamalarını bulutta verimli bir şekilde çalıştırmanızı sağlayan bir platform hizmetidir. Azure Batch, yönetilen üzerinde çalıştırılacak işlem yoğunluklu işi zamanlar **sanal makine koleksiyonunu**, ve ölçek işlerinizi ihtiyaçlarını karşılamak için kaynakları işlem otomatik olarak. Bkz: [Azure Batch temel bilgileri] [ batch-technical-overview] Azure Batch hizmetinin ayrıntılı bir genel bakış makalesi.

Öğreticide, VM'lerin bir havuzla Azure Batch hesabı oluşturun. Adımlar aşağıdaki gibidir:

1. Oluşturma bir **Azure Batch hesabı** kullanarak [Azure portalında](https://portal.azure.com). Bkz: [oluşturun ve bir Azure Batch hesap] [ batch-create-account] makaledeki yönergelere.
2. Azure Batch hesabı adı, hesap anahtarı, URI ve havuz adı unutmayın. Bir Azure Batch bağlı hizmeti oluşturmak için ihtiyaç.
    1. Azure Batch hesabı için giriş sayfasında, gördüğünüz bir **URL** şu biçimde: `https://myaccount.westus.batch.azure.com`. Bu örnekte, **myaccount** Azure Batch hesabının adıdır. URI bağlı hizmet tanımında kullandığınız hesabın adı olmadan URL'dir. Örneğin: `https://<region>.batch.azure.com`.
    2. Tıklayın **anahtarları** sol menü ve kopyalama **birincil erişim anahtarı**.
    3. Mevcut havuzlardan kullanmak için **havuzları** menü ve not edin **kimliği** havuzu. Mevcut bir havuza sahip değilseniz, sonraki adıma taşıyın.
2. Oluşturma bir **Azure Batch havuzu**.

   1. İçinde [Azure portalında](https://portal.azure.com), tıklayın **Gözat** sol menü seçeneğine tıklayıp **Batch hesapları**.
   2. Azure Batch hesabınızı açmak için seçin **Batch hesabı** dikey penceresi.
   3. Tıklayın **havuzları** Döşe.
   4. İçinde **havuzları** dikey penceresinde, bir havuzu eklemek için araç Ekle düğmesine tıklayın.
      1. Havuz (Havuz kimliği) için bir kimlik girin. Not **havuzun kimliği**; Data Factory çözümü oluştururken gerekir.
      2. Belirtin **Windows Server 2012 R2** işletim sistemi ailesi ayarı için.
      3. Seçin bir **fiyatlandırma katmanında düğüm**.
      4. Girin **2** olarak değer **hedef adanmış** ayarı.
      5. Girin **2** olarak değer **düğüm başına en fazla görev** ayarı.
   5. Havuzu oluşturmak için **Tamam**'a tıklayın.
   6. Not **kimliği** havuzu.

### <a name="high-level-steps"></a>Üst düzey adımları
Bu kılavuzda bir parçası olarak gerçekleştireceğiniz iki üst düzey adımlar şunlardır:

1. Basit veri dönüştürme/işleme mantığı içeren özel bir etkinlik oluşturursunuz.
2. Özel Etkinlik kullanan bir işlem hattı ile bir Azure veri fabrikası oluşturun.

### <a name="create-a-custom-activity"></a>Özel etkinlik oluşturma
.NET özel etkinliği oluşturmak için bir **.NET sınıf kitaplığı** uygulayan bir sınıf ile proje **Idotnetactivity** arabirimi. Bu arabirim, yalnızca bir yöntemi vardır: [Yürütme](https://msdn.microsoft.com/library/azure/mt603945.aspx) ve imzası:

```csharp
public IDictionary<string, string> Execute(
    IEnumerable<LinkedService> linkedServices,
    IEnumerable<Dataset> datasets,
    Activity activity,
    IActivityLogger logger)
```

Yöntemi dört parametre alır:

- **linkedServices**. Bu özellik etkinlik için giriş/çıkış veri kümesi tarafından başvurulan veri Store bağlı Hizmetleri numaralandırılabilir bir listesidir.
- **veri kümeleri**. Bu özellik etkinlik için giriş/çıkış veri kümesi sıralanabilir bir listesi verilmiştir. Bu parametre, girdi ve çıktı veri kümeleri tarafından tanımlı şemalar ve konumları almak için kullanabilirsiniz.
- **Etkinlik**. Bu özellik geçerli etkinliği temsil eder. Özel Etkinlik ile ilişkili genişletilmiş özelliklere erişmek için kullanılabilir. Bkz: [genişletilmiş özellikler erişim](#access-extended-properties) Ayrıntılar için.
- **Günlükçü**. Bu nesne hata ayıklama açıklamaları, işlem hattının kullanıcı günlüğünde, surface yazmanızı sağlar.

Bu yöntem, özel etkinlikler gelecekte zincir için kullanılan bir sözlüğü döndürür. Bu özellik henüz uygulanmadı, bu nedenle boş bir sözlük yöntemi döndürür.

### <a name="procedure"></a>Yordam
1. Oluşturma bir **.NET sınıf kitaplığı** proje.
   <ol type="a">
     <li>Başlatma <b>Visual Studio 2017</b> veya <b>Visual Studio 2015</b> veya <b>Visual Studio 2013'ün</b> veya <b>Visual Studio 2012</b>.</li>
     <li><b>Dosya</b>’ya tıklayın, <b>Yeni</b>’nin üzerine gelin ve <b>Proje</b>’ye tıklayın.</li>
     <li><b>Şablonlar</b>’ı genişletin ve <b>Visual C#</b> seçeneğini belirleyin. Bu izlenecek yolda, C# kullanıyor, ancak özel etkinlik geliştirmek için dilediğiniz .NET dilini kullanabilirsiniz.</li>
     <li>Seçin <b>sınıf kitaplığı</b> sağ taraftaki proje türleri listesinden. VS 2017'deki seçin <b>sınıf kitaplığı (.NET Framework)</b> </li>
     <li>Girin <b>MyDotNetActivity</b> için <b>adı</b>.</li>
     <li>Seçin <b>C:\ADFGetStarted</b> için <b>konumu</b>.</li>
     <li>Projeyi oluşturmak için <b>Tamam</b>'a tıklayın.</li>
   </ol>

2. **Araçlar**'a tıklayın, **NuGet Paket Yöneticisi**'nin üzerine gelin ve ardından **Paket Yöneticisi Konsolu**'na tıklayın.

3. Paket Yöneticisi Konsolu'nda içeri aktarmak için aşağıdaki komutu yürütün **Microsoft.Azure.Management.DataFactories**.

    ```powershell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. İçeri aktarma **Azure depolama** NuGet paketini projeye.

    ```powershell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    > [!IMPORTANT]
    > Data Factory hizmeti Başlatıcısı WindowsAzure.Storage 4.3 sürümünü gerektirir. Özel Etkinlik projenizde Azure depolama derlemenin daha yeni bir sürümünü bir başvuru ekleyin, etkinlik yürütüldüğünde, bir hata görürsünüz. Hatayı gidermek için bkz: [Appdomain yalıtım](#appdomain-isolation) bölümü.
5. Aşağıdaki **kullanarak** projedeki kaynak dosyası deyimleriyle.

    ```csharp

    // Comment these lines if using VS 2017
    using System.IO;
    using System.Globalization;
    using System.Diagnostics;
    using System.Linq;
    // --------------------

    // Comment these lines if using <= VS 2015
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    // ---------------------

    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Runtime;

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```
6. Adını değiştirmek **ad alanı** için **MyDotNetActivityNS**.

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. İçin sınıfın adını değiştirmek **MyDotNetActivity** ve türetmeniz **Idotnetactivity** arabirimi aşağıdaki kod parçacığında gösterildiği gibi:

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. Uygulama (Ekle) **yürütme** yöntemi **Idotnetactivity** arabirimini **MyDotNetActivity** sınıfı ve aşağıdaki örnek kod yönteme kopyalayın.

    Aşağıdaki örnek, bir veri dilimi ile ilişkili her BLOB ("Microsoft") arama terimi oluşum sayısını sayar.

    ```csharp
    /// <summary>
    /// Execute method is the only method of IDotNetActivity interface you must implement.
    /// In this sample, the method invokes the Calculate method to perform the core logic.
    /// </summary>

    public IDictionary<string, string> Execute(
        IEnumerable<LinkedService> linkedServices,
        IEnumerable<Dataset> datasets,
        Activity activity,
        IActivityLogger logger)
    {
        // get extended properties defined in activity JSON definition
        // (for example: SliceStart)
        DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
        string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];

        // to log information, use the logger object
        // log all extended properties
        IDictionary<string, string> extendedProperties = dotNetActivity.ExtendedProperties;
        logger.Write("Logging extended properties if any...");
        foreach (KeyValuePair<string, string> entry in extendedProperties)
        {
            logger.Write("<key:{0}> <value:{1}>", entry.Key, entry.Value);
        }

        // linked service for input and output data stores
        // in this example, same storage is used for both input/output
        AzureStorageLinkedService inputLinkedService;

        // get the input dataset
        Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);

        // declare variables to hold type properties of input/output datasets
        AzureBlobDataset inputTypeProperties, outputTypeProperties;

        // get type properties from the dataset object
        inputTypeProperties = inputDataset.Properties.TypeProperties as AzureBlobDataset;

        // log linked services passed in linkedServices parameter
        // you will see two linked services of type: AzureStorage
        // one for input dataset and the other for output dataset
        foreach (LinkedService ls in linkedServices)
            logger.Write("linkedService.Name {0}", ls.Name);

        // get the first Azure Storage linked service from linkedServices object
        // using First method instead of Single since we are using the same
        // Azure Storage linked service for input and output.
        inputLinkedService = linkedServices.First(
            linkedService =>
            linkedService.Name ==
            inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
            as AzureStorageLinkedService;

        // get the connection string in the linked service
        string connectionString = inputLinkedService.ConnectionString;

        // get the folder path from the input dataset definition
        string folderPath = GetFolderPath(inputDataset);
        string output = string.Empty; // for use later.

        // create storage client for input. Pass the connection string.
        CloudStorageAccount inputStorageAccount = CloudStorageAccount.Parse(connectionString);
        CloudBlobClient inputClient = inputStorageAccount.CreateCloudBlobClient();

        // initialize the continuation token before using it in the do-while loop.
        BlobContinuationToken continuationToken = null;
        do
        {   // get the list of input blobs from the input storage client object.
            BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                     true,
                                     BlobListingDetails.Metadata,
                                     null,
                                     continuationToken,
                                     null,
                                     null);

            // Calculate method returns the number of occurrences of
            // the search term (“Microsoft”) in each blob associated
            // with the data slice. definition of the method is shown in the next step.

            output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");

        } while (continuationToken != null);

        // get the output dataset using the name of the dataset matched to a name in the Activity output collection.
        Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);

        // get type properties for the output dataset
        outputTypeProperties = outputDataset.Properties.TypeProperties as AzureBlobDataset;

        // get the folder path from the output dataset definition
        folderPath = GetFolderPath(outputDataset);

        // log the output folder path
        logger.Write("Writing blob to the folder: {0}", folderPath);

        // create a storage object for the output blob.
        CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
        // write the name of the file.
        Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));

        // log the output file name
        logger.Write("output blob URI: {0}", outputBlobUri.ToString());

        // create a blob and upload the output text.
        CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
        logger.Write("Writing {0} to the output blob", output);
        outputBlob.UploadText(output);

        // The dictionary can be used to chain custom activities together in the future.
        // This feature is not implemented yet, so just return an empty dictionary.

        return new Dictionary<string, string>();
    }
    ```
9. Aşağıdaki yardımcı yöntemler ekleyin:

    ```csharp
    /// <summary>
    /// Gets the folderPath value from the input/output dataset.
    /// </summary>

    private static string GetFolderPath(Dataset dataArtifact)
    {
        if (dataArtifact == null || dataArtifact.Properties == null)
        {
            return null;
        }

        // get type properties of the dataset
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }

        // return the folder path found in the type properties
        return blobDataset.FolderPath;
    }

    /// <summary>
    /// Gets the fileName value from the input/output dataset.
    /// </summary>

    private static string GetFileName(Dataset dataArtifact)
    {
        if (dataArtifact == null || dataArtifact.Properties == null)
        {
            return null;
        }

        // get type properties of the dataset
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }

        // return the blob/file name in the type properties
        return blobDataset.FileName;
    }

    /// <summary>
    /// Iterates through each blob (file) in the folder, counts the number of instances of search term in the file,
    /// and prepares the output text that is written to the output blob.
    /// </summary>

    public static string Calculate(BlobResultSegment Bresult, IActivityLogger logger, string folderPath, ref BlobContinuationToken token, string searchTerm)
    {
        string output = string.Empty;
        logger.Write("number of blobs found: {0}", Bresult.Results.Count<IListBlobItem>());
        foreach (IListBlobItem listBlobItem in Bresult.Results)
        {
            CloudBlockBlob inputBlob = listBlobItem as CloudBlockBlob;
            if ((inputBlob != null) && (inputBlob.Name.IndexOf("$$$.$$$") == -1))
            {
                string blobText = inputBlob.DownloadText(Encoding.ASCII, null, null, null);
                logger.Write("input blob text: {0}", blobText);
                string[] source = blobText.Split(new char[] { '.', '?', '!', ' ', ';', ':', ',' }, StringSplitOptions.RemoveEmptyEntries);
                var matchQuery = from word in source
                                 where word.ToLowerInvariant() == searchTerm.ToLowerInvariant()
                                 select word;
                int wordCount = matchQuery.Count();
                output += string.Format("{0} occurrences(s) of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
            }
        }
        return output;
    }
    ```

    Veri kümesini işaret klasöre yolunu GetFolderPath yöntemi döndürür ve GetFileName yöntemi blob/dataset işaret eden dosya adını döndürür. {Year} gibi değişkenler kullanarak folderPath varsa tanımlar {Month} olarak dizedir çalışma zamanı değerlerinizle değiştirerek olmadan {Day} vs. yöntemi döndürür. Bkz: [genişletilmiş özellikler erişim](#access-extended-properties) bölümüne erişme SliceStart, SliceEnd ilişkin ayrıntılar için.

    ```JSON
    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "adftutorial/inputfolder/",
    ```

    Calculate yöntemi anahtar sözcüğü Giriş dosyaları (klasördeki blobları) Microsoft örnek sayısını hesaplar. Arama terimi, kodda sabit kodlanmış ("Microsoft").
10. Projeyi derle. Tıklayın **derleme** tıklatın ve menüden **Çözümü Derle**.

    > [!IMPORTANT]
    > Projeniz için hedef çerçeveyi .NET Framework 4.5.2 kümesi sürümü: projeye sağ tıklayın ve **özellikleri** hedef Framework'ü ayarlamak için. Veri Fabrikası karşı .NET Framework sürüm 4.5.2 daha sonra derlenmiş olan özel etkinlikler desteklemez.

11. Başlatma **Windows Explorer**gidin **bin\debug** veya **bin\release** klasör yapı türüne bağlıdır.
12. ZIP dosyası oluşturma **MyDotNetActivity.zip** tüm ikili dosyaları içeren \<proje klasörü\>\bin\Debug klasörüne. Dahil **MyDotNetActivity.pdb** kaynak kodunda bir hata varsa, soruna neden satır numarası gibi ek ayrıntıları almak için dosya.

    > [!IMPORTANT]
    > Özel etkinliğin zip dosyasındaki tüm dosyalar alt klasör olmadan **en üst düzeyde** olmalıdır.

    ![İkili çıktı dosyaları](./media/data-factory-use-custom-activities/Binaries.png)
14. Adlı bir blob kapsayıcısı oluşturursunuz **customactivitycontainer** zaten yoksa.
15. Bir BLOB içinde customactivitycontainer olarak MyDotNetActivity.zip karşıya bir **genel amaçlı** AzureStorageLinkedService tarafından başvurulan Azure blob depolama (değil sık/seyrek erişimli Blob Depolama).

> [!IMPORTANT]
> Bu .NET etkinliği proje Visual Studio'da bir Data Factory projesi içeren bir çözüme ekleyin ve Data Factory uygulama projesinden bir .NET etkinliği projeye başvuru ekleyin, zip el ile oluşturma, son iki adımı gerçekleştirmeniz gerekmez Dosya ve genel amaçlı bir Azure blob depolama alanına yükleniyor. Visual Studio kullanarak Data Factory varlıkları yayımladığınızda, bu adımlar yayımlama işlemi tarafından otomatik olarak gerçekleştirilir. Daha fazla bilgi için [Visual Studio'da Data Factory projesine](#data-factory-project-in-visual-studio) bölümü.

## <a name="create-a-pipeline-with-custom-activity"></a>Özel Etkinlik ile işlem hattı oluşturma
Özel bir etkinlik oluşturulur ve ikili dosyaları zip dosyasıyla, bir blob kapsayıcısını karşıya bir **genel amaçlı** Azure depolama hesabı. Bu bölümde, özel etkinliği kullanan bir işlem hattı ile bir Azure veri fabrikası oluşturun.

Özel etkinliğin giriş veri kümesi customactivityinput klasöründe adftutorial blob depolama kapsayıcısında bloblar (dosyalar) temsil eder. Etkinliğin çıkış veri kümesi, çıktı bloblarını adftutorial blob depolama kapsayıcısında customactivityoutput klasöründe temsil eder.

Oluşturma **dosya.txt** dosya aşağıdaki içeriğe ve bunu **customactivityinput** klasörü **adftutorial** kapsayıcı. Zaten yoksa adftutorial kapsayıcısını oluşturun.

```
test custom activity Microsoft test custom activity Microsoft
```

İki veya daha fazla dosya klasörü sahip olsa bile giriş klasörü Azure Data factory'de bir dilim karşılık gelir. Özel Etkinlik, her işlem hattı tarafından işlendiğinde, dilim için giriş klasöründeki tüm blobları gezinir.

Bir veya daha fazla satır (girdi klasöründe bulunan BLOB sayısı ile aynı) dosyası ile adftutorial\customactivityoutput klasöründe bir çıkış görürsünüz:

```
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2016-11-16-00/file.txt.
```


Bu bölümde gerçekleştireceğiniz adımlar şunlardır:

1. Oluşturma bir **veri fabrikası**.
2. Oluşturma **bağlı hizmetler** Vm'lerinde Azure Batch havuzu için özel etkinliği çalışır ve giriş/çıkış BLOB'ları içeren Azure depolama.
3. Girdi ve çıktı oluşturma **veri kümeleri** temsil eden girdi ve çıktı özel etkinlik.
4. Oluşturma bir **işlem hattı** , özel etkinliği kullanır.

> [!NOTE]
> Oluşturma **dosya.txt** ve zaten yapmadıysanız blob kapsayıcısına yükleyin. Önceki bölümdeki yönergelere bakın.

### <a name="step-1-create-the-data-factory"></a>1. Adım: Veri Fabrikası oluşturma
1. Azure portalında oturum açtıktan sonra aşağıdaki adımları uygulayın:
   1. Tıklayın **kaynak Oluştur** sol menüsünde.
   2. Tıklayın **veri ve analiz** içinde **yeni** dikey penceresi.
   3. **Veri analizi** dikey penceresinde **Data Factory**’ye tıklayın.

      ![Yeni Azure Data Factory menüsü](media/data-factory-use-custom-activities/new-azure-data-factory-menu.png)
2. İçinde **yeni veri fabrikası** dikey penceresinde girin **CustomActivityFactory** adı. Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Hatayı alırsanız: **Veri Fabrikası adı "CustomActivityFactory" kullanılamıyor**, veri fabrikasının adını değiştirin (örneğin, **yournameCustomActivityFactory**) ve oluşturmayı yeniden deneyin.

    ![Yeni Azure Data Factory dikey penceresi](media/data-factory-use-custom-activities/new-azure-data-factory-blade.png)
3. Tıklayın **kaynak grubu adı**, mevcut bir kaynak grubunu seçin ve bir kaynak grubu oluşturun.
4. Doğru kullandığınızı doğrulayın **abonelik** ve **bölge** oluşturulacak veri fabrikasının istediğiniz.
5. **Yeni data factory** dikey penceresinde **Oluştur**’a tıklayın.
6. İçinde oluşturduğunuz veri fabrikasına gördüğünüz **Pano** Azure portal'ın.
7. Data factory başarıyla oluşturulduktan sonra data Factory içeriği gösterilir Data Factory dikey penceresine bakın.

    ![Data Factory dikey penceresi](media/data-factory-use-custom-activities/data-factory-blade.png)

### <a name="step-2-create-linked-services"></a>2. Adım: Bağlı hizmetler oluşturma
Bağlı hizmetler veri depolarını veya işlem hizmetlerini Azure data factory’ye bağlar. Bu adımda, Azure depolama hesabınız ve Azure Batch hesabı veri fabrikanıza bağlarsınız.

#### <a name="create-azure-storage-linked-service"></a>Azure Storage bağlı hizmeti oluşturma
1. Tıklayın **yazar ve dağıtma** kutucuğundan **DATA FACTORY** dikey **CustomActivityFactory**. Data Factory Düzenleyicisi’ni görürsünüz.
2. Tıklayın **yeni veri deposu** komut çubuğu ve seçin **Azure depolama**. Düzenleyicide Azure Storage bağlı hizmeti oluşturmak için JSON betiğini görmeniz gerekir.

    ![Yeni veri deposu - Azure depolama](media/data-factory-use-custom-activities/new-data-store-menu.png)
3. Değiştirin `<accountname>` Azure depolama hesabınızın adıyla ve `<accountkey>` Azure depolama hesabının erişim anahtarı ile. Depolama erişim anahtarınızı nasıl alacağınız hakkında bilgi için bkz. [Depolama erişim anahtarlarını görüntüleme, kopyalama ve yeniden oluşturma](../../storage/common/storage-account-manage.md#access-keys).

    ![Azure depolama hizmeti beğenmediğinizi](media/data-factory-use-custom-activities/azure-storage-linked-service.png)
4. Bağlı hizmeti dağıtmak için komut çubuğunda **Dağıt**’a tıklayın.

#### <a name="create-azure-batch-linked-service"></a>Azure Batch bağlı hizmeti oluşturma
1. Data Factory Düzenleyicisi'nde tıklayın **... Daha fazla** komut çubuğunda **yeni işlem**ve ardından **Azure Batch** menüsünde.

    ![Yeni işlem - Azure Batch](media/data-factory-use-custom-activities/new-azure-compute-batch.png)
2. JSON betiği aşağıdaki değişiklikleri yapın:

   1. Azure Batch hesap adı belirtin **accountName** özelliği. **URL** gelen **Azure Batch hesabı dikey penceresi** aşağıdaki biçimdedir: `http://accountname.region.batch.azure.com`. İçin **batchUri** JSON özelliğinde kaldırmak için ihtiyacınız `accountname.` kullanın ve URL `accountname` için `accountName` JSON özelliği.
   2. Azure Batch hesap anahtarı belirtin **accessKey** özelliği.
   3. İçin ön koşulların bir parçası olarak oluşturduğunuz havuzunun adını belirtin **poolName** özelliği. Havuz adı yerine havuzun kimliği belirtebilirsiniz.
   4. Azure Batch URI'si belirtin **batchUri** özelliği. Örnek: `https://westus.batch.azure.com`.
   5. Belirtin **AzureStorageLinkedService** için **linkedServiceName** özelliği.

        ```json
        {
          "name": "AzureBatchLinkedService",
          "properties": {
            "type": "AzureBatch",
            "typeProperties": {
              "accountName": "myazurebatchaccount",
              "batchUri": "https://westus.batch.azure.com",
              "accessKey": "<yourbatchaccountkey>",
              "poolName": "myazurebatchpool",
              "linkedServiceName": "AzureStorageLinkedService"
            }
          }
        }
        ```

       İçin **poolName** özelliği, havuzun havuz adı yerine Kimliğini belirtebilirsiniz.

### <a name="step-3-create-datasets"></a>3. Adım: Veri kümeleri oluşturma
Bu adımda, girdi ve çıktı verilerini temsil eden veri kümeleri oluşturun.

#### <a name="create-input-dataset"></a>Girdi veri kümesi oluşturma
1. Data Factory **Düzenleyici**’de açılır listeden **... Daha fazla** komut çubuğunda **yeni veri kümesi**ve ardından **Azure Blob Depolama** aşağı açılan menüden.
2. Sağ bölmede JSON'u aşağıdaki JSON parçacığıyla değiştirin:

    ```json
    {
        "name": "InputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "adftutorial/customactivityinput/",
                "format": {
                    "type": "TextFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }
    ```

   Başlangıç saatine sahip bu kılavuzda daha sonra bir işlem hattı oluşturun: 2016-11-16T00:00:00Z ve bitiş zamanı: 2016-11-16T05:00:00Z. Beş girdi/çıktı dilimleri olduklarından, verileri üretmek üzere planlanmıştır (arasında **00**: 00:00 -> **05**: 00:00).

   **Sıklığı** ve **aralığı** giriş veri kümesi ayarlanmıştır **saat** ve **1**, giriş dilimi saatlik kullanılabilir olduğunu anlamına gelir. Bu örnekte, buna intputfolder aynı dosyada (dosya.txt) var.

   Yukarıdaki JSON kod parçacığında SliceStart sistem değişkeni tarafından temsil edilen her dilim için başlangıç zamanlarını aşağıda verilmiştir.
3. Tıklayın **Dağıt** oluşturmak ve dağıtmak için araç çubuğunda **Inputdataset**. Düzenleyici’nin başlık çubuğunda **TABLO BAŞARIYLA OLUŞTURULDU** iletisini gördüğünüzü onaylayın.

#### <a name="create-an-output-dataset"></a>Çıktı veri kümesi oluşturma
1. İçinde **Data Factory Düzenleyicisi'ni**, tıklayın **... Daha fazla** komut çubuğunda **yeni veri kümesi**ve ardından **Azure Blob Depolama**.
2. Sağ bölmedeki JSON betiği aşağıdaki JSON kod ile değiştirin:

    ```JSON
    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "{slice}.txt",
                "folderPath": "adftutorial/customactivityoutput/",
                "partitionedBy": [
                    {
                        "name": "slice",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy-MM-dd-HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```

     Çıkış konumu **adftutorial/customactivityoutput/** ve yyyy-MM-dd-ss yıl, ay, tarih ve saat diliminin üretilmekte olduğu yyyy-aa-gg-HH.txt çıkış dosyası adı. Bkz: [Geliştirici Başvurusu] [ adf-developer-reference] Ayrıntılar için.

    Bir çıkış blob/dosyası, her giriş dilimi için oluşturulur. İşte bir çıktı dosyası için her bir dilimi nasıl adlandırılır. Bir klasörde oluşturulan tüm çıktı dosyaları: **adftutorial\customactivityoutput**.

   | Dilim | Başlangıç saati | Çıktı dosyası |
   |:--- |:--- |:--- |
   | 1 |2016-11-16T00:00:00 |2016-11-16-00.txt |
   | 2 |2016-11-16T01:00:00 |2016-11-16-01.txt |
   | 3 |2016-11-16T02:00:00 |2016-11-16-02.txt |
   | 4 |2016-11-16T03:00:00 |2016-11-16-03.txt |
   | 5 |2016-11-16T04:00:00 |2016-11-16-04.txt |

    Giriş bir klasördeki tüm dosyaları bir dilim başlangıç süresi, yukarıda belirtilen bir parçası olduğunu unutmayın. Bu dilim işlenirken özel etkinlik her dosyanın tarar ve çıkış dosyasına arama terimi ("Microsoft") oluşum sayısını içeren bir satır üretir. İçinde inputfolder üç dosya varsa, üç satırı yok her saatlik dilim için çıkış dosyasına: 2016-11-16-00.txt, 2016-11-16:01:00:00.txt, vs.
3. Dağıtılacak **OutputDataset**, tıklayın **Dağıt** komut çubuğunda.

### <a name="create-and-run-a-pipeline-that-uses-the-custom-activity"></a>Oluşturma ve özel etkinliği kullanan bir işlem hattı çalıştırma
1. Data Factory Düzenleyicisi'nde tıklayın **... Daha fazla**ve ardından **yeni işlem hattı** komut çubuğunda.
2. Sağ bölmede JSON'u aşağıdaki JSON betiği ile değiştirin:

    ```JSON
    {
      "name": "ADFTutorialPipelineCustom",
      "properties": {
        "description": "Use custom activity",
        "activities": [
          {
            "Name": "MyDotNetActivity",
            "Type": "DotNetActivity",
            "Inputs": [
              {
                "Name": "InputDataset"
              }
            ],
            "Outputs": [
              {
                "Name": "OutputDataset"
              }
            ],
            "LinkedServiceName": "AzureBatchLinkedService",
            "typeProperties": {
              "AssemblyName": "MyDotNetActivity.dll",
              "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
              "PackageLinkedService": "AzureStorageLinkedService",
              "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
              "extendedProperties": {
                "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
              }
            },
            "Policy": {
              "Concurrency": 2,
              "ExecutionPriorityOrder": "OldestFirst",
              "Retry": 3,
              "Timeout": "00:30:00",
              "Delay": "00:00:00"
            }
          }
        ],
        "start": "2016-11-16T00:00:00Z",
        "end": "2016-11-16T05:00:00Z",
        "isPaused": false
      }
    }
    ```

    Aşağıdaki noktalara dikkat edin:

   * **Eşzamanlılık** ayarlanır **2** böylece iki dilimlerini Azure Batch havuzu içinde 2 VM tarafından paralel olarak işlenir.
   * Etkinlikler bölümünde bir etkinlik olduğundan ve türü: **DotNetActivity**.
   * **AssemblyName** DLL adına ayarlanır: **MyDotnetActivity.dll**.
   * **EntryPoint** ayarlanır **MyDotNetActivityNS.MyDotNetActivity**.
   * **PackageLinkedService** ayarlanır **AzureStorageLinkedService** özel etkinliğin zip dosyasını içeren blob depolama alanına işaret eder. Girdi/çıktı dosyalarını ve özel etkinliğin zip dosyası için farklı bir Azure depolama hesapları kullanıyorsanız, başka bir Azure depolama bağlı hizmeti oluşturun. Bu makalede, aynı Azure depolama hesabını kullandığınızı varsayar.
   * **PackageFile** ayarlanır **customactivitycontainer/MyDotNetActivity.zip**. Şu biçimdedir: containerforthezip/nameofthezip.zip.
   * Özel Etkinlik alır **Inputdataset** giriş olarak ve **OutputDataset** çıktı olarak.
   * Özel Etkinlik linkedServiceName özelliğini işaret **AzureBatchLinkedService**, Azure Data Factory, Azure Batch Vm'leri üzerinde çalışacak şekilde özel etkinlik gereken söyler.
   * **isPaused** özelliği **false** varsayılan olarak. Geçmiş dilimler başlatmak için işlem hattı hemen bu örnekte çalışır. Bu özellik, işlem hattı duraklatma ve yeniden geri false olarak ayarlamak için true olarak ayarlayabilirsiniz.
   * **Başlat** zaman ve **son** sürelerinin **beş** uzaklıkta saat ve dilim oluşturulur, işlem hattı tarafından beş dilimlerinin şekilde.
3. İşlem hattını dağıtmak için **Dağıt** komut çubuğunda.

### <a name="monitor-the-pipeline"></a>İşlem hattını izleme
1. Azure Portal'da Data Factory dikey penceresinde **diyagram**.

    ![Diyagram kutucuğu](./media/data-factory-use-custom-activities/DataFactoryBlade.png)
2. Diyagram Görünümü'nde OutputDataset artık'a tıklayın.

    ![Diyagram görünümü](./media/data-factory-use-custom-activities/diagram.png)
3. Beş çıktı dilimi hazır durumda olduğunu görmeniz gerekir. Bunlar, hazır durumda değilse, bunlar henüz üretilmiş henüz.

   ![Çıktı dilimleri](./media/data-factory-use-custom-activities/OutputSlices.png)
4. Çıktı dosyaları blob depolama içinde oluşturulan doğrulayın **adftutorial** kapsayıcı.

   ![Özel Etkinlik çıkışı][image-data-factory-output-from-custom-activity]
5. Çıktı dosyasını açın, aşağıdaki çıktıya benzer bir çıktı görmeniz gerekir:

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2016-11-16-00/file.txt.
    ```
6. Kullanım [Azure portalında] [ azure-preview-portal] ya da, data factory, işlem hatlarını ve veri kümelerini izlemek için Azure PowerShell cmdlet'leri. Gelen iletileri görebilirsiniz **ActivityLogger** kodunda özel etkinlik günlüklerindeki portalı veya cmdlet'lerini kullanarak yükleyebileceğiniz (özellikle kullanıcı-0.log).

   ![indirme günlükleri özel etkinliği][image-data-factory-download-logs-from-custom-activity]

Bkz: [yönetme işlem hatlarını izleme ve](data-factory-monitor-manage-pipelines.md) veri kümeleri ve işlem hatlarını izleme ile ilgili ayrıntılı adımlar.

## <a name="data-factory-project-in-visual-studio"></a>Visual Studio'da Data Factory projesi
Oluşturun ve Azure portalını kullanarak yerine Visual Studio kullanarak Data Factory varlıklarını yayımlama. Ayrıntılı oluşturma hakkında bilgi ve Visual Studio kullanarak Data Factory varlıklarını yayımlama, görmek için [Visual Studio kullanarak ilk işlem hattınızı](data-factory-build-your-first-pipeline-using-vs.md) ve [kopyalayın verileri Azure Blob'tan Azure SQL'e](data-factory-copy-activity-tutorial-using-visual-studio.md) makaleler.

Visual Studio'da Data Factory proje oluşturuyorsanız, aşağıdaki ek adımları uygulayın:

1. Data Factory projenize özel etkinlik projesini içeren Visual Studio çözümünü ekleyin.
2. Data Factory proje .NET etkinliği projeye bir başvuru ekleyin. Data Factory projesine sağ tıklayın, fareyle **Ekle**ve ardından **başvuru**.
3. İçinde **Başvuru Ekle** iletişim kutusunda **MyDotNetActivity** proje öğesini tıklatıp **Tamam**.
4. Derleme ve çözüm yayımlayın.

    > [!IMPORTANT]
    > Data Factory varlıkları yayımladığınızda bir zip dosyası sizin için otomatik olarak oluşturulur ve blob kapsayıcısına yüklenir: customactivitycontainer. Blob kapsayıcısında mevcut değilse otomatik olarak çok oluşturulur.

## <a name="data-factory-and-batch-integration"></a>Data Factory ve Batch tümleştirme
Data Factory hizmetinin bir iş adı ile Azure Batch hizmetinde oluşturur: **adf poolname: iş-xxx**. Tıklayın **işleri** sol menüden.

![Azure Data Factory - Batch işleri](media/data-factory-use-custom-activities/data-factory-batch-jobs.png)

Bir dilim her etkinlik çalışması için bir görev oluşturulur. Beş dilimleri işlenmeye hazır varsa, beş görev alanında bu işin oluşturulur. Batch havuzu birden çok işlem düğümleri varsa, iki veya daha fazla dilim paralel olarak çalıştırılabilir. İşlem düğümü başına en fazla görev > 1 olarak ayarlanırsa, aynı işlem üzerinde çalışan birden fazla dilim da olabilir.

![Azure Data Factory - Batch iş görevleri](media/data-factory-use-custom-activities/data-factory-batch-job-tasks.png)

Aşağıdaki diyagram, Azure Data Factory ve Batch görevleri arasındaki ilişkiyi gösterir.

![Veri Fabrikası ve toplu işlem](./media/data-factory-use-custom-activities/DataFactoryAndBatch.png)

## <a name="troubleshoot-failures"></a>Hatalarını giderme
Sorun giderme birkaç temel teknikten oluşur:

1. Aşağıdaki hatayı görürseniz, sık erişimli/seyrek erişimli blob depolama genel amaçlı bir Azure blob depolama alanı kullanmak yerine kullanıyor olabilir. Zip dosyasını karşıya bir **genel amaçlı Azure depolama hesabı**.

    ```
    Error in Activity: Job encountered scheduling error. Code: BlobDownloadMiscError Category: ServerError Message: Miscellaneous error encountered while downloading one of the specified Azure Blob(s).
    ```
2. Aşağıdaki hatayı görürseniz, CS dosyasında sınıfın adı için belirttiğiniz ad eşleştiğini doğrulamak **EntryPoint** JSON işlem hattının özelliği. Bu izlenecek yolda, sınıfın adıdır: MyDotNetActivity ve JSON EntryPoint şöyledir: MyDotNetActivityNS. **MyDotNetActivity**.

    ```
    MyDotNetActivity assembly does not exist or doesn't implement the type Microsoft.DataFactories.Runtime.IDotNetActivity properly
    ```

   Adları aynı ise, tüm ikili dosyaların olduğunu onaylamak **kök klasörü** ZIP dosyasının. Zip dosyasını açtığınızda, diğer bir deyişle, tüm dosyaları kök klasöründe bulunan, tüm alt klasörlerdeki görmeniz gerekir.
3. Giriş dilimi olarak ayarlanmazsa, **hazır**, giriş klasörü yapısının doğru olduğundan emin olun ve **dosya.txt** giriş klasörlerinde bulunmaktadır.
3. İçinde **yürütme** yöntemi özel etkinliği, kullanım **Iactivitylogger** sorunlarını gidermenize yardımcı olacak bilgileri günlüğe kaydetmek için nesne. Günlüğe kaydedilen iletiler kullanıcı günlük dosyalarında gösterilir (adlı bir veya daha fazla dosyaları: user-0.log, user-1.log, user-2.log vb..).

   İçinde **OutputDataset** dikey penceresinde görmek için dilime tıklayın **veri DİLİMİ** dikey penceresinde, dilim için. Gördüğünüz **etkinlik çalıştırmalarını** , dilim için. Bir etkinlik dilimi için çalıştırılan görmeniz gerekir. Komut çubuğunda Çalıştır'a tıklayın, aynı dilimi için çalıştırılan başka bir etkinlik başlayabilirsiniz.

   Etkinlik çalıştırma tıkladığınızda, gördüğünüz **etkinlik çalışma ayrıntıları** günlük dosyalarının listesini içeren dikey pencere. User_0.log dosyasında günlüğe kaydedilen iletilere bakın. Bir hata oluştuğunda, yeniden deneme sayısı 3 işlem hattı/etkinlik JSON olarak ayarlandığından üç Etkinlik çalıştırmalarını görürsünüz. Etkinlik Çalıştır'a tıklayın, hatayı gidermek için gözden geçirebileceğiniz günlük dosyalarına bakın.

   Günlük dosyalarının listesinde tıklayın **kullanıcı-0.log**. Sağ bölmede kullanarak sonucu olan **IActivityLogger.Write** yöntemi. Tüm iletileri görmüyorsanız, daha fazla günlük dosyası adında olup olmadığını denetleyin: user_1.log, user_2.log vs. Son ileti günlüğe sonra Aksi takdirde, kod başarısız olmuş olabilir.

   Ayrıca, kontrol **system-0.log** herhangi bir sistem hata iletileri ve özel durumlar için.
4. Dahil **PDB** zip dosyasında hata ayrıntılarını aşağıdaki gibi bilgileri sahip dosya **çağrı yığını** bir hata oluştuğunda.
5. Özel etkinliğin zip dosyasındaki tüm dosyalar alt klasör olmadan **en üst düzeyde** olmalıdır.
6. Emin **assemblyName** (MyDotNetActivity.dll), **entryPoint**(MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer / MyDotNetActivity.zip) ve **packageLinkedService** (ulaştıracağı **genel amaçlı**zip dosyasını içeren Azure blob depolama) doğru değerlere ayarlanır.
7. Bir hatayı düzelttiyseniz ve dilimi yeniden işlemek istiyorsanız **OutputDataset** dikey penceresindeki dilime sağ tıklayın ve **Çalıştır**’a tıklayın.
8. Aşağıdaki hatayı görürseniz, Azure depolama paketi sürümü > 4.3.0 kullanıyor. Data Factory hizmeti Başlatıcısı WindowsAzure.Storage 4.3 sürümünü gerektirir. Bkz: [Appdomain yalıtım](#appdomain-isolation) bölümünde geçici için Azure depolama derlemenin daha yeni sürümünü kullanmanız gerekiyorsa.

    ```
    Error in Activity: Unknown error in module: System.Reflection.TargetInvocationException: Exception has been thrown by the target of an invocation. ---> System.TypeLoadException: Could not load type 'Microsoft.WindowsAzure.Storage.Blob.CloudBlob' from assembly 'Microsoft.WindowsAzure.Storage, Version=4.3.0.0, Culture=neutral,
    ```

    4.3.0 kullandığınız Azure depolama paketin sürümü Azure depolama paketi sürümü > 4.3.0 mevcut başvurusunu kaldırın. Ardından, NuGet Paket Yöneticisi konsolundan aşağıdaki komutu çalıştırın.

    ```powershell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    Projeyi derleyin. Sürüm > 4.3.0 Azure.Storage derlemenin bin\Debug klasöründen silin. İkili dosyalar ve PDB dosyası bir zip dosyası oluşturun. Bu bir blob kapsayıcısında (customactivitycontainer) eski zip dosyasını değiştirin. Yeniden çalıştırma başarısız olan dilimler (dilime sağ tıklayın ve Çalıştır'a tıklayın).
8. Özel Etkinlik kullanmaz **app.config** dosyasını. Bu nedenle, kodunuz yapılandırma dosyasından herhangi bir bağlantı dizesini okuyorsa, çalışma zamanında çalışmıyor. Azure Batch tüm gizli tutmak için kullanırken en iyi bir **Azure KeyVault**, korumak için sertifika tabanlı hizmet sorumlusu kullanmak **keyvault**ve Azure Batch için sertifika dağıtma havuzu. Böylece .NET özel etkinliği çalıştırma sırasında KeyVault’tan parolalara erişebilir. Bu çözüm, genel bir çözümdür ve gizli dizi, yalnızca bağlantı dizesi türüne ölçeklendirebilirsiniz.

   Daha kolay bir geçici çözüm (ancak değil en iyi uygulama): oluşturabileceğiniz bir **Azure SQL bağlı hizmeti** bağlantı dizesi ayarlarını bağlı hizmetini kullanan bir veri kümesi oluşturma ve veri kümesi olarak bir işlevsiz bir giriş veri kümesi için zincir oluşturma özel bir .NET etkinliği. Özel Etkinlik kod bağlantılı hizmetin bağlantı dizesinde erişebilirsiniz.

## <a name="update-custom-activity"></a>Özel Etkinlik güncelleştirme
Özel Etkinlik için kodu güncelleştirmeniz gerekirse oluşturun ve blob depolama alanına yeni ikili dosyaları içeren bir ZIP dosyasını karşıya yükleme.

## <a name="appdomain-isolation"></a>Uygulama etki alanı yalıtımı
Bkz: [çapraz AppDomain örnek](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) , Data Factory başlatıcısı tarafından kullanılan derleme sürümlerini kısıtlı değil özel bir etkinlik oluşturma işlemini gösterir (örnek: WindowsAzure.Storage verze 4.3.0, aynı zamanda Newtonsoft.Json v6.0.x, vb.).

## <a name="access-extended-properties"></a>Genişletilmiş özellikler erişim
Genişletilmiş özellikler, aşağıdaki örnekte gösterildiği gibi JSON etkinliğinde bildirebilirsiniz:

```JSON
"typeProperties": {
  "AssemblyName": "MyDotNetActivity.dll",
  "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
  "PackageLinkedService": "AzureStorageLinkedService",
  "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
  "extendedProperties": {
    "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))",
    "DataFactoryName": "CustomActivityFactory"
  }
},
```

Örnekte, iki genişletilmiş özellikler vardır: **SliceStart** ve **DataFactoryName**. SliceStart değeri SliceStart sistem değişkeninde temel alır. Bkz: [sistem değişkenlerini](data-factory-functions-variables.md) desteklenen sistem değişkenleri listesi. DataFactoryName CustomActivityFactory için sabit kodlanmış değerdir.

Bu özellikler genişletilmiş erişmeye **yürütme** yöntemi, aşağıdaki koda benzer kod kullanın:

```csharp
// to get extended properties (for example: SliceStart)
DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];

// to log all extended properties
IDictionary<string, string> extendedProperties = dotNetActivity.ExtendedProperties;
logger.Write("Logging extended properties if any...");
foreach (KeyValuePair<string, string> entry in extendedProperties)
{
    logger.Write("<key:{0}> <value:{1}>", entry.Key, entry.Value);
}
```

## <a name="auto-scaling-of-azure-batch"></a>Azure batch otomatik olarak ölçeklendirme
Bir Azure Batch havuzu de oluşturabilirsiniz **otomatik ölçeklendirme** özelliği. Örneğin, 0 adanmış VM'ler ve Bekleyen Görevler sayısına bağlı olarak bir otomatik ölçeklendirme formülü ile bir azure batch havuzu oluşturabilirsiniz.

Burada örnek formülü aşağıdaki davranışı elde eder: Havuz başlangıçta oluşturulduğunda, 1 sanal makine ile başlar. $PendingTasks ölçüm çalışan + (kuyruğa alınmış) etkin içindeki görevlerin sayısını tanımlar durumu.  Formül, Son 180 saniye cinsinden ortalama sayısı Bekleyen Görevler bulur ve TargetDedicated uygun şekilde ayarlar. TargetDedicated hiçbir zaman 25 VM'lerin ötesine geçen gider sağlar. Bu nedenle, yeni görevler gönderilen, havuzu otomatik olarak büyür ve görevler tamamlanınca ücretsiz tek tek sanal makineleri olur ve bu sanal makineler için otomatik ölçeklendirme küçültür. startingNumberOfVMs ve maxNumberofVMs ihtiyaçlarınıza göre ayarlanabilir.

Otomatik ölçeklendirme formülü:

```
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
```

Bkz: [işlem düğümleri Azure Batch havuzunda otomatik olarak](../../batch/batch-automatic-scaling.md) Ayrıntılar için.

Varsayılan havuz kullanıyorsa [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), Batch hizmeti, sanal Makinenin özel etkinlik çalıştırmadan önce hazırlamak için 15-30 dakika sürebilir.  Havuz farklı autoScaleEvaluationInterval kullanıyorsanız, Batch hizmeti autoScaleEvaluationInterval + 10 dakika sürebilir.


## <a name="create-a-custom-activity-by-using-net-sdk"></a>.NET SDK kullanarak özel bir etkinlik oluşturma
Bu makaledeki kılavuzda, Azure portalını kullanarak özel etkinliği kullanan bir işlem hattıyla veri fabrikası oluşturun. Aşağıdaki kodu yerine .NET SDK kullanarak data factory oluşturulacağını gösterir. Program aracılığıyla işlem hatlarında oluşturmak için SDK'sı kullanma hakkında daha fazla ayrıntı bulabilirsiniz [.NET API kullanarak kopyalama etkinlikli bir işlem hattı oluşturma](data-factory-copy-activity-tutorial-using-dotnet-api.md) makalesi.

```csharp
using System;
using System.Configuration;
using System.Collections.ObjectModel;
using System.Threading;
using System.Threading.Tasks;

using Microsoft.Azure;
using Microsoft.Azure.Management.DataFactories;
using Microsoft.Azure.Management.DataFactories.Models;
using Microsoft.Azure.Management.DataFactories.Common.Models;

using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Collections.Generic;

namespace DataFactoryAPITestApp
{
    class Program
    {
        static void Main(string[] args)
        {
            // create data factory management client

            // TODO: replace ADFTutorialResourceGroup with the name of your resource group.
            string resourceGroupName = "ADFTutorialResourceGroup";

            // TODO: replace APITutorialFactory with a name that is globally unique. For example: APITutorialFactory04212017
            string dataFactoryName = "APITutorialFactory";

            TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
                ConfigurationManager.AppSettings["SubscriptionId"],
                GetAuthorizationHeader().Result);

            Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

            DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

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
                            // TODO: Replace <accountname> and <accountkey> with name and key of your Azure Storage account.
                            new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>")
                        )
                    }
                }
            );

            // create a linked service for output data store: Azure SQL Database
            Console.WriteLine("Creating Azure Batch linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureBatchLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            // TODO: replace <batchaccountname> and <yourbatchaccountkey> with name and key of your Azure Batch account
                            new AzureBatchLinkedService("<batchaccountname>", "https://westus.batch.azure.com", "<yourbatchaccountkey>", "myazurebatchpool", "AzureStorageLinkedService")
                        )
                    }
                }
            );

            // create input and output datasets
            Console.WriteLine("Creating input and output datasets");
            string Dataset_Source = "InputDataset";
            string Dataset_Destination = "OutputDataset";

            Console.WriteLine("Creating input dataset of type: Azure Blob");
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
                                FolderPath = "adftutorial/customactivityinput/",
                                Format = new TextFormat()
                            },
                            External = true,
                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },

                            Policy = new Policy() { }
                        }
                    }
                });

            Console.WriteLine("Creating output dataset of type: Azure Blob");
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
                                FileName = "{slice}.txt",
                                FolderPath = "adftutorial/customactivityoutput/",
                                PartitionedBy = new List<Partition>()
                                {
                                    new Partition()
                                    {
                                        Name = "slice",
                                        Value = new DateTimePartitionValue()
                                        {
                                            Date = "SliceStart",
                                            Format = "yyyy-MM-dd-HH"
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

            Console.WriteLine("Creating a custom activity pipeline");
            DateTime PipelineActivePeriodStartTime = new DateTime(2017, 3, 9, 0, 0, 0, 0, DateTimeKind.Utc);
            DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
            string PipelineName = "ADFTutorialPipelineCustom";

            client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new PipelineCreateOrUpdateParameters()
                {
                    Pipeline = new Pipeline()
                    {
                        Name = PipelineName,
                        Properties = new PipelineProperties()
                        {
                            Description = "Use custom activity",

                            // Initial value for pipeline's active period. With this, you won't need to set slice status
                            Start = PipelineActivePeriodStartTime,
                            End = PipelineActivePeriodEndTime,
                            IsPaused = false,

                            Activities = new List<Activity>()
                            {
                                new Activity()
                                {
                                    Name = "MyDotNetActivity",
                                    Inputs = new List<ActivityInput>()
                                    {
                                        new ActivityInput() {
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
                                    LinkedServiceName = "AzureBatchLinkedService",
                                    TypeProperties = new DotNetActivity()
                                    {
                                        AssemblyName = "MyDotNetActivity.dll",
                                        EntryPoint = "MyDotNetActivityNS.MyDotNetActivity",
                                        PackageLinkedService = "AzureStorageLinkedService",
                                        PackageFile = "customactivitycontainer/MyDotNetActivity.zip",
                                        ExtendedProperties = new Dictionary<string, string>()
                                        {
                                            { "SliceStart", "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"}
                                        }
                                    },
                                    Policy = new ActivityPolicy()
                                    {
                                        Concurrency = 2,
                                        ExecutionPriorityOrder = "OldestFirst",
                                        Retry = 3,
                                        Timeout = new TimeSpan(0,0,30,0),
                                        Delay = new TimeSpan()
                                    }
                                }
                            }
                        }
                    }
                });
        }

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
    }
}
```

## <a name="debug-custom-activity-in-visual-studio"></a>Özel Etkinlik Visual Studio'da hata ayıklama
[Azure Data Factory - yerel ortam](https://github.com/gbrueckl/Azure.DataFactory.LocalEnvironment) github'daki örnek özel .NET etkinlikleri Visual Studio'dan hata ayıklama olanak tanıyan bir araç içerir.

## <a name="sample-custom-activities-on-github"></a>GitHub üzerinde örnek özel etkinlikler
| Örnek | Hangi özel etkinlik yok |
| --- | --- |
| [HTTP veri yükleyici](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample). |Verileri, özel C# etkinlik Data Factory kullanarak Azure Blob Depolama için bir HTTP uç noktasından indirir. |
| [Twitter yaklaşım analizi örneği](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |Bir Azure Machine Learning studio model ve yaklaşım analizi yapın, Puanlama, tahmin vb. çağırır. |
| [R betiğini Çalıştır](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). |R betiği HDInsight kümenizde R yüklü üzerinde zaten RScript.exe çalıştırarak çağırır. |
| [Çapraz AppDomain .NET etkinliği](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) |Data Factory başlatıcısı tarafından kullanılan olanları farklı derleme sürümlerini kullanır |
| [Azure Analysis Services modelinde yeniden işleme](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/AzureAnalysisServicesProcessSample) |  Azure Analysis Services modelinde yeniden işler. |

[batch-net-library]: ../../batch/batch-dotnet-get-started.md
[batch-create-account]: ../../batch/batch-account-create-portal.md
[batch-technical-overview]: ../../batch/batch-technical-overview.md
[batch-get-started]: ../../batch/batch-dotnet-get-started.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[data-factory-introduction]: data-factory-introduction.md
[azure-powershell-install]: https://github.com/Azure/azure-sdk-tools/releases


[developer-reference]: https://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: https://go.microsoft.com/fwlink/?LinkId=517456

[new-azure-batch-account]: https://msdn.microsoft.com/library/mt125880.aspx
[new-azure-batch-pool]: https://msdn.microsoft.com/library/mt125936.aspx
[azure-batch-blog]: https://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx

[nuget-package]: https://go.microsoft.com/fwlink/?LinkId=517478
[adf-developer-reference]: https://go.microsoft.com/fwlink/?LinkId=516908
[azure-preview-portal]: https://portal.azure.com/

[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[hivewalkthrough]: data-factory-data-transformation-activities.md

[image-data-factory-output-from-custom-activity]: ./media/data-factory-use-custom-activities/OutputFilesFromCustomActivity.png

[image-data-factory-download-logs-from-custom-activity]: ./media/data-factory-use-custom-activities/DownloadLogsFromCustomActivity.png
