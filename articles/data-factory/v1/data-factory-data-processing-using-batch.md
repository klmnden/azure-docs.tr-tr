---
title: Veri Fabrikası ve toplu kullanarak büyük ölçekli veri kümeleri işlem | Microsoft Docs
description: Azure Batch yeteneğini işleme paralel kullanarak büyük miktarlarda bir Azure Data Factory işlem hattı verileri işlemek açıklar.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 688b964b-51d0-4faa-91a7-26c7e3150868
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 8f0cd8aad2d5c5142fc66c78393b57ff210a7b83
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="process-large-scale-datasets-by-using-data-factory-and-batch"></a>Veri Fabrikası ve toplu kullanarak işlem büyük ölçekli veri kümeleri
> [!NOTE]
> Bu makale, Azure Data Factory’nin genel kullanıma açık olan 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [özel etkinlikleri sürüm 2 veri fabrikasında](../transform-data-using-dotnet-custom-activity.md).

Bu makalede bir taşır ve büyük ölçekli veri kümeleri otomatik ve zamanlanmış bir şekilde işleyen örnek bir çözüm mimarisini açıklar. Ayrıca, veri fabrikası ve Azure Batch kullanarak çözümü uygulamak için bir uçtan uca izlenecek yol da sağlar.

Bu makalede tipik bir makale uzun çünkü tüm örnek çözümünü bir kılavuz içerir. Bu hizmetler hakkında bilgi edinebilirsiniz toplu ve veri fabrikası için yeni ve nasıl birlikte çalışır. Hizmetler hakkında bir şey bilmeniz ve tasarlama/çözüm mimarisi oluşturma, üzerinde odaklanabilirsiniz [mimarisi bölümüne](#architecture-of-sample-solution) makalenin. Bir prototip ya da bir çözüm geliştirirken, adım adım yönergeleri denemek isteyebilirsiniz [izlenecek](#implementation-of-sample-solution). Bu içerik ve bunu nasıl kullandığı hakkında yorumlarınızı davet ediyoruz.

İlk olarak, veri fabrikası ve toplu işlem hizmetleri size nasıl yardımcı olabilir işlem büyük veri kümelerini bulutta bakalım.     

## <a name="why-azure-batch"></a>Neden Azure toplu işlem?
 Batch, büyük ölçekli paralel ve yüksek performanslı) bilgi işlem (HPC uygulamalarını bulutta verimli bir şekilde çalıştırmak için kullanabilirsiniz. Sanal makineleri (VM'ler) üzerinde bir yönetilen koleksiyonu çalıştırılacak işlem yoğunluklu işi zamanlayan bir platform hizmetidir. Otomatik olarak işleriniz ihtiyaçlarını karşılamak için işlem kaynaklarını ölçeklendirme yapabilir.

Batch hizmetiyle, uygulamalarınızı paralel olarak ve ölçekte yürütmek için Azure işlem kaynaklarını tanımlayın. İsteğe bağlı çalıştırabilirsiniz veya zamanlanmış işler. El ile oluşturmak, yapılandırmak ve HPC Kümesi, tek tek sanal makineleri, sanal ağlar veya karmaşık iş ve görev zamanlama altyapısını yönetmek gerekmez.

 Toplu işlemle alışık değilseniz, aşağıdaki makalelere, bu makalede açıklanan çözüm mimarisi/uyarlamasını anlamanıza yardımcı:   

* [Batch temel bilgileri](../../batch/batch-technical-overview.md)
* [Batch özelliklerine genel bakış](../../batch/batch-api-basics.md)

İsteğe bağlı olarak, toplu işlem hakkında daha fazla bilgi için bkz: [Batch öğrenme yolu](https://azure.microsoft.com/documentation/learning-paths/batch/).

## <a name="why-azure-data-factory"></a>Neden Azure Data Factory?
Data Factory, verilerin taşınmasını ve dönüştürülmesini düzenleyen ve otomatikleştiren bulut tabanlı bir veri tümleştirme hizmetidir. Data Factory, şirket içi veri taşıma ve bir merkezi veri deposuna veri depolarına bulut yönetilen veri işlem hatlarını oluşturmak için kullanabilirsiniz. Azure Blob Depolama örneğidir. Data Factory işlem/Veri Dönüştürme Hizmetleri Azure Hdınsight ve Azure Machine Learning gibi kullanarak için kullanabilirsiniz. Ayrıca, zamanlanmış bir şekilde (örneğin, saatlik, günlük ve haftalık) çalıştırmak için veri ardışık zamanlayabilirsiniz. İzleme ve ardışık düzen sorunları belirlemek ve eylem için bir bakışta yönetme.

  Data Factory ile alışık değilseniz, aşağıdaki makaleler, bu makalede açıklanan çözüm mimarisi/uyarlamasını anlamanıza yardımcı:  

* [Data Factory'ye giriş](data-factory-introduction.md)
* [İlk veri hattınızı oluşturma](data-factory-build-your-first-pipeline.md)   

İsteğe bağlı olarak, veri fabrikası hakkında daha fazla bilgi için bkz: [Data Factory öğrenme yolu](https://azure.microsoft.com/documentation/learning-paths/data-factory/).

## <a name="data-factory-and-batch-together"></a>Veri Fabrikası ve birlikte toplu işlem
Veri Fabrikası yerleşik etkinlikler içerir. Örneğin, kopyalama etkinliği, bir hedef veri deposu için bir kaynak veri deposundan copy/move veriler için kullanılır. Hive etkinliği, Azure üzerinde Hadoop kümeleri (Hdınsight) kullanarak verileri işlemek için kullanılır. Desteklenen dönüştürme etkinliklerinin listesi için bkz: [veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md).

Taşımak veya kendi mantığı ile verileri işlemek için özel .NET etkinlikler de oluşturabilirsiniz. Bu etkinlikler, Hdınsight kümesi ya da sanal makineleri bir Batch havuzu çalıştırabilirsiniz. Batch hizmetini kullanırken otomatik ölçeklendirme havuzuna yapılandırabilirsiniz (ekleme veya iş yüküne göre sanal makineleri kaldırın) sağladığınız bir formüle dayanarak.     

## <a name="architecture-of-a-sample-solution"></a>Örnek bir çözüm mimarisi
  Bu makalede açıklanan mimarisi için basit bir çözümdür. Finansal Hizmetler, görüntü işleme ve işleme ve genomic analiz tarafından modelleme risk gibi karmaşık senaryolar için de geçerlidir.

Aşağıdaki diyagramda, nasıl Data Factory veri hareketlerini ve işleme düzenler açıklanmıştır. Toplu veri paralel bir şekilde nasıl işlediği gösterilmektedir. Karşıdan yükleyip diyagramı (11 x 17 inç veya A3 boyutu) kolayca başvurmak için yazdırın. Böylece yazdırabilmek diyagram erişmek için bkz: [toplu ve Data Factory kullanarak HPC ve veriler orchestration](http://go.microsoft.com/fwlink/?LinkId=717686).

[![Büyük ölçekli veri işleme diyagramı](./media/data-factory-data-processing-using-batch/image1.png)](http://go.microsoft.com/fwlink/?LinkId=717686)

Aşağıdaki listede işleminin temel adımları sağlar. Çözüm, kod ve uçtan uca çözümü oluşturmak için açıklamalar içerir.

* **Toplu işlem düğümleri (VM'ler) havuzuyla yapılandırın.** Düğüm sayısını ve her düğümün boyutu belirtebilirsiniz.

* **Data Factory örneğini oluşturabilir** blob depolama, toplu işlem hizmeti, girdi/çıktı verilerini ve iş akışı/ardışık taşıyın ve veri dönüştürme etkinlikleri ile temsil eden varlık ile yapılandırılmış.

* **Özel bir .NET etkinliği içinde Data Factory işlem hattı oluşturun.** Batch havuzu üzerinde çalışır, kullanıcı kodu etkinliktir.

* **Azure Storage blobları olarak büyük miktarlarda giriş veri depolayın.** Veriler (genellikle zamanına göre) mantıksal dilimlere bölünür.

* **Veri Fabrikası paralel olarak işlenir verileri kopyalar** ikincil konuma.

* **Veri Fabrikası toplu işi tarafından ayrılan havuzu kullanılarak özel etkinlik çalıştırılır.** Veri Fabrikası etkinlikleri birlikte çalışabilir. Her etkinlik veri dilimini işler. Sonuçları depolama alanında depolanır.

* **Veri Fabrikası son sonuçları üçüncü bir konuma taşır** dağıtımı bir uygulama aracılığıyla veya diğer araçları tarafından işlenmesi için.

## <a name="implementation-of-the-sample-solution"></a>Örnek çözümü uygulaması
Örnek çözümü kasıtlı olarak basit bir işlemdir. Veri Fabrikası ve toplu işlem veri kümeleri birlikte nasıl kullanılacağını göstermek için tasarlanmıştır. Çözüm arama terimi bir zaman serisinin düzenlenir giriş dosyaları "Microsoft" oluşumlarını sayar. Ardından, çıktı dosyalarını sayıya çıkarır.

**Süre:** Azure, veri fabrikası ve Batch temelleri tanıdık ve aşağıdaki önkoşulları tamamladığınızdan varsa, bu çözüm tamamlamak için bir ila iki saat alır.

### <a name="prerequisites"></a>Önkoşullar
#### <a name="azure-subscription"></a>Azure aboneliği
Bir Azure aboneliğiniz yoksa, ücretsiz bir deneme hesabı hızlı bir şekilde oluşturabilirsiniz. Daha fazla bilgi için bkz: [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).

#### <a name="azure-storage-account"></a>Azure depolama hesabı
Bu öğreticide verileri depolamak için bir depolama hesabı kullanın. Bir depolama hesabınız yoksa bkz [depolama hesabı oluşturma](../../storage/common/storage-create-storage-account.md#create-a-storage-account). Örnek çözümü blob depolama kullanır.

#### <a name="azure-batch-account"></a>Azure toplu işlem hesabı
Kullanarak Batch hesabı oluşturma [Azure portal](http://portal.azure.com/). Daha fazla bilgi için bkz: [oluşturma ve Batch hesabını yönetmek](../../batch/batch-account-create-portal.md). Toplu işlem hesabı adı ve hesap anahtarını unutmayın. Aynı zamanda [New-AzureRmBatchAccount](https://msdn.microsoft.com/library/mt603749.aspx) cmdlet'ini bir toplu işlem hesabı oluşturun. Bu cmdlet'in nasıl kullanılacağı hakkında yönergeler için bkz [Batch PowerShell cmdlet'leri kullanmaya başlama](../../batch/batch-powershell-cmdlets-get-started.md).

Örnek çözümü (VM'ler yönetilen koleksiyonu) işlem düğümlerinin havuzunda paralel şekilde veri işlemek için Batch (üzerinden dolaylı olarak data factory işlem hattı) kullanır.

#### <a name="azure-batch-pool-of-virtual-machines"></a>Azure Batch havuzundaki sanal makineler
En az iki işlem düğümleriyle Batch havuzu oluşturma.

1. İçinde [Azure portal](https://portal.azure.com)seçin **Gözat** soldaki menüden ve seçin **toplu işlem hesaplarını**.

2. Açmak için toplu işlem hesabınızı seçin **toplu işlem hesabı** dikey.

3. Seçin **havuzları** döşeme.

4. Üzerinde **havuzları** dikey penceresinde, select **Ekle** bir havuzu eklemek için araç çubuğunda.

   a. Havuzu için bir kimlik girin (**havuzu kimliği**). Havuz Kimliğini not alın. Veri Fabrikası çözüm oluşturduğunuzda gerekir.

   b. Belirtin **Windows Server 2012 R2** için **işletim sistemi ailesi** ayarı.

   c. Seçin bir **düğüm fiyatlandırma katmanı**.

   d. Girin **2** değeri olarak **hedef ayrılmış** ayarı.

   e. Girin **2** değeri olarak **en fazla düğüm başına görevleri** ayarı.

   f. Seçin **Tamam** havuzu oluşturmak için.

#### <a name="azure-storage-explorer"></a>Azure Depolama Gezgini
Kullandığınız [Azure Depolama Gezgini 6](https://azurestorageexplorer.codeplex.com/) veya [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) (ClumsyLeaf yazılımından) inceleyebilir ve depolama projelerinizi verileri değiştirmek için. Ayrıca inceleyin ve verileri bulutta barındırılan uygulamalarınızın günlüklerine alter.

1. Adlı bir kapsayıcı oluşturmak **mycontainer** özel erişim (anonim erişimi yok).

2. CloudXplorer kullanırsanız, klasörler ve alt klasörler ile aşağıdaki yapısını oluşturun:

   ![Klasör ve alt klasör yapısı](./media/data-factory-data-processing-using-batch/image3.png)

   `Inputfolder` ve `outputfolder` en üst düzey klasörlerde bulunan `mycontainer`. `inputfolder` Klasörü tarih-saat Damgalar (YYYY-AA-GG-ss) ile alt klasörler bulunur.

   Depolama Gezgini, sonraki adımda kullanırsanız, şu adlara sahip dosyaları karşıya yükleme: `inputfolder/2015-11-16-00/file.txt`, `inputfolder/2015-11-16-01/file.txt`ve benzeri. Bu adım, klasörleri otomatik olarak oluşturur.

3. Bir metin dosyası oluşturun **dosya.txt'yi** anahtar sözcüğü sahip içerikle makinenizde **Microsoft**. "Özel Etkinlik Microsoft test özel etkinlik Microsoft test" örneğidir

4. Dosyayı karşıya yüklemeyi blob depolama aşağıdaki giriş klasörlerde:

   ![Giriş klasörleri](./media/data-factory-data-processing-using-batch/image4.png)

   Depolama Gezgini kullanırsanız, karşıya yükleme **dosya.txt'yi** dosya **mycontainer**. Seçin **kopyalama** blob bir kopyasını oluşturmak için araç çubuğunda. İçinde **kopyalama Blob** iletişim kutusu, değişiklik **hedef blob adı** için `inputfolder/2015-11-16-00/file.txt`. Oluşturmak için bu adımı yineleyin `inputfolder/2015-11-16-01/file.txt`, `inputfolder/2015-11-16-02/file.txt`, `inputfolder/2015-11-16-03/file.txt`, `inputfolder/2015-11-16-04/file.txt`ve benzeri. Bu eylem, klasörleri otomatik olarak oluşturur.

5. Adlı başka bir kapsayıcı oluşturmak `customactivitycontainer`. Özel Etkinlik zip dosyası bu kapsayıcıya yükleyin.

#### <a name="visual-studio"></a>Visual Studio
Veri Fabrikası çözümde kullanılacak özel toplu iş etkinliği oluşturmak için Visual Studio 2012 veya sonraki sürümünü yükleyin.

### <a name="high-level-steps-to-create-the-solution"></a>Çözüm oluşturmak için üst düzey adımlar
1. Veri işleme mantığı içeren özel bir aktivite oluşturun.

2. Özel Etkinlik kullanan bir veri fabrikası oluşturun.

### <a name="create-the-custom-activity"></a>Özel etkinlik oluşturma
Veri Fabrikası özel Bu örnek çözümü Kalp etkinliktir. Örnek çözümü toplu Özel Etkinlik çalıştırmak için kullanır. Özel etkinlikler geliştirmek ve bunları veri fabrikası ardışık düzenlerinde hakkında daha fazla bilgi için bkz: [data factory işlem hattı içinde özel etkinlikleri kullanmak](data-factory-use-custom-activities.md).

Bir data factory işlem hattı kullanabileceğiniz bir .NET özel etkinlik oluşturmak için IDotNetActivity arabirimini uygulayan bir sınıf ile bir .NET sınıf kitaplığı projesi oluşturun. Bu arabirim yalnızca bir yöntemi vardır: yürütün. Yöntem imzası şöyledir:

```csharp
public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices,
            IEnumerable<Dataset> datasets,
            Activity activity,
            IActivityLogger logger)
```

Yöntemi anlamanız için gereken birkaç önemli bileşeni vardır:

* Yöntemi, dört parametreleri alır:

  * **linkedServices**. Bu parametre, data factory giriş/çıkış veri kaynakları (örneğin, blob depolama) bağlantı bağlı hizmetler numaralandırılabilir listesidir. Bu örnekte, yalnızca bir bağlı hizmeti Azure giriş ve çıkış için kullanılan depolama türü yok.
  * **veri kümeleri**. Bu parametre, veri kümeleri numaralandırılabilir listesidir. Giriş ve çıkış veri kümeleri tarafından tanımlanan şemaları ve konumlarını almak için bu parametreyi kullanın.
  * **Etkinlik**. Bu parametre, geçerli işlem varlığı temsil eder. Bu durumda, bir toplu işlem hizmeti sunulmaktadır.
  * **Günlükçü**. Günlükçü ardışık düzeni için "Kullanıcı" günlük olarak o yüzeyini hata ayıklama yorum yazmak için kullanabilirsiniz.
* Bu yöntem, özel etkinlikler gelecekte zincir için kullanılan bir sözlüğü döndürür. Bu özellik henüz uygulanmadı, böylece yalnızca boş bir sözlük döndürme.

#### <a name="procedure-create-the-custom-activity"></a>Yordam: özel etkinlik oluşturma
1. Visual Studio'da .NET sınıf kitaplığı projesi oluşturun.

   a. Visual Studio 2012/2013/2015 başlatın.

   b. **Dosya** > **Yeni** > **Proje**’yi seçin.

   c. Genişletme **şablonları**seçip **Visual C\#**. Bu kılavuzda, kullandığınız C\#, ancak özel etkinlik geliştirmek için herhangi bir .NET dil kullanabilirsiniz.

   d. Seçin **sınıf kitaplığı** proje türleri sağdaki listeden.

   e. Girin **MyDotNetActivity** için **adı**.

   f. Seçin **C:\\ADF** için **konumu**. Bir klasör oluşturun **ADF** yoksa.

   g. Seçin **Tamam** projesi oluşturmak için.

2. Seçin **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.

3. Paket Yöneticisi konsolunda Microsoft.Azure.Management.DataFactories içeri aktarmak için aşağıdaki komutu yürütün:

    ```powershell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. İçeri aktarma **Azure Storage** NuGet paketini projeye. Bu örnek Blob Depolama API kullandığından bu paketi gerekir:

    ```powershell
    Install-Package Azure.Storage
    ```
5. Aşağıdakileri ekleyin projenin kaynak dosyasında yönergeleri kullanarak:

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
6. Ad alanına adını değiştirmek **MyDotNetActivityNS**.

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. Sınıfın adını değiştirmek **MyDotNetActivity**ve buradan türetebilir **IDotNetActivity** arabirim gösterildiği gibi:

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. (Ekle) uygulama **yürütme** yöntemi **IDotNetActivity** arabirimini **MyDotNetActivity** sınıfı. Aşağıdaki örnek kod yönteme kopyalayın. Bu yöntemde kullanılan mantığı açıklaması için bkz: [yöntemin](#execute-method) bölümü.

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
9. Aşağıdaki yardımcı yöntemler sınıfına ekleyin. Bu yöntemler tarafından çağrılan **yürütme** yöntemi. En önemli **Hesapla** yöntemi her bir blob tekrarlanan kod yalıtır.

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
    Veri kümesi işaret klasörüne yol GetFolderPath yöntemi döndürür ve GetFileName yöntemi dataset işaret blob/dosya adını döndürür.

    ```csharp

    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
    ```

    Calculate yöntemi girdi dosyaları (BLOB klasöründe) içinde "Microsoft" anahtar sözcüğü örneklerinin sayısını hesaplar. Arama "Microsoft" kodda sabit kodlanmış bir terimdir.

10. Projeyi derleyin. Seçin **yapı** menüsünden ve ardından **yapı çözümü**.

11. Windows Gezgini'ni başlatın ve Git **bin\\hata ayıklama** veya **bin\\yayın** klasör. Klasör Seçimi yapı türüne bağlıdır.

12. Zip dosyası oluşturun **MyDotNetActivity.zip** içindeki tüm ikili dosyaları içeren  **\\bin\\hata ayıklama** klasör. MyDotNetActivity eklemek isteyebilirsiniz. **pdb** bir hata oluştuğunda, soruna neden kaynak kodunda satır numarası gibi ek ayrıntıları almak için dosya.

   ![Bin\Debug klasör listesi](./media/data-factory-data-processing-using-batch/image5.png)

13. Karşıya yükleme **MyDotNetActivity.zip** blob kapsayıcıya bir BLOB `customactivitycontainer` StorageLinkedService ADFTutorialDataFactory kullanır hizmetinde bağlı blob storage'da. Blob kapsayıcı oluşturun `customactivitycontainer` zaten yoksa.

#### <a name="execute-method"></a>Execute yöntemi
Bu bölümde kodda çalıştırma yöntemi hakkında daha fazla ayrıntı sağlar.

1. Giriş koleksiyonu yineleme için üyeleri bulunan [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) ad alanı. Blob koleksiyonda yinelemek için kullanmak isteniyor **BlobContinuationToken** sınıfı. Esas olarak, bir kullanın-while döngüsünü döngüden çıkma mekanizması olarak belirteci ile. Daha fazla bilgi için bkz: [kullanım Blob depolama alanından .NET](../../storage/blobs/storage-dotnet-how-to-use-blobs.md). Temel bir döngü burada gösterilir:

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

2. BLOB'ları kümesi aracılığıyla mantıksal olarak çalışmak için kod içinde do gider-while döngüsünü. İçinde **yürütme** yöntemi, do-döngü BLOB'ları listesi adlı bir yönteme geçirir **Hesapla**. Adlı bir dize değişkeni yöntemi döndürür **çıkış** kesimindeki tüm BLOB'lar aracılığıyla yinelendiğinde başka bir deyişle sonucu.

   Blob içinde "Microsoft" geçirilen arama terimi oluşumları sayısını döndürür **Hesapla** yöntemi.

    ```csharp
    output += string.Format("{0} occurrences of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
    ```
3. Sonra **Hesapla** yöntemi tamamlandığında, bunu yeni bir blob için yazılmış olmalıdır. İşlenen BLOB'lar her kümesi için yeni bir blob sonuçlarıyla yazılabilir. Yeni bir blob yazmak için ilk çıkış veri kümesi bulun.

    ```csharp
    // Get the output dataset by using the name of the dataset matched to a name in the Activity output collection.
    Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    ```
4. Kod ayrıca yardımcı yöntemini çağırır **GetFolderPath** klasör yolu (depolama kapsayıcısı adı) alınamadı.

    ```csharp
    folderPath = GetFolderPath(outputDataset);
    ```
   GetFolderPath yöntemi FolderPath adlı bir özelliği olan bir AzureBlobDataSet DataSet nesnesine çevirir.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FolderPath;
    ```
5. Kod çağrıları **GetFileName** dosya adı (blob) alma yöntemi. Klasör yolu almak için kullanılan önceki kod için kod benzer.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FileName;
    ```
6. Dosyanın adını bir URI nesnesinden oluşturarak yazılır. URI Oluşturucusu kullanan **BlobEndpoint** kapsayıcı adını döndürmek için özellik. Klasör yolunu ve dosya adı, çıktı blob URI'si oluşturmak eklenir.  

    ```csharp
    // Write the name of the file.
    Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    ```
7. Dosyanın adını yazıldıktan sonra çıktı dizeden yazabilirsiniz **Hesapla** yöntemi yeni bir blob için:

    ```csharp
    // Create a blob and upload the output text.
    CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
    logger.Write("Writing {0} to the output blob", output);
    outputBlob.UploadText(output);
    ```

### <a name="create-the-data-factory"></a>Veri Fabrikası oluşturma
İçinde [özel etkinlik oluşturmak](#create-the-custom-activity) bölümünde, özel bir aktivite oluşturulur ve ikili dosyaları zip dosyasıyla ve PDB dosya bir blob kapsayıcıya karşıya yüklenmedi. Bu bölümde, veri fabrikası özel etkinlik kullanan sahip işlem hattı oluşturun.

Özel Etkinlik için giriş veri kümesi (dosyaları) BLOB giriş klasöründe temsil eder (`mycontainer\\inputfolder`) blob depolama. Çıktı veri kümesi etkinliğinin çıkış klasöründe çıkış BLOB'ları temsil eder (`mycontainer\\outputfolder`) blob depolama.

Bir veya daha fazla giriş klasörler halinde bırak:

```
mycontainer -\> inputfolder
    2015-11-16-00
    2015-11-16-01
    2015-11-16-02
    2015-11-16-03
    2015-11-16-04
```

Örneğin, bir dosya (dosya.txt'yi) aşağıdaki içerik ile her klasörler bırak:

```
test custom activity Microsoft test custom activity Microsoft
```

İki veya daha fazla dosya klasör sahip olsa bile her giriş klasörü veri fabrikasında bir dilim karşılık gelir. Her dilimi ardışık düzen tarafından işlendiğinde, özel etkinlik tüm BLOB'ları, dilim için giriş klasörü dolaşır.

Aynı içeriğe sahip beş çıktı dosyalarına bakın. Örneğin, 2015-11-16-00 klasöründeki dosyaya işlemesini çıktı dosyası aşağıdaki içeriğe sahip:

```
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
```

Giriş klasörüne aynı içeriğe sahip birden çok dosya (dosya.txt'yi, file2.txt, file3.txt) bırakma, çıktı dosyasında aşağıdaki içeriğe bakın. Her bir klasör (2015-11-16-00, vb.) birden çok giriş dosyaları klasörü olmasına rağmen bir dilim bu örnekteki karşılık gelir.

```csharp
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file2.txt.
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file3.txt.
```

Çıktı dosyası üç satır artık, her girdi dosyasında (blob) (2015-11-16-00) dilimle ilişkili klasörü için bir tane var.

Bir görev çalıştırmak her etkinlik için oluşturulur. Bu örnekte, ardışık düzeninde yalnızca bir etkinlik yok. Bir dilim ardışık düzen tarafından işlendiğinde, özel etkinlik dilim işlemek için toplu olarak çalışır. Beş dilimleri (her dilim birden çok BLOB veya dosya olabilir) olduğundan, beş görevleri toplu işlemde oluşturulur. Toplu işlemindeki bir görevin çalıştığı çalışan özel etkinlik olur.

Aşağıdaki örneklerde ek ayrıntılar sağlar.

#### <a name="step-1-create-the-data-factory"></a>1. adım: veri fabrikası oluşturma
1. İçin oturum açtıktan sonra [Azure portal](https://portal.azure.com/), aşağıdaki adımları uygulayın:

   a. Seçin **yeni** sol menüde.

   b. Seçin **veri + analiz** üzerinde **yeni** dikey.

   c. Seçin **Data Factory** üzerinde **veri analizi** dikey.

2. Üzerinde **yeni data factory** dikey penceresinde girin **CustomActivityFactory** adı. Veri fabrikasının adı genel olarak benzersiz olmalıdır. "Veri fabrikası adı CustomActivityFactory kullanılabilir değil" hatasını alırsanız data factory adını değiştirin. Örneğin, yournameCustomActivityFactory kullanın ve veri fabrikası yeniden oluşturun.

3. Seçin **kaynak grubu adı**, varolan bir kaynak grubu seçin veya bir kaynak grubu oluşturun.

4. Oluşturulacak data factory bölgesini ve abonelik doğru olduğundan emin olun.

5. Seçin **oluşturma** üzerinde **yeni data factory** dikey.

6. Veri Fabrikası portal panosunda oluşturulur.

7. Veri Fabrikası başarıyla oluşturulduktan sonra gördüğünüz **veri fabrikası** sayfasında, veri fabrikası içeriğini gösterir.

   ![Veri Fabrikası sayfası](./media/data-factory-data-processing-using-batch/image6.png)

#### <a name="step-2-create-linked-services"></a>2. adım: bağlı hizmetler oluşturma
Bağlı hizmetler veri depolarını veya işlem Hizmetleri veri fabrikası için. Bu adımda, veri fabrikanıza depolama hesabı ve toplu işlem hesabı bağlayın.

#### <a name="create-an-azure-storage-linked-service"></a>Azure Depolama bağlı hizmeti oluşturma
1. Seçin **yazar ve dağıtma** döşemesinin **veri fabrikası** dikey **CustomActivityFactory**. Data Factory Düzenleyici görüntülenir.

2. Seçin **yeni veri deposu** komut çubuğunda ve **Azure depolama.** JSON betiği bağlantılı Hizmet Düzenleyicisi'nde görüntülenen bir depolama alanı oluşturmak için kullanın.

   ![Yeni veri deposu](./media/data-factory-data-processing-using-batch/image7.png)

3. **Hesap adı** değerini depolama hesabınızın adıyla değiştirin. **Hesap anahtarı** değerini depolama hesabının erişim anahtarıyla değiştirin. Depolama erişim anahtarınızı alma hakkında bilgi için bkz: [görüntüleme, kopyalama ve erişim anahtarları yeniden oluşturma depolama](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).

4. Bağlı hizmeti dağıtmak için komut çubuğunda **Dağıt**’ı seçin.

   ![Dağıtma](./media/data-factory-data-processing-using-batch/image8.png)

#### <a name="create-an-azure-batch-linked-service"></a>Bir Azure Batch bağlı hizmeti oluşturma
Bu adımda, veri fabrikası özel etkinliği çalıştırmak için kullanılan, toplu işlem hesabı için bağlı hizmet oluşturun.

1. Seçin **yeni işlem** komut çubuğunda ve **Azure Batch.** JSON betiği bağlantılı Hizmet Düzenleyicisi'nde görüntülenen toplu oluşturmak için kullanın.

2. JSON betiği:

   a. Değiştir **hesap adı** toplu işlem hesabı adı.

   b. Değiştir **erişim tuşu** toplu işlem hesabının erişim anahtarı ile.

   c. İçin havuz Kimliğini girin **poolName** özelliği. Bu özellik için havuz adını veya havuz kimliğini belirtebilirsiniz

   d. URI toplu girmek için **batchUri** JSON özelliği.

      > [!IMPORTANT]
      > URL'den **toplu işlem hesabı** dikey olan şu biçimde: \<accountname\>.\< Bölge\>. batch.azure.com. İçin **batchUri** özelliği JSON komut dosyasında, ihtiyacınız a88 "accountname." kaldırmak ** URL'den. `"batchUri": "https://eastus.batch.azure.com"` bunun bir örneğidir.
      >
      >

      ![Batch hesabı dikey penceresi](./media/data-factory-data-processing-using-batch/image9.png)

      İçin **poolName** özelliği, havuzu havuzunun adı yerine Kimliğini de belirtebilirsiniz.

      > [!NOTE]
      > Hdınsight için yaptığı gibi Data Factory hizmeti toplu işlemi için bir isteğe bağlı seçeneği desteklemiyor. Veri fabrikasında yalnızca kendi Batch havuzu kullanabilirsiniz.
      >
      >
   
   e. Belirtin **StorageLinkedService** için **linkedServiceName** özelliği. Bu bağlı hizmeti önceki adımda oluşturduğunuz. Bu depolama dosyaları ve günlükleri için hazırlama alanı kullanılır.

3. Bağlı hizmeti dağıtmak için komut çubuğunda **Dağıt**’ı seçin.

#### <a name="step-3-create-datasets"></a>3. adım: veri kümeleri oluşturma
Bu adımda, girdi ve çıktı verilerini temsil edecek veri kümeleri oluşturma.

#### <a name="create-the-input-dataset"></a>Girdi veri kümesini oluşturma
1. Data Factory düzenleyici seçin **yeni veri kümesi** araç çubuğunda. Seçin **Azure Blob Depolama** aşağı açılan listeden.

2. Sağ bölmedeki JSON betiği aşağıdaki JSON parçacığıyla değiştirin:

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

    Başlangıç zamanı 2015 ile bu kılavuzda daha sonra bir işlem hattı oluşturma-11-16T00:00:00Z ve bitiş zamanı 2015-11-16T05:00:00Z. Beş giriş/çıkış dilimler olduklarından, verilerin saatlik, üretmek için zamanlanmış (arasında **00**: 00:00 -\> **05**: 00:00).

    **Sıklığı** ve **aralığı** giriş veri kümesi için ayarlamak **saat** ve **1**, girdi dilimi saatlik kullanılabilir olduğunu anlamına gelir.

    Her dilim için başlangıç saatini tarafından temsil edilen **SliceStart** önceki JSON parçacığında bulunan sistem değişkeni. Burada, her dilim için başlangıç zamanlarını bulunmaktadır.

    | **Dilim** | **Başlangıç saati**          |
    |-----------|-------------------------|
    | 1         | 2015-11-16T**00**:00:00 |
    | 2         | 2015-11-16T**01**:00:00 |
    | 3         | 2015-11-16T**02**:00:00 |
    | 4         | 2015-11-16T**03**:00:00 |
    | 5         | 2015-11-16T**04**:00:00 |

    **FolderPath** dilim başlangıç zamanı yıl, ay, gün ve saat parçası kullanılarak hesaplanır (**SliceStart**). İşte bir giriş klasörü için bir dilim nasıl eşlendi.

    | **Dilim** | **Başlangıç saati**          | **Giriş klasörü**  |
    |-----------|-------------------------|-------------------|
    | 1         | 2015-11-16T**00**:00:00 | 2015-11-16-**00** |
    | 2         | 2015-11-16T**01**:00:00 | 2015-11-16-**01** |
    | 3         | 2015-11-16T**02**:00:00 | 2015-11-16-**02** |
    | 4         | 2015-11-16T**03**:00:00 | 2015-11-16-**03** |
    | 5         | 2015-11-16T**04**:00:00 | 2015-11-16-**04** |

3. Seçin **dağıtma** oluşturmak ve dağıtmak için araç çubuğunda **InputDataset** tablo.

#### <a name="create-the-output-dataset"></a>Çıktı veri kümesini oluşturma
Bu adımda, çıktı verilerini göstermek için AzureBlob türünde başka bir veri kümesi oluşturun.

1. Data Factory düzenleyici seçin **yeni veri kümesi** araç çubuğunda. Seçin **Azure Blob Depolama** aşağı açılan listeden.

2. Sağ bölmedeki JSON betiği aşağıdaki JSON parçacığıyla değiştirin:

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

    Bir çıkış blob/dosyası, her girdi dilimi için oluşturulur. İşte bir çıktı dosyası için her dilimi nasıl adlandırılır. Bir çıkış klasöründe oluşturulan tüm çıktı dosyaları `mycontainer\\outputfolder`.

    | **Dilim** | **Başlangıç saati**          | **Çıkış dosyası**       |
    |-----------|-------------------------|-----------------------|
    | 1         | 2015-11-16T**00**:00:00 | 2015-11-16 -**00. txt** |
    | 2         | 2015-11-16T**01**:00:00 | 2015-11-16 -**01. txt** |
    | 3         | 2015-11-16T**02**:00:00 | 2015-11-16 -**02. txt** |
    | 4         | 2015-11-16T**03**:00:00 | 2015-11-16 -**03. txt** |
    | 5         | 2015-11-16T**04**:00:00 | 2015-11-16 -**04. txt** |

    Bir giriş klasöründeki tüm dosyalar (örneğin, 2015-11-16-00) bir dilim başlangıç saati 2015-11-16-00 ile bir parçası olduğunu unutmayın. Bu dilim işlendiğinde özel etkinlik her dosyası aracılığıyla tarar ve arama terimi "Microsoft" oluşum sayısı ile çıkış dosyasındaki bir satır oluşturur 2015-11-16-00 klasöründe üç dosya varsa, çıktı dosyası 2015-11-16-00.txt içinde üç satır vardır.

3. Seçin **dağıtma** oluşturmak ve dağıtmak için araç çubuğunda **OutputDataset**.

#### <a name="step-4-create-and-run-the-pipeline-with-a-custom-activity"></a>4. adım: Oluşturma ve ardışık düzen ile özel bir aktivite çalıştırma
Bu adımda, bir etkinlik, daha önce oluşturduğunuz özel etkinliği ile işlem hattı oluşturun.

> [!IMPORTANT]
> Karşıya henüz yüklediyseniz **dosya.txt'yi** işlem hattı oluşturmadan önce klasörleri blob kapsayıcısında giriş için bunu. **İsPaused** özelliği ayarlanmış JSON, işlem hattındaki false ardışık düzeni için hemen çalışır şekilde **Başlat** geçmişte tarihidir.
>
>

1. Data Factory düzenleyici seçin **yeni işlem hattı** komut çubuğunda. Komut görmüyorsanız, görüntülemek için üç nokta simgesini seçin.

2. Sağ bölmedeki JSON betiği aşağıdaki JSON parçacığıyla değiştirin:

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

   * Yalnızca bir etkinliktir ardışık düzeninde ve türü **DotNetActivity**.
   * **AssemblyName** DLL adına ayarlanır **MyDotNetActivity.dll**.
   * **EntryPoint** ayarlanır **MyDotNetActivityNS.MyDotNetActivity**. Temelde olan \<ad alanı\>.\< ClassName\> kodunuzda.
   * **PackageLinkedService** ayarlanır **StorageLinkedService**, özel etkinlik zip dosyasını içeren blob depolama alanına işaret eder. Girdi/çıktı dosyalarını ve özel etkinlik zip dosyası için farklı depolama hesapları kullanıyorsanız, başka bir depolama bağlı hizmeti oluşturmanız gerekir. Bu makale, aynı depolama hesabını kullanan varsaymaktadır.
   * **PackageFile** ayarlanır **customactivitycontainer/MyDotNetActivity.zip**. Şu biçimdedir \<containerforthezip\>/\<nameofthezip.zip\>.
   * Özel Etkinlik alır **InputDataset** giriş olarak ve **OutputDataset** çıktı olarak.
   * **LinkedServiceName** özel etkinlik özelliğinin işaret **AzureBatchLinkedService**, özel etkinlik toplu olarak çalıştırmak için gereken veri fabrikası söyler.
   * **Eşzamanlılık** ayarı önemlidir. 1, iki veya varsa daha fazla işlem düğümleri Batch havuzunda olsa dahi, varsayılan değeri kullanırsanız dilimleri işlenir birbiri ardından. Bu nedenle, işlem yeteneği batch paralel özelliğinden yararlandığında değil. Ayarlarsanız **eşzamanlılık** daha yüksek bir değere deyin 2, bu iki dilimler anlamına gelir (toplu iki görevlere karşılık gelir) aynı anda işlenebilir. Bu durumda, Batch havuzundaki her iki VM yararlanılmıştır. Eşzamanlılık özelliğini uygun şekilde ayarlayın.
   * Yalnızca bir görev (dilim) varsayılan olarak, varsayılan olarak herhangi bir noktada bir VM üzerinde yürütülür. Varsayılan olarak, **maksimum VM başına görevleri** için Batch havuzu 1 ayarlayın. Önkoşullar bir parçası olarak, bu özelliği 2 olarak ayarlanmış bir havuzu oluşturuldu. Bu nedenle, iki veri fabrikası dilimler bir VM üzerinde aynı anda çalıştırabilirsiniz.
    - **İsPaused** özelliği varsayılan olarak false değerine ayarlanır. Geçmişte dilimleri başlatmak için ardışık düzen hemen bu örnekte çalışır. Bu özelliği ayarlamak **true** yedeklemek için ayarlama ve ardışık düzen duraklatmak için **false** yeniden başlatmak için.
    -   **Başlat** ve **son** kez beş saatten birbirinden olur. Ardışık düzen tarafından beş dilimlerinin şekilde dilimler saatlik, oluşturulur.

3. İşlem hattını dağıtmak için komut çubuğundan **Dağıt**’ı seçin.

#### <a name="step-5-test-the-pipeline"></a>5. adım: Test ardışık düzeni
Bu adımda, ardışık düzen giriş klasörler halinde dosyaları bırakarak sınayın. Her giriş klasörü için bir dosya ile işlem hattı test ederek başlatın.

1. Üzerinde **veri fabrikası** select Azure portaldaki dikey pencere **diyagramı**.

   ![Diyagram](./media/data-factory-data-processing-using-batch/image10.png)

2. İçinde **diyagramı** görüntülemek için girdi veri kümesi çift **InputDataset**.

   ![InputDataset](./media/data-factory-data-processing-using-batch/image11.png)

3. **InputDataset** tüm beş dilimleri ile hazır dikey penceresi görünür. Bildirim **DİLİM başlangıç saati** ve **DİLİM bitiş saati** her dilim için.

   ![Dilim başlangıç giriş ve bitiş saatlerini](./media/data-factory-data-processing-using-batch/image12.png)

4. İçinde **diyagramı** görünümü, select **OutputDataset**.

5. Beş çıkış dilimler görünür **hazır** bunlar oluşturulamadığı varsa durum.

   ![Çıktı dilim başlangıç ve bitiş saatlerini](./media/data-factory-data-processing-using-batch/image13.png)

6. Dilimler ile ilişkili görevleri görüntülemek üzere portalı kullanın ve her dilim çalıştırdı hangi VM bakın. Daha fazla bilgi için bkz: [Data Factory ve toplu tümleştirme](#data-factory-and-batch-integration) bölümü.

7. Çıkış dosyaları altında göründüğünü `mycontainer` içinde `outputfolder` blob depolama alanınızın içinde.

   ![Depolama çıktı dosyaları](./media/data-factory-data-processing-using-batch/image15.png)

   Beş çıktı dosyaları listelenir, her bir dilim giriş. Her ve çıkış dosyalarının içerik aşağıdaki çıkış benzer vardır:

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
    ```
   Aşağıdaki diyagram, veri fabrikası dilimler batch'teki görevleri nasıl eşleneceğine gösterir. Bu örnekte, yalnızca bir çalışma bir dilimi var.

   ![Dilim eşlemesi diyagramı](./media/data-factory-data-processing-using-batch/image16.png)

8. Bir klasördeki birden çok dosyalarla şimdi deneyin. Dosyaları oluşturmak **file2.txt**, **file3.txt**, **file4.txt**, ve **file5.txt** klasöründekidosya.txt'yiolduğugibiaynıiçeriğesahip**2015-11-06-01**.

9. Çıkış klasörüne çıktı dosyasını silme **2015-11-16-01.txt**.

10. Üzerinde **OutputDataset** dikey penceresinde bir dilimle sağ **DİLİM başlangıç saati** kümesine **11/16/2015 01:00:00 AM**. Seçin **çalıştırmak** dilimi yeniden çalıştırın/yeniden için. Dilim beş dosya yerine bir dosya artık sahiptir.

    ![Çalıştırın](./media/data-factory-data-processing-using-batch/image17.png)

11. Dilim çalıştırır ve durumu sonra **hazır**, bu dilim için çıktı dosyasında içeriği doğrulayın (**2015-11-16-01.txt**). Çıktı dosyası altında görünür `mycontainer` içinde `outputfolder` blob depolama alanınızın içinde. Dilimin her dosya için bir satır olması gerekir.

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file2.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file3.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file4.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file5.txt.
    ```

> [!NOTE]
> Çıkış dosyası 2015-11-16-01.txt beş giriş dosyalarıyla denediniz önce silerseniz alamadık, tek bir çizgi önceki dilim çalıştırma ve geçerli dilimin çalıştırma beş satırları bakın. Zaten varsa, varsayılan olarak, içeriğin çıktı dosyasına eklenir.
>
>

#### <a name="data-factory-and-batch-integration"></a>Veri Fabrikası ve toplu tümleştirmesi
Data Factory hizmetinin bir iş toplu işlemde adıyla oluşturur `adf-poolname:job-xxx`.

![Toplu işler](media/data-factory-data-processing-using-batch/data-factory-batch-jobs.png)

İş bir görevde bir dilim her etkinlik çalıştırması için oluşturulur. 10 dilimler işlenmek üzere hazır olduğunuzda, 10 görevleri işi oluşturulur. Birden çok işlem düğümleri havuzunda varsa paralel olarak çalışan birden fazla dilim olabilir. Görevler işlem düğümü başına en fazla sayısını birden büyük ayarlanmışsa, birden fazla dilim aynı işlem üzerinde çalışabilir.

Bu örnekte, vardır beş dilimler toplu işlemde beş görevler için. İle **eşzamanlılık** kümesine **5** JSON veri fabrikasında ardışık düzeninde ve **maksimum VM başına görevleri** kümesine **2** ileBatchhavuzunda**2** VM'ler, görevleri hızlı çalışır. (Görevler için başlangıç ve bitiş zamanları denetleyin.)

Toplu işlem ve dilimleri ile ilişkili görevleri görüntülemek üzere portalı kullanın ve her dilim çalıştırdı hangi VM bakın.

![Toplu iş görevleri](media/data-factory-data-processing-using-batch/data-factory-batch-job-tasks.png)

### <a name="debug-the-pipeline"></a>Ardışık Düzen hata ayıklama
Hata ayıklama birkaç temel teknikleri oluşur.

1. Girdi dilimi ayarlanmamışsa **hazır**, giriş klasör yapısı doğru olduğunu ve bu dosya.txt'yi giriş klasörlerde var olduğunu doğrulayın.

   ![Giriş klasör yapısı](./media/data-factory-data-processing-using-batch/image3.png)

2. İçinde **yürütme** özel etkinliklerinizi, kullanım yöntemi **IActivityLogger** sorunlarını gidermenize yardımcı olacak bilgileri oturum nesnesi. Günlüğe kaydedilen iletilere kullanıcı görünmesini\_0. günlük dosyası.

   Üzerinde **OutputDataset** dikey penceresinde görmek için dilimi seçin **veri dilimi** dikey penceresinde, dilim için. Altında **etkinlik çalışır**, dilim için bir etkinlik bakın. Seçerseniz **çalıştırmak** komut çubuğunda, aynı dilim için başka bir etkinlik başlatabilirsiniz.

   Etkinliğin çalışma öğesini seçtiğinizde, gördüğünüz **etkinlik çalışma ayrıntıları** günlük dosyalarının listesini içeren dikey. Kullanıcı günlüğe kaydedilen iletilere bakın\_0. günlük dosyası. Hata oluştuğunda, yeniden deneme sayısı 3 JSON ardışık düzeni/etkinliği olarak ayarlandığından, üç Etkinlik çalışması görürsünüz. Etkinlik Çalıştır'ı seçin, hatayı gidermek için gözden geçirebilirsiniz günlük dosyalarına bakın.

   ![OutputDataset ve veri dilimi dikey pencereleri](./media/data-factory-data-processing-using-batch/image18.png)

   Günlük dosyaları listesinde seçin **kullanıcı 0.log**. Sağ bölmede, kullanarak sonuçlarını **IActivityLogger.Write** yöntemi görünür.

   ![Etkinlik ayrıntıları dikey penceresinde çalıştırma](./media/data-factory-data-processing-using-batch/image19.png)

   Sistem hata iletileri ve özel durumlar için sistem 0.log denetleyin.

    ```
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Loading assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Creating an instance of MyDotNetActivityNS.MyDotNetActivity from assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Executing Module
    
    Trace\_T\_D\_12/6/2015 1:43:38 AM\_T\_D\_\_T\_D\_Information\_T\_D\_0\_T\_D\_Activity e3817da0-d843-4c5c-85c6-40ba7424dce2 finished successfully
    ```
3. Dahil **PDB** zip dosyasında bir hata oluştuğunda hata ayrıntılarını çağrı yığını gibi bilgileri böylece dosya.

4. Özel Etkinlik için zip dosyası tüm dosyalarda en üst düzeyinde klasörsüz olması gerekir.

   ![Özel Etkinlik zip dosyası listesi](./media/data-factory-data-processing-using-batch/image20.png)

5. Emin **assemblyName** (MyDotNetActivity.dll) **entryPoint** (MyDotNetActivityNS.MyDotNetActivity) **packageFile** (customactivitycontainer / MyDotNetActivity.zip) ve **packageLinkedService** (zip dosyasını içeren blob depolama alanına işaret etmelidir) doğru değerleri ayarlayın.

6. Sabit bir hata ve dilim yeniden işlemek istiyorsanız, dilimi sağ **OutputDataset** dikey penceresinde ve select **çalıştırmak**.

   ![OutputDataset dikey Çalıştır seçeneği](./media/data-factory-data-processing-using-batch/image21.png)

   > [!NOTE]
   > Adlı blob storage'da kapsayıcıdır `adfjobs`. Bu kapsayıcı otomatik olarak silinmez, ancak çözüm test ediliyor tamamladıktan sonra güvenle silebilirsiniz. Benzer şekilde, veri fabrikası çözüm adlı bir toplu işi oluşturur `adf-\<pool ID/name\>:job-0000000001`. İsterseniz, çözümü test ettikten sonra bu iş silebilirsiniz.
   >
   >
7. Özel Etkinlik kullanmayan **app.config** paketinizi dosyasından. Bu nedenle, kodunuzu yapılandırma dosyasından bağlantı dizelerini yazıyorsa, çalışma zamanında çalışmıyor. Tüm gizli anahtarları Azure anahtar kasası tutmak için Batch hizmetini kullanırken en iyi yöntem olacaktır. Daha sonra anahtar kasası korumak ve Batch havuzu sertifikayı dağıtmak için bir sertifika tabanlı hizmet sorumlusu kullanın. .NET özel etkinlik gizli çalışma zamanında anahtar Kasası'ndan erişebilirsiniz. Bu genel çözüm gizli anahtarı, yalnızca bir bağlantı dizesi herhangi bir türde ölçeklendirebilirsiniz.

    Daha kolay bir geçici çözüm yoktur, ancak en iyi yöntem değildir. Bir SQL veritabanı bağlantılı hizmet ile bağlantı dizesi ayarlarının oluşturabilirsiniz. Ardından bağlantılı hizmet kullanan bir veri kümesi oluşturma ve sahte bir giriş veri kümesi için özel .NET etkinlik olarak dataset zincir. Özel Etkinlik kodu bağlantılı hizmetin bağlantı dizesinde daha sonra erişebilirsiniz. Çalışma zamanında düzgün çalışması gerekir.  

#### <a name="extend-the-sample"></a>Örnek genişletme
Veri Fabrikası ve toplu işlem özellikleri hakkında daha fazla bilgi için bu örnek genişletebilirsiniz. Örneğin, farklı bir zaman aralığı dilimleri işlemek için aşağıdaki adımları uygulayın:

1. Aşağıdaki klasörlerdeki eklemek `inputfolder`: 2015-11-16-05, 2015-11-16-06, 201-11-16-07, 2011-11-16-08 ve 2015-11-16-09. Giriş dosyaları bu klasörlerdeki yerleştirin. Ardışık düzen tarafından bitiş zamanını değiştirin `2015-11-16T05:00:00Z` için `2015-11-16T10:00:00Z`. İçinde **diyagramı** görüntülemek için çift **InputDataset** ve girdi dilimlerinin hazır olduğunu doğrulayın. Çift **OutputDataset** çıkış dilimler durumunu görmek için. İçinde iseniz **hazır** durumu, çıkış dosyaları için çıkış klasörünü denetleyin.

2. Artırma veya azaltma **eşzamanlılık** özellikle toplu olarak oluşan işleme çözümünüzün performansını nasıl etkilediği anlamak için ayarı. Daha fazla bilgi için **eşzamanlılık** ayar bkz. "4. adım: oluşturmak ve ardışık düzen ile özel bir aktivite çalıştırmak."

3. Küçük daha yüksek bir havuz oluşturma **maksimum VM başına görevleri**. Oluşturduğunuz yeni havuzu kullanmak için veri fabrikası çözümüne bağlı Batch hizmeti güncelleştirin. Daha fazla bilgi için **maksimum VM başına görevleri** ayar bkz. "4. adım: oluşturmak ve ardışık düzen ile özel bir aktivite çalıştırmak."

4. İle Batch havuzu oluşturma **otomatik ölçeklendirme** özelliği. Bir Batch havuzunda işlem düğümlerini otomatik olarak ölçeklendirme, uygulamanız tarafından kullanılan güç işleme dinamik ayarlanmasıdır. 

    Formül örneği burada aşağıdaki davranışı elde eder. Havuz başlangıçta oluşturulduğunda, bir VM ile başlar. $PendingTasks ölçüm çalışır görevlerin sayısını tanımlar ve etkin (kuyruğa alınmış) belirtir. Formül Son 180 saniye içinde görevleri bekleyen ortalama sayısı bulur ve TargetDedicated uygun şekilde ayarlar. TargetDedicated hiçbir zaman 25 VM'ler gider sağlar. Yeni görevler gönderildiği haliyle havuzu otomatik olarak artar. Görevleri tam olarak boş bir birer birer VM'ler olur ve bu sanal makineleri otomatik ölçeklendirmeyi küçültür. StartingNumberOfVMs ve maxNumberofVMs gereksinimlerinize göre ayarlayabilirsiniz.
 
    Otomatik ölçeklendirme formülü:

    ``` 
    startingNumberOfVMs = 1;
    maxNumberofVMs = 25;
    pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
    pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
    $TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
    ```

   Daha fazla bilgi için bkz: [otomatik olarak ölçek havuzunda işlem düğümlerini bir toplu](../../batch/batch-automatic-scaling.md).

   Varsayılan havuzu kullanıyorsa, [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), Batch hizmeti VM özel etkinlik çalıştırmadan önce hazırlamak için 15 ila 30 dakika sürebilir. Havuz farklı autoScaleEvaluationInterval kullanıyorsa, Batch hizmeti autoScaleEvaluationInterval artı 10 dakika sürebilir.

5. Örnek çözümde **yürütme** yöntemini çağırır **Hesapla** bir çıktı veri dilimi üretmek için bir giriş veri dilimi işleyen yöntem. Girdi verilerini işlemek ve değiştirmek için kendi yönteminizi yazabilirsiniz **Hesapla** yöntem çağrısı **yürütme** yönteminizi çağrısıyla yöntemi.

### <a name="next-steps-consume-the-data"></a>Sonraki adımlar: verileri kullanmak
Veri işleme sonra Power BI gibi çevrimiçi araçları ile kullanmasını sağlayabilirsiniz. Power BI ve Azure'da kullanma anlamanıza yardımcı olması için bağlantılar şunlardır:

* [Power BI kümesinde keşfedin](https://powerbi.microsoft.com/documentation/powerbi-service-get-data/)
* [Power BI Desktop ile çalışmaya başlama](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/)
* [Power bı'da veri yenileme](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/)
* [Azure ve Power BI: temel genel bakış](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)

## <a name="references"></a>Başvurular
* [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)

  * [Data Factory Hizmeti'ne Giriş](data-factory-introduction.md)
  * [Data Factory ile çalışmaya başlama](data-factory-build-your-first-pipeline.md)
  * [Bir Data Factory işlem hattı özel etkinlikleri kullanmak](data-factory-use-custom-activities.md)
* [Azure Batch](https://azure.microsoft.com/documentation/services/batch/)

  * [Batch temel bilgileri](../../batch/batch-technical-overview.md)
  * [Batch özelliklerine genel bakış](../../batch/batch-api-basics.md)
  * [Azure portalında bir Batch hesabı oluşturabilir ve yönetebilirsiniz](../../batch/batch-account-create-portal.md)
  * [.NET için Batch istemci kitaplığını kullanmaya başlama](../../batch/batch-dotnet-get-started.md)

[batch-explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch-explorer-walkthrough]: http://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx
