---
title: Azure Blob ile çalışmaya başlama depolama ve Visual Studio bağlı hizmetler (ASP.NET Core) | Microsoft Docs
description: Başlamak nasıl bir ASP.NET Core projesi Visual Studio'da Azure Blob Depolama kullanarak Visual Studio kullanarak bir depolama hesabına bağlandıktan sonra bağlı hizmetler
services: storage
documentationcenter: ''
author: camsoper
manager: wpickett
editor: ''
ms.service: storage
ms.workload: web
ms.custom: vs-azure
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: casoper
ms.openlocfilehash: f8de7f7a263a087abb16ed48d05b2cae9834b4ad
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62110527"
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-core"></a>Azure Blob ile çalışmaya başlama depolama ve Visual Studio bağlı hizmetler (ASP.NET Core)

> [!div class="op_single_selector"]
> - [ASP.NET](./vs-storage-aspnet-getting-started-blobs.md)
> - [ASP.NET Core](./vs-storage-aspnet-core-getting-started-blobs.md)

Azure Blob Depolama, yapılandırılmamış verileri nesne veya BLOB olarak bulutta depolayan bir hizmettir. Blob Storage belge, medya dosyası veya uygulama yükleyici gibi her tür metin veya ikili veri depolayabilir. Blob Storage aynı zamanda nesne depolama olarak adlandırılır. Blob Depolama hakkında daha fazla bilgi için bkz: [Azure Blob depolamaya giriş](../storage/blobs/storage-blobs-introduction.md).

Bu öğreticide, Blob Depolama kullanan bazı genel senaryolar için ASP.NET Core kodunun nasıl yazılacağını gösterir. Bir blob kapsayıcısı oluşturma ve karşıya yükleme, listeleme, indirme ve BLOB'ları silmeden senaryolar içerir.

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="prerequisites"></a>Önkoşullar

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)

## <a name="set-up-the-development-environment"></a>Geliştirme ortamını ayarlama

Bu bölüm geliştirme ortamını ayarlama aracılığıyla size yol gösterir. Bu, ASP.NET Model View Controller (MVC) uygulama oluşturma, bağlı hizmetler bağlantı ekleme, denetleyici ekleme ve gereken ad alanının yönergelerini belirtme içerir.

### <a name="create-an-aspnet-mvc-app-project"></a>Bir ASP.NET MVC uygulaması projesi oluşturma

1. Visual Studio'yu açın.

1. Ana menüden **dosya** > **yeni** > **proje**.

1. İçinde **yeni proje** iletişim kutusunda **Web** > **ASP.NET Core Web uygulaması** > **AspNetCoreStorage**. Sonra **Tamam**’ı seçin.

    ![Ekran görüntüsü, Visual Studio yeni proje iletişim kutusu](./media/vs-storage-aspnet-core-getting-started-blobs/new-project.png)

1. İçinde **yeni ASP.NET Core Web uygulaması** iletişim kutusunda **.NET Core** > **ASP.NET Core 2.0** >  **(Web uygulaması Model-View-Controller)** . Sonra **Tamam**’ı seçin.

    ![Ekran görüntüsü, yeni ASP.NET Core Web uygulaması iletişim kutusu](./media/vs-storage-aspnet-core-getting-started-blobs/new-mvc.png)

### <a name="use-connected-services-to-connect-to-an-azure-storage-account"></a>Bir Azure depolama hesabına bağlanmak için bağlı hizmetler kullanın

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayın.

2. Bağlam menüsünden seçin **Ekle** > **bağlı hizmet**.

1. İçinde **bağlı hizmetler** iletişim kutusunda **Azure depolama ile bulut depolama**ve ardından **yapılandırma**.

    ![Bağlı hizmetler ekran iletişim kutusu](./media/vs-storage-aspnet-core-getting-started-blobs/connected-services.png)

1. İçinde **Azure depolama** Azure depolama hesabı Bu öğretici için kullanılacak iletişim kutusunda seçin. Yeni bir Azure depolama hesabı oluşturmak için Seç **yeni depolama hesabı oluşturma**ve formu doldurun. Ya da mevcut bir depolama hesabını seçmek veya yeni bir oluşturduktan sonra seçin **Ekle**. Visual Studio, Azure depolama ve bir depolama bağlantı dizesi için NuGet paketi yükler **appsettings.json**.

> [!TIP]
> Bir depolama hesabı oluşturma hakkında bilgi edinmek için [Azure portalında](https://portal.azure.com), bakın [depolama hesabı oluşturma](../storage/common/storage-quickstart-create-account.md).
>
> Kullanarak bir depolama hesabı oluşturabilirsiniz [Azure PowerShell](../storage/common/storage-powershell-guide-full.md), [Azure CLI](../storage/common/storage-azure-cli.md), veya [Azure Cloud Shell](../cloud-shell/overview.md).


### <a name="create-an-mvc-controller"></a>MVC denetleyicisi oluşturma 

1. İçinde **Çözüm Gezgini**, sağ **denetleyicileri**.

2. Bağlam menüsünden seçin **Ekle** > **denetleyicisi**.

    ![Çözüm Gezgini'nin ekran görüntüsü](./media/vs-storage-aspnet-core-getting-started-blobs/add-controller-menu.png)

1. İçinde **İskele Ekle** iletişim kutusunda **MVC denetleyicisi - boş**seçip **Ekle**.

    ![Ekran görüntüsü, İskele Ekle iletişim kutusu](./media/vs-storage-aspnet-core-getting-started-blobs/add-controller.png)

1. İçinde **boş MVC denetleyicisi Ekle** iletişim kutusunda, denetleyici adı *BlobsController*seçip **Ekle**.

    ![Ekran görüntüsü, boş MVC denetleyici Ekle iletişim kutusu](./media/vs-storage-aspnet-core-getting-started-blobs/add-controller-name.png)

1. Aşağıdaki `using` yönergelerini `BlobsController.cs` dosyası:

    ```csharp
    using System.IO;
    using Microsoft.Extensions.Configuration;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="connect-to-a-storage-account-and-get-a-container-reference"></a>Bir kapsayıcı başvurusu alın ve bir depolama hesabına bağlama

Bir blob kapsayıcı, BLOB ve klasörleri iç içe hiyerarşisini ' dir. Bu belgedeki adımlarda geri kalanı ölçeklendirilebileceği için kendi yöntemindeki kod yerleştirilmelidir. böylece bir blob kapsayıcısına bir başvuru gerektirir.

Aşağıdaki adımlar bağlantı dizesini kullanarak depolama hesabına bağlanmak için bir yöntem oluşturma **appsettings.json**. Adımları ayrıca bir başvuru için bir kapsayıcı oluşturun. Bağlantı dizesi ayarı **appsettings.json** biçimiyle adlı `<storageaccountname>_AzureStorageConnectionString`. 

1. `BlobsController.cs` dosyasını açın.

1. Adlı bir yöntem ekleyin **GetCloudBlobContainer** döndüren bir **CloudBlobContainer**. Değiştirdiğinizden emin olun `<storageaccountname>_AzureStorageConnectionString` anahtarı gerçek adıyla **Web.config**.
    
    ```csharp
    private CloudBlobContainer GetCloudBlobContainer()
    {
        var builder = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("appsettings.json");
        IConfigurationRoot Configuration = builder.Build();
        CloudStorageAccount storageAccount = 
            CloudStorageAccount.Parse(Configuration["ConnectionStrings:aspnettutorial_AzureStorageConnectionString"]);
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
    ViewBag.Success = container.CreateIfNotExistsAsync().Result;
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
        ViewBag.Success = container.CreateIfNotExistsAsync().Result;
        ViewBag.BlobContainerName = container.Name;

        return View();
    }
    ```

1. İçinde **Çözüm Gezgini**, sağ **görünümleri** klasör.

2. Bağlam menüsünden seçin **Ekle** > **yeni klasör**. Yeni klasör adı *Blobları*. 

1. İçinde **Çözüm Gezgini**, genişletme **görünümleri** klasörünü açın ve sağ **Blobları**.

4. Bağlam menüsünden seçin **Ekle** > **görünümü**.

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

1. Şuna benzer sırasız liste arayın: `<ul class="nav navbar-nav">`.  En son `<li>` liste öğesine başka gezinme menü öğesi eklemek için aşağıdaki HTML'yi ekleyin:

    ```html
    <li><a asp-area="" asp-controller="Blobs" asp-action="CreateBlobContainer">Create blob container</a></li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **Blob kapsayıcısı Oluştur** aşağıdaki ekran görüntüsüne benzer bir sonuç görmek için:
  
    ![Blob kapsayıcısı Oluştur ekran görüntüsü](./media/vs-storage-aspnet-core-getting-started-blobs/create-blob-container-results.png)

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
        blob.UploadFromStreamAsync(fileStream).Wait();
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
            blob.UploadFromStreamAsync(fileStream).Wait();
        }
        return "success!";
    }
    ```

1. İçinde **Çözüm Gezgini**, genişletme **görünümleri** > **paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. En son `<li>` liste öğesine başka gezinme menü öğesi eklemek için aşağıdaki HTML'yi ekleyin:

    ```html
    <li><a asp-area="" asp-controller="Blobs" asp-action="UploadBlob">Upload blob</a></li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **blobu karşıya yükleme**. Word *başarılı!* görüntülenmesi gerekir.
    
    ![Ekran görüntüsü başarısını doğrulama](./media/vs-storage-aspnet-core-getting-started-blobs/upload-blob.png)
  
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
   
