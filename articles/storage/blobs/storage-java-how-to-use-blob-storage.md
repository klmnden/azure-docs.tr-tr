---
title: Java'dan Azure Blob storage (nesne depolama) kullanma | Microsoft Docs
description: "Azure Blob Storage (nesne depolama) ile bulutta yapılandırılmamış veri depolayın."
services: storage
documentationcenter: java
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 2e223b38-92de-4c2f-9254-346374545d32
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 12/08/2016
ms.author: tamram
ms.openlocfilehash: 91ef09916dbb587305572ea640fb4408ea9aebb6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-blob-storage-from-java"></a>Java’dan Blob Storage kullanma
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../../includes/storage-check-out-samples-java.md)]

## <a name="overview"></a>Genel Bakış
Azure Blob Storage, bulutta nesne/blob olarak yapılandırılmamış veri depolayan bir hizmettir. Blob Storage belge, medya dosyası veya uygulama yükleyici gibi her tür metin veya ikili veri depolayabilir. Blob Storage aynı zamanda nesne depolama olarak adlandırılır.

Bu makalede Microsoft Azure Blob storage kullanarak yaygın senaryolar gerçekleştirmek nasıl yapacağınızı gösterir. Java ve kullanım örnekleri yazılır [Java için Azure depolama SDK'sı][Azure Storage SDK for Java]. Kapsamdaki senaryolar dahil **karşıya**, **listeleme**, **indirme**, ve **silme** BLOB'lar. BLOB'ları hakkında daha fazla bilgi için bkz: [sonraki adımlar](#Next-Steps) bölümü.

> [!NOTE]
> Bir SDK'sı, Android cihazlarda Azure depolama kullanan geliştiriciler için kullanılabilir. Daha fazla bilgi için bkz: [Android için Azure depolama SDK'sı][Azure Storage SDK for Android].
>
>

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Java uygulaması oluşturma
Bu makalede, bir Java uygulaması içinde yerel olarak veya bir web rolü veya Azure çalışan rolünde çalışan kodu çalıştırılabilir depolama özelliklerini kullanır.

