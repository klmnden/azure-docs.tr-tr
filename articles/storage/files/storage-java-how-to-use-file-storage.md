---
title: Java ile Azure dosyaları için geliştirme | Microsoft Docs
description: Java uygulamalarını ve dosya verilerini depolamak için Azure dosyaları'nı kullanma hizmetlerini geliştirmeyi öğrenin.
services: storage
author: roygara
ms.service: storage
ms.devlang: Java
ms.topic: article
ms.date: 09/19/2017
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 238e5971e79b192e0ef422dcd452859ff7566580
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64721673"
---
# <a name="develop-for-azure-files-with-java"></a>Java ile Azure dosyaları için geliştirme
[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../../includes/storage-check-out-samples-java.md)]

## <a name="about-this-tutorial"></a>Bu öğretici hakkında
Bu öğretici, dosya verilerini depolamak için Azure dosyaları'nı kullanma uygulamaları veya hizmetleri geliştirmek için Java kullanmanın temellerini gösterir. Bu öğreticide, biz bir konsol uygulaması oluşturun ve nasıl Java ve Azure dosyaları ile temel Eylemler gerçekleştirileceğini göstereceğiz:

* Oluşturma ve Azure dosya paylaşımını silme
* Oluşturma ve dizinleri silme
* Dosyalar ve dizinler bir Azure dosya paylaşımı içindeki numaralandırın
* Yükleme, indirme ve dosya silme

> [!Note]  
> Azure dosyaları SMB üzerinden erişilebildiğinden, standart Java g/ç sınıflarını kullanarak Azure dosya paylaşımına erişen uygulamalar yazmak mümkündür. Bu makalede, Azure depolama Java kullanan SDK'yı kullanan uygulamaların nasıl yazılacağı anlatmaktadır [Azure dosya REST API'sini](https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api) Azure dosyaları'na bahsedeceğiz.

