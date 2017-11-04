---
title: "Bir Azure Data Factory işlem hattında özel etkinlikler kullanma"
description: "Özel etkinlikler oluşturmak ve bunları bir Azure Data Factory ardışık düzeninde öğrenin."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 8dd7ba14-15d2-4fd9-9ada-0b2c684327e9
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/01/2017
ms.author: spelluru
robots: noindex
ms.openlocfilehash: 0794952fdfbcc49cc66273be2d46484014ae1677
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="use-custom-activities-in-an-azure-data-factory-pipeline"></a>Bir Azure Data Factory işlem hattında özel etkinlikler kullanma
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](data-factory-use-custom-activities.md)
> * [Sürüm 2 - Önizleme](../transform-data-using-dotnet-custom-activity.md)

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 özel etkinlikler](../transform-data-using-dotnet-custom-activity.md).

İki tür kullanabileceğiniz bir Azure Data Factory ardışık düzeninde etkinlik yok.

- [Veri taşıma etkinlikleri](data-factory-data-movement-activities.md) arasında veri taşımak için [desteklenen kaynak ve havuz veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats).
- [Veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md) verileri dönüştürmek için kullanarak işlem hizmetlerini Azure Hdınsight, Azure Batch ve Azure Machine Learning gibi. 

Oluştur/Veri Fabrikası desteği olmayan bir veri deposundan verileri taşımak için bir **özel etkinlik** kendi veri taşıma mantığı ve ardışık düzeninde etkinlik kullanın. Benzer şekilde, veri fabrikası tarafından desteklenmeyen bir şekilde veri dönüştürme/işlemi için kendi veri dönüştürme mantığı ile özel bir etkinlik oluşturmak ve bir ardışık düzende etkinlik kullanın. 

Çalıştırmak için özel bir etkinliği yapılandırabilirsiniz bir **Azure Batch** sanal makine ya da Windows tabanlı bir havuzu **Azure Hdınsight** küme. Azure Batch kullanırken, mevcut bir Azure Batch havuzu kullanabilirsiniz. Oysa Hdınsight kullanırken, mevcut bir Hdınsight kümesine ya da otomatik olarak oluşturulan bir küme, isteğe bağlı çalışma zamanında için kullanabilirsiniz.  

