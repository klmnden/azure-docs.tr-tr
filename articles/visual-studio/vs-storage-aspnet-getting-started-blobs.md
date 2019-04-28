---
title: Azure Blob ile çalışmaya başlama depolama ve Visual Studio bağlı hizmetler (ASP.NET) | Microsoft Docs
description: Başlamak nasıl bir ASP.NET projesi Visual Studio'da Azure Blob Depolama kullanarak Visual Studio kullanarak bir depolama hesabına bağlandıktan sonra bağlı hizmetler
services: storage
author: ghogen
manager: douge
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/07/2017
ms.author: ghogen
ms.openlocfilehash: 557b47aab5cdadef79427c4da9c18477daac0689
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62123051"
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet"></a>Azure Blob ile çalışmaya başlama depolama ve Visual Studio bağlı hizmetler (ASP.NET)

> [!div class="op_single_selector"]
> - [ASP.NET](./vs-storage-aspnet-getting-started-blobs.md)
> - [ASP.NET Core](./vs-storage-aspnet-core-getting-started-blobs.md)

Azure Blob Depolama, yapılandırılmamış verileri nesne veya BLOB olarak bulutta depolayan bir hizmettir. Blob Storage belge, medya dosyası veya uygulama yükleyici gibi her tür metin veya ikili veri depolayabilir. Blob Storage aynı zamanda nesne depolama olarak adlandırılır. Blob Depolama hakkında daha fazla bilgi için bkz: [Azure Blob depolamaya giriş](../storage/blobs/storage-blobs-introduction.md).

Bu öğreticide, Blob Depolama kullanan bazı genel senaryolar için ASP.NET kodunun nasıl yazılacağını gösterir. Bir blob kapsayıcısı oluşturma ve karşıya yükleme, listeleme, indirme ve BLOB'ları silmeden senaryolar içerir.

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="prerequisites"></a>Önkoşullar

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

## <a name="create-an-mvc-controller"></a>MVC denetleyicisi oluşturma 

1. İçinde **Çözüm Gezgini**, sağ **denetleyicileri**.

2. Bağlam menüsünden seçin **Ekle** > **denetleyicisi**.

    ![Çözüm Gezgini'nin ekran görüntüsü, Ekle ve vurgulanmış denetleyicisi](./media/vs-storage-aspnet-getting-started-blobs/add-controller-menu.png)

1. İçinde **İskele Ekle** iletişim kutusunda **MVC 5 denetleyici - boş**seçip **Ekle**.

    ![Ekran görüntüsü, İskele Ekle iletişim kutusu](./media/vs-storage-aspnet-getting-started-blobs/add-controller.png)

1. İçinde **denetleyici Ekle** iletişim kutusunda, denetleyici adı *BlobsController*seçip **Ekle**.

    ![Ekran görüntüsü, denetleyici Ekle iletişim kutusu](./media/vs-storage-aspnet-getting-started-blobs/add-controller-name.png)

1. Aşağıdaki `using` yönergelerini `BlobsController.cs` dosyası:

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="connect-to-a-storage-account-and-get-a-container-reference"></a>Bir kapsayıcı başvurusu alın ve bir depolama hesabına bağlama

Bir blob kapsayıcı, BLOB ve klasörleri iç içe hiyerarşisini ' dir. Bu belgedeki adımlarda geri kalanı ölçeklendirilebileceği için kendi yöntemindeki kod yerleştirilmelidir. böylece bir blob kapsayıcısına bir başvuru gerektirir.

Aşağıdaki adımlar bağlantı dizesini kullanarak depolama hesabına bağlanmak için bir yöntem oluşturma **Web.config**. Adımları ayrıca bir başvuru için bir kapsayıcı oluşturun.  Bağlantı dizesi ayarı **Web.config** biçimiyle adlı `<storageaccountname>_AzureStorageConnectionString`. 

1. `BlobsController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **GetCloudBlobContainer** döndüren bir **CloudBlobContainer**.  Değiştirdiğinizden emin olun `<storageaccountname>_AzureStorageConnectionString` anahtarı gerçek adıyla **Web.config**.
    
    ```csharp
    private CloudBlobContainer GetCloudBlobContainer()
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
                CloudConfigurationManager.GetSetting("<storageaccountname>_AzureStorageConnectionString"));
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
        return container;
    }
    ```

> [!NOTE]
> Olsa da *test blob kapsayıcısı* yok henüz bu kod buna bir başvuru oluşturur. Kapsayıcı ile oluşturulan bu olduğundan `CreateIfNotExists` sonraki adımda gösterilen yöntemi.

## <a name="create-a-blob-container"></a>Blob kapsayıcısı oluşturma

Aşağıdaki adımlar, bir blob kapsayıcısı oluşturma işlemini göstermektedir:

1. Adlı bir yöntem ekleyin `CreateBlobContainer` döndüren bir `ActionResult`.

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. Alma bir `CloudBlobContainer` istenen blob kapsayıcı adı için bir başvuru temsil eden nesne. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Çağrı `CloudBlobContainer.CreateIfNotExists` henüz yoksa, kapsayıcı oluşturmak için yöntemi. `CloudBlobContainer.CreateIfNotExists` Yöntemi döndürür **true** kapsayıcı yok ve başarıyla oluşturuldu. Aksi takdirde yöntem döndürür **false**.    

    ```csharp
    ViewBag.Success = container.CreateIfNotExists();
    ```

1. Güncelleştirme `ViewBag` ile blob kapsayıcısının adı.

    ```csharp
    ViewBag.BlobContainerName = container.Name;
    ```
    
    Aşağıdaki tamamlanmış gösterir `CreateBlobContainer` yöntemi:

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        ViewBag.Success = container.CreateIfNotExists();
        ViewBag.BlobContainerName = container.Name;

        return View();
    }
    ```

1. İçinde **Çözüm Gezgini**, sağ **görünümleri** klasör.

1. Mevcut değilse bir **Blobları** klasörü oluşturun. Bağlam menüsünden seçin **Ekle** > **yeni klasör**. Yeni klasör adı *Blobları*. 
 
1. İçinde **Çözüm Gezgini**, genişletme **görünümleri** klasörünü açın ve sağ **Blobları**.

1. Bağlam menüsünden seçin **Ekle** > **görünümü**.

1. İçinde **Görünüm Ekle** iletişim kutusuna **CreateBlobContainer** görünüm adını ve seçin için **Ekle**.

1. Açık `CreateBlobContainer.cshtml`ve aşağıdaki kod parçacığı gibi görünüyor şekilde değiştirin:

    ```csharp
    @{
        ViewBag.Title = "Create Blob Container";
    }
    
    <h2>Create Blob Container results</h2>

    Creation of @ViewBag.BlobContainerName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. İçinde **Çözüm Gezgini**, genişletme **görünümleri** > **paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. En son **Html.ActionLink**, aşağıdaki **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Create blob container", "CreateBlobContainer", "Blobs")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **Blob kapsayıcısı Oluştur** aşağıdaki ekran görüntüsüne benzer bir sonuç görmek için:
  
    ![Blob kapsayıcısı Oluştur ekran görüntüsü](./media/vs-storage-aspnet-getting-started-blobs/create-blob-container-results.png)

    Daha önce de belirtildiği `CloudBlobContainer.CreateIfNotExists` yöntemi döndürür **true** yalnızca kapsayıcı yok ve oluşturulur. Bu nedenle, kapsayıcı mevcut olduğunda, uygulama çalıştırıldığında, yöntem döndürür **false**.

## <a name="upload-a-blob-into-a-blob-container"></a>Bir blob kapsayıcıya bir blob yükleme

Zaman [blob kapsayıcısı oluşturulur](#create-a-blob-container), dosyalar bu kapsayıcıya yükleyin. Bu bölüm bir yerel dosya bir blob kapsayıcısına yüklemek aracılığıyla size yol gösterir. Adlı bir blob kapsayıcısı yok adımlarda varsayılır *test blob kapsayıcısı*. 

1. `BlobsController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin `UploadBlob` , bir dize döndürür.

    ```csharp
    public string UploadBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```
 
