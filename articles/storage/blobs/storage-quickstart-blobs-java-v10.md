---
title: "Azure hızlı başlangıç: Java depolama SDK'sı V10 kullanarak nesne depolamada blob oluşturma | Microsoft Docs"
description: Bu hızlı başlangıçta, Java Depolama SDK'sını kullanarak nesne (Azure Blob) depolamasında kapsayıcı oluşturacak, dosya yükleyecek, nesneleri listeleyecek ve indireceksiniz.
services: storage
author: roygara
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 11/14/2018
ms.author: rogarana
ms.openlocfilehash: f44a6b825f9e8871bb7d7877ebd1821038b45f65
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60392390"
---
# <a name="quickstart-upload-download-and-list-blobs-by-using-the-java-storage-sdk-v10"></a>Hızlı Başlangıç: Karşıya yükleme, indirme ve Java için depolama SDK'sı V10 kullanarak blobları Listele

Bu hızlı başlangıçta, Azure Blob depolamadaki bir kapsayıcıda blok bloblarını karşıya yüklemek, indirmek ve listelemek için yeni Java Depolama SDK’sını nasıl kullanabileceğinizi öğreneceksiniz. Yeni Java SDK'sı RxJava ile duyarlı programlama modelini kullanarak zaman uyumsuz işlemler sağlar. [Java VM için RxJava duyarlı uzantıları](https://github.com/ReactiveX/RxJava) hakkında daha fazla bilgi edinin. 

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

Aşağıdaki ek önkoşulların yüklü olduğundan emin olun:

* [Maven](https://maven.apache.org/download.cgi) komut satırı veya tercih ettiğiniz herhangi bir Java tümleşik geliştirme ortamında çalışmaya.
* [JDK](https://aka.ms/azure-jdks)

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin:

Bu hızlı başlangıçta kullanılan [örnek uygulama](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart), temel bir konsol uygulamasıdır. 

Uygulamanın bir kopyasını geliştirme ortamınıza indirmek için [Git](https://git-scm.com/)'i kullanın.

```bash
git clone https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart.git
```

Bu komut, depoyu yerel Git klasörünüze kopyalar.

Projenizin içeri aktarma aşaması tamamlandıktan sonra **src/main/java/quickstart** konumunda yer alan **Quickstart.java** dosyasını açın.

[!INCLUDE [storage-copy-account-key-portal](../../../includes/storage-copy-account-key-portal.md)]

## <a name="configure-your-storage-connection-string"></a>Depolama bağlantı dizelerinizi yapılandırma
Bu çözüm için depolama hesabınızın adını ve anahtarını güvenli bir şekilde depolamanız gerekir. Bu bilgileri, örneği çalıştıran makinedeki yerel ortam değişkenlerinde depolayın. Ortam değişkenlerini oluşturmak için işletim sisteminize bağlı olarak Linux veya Windows örneğini izleyin.

### <a name="linux-example"></a>Linux örneği

```
export AZURE_STORAGE_ACCOUNT="<youraccountname>"
export AZURE_STORAGE_ACCESS_KEY="<youraccountkey>"
```

### <a name="windows-example"></a>Windows örneği

```
setx AZURE_STORAGE_ACCOUNT "<youraccountname>"
setx AZURE_STORAGE_ACCESS_KEY "<youraccountkey>"
```

## <a name="run-the-sample"></a>Örneği çalıştırma

Bu örnekte Windows kullanıcıları için **AppData\Local\Temp** varsayılan dizininde bir test dosyası oluşturulmaktadır. Ardından aşağıdaki adımları gerçekleştirmeniz istenir:

1. Test dosyasını Azure Blob depolamasına yüklemek için gerekli komutları girme.
2. Kapsayıcı içindeki blobları listeleme.
3. Eski ve yeni dosyaları karşılaştırmak için dosyayı indirme ve yeni bir adla yükleme. 

Örneği komut satırında Maven kullanarak çalıştırmak istiyorsanız bir kabuğu açın ve kopyalanan dizininizdeki **storage-blobs-java-v10-quickstart** bölümüne gidin. Sonra `mvn compile exec:java` komutunu girin.

Bu örnekte uygulamayı Windows'da çalıştırdığınızda elde edeceğiniz çıkış gösterilmiştir.

```
Created quickstart container
Enter a command
(P)utBlob | (L)istBlobs | (G)etBlob | (D)eleteBlobs | (E)xitSample
# Enter a command :
P
Uploading the sample file into the container: https://<storageaccount>.blob.core.windows.net/quickstart
# Enter a command :
L
Listing blobs in the container: https://<storageaccount>.blob.core.windows.net/quickstart
Blob name: SampleBlob.txt
# Enter a command :
G
Get the blob: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt
The blob was downloaded to C:\Users\<useraccount>\AppData\Local\Temp\downloadedFile13097087873115855761.txt
# Enter a command :
D
Delete the blob: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt

# Enter a command :
>> Blob deleted: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt
E
Cleaning up the sample and exiting!
```

Örneğin denetimi sizdedir; bu nedenle, kodu çalıştırmasını sağlamak için komutları girin. Girişler büyük/küçük harfe duyarlıdır.

Ayrıca, Blob depolamadaki dosyaları görüntülemek için, [Azure Depolama Gezgini](https://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) gibi bir araç da kullanabilirsiniz. Azure Depolama Gezgini, depolama hesabı bilgilerinize erişmenizi sağlayan ücretsiz ve platformlar arası bir araçtır. 

Dosyaları doğrulayın. Ardından **E**'yi seçip **Enter**'a basarak tanıtımı tamamlayın ve test dosyalarını silin. Artık örnek dosyanın ne yaptığını gördüğünüze göre, koda göz atmak için **Quickstart.java** dosyasını açabilirsiniz. 

## <a name="understand-the-sample-code"></a>Örnek kodu anlama

Aşağıdaki bölümlerde nasıl çalıştığını anlayabilmeniz için örnek kod incelenmektedir.

### <a name="get-references-to-the-storage-objects"></a>Depolama nesneleriyle ilgili başvuruları alma

İlk önce, Blob depolamaya erişmek ve Blob depolamayı yönetmek için kullanılan nesnelere başvuru oluşturun. Bu nesneler birbirlerinin üzerinde oluşturulur. Her bir listedeki bir sonraki nesne tarafından kullanılır.

1. **StorageURL** nesnesinin depolama hesabına işaret eden bir örneğini oluşturun.

    * [StorageURL](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._storage_u_r_l?view=azure-java-stable) nesnesi, depolama hesabınızı temsil eder. Yeni bir işlem hattı oluşturmak için bunu kullanın. 
    * İşlem hattı yetkilendirme, günlüğe kaydetme ve yeniden deneme mekanizmalarıyla istekleri ve yanıtları işlemek için kullanılan bir dizi ilkedir. Daha fazla bilgi için bkz. [HTTP İşlem Hattı](https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview#url-types--http-pipeline).  
    * İşlem hattını kullanarak [ServiceURL](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._service_u_r_l?view=azure-java-stable) nesnesinin bir örneğini oluşturun.
    * **ServiceURL** nesnesini kullanarak [ContainerURL](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._container_u_r_l?view=azure-java-stable) nesnesinin bir örneğini oluşturun.
    * **ContainerURL**, blob kapsayıcılarında işlem çalıştırmak için gereklidir.

2. **ContainerURL** nesnesinin, eriştiğiniz kapsayıcıyı temsil eden bir örneğini oluşturun. Kapsayıcılar, tıpkı bilgisayarınızdaki dosyaları düzenlemek için klasörleri kullandığınız gibi bloblarınızı düzenlemek için kullanılır.

    * **ContainerURL** kapsayıcı hizmetine bir erişim noktası sağlar. 
    * [ContainerURL](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._container_u_r_l?view=azure-java-stable) nesnesini kullanarak [BlobURL](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._blob_u_r_l?view=azure-java-stable) nesnesinin bir örneğini oluşturursunuz.
    * **BlobURL**, blobları oluşturmak için gereklidir.

3. **BlobURL** nesnesinin ilgilendiğiniz bloba işaret eden bir örneğini oluşturun. 

> [!IMPORTANT]
> Kapsayıcı adlarının küçük harfle yazılması gerekir. Kapsayıcılar ve blob adları hakkında daha fazla bilgi için bkz. [Kapsayıcıları, Blobları ve Meta Verileri Adlandırma ve Bunlara Başvurma](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

### <a name="create-a-container"></a>Bir kapsayıcı oluşturma 

Bu bölümde **ContainerURL** nesnesinin bir örneğini oluşturacaksınız. Bununla birlikte yeni bir kapsayıcı da oluşturacaksınız. Örnekteki kapsayıcının adı **quickstart**'tır. 

Bu örnekte kullanılan [ContainerURL.create](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._container_u_r_l.create?view=azure-java-stable#com_microsoft_azure_storage_blob__container_u_r_l_create_Metadata_PublicAccessType_Context_) sayesinde örnek her çalıştığında yeni bir kapsayıcı oluşturabilirsiniz. Alternatif olarak, kapsayıcıyı önceden oluşturabilirsiniz. Böylece kapsayıcıyı kodda oluşturmanıza gerek kalmaz.

```java
// Create a ServiceURL to call the Blob service. We will also use this to construct the ContainerURL
SharedKeyCredentials creds = new SharedKeyCredentials(accountName, accountKey);
// We are using a default pipeline here, you can learn more about it at https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview
final ServiceURL serviceURL = new ServiceURL(new URL("http://" + accountName + ".blob.core.windows.net"), StorageURL.createPipeline(creds, new PipelineOptions()));

// Let's create a container using a blocking call to Azure Storage
// If container exists, we'll catch and continue
containerURL = serviceURL.createContainerURL("quickstart");

try {
    ContainersCreateResponse response = containerURL.create(null, null).blockingGet();
    System.out.println("Container Create Response was " + response.statusCode());
} catch (RestException e){
    if (e instanceof RestException && ((RestException)e).response().statusCode() != 409) {
        throw e;
    } else {
        System.out.println("quickstart container already exists, resuming...");
    }
}
```

### <a name="upload-blobs-to-the-container"></a>Blobları kapsayıcıya yükleme

Blob depolama blok blobları, ekleme bloblarını ve sayfa bloblarını destekler. Blok blobları, en sık kullanılan bloblardır. Bunlar bu hızlı başlangıçta kullanılmaktadır. 

1. Dosyayı bloba yüklemek için, hedef kapsayıcıda bloba bir başvuru alın. 
2. Blob başvurusunu aldıktan sonra aşağıdaki API'lerden birini kullanarak dosya yükleyebilirsiniz:

   * Düşük düzey API'ler. **BlockBlobURL** örneğindeki PutBlob olarak da adlandırılan [BlockBlobURL.upload](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._block_blob_u_r_l.upload?view=azure-java-stable#com_microsoft_azure_storage_blob__block_blob_u_r_l_upload_Flowable_ByteBuffer__long_BlobHTTPHeaders_Metadata_BlobAccessConditions_Context_) ve PutBLock olarak da adlandırılan [BlockBlobURL.stageBlock](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._block_blob_u_r_l.stageblock?view=azure-java-stable) örnekleri verilebilir. 

   * [TransferManager sınıfında](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._transfer_manager?view=azure-java-stable) sağlanan yüksek düzey API'ler. [TransferManager.uploadFileToBlockBlob](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._transfer_manager.uploadfiletoblockblob?view=azure-java-stable) metodu örnek olarak verilebilir. 

     Bu işlemle, daha önce oluşturulmadıysa bir blob oluşturulur. Varsa blobun üzerine yazılır.

Örnek kod, karşıya yükleme ve indirme işlemleri için kullanılacak bir yerel dosya oluşturur. Yüklenecek dosyayı **sourceFile** olarak, blob URL'sini de **blob** içinde depolar. Aşağıdaki örnek, dosyayı **quickstart** adlı kapsayıcınıza yükler.

```java
static void uploadFile(BlockBlobURL blob, File sourceFile) throws IOException {

    FileChannel fileChannel = FileChannel.open(sourceFile.toPath());

    // Uploading a file to the blobURL using the high-level methods available in TransferManager class
    // Alternatively call the Upload/StageBlock low-level methods from BlockBlobURL type
    TransferManager.uploadFileToBlockBlob(fileChannel, blob, 8*1024*1024, null)
        .subscribe(response-> {
            System.out.println("Completed upload request.");
            System.out.println(response.response().statusCode());
        });
}
```

Blok blobları herhangi bir metin veya ikili dosya türünde olabilir. Sayfa blobları öncelikli olarak IaaS VM'leri desteklemek için kullanılan VHD dosyalarında kullanılır. Ekleme blobları verilerin sona eklenmesi için kullanılır ve çoğunlukla günlüğe kaydetme için tercih edilir. Blob depolamada depolanan nesnelerin çoğu blok blobudur.

### <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

[ContainerURL.listBlobsFlatSegment](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._container_u_r_l.listblobsflatsegment?view=azure-java-stable) kullanarak kapsayıcıdaki nesnelerin listesini alabilirsiniz. Bu yöntem bir kerede en çok 5000 nesne ve listede daha fazla nesne varsa bir devamlılık veya ileri işareti döndürür. Önceki **listBlobsFlatSegment** yanıtında ileri işareti bulunduğu sürece tekrar tekrar kendini çağıran bir yardımcı işlevi oluşturun.

```java
static void listBlobs(ContainerURL containerURL) {
    // Each ContainerURL.listBlobsFlatSegment call return up to maxResults (maxResults=10 passed into ListBlobOptions below).
    // To list all Blobs, we are creating a helper static method called listAllBlobs,
    // and calling it after the initial listBlobsFlatSegment call
    ListBlobsOptions options = new ListBlobsOptions(null, null, 10);

    containerURL.listBlobsFlatSegment(null, options)
        .flatMap(containersListBlobFlatSegmentResponse ->
            listAllBlobs(containerURL, containersListBlobFlatSegmentResponse))
                .subscribe(response-> {
                    System.out.println("Completed list blobs request.");
                    System.out.println(response.statusCode());
                });
}

private static Single <ContainersListBlobFlatSegmentResponse> listAllBlobs(ContainerURL url, ContainersListBlobFlatSegmentResponse response) {
    // Process the blobs returned in this result segment (if the segment is empty, blobs() will be null.
    if (response.body().blobs() != null) {
        for (Blob b : response.body().blobs().blob()) {
            String output = "Blob name: " + b.name();
            if (b.snapshot() != null) {
                output += ", Snapshot: " + b.snapshot();
            }
            System.out.println(output);
        }
    }
    else {
        System.out.println("There are no more blobs to list off.");
    }

    // If there is not another segment, return this response as the final response.
    if (response.body().nextMarker() == null) {
        return Single.just(response);
    } else {
        /*
        IMPORTANT: ListBlobsFlatSegment returns the start of the next segment; you MUST use this to get the next
        segment (after processing the current result segment
        */

        String nextMarker = response.body().nextMarker();

        /*
        The presence of the marker indicates that there are more blobs to list, so we make another call to
        listBlobsFlatSegment and pass the result through this helper function.
        */

        return url.listBlobsFlatSegment(nextMarker, new ListBlobsOptions(null, null,1))
            .flatMap(containersListBlobFlatSegmentResponse ->
                listAllBlobs(url, containersListBlobFlatSegmentResponse));
    }
}
```

### <a name="download-blobs"></a>Blob’ları indirme

[BlobURL.download](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._blob_u_r_l.download?view=azure-java-stable) nesnesini kullanarak blobları yerel diskinize indirin.

Aşağıdaki kod, önceki bir bölüme yüklenen blobu indirir. Yerel diskte yer alan iki dosyayı da görebilmeniz için, blob adının sonuna **_DOWNLOADED** son eki getirilir. 

```java
static void getBlob(BlockBlobURL blobURL, File sourceFile) {
    try {
        // Get the blob using the low-level download method in BlockBlobURL type
        // com.microsoft.rest.v2.util.FlowableUtil is a static class that contains helpers to work with Flowable
        blobURL.download(new BlobRange(0, Long.MAX_VALUE), null, false)
            .flatMapCompletable(response -> {
                AsynchronousFileChannel channel = AsynchronousFileChannel.open(Paths
                    .get(sourceFile.getPath()), StandardOpenOption.CREATE,  StandardOpenOption.WRITE);
                        return FlowableUtil.writeFile(response.body(), channel);
            }).doOnComplete(()-> System.out.println("The blob was downloaded to " + sourceFile.getAbsolutePath()))
            // To call it synchronously add .blockingAwait()
            .subscribe();
    } catch (Exception ex){
        System.out.println(ex.toString());
    }
}
```

### <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıçta karşıya yüklenen bloblara ihtiyacınız kalmadığında, [ContainerURL.delete](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._container_u_r_l.delete?view=azure-java-stable) kullanarak kapsayıcının tamamını silebilirsiniz. Bu yöntem, kapsayıcıdaki dosyaları da siler.

```java
containerURL.delete(null).blockingGet();
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, dosyaları Java kullanarak yerel bir disk ve Azure Blob depolama arasında aktarmayı öğrendiniz. 

> [!div class="nextstepaction"]
> [Java kaynak kodu için Depolama SDK V10](https://github.com/Azure/azure-storage-java/)
> [API Başvurusu](https://docs.microsoft.com/java/api/overview/azure/storage/client?view=azure-java-stable)
> [RxJava hakkında daha fazla bilgi edinin](https://github.com/ReactiveX/RxJava)