Aşağıdaki örneklerde, özel bir .NET etkinlik oluşturmak ve bir ardışık düzende özel etkinlik kullanmak için adım adım yönergeler sağlar. İzlenecek yol kullanan bir **Azure Batch** bağlı hizmeti. Bunun yerine hizmeti bağlı Azure Hdınsight kullanma, bağlı hizmet türü oluşturma **Hdınsight** (kendi Hdınsight kümenizi) veya **HDInsightOnDemand** (Data Factory oluşturduğu bir Hdınsight kümesi isteğe bağlı). Ardından, Hdınsight bağlı hizmeti kullanmak için özel etkinlik yapılandırın. Bkz: [kullanım Azure Hdınsight bağlantılı Hizmetleri](#use-hdinsight-compute-service) özel etkinliği çalıştırmak için Azure Hdınsight kullanımıyla ilgili ayrıntılar için bölüm.

> [!IMPORTANT]
> - Özel .NET etkinlikler yalnızca Windows tabanlı Hdınsight kümelerinde çalıştırın. Geçici bir çözüm için bu sınırlama, Linux tabanlı Hdınsight kümesinde özel Java kodu çalıştırmak için azaltmak Haritası aktivitesi kullanmaktır. Bir Azure Batch havuzu VM'lerin bir Hdınsight kümesi yerine özel etkinlikleri çalıştırmak için kullandığınız başka bir seçenektir.
> - Şirket içi veri kaynaklarına erişmek için veri yönetimi ağ geçidi özel etkinliğinden kullanmak mümkün değil. Şu anda [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) yalnızca kopyalama etkinliği ve saklı yordam etkinliği veri fabrikasında destekler.   

## <a name="walkthrough-create-a-custom-activity"></a>İzlenecek yol: özel etkinlik oluşturma
### <a name="prerequisites"></a>Ön koşullar
* Visual Studio 2012/2013/2015
* [Azure .NET SDK](https://azure.microsoft.com/downloads/)’yı indirip yükleyin

### <a name="azure-batch-prerequisites"></a>Azure Batch önkoşulları
İzlenecek özel .NET etkinliklerinizi Azure toplu işlem kaynak olarak kullanarak çalıştırın. **Azure Batch** (HPC) uygulamalarını bulutta verimli bir şekilde bir platform için büyük ölçekli paralel çalıştırmak için hizmet ve yüksek performanslı bilgi işlem. Azure toplu işlem yönetilen üzerinde çalıştırılacak işlem yoğunluklu işi zamanlayan **sanal makineler koleksiyonunda**, ve ölçek işleriniz ihtiyaçlarını karşılamak için kaynakları otomatik olarak hesaplayabilirsiniz. Bkz: [Azure Batch temel bilgileri] [ batch-technical-overview] ayrıntılı bir genel bakış için makale Azure Batch hizmetinin.

Öğretici için VM'ler havuzuyla Azure Batch hesabı oluşturun. Adımlar aşağıdaki gibidir:

1. Oluşturma bir **Azure Batch hesabı** kullanarak [Azure portal](http://portal.azure.com). Bkz: [oluşturma ve bir Azure Batch hesabını yönetmek] [ batch-create-account] makale yönergeler için.
2. Azure toplu işlem hesabı adı, hesap anahtarı, URI ve havuz adı unutmayın. Bir Azure Batch bağlı hizmet oluşturmak için gereksinim duyarsınız.
    1. Azure toplu işlem hesabı için giriş sayfasında, gördüğünüz bir **URL** şu biçimde: `https://myaccount.westus.batch.azure.com`. Bu örnekte, **myaccount** Azure Batch hesabının adıdır. URI bağlantılı hizmet tanımında kullandığınız hesabın adını olmadan URL'dir. Örneğin: `https://<region>.batch.azure.com`.
    2. Tıklatın **anahtarları** sol menü ve kopyalama **birincil erişim ANAHTARINI**.
    3. Var olan bir havuzu kullanmak için tıklatın **havuzları** menü ve Not **kimliği** havuzu. Var olan bir havuzu sahip değilseniz, sonraki adıma taşıyın.     
2. Oluşturma bir **Azure Batch havuzu**.

   1. İçinde [Azure portal](https://portal.azure.com), tıklatın **Gözat** sol menüsüne ve ardından içinde **toplu işlem hesaplarını**.
   2. Açmak için Azure Batch hesabınızı seçin **toplu işlem hesabı** dikey.
   3. Tıklatın **havuzları** döşeme.
   4. İçinde **havuzları** dikey penceresinde, araç çubuğunda bir havuzu eklemek için Ekle düğmesini tıklatın.
      1. Bir kimlik havuzu (havuzu kimliği) girin. Not **havuzun kimliği**; Data Factory çözüm oluşturulurken gerekir.
      2. Belirtin **Windows Server 2012 R2** işletim sistemi ailesi ayarı için.
      3. Seçin bir **düğüm fiyatlandırma katmanı**.
      4. Girin **2** olarak değer **hedef ayrılmış** ayarı.
      5. Girin **2** olarak değer **en fazla düğüm başına görevleri** ayarı.
   5. Havuzu oluşturmak için **Tamam**'a tıklayın.
   6. Aşağı Not **kimliği** havuzu. 



### <a name="high-level-steps"></a>Üst düzey adımlar
Bu kılavuz kapsamında gerçekleştirme iki üst düzey adımlar şunlardır: 

1. Basit veri dönüştürme/işleme mantığı içeren özel bir aktivite oluşturun.
2. Bir Azure data factory özel etkinlik kullanan bir sahip işlem hattı oluşturacaksınız.

### <a name="create-a-custom-activity"></a>Özel etkinlik oluşturma
.NET özel etkinlik oluşturmak için Oluştur bir **.NET sınıf kitaplığı** uygulayan bir sınıf projeyle **IDotNetActivity** arabirimi. Bu arabirim yalnızca bir yöntemi vardır: [yürütme](https://msdn.microsoft.com/library/azure/mt603945.aspx) ve imzası:

```csharp
public IDictionary<string, string> Execute(
        IEnumerable<LinkedService> linkedServices,
        IEnumerable<Dataset> datasets,
        Activity activity,
        IActivityLogger logger)
```


Yöntemi, dört parametreleri alır:

- **linkedServices**. Bu özellik etkinlik için giriş/çıkış veri kümesi tarafından başvurulan veri deposu bağlı Hizmetleri numaralandırılabilir bir listesi verilmiştir.   
- **veri kümeleri**. Bu özellik etkinlik için giriş/çıkış veri kümesi numaralandırılabilir bir listesi verilmiştir. Giriş ve çıkış veri kümeleri tarafından tanımlanan şemaları ve konumlarını almak için bu parametreyi kullanın.
- **Etkinlik**. Bu özellik geçerli etkinliği temsil eder. Özel Etkinlik ile ilişkili genişletilmiş özelliklere erişmek için kullanılabilir. Bkz: [genişletilmiş özellikler erişim](#access-extended-properties) Ayrıntılar için.
- **Günlükçü**. Bu nesne ardışık düzeni için kullanıcı günlüğünde bu yüzeyini hata ayıklama açıklamaları yazmanızı sağlar.

Bu yöntem, özel etkinlikler gelecekte zincir için kullanılan bir sözlüğü döndürür. Bu özellik henüz uygulanmadı, bu nedenle boş bir sözlük döndürme.  

### <a name="procedure"></a>Yordam
1. Oluşturma bir **.NET sınıf kitaplığı** projesi.
   <ol type="a">
     <li>Başlatma <b>Visual Studio 2017</b> veya <b>Visual Studio 2015</b> veya <b>Visual Studio 2013'ün</b> veya <b>Visual Studio 2012</b>.</li>
     <li><b>Dosya</b>’ya tıklayın, <b>Yeni</b>’nin üzerine gelin ve <b>Proje</b>’ye tıklayın.</li>
     <li><b>Şablonlar</b>’ı genişletin ve <b>Visual C#</b> seçeneğini belirleyin. Bu kılavuzda, C# kullanın, ancak özel etkinlik geliştirmek için herhangi bir .NET dil kullanabilirsiniz.</li>
     <li>Seçin <b>sınıf kitaplığı</b> proje türleri sağdaki listeden. VS 2017 ' seçin <b>sınıf kitaplığı (.NET Framework)</b> </li>
     <li>Girin <b>MyDotNetActivity</b> için <b>adı</b>.</li>
     <li>Seçin <b>C:\ADFGetStarted</b> için <b>konumu</b>.</li>
     <li>Projeyi oluşturmak için <b>Tamam</b>'a tıklayın.</li>
   </ol>
2.Tıklatın **Araçları**, işaret **NuGet Paket Yöneticisi**, tıklatıp **Paket Yöneticisi Konsolu**.
3. Paket Yöneticisi konsolunda içeri aktarmak için aşağıdaki komutu yürütün **Microsoft.Azure.Management.DataFactories**.

    ```PowerShell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. İçeri aktarma **Azure Storage** projesi için NuGet paketi.

    ```PowerShell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    > [!IMPORTANT]
    > Veri Fabrikası Başlatıcı WindowsAzure.Storage 4.3 sürümünü gerektirir. Özel Etkinlik projenizde Azure Storage derleme daha sonraki bir sürümü başvuru eklerseniz, etkinlik yürütüldüğünde, bir hata görürsünüz. Hatayı gidermek için bkz: [Appdomain yalıtım](#appdomain-isolation) bölümü. 
5. Aşağıdakileri ekleyin **kullanarak** projenin kaynak dosyasında deyimlerini.

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
7. Sınıfın adını değiştirmek **MyDotNetActivity** ve buradan türetebilir **IDotNetActivity** arabirimi aşağıdaki kod parçacığında gösterildiği gibi:

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. Uygulama (Ekle) **yürütme** yöntemi **IDotNetActivity** arabirimini **MyDotNetActivity** sınıfı ve aşağıdaki örnek kod yöntemi kopyalayabilirsiniz.

    Aşağıdaki örnek veri dilimi ile ilişkili her bir blob içinde arama terimi ("Microsoft") oluşumları sayısını sayar.

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
    
        // get the first Azure Storate linked service from linkedServices object
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
9. Aşağıdaki yardımcı yöntem ekler: 

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

    Veri kümesi işaret klasörüne yol GetFolderPath yöntemi döndürür ve GetFileName yöntemi dataset işaret blob/dosya adını döndürür. {Year} gibi değişkenler kullanarak, havefolderPath tanımlıyorsa {Month} olarak dizedir çalışma zamanı değerlerle değiştirmeden {Day} vb., yöntemi döndürür. Bkz: [genişletilmiş özellikler erişim](#access-extended-properties) erişme SliceStart, SliceEnd, vb. ile ilgili ayrıntılar için bölüm.    

    ```JSON
    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "adftutorial/inputfolder/",
    ```

    Calculate yöntemi anahtar sözcüğü girdi dosyaları (BLOB klasöründe) Microsoft örneklerinin sayısını hesaplar. Arama terimi kodda sabit kodlanmış ("Microsoft").
10. Projeyi derleyin. Tıklatın **yapı** tıklatın ve menüden **yapı çözümü**.

    > [!IMPORTANT]
    > Projeniz için hedef çerçeve olarak .NET Framework 4.5.2 kümesi sürümü: projeye sağ tıklayın ve **özellikleri** hedef Framework'ü ayarlamak için. Veri Fabrikası karşı .NET Framework sürüm 4.5.2 daha sonra derlenmiş özel etkinlikler desteklemez.

11. Başlatma **Windows Explorer**ve gidin **bin\debug** veya **bin\release** klasörü yapı türüne bağlı olarak.
12. Zip dosyası oluşturma **MyDotNetActivity.zip** içindeki tüm ikili dosyaları içeren <project folder>\bin\Debug klasörünü. Dahil **MyDotNetActivity.pdb** başarısız olduysa, soruna neden kaynak kodunda satır numarası gibi ek ayrıntıları almak için dosya. 

    > [!IMPORTANT]
    > Özel etkinliğin zip dosyasındaki tüm dosyalar alt klasör olmadan **en üst düzeyde** olmalıdır.

    ![İkili çıktı dosyaları](./media/data-factory-use-custom-activities/Binaries.png)
14. Adlı bir blob kapsayıcı oluşturun **customactivitycontainer** zaten yoksa. 
15. Bir BLOB customactivitycontainer olarak MyDotNetActivity.zip karşıya bir **genel amaçlı** AzureStorageLinkedService tarafından başvurulan Azure blob storage (değil seyrek/cool Blob Depolama).  

> [!IMPORTANT]
> Visual Studio'da bir Data Factory projesi içeren bir çözüm için bu .NET etkinliği proje ekleyin ve .NET etkinliği projesine bir başvuru veri fabrikası uygulama projesi eklerseniz, el ile zip dosyası oluşturma ve genel amaçlı Azure blob depolama alanına yüklemek son iki adımı gerçekleştirin gerekmez. Visual Studio kullanarak Data Factory varlıklarını yayımladığınızda, yayımlama işlemi tarafından adımları otomatik olarak yapılır. Daha fazla bilgi için bkz: [Visual Studio Data Factory projesindeki](#data-factory-project-in-visual-studio) bölümü.

## <a name="create-a-pipeline-with-custom-activity"></a>Özel etkinliği ile işlem hattı oluşturma
Özel bir aktivite oluşturulur ve bir blob kapsayıcısını ikililerini zip dosyası karşıya bir **genel amaçlı** Azure depolama hesabı. Bu bölümde, bir Azure data factory özel etkinlik kullanan sahip işlem hattı oluşturun.

Özel Etkinlik için giriş veri kümesi adftutorial kapsayıcı blob depolama customactivityinput klasöründeki BLOB'lar (dosyaları) temsil eder. Çıktı veri kümesi etkinliğinin çıkış BLOB'lar adftutorial kapsayıcı blob depolama customactivityoutput klasöründeki temsil eder.

Oluşturma **dosya.txt'yi** dosya aşağıdaki içeriğe ve için karşıya **customactivityinput** klasöründe **adftutorial** kapsayıcı. Zaten yoksa adftutorial kapsayıcı oluşturun. 

```
test custom activity Microsoft test custom activity Microsoft
```

İki veya daha fazla dosya klasör sahip olsa bile giriş klasörü Azure Data factory'de bir dilim karşılık gelir. Her dilimi ardışık düzen tarafından işlendiğinde, özel etkinlik tüm BLOB'ları, dilim için giriş klasörü dolaşır.

Bir dosyayla adftutorial\customactivityoutput klasöründeki bir veya daha fazla satırlara sahip (aynı sayıda BLOB giriş klasöründe) çıkış bakın:

```
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2016-11-16-00/file.txt.
```


Bu bölümde gerçekleştirdiğiniz adımlar şunlardır:

1. Oluşturma bir **veri fabrikası**.
2. Oluşturma **bağlantılı Hizmetleri** Vm'lerinde Azure Batch havuzu için özel etkinlik çalışır ve giriş/çıkış BLOB'ları tutan Azure Storage.
3. Giriş ve çıkış oluşturma **veri kümeleri** giriş ve çıkış özel etkinliği temsil eden.
4. Oluşturma bir **ardışık düzen** özel etkinlik kullanır.

> [!NOTE]
> Oluşturma **dosya.txt'yi** ve zaten yapmadıysanız bir blob kapsayıcısına yükleyin. Önceki bölümdeki yönergelere bakın.   

### <a name="step-1-create-the-data-factory"></a>1. adım: veri fabrikası oluşturma
1. Azure portalında oturum açtıktan sonra aşağıdaki adımları uygulayın:
   1. Sol menüde **YENİ**’ye tıklayın.
   2. Tıklatın **veri + analiz** içinde **yeni** dikey.
   3. **Veri analizi** dikey penceresinde **Data Factory**’ye tıklayın.
   
    ![Yeni Azure Data Factory menüsü](media/data-factory-use-custom-activities/new-azure-data-factory-menu.png)
2. İçinde **yeni data factory** dikey penceresinde girin **CustomActivityFactory** adı. Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Hatayı alırsanız: **veri fabrikası adı "CustomActivityFactory" kullanılabilir değil**, data factory adını değiştirin (örneğin, **yournameCustomActivityFactory**) ve oluşturmayı yeniden deneyin.

    ![Yeni Azure Data Factory dikey penceresi](media/data-factory-use-custom-activities/new-azure-data-factory-blade.png)
3. Tıklatın **kaynak grubu adı**, varolan bir kaynak grubu seçin veya bir kaynak grubu oluşturun.
4. Doğru kullandığını doğrulayın **abonelik** ve **bölge** oluşturulacak data factory istediğiniz.
5. **Yeni data factory** dikey penceresinde **Oluştur**’a tıklayın.
6. Oluşturulmakta veri fabrikası gördüğünüz **Pano** Azure portalının.
7. Veri Fabrikası başarıyla oluşturulduktan sonra data factory içeriği gösterilir Data Factory dikey penceresine bakın.
    
    ![Data Factory dikey penceresi](media/data-factory-use-custom-activities/data-factory-blade.png)

### <a name="step-2-create-linked-services"></a>2. adım: bağlı hizmetler oluşturma
Bağlı hizmetler veri depolarını veya işlem hizmetlerini Azure data factory’ye bağlar. Bu adımda data factory'nizi Azure Storage hesabını ve Azure Batch hesabı bağlayın.

#### <a name="create-azure-storage-linked-service"></a>Azure Storage bağlı hizmeti oluşturma
1. Tıklatın **yazar ve dağıtma** döşemesinin **DATA FACTORY** dikey **CustomActivityFactory**. Data Factory Düzenleyici bakın.
2. Tıklatın **yeni veri deposu** komut çubuğu ve seçin **Azure depolama**. Düzenleyicide Azure Storage bağlı hizmeti oluşturmak için JSON betiğini görmeniz gerekir.
    
    ![Yeni veri deposu - Azure depolama](media/data-factory-use-custom-activities/new-data-store-menu.png)
3. Değiştir `<accountname>` Azure depolama hesabınızın adıyla ve `<accountkey>` Azure depolama hesabının erişim anahtarı ile. Depolama erişim anahtarınızı nasıl alacağınız hakkında bilgi için bkz. [Depolama erişim anahtarlarını görüntüleme, kopyalama ve yeniden oluşturma](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).

    ![Azure depolama hizmeti beğendiğinizi](media/data-factory-use-custom-activities/azure-storage-linked-service.png)
4. Bağlı hizmeti dağıtmak için komut çubuğunda **Dağıt**’a tıklayın.

#### <a name="create-azure-batch-linked-service"></a>Azure Batch bağlı hizmet oluşturma
1. Data Factory Düzenleyici'yi tıklatın **... Daha fazla** komut çubuğunda **yeni işlem**ve ardından **Azure Batch** menüsünde.

    ![Yeni işlem - Azure toplu işlem](media/data-factory-use-custom-activities/new-azure-compute-batch.png)
2. JSON betiği aşağıdaki değişiklikleri yapın:

   1. Azure Batch hesabı adını belirtin **accountName** özelliği. **URL** gelen **Azure Batch hesabı dikey** aşağıdaki biçimdedir: `http://accountname.region.batch.azure.com`. İçin **batchUri** özelliği JSON içinde ihtiyacınız kaldırmak `accountname.` URL ve kullanım `accountname` için `accountName` JSON özelliği.
   2. Azure Batch hesabı anahtar belirtmeniz **accessKey** özelliği.
   3. Önkoşulları bir parçası olarak, oluşturduğunuz havuzunun adını belirtin **poolName** özelliği. Havuz adı yerine havuzun kimliği de belirtebilirsiniz.
   4. Azure Batch URI'sini belirtin **batchUri** özelliği. Örnek: `https://westus.batch.azure.com`.  
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

       İçin **poolName** özelliği, havuzu havuzunun adı yerine Kimliğini de belirtebilirsiniz.

      > [!IMPORTANT]
      > Hdınsight için yaptığı gibi Data Factory hizmeti Azure toplu işlem için bir isteğe bağlı seçeneği desteklemiyor. Bu gibi durumlarda, kendi Azure Batch havuzu yalnızca bir Azure data factory kullanabilirsiniz.   
    

### <a name="step-3-create-datasets"></a>3. adım: veri kümeleri oluşturma
Bu adımda, girdi ve çıktı verilerini temsil edecek veri kümeleri oluşturma.

#### <a name="create-input-dataset"></a>Girdi veri kümesi oluşturma
1. Data Factory **Düzenleyici**’de açılır listeden **... Daha fazla** komut çubuğunda **yeni veri kümesi**ve ardından **Azure Blob Depolama** açılır menüsünden.
2. Sağ bölmedeki JSON aşağıdaki JSON parçacığıyla değiştirin:

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

   Başlangıç saati ile bu kılavuzda daha sonra bir işlem hattı oluşturma: 2016-11-16T00:00:00Z ve bitiş zamanı: 2016-11-16T05:00:00Z. Beş giriş/çıkış dilimler olduklarından veri saatlik, üretmek için planlanmıştır (arasında **00**: 00:00 -> **05**: 00:00).

   **Sıklığı** ve **aralığı** girdi veri kümesi ayarlanmıştır **saat** ve **1**, girdi dilimi saatlik kullanılabilir olduğunu anlamına gelir. Bu örnekte, aynı (dosya.txt'yi) intputfolder dosyasıdır.

   Yukarıdaki JSON parçacığında bulunan SliceStart sistem değişkeni tarafından temsil edilen her dilim başlangıç zamanlarını şunlardır.
3. Tıklatın **dağıtma** oluşturmak ve dağıtmak için araç çubuğunda **InputDataset**. Düzenleyici’nin başlık çubuğunda **TABLO BAŞARIYLA OLUŞTURULDU** iletisini gördüğünüzü onaylayın.

#### <a name="create-an-output-dataset"></a>Çıktı veri kümesi oluşturma
1. İçinde **Data Factory düzenleyici**, tıklatın **... Daha fazla** komut çubuğunda **yeni veri kümesi**ve ardından **Azure Blob Depolama**.
2. Sağ bölmedeki JSON betiği aşağıdaki JSON betiği ile değiştirin:

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

     Çıktı konumu **adftutorial/customactivityoutput/** ve çıktı dosyası adını yyyy-aa-gg-HH.txt yyyy-aa-gg-HH yıl, ay, tarih ve saat üretilen dilimin olduğu. Bkz: [Geliştirici Başvurusu] [ adf-developer-reference] Ayrıntılar için.

    Bir çıkış blob/dosyası, her girdi dilimi için oluşturulur. İşte bir çıktı dosyası için her dilimi nasıl adlandırılır. Bir çıkış klasöründe oluşturulan tüm çıktı dosyaları: **adftutorial\customactivityoutput**.

   | Dilim | Başlangıç zamanı | Çıkış dosyası |
   |:--- |:--- |:--- |
   | 1 |2016-11-16T00:00:00 |2016-11-16-00.txt |
   | 2 |2016-11-16T01:00:00 |2016-11-16-01.txt |
   | 3 |2016-11-16T02:00:00 |2016-11-16-02.txt |
   | 4 |2016-11-16T03:00:00 |2016-11-16-03.txt |
   | 5 |2016-11-16T04:00:00 |2016-11-16-04.txt |

    Giriş bir klasördeki tüm dosyaları yukarıda belirtilen başlangıç zamanlarını dilimle parçası olduğunu unutmayın. Bu dilim işlendiğinde özel etkinlik her dosyası aracılığıyla tarar ve arama terimi ("Microsoft"), yineleme sayısı ile çıkış dosyasındaki bir satır üretir. İnputfolder üç dosya varsa vardır üç satır her saat dilimi için çıktı dosyasına: 2016-11-16-00.txt, 2016-11-16:01:00:00.txt, vs.
3. Dağıtmak için **OutputDataset**, tıklatın **dağıtma** komut çubuğunda.

### <a name="create-and-run-a-pipeline-that-uses-the-custom-activity"></a>Oluşturma ve özel etkinlik kullanan bir ardışık düzen çalıştırma
1. Data Factory Düzenleyici'yi tıklatın **... Daha fazla**ve ardından **yeni işlem hattı** komut çubuğunda. 
2. Sağ bölmedeki JSON aşağıdaki JSON betiği ile değiştirin:

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

   * **Eşzamanlılık** ayarlanır **2** böylece iki dilimler Azure Batch havuzunda 2 VM tarafından paralel olarak işlenir.
   * Etkinlikler bölümünde bir etkinlik vardır ve bu tür: **DotNetActivity**.
   * **AssemblyName** DLL adına ayarlayın: **MyDotnetActivity.dll**.
   * **EntryPoint** ayarlanır **MyDotNetActivityNS.MyDotNetActivity**.
   * **PackageLinkedService** ayarlanır **AzureStorageLinkedService** özel etkinlik zip dosyasını içeren blob depolama alanına işaret eder. Giriş/çıkış dosyaları ve özel etkinlik zip dosyası için farklı Azure depolama hesapları kullanıyorsanız, başka bir Azure depolama bağlantılı hizmeti oluşturun. Bu makalede, aynı Azure Storage hesabını kullandığınızı varsayar.
   * **PackageFile** ayarlanır **customactivitycontainer/MyDotNetActivity.zip**. Şu biçimdedir: containerforthezip/nameofthezip.zip.
   * Özel Etkinlik alır **InputDataset** giriş olarak ve **OutputDataset** çıktı olarak.
   * Özel Etkinlik linkedServiceName özelliğinin işaret **AzureBatchLinkedService**, özel etkinlik Azure toplu işlemi Vm'lerinde çalıştırmak için gereken bir Azure Data Factory söyler.
   * **isPaused** özelliği ayarlanmış **false** varsayılan olarak. Geçmişte dilimleri başlatmak için ardışık düzen hemen bu örnekte çalışır. Bu özelliği, ardışık düzen duraklatma ve yeniden başlatmak için geri false olarak ayarlamak için true olarak ayarlayın.
   * **Başlat** zaman ve **son** sürelerinin **beş** ardışık düzen tarafından beş dilimlerinin şekilde birbirinden saatleri ve dilimler saatlik, üretilmektedir.
3. İşlem hattını dağıtmak için **dağıtma** komut çubuğunda.

### <a name="monitor-the-pipeline"></a>İşlem hattını izleme
1. Azure Portal'daki Data Factory dikey penceresinde tıklayın **diyagramı**.

    ![Diyagram kutucuğu](./media/data-factory-use-custom-activities/DataFactoryBlade.png)
2. Diyagram Görünümü'nde şimdi OutputDataset'yi tıklatın.

    ![Diyagram görünümü](./media/data-factory-use-custom-activities/diagram.png)
3. Beş çıkış dilimi hazır durumda olduğunu görmeniz gerekir. Bunlar hazır durumda değilse, bunlar henüz üretilmiş henüz. 

   ![Çıktı dilimleri](./media/data-factory-use-custom-activities/OutputSlices.png)
4. Çıkış dosyaları blob depolamada oluşturulan doğrulayın **adftutorial** kapsayıcı.

   ![Özel etkinliğinden çıktı][image-data-factory-ouput-from-custom-activity]
5. Çıktı dosyası açarsanız, aşağıdaki çıkış benzer bir çıktı görmeniz gerekir:

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2016-11-16-00/file.txt.
    ```
6. Kullanım [Azure portal] [ azure-preview-portal] veya veri fabrikası, ardışık düzen ve veri kümelerini izlemek için Azure PowerShell cmdlet'leri. Gelen iletileri görebilirsiniz **ActivityLogger** kodunda özel etkinlik portal veya cmdlet'ler kullanarak indirebilir günlüklerine (özellikle kullanıcı-0.log).

   ![Özel etkinliğinden günlüklerini indirin][image-data-factory-download-logs-from-custom-activity]

Bkz: [izleme ve yönetme ardışık düzen](data-factory-monitor-manage-pipelines.md) veri kümelerini ve işlem hatlarını izleme için ayrıntılı adımlar için.      

## <a name="data-factory-project-in-visual-studio"></a>Visual Studio Data Factory projesi  
Oluşturma ve Azure portalını kullanarak yerine Visual Studio kullanarak Data Factory varlıklarını yayımlama. Visual Studio kullanarak Data Factory varlıklarını yayımlama, bakın ve ayrıntılı oluşturma hakkında daha fazla bilgi için [Visual Studio kullanarak ilk işlem hattınızı oluşturma](data-factory-build-your-first-pipeline-using-vs.md) ve [kopyalama verileri Azure Blob'tan Azure SQL'e](data-factory-copy-activity-tutorial-using-visual-studio.md) makaleleri.

Visual Studio'da veri fabrikası proje oluşturuyorsanız, aşağıdaki ek adımları uygulayın:
 
1. Özel Etkinlik projeyi içeren bir Visual Studio çözümüne Data Factory projenize ekleyin. 
2. Veri Fabrikası projeden .NET etkinliği projesine bir başvuru ekleyin. Data Factory projesine sağ tıklayın, fareyle **Ekle**ve ardından **başvuru**. 
3. İçinde **Başvuru Ekle** iletişim kutusunda **MyDotNetActivity** proje öğesini tıklatıp **Tamam**.
4. Derleme ve çözüm yayımlayın.

    > [!IMPORTANT]
    > Data Factory varlıklarını yayımladığınızda, ZIP dosyası sizin için otomatik olarak oluşturulur ve blob kapsayıcısına karşıya: customactivitycontainer. Blob kapsayıcısı mevcut değilse otomatik olarak çok oluşturulur.  


## <a name="data-factory-and-batch-integration"></a>Veri Fabrikası ve toplu tümleştirmesi
Data Factory hizmetinin bir iş Azure Batch adıyla oluşturur: **adf poolname: iş xxx**. Tıklatın **işleri** sol menüden. 

![Azure Data Factory - toplu işler](media/data-factory-use-custom-activities/data-factory-batch-jobs.png)

Bir görev, her bir dilim etkinlik çalıştırması için oluşturulur. Beş dilimler işlenmeye hazır varsa, beş görev alanında bu işin oluşturulur. Batch havuzundaki birden çok işlem düğümleri varsa, iki veya daha fazla dilim paralel olarak çalıştırılabilir. İşlem düğümü başına en fazla görevleri ayarlarsanız > 1'e, aynı işlem üzerinde çalışan birden fazla dilim sahip olabilir.

![Azure Data Factory - toplu iş görevleri](media/data-factory-use-custom-activities/data-factory-batch-job-tasks.png)

Aşağıdaki diyagram, Azure Data Factory ve toplu işlem görevlerini arasındaki ilişkiyi gösterir.

![Veri Fabrikası & toplu işlem](./media/data-factory-use-custom-activities/DataFactoryAndBatch.png)

## <a name="troubleshoot-failures"></a>Hatalarını giderme
Sorun giderme birkaç temel teknikleri oluşur:

1. Aşağıdaki hata görürseniz, bir etkin/Cool blob depolama genel amaçlı Azure blob storage kullanarak yerine kullanıyor olabilir. Zip dosyasını karşıya bir **genel amaçlı Azure depolama hesabı**. 
 
    ```
    Error in Activity: Job encountered scheduling error. Code: BlobDownloadMiscError Category: ServerError Message: Miscellaneous error encountered while downloading one of the specified Azure Blob(s).
    ``` 
2. Aşağıdaki hatayla karşılaşırsanız, CS dosyasındaki sınıfın adı için belirtilen ad eşleştiğini onaylayın **EntryPoint** JSON işlem hattındaki özelliği. Kılavuzda sınıfı adıdır: MyDotNetActivity ve JSON EntryPoint: MyDotNetActivityNS. **MyDotNetActivity**.

    ```
    MyDotNetActivity assembly does not exist or doesn't implement the type Microsoft.DataFactories.Runtime.IDotNetActivity properly
    ```

   Adları eşleşiyorsa, bu tüm ikili dosyaları olduğunu onaylamak **kök klasörü** ZIP dosyasının. Diğer bir deyişle, ZIP dosyasının açtığınızda, tüm dosyaları kök klasöründe, tüm alt klasörlerde görmeniz gerekir.   
3. Girdi dilimi ayarlanmamışsa **hazır**, giriş klasör yapısı doğru olduğunu onaylayın ve **dosya.txt'yi** giriş klasörlerde bulunmaktadır.
3. İçinde **yürütme** özel etkinliklerinizi, kullanım yöntemi **IActivityLogger** sorunlarını gidermenize yardımcı olacak bilgileri oturum nesnesi. Günlüğe kaydedilen iletilere kullanıcı günlük dosyalarında görünmesini (adlı bir veya daha fazla: kullanıcı 0.log, kullanıcı 1.log, kullanıcı 2.log, vs.).

   İçinde **OutputDataset** dikey penceresinde görmek için dilimi tıklatın **veri DİLİMİ** dikey penceresinde, dilim için. Gördüğünüz **etkinlik çalışması** bu dilim için. Bir etkinlik için dilim görmeniz gerekir. Komut çubuğunda Çalıştır'ı tıklatın, aynı dilim için başka bir etkinlik başlatabilirsiniz.

   Etkinliğin çalışma tıklattığınızda gördüğünüz **etkinlik çalışma ayrıntıları** günlük dosyalarının listesini içeren dikey. User_0.log dosyasında günlüğe kaydedilen iletilere bakın. Hata oluştuğunda, yeniden deneme sayısı 3 JSON ardışık düzeni/etkinliği olarak ayarlandığından, üç Etkinlik çalışması görürsünüz. Etkinlik Çalıştır'ı tıklatın, hatayı gidermek için gözden geçirebilirsiniz günlük dosyalarına bakın.

   Günlük dosyaları listesinde tıklatın **kullanıcı 0.log**. Sağ panelde kullanarak sonucu olan **IActivityLogger.Write** yöntemi. Tüm iletileri görmüyorsanız adlı daha fazla günlük dosyaları olup olmadığını denetleyin: user_1.log, user_2.log vs. Son ileti günlüğe sonra Aksi halde, kod başarısız olmuş olabilir.

   Ayrıca, denetleme **sistem 0.log** sistem hata iletileri ve özel durumlar için.
4. Dahil **PDB** hata ayrıntılarını bilgileri gibi böylece zip dosyasında dosya **çağrı yığını** bir hata oluştuğunda.
5. Özel etkinliğin zip dosyasındaki tüm dosyalar alt klasör olmadan **en üst düzeyde** olmalıdır.
6. Emin **assemblyName** (MyDotNetActivity.dll) **entryPoint**(MyDotNetActivityNS.MyDotNetActivity) **packageFile** (customactivitycontainer/MyDotNetActivity.zip) ve **packageLinkedService** (işaret etmelidir **genel amaçlı**zip dosyasını içeren Azure blob depolama) için doğru değerleri ayarlayın.
7. Bir hatayı düzelttiyseniz ve dilimi yeniden işlemek istiyorsanız **OutputDataset** dikey penceresindeki dilime sağ tıklayın ve **Çalıştır**’a tıklayın.
8. Aşağıdaki hata görürseniz, Azure Storage paketi sürümü > 4.3.0 kullanıyor. Veri Fabrikası Başlatıcı WindowsAzure.Storage 4.3 sürümünü gerektirir. Bkz: [Appdomain yalıtım](#appdomain-isolation) bölümünde bir iş-geçici bir çözüm bulmak için Azure Storage derlemenin sonraki bir sürümünü kullanmanız gerekiyorsa. 

    ```
    Error in Activity: Unknown error in module: System.Reflection.TargetInvocationException: Exception has been thrown by the target of an invocation. ---> System.TypeLoadException: Could not load type 'Microsoft.WindowsAzure.Storage.Blob.CloudBlob' from assembly 'Microsoft.WindowsAzure.Storage, Version=4.3.0.0, Culture=neutral, 
    ```

    4.3.0 kullanırsanız, Azure Storage paketin sürümü Azure Storage paketi sürümü > 4.3.0 varolan başvurusunu kaldırın. Ardından, NuGet Paket Yöneticisi konsolundan aşağıdaki komutu çalıştırın. 

    ```PowerShell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    Projeyi derleyin. Sürüm > 4.3.0 Azure.Storage bütünleştirilmiş klasörüne silin. İkili dosyaları ve PDB dosya ile bir zip dosyası oluşturun. Bu bir blob kapsayıcısında (customactivitycontainer) eski zip dosyasını değiştirin. Başarısız olan dilimler yeniden (dilim sağ tıklatın ve Çalıştır'ı tıklatın).   
8. Özel Etkinlik kullanmayan **app.config** paketinizi dosyasından. Bu nedenle, kodunuzu yapılandırma dosyasından bağlantı dizelerini yazıyorsa, çalışma zamanında çalışmıyor. Azure Batch kullanarak tüm gizli tutmak için olduğunda en iyi uygulama bir **Azure KeyVault**, korumak için bir sertifika tabanlı hizmet asıl adı kullanmasını **keyvault**ve Azure Batch havuzu sertifikaya dağıtın. Böylece .NET özel etkinliği çalıştırma sırasında KeyVault’tan parolalara erişebilir. Bu çözüm genel bir çözümdür ve gizli anahtarı, yalnızca bağlantı dizesi herhangi bir türde ölçeklendirebilirsiniz.

   Daha kolay bir çözüm (ancak en iyi yöntem değildir): oluşturabileceğiniz bir **Azure SQL bağlı hizmeti** bağlantı dizesi ayarlarıyla bağlantılı hizmet kullanan bir veri kümesi oluşturma ve sahte bir giriş veri kümesi için özel .NET etkinlik olarak dataset zincir. Özel Etkinlik kodu bağlantılı hizmetin bağlantı dizesinde daha sonra erişebilirsiniz.  

## <a name="update-custom-activity"></a>Özel Etkinlik güncelleştirme
Özel Etkinlik için kod güncelleştirirseniz, onu oluşturmak ve blob depolama birimine yeni ikili dosyaları içeren zip dosyasını karşıya yükleyin.

## <a name="appdomain-isolation"></a>Uygulama etki alanı yalıtımı
Bkz [arası AppDomain örnek](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) , veri fabrikası başlatıcısı tarafından kullanılan derleme sürümleri için sınırlı değildir özel bir aktivite oluşturulacağını gösterir (örnek: WindowsAzure.Storage v4.3.0, Newtonsoft.Json v6.0.x, vs.).

## <a name="access-extended-properties"></a>Genişletilmiş özellikler erişim
Aşağıdaki örnekte gösterildiği gibi JSON etkinliğinde genişletilmiş özellikler bildirebilirsiniz:

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


Örnekte, iki genişletilmiş özellik vardır: **SliceStart** ve **DataFactoryName**. SliceStart değeri SliceStart sistem değişkeni temel alır. Bkz: [sistem değişkenleri](data-factory-functions-variables.md) desteklenen sistem değişkenleri listesi. İlgili DataFactoryName CustomActivityFactory için sabit kodlu değer.

Bu özellikler genişletilmiş erişmek için **yürütme** yöntemi, aşağıdaki kodu benzer kod kullanın:

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
Bir Azure Batch havuzuyla oluşturabilirsiniz **otomatik ölçeklendirme** özelliği. Örneğin, 0 özel VM'ler ve beklemedeki görevlerin sayısına dayalı bir otomatik ölçeklendirme formülü ile bir azure batch havuzu oluşturabilirsiniz. 

Formül örneği burada aşağıdaki davranışı elde eder: havuzu başlangıçta oluşturulduğunda 1 VM ile başlar. $PendingTasks ölçüm tanımlar görev sayısı çalışan + (kuyruğa alınmış) etkin durumu.  Formül Son 180 saniye içinde görevleri bekleyen ortalama sayısı bulur ve TargetDedicated uygun şekilde ayarlar. TargetDedicated hiçbir zaman 25 VM'ler gider sağlar. Bu nedenle, yeni görevler gönderildiği haliyle havuzu otomatik olarak büyür ve görevler tamamlanınca VM'ler boş bir birer birer hale ve bu sanal makineleri otomatik ölçeklendirmeyi küçültür. startingNumberOfVMs ve maxNumberofVMs gereksinimlerinize göre ayarlanabilir.

Otomatik ölçeklendirme formülü:

``` 
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
```

Bkz: [ölçek işlem düğümlerini Azure Batch havuzunda otomatik olarak](../../batch/batch-automatic-scaling.md) Ayrıntılar için.

Varsayılan havuzu kullanıyorsanız [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), Batch hizmeti VM özel etkinlik çalıştırmadan önce hazırlamak için 15-30 dakika sürebilir.  Havuz farklı autoScaleEvaluationInterval kullanıyorsanız, Batch hizmeti autoScaleEvaluationInterval + 10 dakika sürebilir.

## <a name="use-hdinsight-compute-service"></a>Hdınsight işlem hizmeti kullanın
İzlenecek özel etkinliği çalıştırmak için Azure toplu işlem kullanılır. Ayrıca kendi Windows tabanlı Hdınsight kümesi kullanın veya isteğe bağlı Windows tabanlı Hdınsight kümesi oluşturmak ve Hdınsight kümesi üzerinde çalışan özel etkinlik sahip veri fabrikası sahip. Hdınsight kümesi kullanmaya yönelik üst düzey adımlar şunlardır.

> [!IMPORTANT]
> Özel .NET etkinlikler yalnızca Windows tabanlı Hdınsight kümelerinde çalıştırın. Geçici bir çözüm için bu sınırlama, Linux tabanlı Hdınsight kümesinde özel Java kodu çalıştırmak için azaltmak Haritası aktivitesi kullanmaktır. Bir Azure Batch havuzu VM'lerin bir Hdınsight kümesi yerine özel etkinlikleri çalıştırmak için kullandığınız başka bir seçenektir.
 

1. Bir Azure Hdınsight bağlı hizmeti oluşturma.   
2. Kullanım Hdınsight bağlantılı hizmeti yerine **AzureBatchLinkedService** JSON işlem hattındaki.

İzlenecek yol ile test etmek isterseniz, değiştirmek **Başlat** ve **son** böylece Azure Hdınsight hizmeti ile senaryoyu test için ardışık zaman.

#### <a name="create-azure-hdinsight-linked-service"></a>Azure HDInsight bağlı hizmeti oluşturma
Azure Data Factory hizmetine bir istek üzerine küme oluşturmayı destekler ve çıktı verileri üretemedi girişi işlemek için kullanın. Kendi kümenizi, aynı gerçekleştirmek için de kullanabilirsiniz. İsteğe bağlı Hdınsight kümesi kullandığınızda, bir küme için her bir dilim oluşturulan. Oysa kendi Hdınsight kümenizi kullanırsanız, küme dilim hemen işlemek hazırdır. İsteğe bağlı küme kullandığınızda, bu nedenle, çıktı verilerini kendi kümenizi kullandığınızda, olabildiğince çabuk göremeyebilirsiniz.

> [!NOTE]
> Çalışma zamanında .NET etkinliği bir örneği yalnızca Hdınsight kümesinde bir alt düğüm üzerinde çalışır; birden çok düğümde çalıştırmak için genişletilemez. .NET etkinliği birden çok örneğini farklı Hdınsight küme düğümlerinde paralel olarak çalıştırılabilir.
>
>

##### <a name="to-use-an-on-demand-hdinsight-cluster"></a>İsteğe bağlı Hdınsight kümesi kullanmak için
1. İçinde **Azure portal**, tıklatın **geliştir ve Dağıt** Data Factory giriş sayfasında.
2. Data Factory Düzenleyici'yi tıklatın **yeni işlem** seçin ve komut çubuğunda **isteğe bağlı Hdınsight kümesi** menüsünde.
3. JSON betiği aşağıdaki değişiklikleri yapın:

   1. İçin **clusterSize** özelliği, Hdınsight küme boyutunu belirtin.
   2. İçin **timeToLive** özelliği, silinmeden önce ne kadar süreyle müşteri süreyle boşta kalabileceğini belirtin.
   3. İçin **sürüm** özelliği, kullanmak istediğiniz Hdınsight sürüm belirtin. Bu özellik bıraksanız en son sürümü kullanılır.  
   4. İçin **linkedServiceName**, belirtin **AzureStorageLinkedService**.

        ```JSON
        {
           "name": "HDInsightOnDemandLinkedService",
           "properties": {
               "type": "HDInsightOnDemand",
               "typeProperties": {
                   "clusterSize": 4,
                   "timeToLive": "00:05:00",
                   "osType": "Windows",
                   "linkedServiceName": "AzureStorageLinkedService",
               }
           }
        }
        ```

    > [!IMPORTANT]
    > Özel .NET etkinlikler yalnızca Windows tabanlı Hdınsight kümelerinde çalıştırın. Geçici bir çözüm için bu sınırlama, Linux tabanlı Hdınsight kümesinde özel Java kodu çalıştırmak için azaltmak Haritası aktivitesi kullanmaktır. Bir Azure Batch havuzu VM'lerin bir Hdınsight kümesi yerine özel etkinlikleri çalıştırmak için kullandığınız başka bir seçenektir.

4. Bağlı hizmeti dağıtmak için komut çubuğunda **Dağıt**’a tıklayın.

##### <a name="to-use-your-own-hdinsight-cluster"></a>Kendi Hdınsight kümenizi kullanmak için:
1. İçinde **Azure portal**, tıklatın **geliştir ve Dağıt** Data Factory giriş sayfasında.
2. İçinde **Data Factory düzenleyici**, tıklatın **yeni işlem** seçin ve komut çubuğunda **Hdınsight kümesi** menüsünde.
3. JSON betiği aşağıdaki değişiklikleri yapın:

   1. İçin **clusterUri** özelliği, Hdınsight için URL'yi girin. Örneğin: https://<clustername>.azurehdinsight.net/     
   2. İçin **kullanıcıadı** özelliği, Hdınsight kümesi erişimi olan kullanıcı adı girin.
   3. İçin **parola** özelliği, kullanıcı için parola girin.
   4. İçin **LinkedServiceName** özelliği girin **AzureStorageLinkedService**.
4. Bağlı hizmeti dağıtmak için komut çubuğunda **Dağıt**’a tıklayın.

Bkz: [işlem bağlı Hizmetleri](data-factory-compute-linked-services.md) Ayrıntılar için.

İçinde **JSON kanal**, Hdınsight kullanma (isteğe bağlı veya kendi) bağlantılı hizmeti:

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
        "LinkedServiceName": "HDInsightOnDemandLinkedService",
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

## <a name="create-a-custom-activity-by-using-net-sdk"></a>.NET SDK kullanarak bir özel etkinlik oluşturma
Kılavuzda bu makalede, Azure portalını kullanarak özel etkinlik kullanan bir ardışık düzen ile bir veri fabrikası oluşturun. Aşağıdaki kod .NET SDK kullanarak data factory oluşturulacağını gösterir. Program aracılığıyla ardışık düzenlerinde oluşturmak için SDK'sını kullanma hakkında daha fazla ayrıntı bulabilirsiniz [.NET API kullanarak kopyalama etkinliği ile işlem hattı oluşturma](data-factory-copy-activity-tutorial-using-dotnet-api.md) makalesi. 

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
[Azure Data Factory - yerel ortamınıza](https://github.com/gbrueckl/Azure.DataFactory.LocalEnvironment) örneği github'daki özel .NET etkinlikler Visual Studio içinde hata ayıklama olanak tanıyan bir araç içerir.  


## <a name="sample-custom-activities-on-github"></a>Github'da örnek özel etkinlikler
| Örnek | Hangi özel etkinlik mu |
| --- | --- |
| [HTTP veri yükleyici](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample). |Verileri Azure Blob veri fabrikasında özel C# etkinlik kullanarak depolama için bir HTTP uç noktasından indirir. |
| [Twitter düşünceleri çözümleme örneği](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |Bir Azure ML model ve düşünceleri analiz, Puanlama, tahmin vb. çağırır. |
| [R betiği Çalıştır](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). |R betiği Hdınsight kümenize zaten R yüklü üzerinde olan RScript.exe çalıştırarak çağırır. |
| [AppDomain .NET etkinliği arası](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) |Olanları Data Factory başlatıcısı tarafından kullanılan farklı derleme sürümlerini kullanır |
| [Azure Analysis Services modelinde yeniden işleyin](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/AzureAnalysisServicesProcessSample) |  Azure Analysis Services modelinde yeniden işler. |

[batch-net-library]: ../../batch/batch-dotnet-get-started.md
[batch-create-account]: ../../batch/batch-account-create-portal.md
[batch-technical-overview]: ../../batch/batch-technical-overview.md
[batch-get-started]: ../../batch/batch-dotnet-get-started.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[data-factory-introduction]: data-factory-introduction.md
[azure-powershell-install]: https://github.com/Azure/azure-sdk-tools/releases


[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456

[new-azure-batch-account]: https://msdn.microsoft.com/library/mt125880.aspx
[new-azure-batch-pool]: https://msdn.microsoft.com/library/mt125936.aspx
[azure-batch-blog]: http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx

[nuget-package]: http://go.microsoft.com/fwlink/?LinkId=517478
[adf-developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[azure-preview-portal]: https://portal.azure.com/

[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[hivewalkthrough]: data-factory-data-transformation-activities.md

[image-data-factory-ouput-from-custom-activity]: ./media/data-factory-use-custom-activities/OutputFilesFromCustomActivity.png

[image-data-factory-download-logs-from-custom-activity]: ./media/data-factory-use-custom-activities/DownloadLogsFromCustomActivity.png