1. İçinde `UploadBlob` yöntemi almak bir `CloudBlobContainer` istenen blob kapsayıcı adı için bir başvuru temsil eden nesne. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Azure depolama blob farklı türlerini destekler. Bu öğreticide, blok blobları kullanılır. Bir blok blobuna bir başvuru almak için arama `CloudBlobContainer.GetBlockBlobReference` yöntemi.

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```
    
    > [!NOTE]
    > Blob adı bir blob almak için kullanılan URL'nin bir parçasıdır ve dosya adı dahil olmak üzere herhangi bir dize olabilir.

1. Bir blob başvurusu sonra herhangi bir veri akışı için blob başvurusu nesnenin çağırarak yükleyebilirsiniz `UploadFromStream` yöntemi. `UploadFromStream` Yöntemi oluşturur blobu mevcut değil veya varsa üzerine yazar. (Değişiklik  *&lt;dosya karşıya yükleme >* karşıya yüklenecek bir dosya için tam olarak nitelenmiş bir yola.)

    ```csharp
    using (var fileStream = System.IO.File.OpenRead(@"<file-to-upload>"))
    {
        blob.UploadFromStream(fileStream);
    }
    ```
    
    Aşağıdaki tamamlanmış gösterir `UploadBlob` yöntemiyle (karşıya yüklenecek dosyanın tam yolu):

    ```csharp
    public string UploadBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        using (var fileStream = System.IO.File.OpenRead(@"c:\src\sample.txt"))
        {
            blob.UploadFromStream(fileStream);
        }
        return "success!";
    }
    ```

1. İçinde **Çözüm Gezgini**, genişletme **görünümleri** > **paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. En son **Html.ActionLink**, aşağıdaki **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Upload blob", "UploadBlob", "Blobs")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **blobu karşıya yükleme**.  Word *başarılı!* görüntülenmesi gerekir.
    
    ![Ekran görüntüsü başarısını doğrulama](./media/vs-storage-aspnet-getting-started-blobs/upload-blob.png)
  
## <a name="list-the-blobs-in-a-blob-container"></a>Blob kapsayıcı içindeki blobları Listele

Bu bölümde, bir blob kapsayıcıdaki blobları listelemek gösterilmektedir. Örnek kod başvurularını *test blob kapsayıcısı* bölümünde oluşturduğunuz [bir blob kapsayıcısı oluşturursunuz](#create-a-blob-container).

1. `BlobsController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin `ListBlobs` döndüren bir `ActionResult`.

    ```csharp
    public ActionResult ListBlobs()
    {
        // The code in this section goes here.

    }
    ```
 