1. Bir blob kapsayıcıdaki blobları listelemek için kullanın `CloudBlobContainer.ListBlobsSegmentedAsync` yöntemi. `CloudBlobContainer.ListBlobsSegmentedAsync` Yöntemi döndürür bir `BlobResultSegment`. Bu içeren `IListBlobItem` atanabilecek nesneleri `CloudBlockBlob`, `CloudPageBlob`, veya `CloudBlobDirectory` nesneleri. Aşağıdaki kod parçacığı, bir blob kapsayıcıdaki tüm blob'lara numaralandırır. Her blobun kendi türüne göre uygun nesne türüne dönüştürülür. Adı (veya URI durumunda bir `CloudBlobDirectory`) bir listesine eklenir.

    ```csharp
    List<string> blobs = new List<string>();
    BlobResultSegment resultSegment = container.ListBlobsSegmentedAsync(null).Result;
    foreach (IListBlobItem item in resultSegment.Results)
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
    Aşağıdaki tamamlanmış gösterir `ListBlobs` yöntemi:

    ```csharp
    public ActionResult ListBlobs()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        List<string> blobs = new List<string>();
        BlobResultSegment resultSegment = container.ListBlobsSegmentedAsync(null).Result;
        foreach (IListBlobItem item in resultSegment.Results)
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

1. En son `<li>` liste öğesine başka gezinme menü öğesi eklemek için aşağıdaki HTML'yi ekleyin:

    ```html
    <li><a asp-area="" asp-controller="Blobs" asp-action="ListBlobs">List blobs</a></li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **listeleme** aşağıdaki ekran görüntüsüne benzer bir sonuç görmek için:
  
    ![Ekran listesi BLOB'ları](./media/vs-storage-aspnet-core-getting-started-blobs/listblobs.png)

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
        blob.DownloadToStreamAsync(fileStream).Wait();
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
            blob.DownloadToStreamAsync(fileStream).Wait();
        }
        return "success!";
    }
    ```

1. İçinde **Çözüm Gezgini**, genişletme **görünümleri** > **paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. En son `<li>` liste öğesine başka gezinme menü öğesi eklemek için aşağıdaki HTML'yi ekleyin:

    ```html
    <li><a asp-area="" asp-controller="Blobs" asp-action="DownloadBlob">Download blob</a></li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **indirme blob** blobu indirmek için. Belirtilen blob `CloudBlobContainer.GetBlockBlobReference` yöntem çağrısının indirir, belirtilen konuma `File.OpenWrite` yöntem çağrısı. Metin *başarılı!* tarayıcı içinde görüntülenmesi gerekir. 

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
    blob.DeleteAsync().Wait();
    ```
    
    Tamamlanan `DeleteBlob` yöntemi aşağıdaki gibi görünmelidir:
    
    ```csharp
    public string DeleteBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        blob.DeleteAsync().Wait();
        return "success!";
    }
    ```

1. İçinde **Çözüm Gezgini**, genişletme **görünümleri** > **paylaşılan** klasörü ve açık `_Layout.cshtml`.

1. En son `<li>` liste öğesine başka gezinme menü öğesi eklemek için aşağıdaki HTML'yi ekleyin:

    ```html
    <li><a asp-area="" asp-controller="Blobs" asp-action="DeleteBlob">Delete blob</a></li>
    ```

1. Uygulamayı çalıştırmak ve seçmek **silme blob** belirtilen blob silinemedi `CloudBlobContainer.GetBlockBlobReference` yöntem çağrısı. Metin *başarılı!* tarayıcı içinde görüntülenmesi gerekir. Tarayıcının seçin **geri** düğmesini ve ardından **listeleme** blob kapsayıcısında artık olduğu doğrulanamadı.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, depolamak, liste ve ASP.NET Core kullanarak Azure Depolama'daki blobları almak öğrendiniz. Azure’da veri depolama ile ilgili ek seçenekler hakkında daha fazla bilgi edinmek için daha fazla özellik kılavuzu görüntüleyin.

  * [Azure tablosu ile çalışmaya başlama depolama ve Visual Studio bağlı hizmetler (ASP.NET)](vs-storage-aspnet-getting-started-tables.md)
  * [Azure kuyruk ile çalışmaya başlama depolama ve Visual Studio bağlı hizmetler (ASP.NET)](vs-storage-aspnet-getting-started-queues.md)
