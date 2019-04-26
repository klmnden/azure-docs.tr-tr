---
title: Data Factory ve Batch kullanarak büyük ölçekli veri kümelerini işleme | Microsoft Docs
description: Paralel işleme özelliği, Azure Batch kullanarak büyük miktarda verileri bir Azure Data Factory işlem hattı işlemi açıklar.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 688b964b-51d0-4faa-91a7-26c7e3150868
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: f78275af5faaf19a4993a5ae4414b0163f9a4d9d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60487813"
---
# <a name="process-large-scale-datasets-by-using-data-factory-and-batch"></a>Data Factory ve Batch kullanarak işlem büyük ölçekli veri kümeleri
> [!NOTE]
> Bu makale, Azure Data Factory’nin genel kullanıma açık olan 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [özel etkinlikleri Data factory'de](../transform-data-using-dotnet-custom-activity.md).

Bu makalede, bir taşır ve büyük ölçekli veri kümelerini otomatik ve zamanlanmış bir şekilde işleyen örnek bir çözüm mimarisini açıklar. Ayrıca, Data Factory ve Azure Batch kullanarak çözüm uygulamak için bir uçtan uca kılavuz sağlar.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Tüm örnek çözümünü kılavuz içerdiğinden bu tipik bir makale uzun bir makaledir. Bu hizmetler hakkında bilgi edinebilirsiniz Batch ve Data Factory için yeni başladıysanız ve nasıl birlikte çalışır. Hizmetleri ile ilgili bir sorun bildirin ve Tasarım/Çözüm Mimarileri mi oluşturuyorsunuz, makalenin mimarisi bölümüne odaklanabilirsiniz. Bir prototip veya çözüm geliştiriyorsanız, gözden geçirme hakkında adım adım yönergeleri denemek isteyebilirsiniz. Bu içerik ve nasıl kullanacağınız hakkındaki yorumlarınızı davet ediyoruz.

İlk olarak, Data Factory ve Batch hizmetleri size nasıl yardımcı olabilir işlem büyük veri kümelerini bulutta bakalım.     


## <a name="why-azure-batch"></a>Neden Azure Batch?
 Batch, büyük ölçekli paralel ve yüksek performanslı hesaplama (HPC) uygulamalarını bulutta verimli bir şekilde çalıştırmak için kullanabilirsiniz. Bu sanal makinelerin (VM'ler) yönetilen koleksiyonunda çalıştırılacak işlem yoğunluklu işleri zamanlayan bir platform hizmetidir. Ayrıca, işinizin gereksinimlerini karşılayacak şekilde işlem kaynaklarını otomatik olarak ölçeklendirebilirsiniz.

Batch hizmetiyle, uygulamalarınızı paralel olarak ve ölçekte yürütmek için Azure işlem kaynaklarını tanımlayın. Çalıştırabileceğiniz isteğe bağlı veya zamanlanmış işler. El ile oluşturma, yapılandırma ve bir HPC Kümesi, tek tek sanal makineleri, sanal ağlar veya karmaşık iş ve görev zamanlama altyapısını yönetmek gerek yoktur.

 Batch ile ilgili bilgi sahibi değilseniz, aşağıdaki makalelere, bu makalede açıklanan çözüm mimarisi/uygulama anlamanıza yardımcı:   

* [Batch temel bilgileri](../../batch/batch-technical-overview.md)
* [Batch özelliklerine genel bakış](../../batch/batch-api-basics.md)

İsteğe bağlı olarak Batch hakkında daha fazla bilgi için bkz: [Batch belgelerini](https://docs.microsoft.com/azure/batch/).

## <a name="why-azure-data-factory"></a>Neden Azure Data Factory?
Data Factory, verilerin taşınmasını ve dönüştürülmesini düzenleyen ve otomatikleştiren bulut tabanlı bir veri tümleştirme hizmetidir. Data Factory, şirket içinden veri taşıma ve merkezi bir veri deposuna veri depolarında bulut yönetilen veri işlem hatları oluşturmak için kullanabilirsiniz. Azure Blob Depolama buna bir örnektir. Data Factory işlem/veri dönüştürme gibi Azure HDInsight ve Azure Machine Learning hizmetlerini kullanarak için kullanabilirsiniz. Ayrıca, zamanlanmış bir şekilde (örneğin, saatlik, günlük ve haftalık) çalıştırmak için veri işlem hatlarını zamanlayabilirsiniz. İzleme ve işlem hatlarını sorunlarını tanımlamak ve eylem için bir bakışta yönetme.

  Data factory'yi kullanmaya alışık değilseniz, aşağıdaki makaleler, bu makalede açıklanan çözüm mimarisi/uygulamayı anlamaya yardımcı:  

* [Data Factory'ye giriş](data-factory-introduction.md)
* [İlk veri işlem hattı oluşturma](data-factory-build-your-first-pipeline.md)   

İsteğe bağlı olarak, Data Factory hakkında daha fazla bilgi için bkz: [Data Factory belgeleri](https://docs.microsoft.com/rest/api/datafactory/v1/data-factory-data-factory).

## <a name="data-factory-and-batch-together"></a>Data Factory ve Batch birlikte
Veri Fabrikası yerleşik etkinlikler içerir. Örneğin, kopyalama etkinliği, kaynak veri deposundan hedef veri deposuna veri kopyalama/taşıma için kullanılır. Hive etkinliği, Azure üzerinde Hadoop kümeleri (HDInsight) kullanarak verileri işlemek için kullanılır. Desteklenen dönüştürme etkinliklerinin listesi için bkz. [veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md).

Özel .NET etkinlikleri taşımak ya da kendi mantığınızı ile verileri işlemek için de oluşturabilirsiniz. Bu etkinlikler, bir Batch havuzu vm'leri veya bir HDInsight kümesi üzerinde çalıştırabilirsiniz. Batch kullandığınızda, havuz için otomatik ölçeklendirme yapılandırabilirsiniz (ekleme veya iş yüküne göre Vm'leri kaldırma) sağladığınız bir formüle göre.     

## <a name="architecture-of-a-sample-solution"></a>Örnek bir çözüm mimarisi
  Bu makalede açıklanan mimarisi için basit bir çözümdür. Finansal Hizmetler, görüntü işleme ve işleme ve genetik analiz modelleme risk gibi karmaşık senaryolar için de geçerlidir.

Data Factory veri taşıma ve işleme nasıl düzenler diyagramda gösterilmektedir. Ayrıca, toplu verileri paralel bir şekilde nasıl işleyeceğini gösterir. Karşıdan yükleyip diyagramı (11 x 17 inç veya A3 boyutu) bir kolayca başvurmak için yazdırın. Böylece, yazdırabilirsiniz diyagram erişmek için bkz [Batch ve Data Factory kullanarak HPC ve veri düzenleme](https://go.microsoft.com/fwlink/?LinkId=717686).

[![Büyük ölçekli veri işleme diyagramı](./media/data-factory-data-processing-using-batch/image1.png)](https://go.microsoft.com/fwlink/?LinkId=717686)

Aşağıdaki listede işleminin temel adımları sağlar. Çözüm, kod ve uçtan uca çözümü derlemek için açıklamaları içerir.

* **Batch işlem düğümleri (VM) bir havuzla yapılandırın.** Düğüm sayısı ve her bir düğümün boyutu belirtebilirsiniz.

* **Data Factory örneği oluşturma** blob depolama, toplu işlem hizmeti, girdi/çıktı verilerin ve bir iş akışı/işlem hattı ile taşıma ve veri dönüştürme etkinlikleri temsil eden varlıklar ile yapılandırılmış.

* **Data Factory işlem hattında özel bir .NET etkinliği oluşturun.** Batch havuzunda çalışan, kullanıcı kodu etkinliğidir.

* **Azure Depolama'daki blobları olarak yüksek miktarda giriş verisi Store.** Veriler (genellikle zamanına göre) mantıksal dilimlere bölünür.

* **Veri fabrikası, paralel olarak işlenir veri kopyalar** ikincil konumda.

* **Veri fabrikası, Batch tarafından ayrılmamış bir havuz kullanarak özel bir etkinlik çalışır.** Data Factory etkinlikler aynı anda çalıştırabilirsiniz. Her etkinlik bir veri dilimi işler. Sonuçları depolama alanında depolanır.

* **Veri Fabrikası son sonuçları üçüncü bir konuma taşır** aracılığıyla uygulama dağıtımı veya diğer araçları tarafından daha fazla işleme için.

## <a name="implementation-of-the-sample-solution"></a>Örnek çözüm uygulaması
Örnek çözüm, kasıtlı olarak basit bir işlemdir. Data Factory ve Batch işlem veri kümelerine arada kullanımını göstermek için tasarlanmıştır. Çözüm zaman serileri düzenlenir giriş dosyaları "Microsoft" arama terimini oluşum sayısını sayar. Ardından, Çıkış dosyalarını sayıyı çıkarır.

**Süre:** Azure Data Factory ve Batch temelleri biliyor ve aşağıdaki önkoşulları tamamladınız, bu çözüm tamamlamak için bir ila iki saat alır.

### <a name="prerequisites"></a>Önkoşullar
#### <a name="azure-subscription"></a>Azure aboneliği
Azure aboneliğiniz yoksa, ücretsiz bir deneme hesabı hızlıca oluşturabilirsiniz. Daha fazla bilgi için [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).

#### <a name="azure-storage-account"></a>Azure depolama hesabı
Bu öğreticide verileri depolamak için bir depolama hesabı kullanın. Bir depolama hesabına sahip değilseniz, bkz. [depolama hesabı oluşturma](../../storage/common/storage-quickstart-create-account.md). Örnek çözüm, blob depolama kullanır.

#### <a name="azure-batch-account"></a>Azure Batch hesabı
Kullanarak bir Batch hesabı oluşturma [Azure portalında](https://portal.azure.com/). Daha fazla bilgi için [oluşturun ve bir Batch hesabı yönetme](../../batch/batch-account-create-portal.md). Batch hesabı adı ve hesap anahtarını not edin. Ayrıca [yeni AzBatchAccount](https://docs.microsoft.com/powershell/module/az.batch/new-azbatchaccount) bir Batch hesabı oluşturmak için cmdlet'i. Bu cmdlet'in nasıl kullanılacağı hakkında yönergeler için bkz [Batch PowerShell cmdlet'leri ile başlama](../../batch/batch-powershell-cmdlets-get-started.md).

Örnek çözüm (üzerinden dolaylı olarak bir veri fabrikası işlem hattı) Batch havuzunda işlem düğümleri (sanal makineleri yönetilen koleksiyonu) paralel bir şekilde verileri işlemek için kullanır.

#### <a name="azure-batch-pool-of-virtual-machines"></a>Azure Batch havuzu sanal makineler
En az iki işlem düğümleri ile bir Batch havuzu oluşturun.

1. İçinde [Azure portalında](https://portal.azure.com)seçin **Gözat** seçin ve soldaki menüden **Batch hesapları**.

1. Batch hesabınızı açmak için seçin **Batch hesabı** dikey penceresi.

1. Seçin **havuzları** Döşe.

1. Üzerinde **havuzları** dikey penceresinde **Ekle** havuzu eklemek için araç çubuğunda.

   a. Havuz için bir kimlik girin (**Havuz kimliği**). Havuzu Kimliğine dikkat edin. Veri Fabrikası çözümü oluşturduğunuzda gerekir.

   b. Belirtin **Windows Server 2012 R2** için **işletim sistemi ailesi** ayarı.

   c. Seçin bir **fiyatlandırma katmanında düğüm**.

   d. Girin **2** değeri olarak **hedef adanmış** ayarı.

   e. Girin **2** değeri olarak **düğüm başına en fazla görev** ayarı.

   f. Seçin **Tamam** havuzu oluşturun.

#### <a name="azure-storage-explorer"></a>Azure Depolama Gezgini
Kullandığınız [Azure Depolama Gezgini 6](https://azurestorageexplorer.codeplex.com/) veya [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) (ürünü olan yazılımından) incelemek ve veri depolama projelerinizde değiştirmek için. Ayrıca inceleyin ve veriyi bulutta barındırılan uygulamalarınızı günlüklerinde değiştirin.

1. Adlı bir kapsayıcı oluşturun **mycontainer** özel erişim (anonim erişim yok).

1. CloudXplorer kullanırsanız, klasörler ve alt klasörler ile aşağıdaki yapısını oluşturun:

   ![Klasör ve alt klasör yapısı](./media/data-factory-data-processing-using-batch/image3.png)

   `Inputfolder` ve `outputfolder` en üst düzey klasörlerde bulunan `mycontainer`. `inputfolder` Tarih-zaman damgaları (YYYY-MM-DD-ss) ile alt klasörü bulunur.

   Sonraki adımda Depolama Gezgini'ni kullanıyorsanız, aşağıdaki adları ile dosyaları karşıya yükleme: `inputfolder/2015-11-16-00/file.txt`, `inputfolder/2015-11-16-01/file.txt`ve benzeri. Bu adım, otomatik olarak klasörleri oluşturur.

1. Bir metin dosyası oluşturun **dosya.txt** makinenizde anahtar sözcüğü olan içeriği **Microsoft**. "Sınama özel etkinlik Microsoft test özel etkinlik Microsoft." örneğidir

1. Dosyayı blob depolama aşağıdaki giriş klasörlerinde yükleyin:

   ![Giriş klasörleri](./media/data-factory-data-processing-using-batch/image4.png)

   Depolama Gezgini'ni kullanıyorsanız, karşıya yükleme **dosya.txt** dosyasını **mycontainer**. Seçin **kopyalama** Kabarcık bir kopyasını oluşturmak için araç çubuğunda. İçinde **kopya blob'u** iletişim kutusunda, değişiklik **hedef blob adı** için `inputfolder/2015-11-16-00/file.txt`. Oluşturmak için bu adımı yineleyin `inputfolder/2015-11-16-01/file.txt`, `inputfolder/2015-11-16-02/file.txt`, `inputfolder/2015-11-16-03/file.txt`, `inputfolder/2015-11-16-04/file.txt`ve benzeri. Bu eylem, klasörleri otomatik olarak oluşturur.

1. Adlı başka bir kapsayıcı oluşturun `customactivitycontainer`. Özel etkinliğin zip dosyası bu kapsayıcısına yükleyin.

#### <a name="visual-studio"></a>Visual Studio
Data factory çözümünde kullanılacak özel toplu etkinlik oluşturmak için Visual Studio 2012 veya sonraki sürümünü yükleyin.

### <a name="high-level-steps-to-create-the-solution"></a>Çözümü oluşturmak için üst düzey adımları
1. Veri işleme mantığı içeren özel bir etkinlik oluşturursunuz.

1. Özel Etkinlik kullanan bir veri fabrikası oluşturun.

### <a name="create-the-custom-activity"></a>Özel etkinlik oluşturma
Data factory özel etkinliği, bu örnek çözümde kalbidir. Örnek çözüm, Batch özel etkinliği çalıştırmak için kullanır. Özel etkinlikler geliştirmeyi ve bunları veri fabrikası ardışık kullanma hakkında daha fazla bilgi için bkz. [bir data factory işlem hattında özel etkinlikler kullanma](data-factory-use-custom-activities.md).

Kullanabileceğiniz bir .NET özel etkinliği bir data factory işlem hattı oluşturmak için Idotnetactivity arabiriminin uygulayan bir sınıf ile bir .NET sınıf kitaplığı projesi oluşturun. Bu arabirim, yalnızca bir yöntemi vardır: Yürütün. Metodun imzası şu şekildedir:

```csharp
public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices,
            IEnumerable<Dataset> datasets,
            Activity activity,
            IActivityLogger logger)
```

Yöntemi anlamanız gereken birkaç önemli bileşenden oluşur:

* Yöntemi dört parametre alır:

  * **linkedServices**. Bu parametre, giriş/çıkış veri kaynakları (örneğin, blob depolama) veri fabrikasına bağlarsınız bağlı hizmetler, numaralandırılabilir bir listesidir. Bu örnekte, giriş ve çıkış için kullanılan Azure depolama türünde yalnızca bir bağlantılı hizmet yok.
  * **veri kümeleri**. Bu parametre veri kümeleri sıralanabilir bir listesi verilmiştir. Bu parametre, girdi ve çıktı veri kümeleri tarafından tanımlı şemalar ve konumları almak için kullanabilirsiniz.
  * **Etkinlik**. Bu parametre, geçerli işlem varlığı temsil eder. Bu durumda, bir toplu işlem hizmetidir.
  * **Günlükçü**. Günlükçü işlem hattının "Kullanıcı" günlük olarak, bu surface hata ayıklama yorum yazmak için kullanabilirsiniz.
* Bu yöntem, özel etkinlikler gelecekte zincir için kullanılan bir sözlüğü döndürür. Bu özellik henüz uygulanmadı, bu nedenle yalnızca boş bir sözlük yöntemi döndürür.

#### <a name="procedure-create-the-custom-activity"></a>Yordam: Özel etkinlik oluşturma
1. Visual Studio'da .NET sınıf kitaplığı projesi oluşturun.

   a. Visual Studio 2012/2013/2015'i başlatın.

   b. **Dosya** > **Yeni** > **Proje**’yi seçin.

   c. Genişletin **şablonları**seçip **Visual C\#**. Bu kılavuzda, kullandığınız C\#, ancak özel etkinlik geliştirmek için dilediğiniz .NET dilini kullanabilirsiniz.

   d. Seçin **sınıf kitaplığı** sağ taraftaki proje türleri listesinden.

   e. Girin **MyDotNetActivity** için **adı**.

   f. Seçin **C:\\ADF** için **konumu**. Klasör Oluştur **ADF** yoksa.

   g. Projeyi oluşturmak için **Tamam**'ı seçin.

1. Seçin **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.

1. Paket Yöneticisi Konsolu'nda Microsoft.Azure.Management.DataFactories içeri aktarmak için aşağıdaki komutu yürütün:

    ```powershell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
1. İçeri aktarma **Azure depolama** NuGet paketini projeye. Bu örnekte, Blob Depolama API'si kullandığından bu paketi gerekir:

    ```powershell
    Install-Package Az.Storage
    ```
1. Aşağıdakileri ekleyin projesinde kaynak dosyasına yönergeleri kullanarak:

    ```csharp
    using System.IO;
    using System.Globalization;
    using System.Diagnostics;
    using System.Linq;
    
    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Runtime;
    
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```
1. Ad alanına adını değiştirmek **MyDotNetActivityNS**.

    ```csharp
    namespace MyDotNetActivityNS
    ```
1. İçin sınıfın adını değiştirmek **MyDotNetActivity**ve türetmeniz **Idotnetactivity** arabirim gösterildiği gibi:

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
1. Uygulama (Ekle) **yürütme** yöntemi **Idotnetactivity** arabirimini **MyDotNetActivity** sınıfı. Aşağıdaki örnek kod yönteme kopyalayın. Bu yöntemde kullanılan mantıksal bir açıklaması için bkz: [yöntemin](#execute-method) bölümü.

    ```csharp
    /// <summary>
    /// The Execute method is the only method of IDotNetActivity interface you must implement.
    /// In this sample, the method invokes the Calculate method to perform the core logic.  
    /// </summary>
    public IDictionary<string, string> Execute(
       IEnumerable<LinkedService> linkedServices,
       IEnumerable<Dataset> datasets,
       Activity activity,
       IActivityLogger logger)
    {
    
       // Declare types for the input and output data stores.
       AzureStorageLinkedService inputLinkedService;
    
       Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
       foreach (LinkedService ls in linkedServices)
           logger.Write("linkedService.Name {0}", ls.Name);
    
       // Use the First method instead of Single because we are using the same
       // Azure Storage linked service for input and output.
       inputLinkedService = linkedServices.First(
           linkedService =>
           linkedService.Name ==
           inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
           as AzureStorageLinkedService;
    
       string connectionString = inputLinkedService.ConnectionString; // To create an input storage client.
       string folderPath = GetFolderPath(inputDataset);
       string output = string.Empty; // for use later.
    
       // Create the storage client for input. Pass the connection string.
       CloudStorageAccount inputStorageAccount = CloudStorageAccount.Parse(connectionString);
       CloudBlobClient inputClient = inputStorageAccount.CreateCloudBlobClient();
    
       // Initialize the continuation token before using it in the do-while loop.
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
    
           // The Calculate method returns the number of occurrences of
           // the search term "Microsoft" in each blob associated
           // with the data slice.
           //
           // The definition of the method is shown in the next step.
           output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
       } while (continuationToken != null);
    
       // Get the output dataset by using the name of the dataset matched to a name in the Activity output collection.
       Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    
       folderPath = GetFolderPath(outputDataset);
    
       logger.Write("Writing blob to the folder: {0}", folderPath);
    
       // Create a storage object for the output blob.
       CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
       // Write the name of the file.
       Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
       logger.Write("output blob URI: {0}", outputBlobUri.ToString());
       // Create a blob and upload the output text.
       CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
       logger.Write("Writing {0} to the output blob", output);
       outputBlob.UploadText(output);
    
       // The dictionary can be used to chain custom activities together in the future.
       // This feature is not implemented yet, so just return an empty dictionary.
       return new Dictionary<string, string>();
    }
    ```
1. Aşağıdaki yardımcı yöntemler sınıfına ekleyin. Bu yöntemleri tarafından çağırılan **yürütme** yöntemi. En önemli **Calculate** yöntemi her blob yinelenir kodu yalıtır.

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
    
       AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
       if (blobDataset == null)
       {
           return null;
       }
    
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
    
       AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
       if (blobDataset == null)
       {
           return null;
       }
    
       return blobDataset.FileName;
    }
    
    /// <summary>
    /// Iterates through each blob (file) in the folder, counts the number of instances of the search term in the file,
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
    Veri kümesini işaret klasöre yolunu GetFolderPath yöntemi döndürür ve GetFileName yöntemi blob/dataset işaret eden dosya adını döndürür.

    ```csharp

    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
    ```

    Calculate yöntemi giriş dosyaları (klasördeki blobları) "Microsoft" anahtar sözcüğü örnek sayısını hesaplar. Arama terimi, kodda sabit kodlanmış "Microsoft" ise.

1. Projeyi derle. Seçin **derleme** menüsünü ve ardından **Çözümü Derle**.

1. Windows Gezgini'ni başlatın ve Git **bin\\hata ayıklama** veya **bin\\yayın** klasör. Klasör Seçimi yapı türüne bağlıdır.

1. ZIP dosyası oluşturma **MyDotNetActivity.zip** tüm ikili dosyaları içeren  **\\bin\\hata ayıklama** klasör. MyDotNetActivity eklemek isteyebilirsiniz. **pdb** kaynak kodunda bir hata oluşursa, sorunu neden olan satır numarası gibi ek ayrıntıları almak için dosya.

   ![Bin\Debug klasör listesi](./media/data-factory-data-processing-using-batch/image5.png)

1. Karşıya yükleme **MyDotNetActivity.zip** blob kapsayıcısına bir blob olarak `customactivitycontainer` blob depolamadaki StorageLinkedService ADFTutorialDataFactory kullandığı hizmetinde bağlı. Blob kapsayıcısı oluşturma `customactivitycontainer` zaten yoksa.

#### <a name="execute-method"></a>Execute yöntemi
Bu bölümde, yürütme yönteminde kod hakkında daha fazla ayrıntı sağlar.

1. Giriş koleksiyonu yineleme üyeleri bulunan [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) ad alanı. Blob toplulukta tekrarlama için kullanmak isteniyor **BlobContinuationToken** sınıfı. Esas olarak, do kullanmanız gerekir-while döngüsü döngüden çıkma mekanizması olarak belirtecine sahip. Daha fazla bilgi için [.NET kullanım Blob depolamadan](../../storage/blobs/storage-dotnet-how-to-use-blobs.md). Temel bir döngü burada gösterilmiştir:

    ```csharp
    // Initialize the continuation token.
    BlobContinuationToken continuationToken = null;
    do
    {
    // Get the list of input blobs from the input storage client object.
    BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
    
                         true,
                                   BlobListingDetails.Metadata,
                                   null,
                                   continuationToken,
                                   null,
                                   null);
    // Return a string derived from parsing each blob.
    
     output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
    } while (continuationToken != null);

    ```
   Daha fazla bilgi için belgelerine bakın [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) yöntemi.

1. BLOB'ları kümesi aracılığıyla mantıksal olarak çalışmak için kodu içinde do gider-while döngüsü. İçinde **yürütme** yöntemi, do-döngü BLOB listesini adlı bir yönteme geçirir **Calculate**. Yöntem adı bir dize değişkeni döndürür **çıkış** diğer bir deyişle kesimdeki tüm blobları aracılığıyla yinelenir sonucu.

   "Microsoft" BLOB geçirilen arama terimi oluşum sayısını döndürür **Calculate** yöntemi.

    ```csharp
    output += string.Format("{0} occurrences of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
    ```
1. Sonra **Calculate** yöntemi tamamlandığında, yeni bir blob için yazılmış olmalıdır. Her işlenen BLOB'ları kümesi için yeni bir blob sonuçlarıyla yazılabilir. Yeni bir bloba yazma için ilk çıkış veri kümesini bulun.

    ```csharp
    // Get the output dataset by using the name of the dataset matched to a name in the Activity output collection.
    Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    ```
1. Kod ayrıca yardımcı yöntemi çağırır **GetFolderPath** klasör yolu (depolama kapsayıcısı adı) alınamıyor.

    ```csharp
    folderPath = GetFolderPath(outputDataset);
    ```
   FolderPath adlı bir özelliğe sahip bir AzureBlobDataSet DataSet nesnesine GetFolderPath yöntemi uygular.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FolderPath;
    ```
1. Kod çağrıları **GetFileName** dosya adı (blob) almak için yöntemi. Kod, klasör yolunu almak için kullanılan önceki koda benzer.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FileName;
    ```
1. Dosyanın adı, bir URI nesnesinden oluşturarak yazılır. URI Oluşturucu kullanan **BlobEndpoint** kapsayıcı adı döndürülecek özellik. Çıktı blob URI'si oluşturmak için klasör yolu ve dosya adı eklenir.  

    ```csharp
    // Write the name of the file.
    Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    ```
1. Dosyanın adı yazıldıktan sonra çıkış dizesi yazabilirsiniz **Calculate** yöntemi yeni bir blob için:

    ```csharp
    // Create a blob and upload the output text.
    CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
    logger.Write("Writing {0} to the output blob", output);
    outputBlob.UploadText(output);
    ```

### <a name="create-the-data-factory"></a>Veri Fabrikası oluşturma
İçinde [özel etkinlik oluşturma](#create-the-custom-activity) bölüm, özel bir etkinlik oluşturulur ve ikili dosyaları zip dosyasıyla ve PDB dosyası için bir blob kapsayıcısını karşıya. Bu bölümde, özel etkinliği kullanan bir işlem hattıyla veri fabrikası oluşturun.

Özel etkinliğin giriş veri kümesi alanındaki giriş klasörüne bloblar (dosyalar) temsil eder (`mycontainer\\inputfolder`) blob depolama. Etkinliğin çıkış veri kümesi çıktı klasöründe çıktı bloblarını temsil eder (`mycontainer\\outputfolder`) blob depolama.

Bir veya daha fazla giriş klasörler halinde bırakın:

```
mycontainer -\> inputfolder
    2015-11-16-00
    2015-11-16-01
    2015-11-16-02
    2015-11-16-03
    2015-11-16-04
```

Örneğin, bir dosya (dosya.txt) aşağıdaki içerikle her klasörleri bırakın:

```
test custom activity Microsoft test custom activity Microsoft
```

İki veya daha fazla dosya klasörü sahip olsa bile her giriş klasörü veri fabrikasında bir dilim karşılık gelir. Özel Etkinlik, her işlem hattı tarafından işlendiğinde, dilim için giriş klasöründeki tüm blobları gezinir.

Beş Çıkış dosyalarını aynı içeriğe sahip görürsünüz. Örneğin, bir 2015-11-16-00 klasöründeki dosya işlemesini çıkış dosyası aşağıdaki içeriğe sahip:

```
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
```

Aynı içeriğe sahip birden fazla dosyaları (dosya.txt, file2.txt, file3.txt) giriş klasörüne sürükleyip bırakabilir, çıktı dosyası'nda aşağıdaki içeriğe bakın. Her bir klasör (2015-11-16-00, vb.) birden fazla giriş dosyası klasörü sahip olsa da bu örnekte bir dilim karşılık gelir.

```csharp
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file2.txt.
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file3.txt.
```

Çıkış dosyası üç satırı artık, bir dilimin (2015-11-16-00) ile ilişkili klasöründe her giriş dosyası (blob) sahip.

Her etkinlik için bir görev oluşturulur. Bu örnekte, işlem hattında yalnızca bir etkinlik yok. Bir dilim işlem hattı tarafından işlendiğinde, özel etkinlik dilimi işlemesi için toplu olarak çalıştırır. Beş dilimler (her dilim birden çok BLOB veya dosya olabilir) olduğundan, Batch hizmetinde beş görev oluşturulur. Bir görev toplu olarak çalıştığında, çalıştıran özel bir etkinlik olduğundan.

Aşağıdaki örneklerde, ek ayrıntılar sağlar.

#### <a name="step-1-create-the-data-factory"></a>1. Adım: Veri Fabrikası oluşturma
1. İçin oturum açtıktan sonra [Azure portalında](https://portal.azure.com/), aşağıdaki adımları uygulayın:

   a. Seçin **yeni** sol menüsünde.

   b. Seçin **veri ve analiz** üzerinde **yeni** dikey penceresi.

   c. Seçin **Data Factory** üzerinde **veri analizi** dikey penceresi.

1. Üzerinde **yeni veri fabrikası** dikey penceresinde girin **CustomActivityFactory** adı. Veri fabrikasının adı genel olarak benzersiz olmalıdır. "Veri fabrikası adı CustomActivityFactory kullanılabilir değil" hata iletisini alırsanız veri fabrikasının adını değiştirin. Örneğin, yournameCustomActivityFactory kullanın ve veri fabrikasını yeniden oluşturun.

1. Seçin **kaynak grubu adı**, mevcut bir kaynak grubunu seçin ve bir kaynak grubu oluşturun.

1. Oluşturulacak veri fabrikasının istediğiniz bölgeye ve aboneliğe doğru olduğundan emin olun.

1. Seçin **Oluştur** üzerinde **yeni veri fabrikası** dikey penceresi.

1. Data factory, portal panosunda oluşturulur.

1. Data factory sorunsuz oluşturulduktan sonra gördüğünüz **veri fabrikası** sayfasında, data Factory içeriği gösterilir.

   ![Veri Fabrikası sayfası](./media/data-factory-data-processing-using-batch/image6.png)

#### <a name="step-2-create-linked-services"></a>2. Adım: Bağlı hizmetler oluşturma
Bağlı hizmetler veri depolarını veya işlem Hizmetleri data factory'ye. Bu adımda, depolama hesabınızın ve Batch hesabı veri fabrikanıza bağlarsınız.

#### <a name="create-an-azure-storage-linked-service"></a>Azure Depolama bağlı hizmeti oluşturma
1. Seçin **yazar ve dağıtma** kutucuğundan **veri fabrikası** dikey **CustomActivityFactory**. Data Factory Düzenleyicisi görünür.

1. Seçin **yeni veri deposu** komut çubuğunda ve **Azure depolama.** Bir depolama bağlı hizmeti Düzenleyicisi'nde görünür oluşturmak için kullandığınız JSON betiği.

   ![Yeni veri deposu](./media/data-factory-data-processing-using-batch/image7.png)

1. **Hesap adı** değerini depolama hesabınızın adıyla değiştirin. **Hesap anahtarı** değerini depolama hesabının erişim anahtarıyla değiştirin. Depolama erişim anahtarınızı alma hakkında bilgi için bkz: [görüntüleme, kopyalama ve yeniden oluşturma depolama erişim anahtarlarını](../../storage/common/storage-account-manage.md#access-keys).

1. Bağlı hizmeti dağıtmak için komut çubuğunda **Dağıt**’ı seçin.

   ![Dağıtma](./media/data-factory-data-processing-using-batch/image8.png)

#### <a name="create-an-azure-batch-linked-service"></a>Bir Azure Batch bağlı hizmeti oluşturma
Bu adımda, data factory özel etkinliği çalıştırmak için kullanılan Batch hesabınız için bağlı hizmet oluşturun.

1. Seçin **yeni işlem** komut çubuğunda ve **Azure Batch.** JSON betik bağlı hizmeti Düzenleyicisi'nde görünür bir toplu iş oluşturmak için kullanın.

1. JSON betik:

   a. Değiştirin **hesap adı** Batch hesabınızın adına sahip.

   b. Değiştirin **erişim anahtarı** Batch hesabının erişim anahtarıyla.

   c. Havuz için Kimliğini girin **poolName** özelliği. Bu özellik için havuz adı ya da havuz kimliğini belirtebilirsiniz

   d. Batch URI girmek için **batchUri** JSON özelliği.

      > [!IMPORTANT]
      > URL'den **Batch hesabı** dikey olan şu biçimde: \<accountname\>.\< Bölge\>. batch.azure.com. İçin **batchUri** JSON betik özelliğinde ihtiyacınız a88 "accountname." kaldırmak ** URL'den. `"batchUri": "https://eastus.batch.azure.com"` bunun bir örneğidir.
      >
      >

      ![Batch hesabı dikey penceresi](./media/data-factory-data-processing-using-batch/image9.png)

      İçin **poolName** özelliği, havuzun havuz adı yerine Kimliğini belirtebilirsiniz.

      > [!NOTE]
      > HDInsight için yaptığı gibi Data Factory hizmetinin toplu işlemi için bir isteğe bağlı seçeneğini desteklemiyor. Bir data factory yalnızca kendi Batch havuzu kullanabilirsiniz.
      >
      >
   
   e. Belirtin **StorageLinkedService** için **linkedServiceName** özelliği. Bu bağlı hizmeti önceki adımda oluşturduğunuz. Bu depolama alanı, dosyaları ve günlükleri için bir hazırlama alanı olarak kullanılır.

1. Bağlı hizmeti dağıtmak için komut çubuğunda **Dağıt**’ı seçin.

#### <a name="step-3-create-datasets"></a>3. Adım: Veri kümeleri oluşturma
Bu adımda, girdi ve çıktı verilerini temsil eden veri kümeleri oluşturun.

#### <a name="create-the-input-dataset"></a>Girdi veri kümesini oluşturma
1. Data Factory Düzenleyicisi'nde seçin **yeni veri kümesi** araç çubuğunda. Seçin **Azure Blob Depolama** aşağı açılan listeden.

1. Sağ bölmedeki JSON betiği aşağıdaki JSON parçacığıyla değiştirin:

    ```json
    {
       "name": "InputDataset",
       "properties": {
           "type": "AzureBlob",
           "linkedServiceName": "AzureStorageLinkedService",
           "typeProperties": {
               "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
               "format": {
                   "type": "TextFormat"
               },
               "partitionedBy": [
                   {
                       "name": "Year",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "yyyy"
                       }
                   },
                   {
                       "name": "Month",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "MM"
                       }
                   },
                   {
                       "name": "Day",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "dd"
                       }
                   },
                   {
                       "name": "Hour",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "HH"
                       }
                   }
               ]
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

    Başlangıç zamanı 2015 ile bu kılavuzda daha sonra işlem hattı oluşturma-11-2015 16T00:00:00Z ve son saat-11-16T05:00:00Z. Beş girdi/çıktı dilimleri olduklarından, verileri üretmek üzere planladı (arasında **00**: 00:00 -\> **05**: 00:00).

    **Sıklığı** ve **aralığı** giriş veri kümesi için ayarlanmış olan **saat** ve **1**, giriş dilimi saatlik kullanılabilir olduğunu anlamına gelir.

    Her dilim için başlangıç saatini tarafından temsil edilen **SliceStart** önceki JSON kod parçacığında sistem değişkeni. Her dilim için başlangıç zamanlarını aşağıda verilmiştir.

    | **Dilim** | **Başlangıç saati**          |
    |-----------|-------------------------|
    | 1         | 2015-11-16T**00**:00:00 |
    | 2         | 2015-11-16T**01**:00:00 |
    | 3         | 2015-11-16T**02**:00:00 |
    | 4         | 2015-11-16T**03**:00:00 |
    | 5         | 2015-11-16T**04**:00:00 |

    **FolderPath** dilim başlangıç zamanı yıl, ay, gün ve saat bölümünü kullanılarak hesaplanır (**SliceStart**). Bir giriş klasörü dilime nasıl eşlendiğini aşağıda verilmiştir.

    | **Dilim** | **Başlangıç saati**          | **Giriş klasörü**  |
    |-----------|-------------------------|-------------------|
    | 1         | 2015-11-16T**00**:00:00 | 2015-11-16-**00** |
    | 2         | 2015-11-16T**01**:00:00 | 2015-11-16-**01** |
    | 3         | 2015-11-16T**02**:00:00 | 2015-11-16-**02** |
    | 4         | 2015-11-16T**03**:00:00 | 2015-11-16-**03** |
    | 5         | 2015-11-16T**04**:00:00 | 2015-11-16-**04** |

1. Seçin **Dağıt** oluşturmak ve dağıtmak için araç çubuğunda **Inputdataset** tablo.

#### <a name="create-the-output-dataset"></a>Çıktı veri kümesini oluşturma
Bu adımda, başka bir veri kümesi türü AzureBlob, çıktı verilerini göstermek için oluşturabilir.

1. Data Factory Düzenleyicisi'nde seçin **yeni veri kümesi** araç çubuğunda. Seçin **Azure Blob Depolama** aşağı açılan listeden.

1. Sağ bölmedeki JSON betiği aşağıdaki JSON parçacığıyla değiştirin:

    ```json
    {
       "name": "OutputDataset",
       "properties": {
           "type": "AzureBlob",
           "linkedServiceName": "AzureStorageLinkedService",
           "typeProperties": {
               "fileName": "{slice}.txt",
               "folderPath": "mycontainer/outputfolder",
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

    Bir çıkış blob/dosyası, her giriş dilimi için oluşturulur. İşte bir çıktı dosyası için her bir dilimi nasıl adlandırılır. Bir çıkış klasöründe oluşturulan tüm çıktı dosyaları `mycontainer\\outputfolder`.

    | **Dilim** | **Başlangıç saati**          | **Çıkış dosyası**       |
    |-----------|-------------------------|-----------------------|
    | 1         | 2015-11-16T**00**:00:00 | 2015-11-16 -**00. txt** |
    | 2         | 2015-11-16T**01**:00:00 | 2015-11-16 -**01. txt** |
    | 3         | 2015-11-16T**02**:00:00 | 2015-11-16 -**02. txt** |
    | 4         | 2015-11-16T**03**:00:00 | 2015-11-16 -**03. txt** |
    | 5         | 2015-11-16T**04**:00:00 | 2015-11-16 -**04. txt** |

    Giriş bir klasördeki tüm dosyaları (örneğin, 2015-11-16-00) bir 2015-11-16-00 başlangıç saati dilimi parçası olduğunu unutmayın. Özel Etkinlik bu dilim işlendiğinde, her dosyanın tarar ve çıkış dosyasına arama terimi "Microsoft." oluşum sayısını içeren bir satır üretir 2015-11-16-00 klasöründe üç dosya varsa, çıktı dosyası 2015-11-16-00.txt içinde üç satır vardır.

1. Seçin **Dağıt** oluşturmak ve dağıtmak için araç çubuğunda **OutputDataset**.

#### <a name="step-4-create-and-run-the-pipeline-with-a-custom-activity"></a>4. Adım: Oluşturma ve özel bir etkinlik ile işlem hattı çalıştırma
Bu adımda, bir etkinlik, daha önce oluşturduğunuz özel etkinliği ile işlem hattı oluşturun.

> [!IMPORTANT]
> Henüz yüklediyseniz **dosya.txt** işlem hattı oluşturmadan önce blob kapsayıcısındaki klasörleri giriş için bunu yapın. **İsPaused** ayarlanırsa false JSON, işlem hattının işlem hattı için hemen çalışır böylece **Başlat** geçmişte tarihtir.
>
>

1. Data Factory Düzenleyicisi'nde seçin **yeni işlem hattı** komut çubuğunda. Komut görmüyorsanız görüntülemek için üç nokta simgesini seçin.

1. Sağ bölmedeki JSON betiği aşağıdaki JSON parçacığıyla değiştirin:

    ```json
    {
       "name": "PipelineCustom",
       "properties": {
           "description": "Use custom activity",
           "activities": [
               {
                   "type": "DotNetActivity",
                   "typeProperties": {
                       "assemblyName": "MyDotNetActivity.dll",
                       "entryPoint": "MyDotNetActivityNS.MyDotNetActivity",
                       "packageLinkedService": "AzureStorageLinkedService",
                       "packageFile": "customactivitycontainer/MyDotNetActivity.zip"
                   },
                   "inputs": [
                       {
                           "name": "InputDataset"
                       }
                   ],
                   "outputs": [
                       {
                           "name": "OutputDataset"
                       }
                   ],
                   "policy": {
                       "timeout": "00:30:00",
                       "concurrency": 5,
                       "retry": 3
                   },
                   "scheduler": {
                       "frequency": "Hour",
                       "interval": 1
                   },
                   "name": "MyDotNetActivity",
                   "linkedServiceName": "AzureBatchLinkedService"
               }
           ],
           "start": "2015-11-16T00:00:00Z",
           "end": "2015-11-16T05:00:00Z",
           "isPaused": false
      }
    }
    ```
   Aşağıdaki noktalara dikkat edin:

   * İşlem hattında yalnızca bir etkinlik olduğundan ve türü **DotNetActivity**.
   * **AssemblyName** DLL adına ayarlı **MyDotNetActivity.dll**.
   * **EntryPoint** ayarlanır **MyDotNetActivityNS.MyDotNetActivity**. Temel olan \<ad alanı\>.\< ClassName\> kodunuzda.
   * **PackageLinkedService** ayarlanır **StorageLinkedService**, özel etkinliğin zip dosyasını içeren blob depolama alanına işaret eder. Girdi/çıktı dosyalarını ve özel etkinliğin zip dosyası için farklı depolama hesapları kullanıyorsanız, başka bir depolama bağlı hizmeti oluşturmanız gerekir. Bu makalede, aynı depolama hesabını kullandığınız varsayılır.
   * **PackageFile** ayarlanır **customactivitycontainer/MyDotNetActivity.zip**. Şu biçimdedir \<containerforthezip\>/\<nameofthezip.zip\>.
   * Özel Etkinlik alır **Inputdataset** giriş olarak ve **OutputDataset** çıktı olarak.
   * **LinkedServiceName** özel etkinlik özelliğini işaret **AzureBatchLinkedService**, Data Factory, toplu olarak çalıştırmak için özel etkinlik gereken söyler.
   * **Eşzamanlılık** ayar önemlidir. İki veya daha fazla işlem düğümleri Batch havuzundaki bile 1 ' dir, varsayılan değeri kullanırsanız dilim işlenir birbiri ardına. Bu nedenle, batch'in yeteneği paralel yararlanarak değildir. Ayarlarsanız **eşzamanlılık** daha yüksek bir değer 2 varsayalım, bu iki dilimler anlamına gelir (iki görevleri karşılık gelir) aynı anda işlenebilir. Bu durumda, hem Batch havuzu Vm'leri kullanılır. Eşzamanlılık özelliği uygun şekilde ayarlayın.
   * Yalnızca bir görev (dilim), varsayılan olarak herhangi bir noktada bir VM'de çalıştırılır. Varsayılan olarak, **VM başına en fazla görev** bir Batch havuzu için 1 olarak ayarlanır. Önkoşulların bir parçası, bu özelliği 2 olarak ayarlanmış bir havuz oluşturmuş. Bu nedenle, iki veri fabrikası dilimleri bir VM'de aynı anda çalıştırabilirsiniz.
     - **İsPaused** özelliği varsayılan olarak false olarak ayarlanır. Geçmiş dilimler başlatmak için işlem hattı hemen bu örnekte çalışır. Bu özelliği ayarlamak **true** bunu döner ve işlem hattı duraklatmak için **false** yeniden başlatmak için.
     -   **Başlat** ve **son** süreleri, beş saat artırırız. Dilimleri, işlem hattı tarafından beş dilimlerinin şekilde saatlik olarak oluşturulur.

1. İşlem hattını dağıtmak için komut çubuğundan **Dağıt**’ı seçin.

#### <a name="step-5-test-the-pipeline"></a>5. Adım: İşlem hattını test etme
Bu adımda, işlem hattının giriş klasörler halinde dosyaları bırakarak sınayın. Her giriş klasörü için bir dosya ile işlem hattı test ederek başlayın.

1. Üzerinde **veri fabrikası** seçin Azure portalındaki dikey penceresinde **diyagram**.

   ![Diyagram](./media/data-factory-data-processing-using-batch/image10.png)

1. İçinde **diyagram** görüntülemek için giriş veri kümesi çift **Inputdataset**.

   ![Inputdataset](./media/data-factory-data-processing-using-batch/image11.png)

1. **Inputdataset** tüm beş dilimler hazır dikey penceresi görünür. Bildirim **DİLİM başlangıç saati** ve **DİLİM bitiş zamanı** her dilim için.

   ![Giriş dilimi başlangıç ve bitiş saatleri](./media/data-factory-data-processing-using-batch/image12.png)

1. İçinde **diyagram** görüntülenecek **OutputDataset**.

1. Beş çıktı dilimleri görünür **hazır** bunlar oluşturulamadığı varsa durum.

   ![Çıktı dilimi başlangıç ve bitiş saatleri](./media/data-factory-data-processing-using-batch/image13.png)

1. Dilimler ilişkilendirilmiş görevleri görmek için portalı kullanma ve her bir dilimi çalıştırıldığı yer hangi VM bakın. Daha fazla bilgi için [Data Factory ve Batch tümleştirme](#data-factory-and-batch-integration) bölümü.

1. Çıkış dosyalarının altında görünür `mycontainer` içinde `outputfolder` blob depolama alanınızda.

   ![Çıkış dosyaları depolama](./media/data-factory-data-processing-using-batch/image15.png)

   Beş Çıkış dosyalarını listelenir, her bir dilimi giriş. Her çıkış dosyalarının içeriğini aşağıdaki çıktıya benzer vardır:

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
    ```
   Aşağıdaki diyagramda, data factory dilimleri batch'teki görevleri nasıl eşleneceğine gösterilmektedir. Bu örnekte, yalnızca bir çalışma bir dilim vardır.

   ![Dilim eşleme diyagramı](./media/data-factory-data-processing-using-batch/image16.png)

1. Bir klasördeki birden çok dosya ile hemen deneyin. Dosyaları oluşturmak **file2.txt**, **file3.txt**, **file4.txt**, ve **file5.txt** başvurduğu ' % s'klasöründe olduğugibiaynıiçeriğesahip**2015-11-06-01**.

1. Çıktı klasöründe çıktı dosyasının Sil **2015-11-16-01.txt**.

1. Üzerinde **OutputDataset** dikey penceresinde dilimle sağ **DİLİM başlangıç saati** kümesine **16/11/2015 01:00:00 AM'den**. Seçin **çalıştırma** dilimi yeniden çalıştırma/yeniden işleme için. Dilim beş dosya yerine bir dosya artık sahiptir.

    ![Çalıştırın](./media/data-factory-data-processing-using-batch/image17.png)

1. Dilim çalıştırır ve durumunun sonra **hazır**, bu dilim için çıkış dosyasının içeriğini doğrulayın (**2015-11-16-01.txt**). Çıkış dosyası altında görünür `mycontainer` içinde `outputfolder` blob depolama alanınızda. Dilimin her dosya için bir satır olması gerekir.

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file2.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file3.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file4.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file5.txt.
    ```

> [!NOTE]
> Çıkış dosyası 2015-11-16-01.txt ile beş giriş dosyaları denediniz önce silerseniz yaramadı önceki dilimin çalıştırılma bir satır ve geçerli dilimin çalıştırılma beş satır bakın. Zaten varsa, varsayılan olarak, içeriğin çıktı dosyasına eklenir.
>
>

#### <a name="data-factory-and-batch-integration"></a>Data Factory ve Batch tümleştirme
Data Factory hizmetinin bir iş adıyla Batch'de oluşturur `adf-poolname:job-xxx`.

![Toplu işleri](media/data-factory-data-processing-using-batch/data-factory-batch-jobs.png)

Bir dilim her etkinlik çalıştırma için iş bir görev oluşturulur. 10 dilimleri işlenecek hazırsanız 10 görevleri işi oluşturulur. Birden çok işlem düğümleri havuzu varsa, paralel olarak çalışan birden fazla dilim olabilir. İşlem düğümü başına görev sayısı birden fazla ayarlanırsa, birden fazla dilim aynı işlem üzerinde çalıştırabilirsiniz.

Bu örnekte, beş dilimi vardır, Batch hizmetinde beş görevleri için. İle **eşzamanlılık** kümesine **5** JSON data factory'de işlem hattının ve **VM başına en fazla görev** kümesine **2** ileBatchhavuzundaki**2** VM'ler, görevleri hızlı çalışır. (Görevler için başlangıç ve bitiş zamanlarını kontrol edin.)

Toplu işlem ve dilimleri ile ilişkili görevleri görüntülemek için portal'ı kullanın ve her bir dilimi çalıştırıldığı yer hangi VM bakın.

![Toplu iş görevleri](media/data-factory-data-processing-using-batch/data-factory-batch-job-tasks.png)

### <a name="debug-the-pipeline"></a>İşlem hattında hata ayıklama
Hata ayıklama birkaç temel teknikten oluşur.

1. Giriş dilimi ayarlanmadıysa **hazır**, giriş klasörü yapısının doğru olduğundan ve bu dosya.txt giriş klasörlerinde var olduğunu doğrulayın.

   ![Giriş klasörü yapısının](./media/data-factory-data-processing-using-batch/image3.png)

1. İçinde **yürütme** yöntemi özel etkinliği, kullanım **Iactivitylogger** sorunlarını gidermenize yardımcı olacak bilgileri günlüğe kaydetmek için nesne. Günlüğe kaydedilen iletiler kullanıcı görünmesini\_0. günlük dosyası.

   Üzerinde **OutputDataset** dikey penceresinde görmek için dilimi seçin **veri dilimi** dikey penceresinde, dilim için. Altında **etkinlik çalıştırmalarını**, dilimi için çalıştırılan bir etkinlik görürsünüz. Seçerseniz **çalıştırma** komut çubuğunda, aynı dilimi için çalıştırılan başka bir etkinlik başlayabilirsiniz.

   Etkinlik çalıştırma seçtiğinizde, gördüğünüz **etkinlik çalıştırması ayrıntıları** günlük dosyalarının listesini içeren dikey pencere. Kullanıcının günlüğe kaydedilen iletilere bakın\_0. günlük dosyası. Bir hata oluştuğunda, yeniden deneme sayısı 3 işlem hattı/etkinlik JSON olarak ayarlandığından üç Etkinlik çalıştırmalarını görürsünüz. Etkinlik Çalıştır'ı seçin, hatayı gidermek için gözden geçirebileceğiniz günlük dosyalarına bakın.

   ![OutputDataset ve veri dilimi dikey pencereleri](./media/data-factory-data-processing-using-batch/image18.png)

   Günlük dosyalarının listesinde seçin **kullanıcı-0.log**. Sağ bölmede, kullanarak sonuçları **IActivityLogger.Write** yöntemi görünür.

   ![Etkinlik çalışma ayrıntıları dikey penceresi](./media/data-factory-data-processing-using-batch/image19.png)

   Tüm sistem hata iletileri ve özel durumlar için sistemi-0.log denetleyin.

    ```
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Loading assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Creating an instance of MyDotNetActivityNS.MyDotNetActivity from assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Executing Module
    
    Trace\_T\_D\_12/6/2015 1:43:38 AM\_T\_D\_\_T\_D\_Information\_T\_D\_0\_T\_D\_Activity e3817da0-d843-4c5c-85c6-40ba7424dce2 finished successfully
    ```
1. Dahil **PDB** dosya zip dosyasında bir hata oluştuğunda hata ayrıntılarını çağrı yığını gibi bilgileri edinecek olmanızdır.

1. Özel etkinliğin zip dosyasındaki tüm dosyalar alt klasörsüz ile en üst düzeyinde olması gerekir.

   ![Özel etkinliğin zip dosya listesi](./media/data-factory-data-processing-using-batch/image20.png)

1. Emin **assemblyName** (MyDotNetActivity.dll), **entryPoint** (MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer / MyDotNetActivity.zip) ve **packageLinkedService** (zip dosyasını içeren blob depolamasına işaret etmelidir) doğru değerlere ayarlanır.

1. Bir hata ve dilimi yeniden işlemek istiyorsanız fixed ise dilime sağ tıklayın **OutputDataset** dikey penceresinde ve select **çalıştırma**.

   ![OutputDataset dikey Çalıştır seçeneği](./media/data-factory-data-processing-using-batch/image21.png)

   > [!NOTE]
   > Blob depolamanızda adlı kapsayıcıdır `adfjobs`. Bu kapsayıcı otomatik olarak silinmez, ancak, çözümü test etme işlemini tamamladıktan sonra güvenli bir şekilde silebilirsiniz. Benzer şekilde, data factory çözümü adlı bir Batch işi oluşturur `adf-\<pool ID/name\>:job-0000000001`. İsterseniz, çözümü test ettikten sonra bu iş silebilirsiniz.
   >
   >
1. Özel Etkinlik kullanmaz **app.config** dosyasını. Bu nedenle, kodunuz yapılandırma dosyasından herhangi bir bağlantı dizesini okuyorsa, çalışma zamanında çalışmıyor. Herhangi bir gizli anahtar Azure anahtar Kasası'nda tutmak için Batch hizmetini kullanırken en iyi yöntem olacaktır. Daha sonra anahtar kasasına korumak ve Batch havuzu sertifikayı dağıtmak için bir sertifika tabanlı hizmet sorumlusu kullanın. .NET özel etkinliği, çalışma zamanında anahtar kasasından gizli dizileri erişebilirsiniz. Bu genel bir çözüm herhangi bir gizli dizi, yalnızca bir bağlantı dizesi türüne ölçeklendirebilirsiniz.

    Daha kolay bir geçici çözüm yoktur, ancak en iyi yöntem değildir. SQL veritabanı bağlı hizmeti ile bağlantı dizesi ayarlarını oluşturabilirsiniz. Ardından bağlantılı hizmetin kullandığı veri kümesi oluşturma ve veri kümesi olarak bir işlevsiz bir giriş veri kümesi için özel bir .NET etkinliği zincirleyebilir, yani. Özel Etkinlik kod bağlantılı hizmetin bağlantı dizesinde erişebilirsiniz. Çalışma zamanında düzgün çalışması gerekir.  

#### <a name="extend-the-sample"></a>Örneği genişletme
Data Factory ve Batch özellikleri hakkında daha fazla bilgi edinmek için bu örneği genişletebilirsiniz. Örneğin, farklı bir zaman aralığı dilimleri işlemek için aşağıdaki adımları uygulayın:

1. Aşağıdaki alt klasörlerini ekleyin `inputfolder`: 2015-11-16-05, 2015-11-16-06, 201-11-16-07, 2011-11-16-08 ve 2015-11-16-09. Giriş dosyaları bu klasörlerdeki yerleştirin. Ardışık düzen tarafından bitiş zamanını değiştirin `2015-11-16T05:00:00Z` için `2015-11-16T10:00:00Z`. İçinde **diyagram** görüntülemek için çift **Inputdataset** ve girdi dilimi hazır olduğundan emin olun. Çift **OutputDataset** çıktı dilimleri durumunu görmek için. İçinde iseler **hazır** durum, çıkış dosyaları için çıkış klasörünü denetleyin.

1. Artırma veya azaltma **eşzamanlılık** çözümünüzü, özellikle toplu olarak oluşan işleme performansını nasıl etkilediği anlamak için ayarı. Daha fazla bilgi için **eşzamanlılık** ayar bkz. "4. adım: Oluşturun ve özel bir etkinlik ile işlem hattını çalıştırın."

1. Daha yüksek/düşük ile havuz oluşturma **VM başına en fazla görev**. Oluşturduğunuz yeni havuz kullanmak için veri fabrikası çözümü bağlı Batch hizmetinde güncelleştirin. Daha fazla bilgi için **VM başına en fazla görev** ayar bkz. "4. adım: Oluşturun ve özel bir etkinlik ile işlem hattını çalıştırın."

1. İle bir Batch havuzu oluşturma **otomatik ölçeklendirme** özelliği. Batch havuzundaki işlem düğümlerini otomatik ölçeklendirme uygulamanız tarafından kullanılan güç işleme yerleştirmenin dinamik ayarına olur. 

    Burada örnek formülü aşağıdaki davranışı elde eder. Havuz başlangıçta oluşturulduğunda, bir VM ile başlar. $PendingTasks ölçüm çalışır görevlerin sayısını tanımlar ve etkin (kuyruğa alınmış) belirtir. Formül, Son 180 saniye cinsinden ortalama sayısı Bekleyen Görevler bulur ve TargetDedicated uygun şekilde ayarlar. TargetDedicated hiçbir zaman 25 VM'lerin ötesine geçen gider sağlar. Yeni görevler gönderilen gibi havuzun otomatik olarak büyür. Görevler tamamlandı olarak ücretsiz tek tek sanal makineleri olur ve bu sanal makineler için otomatik ölçeklendirme küçültür. İhtiyaçlarınıza startingNumberOfVMs ve maxNumberofVMs ayarlayabilirsiniz.
 
    Otomatik ölçeklendirme formülü:

    ``` 
    startingNumberOfVMs = 1;
    maxNumberofVMs = 25;
    pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
    pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
    $TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
    ```

   Daha fazla bilgi için [işlem düğümleri Batch havuzunda otomatik olarak](../../batch/batch-automatic-scaling.md).

   Varsayılan havuz kullanıyorsa, [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), Batch hizmeti, sanal Makinenin özel etkinlik çalıştırmadan önce hazırlamak için 15 ila 30 dakika sürebilir. Havuz farklı autoScaleEvaluationInterval kullanıyorsa, Batch hizmeti autoScaleEvaluationInterval artı 10 dakika sürebilir.

1. Örnek çözümde **yürütme** yöntemini çağırır **Calculate** bir çıktı veri dilimi üretmek için bir giriş veri dilimi işleyen yöntem. Girdi verilerini işlemek ve değiştirmek için kendi yönteminizi yazabileceğiniz **Calculate** yöntem çağrısı **yürütme** yöntemi, yönteme bir çağrı ile.

### <a name="next-steps-consume-the-data"></a>Sonraki adımlar: Verileri kullanır
Veri işleme sonra Power BI gibi çevrimiçi araçları kullanabilir. Power BI ve nasıl Azure'da kullanılacağını anlamanıza yardımcı olması için bağlantılar şunlardır:

* [Power bı'da bir veri kümesine keşfedin](https://powerbi.microsoft.com/documentation/powerbi-service-get-data/)
* [Power BI Desktop ile çalışmaya başlama](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/)
* [Power bı'da veri yenileme](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/)
* [Azure ve Power BI için: Temel genel bakış](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)

## <a name="references"></a>Başvurular
* [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)

  * [Data Factory hizmetine giriş](data-factory-introduction.md)
  * [Data Factory ile çalışmaya başlama](data-factory-build-your-first-pipeline.md)
  * [Bir Data Factory işlem hattında özel etkinlikler kullanma](data-factory-use-custom-activities.md)
* [Azure Batch](https://azure.microsoft.com/documentation/services/batch/)

  * [Batch temel bilgileri](../../batch/batch-technical-overview.md)
  * [Batch özelliklerine genel bakış](../../batch/batch-api-basics.md)
  * [Azure portalında bir Batch hesabı oluşturabilir ve yönetebilirsiniz](../../batch/batch-account-create-portal.md)
  * [.NET için Batch istemci kitaplığını kullanmaya başlama](../../batch/quick-run-dotnet.md)

[batch-explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch-explorer-walkthrough]: https://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx
