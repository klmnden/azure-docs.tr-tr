---
title: Azure Hızlı Başlangıç - Java kullanarak nesne depolamada blob oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta, nesne (Blob) depolamada depolama hesabı ve kapsayıcı oluşturursunuz. Sonra, Azure Depolama’ya blob yüklemek, blob indirmek ve bir kapsayıcıdaki blobları listelemek amacıyla Java için depolama istemcisi kitaplığını kullanırsınız.
services: storage
author: roygara
manager: jeconnoc
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 04/09/2018
ms.author: rogarana
ms.openlocfilehash: 197777971b92ad9cd53e91602b88858a371ce1d8
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-upload-download-and-list-blobs-using-java"></a>Hızlı Başlangıç: Java kullanarak blobları yükleme, indirme ve listeleme

Bu hızlı başlangıçta, Azure Blob depolamadaki bir kapsayıcıda blok bloblarını karşıya yüklemek, indirmek ve listelemek için Java’yı nasıl kullanabileceğinizi öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için:

* Maven tümleştirmesine sahip bir IDE yükleyin

* Alernatif olarak, Maven’ı yükleyip komut satırından çalışacak şekilde de yapılandırabilirsiniz

Bu öğreticide, "Java Geliştiriciler için Eclipse IDE" ile [Eclipse](http://www.eclipse.org/downloads/) kullanılmaktadır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin:

Bu hızlı başlangıçta kullanılan [örnek uygulama](https://github.com/Azure-Samples/storage-blobs-java-quickstart), temel bir konsol uygulamasıdır. 

Uygulamanın bir kopyasını geliştirme ortamınıza indirmek için [Git](https://git-scm.com/)'i kullanın. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-java-quickstart.git
```

Bu komut, depoyu yerel Git klasörünüze kopyalar. Projeyi açmak için, Eclipse’ı başlatın ve karşılama ekranını kapatın. **Dosya**’yı ve ardından **Dosya Sisteminden Proje Aç...**’ı seçin. **Proje niteliklerini algıla ve yapılandır** seçeneğinin belirlenmiş olduğundan emin olun. **Dizin**’i seçin ve ardından kopyalanan dizini depoladığınız konuma gidin. Bu konumda, **JavaBlobsQuickstart** klasörünü seçin. **javaBlobsQuickstarts** projesinin Eclipse projesi olarak göründüğünden emin olun ve sonra **Son**’u seçin.

Projenin içeri aktarılması tamamlandığında, **AzureApp.java**’yı açın (**src/main/java** içinde, **blobQuickstart.blobAzureApp** konumundadır) ve `storageConnectionString` dizesindeki `accountname` ve `accountkey` öğelerini değiştirin. Sonra, uygulamayı çalıştırın.

[!INCLUDE [storage-copy-connection-string-portal](../../../includes/storage-copy-connection-string-portal.md)]   

## <a name="configure-your-storage-connection-string"></a>Depolama bağlantı dizelerinizi yapılandırma
    
Uygulamada, depolama hesabınız için bağlantı dizesi sağlamanız gerekir. **AzureApp.Java** dosyasını açın. `storageConnectionString` değişkenini bulun ve önceki bölümde kopyaladığınız bağlantı dizesi değerini yapıştırın. `storageConnectionString` değişkeni, aşağıdakine benzer olmalıdır:

```java
public static final String storageConnectionString =
"DefaultEndpointsProtocol=https;" +
"AccountName=<account-name>;" +
"AccountKey=<account-key>";
```

## <a name="run-the-sample"></a>Örneği çalıştırma

Bu örnek, varsayılan dizininizde (Windows kullanıcıları için Belgelerim) bir test dosyası oluşturur, bunu Blob depolamaya yükler, blobları kapsayıcıda listeler ve ardından eski ve yeni dosyaları karşılaştırabilmeniz için dosyayı yeni bir adla indirir. 

Eclipse’te **Ctrl+F11** tuşlarına basarak örneği çalıştırın.

Örneği komut satırındaki Maven ile çalıştırmak istiyorsanız bir kabuğu açın ve kopyalanan dizininizdeki **blobAzureApp**’e gidin. Sonra `mvn compile exec:java` komutunu girin.

Aşağıda, uygulamayı Windows’da çalıştırmak istemeniz durumunda karşılaşacağınız çıktının bir örneği verilmiştir.

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

 Devam etmeden önce, varsayılan dizininizi (Windows kullanıcıları için Belgelerim) iki dosya için kontrol edin. Dosyaları açarak aynı olduklarını görebilirsiniz. Blob depolamadaki dosyanın içeriğini görüntülemek için, blobun URL'sini konsol penceresinden kopyalayın ve tarayıcıya yapıştırın. Devam etmek için Enter tuşuna bastığınızda, depolama kapsayıcısını ve dosyaları siler.

Ayrıca, Blob depolamadaki dosyaları görüntülemek için, [Azure Depolama Gezgini](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) gibi bir araç da kullanabilirsiniz. Azure Depolama Gezgini, depolama hesabı bilgilerinize erişmenize olanak tanıyan ücretsiz ve platformlar arası bir araçtır. 

Dosyaları doğruladıktan sonra, tanıtımı tamamlamak ve test dosyalarını silmek için Enter tuşuna basın. Artık örnek dosyanın ne yaptığını gördüğünüze göre, koda göz atmak için **AzureApp.java** dosyasını açabilirsiniz. 

## <a name="understand-the-sample-code"></a>Örnek kodu anlama

Sonraki aşamada, nasıl çalıştığını anlayabilmeniz için örnek kodu inceleyeceğiz.

### <a name="get-references-to-the-storage-objects"></a>Depolama nesneleriyle ilgili başvuruları alma

İlk önce, Blob depolamaya erişmek ve Blob depolamayı yönetmek için kullanılan nesnelere başvuru oluşturmaktır. Bu nesneler birbirleri üzerinde derlenir; her dosya, listede yanında yer alan dosya tarafından kullanılır.

* [CloudStorageAccount](/java/api/com.microsoft.azure.management.storage._storage_account) nesnesinin depolama hesabına işaret eden bir örneğini oluşturun.

    **CloudStorageAccount** nesnesi, depolama hesabınızın bir gösterimidir ve programlamayla depolama hesabı özelliklerini ayarlayabilmenizi ve bu özelliklere erişebilmenizi sağlar. **CloudStorageAccount** nesnesini kullanarak, blob hizmetine erişmek için kullanılan **CloudBlobClient**’in bir örneğini oluşturabilirsiniz.

* **CloudBlobClient** nesnesinin, depolama hesabınızdaki [Blob hizmetine](/java/api/com.microsoft.azure.storage.blob._cloud_blob_client) işaret eden bir örneğini oluşturun.

    **CloudBlobClient**, blob hizmetine erişim noktası sunarak programlamayla depolama hesabı özelliklerini ayarlayabilmenizi ve bu özelliklere erişebilmenizi sağlar. **CloudBlobClient** nesnesini kullanarak, kapsayıcı oluşturmak için kullanılan **CloudBlobContainer**ıin bir örneğini oluşturabilirsiniz.

* [CloudBlobContainer](/java/api/com.microsoft.azure.storage.blob._cloud_blob_container) nesnesinin, eriştiğiniz kapsayıcıyı temsil eden bir örneğini oluşturun. Kapsayıcılar, tıpkı bilgisayarınızdaki dosyaları düzenlemek için klasörleri kullandığınız gibi blobları düzenlemek için kullanılır.    

    **CloudBlobContainer** nesneniz olduktan sonra, ilgilendiğiniz belirli bir bloba işaret eden bir [CloudBlockBlob](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob) nesnesi örneği oluşturabilir ve karşıya yükleme, indirme, kopyalama gibi işlemler yapabilirsiniz.

> [!IMPORTANT]
> Kapsayıcı adlarının küçük harfle yazılması gerekir. Kapsayıcılar ve blob adları hakkında daha fazla bilgi için bkz. [Kapsayıcıları, Blobları ve Meta Verileri Adlandırma ve Bunlara Başvurma](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

### <a name="create-a-container"></a>Bir kapsayıcı oluşturma 

Bu bölümde, nesnelerin örneğini oluşturuyor, yeni kapsayıcı oluşturuyor ve ardından kapsayıcıdaki izinleri bloblar herkese açık olacak ve bloblara yalnızca bir URL ile erişilebilecek şekilde ayarlıyorsunuz. Bu kapsayıcının adı **quickstartblobs**'dur. 

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

Blob depolama blok blobları, ekleme bloblarını ve sayfa bloblarını destekler. Blok blobları en sık kullanılan bloblardır ve bu hızlı başlangıçta bu bloblar kullanılmıştır. 

Dosyayı bloba yüklemek için, hedef kapsayıcıda bloba bir başvuru alın. Blob başvurusunu aldıktan sonra, [CloudBlockBlob.Upload](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.upload#com_microsoft_azure_storage_blob__cloud_block_blob_upload_final_InputStream_final_long) kullanarak verileri karşıya yükleyebilirsiniz. Bu işlemle, daha önce oluşturulmadıysa bir blob oluşturulur, blob zaten varsa blobun üzerine yazılır.

Örnek kod, karşıya yükleme ve indirme işlemleri için kullanılacak bir yerel dosya oluşturur. karşıya yüklenecek dosyayı **kaynak**, blobun adını **blob** olarak depolama. Aşağıdaki örnek, dosyayı **quickstartblobs** adlı kapsayıcınıza yükler.

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

Blob depolama için kullanabileceğiniz [upload](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.upload), [uploadBlock](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.uploadblock), [uploadFullBlob](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.uploadfullblob), [uploadStandardBlobTier](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.uploadstandardblobtier) ve [uploadText](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.uploadtext)’in dahil olduğu birkaç karşıya yükleme yöntemi vardır. Örneğin, dizelerinizi Upload metodu yerine UploadText metoduyla karşıya yükleyebilirsiniz. 

Blok blobları herhangi bir metin veya ikili dosya türünde olabilir. Sayfa blobları öncelikli olarak IaaS VM'leri desteklemek için kullanılan VHD dosyalarında kullanılır. Ekleme blobları, bir dosyaya yazıp daha sonradan daha fazla bilgi eklemek istediğiniz durumlarda günlüğe kaydetme için kullanılır. Blob depolamada depolanan nesnelerin çoğu blok blobudur.

### <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

[CloudBlobContainer.ListBlobs](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.listblobs#com_microsoft_azure_storage_blob__cloud_blob_container_listBlobs) kullanarak kapsayıcıdaki dosyaların listesini alabilirsiniz. Aşağıdaki kod blob listesini alır, ardından bu bloblarda döngü yapar ve bulunan blobların URI değerlerini gösterir. Dosyayı görüntülemek için URI değerini komut penceresinden kopyalayıp tarayıcıya yapıştırabilirsiniz.

```java
//Listing contents of container
for (ListBlobItem blobItem : container.listBlobs()) {
    System.out.println("URI of blob is: " + blobItem.getUri());
}
```

### <a name="download-blobs"></a>Blob’ları indirme

[CloudBlob.DownloadToFile](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob.downloadtofile#com_microsoft_azure_storage_blob__cloud_blob_downloadToFile_final_String) kullanarak blobları yerel diskinize indirin.

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

Bu hızlı başlangıçta karşıya yüklenen bloblara artık ihtiyacınız kalmadığında, [CloudBlobContainer.DeleteIfExists](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.deleteifexists#com_microsoft_azure_storage_blob__cloud_blob_container_deleteIfExists) kullanarak kapsayıcının tamamını silebilirsiniz. Bu işlem, kapsayıcıdaki dosyaları da siler.

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

## <a name="resources-for-developing-java-applications-with-blobs"></a>Bloblarla Java uygulamaları geliştirme kaynakları

Blob depolama ile Java geliştirmeye yönelik şu ek kaynaklara bakın:

### <a name="binaries-and-source-code"></a>İkili dosyalar ve kaynak kodu

- GitHub’da Azure Depolama için [Java istemci kitaplığı kaynak kodunu](https://github.com/Azure/azure-storage-java) görüntüleyin ve indirin.

### <a name="client-library-reference-and-samples"></a>İstemci kitaplığı başvurusu ve örnekleri

- Java istemci kitaplığı hakkında daha fazla bilgi için bkz. [Java API başvurusu](https://docs.microsoft.com/java/api/overview/azure/storage).
- Java istemci kitaplığı kullanılarak yazılmış [Blob depolama örneklerini](https://azure.microsoft.com/resources/samples/?sort=0&service=storage&platform=java&term=blob) araştırın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, dosyaları Java kullanarak yerel bir disk ve Azure Blob depolama arasında aktarmayı öğrendiniz. Blob depolamayla çalışma hakkında daha fazla bilgi edinmek için, Blob depolama nasıl yapılır öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Blob Depolama İşlemleri Nasıl Yapılır](storage-java-how-to-use-blob-storage.md)

Depolama Gezgini ve Bloblar hakkında daha fazla bilgi için bkz. [Azure Blob depolama kaynaklarını Depolama Gezgini'yle yönetme](../../vs-azure-tools-storage-explorer-blobs.md).

Daha fazla Java örnekleri için bkz. [Java kullanan Azure Depolama Örnekleri](../common/storage-samples-java.md).
