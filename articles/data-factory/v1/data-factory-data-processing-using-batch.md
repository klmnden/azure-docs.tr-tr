---
title: "Veri Fabrikası ve toplu işlemi kullanarak büyük ölçekli veri kümeleri işlem | Microsoft Docs"
description: "Azure Batch paralel işleme yeteneğini kullanarak büyük miktarlarda bir Azure Data Factory işlem hattı verileri işlemek açıklar."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 688b964b-51d0-4faa-91a7-26c7e3150868
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2017
ms.author: spelluru
robots: noindex
ms.openlocfilehash: 2ceef65eaf195b605fada2f8dfe511fe33a5daa0
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="process-large-scale-datasets-using-data-factory-and-batch"></a>Data Factory ve Batch kullanarak büyük ölçekli veri kümelerini işleme
> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [Data Factory sürüm 2 özel etkinlikleri](../transform-data-using-dotnet-custom-activity.md).

Bu makalede bir taşır ve büyük ölçekli veri kümeleri otomatik ve zamanlanmış bir şekilde işleyen örnek bir çözüm mimarisini açıklar. Ayrıca, Azure Data Factory ile Azure Batch çözümü uygulamak için bir uçtan uca izlenecek yol da sağlar.

Bu makalede bizim tipik makale uzun çünkü tüm örnek çözümünü bir kılavuz içerir. Bu hizmetler hakkında bilgi edinebilirsiniz Batch ve Data Factory, yeni varsa ve nasıl birlikte çalışır. Hizmetler hakkında bir şey bilmeniz ve tasarlama/çözüm mimarisi oluşturma, yalnızca üzerinde odaklanmanıza [mimarisi bölümüne](#architecture-of-sample-solution) makalenin ve prototip ya da bir çözüm geliştiriyorsanız, ayrıca denemek isteyebilirsiniz adım adım yönergeleri [izlenecek](#implementation-of-sample-solution). Bu içerik ve bunu nasıl kullandığı hakkında yorumlarınızı davet ediyoruz.

İlk olarak, nasıl veri fabrikası ve toplu işlem Hizmetleri ile büyük veri kümelerini bulutta işleme yardımcı olabilir bakalım.     

## <a name="why-azure-batch"></a>Neden Azure toplu işlem?
Azure Batch, büyük ölçekli paralel ve yüksek performanslı bilgi işlem (HPC) uygulamalarını bulutta verimli bir şekilde çalıştırmanızı sağlar. Yönetilen sanal makineler koleksiyonunda çalıştırılacak işlem yoğunluklu işi zamanlayan ve işinizin gereksinimlerini karşılayacak işlem kaynakların otomatik olarak ölçekleyebilen bir platform hizmetidir.

Batch hizmetiyle, uygulamalarınızı paralel olarak ve ölçekte yürütmek için Azure işlem kaynaklarını tanımlayın. İsteğe bağlı veya zamanlanmış işlerinizi çalıştırabilirsiniz; HPC kümesi, tek tek sanal makineler, sanal ağlar veya karmaşık iş ve görev zamanlama altyapısını el ile oluşturmanız, yapılandırmanız ve yönetmeniz gerekmez.

Bu makalede açıklanan çözüm mimarisi/uyarlamasını anlamaya yardımcı olması gibi Azure Batch ile bilmiyorsanız aşağıdaki makalelere bakın.   

* [Azure Batch temel bilgileri](../../batch/batch-technical-overview.md)
* [Batch özelliklerine genel bakış](../../batch/batch-api-basics.md)

(isteğe bağlı) Azure Batch hakkında daha fazla bilgi için bkz: [için Azure Batch öğrenme yolu](https://azure.microsoft.com/documentation/learning-paths/batch/).

## <a name="why-azure-data-factory"></a>Neden Azure Data Factory?
Data Factory, verilerin taşınmasını ve dönüştürülmesini düzenleyen ve otomatikleştiren bulut tabanlı bir veri tümleştirme hizmetidir. Data Factory hizmeti kullanıldığında, şirket içi veri taşıma ve bulut merkezi veri deposuna veri depolarına yönetilen veri ardışık oluşturabilirsiniz (örneğin: Azure Blob Storage) ve işlem/dönüştürme Azure Hdınsight ve Azure gibi hizmetleri kullanarak verileri Machine Learning. Veri ardışık zamanlanmış şekilde (saatlik, günlük, haftalık, vb.) ve İzleyicisi'nde çalıştırın ve sorunları belirlemek ve eylem için bir bakışta yönetmek için de zamanlayabilirsiniz.

Bu makalede açıklanan çözüm mimarisi/uyarlamasını anlamaya yardımcı olması gibi Azure Data Factory ile bilmiyorsanız aşağıdaki makalelere bakın.  

* [Azure Data Factory giriş](data-factory-introduction.md)
* [İlk veri hattınızı oluşturma](data-factory-build-your-first-pipeline.md)   

(isteğe bağlı) Azure Data Factory hakkında daha fazla bilgi için bkz: [Azure Data Factory öğrenme yolu](https://azure.microsoft.com/documentation/learning-paths/data-factory/).

## <a name="data-factory-and-batch-together"></a>Veri Fabrikası ve birlikte toplu işlem
Veri Fabrikası hedef veri deposu kaynak veri deposuna ve Hive etkinliği Azure üzerinde Hadoop kümeleri (Hdınsight) kullanarak verileri işlemek için copy/move veri kopyalama etkinliği gibi yerleşik etkinlikler içerir. Bkz: [veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md) desteklenen dönüştürme etkinliklerinin listesi.

Ayrıca, taşımak veya kendi mantığı ile verileri işlemek ve bu etkinlikler Azure Hdınsight kümesinde veya bir Azure Batch havuzunda VM'lerin çalıştırmak için özel .NET etkinlikler oluşturmanıza olanak sağlar. Azure Batch kullandığınızda otomatik ölçek havuzuna yapılandırabilirsiniz (ekleme veya iş yüküne göre sanal makineleri kaldırın) sağladığınız bir formüle dayanarak.     

## <a name="architecture-of-sample-solution"></a>Örnek çözümü mimarisi
Bu makalede açıklanan mimarisi için basit bir çözüm olsa da, finansal hizmetler, görüntü işleme ve işleme ve genomic analiz tarafından modelleme risk gibi karmaşık senaryolar için geçerlidir.

Aşağıdaki diyagramda, 1) nasıl Data Factory veri hareketlerini ve işleme düzenleyen ve Azure Batch verileri paralel bir biçimde 2) nasıl işlediği açıklanmıştır. Karşıdan yükle ve kolay başvuru (11 x 17 inç. diyagramı yazdırma veya A3 boyutu): [Azure Batch ve Data Factory kullanarak HPC ve veriler orchestration](http://go.microsoft.com/fwlink/?LinkId=717686).

[![Büyük ölçekli veri işleme diyagramı](./media/data-factory-data-processing-using-batch/image1.png)](http://go.microsoft.com/fwlink/?LinkId=717686)

Aşağıdaki listede işleminin temel adımları sağlar. Çözüm, kod ve uçtan uca çözümü oluşturmak için açıklamalar içerir.

1. **Azure Batch (VM'ler) işlem düğümlerinin bir havuzuyla yapılandırma**. Düğüm sayısını ve her düğümün boyutu belirtebilirsiniz.
2. **Azure Data Factory örneğini oluşturabilir** Azure blob depolama, Azure toplu işlem hizmeti, girdi/çıktı verilerini ve iş akışı/ardışık taşıyın ve veri dönüştürme etkinlikleri ile temsil eden varlık ile yapılandırılmış.
3. **Özel bir .NET etkinliği içinde Data Factory işlem hattı oluşturma**. Azure Batch havuzunda çalıştıran kullanıcı kodunuzu etkinliktir.
4. **Azure depolama alanında bloblar büyük miktarlarda giriş veri depolamak**. Veriler (genellikle zamanına göre) mantıksal dilimlere bölünür.
5. **Veri Fabrikası paralel olarak işlenir verileri kopyalar** ikincil konuma.
6. **Veri Fabrikası çalışan toplu işi tarafından ayrılan havuzunu kullanan özel etkinlik**. Veri Fabrikası etkinlikleri birlikte çalışabilir. Her etkinlik veri dilimini işler. Sonuçları Azure depolama alanında depolanır.
7. **Veri Fabrikası son sonuçları üçüncü bir konuma taşır**, bir uygulama aracılığıyla dağıtım için veya diğer araçları tarafından işlenmesi için.

## <a name="implementation-of-sample-solution"></a>Örnek çözümü uygulaması
Örnek çözümü kasıtlı olarak basit bir işlemdir ve Data Factory ve toplu birlikte veri kümeleri işlemek için nasıl kullanılacağını gösterir. Çözüm yalnızca bir arama terimi ("Microsoft") oluşumu bir zaman serisinin düzenlenmiş giriş dosyalarında sayar. Çıkış dosyaları için sayısı çıkarır.

**Zaman**: Azure Data Factory ve Batch temelleri ile tanıdık ve aşağıda listelenen önkoşulları tamamladığınızdan varsa, bu çözüm tamamlanması 1-2 saat sürer tahmin ediyoruz.

### <a name="prerequisites"></a>Ön koşullar
#### <a name="azure-subscription"></a>Azure aboneliği
Bir Azure aboneliğiniz yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Bkz: [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).

#### <a name="azure-storage-account"></a>Azure depolama hesabı
Bu öğreticide verileri depolamak için bir Azure depolama hesabı kullanın. Bir Azure depolama hesabınız yoksa bkz [depolama hesabı oluşturma](../../storage/common/storage-create-storage-account.md#create-a-storage-account). Örnek çözümü blob depolama kullanır.

#### <a name="azure-batch-account"></a>Azure toplu işlem hesabı
Bir Azure Batch hesabı kullanarak oluşturduğunuz [Azure portal](http://portal.azure.com/). Bkz: [oluşturma ve bir Azure Batch hesabını yönetmek](../../batch/batch-account-create-portal.md). Azure Batch hesabı adını ve hesap anahtarını unutmayın. Aynı zamanda [New-AzureRmBatchAccount](https://msdn.microsoft.com/library/mt603749.aspx) bir Azure Batch hesabı oluşturmak için cmdlet'i. Bkz: [Azure Batch PowerShell cmdlet'leri kullanmaya başlama](../../batch/batch-powershell-cmdlets-get-started.md) bu cmdlet'in kullanımı hakkında ayrıntılı yönergeler için.

Örnek çözümü (üzerinden dolaylı olarak bir Azure Data Factory işlem hattı) Azure Batch havuzunda işlem düğümlerinin (sanal makineler yönetilen koleksiyonu) paralel şekilde veri işlemek için kullanır.

#### <a name="azure-batch-pool-of-virtual-machines-vms"></a>Azure Batch havuzundaki sanal makineler (VM'ler)
Oluşturma bir **Azure Batch havuzu** en az 2 ile işlem düğümleri.

1. İçinde [Azure portal](https://portal.azure.com), tıklatın **Gözat** sol menüsüne ve ardından içinde **toplu işlem hesaplarını**.
2. Açmak için Azure Batch hesabınızı seçin **toplu işlem hesabı** dikey.
3. Tıklatın **havuzları** döşeme.
4. İçinde **havuzları** dikey penceresinde, araç çubuğunda bir havuzu eklemek için Ekle düğmesini tıklatın.
   1. Havuzu için bir kimlik girin (**havuzu kimliği**). Not **havuzun kimliği**; Data Factory çözüm oluşturulurken gerekir.
   2. Belirtin **Windows Server 2012 R2** işletim sistemi ailesi ayarı için.
   3. Seçin bir **düğüm fiyatlandırma katmanı**.
   4. Girin **2** olarak değer **hedef ayrılmış** ayarı.
   5. Girin **2** olarak değer **en fazla düğüm başına görevleri** ayarı.
   6. Havuzu oluşturmak için **Tamam**'a tıklayın.

#### <a name="azure-storage-explorer"></a>Azure Depolama Gezgini
[Azure Depolama Gezgini 6 (aracı)](https://azurestorageexplorer.codeplex.com/) veya [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) (ClumsyLeaf yazılımından). İnceleme veya bulutta barındırılan uygulamalarınızı günlükleri de dahil olmak üzere Azure Storage projelerinizi verileri değiştirme için bu araçları kullanın.

1. Adlı bir kapsayıcı oluşturmak **mycontainer** özel erişim (anonim erişimi yok)
2. Kullanıyorsanız **CloudXplorer**, klasörler ve alt klasörler ile aşağıdaki yapısını oluşturun:

   ![](./media/data-factory-data-processing-using-batch/image3.png)

   `Inputfolder`ve `outputfolder` en üst düzey klasörlerde bulunan `mycontainer`. `inputfolder` Tarih-saat Damgalar (YYYY-AA-GG-ss) ile klasörleri varsa.

   Kullanıyorsanız **Azure Storage Gezgini**, sonraki adımda adlara sahip dosyaları karşıya yüklemek gerekir: `inputfolder/2015-11-16-00/file.txt`, `inputfolder/2015-11-16-01/file.txt` ve benzeri. Bu adım, klasörleri otomatik olarak oluşturur.
3. Bir metin dosyası oluşturun **dosya.txt'yi** anahtar sözcüğü sahip içerikle makinenizde **Microsoft**. Örneğin: "Özel Etkinlik Microsoft test özel etkinlik Microsoft test".
4. Azure blob depolama alanındaki aşağıdaki giriş klasörlere dosyası yükleyin.

   ![](./media/data-factory-data-processing-using-batch/image4.png)

   Kullanıyorsanız **Azure Storage Gezgini**, dosyayı karşıya yüklemeyi **dosya.txt'yi** için **mycontainer**. Tıklatın **kopyalama** blob bir kopyasını oluşturmak için araç çubuğunda. İçinde **kopyalama Blob** iletişim kutusu, değişiklik **hedef blob adı** için `inputfolder/2015-11-16-00/file.txt`. Oluşturmak için bu adımı yineleyin `inputfolder/2015-11-16-01/file.txt`, `inputfolder/2015-11-16-02/file.txt`, `inputfolder/2015-11-16-03/file.txt`, `inputfolder/2015-11-16-04/file.txt` ve benzeri. Bu eylem, klasörleri otomatik olarak oluşturur.
5. Adlı başka bir kapsayıcı oluşturma: `customactivitycontainer`. Bu kapsayıcıya özel etkinlik zip dosyasını karşıya yükleyin.

#### <a name="visual-studio"></a>Visual Studio
Veri Fabrikası çözümde kullanılacak özel toplu iş etkinliği oluşturmak için Microsoft Visual Studio 2012 veya sonraki sürümünü yükleyin.

### <a name="high-level-steps-to-create-the-solution"></a>Çözüm oluşturmak için üst düzey adımlar
1. Veri işleme mantığı içeren özel bir aktivite oluşturun.
2. Özel Etkinlik kullanan bir Azure data factory oluşturun:

### <a name="create-the-custom-activity"></a>Özel etkinlik oluşturma
Veri Fabrikası özel Bu örnek çözümü Kalp etkinliktir. Örnek çözümü Özel Etkinlik çalıştırmak için Azure Batch kullanır. Bkz: [bir Azure Data Factory ardışık düzeninde özel etkinlikleri kullanmak](data-factory-use-custom-activities.md) özel etkinlikler geliştirmek ve bunları Azure Data Factory ardışık düzenlerinde temel bilgi.

Oluşturmanıza gerek bir Azure Data Factory ardışık düzeninde kullanabileceğiniz bir .NET özel etkinlik oluşturmak için bir **.NET sınıf kitaplığı** uygulayan bir sınıf projeyle **IDotNetActivity** arabirimi. Bu arabirim yalnızca bir yöntemi vardır: **yürütme**. Yöntem imzası şöyledir:

```csharp
public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices,
            IEnumerable<Dataset> datasets,
            Activity activity,
            IActivityLogger logger)
```

Yöntem anlamanız için gereken birkaç anahtar bileşenleri içerir.

* Yöntemi, dört parametreleri alır:

  1. **linkedServices**. Giriş/Çıkış veri kaynakları bağlantı bağlı hizmetler numaralandırılabilir bir listesini (örneğin: Azure Blob Storage) veri fabrikası için. Bu örnekte, yalnızca bir bağlantılı hizmet türü girdi ve çıktı için kullanılan Azure Storage yok.
  2. **veri kümeleri**. Bu veri kümeleri numaralandırılabilir bir listesidir. Giriş ve çıkış veri kümeleri tarafından tanımlanan şemaları ve konumlarını almak için bu parametreyi kullanın.
  3. **Etkinlik**. Bu parametre, geçerli işlem varlık - bu durumda, bir Azure Batch hizmetini temsil eder.
  4. **Günlükçü**. Günlükçü ardışık düzeni için "Kullanıcı" günlük olarak o yüzeyini hata ayıklama açıklamaları yazmanızı sağlar.
* Bu yöntem, özel etkinlikler gelecekte zincir için kullanılan bir sözlüğü döndürür. Bu özellik henüz uygulanmadı, bu nedenle boş bir sözlük döndürme.

#### <a name="procedure-create-the-custom-activity"></a>Yordam: özel etkinlik oluşturma
1. Visual Studio'da .NET sınıf kitaplığı proje oluşturun.

   1. Başlatma **Visual Studio 2012**/**2013/2015**.
   2. **Dosya**’ya tıklayın, **Yeni**’nin üzerine gelin ve **Proje**’ye tıklayın.
   3. Genişletme **şablonları**seçip **Visual C\#**. Bu kılavuzda, kullandığınız C\#, ancak özel etkinlik geliştirmek için herhangi bir .NET dil kullanabilirsiniz.
   4. Seçin **sınıf kitaplığı** proje türleri sağdaki listeden.
   5. Girin **MyDotNetActivity** için **adı**.
   6. Seçin **C:\\ADF** için **konumu**. Bir klasör oluşturun **ADF** henüz yoksa.
   7. Projeyi oluşturmak için **Tamam**'a tıklayın.
2. **Araçlar**'a tıklayın, **NuGet Paket Yöneticisi**'nin üzerine gelin ve ardından **Paket Yöneticisi Konsolu**'na tıklayın.
3. İçinde **Paket Yöneticisi Konsolu**, içeri aktarmak için aşağıdaki komutu yürütün **Microsoft.Azure.Management.DataFactories**.

    ```powershell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. İçeri aktarma **Azure Storage** projesi için NuGet paketi. Bu örnek Blob storage'da API kullandığından, bu paketi gerekir.

    ```powershell
    Install-Package Azure.Storage
    ```
5. Aşağıdakileri ekleyin **kullanarak** projenin kaynak dosyasında yönergeleri.

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
6. Adını değiştirmek **ad alanı** için **MyDotNetActivityNS**.

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. Sınıfın adını değiştirmek **MyDotNetActivity** ve buradan türetebilir **IDotNetActivity** arabirim aşağıda gösterildiği gibi.

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. Uygulama (Ekle) **yürütme** yöntemi **IDotNetActivity** arabirimini **MyDotNetActivity** sınıfı ve aşağıdaki örnek kod yöntemi kopyalayabilirsiniz. Bkz: [yöntemi Çalıştır](#execute-method) Bu yöntemde kullanılan mantığı için bir açıklama için bölüm.

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
    
       // declare types for input and output data stores
       AzureStorageLinkedService inputLinkedService;
    
       Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
       foreach (LinkedService ls in linkedServices)
           logger.Write("linkedService.Name {0}", ls.Name);
    
       // using First method instead of Single since we are using the same
       // Azure Storage linked service for input and output.
       inputLinkedService = linkedServices.First(
           linkedService =>
           linkedService.Name ==
           inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
           as AzureStorageLinkedService;
    
       string connectionString = inputLinkedService.ConnectionString; // To create an input storage client.
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
           // with the data slice.
           //
           // definition of the method is shown in the next step.
           output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
       } while (continuationToken != null);
    
       // get the output dataset using the name of the dataset matched to a name in the Activity output collection.
       Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    
       folderPath = GetFolderPath(outputDataset);
    
       logger.Write("Writing blob to the folder: {0}", folderPath);
    
       // create a storage object for the output blob.
       CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
       // write the name of the file.
       Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
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
9. Aşağıdaki yardımcı yöntemler sınıfına ekleyin. Bu yöntemler tarafından çağrılan **yürütme** yöntemi. En önemlisi, **Hesapla** yöntemi her bir blob tekrarlanan kod yalıtır.

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
    **GetFolderPath** yöntemi dataset işaret klasörünün yolunu döndürür ve **GetFileName** yöntemi dataset işaret blob/dosya adını döndürür.

    ```csharp

    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
    ```

    **Hesapla** yöntemi hesaplar anahtar sözcüğü örnek sayısı **Microsoft** giriş dosyalarında (BLOB klasöründe). Arama terimi kodda sabit kodlanmış ("Microsoft").

1. Projeyi derleyin. Tıklatın **yapı** tıklatın ve menüden **yapı çözümü**.
2. Başlatma **Windows Explorer**ve gidin **bin\\hata ayıklama** veya **bin\\yayın** klasörü yapı türüne bağlı olarak.
3. Zip dosyası oluşturun **MyDotNetActivity.zip** içindeki tüm ikili dosyaları içeren  **\\bin\\hata ayıklama** klasör. MyDotNetActivity eklemek isteyebilirsiniz. **pdb** bir hata oluştuğunda, soruna neden kaynak kodunda satır numarası gibi ek ayrıntıları almak için dosya.

   ![](./media/data-factory-data-processing-using-batch/image5.png)
4. Karşıya yükleme **MyDotNetActivity.zip** blob kapsayıcıya bir BLOB: `customactivitycontainer` Azure blob depolamada, **StorageLinkedService** bağlı hizmetinde  **ADFTutorialDataFactory** kullanır. Blob kapsayıcı oluşturun `customactivitycontainer` zaten yoksa.

#### <a name="execute-method"></a>Execute yöntemi
Bu bölümde daha fazla ayrıntı ve yürütme yönteminin kodda hakkında notlar sağlanmaktadır.

1. Giriş koleksiyonu yineleme için üyeleri bulunan [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) ad alanı. Blob koleksiyonu yineleme yapma kullanılması **BlobContinuationToken** sınıfı. Esas olarak, bir kullanın-while döngüsünü döngüden çıkma mekanizması olarak belirteci ile. Daha fazla bilgi için bkz: [Blob storage kullanma konusunda](../../storage/blobs/storage-dotnet-how-to-use-blobs.md). Temel bir döngü burada gösterilir:

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
   Belgelerine bakın [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) Ayrıntılar için yöntem.
2. BLOB'ları kümesi aracılığıyla mantıksal olarak çalışmak için kod içinde do gider-while döngüsünü. İçinde **yürütme** yöntemi, do-döngü BLOB'ları listesi adlı bir yönteme geçirir **Hesapla**. Adlı bir dize değişkeni yöntemi döndürür **çıkış** kesimindeki tüm BLOB'lar aracılığıyla yinelendiğinde başka bir deyişle sonucu.

   Arama terimi oluşumları sayısını döndürür (**Microsoft**) için geçirilen blob'daki **Hesapla** yöntemi.

    ```csharp
    output += string.Format("{0} occurrences of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
    ```
3. Bir kez **Hesapla** yöntemi iş Bitti, bunu yeni bir blob için yazılmış olmalıdır. Bu nedenle her işlenen BLOB'lar kümesi için yeni blob sonuçlarıyla yazılabilir. Yeni bir blob yazmak için ilk çıkış veri kümesi bulun.

    ```csharp
    // Get the output dataset using the name of the dataset matched to a name in the Activity output collection.
    Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    ```
4. Kod ayrıca yardımcı yöntemini çağırır: **GetFolderPath** klasör yolu (depolama kapsayıcısı adı) alınamadı.

    ```csharp
    folderPath = GetFolderPath(outputDataset);
    ```
   **GetFolderPath** FolderPath adlı bir özelliği olan bir AzureBlobDataSet DataSet nesnesine çevirir.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FolderPath;
    ```
5. Kod çağrıları **GetFileName** dosya adı (blob) alma yöntemi. Klasör yolu almak için yukarıdaki kod için kod benzer.

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FileName;
    ```
6. Dosyanın adını bir URI nesnesinden oluşturarak yazılır. URI Oluşturucusu kullanan **BlobEndpoint** kapsayıcı adını döndürmek için özellik. Klasör yolunu ve dosya adı, çıktı blob URI'si oluşturmak eklenir.  

    ```csharp
    // Write the name of the file.
    Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    ```
7. Dosyanın adını yazılmış ve şimdi çıktı dizeden yazabilirsiniz **Hesapla** yöntemi yeni bir blob için:

    ```csharp
    // Create a blob and upload the output text.
    CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
    logger.Write("Writing {0} to the output blob", output);
    outputBlob.UploadText(output);
    ```

### <a name="create-the-data-factory"></a>Veri Fabrikası oluşturma
İçinde [özel etkinlik oluşturmak](#create-the-custom-activity) bölüm, özel bir aktivite oluşturulur ve bir Azure blob kapsayıcısına ikili dosyaları zip dosyasıyla ve PDB dosya karşıya. Bu bölümde, oluşturduğunuz Azure **veri fabrikası** ile bir **ardışık düzen** kullanan **özel etkinlik**.

Özel Etkinlik için giriş veri kümesi (dosyaları) BLOB giriş klasöründe temsil eder (`mycontainer\\inputfolder`) blob depolama. Çıktı veri kümesi etkinliğinin çıkış klasöründe çıkış BLOB'ları temsil eder (`mycontainer\\outputfolder`) blob depolama.

Bir veya daha fazla giriş klasörlerde bırak:

```
mycontainer -\> inputfolder
    2015-11-16-00
    2015-11-16-01
    2015-11-16-02
    2015-11-16-03
    2015-11-16-04
```

Örneğin, bir dosya (dosya.txt'yi) aşağıdaki içerik ile her klasörler bırakın.

```
test custom activity Microsoft test custom activity Microsoft
```

2 veya daha fazla dosya klasör sahip olsa bile her giriş klasörü Azure Data factory'de bir dilim karşılık gelir. Her dilimi ardışık düzen tarafından işlendiğinde, özel etkinlik tüm BLOB'ları, dilim için giriş klasörü dolaşır.

Aynı içeriğe sahip beş çıktı dosyalarına bakın. Örneğin, 2015-11-16-00 klasöründeki dosyaya işlemesini çıktı dosyası aşağıdaki içeriğe sahip:

```
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
```

Giriş klasörü aynı içeriğe sahip birden çok dosya (dosya.txt'yi, file2.txt, file3.txt) bırakma, çıktı dosyasında aşağıdaki içeriğe bakın. Her bir klasör (2015-11-16-00, vb.) birden çok giriş dosyaları klasörü olmasına rağmen bir dilim bu örnekteki karşılık gelir.

```csharp
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file2.txt.
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file3.txt.
```

Çıktı dosyası üç satır artık, her girdi dosyasında (blob) (2015-11-16-00) dilimle ilişkili klasörü için bir tane var.

Bir görev çalıştırmak her etkinlik için oluşturulur. Bu örnekte, ardışık düzeninde yalnızca bir etkinlik yok. Bir dilim ardışık düzen tarafından işlendiğinde, özel etkinlik dilim işlemek için Azure Batch üzerinde çalışır. Beş dilimleri (her dilim birden çok BLOB veya dosya olabilir) olduğundan, Azure Batch oluşturulan beş görevler vardır. Bir görev toplu olarak çalıştığında, gerçekte çalıştıran özel etkinlik olduğu.

Aşağıdaki örneklerde ek ayrıntılar sağlar.

#### <a name="step-1-create-the-data-factory"></a>1. adım: veri fabrikası oluşturma
1. Oturum açtıktan sonra [Azure portal](https://portal.azure.com/), aşağıdaki adımları uygulayın:

   1. Sol menüde **YENİ**’ye tıklayın.
   2. Tıklatın **veri + analiz** içinde **yeni** dikey.
   3. **Veri analizi** dikey penceresinde **Data Factory**’ye tıklayın.
2. İçinde **yeni data factory** dikey penceresinde girin **CustomActivityFactory** adı. Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Hatayı alırsanız: **veri fabrikası adı "CustomActivityFactory" kullanılabilir değil**, data factory adını değiştirin (örneğin, **yournameCustomActivityFactory**) ve oluşturmayı yeniden deneyin.
3. Tıklatın **kaynak grubu adı**, varolan bir kaynak grubu seçin veya bir kaynak grubu oluşturun.
4. Oluşturulacak data factory bölgesini ve doğru aboneliğin kullandığınızdan emin olun.
5. **Yeni data factory** dikey penceresinde **Oluştur**’a tıklayın.
6. Oluşturulmakta veri fabrikası gördüğünüz **Pano** Azure portalının.
7. Data factory sorunsuz oluşturulduktan sonra data factory sayfasını görürsünüz, burada size data factory içeriği gösterilir.

   ![](./media/data-factory-data-processing-using-batch/image6.png)

#### <a name="step-2-create-linked-services"></a>2. adım: bağlı hizmetler oluşturma
Bağlı hizmetler veri depolarını veya işlem hizmetlerini Azure data factory’ye bağlar. Bu adımda, bağlantı, **Azure Storage** hesabı ve **Azure Batch** hesabınızı data factory'nize.

#### <a name="create-azure-storage-linked-service"></a>Azure Storage bağlı hizmeti oluşturma
1. Tıklatın **yazar ve dağıtma** döşemesinin **DATA FACTORY** dikey **CustomActivityFactory**. Data Factory Düzenleyici bakın.
2. Tıklatın **yeni veri deposu** komut çubuğu ve seçin **Azure depolama.** Düzenleyicide Azure Storage bağlı hizmeti oluşturmak için JSON betiğini görmeniz gerekir.

   ![](./media/data-factory-data-processing-using-batch/image7.png)

3. **accountname** sözcüğünü Azure depolama hesabınızın adıyla, **accountkey** sözcüğünü de Azure depolama hesabının erişim anahtarıyla değiştirin. Depolama erişim anahtarınızı nasıl alacağınız hakkında bilgi için bkz. [Depolama erişim anahtarlarını görüntüleme, kopyalama ve yeniden oluşturma](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).

4. Bağlı hizmeti dağıtmak için komut çubuğunda **Dağıt**’a tıklayın.

   ![](./media/data-factory-data-processing-using-batch/image8.png)

#### <a name="create-azure-batch-linked-service"></a>Azure Batch bağlı hizmet oluşturma
Bu adımda oluşturduğunuz için bağlı hizmet, **Azure Batch** Data Factory Özel Etkinlik çalıştırmak için kullanılan hesap.

1. Tıklatın **yeni işlem** komut çubuğu ve seçin **Azure Batch.** Düzenleyicide Azure Batch bağlı hizmeti oluşturmak için JSON betiğini görmeniz gerekir.
2. JSON betiği:

   1. Değiştir **hesap adı** Azure Batch hesabınızın adı.
   2. Değiştir **erişim tuşu** Azure Batch hesabının erişim anahtarı ile.
   3. İçin havuz Kimliğini girin **poolName** özelliği**.** Bu özellik için ya da havuz adı belirtin veya Havuz kimliği.
   4. URI toplu girmek için **batchUri** JSON özelliği.

      > [!IMPORTANT]
      > **URL** gelen **Azure Batch hesabı dikey** aşağıdaki biçimdedir: \<accountname\>.\< Bölge\>. batch.azure.com. İçin **batchUri** özelliği JSON içinde gereken **"accountname." Kaldır** URL. Örnek: `"batchUri": "https://eastus.batch.azure.com"`.
      >
      >

      ![](./media/data-factory-data-processing-using-batch/image9.png)

      İçin **poolName** özelliği, havuzu havuzunun adı yerine Kimliğini de belirtebilirsiniz.

      > [!NOTE]
      > Hdınsight için yaptığı gibi Data Factory hizmeti Azure toplu işlem için bir isteğe bağlı seçeneği desteklemiyor. Bu gibi durumlarda, kendi Azure Batch havuzu yalnızca bir Azure data factory kullanabilirsiniz.
      >
      >
   5. Belirtin **StorageLinkedService** için **linkedServiceName** özelliği. Bu bağlı hizmeti önceki adımda oluşturduğunuz. Bu depolama dosyaları ve günlükleri için hazırlama alanı kullanılır.
3. Bağlı hizmeti dağıtmak için komut çubuğunda **Dağıt**’a tıklayın.

#### <a name="step-3-create-datasets"></a>3. adım: veri kümeleri oluşturma
Bu adımda, girdi ve çıktı verilerini temsil edecek veri kümeleri oluşturma.

#### <a name="create-input-dataset"></a>Girdi veri kümesi oluşturma
1. İçinde **Düzenleyicisi** veri fabrikası için tıklatın **yeni veri kümesi** düğmesini tıklatın ve araç **Azure Blob Depolama** açılır menüsünden.
2. Sağ bölmedeki JSON aşağıdaki JSON parçacığıyla değiştirin:

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

    Başlangıç saati ile bu kılavuzda daha sonra bir işlem hattı oluşturma: 2015-11-16T00:00:00Z ve bitiş zamanı: 2015-11-16T05:00:00Z. Veri üretmek için zamanlanmış **saatlik**, 5 giriş/çıkış dilimler olduklarından (arasında **00**: 00:00 -\> **05**: 00:00).

    **Sıklığı** ve **aralığı** girdi veri kümesi ayarlanmıştır **saat** ve **1**, girdi dilimi saatlik kullanılabilir olduğunu anlamına gelir.

    Tarafından temsil edilen her dilim için başlangıç zamanlarını işte **SliceStart** yukarıdaki JSON parçacığında bulunan sistem değişkeni.

    | **Dilim** | **Başlangıç zamanı**          |
    |-----------|-------------------------|
    | 1         | 2015 11 16T**00**: 00:00 |
    | 2         | 2015 11 16T**01**: 00:00 |
    | 3         | 2015 11 16T**02**: 00:00 |
    | 4         | 2015 11 16T**03**: 00:00 |
    | 5         | 2015 11 16T**04**: 00:00 |

    **FolderPath** dilim başlangıç zamanı yıl, ay, gün ve saat parçası kullanılarak hesaplanır (**SliceStart**). Bu nedenle, işte bir giriş klasörü için bir dilim nasıl eşlendi.

    | **Dilim** | **Başlangıç zamanı**          | **Giriş klasörü**  |
    |-----------|-------------------------|-------------------|
    | 1         | 2015 11 16T**00**: 00:00 | 2015-11-16-**00** |
    | 2         | 2015 11 16T**01**: 00:00 | 2015-11-16-**01** |
    | 3         | 2015 11 16T**02**: 00:00 | 2015-11-16-**02** |
    | 4         | 2015 11 16T**03**: 00:00 | 2015-11-16-**03** |
    | 5         | 2015 11 16T**04**: 00:00 | 2015-11-16-**04** |

1. Tıklatın **dağıtma** oluşturmak ve dağıtmak için araç çubuğunda **InputDataset** tablo.

#### <a name="create-output-dataset"></a>Çıktı veri kümesi oluşturma
Bu adımda, çıktı verilerini göstermek için AzureBlob türünde başka bir veri kümesi oluşturun.

1. İçinde **Düzenleyicisi** veri fabrikası için tıklatın **yeni veri kümesi** düğmesini tıklatın ve araç **Azure Blob Depolama** açılır menüsünden.
2. Sağ bölmedeki JSON aşağıdaki JSON parçacığıyla değiştirin:

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

    Bir çıkış blob/dosyası, her girdi dilimi için oluşturulur. İşte bir çıktı dosyası için her dilimi nasıl adlandırılır. Bir çıkış klasöründe oluşturulan tüm çıktı dosyaları: `mycontainer\\outputfolder`.

    | **Dilim** | **Başlangıç zamanı**          | **Çıkış dosyası**       |
    |-----------|-------------------------|-----------------------|
    | 1         | 2015 11 16T**00**: 00:00 | 2015-11-16 -**00. txt** |
    | 2         | 2015 11 16T**01**: 00:00 | 2015-11-16 -**01. txt** |
    | 3         | 2015 11 16T**02**: 00:00 | 2015-11-16 -**02. txt** |
    | 4         | 2015 11 16T**03**: 00:00 | 2015-11-16 -**03. txt** |
    | 5         | 2015 11 16T**04**: 00:00 | 2015-11-16 -**04. txt** |

    Unutmayın Giriş bir klasördeki tüm dosyalar (örneğin: 2015-11-16-00) bir dilim başlangıç saatine sahip bir parçasıdır: 2015-11-16-00. Bu dilim işlendiğinde özel etkinlik her dosyası aracılığıyla tarar ve arama terimi ("Microsoft"), yineleme sayısı ile çıkış dosyasındaki bir satır üretir. 2015-11-16-00 klasöründe üç dosya varsa, üç satırları vardır çıktı dosyasına: 2015-11-16-00.txt.

1. Tıklatın **dağıtma** oluşturmak ve dağıtmak için araç çubuğunda **OutputDataset**.

#### <a name="step-4-create-and-run-the-pipeline-with-custom-activity"></a>4. adım: Oluşturma ve özel etkinliği ile işlem hattı çalıştırma
Bu adımda, bir etkinlik, daha önce oluşturduğunuz özel etkinliği ile işlem hattı oluşturun.

> [!IMPORTANT]
> Karşıya henüz yüklediyseniz **dosya.txt'yi** klasörleri blob kapsayıcısında giriş için işlem hattı oluşturmadan önce bunu. **İsPaused** özelliği ayarlanmış JSON, işlem hattındaki false ardışık hemen nedenle olarak **Başlat** geçmişte tarihidir.
>
>

1. Data Factory Düzenleyici'yi tıklatın **yeni işlem hattı** komut çubuğunda. Komut görmüyorsanız tıklatın **... (Üç nokta)**  görmek için.
2. Sağ bölmedeki JSON aşağıdaki JSON betiği ile değiştirin:

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

   * Ardışık düzeninde yalnızca bir etkinlik vardır ve bu tür: **DotNetActivity**.
   * **AssemblyName** DLL adına ayarlayın: **MyDotNetActivity.dll**.
   * **EntryPoint** ayarlanır **MyDotNetActivityNS.MyDotNetActivity**. Temelde olan \<ad alanı\>.\< ClassName\> kodunuzda.
   * **PackageLinkedService** ayarlanır **StorageLinkedService** özel etkinlik zip dosyasını içeren blob depolama alanına işaret eder. Giriş/çıkış dosyaları ve özel etkinlik zip dosyası için farklı Azure depolama hesapları kullanıyorsanız, başka bir Azure Storage bağlı hizmeti oluşturmanız gerekir. Bu makalede, aynı Azure Storage hesabını kullandığınızı varsayar.
   * **PackageFile** ayarlanır **customactivitycontainer/MyDotNetActivity.zip**. Şu biçimdedir: \<containerforthezip\>/\<nameofthezip.zip\>.
   * Özel Etkinlik alır **InputDataset** giriş olarak ve **OutputDataset** çıktı olarak.
   * **LinkedServiceName** özel etkinlik özelliğinin işaret **AzureBatchLinkedService**, özel etkinlik Azure toplu olarak çalıştırmak için gereken bir Azure Data Factory söyler.
   * **Eşzamanlılık** ayarı önemlidir. 1, 2 veya daha fazla işlem düğümleri Azure Batch havuzunda olsa dahi, varsayılan değeri kullanırsanız dilimleri işlenir birbiri ardından. Bu nedenle, Azure batch paralel işleme yeteneğinden olmuyor. Ayarlarsanız **eşzamanlılık** daha yüksek bir değere deyin 2, bu iki dilimler anlamına gelir (Azure Batch iki görevlere karşılık gelir) aynı anda; bu durumda VM'ler havuzu kullanıldığı Azure Batch içinde işlenir. Bu nedenle, eşzamanlılık özelliğini uygun şekilde ayarlayın.
   * Yalnızca bir görev (dilim) varsayılan olarak, varsayılan olarak herhangi bir noktada bir VM üzerinde yürütülür. , Varsayılan olarak, bir nedeni **maksimum VM başına görevleri** için bir Azure Batch havuzu 1 ayarlayın. Önkoşullar bir parçası olarak, bu özelliği 2'ye iki Data Factory dilimler bir VM üzerinde aynı anda çalışabilir şekilde ayarlanmış bir havuzu oluşturuldu.

    -   **isPaused** özelliği varsayılan olarak false değerine ayarlanır. Geçmişte dilimleri başlatmak için ardışık düzen hemen bu örnekte çalışır. Bu özelliği, ardışık düzen duraklatma ve yeniden başlatmak için geri false olarak ayarlamak için true olarak ayarlayın.

    -   **Başlat** zaman ve **son** kez beş saatten birbirinden olur ve dilimlerinin saatlik, ardışık düzen tarafından beş dilimlerinin şekilde.

1. İşlem hattını dağıtmak için komut çubuğunda **Dağıt**’a tıklayın.

#### <a name="step-5-test-the-pipeline"></a>5. adım: Test ardışık düzeni
Bu adımda, ardışık düzen giriş klasörler halinde dosyaları bırakarak sınayın. Bir giriş klasörü başına bir dosyayı ile işlem hattı testi ile başlayalım.

1. Azure Portal'daki Data Factory dikey penceresinde tıklayın **diyagramı**.

   ![](./media/data-factory-data-processing-using-batch/image10.png)
2. Diyagram Görünümü'nde girdi veri kümesi çift tıklatın: **InputDataset**.

   ![](./media/data-factory-data-processing-using-batch/image11.png)
3. Görmeniz gerekir **InputDataset** beş dikey dilimi hazır. Bildirim **DİLİM başlangıç saati** ve **DİLİM bitiş saati** her dilim için.

   ![](./media/data-factory-data-processing-using-batch/image12.png)
4. İçinde **diyagram görünümü**, şimdi **OutputDataset**.
5. Bunlar zaten oluşturulmuş olduğunu beş çıkış dilimi hazır durumda olduğunu görmeniz gerekir.

   ![](./media/data-factory-data-processing-using-batch/image13.png)
6. Görüntülemek için Azure portal'ı kullanmanızı **görevleri** ile ilişkili **dilimler** ve her dilim çalıştırdı hangi VM bakın. Bkz: [Data Factory ve toplu tümleştirme](#data-factory-and-batch-integration) ayrıntıları bölümü.
7. Çıkış dosyaları görmelisiniz `outputfolder` , `mycontainer` , Azure blob depolamada.

   ![](./media/data-factory-data-processing-using-batch/image15.png)

   Bir giriş her dilim için beş çıktı dosyalarını görmeniz gerekir. Her çıktı dosyasının içeriği aşağıdaki çıkış benzer olmalıdır:

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
    ```
   Aşağıdaki diyagram, veri fabrikası dilimler Azure Batch görevlerinde nasıl eşleneceğine gösterir. Bu örnekte, yalnızca bir çalışma bir dilimi var.

   ![](./media/data-factory-data-processing-using-batch/image16.png)
8. Şimdi, bir klasördeki birden çok dosyalarla deneyelim. Dosyaları oluşturun: **file2.txt**, **file3.txt**, **file4.txt**, ve **file5.txt** klasöründeki dosya.txt'yi olduğu gibi aynı içeriğe sahip:  **2015-11-06-01**.
9. Çıkış klasöründe **silmek** çıktı dosyası: **2015-11-16-01.txt**.
10. Şimdi **OutputDataset** dikey penceresinde bir dilimle sağ tıklatın **DİLİM başlangıç saati** kümesine **11/16/2015 01:00:00 AM**, tıklatıp **çalıştırmak** için yeniden çalıştır/yeniden-process dilim. Şimdi, dilim beş dosya yerine bir dosya vardır.

    ![](./media/data-factory-data-processing-using-batch/image17.png)
11. Dilim çalıştırır ve durumu sonra **hazır**, bu dilim için çıktı dosyasında içeriği doğrulayın (**2015-11-16-01.txt**) içinde `outputfolder` , `mycontainer` blob depolama alanınızın içinde. Dilimin her dosya için bir satır olması gerekir.

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file2.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file3.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file4.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file5.txt.
    ```

> [!NOTE]
> Beş giriş dosyalarıyla denemeden önce çıktı dosyası 2015-11-16-01.txt silmedi, tek bir çizgi önceki dilim çalıştırma ve geçerli dilimin çalıştırma beş satırları bakın. Varsayılan olarak, içeriği zaten varsa çıkış dosyasına eklenir.
>
>

#### <a name="data-factory-and-batch-integration"></a>Veri Fabrikası ve toplu tümleştirmesi
Data Factory hizmetinin bir iş Azure Batch adıyla oluşturur: `adf-poolname:job-xxx`.

![Azure Data Factory - toplu işler](media/data-factory-data-processing-using-batch/data-factory-batch-jobs.png)

İş bir görevde bir dilim her etkinlik çalıştırması için oluşturulur. 10 dilimler işlenmeye hazır varsa, 10 görevleri işi oluşturulur. Birden çok işlem düğümleri havuzunda varsa paralel olarak çalışan birden fazla dilim olabilir. İşlem düğümü başına en fazla görevleri ayarlarsanız > 1'e, aynı işlem üzerinde çalışan birden fazla dilim olabilir.

Bu örnekte, beş dilimler, Azure Batch kadar beş görevleri vardır. İle **eşzamanlılık** kümesine **5** Azure veri fabrikası'nda JSON işlem hattındaki ve **maksimum VM başına görevleri** kümesine **2** Azure Batch havuzunda ile **2** VM'ler, (görevler için başlangıç ve bitiş zamanlarını denetleyin) görevleri çalıştırır hızlı.

Toplu işlem ve ilişkili görevleri görüntülemek üzere portalı kullanın **dilimler** ve her dilim çalıştırdı hangi VM bakın.

![Azure Data Factory - toplu iş görevleri](media/data-factory-data-processing-using-batch/data-factory-batch-job-tasks.png)

### <a name="debug-the-pipeline"></a>Ardışık Düzen hata ayıklama
Hata ayıklama birkaç temel teknikten oluşur:

1. Girdi dilimi ayarlanmamışsa **hazır**, giriş klasör yapısı doğru olduğunu ve dosya.txt'yi giriş klasörlerde var olduğunu doğrulayın.

   ![](./media/data-factory-data-processing-using-batch/image3.png)
2. İçinde **yürütme** özel etkinliklerinizi, kullanım yöntemi **IActivityLogger** sorunlarını gidermenize yardımcı olacak bilgileri oturum nesnesi. Günlüğe kaydedilen iletilere kullanıcı görünmesini\_0. günlük dosyası.

   İçinde **OutputDataset** dikey penceresinde görmek için dilimi tıklatın **veri DİLİMİ** dikey penceresinde, dilim için. Gördüğünüz **etkinlik çalışması** bu dilim için. Bir etkinlik için dilim görmeniz gerekir. Tıklatırsanız **çalıştırmak** komut çubuğunda, aynı dilim için başka bir etkinlik başlatabilirsiniz.

   Etkinliğin çalışma tıklattığınızda gördüğünüz **etkinlik çalışma ayrıntıları** günlük dosyalarının listesini içeren dikey. İçinde günlüğe kaydedilen iletilere bakın **kullanıcı\_0. günlük** dosya. Hata oluştuğunda, yeniden deneme sayısı 3 JSON ardışık düzeni/etkinliği olarak ayarlandığından, üç Etkinlik çalışması görürsünüz. Etkinlik Çalıştır'ı tıklatın, hatayı gidermek için gözden geçirebilirsiniz günlük dosyalarına bakın.

   ![](./media/data-factory-data-processing-using-batch/image18.png)

   Günlük dosyaları listesinde tıklatın **kullanıcı 0.log**. Sağ panelde kullanarak sonucu olan **IActivityLogger.Write** yöntemi.

   ![](./media/data-factory-data-processing-using-batch/image19.png)

   Sistem hata iletileri ve özel durumlar için sistem 0.log denetleyin.

    ```
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Loading assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Creating an instance of MyDotNetActivityNS.MyDotNetActivity from assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Executing Module
    
    Trace\_T\_D\_12/6/2015 1:43:38 AM\_T\_D\_\_T\_D\_Information\_T\_D\_0\_T\_D\_Activity e3817da0-d843-4c5c-85c6-40ba7424dce2 finished successfully
    ```
3. Dahil **PDB** hata ayrıntılarını bilgileri gibi böylece zip dosyasında dosya **çağrı yığını** bir hata oluştuğunda.
4. Özel Etkinlik için zip dosyası tüm dosyalarda olmalıdır **üst düzey** klasörsüz ile.

   ![](./media/data-factory-data-processing-using-batch/image20.png)
5. Emin **assemblyName** (MyDotNetActivity.dll) **entryPoint** (MyDotNetActivityNS.MyDotNetActivity) **packageFile** (customactivitycontainer / MyDotNetActivity.zip) ve **packageLinkedService** (zip dosyasını içeren Azure blob depolama alanına işaret etmelidir) için doğru değerleri ayarlayın.
6. Bir hatayı düzelttiyseniz ve dilimi yeniden işlemek istiyorsanız **OutputDataset** dikey penceresindeki dilime sağ tıklayın ve **Çalıştır**’a tıklayın.

   ![](./media/data-factory-data-processing-using-batch/image21.png)

   > [!NOTE]
   > Gördüğünüz bir **kapsayıcı** adlı Azure Blob storage'da: `adfjobs`. Bu kapsayıcı otomatik olarak silinmez, ancak tamamlandıktan sonra güvenle silebilirsiniz çözüm test ediliyor. Benzer şekilde, bir Azure Batch Data Factory çözüm oluşturur **iş** adlı: `adf-\<pool ID/name\>:job-0000000001`. İsterseniz, çözümü test ettikten sonra bu iş silebilirsiniz.
   >
   >
7. Özel Etkinlik kullanmayan **app.config** paketinizi dosyasından. Bu nedenle, kodunuzu yapılandırma dosyasından bağlantı dizelerini yazıyorsa, çalışma zamanında çalışmıyor. Azure Batch kullanarak tüm gizli tutmak için olduğunda en iyi uygulama bir **Azure KeyVault**keyvault korumak için bir sertifika tabanlı hizmet sorumlusu kullanın ve Azure Batch havuzu sertifikaya dağıtın. Böylece .NET özel etkinliği çalıştırma sırasında KeyVault’tan parolalara erişebilir. Bu çözüm genel bir ve gizli anahtarı, yalnızca bağlantı dizesi herhangi bir türde ölçeklendirebilirsiniz.

    Daha kolay bir çözüm (ancak en iyi yöntem değildir): oluşturabileceğiniz bir **Azure SQL bağlı hizmeti** bağlantı dizesi ayarlarıyla bağlantılı hizmet kullanan bir veri kümesi oluşturma ve sahte bir giriş veri kümesi için özel .NET etkinlik olarak dataset zincir. Özel Etkinlik kodu bağlantılı hizmetin bağlantı dizesinde sonra erişebilir ve çalışma zamanında düzgün çalışması gerekir.  

#### <a name="extend-the-sample"></a>Örnek genişletme
Azure Data Factory ve Azure Batch özellikleri hakkında daha fazla bilgi için bu örnek genişletebilirsiniz. Örneğin, farklı bir zaman aralığı dilimleri işlemek için aşağıdaki adımları uygulayın:

1. Aşağıdaki klasörlerdeki eklemek `inputfolder`: 2015-11-16-05, 2015-11-16-06, 201-11-16-07, 2011-11-16-08, 2015-11-16-09 ve Yerleştir bu klasörlerdeki girişi dosyaları. Ardışık düzen tarafından bitiş zamanını değiştirin `2015-11-16T05:00:00Z` için `2015-11-16T10:00:00Z`. İçinde **diyagram görünümü**, çift **InputDataset**ve girdi dilimlerinin hazır olduğunu doğrulayın. Çift **OuptutDataset** çıkış dilimler durumunu görmek için. Hazır durumda olmaları durumunda çıkış dosyaları için çıkış klasörünü denetleyin.
2. Artırma veya azaltma **eşzamanlılık** özellikle Azure Batch oluşan işleme çözümünüzün performansını nasıl etkilediği anlamak için ayarı. (Bkz. adım 4: oluşturun ve daha fazla bilgi için ardışık düzen çalıştıracağınız **eşzamanlılık** ayarı.)
3. Küçük daha yüksek bir havuz oluşturma **maksimum VM başına görevleri**. Oluşturduğunuz yeni havuzu kullanmak için veri fabrikası çözüm bağlı Azure Batch hizmetinde güncelleştirin. (Bkz. adım 4: oluşturun ve daha fazla bilgi için ardışık düzen çalıştırmak **maksimum VM başına görevleri** ayarı.)
4. Bir Azure Batch havuzu oluşturma **otomatik ölçeklendirme** özelliği. Bir Azure Batch havuzunda işlem düğümlerini otomatik olarak ölçeklendirme, uygulamanız tarafından kullanılan güç işleme dinamik ayarlanmasıdır. 

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
5. Örnek çözümde **yürütme** yöntemini çağırır **Hesapla** bir çıktı veri dilimi üretmek için bir giriş veri dilimi işleyen yöntem. Girdi verilerini işlemek ve yönteminizi çağrısıyla yürütme yönteminin hesapla yöntem çağrısında değiştirmek için kendi yönteminizi yazabilirsiniz.

### <a name="next-steps-consume-the-data"></a>Sonraki adımlar: verileri kullanmak
Veri işleme sonra onu gibi çevrimiçi araçlarla tüketebileceği **Microsoft Power BI**. Power BI ve Azure'da kullanma anlamanıza yardımcı olması için bağlantılar şunlardır:

* [Power BI kümesinde keşfedin](https://powerbi.microsoft.com/documentation/powerbi-service-get-data/)
* [Power BI Desktop kullanmaya başlama](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/)
* [Power bı'da veri yenileme](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/)
* [Azure ve Power BI - temel genel bakış](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)

## <a name="references"></a>Başvurular
* [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)

  * [Azure Data Factory Hizmeti'ne Giriş](data-factory-introduction.md)
  * [Azure Data Factory ile çalışmaya başlama](data-factory-build-your-first-pipeline.md)
  * [Bir Azure Data Factory işlem hattında özel etkinlikler kullanma](data-factory-use-custom-activities.md)
* [Azure toplu işlem](https://azure.microsoft.com/documentation/services/batch/)

  * [Azure Batch temel bilgileri](../../batch/batch-technical-overview.md)
  * [Azure Batch özelliklerine genel bakış](../../batch/batch-api-basics.md)
  * [Azure portalında Azure Batch hesabı oluşturabilir ve yönetebilirsiniz](../../batch/batch-account-create-portal.md)
  * [Azure Batch kitaplığını .NET ile çalışmaya başlama](../../batch/batch-dotnet-get-started.md)

[batch-explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch-explorer-walkthrough]: http://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx
