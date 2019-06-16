---
title: Java v7 için istemci kitaplığını kullanarak Azure Depolama'daki bir blob oluşturma | Microsoft Docs
description: Nesne (Blob) depolamada depolama hesabı ve kapsayıcı oluşturun. Ardından Azure Depolama'ya blob yüklemek, blob indirmek ve bir kapsayıcıdaki blobları listelemek için Java v7 için Azure depolama istemci kitaplığı kullanın.
services: storage
author: mhopkins-msft
ms.custom: mvc
ms.service: storage
ms.topic: conceptual
ms.date: 02/04/2019
ms.author: mhopkins
ms.reviewer: seguler
ms.openlocfilehash: f7cae5b3c7b0a7da6420674635ff9c3420a6436a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65154407"
---
# <a name="how-to-upload-download-and-list-blobs-using-the-client-library-for-java-v7"></a>Karşıya yükleme, indirme ve Java v7 için istemci kitaplığını kullanarak blobları Listele

Bu nasıl yapılır kılavuzunda Java v7 karşıya yükleme, indirme ve Azure Blob depolamadaki bir kapsayıcıda blok bloblarını listesi için istemci kitaplığını kullanmayı öğrenin.

> [!TIP]
> Java için Azure depolama istemci Kitaplığı'nın en son sürümü v10 ' dir. Microsoft, mümkün olduğunda istemci Kitaplığı'nın en son sürümünü kullanmanızı önerir. V10 kullanmaya başlamak için bkz: [hızlı başlangıç: Karşıya yükleme, indirme ve Java için depolama SDK'sı V10 kullanarak blobları listeleme](storage-quickstart-blobs-java-v10.md).

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Ayrıca bir Azure depolama hesabı oluşturun [Azure portalında](https://portal.azure.com/#create/Microsoft.StorageAccount-ARM). Hesap oluşturmayla ilgili yardım için bkz. [Depolama hesabı oluşturma](../common/storage-quickstart-create-account.md).

Aşağıdaki önkoşulların karşılandığından emin olun:

* Maven tümleştirmesine sahip bir IDE yükleyin.

* Alernatif olarak, Maven’ı yükleyip komut satırından çalışacak şekilde de yapılandırabilirsiniz.

Bu kılavuzda kullanan [Eclipse](https://www.eclipse.org/downloads/) "Eclipse IDE Java geliştiriciler için" yapılandırmaya sahip.

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin:

[Örnek uygulama](https://github.com/Azure-Samples/storage-blobs-java-quickstart) temel bir konsol uygulamasıdır.  

Uygulamanın bir kopyasını geliştirme ortamınıza indirmek için [Git](https://git-scm.com/)'i kullanın. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-java-quickstart.git
```

Bu komut, depoyu yerel Git klasörünüze kopyalar. Projeyi açmak için, Eclipse’ı başlatın ve Karşılama ekranını kapatın. **Dosya**’yı ve ardından **Dosya Sisteminden Proje Aç**’ı seçin. **Proje niteliklerini algıla ve yapılandır** seçeneğinin belirlenmiş olduğundan emin olun. **Dizin**’i seçin ve ardından kopyalanan dizini depoladığınız konuma gidin. Kopyalanan deponun içinde **blobAzureApp** klasörünü seçin. **blobAzureApp** projesinin Eclipse projesi olarak göründüğünden emin olun ve ardından **Son**’u seçin.

Projenin içeri aktarılması tamamlandığında, **AzureApp.java**’yı açın (**src/main/java** içinde, **blobQuickstart.blobAzureApp** konumundadır) ve `storageConnectionString` dizesindeki `accountname` ve `accountkey` öğelerini değiştirin. Sonra, uygulamayı çalıştırın. Aşağıdaki bölümlerde bu görevleri tamamlamak için gereken özel yönergeler açıklanmıştır.

[!INCLUDE [storage-copy-connection-string-portal](../../../includes/storage-copy-connection-string-portal.md)]    

## <a name="configure-your-storage-connection-string"></a>Depolama bağlantı dizelerinizi yapılandırma
    
Uygulamada, depolama hesabınız için bağlantı dizesi sağlamanız gerekir. **AzureApp.Java** dosyasını açın. `storageConnectionString` değişkenini bulun ve önceki bölümde kopyaladığınız bağlantı dizesi değerini yapıştırın. `storageConnectionString` değişkeni, aşağıdaki kod örneğine benzer olmalıdır:

```java
public static final String storageConnectionString =
"DefaultEndpointsProtocol=https;" +
"AccountName=<account-name>;" +
"AccountKey=<account-key>";
```

## <a name="run-the-sample"></a>Örneği çalıştırma

Bu örnek uygulama, varsayılan dizininizde (Windows kullanıcıları için *C:\Users\<kullanıcı>\AppData\Local\Temp*) bir test dosyası oluşturur, bunu Blob depolamaya yükler, blobları kapsayıcıda listeler ve ardından eski ve yeni dosyaları karşılaştırabilmeniz için dosyayı yeni bir adla indirir. 

Maven’i komut satırında kullanarak örneği çalıştırın. Bir kabuk açın ve kopyalanan dizininiz içindeki **blobAzureApp** öğesine gidin. Sonra `mvn compile exec:java` komutunu girin. 

Aşağıdaki örnekte, uygulamayı Windows’da çalıştırmak istemeniz durumunda karşılaşacağınız çıkış gösterilir.

```
Azure Blob storage quick start sample
Creating container: quickstartcontainer
Creating a sample file at: C:\Users\<user>\AppData\Local\Temp\sampleFile514658495642546986.txt
Uploading the sample file 
URI of blob is: https://myexamplesacct.blob.core.windows.net/quickstartcontainer/sampleFile514658495642546986.txt
The program has completed successfully.
Press the 'Enter' key while in the console to delete the sample files, example container, and exit the application.

Deleting the container
Deleting the source, and downloaded files
```

Devam etmeden önce, varsayılan dizininizi (Windows kullanıcıları için *C:\Users\<kullanıcı>\AppData\Local\Temp*) örnek dosya için kontrol edin. Blob depolamadaki dosyanın içeriğini görüntülemek için, blobun URL'sini konsol penceresinden kopyalayın ve tarayıcıya yapıştırın. Dizininizdeki örnek dosyayı Blob depolamada saklanan içerikle karşılaştırırsanız, bunların aynı olduğunu görürsünüz. 

  >[!NOTE]
  >Ayrıca, Blob depolamadaki dosyaları görüntülemek için, [Azure Depolama Gezgini](https://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) gibi bir araç da kullanabilirsiniz. Azure Depolama Gezgini, depolama hesabı bilgilerinize erişmenize olanak tanıyan ücretsiz ve platformlar arası bir araçtır.

Dosyaları doğruladıktan sonra, tanıtımı tamamlamak ve test dosyalarını silmek için **Enter** tuşuna basın. Artık örnek dosyanın ne yaptığını gördüğünüze göre, koda göz atmak için **AzureApp.java** dosyasını açabilirsiniz. 

## <a name="understand-the-sample-code"></a>Örnek kodu anlama

Sonraki aşamada, nasıl çalıştığını anlayabilmeniz için örnek kodu inceleyeceğiz.

### <a name="get-references-to-the-storage-objects"></a>Depolama nesneleriyle ilgili başvuruları alma

İlk önce, Blob depolamaya erişmek ve Blob depolamayı yönetmek için kullanılan nesnelere başvuru oluşturmaktır. Bu nesneler birbirleri üzerinde derlenir; her dosya, listede yanında yer alan dosya tarafından kullanılır.

* [CloudStorageAccount](/java/api/com.microsoft.azure.management.storage.storageaccount) nesnesinin depolama hesabına işaret eden bir örneğini oluşturun.

    **CloudStorageAccount** nesnesi, depolama hesabınızın bir gösterimidir ve programlamayla depolama hesabı özelliklerini ayarlayabilmenizi ve bu özelliklere erişebilmenizi sağlar. **CloudStorageAccount** nesnesini kullanarak, blob hizmetine erişmek için kullanılan **CloudBlobClient**’in bir örneğini oluşturabilirsiniz.

* **CloudBlobClient** nesnesinin, depolama hesabınızdaki [Blob hizmetine](/java/api/com.microsoft.azure.storage.blob._cloud_blob_client) işaret eden bir örneğini oluşturun.

    **CloudBlobClient**, blob hizmetine erişim noktası sunarak programlamayla Blob depolama özelliklerini ayarlayabilmenizi ve bu özelliklere erişebilmenizi sağlar. **CloudBlobClient** nesnesini kullanarak, kapsayıcı oluşturmak için kullanılan **CloudBlobContainer**ıin bir örneğini oluşturabilirsiniz.

* [CloudBlobContainer](/java/api/com.microsoft.azure.storage.blob._cloud_blob_container) nesnesinin, eriştiğiniz kapsayıcıyı temsil eden bir örneğini oluşturun. Tıpkı bilgisayarınızdaki dosyaları düzenlemek için klasörleri kullandığınız gibi blobları düzenlemek için de kapsayıcıları kullanın.    

    **CloudBlobContainer** nesneniz olduktan sonra, ilgilendiğiniz belirli bir bloba işaret eden bir [CloudBlockBlob](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob) nesnesi örneği oluşturabilir ve karşıya yükleme, indirme, kopyalama işlemleri ve başka işlemler yapabilirsiniz.

> [!IMPORTANT]
> Kapsayıcı adlarının küçük harfle yazılması gerekir. Kapsayıcılar hakkında daha fazla bilgi için bkz. [Kapsayıcıları, Blobları ve Meta Verileri Adlandırma ve Bunlara Başvurma](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

### <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Bu bölümde, nesnelerin örneğini oluşturuyor, yeni kapsayıcı oluşturuyor ve ardından kapsayıcıdaki izinleri bloblar herkese açık olacak ve bloblara yalnızca bir URL ile erişilebilecek şekilde ayarlıyorsunuz. Bu kapsayıcının adı **quickstartcontainer**'dır. 

Örnek her çalıştırıldığında yeni bir kapsayıcı oluşturmak istediğimizden, bu örnekte [CreateIfNotExists](/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.createifnotexists) komutu kullanılır. Uygulamanın tamamında aynı kapsayıcıyı kullandığınız bir üretim ortamında, **CreateIfNotExists**’i yalnızca bir kez çağırmanız önerilir. Alternatif olarak, kapsayıcıyı önceden oluşturabilirsiniz. Böylece kapsayıcıyı kodda oluşturmanıza gerek kalmaz.

```java
// Parse the connection string and create a blob client to interact with Blob storage
storageAccount = CloudStorageAccount.parse(storageConnectionString);
blobClient = storageAccount.createCloudBlobClient();
container = blobClient.getContainerReference("quickstartcontainer");

// Create the container if it does not exist with public access.
System.out.println("Creating container: " + container.getName());
container.createIfNotExists(BlobContainerPublicAccessType.CONTAINER, new BlobRequestOptions(), new OperationContext());
```

### <a name="upload-blobs-to-the-container"></a>Blobları kapsayıcıya yükleme

Bir dosyayı bir blok blobuna yüklemek için hedef kapsayıcıda bloba bir başvuru alın. Blob başvurusunu aldıktan sonra, [CloudBlockBlob.Upload](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.upload) kullanarak verileri karşıya yükleyebilirsiniz. Bu işlemle, daha önce oluşturulmadıysa bir blob oluşturulur, blob zaten varsa blobun üzerine yazılır.

Örnek kod, karşıya yükleme ve indirme için kullanılacak yerel bir dosya oluşturur, karşıya yüklenecek dosyayı **kaynak** olarak ve blob adını **blob** olarak depolar. Aşağıdaki örnek, dosyayı **quickstartcontainer** adlı kapsayıcınıza yükler.

```java
//Creating a sample file
sourceFile = File.createTempFile("sampleFile", ".txt");
System.out.println("Creating a sample file at: " + sourceFile.toString());
Writer output = new BufferedWriter(new FileWriter(sourceFile));
output.write("Hello Azure!");
output.close();

//Getting a blob reference
CloudBlockBlob blob = container.getBlockBlobReference(sourceFile.getName());

//Creating blob and uploading file to it
System.out.println("Uploading the sample file ");
blob.uploadFromFile(sourceFile.getAbsolutePath());
```

Blob depolama için kullanabileceğiniz [upload](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.upload), [uploadBlock](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.uploadblock), [uploadFullBlob](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.uploadfullblob), [uploadStandardBlobTier](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.uploadstandardblobtier) ve [uploadText](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.uploadtext)’in de dahil olduğu birkaç `upload` yöntemi vardır. Örneğin, dizelerinizi `Upload` yöntemi yerine `UploadText` yöntemiyle karşıya yükleyebilirsiniz. 

Blok blobları herhangi bir metin veya ikili dosya türünde olabilir. Sayfa blobları öncelikli olarak IaaS VM'lerini destekleyen VHD dosyalarında kullanılır. Ekleme bloblarını, bir dosyaya yazıp daha sonradan daha fazla bilgi eklemek istediğiniz durumlarda günlüğe kaydetmek için kullanın. Blob depolamada depolanan nesnelerin çoğu blok blobudur.

### <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

[CloudBlobContainer.ListBlobs](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.listblobs) kullanarak kapsayıcıdaki dosyaların listesini alabilirsiniz. Aşağıdaki kod blob listesini alır, ardından bu bloblarda döngü yapar ve bulunan blobların URI değerlerini gösterir. Dosyayı görüntülemek için URI değerini komut penceresinden kopyalayıp tarayıcıya yapıştırabilirsiniz.

```java
//Listing contents of container
for (ListBlobItem blobItem : container.listBlobs()) {
    System.out.println("URI of blob is: " + blobItem.getUri());
}
```

### <a name="download-blobs"></a>Blob’ları indirme

[CloudBlob.DownloadToFile](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob.downloadtofile) kullanarak blobları yerel diskinize indirin.

Aşağıdaki kod önceki bir bölümde karşıya yüklenmiş olan blobu indirir; yerel diskinizde her iki dosyayı da görebilmeniz için indirilen blobun adına "_DOWNLOADED" son ekini koyar. 

```java
// Download blob. In most cases, you would have to retrieve the reference
// to cloudBlockBlob here. However, we created that reference earlier, and 
// haven't changed the blob we're interested in, so we can reuse it. 
// Here we are creating a new file to download to. Alternatively you can also pass in the path as a string into downloadToFile method: blob.downloadToFile("/path/to/new/file").
downloadedFile = new File(sourceFile.getParentFile(), "downloadedFile.txt");
blob.downloadToFile(downloadedFile.getAbsolutePath());
```

### <a name="clean-up-resources"></a>Kaynakları temizleme

Karşıya yüklediğiniz bloblara artık ihtiyacınız yoksa kullanarak kapsayıcının tamamını silebilirsiniz [Cloudblobcontainer.deleteıfexists](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.deleteifexists). Bu yöntem, kapsayıcıdaki dosyaları da siler.

```java
try {
if(container != null)
    container.deleteIfExists();
} catch (StorageException ex) {
System.out.println(String.format("Service error. Http code: %d and error code: %s", ex.getHttpStatusCode(), ex.getErrorCode()));
}

System.out.println("Deleting the source, and downloaded files");

if(downloadedFile != null)
downloadedFile.deleteOnExit();
        
if(sourceFile != null)
sourceFile.deleteOnExit();
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, dosyaları yerel bir disk ve Java kullanarak Azure Blob Depolama arasında aktarmayı öğrendiniz. Java ile çalışma hakkında daha fazla bilgi edinmek için GitHub kaynak kod depomuza devam edin.

> [!div class="nextstepaction"]
> [Microsoft Azure depolama SDK'sı için Java v10](https://github.com/azure/azure-storage-java) 
> [Java API Başvurusu](https://docs.microsoft.com/java/azure/)
> [kodu için Java örnekleri](../common/storage-samples-java.md)