1. İçinde `ListBlobs` yöntemi almak bir `CloudBlobContainer` blob kapsayıcısına bir başvuru temsil eden nesne. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```
   
1. Bir blob kapsayıcıdaki blobları listelemek için kullanın `CloudBlobContainer.ListBlobs` yöntemi. `CloudBlobContainer.ListBlobs` Yöntemi döndürür bir `IListBlobItem` atanabilecek bir nesne bir `CloudBlockBlob`, `CloudPageBlob`, veya `CloudBlobDirectory` nesne. Aşağıdaki kod parçacığı, bir blob kapsayıcıdaki tüm blob'lara numaralandırır. Her blobun kendi türüne göre uygun nesne türüne dönüştürülür. Adı (veya URI durumunda bir **CloudBlobDirectory**) bir listesine eklenir.

    ```csharp
    List<string> blobs = new List<string>();

    foreach (IListBlobItem item in container.ListBlobs())
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob blob = (CloudPageBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory dir = (CloudBlobDirectory)item;
            blobs.Add(dir.Uri.ToString());
        }
    }

    return View(blobs);
    ```

    Blob kapsayıcıları, blobları ek olarak, dizinleri içerebilir. Adlı bir blob kapsayıcısında olduğunu varsayın *test blob kapsayıcısı*, aşağıdaki hiyerarşisi:

        foo.png
        dir1/bar.png
        dir2/baz.png

    Yukarıdaki kod örneğinde, kullanarak **blobları** dize listesi aşağıdaki gibi değerleri içerir:

        foo.png
        <storage-account-url>/test-blob-container/dir1
        <storage-account-url>/test-blob-container/dir2

    Gösterildiği gibi liste yalnızca üst düzey varlıkları ve iç içe geçmiş olanlar içerir (*bar.png* ve *baz.png*). Bir blob kapsayıcı içindeki tüm varlıkları listelemek için kodu değiştirin böylece **CloudBlobContainer.ListBlobs** yöntemi geçirilir **true** için **Listblobs** parametre.    

    ```csharp
    //...
    foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing:true))
    //...
    ```

    Ayarı **Listblobs** parametresi **true** blob kapsayıcısında tüm varlıkların düz bir liste döndürür. Bu, aşağıdaki sonuçları verir:

        foo.png
        dir1/bar.png
        dir2/baz.png
    
    Aşağıdaki tamamlanmış gösterir **ListBlobs** yöntemi:

    ```csharp
    public ActionResult ListBlobs()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        List<string> blobs = new List<string>();
        foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing: true))
        {
            if (item.GetType() == typeof(CloudBlockBlob))
            {
                CloudBlockBlob blob = (CloudBlockBlob)item;
                blobs.Add(blob.Name);
            }
            else if (item.GetType() == typeof(CloudPageBlob))
            {
                CloudPageBlob blob = (CloudPageBlob)item;
                blobs.Add(blob.Name);
            }
            else if (item.GetType() == typeof(CloudBlobDirectory))
            {
                CloudBlobDirectory dir = (CloudBlobDirectory)item;
                blobs.Add(dir.Uri.ToString());
            }
        }

        return View(blobs);
    }
    ```

1. İçinde **Çözüm Gezgini**, genişletme **görünümleri** klasörünü açın ve sağ **Blobları**.

2. Bağlam menüsünden seçin **Ekle** > **görünümü**.

1. İçinde **Görünüm Ekle** iletişim kutusuna `ListBlobs` görünüm adını ve seçin için **Ekle**.

1. Açık `ListBlobs.cshtml`ve içeriğini aşağıdaki kodla değiştirin:

    ```html
    @model List<string>
    @{
        ViewBag.Title = "List blobs";
    }
    
    <h2>List blobs</h2>
    
    <ul>
        @foreach (var item in Model)
        {
        <li>@item</li>
        }
    </ul>
    ```

1. İçinde **Çözüm Gezgini**, genişletme **görünümleri** > **paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. En son **Html.ActionLink**, aşağıdaki **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("List blobs", "ListBlobs", "Blobs")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **listeleme** aşağıdaki ekran görüntüsüne benzer bir sonuç görmek için:
  
    ![Ekran listesi BLOB'ları](./media/vs-storage-aspnet-getting-started-blobs/listblobs.png)

## <a name="download-blobs"></a>Blob’ları indirme

Bu bölümde, blob indirmek gösterilmektedir. Yerel depolama alanına kalıcı veya içeriğini bir dizeye okuyun. Örnek kod başvurularını *test blob kapsayıcısı* bölümünde oluşturduğunuz [bir blob kapsayıcısı oluşturursunuz](#create-a-blob-container).

1. `BlobsController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin `DownloadBlob` , bir dize döndürür.

    ```csharp
    public string DownloadBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```
 
