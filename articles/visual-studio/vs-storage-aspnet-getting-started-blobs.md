---
title: Azure Blob ile çalışmaya başlama depolama ve Visual Studio bağlı Hizmetleri (ASP.NET) | Microsoft Docs
description: Nasıl başlayacağınızı Visual Studio'da ASP.NET projesinde Azure Blob storage kullanarak Visual Studio kullanarak bir depolama hesabına bağlandıktan sonra hizmetleri bağlı
services: storage
author: ghogen
manager: douge
ms.assetid: b3497055-bef8-4c95-8567-181556b50d95
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 12/07/2017
ms.author: ghogen
ms.openlocfilehash: 55b083ac5384e749098338d2f3b7b24bfe16fe8f
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet"></a>Azure Blob ile çalışmaya başlama depolama ve Visual Studio bağlı Hizmetleri (ASP.NET)

> [!div class="op_single_selector"]
> - [ASP.NET](./vs-storage-aspnet-getting-started-blobs.md)
> - [ASP.NET Core](./vs-storage-aspnet-core-getting-started-blobs.md)

Azure Blob Depolama nesneleri veya BLOB bulutta yapılandırılmamış veri depolayan bir hizmettir. Blob Storage belge, medya dosyası veya uygulama yükleyici gibi her tür metin veya ikili veri depolayabilir. Blob Storage aynı zamanda nesne depolama olarak adlandırılır.

Bu öğretici, Blob storage kullanma bazı genel senaryolar için ASP.NET kodunun nasıl yazılacağını gösterir. Bir blob kapsayıcısını oluşturma ve karşıya yükleme, listeleme, indirme ve BLOB'ları silme senaryolar içerir.

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="prerequisites"></a>Önkoşullar

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]


