---
title: "Azure blob depolama ve Visual Studio bağlı Hizmetleri (ASP.NET) kullanmaya başlama | Microsoft Docs"
description: "Visual Studio bağlantılı hizmetler kullanarak bir depolama hesabı bağlandıktan sonra Visual Studio'da ASP.NET projesinde Azure blob storage kullanarak nereden başlayacaksınız"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: b3497055-bef8-4c95-8567-181556b50d95
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: kraig
ms.openlocfilehash: f0d0815e71149749cb52efe21e1f0af3cabae21c
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet"></a>Azure blob depolama ve Visual Studio bağlı Hizmetleri (ASP.NET) kullanmaya başlama

> [!div class="op_single_selector"]
> - [ASP.NET](./vs-storage-aspnet-getting-started-blobs.md)
> - [ASP.NET Core](./vs-storage-aspnet-core-getting-started-blobs.md)

Azure blob depolama bulutta nesne/BLOB olarak yapılandırılmamış veri depolayan bir hizmettir. Blob Storage belge, medya dosyası veya uygulama yükleyici gibi her tür metin veya ikili veri depolayabilir. Blob Storage aynı zamanda nesne depolama olarak adlandırılır.

Bu öğretici, Azure blob storage kullanarak bazı genel senaryolar için ASP.NET kodunun nasıl yazılacağını gösterir. Bir blob kapsayıcısını oluşturma ve karşıya yükleme, listeleme, indirme ve BLOB'ları silme senaryolar içerir.

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="prerequisites"></a>Ön koşullar

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]