Bunu yapmak için Java Geliştirme Seti (JDK) yükleyin ve Azure aboneliğinizde bir Azure depolama hesabı oluşturmanız gerekir. Bunu yaptıktan sonra geliştirme sisteminizde içinde listelenen bağımlılıkları ve en düşük gereksinimleri karşıladığını doğrulamanız gerekir [Java için Azure depolama SDK'sı] [ Azure Storage SDK for Java] github'daki. Sisteminiz bu gereksinimleri karşılıyorsa, indirme ve bu depodan sisteminizdeki Java için Azure depolama kitaplıkları yükleme yönergelerini izleyebilirsiniz. Bu görevleri tamamladığınızda, bu makaledeki örnekler kullanan bir Java uygulaması oluşturmak mümkün olacaktır.

## <a name="configure-your-application-to-access-blob-storage"></a>BLOB depolama alanına erişmek için uygulamanızı yapılandırın
Aşağıdaki içeri aktarma deyimlerini Azure depolama API'leri BLOB'lar erişmek için kullanmasını istediğiniz Java dosyasının üstüne ekleyin.

```java
// Include the following imports to use blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a>Bir Azure depolama bağlantı dizesi ayarlama
Bir Azure Storage istemci uç noktaları ve Veri Yönetimi Hizmetleri erişmek için kimlik bilgilerini depolamak için bir depolama bağlantı dizesi kullanır. Bir istemci uygulamasında çalıştırırken, depolama hesabınızın adını kullanarak depolama bağlantı dizesi şu biçimde sağlamanız gerekir ve depolama hesabı için birincil erişim anahtarını listelenen [Azure portal](https://portal.azure.com) için *AccountName* ve *AccountKey* değerleri. Aşağıdaki örnekte, bağlantı dizesi tutmak için statik bir alana nasıl bildirebilir gösterir.

```java
// Define the connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

Çalışan bir uygulama içinde Microsoft Azure rolünde içinde bu dize hizmet yapılandırma dosyasında depolanabilir *ServiceConfiguration.cscfg*ve çağrısıyla erişilebilir **RoleEnvironment.getConfigurationSettings** yöntemi. Aşağıdaki örnek bağlantı dizesinden alır bir **ayarı** adlı öğe *StorageConnectionString* hizmet yapılandırma dosyası.

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

Aşağıdaki örnekler, bu iki yöntemden birini depolama bağlantı dizesini almak için kullanılan olduğunu varsayalım.

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma
A **CloudBlobClient** nesne kapsayıcılar ve bloblar için başvuru nesneleri alma olanak sağlar. Aşağıdaki kod oluşturur bir **CloudBlobClient** nesnesi.

> [!NOTE]
> Oluşturmak için ek yol vardır **CloudStorageAccount** nesneleri; daha fazla bilgi için bkz: **CloudStorageAccount** içinde [Azure Storage istemci SDK'sı başvurusu].
>
>

[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

Kullanmak **CloudBlobClient** kullanmak istediğiniz kapsayıcı için bir başvuru nesnesi. İle yoksa kapsayıcı oluşturabilirsiniz **createIfNotExists** Aksi halde var olan bir kapsayıcı döndürür yöntemi. (Daha önce yaptığınız gibi) Bu kapsayıcıdan BLOB indirmek için depolama erişim anahtarınızı belirtmeniz gerekir böylece varsayılan olarak, yeni özel kapsayıcıdır.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Get a reference to a container.
    // The container name must be lower case
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Create the container if it does not exist.
    container.createIfNotExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

### <a name="optional-configure-a-container-for-public-access"></a>İsteğe bağlı: genel erişim için bir kapsayıcı yapılandırın
Bir kapsayıcının izinleri varsayılan olarak özel erişim için yapılandırılmış, ancak Internet üzerindeki tüm kullanıcılar için genel, salt okunur erişime izin vermek için bir kapsayıcının izinleri kolayca yapılandırabilirsiniz:

```java
// Create a permissions object.
BlobContainerPermissions containerPermissions = new BlobContainerPermissions();

// Include public access in the permissions object.
containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);

// Set the permissions on the container.
container.uploadPermissions(containerPermissions);
```

## <a name="upload-a-blob-into-a-container"></a>Bir kapsayıcıya bir blob yükleme
Bir blobu bir dosyayı karşıya yüklemek için bir kapsayıcı başvurusu alın ve bir blob başvurusu almak için kullanın. Bir blob başvurusu edindiğinizde blob başvurusu karşıya yükleme çağırarak tüm akış karşıya yükleyebilirsiniz. Mevcut veya varsa üzerine değildir, bu işlem blob oluşturun. Aşağıdaki kod örneği, bu gösterir ve kapsayıcının önceden oluşturulduğunu varsayar.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Define the path to a local file.
    final String filePath = "C:\\myimages\\myimage.jpg";

    // Create or overwrite the "myimage.jpg" blob with contents from a local file.
    CloudBlockBlob blob = container.getBlockBlobReference("myimage.jpg");
    File source = new File(filePath);
    blob.upload(new FileInputStream(source), source.length());
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme
BLOB'ları bir kapsayıcıda listelemek için önce bir blob karşıya yüklemek için yaptığınız gibi bir kapsayıcı başvurusu edinin. Kapsayıcının kullanabilirsiniz **listBlobs** yöntemi ile bir **için** döngü. Aşağıdaki kod konsoluna bir kapsayıcıdaki her bir blob URI'si çıkarır.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Loop over blobs within the container and output the URI to each of them.
    for (ListBlobItem blobItem : container.listBlobs()) {
        System.out.println(blobItem.getUri());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

Blobları adlarında yol bilgileriyle adlandırabilirsiniz unutmayın. Bu, geleneksel bir dosya sisteminde olduğu gibi düzenleme ve geçiş yapabileceğiniz sanal bir dizin yapısı oluşturur. Dizin yapısının yalnızca sanal olduğunu unutmayın; Blob Storage’da  kullanılabilecek kaynaklar kapsayıcılar ve bloblardır. Ancak, istemci kitaplığının sunan bir **CloudBlobDirectory** sanal dizine ait ve bu şekilde düzenlenen BLOB'ları ile çalışma sürecini basitleştirmek için nesne.

Örneğin, "" rootphoto1"," 2010/photo1"," 2010/photo2"adlı BLOB'ları ve" 2011/photo1"karşıya neden fotoğrafları" adlı bir kapsayıcı olabilir. Bu "2010" ve "2011.", "fotoğrafları" kapsayıcı içindeki sanal dizinlere oluşturursunuz. Çağırdığınızda **listBlobs** "fotoğrafları" kapsayıcısında döndürülen koleksiyon içerir **CloudBlobDirectory** ve **CloudBlob** dizinleri ve blobları en üst düzeyinde bulunan temsil eden nesne. Bu durumda, fotoğraf "rootphoto1" yanı sıra, dizinler "2010" ve "2011" döndürülür. Kullanabileceğiniz **instanceof** bu nesnelerin ayırt etmek için işleci.

İsteğe bağlı olarak, parametre olarak geçirebilir **listBlobs** yöntemiyle **Listblobs** parametresi true olarak ayarlandığında. Bu bağımsız olarak dizin döndürülen her blob neden olur. Daha fazla bilgi için bkz: **CloudBlobContainer.listBlobs** içinde [Azure Storage istemci SDK'sı başvurusu].

## <a name="download-a-blob"></a>Blob indirme
BLOB'ları indirmek için bir blob başvurusu almak için bir blob yüklemek için yaptığınız gibi aynı adımları izleyin. Karşıya yükleme örnekte karşıya yükleme blob nesnede çağrılır. Aşağıdaki örnekte, blob içeriklerini bir akış nesnesine gibi aktarmak için indirme çağrısı bir **FileOutputStream** blob yerel bir dosyaya kalıcı hale getirmek için kullanabilirsiniz.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Loop through each blob item in the container.
    for (ListBlobItem blobItem : container.listBlobs()) {
        // If the item is a blob, not a virtual directory.
        if (blobItem instanceof CloudBlob) {
            // Download the item and save it to a file with the same name.
            CloudBlob blob = (CloudBlob) blobItem;
            blob.download(new FileOutputStream("C:\\mydownloads\\" + blob.getName()));
        }
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="delete-a-blob"></a>Blob silme
Blob, bir blob başvurusu alın ve çağrı silmek için **deleteIfExists**.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Retrieve reference to a blob named "myimage.jpg".
    CloudBlockBlob blob = container.getBlockBlobReference("myimage.jpg");

    // Delete the blob.
    blob.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="delete-a-blob-container"></a>Bir blob kapsayıcısından silin
Son olarak, bir blob kapsayıcısını silmek için bir blob alma kapsayıcı başvurusu ve çağrı **deleteIfExists**.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.createCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.getContainerReference("mycontainer");

    // Delete the blob container.
    container.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="next-steps"></a>Sonraki adımlar
Blob storage'nın öğrendiğinize göre daha karmaşık depolama görevleri hakkında bilgi edinmek için aşağıdaki bağlantıları izleyin.

* [Azure depolama için Java SDK'sı][Azure Storage SDK for Java]
* [Azure Storage istemci SDK'sı başvurusu][Azure Storage istemci SDK'sı başvurusu]
* [Azure Storage REST API][Azure Storage REST API]
* [Azure depolama ekibi blogu][Azure Storage Team Blog]

Daha fazla bilgi için Ayrıca bkz. [Java geliştiriciler için Azure](/java/azure).

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[Azure Storage istemci SDK'sı başvurusu]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