[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

## <a name="create-an-mvc-controller"></a>Bir MVC denetleyicisi oluşturun. 

1. İçinde **Çözüm Gezgini**, sağ **denetleyicileri**.

2. Bağlam menüsünden seçin **Ekle** > **denetleyicisi**.

    ![Ekran görüntüsü, Çözüm Gezgini'nde, Ekle ve vurgulanmış denetleyicisi](./media/vs-storage-aspnet-getting-started-blobs/add-controller-menu.png)

1. İçinde **İskele Ekle** iletişim kutusunda **MVC 5 denetleyici - boş**seçip **Ekle**.

    ![Ekran görüntüsü, İskele Ekle iletişim kutusu](./media/vs-storage-aspnet-getting-started-blobs/add-controller.png)

1. İçinde **denetleyici Ekle** iletişim kutusunda, denetleyici adı *BlobsController*seçip **Ekle**.

    ![Ekran görüntüsü, denetleyici Ekle iletişim kutusu](./media/vs-storage-aspnet-getting-started-blobs/add-controller-name.png)

1. Aşağıdakileri ekleyin `using` yönergeleri `BlobsController.cs` dosyası:

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="connect-to-a-storage-account-and-get-a-container-reference"></a>Bir depolama hesabına bağlanmak ve bir kapsayıcı başvurusu edinin

Bir blob kapsayıcı, BLOB'ları ve klasörleri iç içe geçmiş bir hiyerarşisidir. Bu belgede yer alan adımlar geri kalanı, böylece kodu yeniden kullanılırlığı için kendi yöntemi yerleştirilmelidir bir blob kapsayıcısına bir başvuru gerektirir.

Aşağıdaki adımlar bağlantı dizesini kullanarak depolama hesabına bağlanmak için bir yöntem oluşturma **Web.config**. Adımları ayrıca bir kapsayıcıya başvuru oluşturun.  Bağlantı dizesi ayarı **Web.config** biçimiyle adlı `<storageaccountname>_AzureStorageConnectionString`. 

1. `BlobsController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **GetCloudBlobContainer** döndüren bir **CloudBlobContainer**.  Değiştirdiğinizden emin olun `<storageaccountname>_AzureStorageConnectionString` anahtarın gerçek adıyla **Web.config**.
    
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
> Olsa bile *test blob kapsayıcısı* yok henüz, bu kod bir başvuru oluşturur. Kapsayıcı ile oluşturulan böylelikle `CreateIfNotExists` sonraki adımda gösterilen yöntemi.

## <a name="create-a-blob-container"></a>Blob kapsayıcısı oluşturma

Aşağıdaki adımları bir blob kapsayıcı oluşturulacağını gösterir:

1. Adlı bir yöntem ekleyin `CreateBlobContainer` döndüren bir `ActionResult`.

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. Alma bir `CloudBlobContainer` istenen blob kapsayıcı adı için başvuru temsil eden nesne. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Çağrı `CloudBlobContainer.CreateIfNotExists` henüz yoksa, kapsayıcı oluşturmak için yöntemi. `CloudBlobContainer.CreateIfNotExists` Yöntemi döndürür **true** kapsayıcı yok ve başarıyla oluşturuldu. Aksi durumda, yöntem döndürür **false**.    

    ```csharp
    ViewBag.Success = container.CreateIfNotExists();
    ```

1. Güncelleştirme `ViewBag` blob kapsayıcı adı.

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

2. Bağlam menüsünden seçin **Ekle** > **yeni klasör**. Yeni bir klasör adı *BLOB'lar*. 
 
1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörü ve sağ **BLOB'lar**.

4. Bağlam menüsünden seçin **Ekle** > **Görünüm**.

1. İçinde **Görünüm Ekle** iletişim kutusunda, girin **CreateBlobContainer** Görünüm adı ' nı seçip için **Ekle**.

1. Açık `CreateBlobContainer.cshtml`ve aşağıdaki kod parçacığını gibi görünüyor şekilde değiştirin:

    ```csharp
    @{
        ViewBag.Title = "Create Blob Container";
    }
    
    <h2>Create Blob Container results</h2>

    Creation of @ViewBag.BlobContainerName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** > **paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. Son sonra **Html.ActionLink**, aşağıdakileri ekleyin **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Create blob container", "CreateBlobContainer", "Blobs")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **Blob kapsayıcısı oluşturmak** aşağıdaki ekran görüntüsüne benzer sonuçlar görmek için:
  
    ![Blob kapsayıcısı, ekran oluşturma](./media/vs-storage-aspnet-getting-started-blobs/create-blob-container-results.png)

    Daha önce belirtildiği gibi `CloudBlobContainer.CreateIfNotExists` yöntemi döndürür **doğru** yalnızca kapsayıcı yok ve oluşturulur. Bu nedenle, kapsayıcı mevcut olduğunda uygulama çalıştırırsanız, yöntem **false**.

## <a name="upload-a-blob-into-a-blob-container"></a>Bir blob kapsayıcıya bir blob karşıya yükleme

Zaman [blob kapsayıcısı oluşturuldu](#create-a-blob-container), bu kapsayıcıya dosyaları karşıya yükleme. Bu bölümde, bir blob kapsayıcısına bir yerel dosya karşıya yükleme aracılığıyla anlatılmaktadır. Adımları adlı bir blob kapsayıcı olduğu varsayılır *test blob kapsayıcısı*. 

1. `BlobsController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin `UploadBlob` bir dize döndürür.

    ```csharp
    public string UploadBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```
 
1. İçinde `UploadBlob` yöntemi, get bir `CloudBlobContainer` istenen blob kapsayıcı adı için başvuru temsil eden nesne. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Azure depolama farklı blob türlerini destekler. Bu öğretici blok blobları kullanır. Bir blok blob başvurusu almak için arama `CloudBlobContainer.GetBlockBlobReference` yöntemi.

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```
    
    > [!NOTE]
    > Blob adı bir blob almak için kullanılan URL'nin bir parçasıdır ve dosya adı dahil olmak üzere herhangi bir dize olmalıdır.

1. Bir blob başvurusu sonra tüm veri akışı için blob başvurusu nesnenin çağırarak karşıya yükleyebilirsiniz `UploadFromStream` yöntemi. `UploadFromStream` Yöntemi, mevcut değil veya mevcut değilse bu raporun üzerine blob oluşturur. (Değişiklik  *&lt;dosya karşıya yükleme >* karşıya yüklenecek bir dosyaya tam bir yol için.)

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

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** > **paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. Son sonra **Html.ActionLink**, aşağıdakileri ekleyin **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Upload blob", "UploadBlob", "Blobs")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **karşıya yükleme blob**.  Word *başarılı!* görüntülenmesi gerekir.
    
    ![Başarı doğrulama ekran görüntüsü](./media/vs-storage-aspnet-getting-started-blobs/upload-blob.png)
  
## <a name="list-the-blobs-in-a-blob-container"></a>Blob kapsayıcısı içinde BLOB'ları Listele

Bu bölümde, bir blob kapsayıcısı içinde BLOB'ları listesi göstermektedir. Örnek kod başvurularını *test blob kapsayıcısı* bölümünde oluşturduğunuz [bir blob kapsayıcı oluşturun](#create-a-blob-container).

1. `BlobsController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin `ListBlobs` döndüren bir `ActionResult`.

    ```csharp
    public ActionResult ListBlobs()
    {
        // The code in this section goes here.

    }
    ```
 
1. İçinde `ListBlobs` yöntemi, get bir `CloudBlobContainer` blob kapsayıcısına bir başvuru temsil eden nesne. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```
   
1. Blob kapsayıcısı içinde BLOB'ları listelemek için kullanın `CloudBlobContainer.ListBlobs` yöntemi. `CloudBlobContainer.ListBlobs` Yöntemi döndürür bir `IListBlobItem` için cast nesnesi bir `CloudBlockBlob`, `CloudPageBlob`, veya `CloudBlobDirectory` nesnesi. Aşağıdaki kod parçacığını bir blob kapsayıcıdaki tüm blob'lara numaralandırır. Kendi türüne göre uygun bir nesne için her bir blob atayın. Kendi adını (veya URI durumunda bir **CloudBlobDirectory**) bir listesine eklenir.

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

    BLOB'ları yanı sıra blob kapsayıcıları dizinleri içerebilir. Adlı blob kapsayıcısında olduğunu varsayın *test blob kapsayıcısı*, aşağıdaki hiyerarşi ile:

        foo.png
        dir1/bar.png
        dir2/baz.png

    Önceki kod örneğinde kullanarak **BLOB'lar** dize listesi aşağıdakine benzer değerleri içerir:

        foo.png
        <storage-account-url>/test-blob-container/dir1
        <storage-account-url>/test-blob-container/dir2

    Gösterildiği gibi listenin yalnızca üst düzey varlıklar, iç içe geçmiş olanlar içerir (*bar.png* ve *baz.png*). Bir blob kapsayıcı içindeki tüm varlıkları listelemek için kodu değiştirin böylece **CloudBlobContainer.ListBlobs** yöntemi geçirilir **true** için **Listblobs** parametre.    

    ```csharp
    //...
    foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing:true))
    //...
    ```

    Ayarı **Listblobs** parametresi **true** blob kapsayıcısında tüm varlıkların düz bir liste döndürür. Aşağıdaki sonuçları verir:

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

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörü ve sağ **BLOB'lar**.

2. Bağlam menüsünden seçin **Ekle** > **Görünüm**.

1. İçinde **Görünüm Ekle** iletişim kutusunda, girin `ListBlobs` Görünüm adı ' nı seçip için **Ekle**.

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

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** > **paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. Son sonra **Html.ActionLink**, aşağıdakileri ekleyin **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("List blobs", "ListBlobs", "Blobs")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **listesinde BLOB'lar** aşağıdaki ekran görüntüsüne benzer sonuçlar görmek için:
  
    ![Ekran listesi BLOB'ları](./media/vs-storage-aspnet-getting-started-blobs/listblobs.png)

## <a name="download-blobs"></a>Blob’ları indirme

Bu bölümde, bir blob indirmek göstermektedir. Yerel depolama alanına devam veya içeriği bir dizeye okuyun. Örnek kod başvurularını *test blob kapsayıcısı* bölümünde oluşturduğunuz [bir blob kapsayıcı oluşturun](#create-a-blob-container).

1. `BlobsController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin `DownloadBlob` bir dize döndürür.

    ```csharp
    public string DownloadBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```
 
1. İçinde `DownloadBlob` yöntemi, get bir `CloudBlobContainer` blob kapsayıcısına bir başvuru temsil eden nesne.
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Bir blob başvurusu nesnesi çağırarak alma `CloudBlobContainer.GetBlockBlobReference` yöntemi. 

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```

1. Bir blob indirmek için kullanacağınız `CloudBlockBlob.DownloadToStream` yöntemi. Aşağıdaki kod bir akış nesnesine bir blob'un içeriği aktarır. Bu nesne, ardından yerel bir dosyaya kalıcıdır. (Değişiklik  *&lt;yerel dosya adı >* olduğu blob indirilmesi gösteren tam dosya adı.) 

    ```csharp
    using (var fileStream = System.IO.File.OpenWrite(<local-file-name>))
    {
        blob.DownloadToStream(fileStream);
    }
    ```
    
    Aşağıdaki tamamlanmış gösterir `ListBlobs` yöntemi (ile oluşturulan yerel dosyanın tam yolu):
    
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

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** > **paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. Son sonra **Html.ActionLink**, aşağıdakileri ekleyin **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Download blob", "DownloadBlob", "Blobs")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **indirme blob** blob indirmek için. Belirtilen blob `CloudBlobContainer.GetBlockBlobReference` yöntem çağrısı indirmeleri için belirttiğiniz konuma `File.OpenWrite` yöntem çağrısı.  Metin *başarılı!* tarayıcıda görüntülenmesi gerekir. 

## <a name="delete-blobs"></a>Blob’ları silme

Aşağıdaki adımlar bir blobu silmek nasıl gösterilmektedir:

1. `BlobsController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin `DeleteBlob` bir dize döndürür.

    ```csharp
    public string DeleteBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```

1. İçinde `DeleteBlob` yöntemi, get bir `CloudBlobContainer` blob kapsayıcısına bir başvuru temsil eden nesne.
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Bir blob başvurusu nesnesi çağırarak alma `CloudBlobContainer.GetBlockBlobReference` yöntemi. 

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

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** > **paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. Son sonra **Html.ActionLink**, aşağıdakileri ekleyin **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete blob", "DeleteBlob", "Blobs")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **Delete blob** belirtilen blobu silmek için `CloudBlobContainer.GetBlockBlobReference` yöntem çağrısı. Metin *başarılı!* tarayıcıda görüntülenmesi gerekir. Tarayıcının seçin **geri** düğmesine tıklayın ve ardından **listesinde BLOB'lar** blob kapsayıcısında artık olduğunu doğrulamak için.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, depolama, liste ve ASP.NET kullanarak Azure Storage blobları almak öğrendiniz. Azure’da veri depolama ile ilgili ek seçenekler hakkında daha fazla bilgi edinmek için daha fazla özellik kılavuzu görüntüleyin.

  * [Azure Table ile çalışmaya başlama depolama ve Visual Studio bağlı Hizmetleri (ASP.NET)](vs-storage-aspnet-getting-started-tables.md)
  * [Azure kuyruk ile çalışmaya başlama depolama ve Visual Studio bağlı Hizmetleri (ASP.NET)](vs-storage-aspnet-getting-started-queues.md)