## <a name="create-a-java-application"></a>Java uygulaması oluşturma
Örnekleri oluşturmak için Java Development Kit (JDK) gerekir ve [Java için Azure depolama SDK'sı](https://github.com/Azure/azure-storage-java). Ayrıca bir Azure depolama hesap oluşturmuş olmanız.

## <a name="set-up-your-application-to-use-azure-files"></a>Azure dosyaları'nı kullanmak için uygulamanızı ayarlama
Azure depolama API'leri kullanmak için burada Depolama hizmetinden erişmek için istediğinize Java dosyasının en üstüne aşağıdaki ifadeyi ekleyin.

```java
// Include the following imports to use blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.file.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a>Bir Azure depolama bağlantı dizesi ayarlama
Azure dosyaları'nı kullanmak için Azure depolama hesabınıza bağlanmanız gerekir. Depolama hesabınıza bağlanmak için kullanacağınız bir bağlantı dizesini yapılandırmak için ilk adım olacaktır. Şimdi bunu yapmak için statik bir değişken tanımlayın.

```java
// Configure the connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account_name;" +
    "AccountKey=your_storage_account_key";
```

> [!NOTE]
> Your_storage_account_name ve your_storage_account_key depolama hesabınız için gerçek değerlerle değiştirin.
> 
> 

## <a name="connecting-to-an-azure-storage-account"></a>Bir Azure depolama hesabına bağlanma
Depolama hesabınıza bağlanmak için kullanmanız gerekir **CloudStorageAccount** nesnesinin bir bağlantı dizesi, **ayrıştırma** yöntemi.

```java
// Use the CloudStorageAccount object to connect to your storage account
try {
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
} catch (InvalidKeyException invalidKey) {
    // Handle the exception
}
```

**CloudStorageAccount.parse** bir try/catch bloğu içinde koymak ihtiyacınız olacak şekilde bir InvalidKeyException oluşturur.

## <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma
Tüm dosyalar ve dizinler Azure dosyaları'nda adlı bir kapsayıcı içinde bulunan bir **paylaşımı**. Depolama hesabınızın hesap kapasitenizi verdiğinden çok paylaşımları olabilir. Bir paylaşımı ve içeriği erişimi elde etmek için bir Azure dosyaları istemci kullanmanız gerekir.

```java
// Create the Azure Files client.
CloudFileClient fileClient = storageAccount.createCloudFileClient();
```

Azure dosyaları İstemcisi'ni kullanarak bir paylaşım başvuru edinebilirsiniz.

```java
// Get a reference to the file share
CloudFileShare share = fileClient.getShareReference("sampleshare");
```

Aslında paylaşımı oluşturmak için kullanmak **Createıfnotexists** CloudFileShare nesnesinin yöntemi.

```java
if (share.createIfNotExists()) {
    System.out.println("New share created");
}
```

Bu noktada, **paylaşmak** adlı bir paylaşım için bir başvuru tutan **sampleshare**.

## <a name="delete-an-azure-file-share"></a>Azure dosya paylaşımını Sil
Bir paylaşımı siliniyor yapılır çağırarak **deleteIfExists** CloudFileShare nesnesi üzerinde yöntemi. Yapan örnek kod aşağıda verilmiştir.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the file client.
   CloudFileClient fileClient = storageAccount.createCloudFileClient();

   // Get a reference to the file share
   CloudFileShare share = fileClient.getShareReference("sampleshare");

   if (share.deleteIfExists()) {
       System.out.println("sampleshare deleted");
   }
} catch (Exception e) {
    e.printStackTrace();
}
```

## <a name="create-a-directory"></a>Dizin oluşturma
Ayrıca, depolama, yerine tümünün kök dizininde alt dizinlerin içindeki dosyaları koyarak de düzenleyebilirsiniz. Azure dosyaları hesabınıza izin verdiği sayıda dizin oluşturmanıza olanak sağlar. Aşağıdaki kodu adlı bir alt dizinde oluşturacak **sampledir** kök dizininin altındaki.

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

//Get a reference to the sampledir directory
CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

if (sampleDir.createIfNotExists()) {
    System.out.println("sampledir created");
} else {
    System.out.println("sampledir already exists");
}
```

## <a name="delete-a-directory"></a>Bir dizini silme
Hala dosyalarını içeren bir dizin ya da diğer dizinleri silinemiyor unutulmamalıdır rağmen bir dizini silme basit bir görev olduğundan.

```java
// Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

// Get a reference to the directory you want to delete
CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

// Delete the directory
if ( containerDir.deleteIfExists() ) {
    System.out.println("Directory deleted");
}
```

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a>Dosyalar ve dizinler bir Azure dosya paylaşımı içindeki numaralandırın
Bir paylaşımı içindeki dosyalara ve dizinlere listesini kolayca yapılır çağırarak **listFilesAndDirectories** CloudFileDirectory başvuru üzerinde. Yöntemi, üzerinde yineleme ListFileItem nesnelerin bir listesini döndürür. Örnek olarak, aşağıdaki kod, dosyalar ve dizinler kök dizin içinde listelenir.

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
    System.out.println(fileItem.getUri());
}
```

## <a name="upload-a-file"></a>Dosyayı karşıya yükleme
Bu bölümde, bir paylaşım kök dizini üzerine yerel depodan bir dosyayı karşıya yüklemeyi öğreneceksiniz.

Bir dosyayı karşıya yüklemeyi ilk adımı, bulunduğu dizinine başvuru elde edilir. Çağrı yaparak bunu **getRootDirectoryReference** Paylaşım nesnesinin yöntemi.

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();
```

Paylaşım kök dizini başvuru olduğuna göre üzerine aşağıdaki kodu kullanarak bir dosyayı karşıya yükleyebilirsiniz.

```java
        // Define the path to a local file.
        final String filePath = "C:\\temp\\Readme.txt";
    
        CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
        cloudFile.uploadFromFile(filePath);
```

## <a name="download-a-file"></a>Dosya indirme
Azure dosyaları karşı gerçekleştirir daha sık operations dosyaları indirmek için biridir. Aşağıdaki örnekte, kod SampleFile.txt indirir ve içeriğini görüntüler.

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

//Get a reference to the directory that contains the file
CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

//Get a reference to the file you want to download
CloudFile file = sampleDir.getFileReference("SampleFile.txt");

//Write the contents of the file to the console.
System.out.println(file.downloadText());
```

## <a name="delete-a-file"></a>Dosyayı silme
Başka bir yaygın Azure dosyaları dosya silme işlemidir. Aşağıdaki kod SampleFile.txt adlı bir dizin içinde depolanan adlı bir dosya siler **sampledir**.

```java
// Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

// Get a reference to the directory where the file to be deleted is in
CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

String filename = "SampleFile.txt"
CloudFile file;

file = containerDir.getFileReference(filename)
if ( file.deleteIfExists() ) {
    System.out.println(filename + " was deleted");
}
```

## <a name="next-steps"></a>Sonraki adımlar
Diğer Azure depolama API'leri hakkında daha fazla bilgi edinmek istiyorsanız, aşağıdaki bağlantıları izleyin.

* [Java geliştiricileri için Azure](/java/azure)/)
* [Java için Azure depolama SDK'si](https://github.com/azure/azure-storage-java)
* [Azure depolama SDK'sı Android için](https://github.com/azure/azure-storage-android)
* [Azure Depolama İstemcisi SDK Başvurusu](http://dl.windowsazure.com/storage/javadoc/)
* [Azure Depolama Hizmetleri REST API'si](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Azure Depolama Ekibi Blog’u](https://blogs.msdn.com/b/windowsazurestorage/)
* [AzCopy Komut Satırı Yardımcı Programı ile veri aktarımı](../common/storage-use-azcopy.md)
* [Azure Dosyaları sorunlarını giderme - Windows](storage-troubleshoot-windows-file-connection-problems.md)
