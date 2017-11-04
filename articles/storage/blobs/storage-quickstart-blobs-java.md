---
title: "Azure hızlı başlangıç - aktarımı nesneleri/Java kullanarak Azure Blob depolama biriminden | Microsoft Docs"
description: "Hızlı bir şekilde nesneleri için/Java kullanarak Azure Blob storage aktarım öğrenin"
author: roygara
manager: timlt
services: storage
ms.service: storage
ms.topic: quickstart
ms.date: 11/01/2017
ms.author: v-rogara
ms.custom: mvc
ms.openlocfilehash: 4c917a500e8d230c5b9885d9c0a96424201588f7
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-java"></a>Aktarım nesneleri/Java kullanarak Azure Blob depolama biriminden

Bu hızlı başlangıç Java karşıya yükleyin, indirin ve blok blobları Azure Blob Depolama birimindeki bir kapsayıcıda listelemek için nasıl kullanılacağını öğrenin.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için:

* Maven tümleştirmesine sahip olduğu bir IDE yükleyin

* Alternatif olarak, yükleme ve komut satırından çalışması için Maven yapılandırma

Bu öğretici kullanır [Eclipse](http://www.eclipse.org/downloads/) "Eclipse IDE için Java geliştiricilerinin" yapılandırmasına sahip.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin:

[Örnek uygulama](https://github.com/Azure-Samples/storage-blobs-java-quickstart) Bu hızlı başlangıç kullanılan bir temel konsol uygulamasıdır. 

Kullanım [git](https://git-scm.com/) geliştirme ortamınızı uygulamaya bir kopyasını indirmek için. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-java-quickstart.git
```

Bu komut, yerel git klasörünüze depoya klonlar. Projeyi açmak için Eclipse başlatın ve Hoş Geldiniz ekranında kapatın. Seçin **dosya** sonra **dosya sisteminden projeleri Aç...** . Emin olun **Algıla ve proje natures yapılandırma** denetlenir. Seçin **Directory** içindeki kopyalanan depo depolandığı için gidin seçin **javaBlobsQuickstart** klasör. Emin olun **javaBlobsQuickstarts** proje görünür bir Eclipse projesi olarak ardından select **son**.

Projeyi içeri aktarma tamamlandıktan sonra açmak **AzureApp.java** (bulunan **blobQuickstart.blobAzureApp** içine **src/main/java**) ve değiştirme`accountname`ve `accountkey` içine `storageConnectionString` dize. Uygulamayı çalıştırın.
     

## <a name="configure-your-storage-connection-string"></a>Depolama bağlantı dizelerinizi yapılandırma
    
Uygulamada, depolama hesabınız için bağlantı dizesi belirtmeniz gerekir. Açık **AzureApp.Java** dosya. Bul `storageConnectionString` değişkeni. Değiştir `AccountName` ve `AccountKey` Azure portalından kaydettiğiniz değerlerle bağlantı dizesinde değerleri. `storageConnectionString` Aşağıdakine benzer görünmelidir:

```java
    public static final String storageConnectionString ="DefaultEndpointsProtocol=https;" +
     "AccountName=<Namehere>;" +
    "AccountKey=<Keyhere>";
```

## <a name="run-the-sample"></a>Örnek çalıştırın

Bu örnek, varsayılan dizininizde (Belgelerim, windows kullanıcıları için) bir test dosyası oluşturur, Blob depolama alanına yükler, kapsayıcıda BLOB'ları listeler ve sonra eski ve yeni dosyalar karşılaştırmak için dosyanın yeni bir adla yükler. 

Örnek basarak çalıştırmak **Ctrl + F11** eclipse'te.

Maven komut satırı kullanarak örneği çalıştırmak istiyorsanız, bir Kabuğu'nu açın ve gidin **blobAzureApp** kopyalanan dizininize içinde. Enter `mvn compile exec:java`.

Windows üzerinde uygulamayı çalıştırmak için olsaydı çıktısı örneği verilmiştir.

```
Location of file: C:\Users\<user>\Documents\results.txt
File has been written
URI of blob is: http://myexamplesacct.blob.core.windows.net/quickstartblobs/results.txt
The program has completed successfully.
Press the 'Enter' key while in the console to delete the sample files, example container, and exit the application.
```

 Devam etmeden önce iki dosya için varsayılan dizini (windows kullanıcıları için Belgelerim klasörünü) denetleyin. Ekleri açmak ve aynı görebilirsiniz. Konsol penceresi dışında blob URL'sini kopyalayın ve Blob depolama alanına dosyasının içeriğini görüntülemek için bir tarayıcıya yapıştırın. Enter tuşuna bastığınızda, depolama kapsayıcısı ve dosyaları siler.

Bir aracı gibi kullanabilir [Azure Storage Gezgini](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) Blob storage'da dosyaları görüntülemek için. Azure Depolama Gezgini, depolama hesabı bilgilerinizi erişmenize olanak sağlayan ücretsiz bir platformlar arası aracıdır. 

Dosyaları doğrulandıktan sonra tanıtım ve test dosyalarını silmeniz enter tuşuna basın. Örnek yaptığı bildiğinize göre açık **AzureApp.java** dosya koda bakın. 

## <a name="get-references-to-the-storage-objects"></a>Depolama nesneleri başvuruları alma

Yapılacak ilk şey erişmek ve Blob Depolama yönetmek için kullanılan nesnelerin referansları oluşturmaktır. Bu nesneler birbirine yapı--her listedeki bir sonraki tarafından kullanılır.

* Bir örneğini oluşturmak **CloudStorageAccount** gösteren nesne [depolama hesabı](/java/api/com.microsoft.azure.management.storage._storage_account). 

* Bir örneğini oluşturmak **CloudBlobClient** işaret nesnesi [Blob hizmeti](/java/api/com.microsoft.azure.storage.blob._cloud_blob_client) depolama hesabınızdaki. 

* Bir örneğini oluşturmak **CloudBlobContainer** temsil eden nesne [kapsayıcı](/java/api/com.microsoft.azure.storage.blob._cloud_blob_container) eriştiğiniz. Kapsayıcıları dosyalarınızı düzenlemek için bilgisayarınızda klasörleri kullanmak gibi bloblarınızın düzenlemek için kullanılır.

Bulduktan sonra **CloudBlobContainer**, bir örneğini oluşturabilirsiniz **CloudBlockBlob** belirli işaret nesnesi [blob](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob) hangi, ilgilendiğiniz içinde ve karşıya yükleme, yükleme, kopyalama, vb. işlemi gerçekleştirin.

> [!IMPORTANT]
> Kapsayıcı adları küçük harf olmalıdır. Bkz: [adlandırma ve başvuran kapsayıcıları, Blobları ve meta veri](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) kapsayıcı ve blob adları hakkında daha fazla bilgi için.

Bu bölümde, nesne örneğini oluşturmak, yeni bir kapsayıcı oluşturmak ve böylece BLOB genel ve yalnızca bir URL ile erişilebilir kapsayıcısında izinleri ayarlama. Kapsayıcı adı verilen **quickstartblobs**. 

Bu örnekte **CreateIfNotExists** her zaman yeni bir kapsayıcı oluşturmak istiyoruz çünkü örneğini çalıştırın. Bir uygulama boyunca aynı kapsayıcı kullandığınız bir üretim ortamında, yalnızca çağırmak için daha iyi bir uygulamadır **CreateIfNotExists** sonra. Alternatif olarak, kodda oluşturabilir gerek kalmaması önceden kapsayıcı oluşturabilirsiniz.

```java
CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
        
CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

// Get a reference to a container.
// The container name must be lower case
CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

// Create the container if it does not exist.
container.createIfNotExists();

// Create a permissions object.
BlobContainerPermissions containerPermissions = new BlobContainerPermissions();

// Include public access in the permissions object.
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);

// Set the permissions on the container.
container.uploadPermissions(containerPermissions);
```

## <a name="upload-blobs-to-the-container"></a>BLOB kapsayıcıya karşıya yükle

Blob depolama blok blobları, ekleme bloblarını ve sayfa bloblarını destekler. Blok blobları en yaygın olarak kullanılır ve bu hızlı başlangıç içinde kullanılan olmasıdır. 

Bir blobu bir dosyayı karşıya yüklemek için hedef kapsayıcıda blob başvurusu alın. Blob başvurusu edindiğinizde, veri kendisine kullanarak karşıya yükleyebilirsiniz [CloudBlockBlob.Upload](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.upload#com_microsoft_azure_storage_blob__cloud_block_blob_upload_final_InputStream_final_long). Bu işlem zaten mevcut değil veya zaten mevcut değilse bu raporun üzerine blob oluşturur.

Örnek kod, yükleme ve indirme, olarak karşıya yüklenecek dosyayı depolamak için kullanılacak yerel bir dosya oluşturur **kaynak** ve içinde blob adını **blob**. Aşağıdaki örnek dosya adında, kapsayıcıya yüklemeleri **quickstartblobs**.

```java
//Getting the path to user's myDocuments folder
String myDocs = FileSystemView.getFileSystemView().getDefaultDirectory().getPath();

//Creating a file in the myDocuments folder
File source = new File(myDocs + File.separator + "results.txt");
try( Writer output = new BufferedWriter(new FileWriter(source)))
{
    System.out.println("Location of file: " + source.toString());

    output.write("Hello Azure!");
    output.close();
    
    System.out.println("File has been written");
}
catch(IOException x) 
{
    System.err.println(x);
}

//Getting blob reference
CloudBlockBlob blob = container.getBlockBlobReference("results.txt");

//Creating a FileInputStream as it is necessary for the upload
FileInputStream myFile = new FileInputStream(source);

//Creating blob and uploading file to it
blob.upload( myFile, source.length());

//Closing FileInputStream
myFile.close();
```

Blob storage ile kullanabileceğiniz çeşitli karşıya yükleme yöntemler vardır. Örneğin, bir bellek akış varsa UploadFromFileAsync yerine UploadFromStreamAsync yöntemi kullanabilirsiniz. 

Blok blobları, metin veya ikili dosya herhangi bir türde olabilir. Sayfa blobları Iaas sanal makineleri yedeklemek için kullanılan VHD dosyalarını için birincil olarak kullanılır. Ekleme blobları gibi bir dosyaya yazmak ve daha fazla bilgi ekleme tutmak istediğiniz günlük için kullanılır. BLOB storage'da depolanan çoğu blok blobları nesneleridir.

## <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

Kapsayıcı kullanarak dosyaların bir listesini alabilirsiniz [CloudBlobContainer.ListBlobs](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.listblobs#com_microsoft_azure_storage_blob__cloud_blob_container_listBlobs). Aşağıdaki kod BLOB'lar listesini alır ve ardından bunları bulunan blobların URI'ler gösteren döngü. Komut penceresinde URI'yi kopyalayın ve dosyayı görüntülemek için bir tarayıcıya yapıştırın.

```java
//Listing contents of container
for (ListBlobItem blobItem : container.listBlobs()) {
    System.out.println("URI of blob is: " + blobItem.getUri());
}
```

## <a name="download-blobs"></a>Blob’ları indirme

Kullanarak yerel disk blobları indirmek [CloudBlob.DownloadToFile](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob.downloadtofile#com_microsoft_azure_storage_blob__cloud_blob_downloadToFile_final_String).

Aşağıdaki kod, önceki bölümde, yerel diskteki hem dosyaları görebilmeniz için blob adı "_DOWNLOADED" sonekine ekleme karşıya blob indirir. 

```java
// Download blob. In most cases, you would have to retrieve the reference
// to cloudBlockBlob here. However, we created that reference earlier, and 
// haven't changed the blob we're interested in, so we can reuse it. 
// First, add a _DOWNLOADED before the .txt so you can see both files in your default directory.
blob.downloadToFile(myDocs + File.separator + "results_DOWNLOADED.txt");
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıcı karşıya BLOB'ları artık ihtiyacınız varsa, tüm kapsayıcı kullanarak silebilirsiniz [CloudBlobContainer.DeleteIfExists](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.deleteifexists#com_microsoft_azure_storage_blob__cloud_blob_container_deleteIfExists). Bu kapsayıcı içindeki dosyaları da siler.

```java
//Deletes container if it exists then deletes files created locally
container.deleteIfExists();
downloadedFile.deleteOnExit();
sourceFile.deleteOnExit();
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç yerel disk ve Java kullanarak Azure Blob storage arasında dosyaları aktarmak nasıl öğrendiniz. Blob storage ile çalışma hakkında daha fazla bilgi için nasıl yapılır Blob depolama alanına devam edin.

> [!div class="nextstepaction"]
> [BLOB Depolama işlemleri nasıl yapılır konuları](storage-java-how-to-use-blob-storage.md)

BLOB'ları ve Depolama Gezgini hakkında daha fazla bilgi için bkz: [Depolama Gezgini ile yönetme Azure Blob storage kaynaklarını](../../vs-azure-tools-storage-explorer-blobs.md).