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
ms.date: 12/21/2016
ms.author: kraig
ms.openlocfilehash: e953c7978705379a28581213e8f1c665473ddd60
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet"></a>Azure blob depolama ve Visual Studio bağlı Hizmetleri (ASP.NET) kullanmaya başlama
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Genel Bakış

Azure blob depolama bulutta nesne/BLOB olarak yapılandırılmamış veri depolayan bir hizmettir. Blob Storage belge, medya dosyası veya uygulama yükleyici gibi her tür metin veya ikili veri depolayabilir. Blob Storage aynı zamanda nesne depolama olarak adlandırılır.

Bu öğretici, Azure blob storage kullanarak bazı genel senaryolar için ASP.NET kodunun nasıl yazılacağını gösterir. Bir blob kapsayıcısını oluşturma ve karşıya yükleme, listeleme, indirme ve BLOB'ları silme senaryolar içerir.

##<a name="prerequisites"></a>Ön koşullar

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure depolama hesabı](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

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
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="create-a-blob-container"></a>Bir blob kapsayıcı oluşturun

Bir blob kapsayıcı, BLOB'ları ve klasörleri iç içe geçmiş bir hiyerarşisidir. Aşağıdaki adımları bir blob kapsayıcı oluşturulacağını gösterir:

> [!NOTE]
> 
> Bu bölümdeki kod bölümündeki adımları tamamladınız varsayar [geliştirme ortamını ayarlama](#set-up-the-development-environment). 

1. `BlobsController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **CreateBlobContainer** döndüren bir **ActionResult**.

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. İçinde **CreateBlobContainer** yöntemi, get bir **CloudStorageAccount** depolama hesabı bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın. (Değişiklik  *&lt;depolama hesabı adı >* Azure depolama hesabı adını eriştiğiniz.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Alma bir **CloudBlobClient** nesnesi, bir blob hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Alma bir **CloudBlobContainer** istenen blob kapsayıcı adı için başvuru temsil eden nesne. **CloudBlobClient.GetContainerReference** yöntemi blob Storage'a karşı istek yapmaz. Blob kapsayıcısı mevcut olup olmadığına bakılmaksızın başvuru döndürülür. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Çağrı **CloudBlobContainer.CreateIfNotExists** yöntemi henüz yoksa kapsayıcıyı oluşturun. **CloudBlobContainer.CreateIfNotExists** yöntemi döndürür **true** kapsayıcı yok ve başarıyla oluşturuldu. Aksi takdirde, **false** döndürülür.    

    ```csharp
    ViewBag.Success = container.CreateIfNotExists();
    ```

1. Güncelleştirme **ViewBag** blob kapsayıcı adı.

    ```csharp
    ViewBag.BlobContainerName = container.Name;
    ```

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
  
    ![BLOB kapsayıcı oluşturun](./media/vs-storage-aspnet-getting-started-blobs/create-blob-container-results.png)

    Daha önce belirtildiği gibi **CloudBlobContainer.CreateIfNotExists** yöntemi döndürür **doğru** yalnızca kapsayıcı yok ve oluşturulur. Bu nedenle, kapsayıcı mevcut olduğunda uygulama çalıştırırsanız, yöntem **false**. Birden çok kez uygulamayı çalıştırmak için uygulamayı yeniden çalıştırmadan önce kapsayıcı silmeniz gerekir. Aracılığıyla kapsayıcı silme yapılabilir **CloudBlobContainer.Delete** yöntemi. Bir kapsayıcı kullanılarak silebilirsiniz [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) veya [Microsoft Azure Storage Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md).  

## <a name="upload-a-blob-into-a-blob-container"></a>Bir blob kapsayıcıya bir blob karşıya yükleme

Seçtiğiniz sonra [bir blob kapsayıcısını oluşturulan](#create-a-blob-container), bu kapsayıcıya dosyaları karşıya yükleyebilir. Bu bölümde, bir blob kapsayıcısına bir yerel dosya karşıya yükleme aracılığıyla açıklanmaktadır. Adlı bir blob kapsayıcı oluşturduğunuz adımlarda varsayılır *test blob kapsayıcısı*. 

> [!NOTE]
> 
> Bu bölümdeki kod bölümündeki adımları tamamladınız varsayar [geliştirme ortamını ayarlama](#set-up-the-development-environment). 

1. `BlobsController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **UploadBlob** döndüren bir **EmptyResult**.

    ```csharp
    public EmptyResult UploadBlob()
    {
        // The code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. İçinde **UploadBlob** yöntemi, get bir **CloudStorageAccount** depolama hesabı bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (değişiklik  *&lt;depolama hesabı adı >* Azure depolama hesabı adını eriştiğiniz.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Alma bir **CloudBlobClient** nesnesi, bir blob hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Alma bir **CloudBlobContainer** blob kapsayıcı adı için başvuru temsil eden nesne. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Daha önce açıklandığı gibi Azure depolama farklı blob türlerini destekler. Bir sayfa blob'u için bir başvuru almak için arama **CloudBlobContainer.GetPageBlobReference** yöntemi. Bir blok blob başvurusu almak için arama **CloudBlobContainer.GetBlockBlobReference** yöntemi. Genellikle, blok blobu kullanmak için önerilen türüdür. (Değiştir < blob-adı > * kez karşıya blob vermek istediğiniz ad.)

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. Bir blob başvurusu edindiğinizde, tüm veri akışı için blob başvurusu nesnenin çağırarak karşıya yükleyebilirsiniz **UploadFromStream** yöntemi. **UploadFromStream** yöntemi, mevcut değil veya mevcut değilse bu raporun üzerine blob oluşturur. (Değişiklik  *&lt;dosya karşıya yükleme >* karşıya yüklemek istediğiniz dosyasının tam yolunu için.)

    ```csharp
    using (var fileStream = System.IO.File.OpenRead(<file-to-upload>))
    {
        blob.UploadFromStream(fileStream);
    }
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörünü sağ tıklatın **BLOB'lar**ve bağlam menüsünden seçin **Ekle -> Görünüm**.

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri -> paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. Son sonra **Html.ActionLink**, aşağıdakileri ekleyin **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Upload blob", "UploadBlob", "Blobs")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **karşıya yükleme blob**.  
  
Bölüm - [blob kapsayıcısı içinde BLOB'ları listesi](#list-the-blobs-in-a-blob-container) -blob kapsayıcısı içinde BLOB'ları listesi verilmektedir.    

## <a name="list-the-blobs-in-a-blob-container"></a>Blob kapsayıcısı içinde BLOB'ları Listele

Bu bölümde, bir blob kapsayıcısı içinde BLOB'ları listesi göstermektedir. Örnek kod başvurularını *test blob kapsayıcısı* bölümünde oluşturduğunuz [bir blob kapsayıcı oluşturun](#create-a-blob-container).

> [!NOTE]
> 
> Bu bölümdeki kod bölümündeki adımları tamamladınız varsayar [geliştirme ortamını ayarlama](#set-up-the-development-environment). 

1. `BlobsController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **ListBlobs** döndüren bir **ActionResult**.

    ```csharp
    public ActionResult ListBlobs()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. İçinde **ListBlobs** yöntemi, get bir **CloudStorageAccount** depolama hesabı bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (değişiklik  *&lt;depolama hesabı adı >* Azure depolama hesabı adını eriştiğiniz.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Alma bir **CloudBlobClient** nesnesi, bir blob hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Alma bir **CloudBlobContainer** blob kapsayıcı adı için başvuru temsil eden nesne. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Blob kapsayıcısı içinde BLOB'ları listelemek için kullanın **CloudBlobContainer.ListBlobs** yöntemi. **CloudBlobContainer.ListBlobs** yöntemi döndürür bir **Ilistblobıtem** için cast nesnesi bir **CloudBlockBlob**, **CloudPageBlob**, veya **CloudBlobDirectory** nesnesi. Aşağıdaki kod parçacığını bir blob kapsayıcıdaki tüm blob'lara numaralandırır. Her bir blob türü ve onun adına göre uygun nesnesine cast (veya URI durumunda bir **CloudBlobDirectory**) bir listesine eklenir.

    ```csharp
    List<string> blobs = new List<string>();

    foreach (IListBlobItem item in container.ListBlobs(null, false))
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

    BLOB'ları yanı sıra blob kapsayıcıları dizinleri içerebilir. Şimdi adlı bir blob kapsayıcıya sahip varsayalım *test blob kapsayıcısı* aşağıdaki hiyerarşi ile:

        foo.png
        dir1/bar.png
        dir2/baz.png

    Önceki kod örneğinde kullanarak **BLOB'lar** dize listesi aşağıdakine benzer değerleri içerir:

        foo.png
        <storage-account-url>/test-blob-container/dir1
        <storage-account-url>/test-blob-container/dir2

    Gördüğünüz gibi yalnızca üst düzey varlıklar listesi içerir; iç içe geçmiş olanlar (*bar.png* ve *baz.png*). Bir blob kapsayıcı içindeki tüm varlıkları listelemek için çağırmalısınız **CloudBlobContainer.ListBlobs** yöntemi ve geçişi **true** için **Listblobs** parametresi.    

    ```csharp
    ...
    foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing:true))
    ...
    ```

    Ayarı **Listblobs** parametresi **true** blob kapsayıcısında tüm varlıkların düz bir liste döndürür ve aşağıdaki sonuçları verir:

        foo.png
        dir1/bar.png
        dir2/baz.png

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri** klasörünü sağ tıklatın **BLOB'lar**ve bağlam menüsünden seçin **Ekle -> Görünüm**.

1. Üzerinde **Görünüm Ekle** iletişim kutusunda, girin **ListBlobs** Görünüm adı ' nı seçip için **Ekle**.

1. Açık `ListBlobs.cshtml`ve aşağıdaki kod parçacığını gibi görünüyor şekilde değiştirin:

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

1. Adlı bir yöntem ekleyin **DownloadBlob** döndüren bir **ActionResult**.

    ```csharp
    public EmptyResult DownloadBlob()
    {
        // The code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. İçinde **DownloadBlob** yöntemi, get bir **CloudStorageAccount** depolama hesabı bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (değişiklik  *&lt;depolama hesabı adı >* Azure depolama hesabı adını eriştiğiniz.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Alma bir **CloudBlobClient** nesnesi, bir blob hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Alma bir **CloudBlobContainer** blob kapsayıcı adı için başvuru temsil eden nesne. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Bir blob başvurusu nesnesi çağırarak alma **CloudBlobContainer.GetBlockBlobReference** veya **CloudBlobContainer.GetPageBlobReference** yöntemi. (Değişiklik  *&lt;blob adı >* blob adını karşıdan yükleniyor.)

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. Bir blob indirmek için kullanacağınız **CloudBlockBlob.DownloadToStream** veya **CloudPageBlob.DownloadToStream** blob türüne bağlı olarak yöntemi. Aşağıdaki kod parçacığını kullanır **CloudBlockBlob.DownloadToStream** yöntemi bir blob'un içeriği sonra yerel bir dosyaya kalıcı bir akış nesnesine aktarmak için: (değişiklik  *&lt;yerel dosya adı >* tam dosya adı blob istediğiniz temsil eden indirilen.) 

    ```csharp
    using (var fileStream = System.IO.File.OpenWrite(<local-file-name>))
    {
        blob.DownloadToStream(fileStream);
    }
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri -> paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. Son sonra **Html.ActionLink**, aşağıdakileri ekleyin **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Download blob", "DownloadBlob", "Blobs")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **indirme blob** blob indirmek için. Belirtilen blob **CloudBlobContainer.GetBlockBlobReference** yöntem çağrısı indirir, belirttiğiniz konuma **File.OpenWrite** yöntem çağrısı. 

## <a name="delete-blobs"></a>Blob’ları silme

Aşağıdaki adımlar bir blobu silmek nasıl gösterilmektedir:

> [!NOTE]
> 
> Bu bölümdeki kod bölümündeki adımları tamamladınız varsayar [geliştirme ortamını ayarlama](#set-up-the-development-environment). 

1. `BlobsController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **DeleteBlob** döndüren bir **ActionResult**.

    ```csharp
    public EmptyResult DeleteBlob()
    {
        // The code in this section goes here.

        return new EmptyResult();
    }
    ```

1. Alma bir **CloudStorageAccount** depolama hesabı bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın: (değişiklik  *&lt;depolama hesabı adı >* Azure depolama hesabı adını eriştiğiniz.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Alma bir **CloudBlobClient** nesnesi, bir blob hizmeti istemcisi temsil eder.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Alma bir **CloudBlobContainer** blob kapsayıcı adı için başvuru temsil eden nesne. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Bir blob başvurusu nesnesi çağırarak alma **CloudBlobContainer.GetBlockBlobReference** veya **CloudBlobContainer.GetPageBlobReference** yöntemi. (Değişiklik  *&lt;blob adı >* blob adını silmekte olduğunuz.)

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
        ```

1. To delete a blob, use the **Delete** method.

    ```csharp
    blob.Delete();
    ```

1. İçinde **Çözüm Gezgini**, genişletin **görünümleri -> paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. Son sonra **Html.ActionLink**, aşağıdakileri ekleyin **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete blob", "DeleteBlob", "Blobs")</li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **Delete blob** belirtilen blobu silmek için **CloudBlobContainer.GetBlockBlobReference** yöntem çağrısı. 

## <a name="next-steps"></a>Sonraki adımlar
Azure’da veri depolama ile ilgili ek seçenekler hakkında daha fazla bilgi edinmek için daha fazla özellik kılavuzu görüntüleyin.

  * [Azure tablo depolaması ve Visual Studio bağlı Hizmetleri (ASP.NET) kullanmaya başlama](vs-storage-aspnet-getting-started-tables.md)
  * [Azure kuyruk depolama ve Visual Studio bağlı Hizmetleri (ASP.NET) kullanmaya başlama](vs-storage-aspnet-getting-started-queues.md)