1. İçinde `DownloadBlob` yöntemi almak bir `CloudBlobContainer` blob kapsayıcısına bir başvuru temsil eden nesne.
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Bir blob başvurusu nesnesini çağırarak alma `CloudBlobContainer.GetBlockBlobReference` yöntemi. 

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```

1. Bir blobu indirmek için kullanmak `CloudBlockBlob.DownloadToStream` yöntemi. Aşağıdaki kod, bir blobun içeriklerini bir akış nesnesine aktarır. Ardından söz konusu nesne yerel bir dosyaya kalıcı hale getirilir. (Değişiklik  *&lt;yerel dosya adı >* tam olarak nitelenmiş dosya adını gösteren blob olduğu indirilmesi.) 

    ```csharp
    using (var fileStream = System.IO.File.OpenWrite(<local-file-name>))
    {
        blob.DownloadToStream(fileStream);
    }
    ```
    
    Aşağıdaki tamamlanmış gösterir `DownloadBlob` yöntemi (ile oluşturulan yerel dosyanın tam yolu):
    
    ```csharp
    public string DownloadBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        using (var fileStream = System.IO.File.OpenWrite(@"c:\src\downloadedBlob.txt"))
        {
            blob.DownloadToStream(fileStream);
        }
        return "success!";
    }
    ```

1. İçinde **Çözüm Gezgini**, genişletme **görünümleri** > **paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. En son **Html.ActionLink**, aşağıdaki **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Download blob", "DownloadBlob", "Blobs")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **indirme blob** blobu indirmek için. Belirtilen blob `CloudBlobContainer.GetBlockBlobReference` yöntem çağrısının indirir, belirtilen konuma `File.OpenWrite` yöntem çağrısı.  Metin *başarılı!* tarayıcı içinde görüntülenmesi gerekir. 

## <a name="delete-blobs"></a>Blob’ları silme

Aşağıdaki adımlar, bir blob silme işlemini göstermektedir:

1. `BlobsController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin `DeleteBlob` , bir dize döndürür.

    ```csharp
    public string DeleteBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```

1. İçinde `DeleteBlob` yöntemi almak bir `CloudBlobContainer` blob kapsayıcısına bir başvuru temsil eden nesne.
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Bir blob başvurusu nesnesini çağırarak alma `CloudBlobContainer.GetBlockBlobReference` yöntemi. 

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```

1. Bir blobu silmek için kullanın `Delete` yöntemi.

    ```csharp
    blob.Delete();
    ```
    
    Tamamlanan `DeleteBlob` yöntemi aşağıdaki gibi görünmelidir:
    
    ```csharp
    public string DeleteBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        blob.Delete();
        return "success!";
    }
    ```

1. İçinde **Çözüm Gezgini**, genişletme **görünümleri** > **paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. En son **Html.ActionLink**, aşağıdaki **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete blob", "DeleteBlob", "Blobs")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **silme blob** belirtilen blob silinemedi `CloudBlobContainer.GetBlockBlobReference` yöntem çağrısı. Metin *başarılı!* tarayıcı içinde görüntülenmesi gerekir. Tarayıcının seçin **geri** düğmesini ve ardından **listeleme** blob kapsayıcısında artık olduğu doğrulanamadı.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, ASP.NET kullanarak Azure Depolama'daki blobları almak depolamak ve liste öğrendiniz. Azure’da veri depolama ile ilgili ek seçenekler hakkında daha fazla bilgi edinmek için daha fazla özellik kılavuzu görüntüleyin.

  * [Azure tablosu ile çalışmaya başlama depolama ve Visual Studio bağlı hizmetler (ASP.NET)](vs-storage-aspnet-getting-started-tables.md)
  * [Azure kuyruk ile çalışmaya başlama depolama ve Visual Studio bağlı hizmetler (ASP.NET)](vs-storage-aspnet-getting-started-queues.md)