[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a>Bir MVC denetleyicisi oluşturun. 

1. İçinde **Çözüm Gezgini**, sağ **denetleyicileri**ve bağlam menüsünden seçin **Ekle -> denetleyicisi**.

    ![Bir ASP.NET MVC uygulamasına denetleyici ekleme](./media/vs-storage-aspnet-getting-started-blobs/add-controller-menu.png)

1. Üzerinde **İskele Ekle** iletişim kutusunda **MVC 5 denetleyici - boş**seçip **Ekle**.

    ![MVC denetleyicisi türünü belirtin](./media/vs-storage-aspnet-getting-started-blobs/add-controller.png)

1. Üzerinde **denetleyici Ekle** iletişim kutusunda, denetleyici adı *BlobsController*seçip **Ekle**.

    ![MVC Denetleyici adı](./media/vs-storage-aspnet-getting-started-blobs/add-controller-name.png)

1. Aşağıdakileri ekleyin *kullanarak* yönergeleri `BlobsController.cs` dosyası:

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="connect-to-a-storage-account-and-get-a-container-reference"></a>Bir depolama hesabına bağlanmak ve bir kapsayıcı başvurusu edinin

Bir blob kapsayıcı, BLOB'ları ve klasörleri iç içe geçmiş bir hiyerarşisidir.  Bu belgede yer alan adımlar geri kalanı, böylece kodu yeniden kullanılırlığı için kendi yöntemi yerleştirilmelidir bir blob kapsayıcısına bir başvuru gerektirir.

Aşağıdaki adımlar bağlantı dizesini kullanarak depolama hesabına bağlanmak için bir yöntem oluşturma **Web.config** ve bir kapsayıcı başvurusu oluşturun.  Bağlantı dizesi ayarı **Web.config** biçimiyle adlı `<storageaccountname>_AzureStorageConnectionString`. 

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
> Olsa bile *test blob kapsayıcısı* yok kapsayıcı ile oluşturulan henüz, bu kod bir başvuru oluşturur, bu nedenle `CreateIfNotExists` sonraki adımda gösterilen yöntemi.

## <a name="create-a-blob-container"></a>Bir blob kapsayıcı oluşturun

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

1. Çağrı `CloudBlobContainer.CreateIfNotExists` yöntemi henüz yoksa kapsayıcıyı oluşturun. `CloudBlobContainer.CreateIfNotExists` Yöntemi döndürür **true** kapsayıcı yok ve başarıyla oluşturuldu. Aksi takdirde, **false** döndürülür.    

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

1. İçinde **Çözüm Gezgini**, sağ **görünümleri** klasörünü ve bağlam menüsünden seçin **Ekle -> Yeni bir klasör**. Yeni bir klasör adı *BLOB'lar*. 
 
1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörünü sağ tıklatın **BLOB'lar**ve bağlam menüsünden seçin **Ekle -> Görünüm**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda, girin **CreateBlobContainer** Görünüm adı ' nı seçip için **Ekle**.

1. Açık `CreateBlobContainer.cshtml`ve aşağıdaki kod parçacığını gibi görünüyor şekilde değiştirin:

    ```csharp
    @{
        ViewBag.Title = "Create Blob Container";
    }
    
    <h2>Create Blob Container results</h2>

    Creation of @ViewBag.BlobContainerName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri -> paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. Son sonra **Html.ActionLink**, aşağıdakileri ekleyin **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Create blob container", "CreateBlobContainer", "Blobs")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **Blob kapsayıcısı oluşturmak** aşağıdaki ekran görüntüsüne benzer sonuçlar görmek için:
  
    ![Blob kapsayıcısı oluşturma](./media/vs-storage-aspnet-getting-started-blobs/create-blob-container-results.png)

    Daha önce belirtildiği gibi `CloudBlobContainer.CreateIfNotExists` yöntemi döndürür **doğru** yalnızca kapsayıcı yok ve oluşturulur. Bu nedenle, kapsayıcı mevcut olduğunda uygulama çalıştırırsanız, yöntem **false**.

## <a name="upload-a-blob-into-a-blob-container"></a>Bir blob kapsayıcıya bir blob karşıya yükleme

Bir kez [blob kapsayıcısı oluşturuldu](#create-a-blob-container), bu kapsayıcıya dosyaları karşıya yükleme. Bu bölümde, bir blob kapsayıcısına bir yerel dosya karşıya yükleme aracılığıyla anlatılmaktadır. Adımları adlı bir blob kapsayıcı olduğu varsayılır *test blob kapsayıcısı*. 

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

1. Daha önce açıklandığı gibi Azure depolama farklı blob türlerini destekler. Bu öğretici blok blobları kullanır.  Bir blok blob başvurusu almak için arama `CloudBlobContainer.GetBlockBlobReference` yöntemi.

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```
    
    > [!NOTE]
    > Blob adı bir blob almak için kullanılan URL'nin bir parçasıdır ve dosya adı dahil olmak üzere herhangi bir dize olmalıdır.

1. Bir blob başvurusu eklendiğinde, tüm veri akışı için blob başvurusu nesnenin çağırarak karşıya `UploadFromStream` yöntemi. `UploadFromStream` Yöntemi, mevcut değil veya mevcut değilse bu raporun üzerine blob oluşturur. (Değişiklik  *&lt;dosya karşıya yükleme >* karşıya yüklenecek bir dosyaya tam bir yol için.)

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

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri -> paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. Son sonra **Html.ActionLink**, aşağıdakileri ekleyin **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Upload blob", "UploadBlob", "Blobs")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **karşıya yükleme blob**.  Word "success!" görüntülemelidir.
    
    ![Başarısını doğrulama](./media/vs-storage-aspnet-getting-started-blobs/upload-blob.png)
  
Bölüm - [blob kapsayıcısı içinde BLOB'ları listesi](#list-the-blobs-in-a-blob-container) -blob kapsayıcısı içinde BLOB'ları listesi verilmektedir.    

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
   
1. Blob kapsayıcısı içinde BLOB'ları listelemek için kullanın `CloudBlobContainer.ListBlobs` yöntemi. `CloudBlobContainer.ListBlobs` Yöntemi döndürür bir `IListBlobItem` için cast nesnesi bir `CloudBlockBlob`, `CloudPageBlob`, veya `CloudBlobDirectory` nesnesi. Aşağıdaki kod parçacığını bir blob kapsayıcıdaki tüm blob'lara numaralandırır. Her bir blob türü ve onun adına göre uygun nesnesine cast (veya URI durumunda bir **CloudBlobDirectory**) bir listesine eklenir.

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

    BLOB'ları yanı sıra blob kapsayıcıları dizinleri içerebilir. Adlı blob kapsayıcısında olduğunu varsayın *test blob kapsayıcısı* aşağıdaki hiyerarşi ile:

        foo.png
        dir1/bar.png
        dir2/baz.png

    Önceki kod örneğinde kullanarak **BLOB'lar** dize listesi aşağıdakine benzer değerleri içerir:

        foo.png
        <storage-account-url>/test-blob-container/dir1
        <storage-account-url>/test-blob-container/dir2

    Gösterildiği gibi yalnızca üst düzey varlıklar listesi içerir; iç içe geçmiş olanlar (*bar.png* ve *baz.png*). Bir blob kapsayıcı içindeki tüm varlıkları listelemek için kodu değiştirin böylece **CloudBlobContainer.ListBlobs** yöntemi geçirilir **true** için **Listblobs** parametre.    

    ```csharp
    //...
    foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing:true))
    //...
    ```

    Ayarı **Listblobs** parametresi **true** blob kapsayıcısında tüm varlıkların düz bir liste döndürür ve aşağıdaki sonuçları verir:

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

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörünü sağ tıklatın **BLOB'lar**ve bağlam menüsünden seçin **Ekle -> Görünüm**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda, girin `ListBlobs` Görünüm adı ' nı seçip için **Ekle**.

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

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri -> paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. Son sonra **Html.ActionLink**, aşağıdakileri ekleyin **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("List blobs", "ListBlobs", "Blobs")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **listesinde BLOB'lar** aşağıdaki ekran görüntüsüne benzer sonuçlar görmek için:
  
    ![BLOB listeleme](./media/vs-storage-aspnet-getting-started-blobs/listblobs.png)

## <a name="download-blobs"></a>Blob’ları indirme

Bu bölümde bir blob indirmek nasıl gösterir ve yerel depolama alanına ya da kalıcı olması veya bir dizeye içeriği okuyun. Örnek kod başvurularını *test blob kapsayıcısı* bölümünde oluşturduğunuz [bir blob kapsayıcı oluşturun](#create-a-blob-container).

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

1. Bir blob indirmek için kullanacağınız `CloudBlockBlob.DownloadToStream` yöntemi. Aşağıdaki kod, bir blob'un içeriği sonra yerel bir dosyaya kalıcı bir akış nesnesine aktarır (değişiklik  *&lt;yerel dosya adı >* olduğu blob indirilmesi gösteren tam dosya adı.): 

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

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri -> paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. Son sonra **Html.ActionLink**, aşağıdakileri ekleyin **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Download blob", "DownloadBlob", "Blobs")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **indirme blob** blob indirmek için. Belirtilen blob `CloudBlobContainer.GetBlockBlobReference` yöntem çağrısı indirmeleri için belirttiğiniz konuma `File.OpenWrite` yöntem çağrısı.  Metin "success!" tarayıcıda görüntülemelidir. 

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

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri -> paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. Son sonra **Html.ActionLink**, aşağıdakileri ekleyin **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete blob", "DeleteBlob", "Blobs")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **Delete blob** belirtilen blobu silmek için `CloudBlobContainer.GetBlockBlobReference` yöntem çağrısı.  Metin "success!" tarayıcıda görüntülenmesi gerekir.  Tarayıcının tıklatın **geri** düğmesine ve ardından **listesinde BLOB'lar** blob kapsayıcısında artık olduğu doğrulanamadı.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, liste, depolanıp ASP.NET kullanarak Azure Storage blobları öğrendiniz.  Azure’da veri depolama ile ilgili ek seçenekler hakkında daha fazla bilgi edinmek için daha fazla özellik kılavuzu görüntüleyin.

  * [Azure tablo depolaması ve Visual Studio bağlı Hizmetleri (ASP.NET) kullanmaya başlama](vs-storage-aspnet-getting-started-tables.md)
  * [Azure kuyruk depolama ve Visual Studio bağlı Hizmetleri (ASP.NET) kullanmaya başlama](vs-storage-aspnet-getting-started-queues.md)
