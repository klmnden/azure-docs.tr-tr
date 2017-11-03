---
title: "Azure Blob depolama alanından iOS kullanma | Microsoft Docs"
description: "Azure Blob Storage (nesne depolama) ile bulutta yapılandırılmamış veri depolayın."
services: storage
documentationcenter: ios
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: df188021-86fc-4d31-a810-1b0e7bcd814b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: objective-c
ms.topic: article
ms.date: 05/11/2017
ms.author: michaelhauss
ms.openlocfilehash: f238804e6031fcf3f194695a06bf5b88733a27b9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-blob-storage-from-ios"></a>BLOB depolama alanından iOS kullanma
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Genel Bakış
Bu makalede Microsoft Azure Blob storage kullanarak yaygın senaryolar gerçekleştirmek nasıl yapacağınızı gösterir. Objective-C ve kullanım örnekleri yazılır [iOS için Azure Storage istemci Kitaplığı](https://github.com/Azure/azure-storage-ios). Kapsamdaki senaryolar dahil **karşıya**, **listeleme**, **indirme**, ve **silme** BLOB'lar. BLOB'ları hakkında daha fazla bilgi için bkz: [sonraki adımlar](#next-steps) bölümü. Ayrıca indirebilirsiniz [örnek uygulaması](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) hızlı bir şekilde bir iOS uygulamasına Azure Storage kullanımda görmek için.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="import-the-azure-storage-ios-library-into-your-application"></a>Azure depolama iOS kitaplığı uygulamanıza alma
Azure depolama iOS kitaplığı uygulamanıza kullanarak alabileceğiniz [Azure depolama CocoaPod](https://cocoapods.org/pods/AZSClient) veya alarak **Framework** dosya. Kolay varolan projeniz için daha az müdahale ancak framework dosyasından içeri aktarma kitaplığı tümleştirme getirir CocoaPod önerilen yoludur.

Bu kitaplığı kullanmak için aşağıdakiler gerekir:
- iOS 8 +
- Xcode 7 +

## <a name="cocoapod"></a>CocoaPod
1. Henüz yapmadıysanız [yükleme CocoaPods](https://guides.cocoapods.org/using/getting-started.html#toc_3) bir terminal penceresi açıp aşağıdaki komutu çalıştırarak, bilgisayarınızdaki
    
    ```shell   
    sudo gem install cocoapods
    ```

2. Ardından, proje dizininde (.xcodeproj dosyanızı içeren dizini) adlı yeni bir dosya oluşturun _Podfile_(dosya uzantısı yok). Aşağıdakileri ekleyin _Podfile_ ve kaydedin.

    ```ruby
    platform :ios, '8.0'

    target 'TargetName' do
      pod 'AZSClient'
    end
    ```

3. Terminal penceresi, proje dizinine gidin ve aşağıdaki komutu çalıştırın

    ```shell    
    pod install
    ```

4. Xcode'da, .xcodeproj açıksa, kapatın. Proje dizininde .xcworkspace uzantısına sahip olacak yeni oluşturulan proje dosyasını açın. Bu gelen için şimdi çalıştığınız dosyasıdır.

## <a name="framework"></a>Framework
Kitaplığı kullanarak diğer bir yol framework el ile oluşturmaktır:

1. İlk olarak, indirme ya da kopyalama [azure depolama ios depodaki](https://github.com/azure/azure-storage-ios).
2. Na *azure depolama ios* -> *Lib* -> *Azure Storage istemci Kitaplığı*, açarak `AZSClient.xcodeproj` xcode'da.
3. Sol üst, Xcode, "Azure Storage istemci kitaplığı" "Framework" etkin düzenine değiştirin.
4. Projeyi (⌘ + B) oluşturun. Bu oluşturacak bir `AZSClient.framework` masaüstünüzde dosya.

Aşağıdakileri yaparak uygulamanıza sonra framework dosyasını içe aktarabilirsiniz:

1. Yeni bir proje oluşturun veya varolan Xcode projenizde açın.
2. Sürükleme ve bırakma `AZSClient.framework` Xcode Proje Gezgini içine.
3. Seçin *gerekirse öğeleri Kopyala*ve tıklayın *son*.
4. Sol taraftaki gezinti projenizde tıklayın ve tıklayın *genel* proje Düzenleyicisi'ni üstündeki sekmesi.
5. Altında *bağlantılı çerçeveler ve kitaplıklar* bölümünde, (+) Ekle düğmesini tıklatın.
6. İçin zaten sağlanan Kitaplıklar listesinde arama `libxml2.2.tbd` ve projenize ekleyin.

## <a name="import-the-library"></a>İçeri aktarma kitaplığı 
```objc
// Include the following import statement to use blob APIs.
#import <AZSClient/AZSClient.h>
```

SWIFT kullanıyorsanız, bir köprü oluşturma üst bilgisi oluşturun ve < AZSClient/AZSClient.h > var. alma gerekir:

1. Bir üst bilgi dosyası oluştur `Bridging-Header.h`ve yukarıdaki içeri aktarma deyimini ekleyin.
2. Git *Build Settings* arayın ve sekmesinde *Objective-C köprü oluşturma üst bilgisi*.
3. Alanın üzerinde çift *Objective-C köprü oluşturma üst bilgisi* ve yolun üstbilgi dosyanıza ekleyin:`ProjectName/Bridging-Header.h`
4. Köprü oluşturma üst bilgisi tarafından Xcode çekilmiş olduğunu doğrulamak için projeyi (⌘ + B) oluşturun.
5. Kitaplık doğrudan SWIFT herhangi dosyasında kullanmaya başlamak, içeri aktarma deyimlerini için gerek yoktur.

[!INCLUDE [storage-mobile-authentication-guidance](../../../includes/storage-mobile-authentication-guidance.md)]

## <a name="asynchronous-operations"></a>Zaman uyumsuz işlemler
> [!NOTE]
> Bir hizmet isteği gerçekleştiren tüm yöntemleri zaman uyumsuz işlemleridir. Kod örnekleri, bu yöntemleri tamamlama işleyici olduğunu bulabilirsiniz. Kod tamamlama işleyici içinde çalışacak **sonra** isteğini tamamladı. Tamamlama işleyici çalıştırdıktan sonra kod **sırada** istek yapılır.
> 
> 

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma
Azure storage'daki her blob bir kapsayıcıda yer almalıdır. Aşağıdaki örnek adlı bir kapsayıcı oluşturulacağını gösterir *newcontainer*, zaten yoksa, depolama hesabınızdaki. Kapsayıcı için bir ad seçerken, yukarıda belirtilen adlandırma kuralları dikkatli olun.

```objc
-(void)createContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"newcontainer"];

    // Create container in your Storage account if the container doesn't already exist
    [blobContainer createContainerIfNotExistsWithCompletionHandler:^(NSError *error, BOOL exists) {
        if (error){
            NSLog(@"Error in creating container.");
        }
    }];
}
```

Bu bakarak çalıştığını onaylayın [Microsoft Azure Storage Gezgini](http://storageexplorer.com) ve olmadığını doğrulayarak *newcontainer* depolama hesabınız için kapsayıcılar listesi bulunmaktadır.

## <a name="set-container-permissions"></a>Kapsayıcı izinleri ayarlama
Bir kapsayıcının izinler için yapılandırılmış olan **özel** varsayılan erişim. Ancak, kapsayıcılar kapsayıcı erişim için birkaç farklı seçenekleri sağlar:

* **Özel**: kapsayıcı ve blob verileri yalnızca hesap sahibi tarafından okunabilir.
* **BLOB**: Bu kapsayıcı içindeki Blob verilerini anonim istek okunabilir ancak kapsayıcı verileri kullanılabilir değil. İstemcilerin anonim istek aracılığıyla kapsayıcıdaki blobları numaralandırılamıyor.
* **Kapsayıcı**: kapsayıcı ve blob verilerini anonim istek okunabilir. İstemcilerin anonim istek aracılığıyla kapsayıcıdaki blobları sıralayabilirsiniz, ancak depolama hesabı kapsayıcılara numaralandırılamıyor.

Aşağıdaki örnekte nasıl ile bir kapsayıcı oluşturulacağını gösterir **kapsayıcı** erişim Internet üzerindeki tüm kullanıcılar için genel, salt okunur erişime izin verip izinlerini:

```objc
-(void)createContainerWithPublicAccess{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Create container in your Storage account if the container doesn't already exist
    [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists){
        if (error){
            NSLog(@"Error in creating container.");
        }
    }];
}
```

## <a name="upload-a-blob-into-a-container"></a>Bir kapsayıcıya bir blob yükleme
Bölümünde belirtildiği gibi [Blob hizmeti kavramları](#blob-service-concepts) bölümü, Blob Storage üç farklı türde BLOB sunar: blok blobları, ekleme blobları ve sayfa blobları. Azure depolama iOS kitaplığı üç türde BLOB destekler. Çoğu durumda, kullanılması önerilen blob türü blok blobudur.

Aşağıdaki örnek, bir NSString blok blobundan karşıya nasıl yükleneceğini gösterir. Aynı ada sahip bir blob bu kapsayıcıda zaten varsa, bu blob içeriğinin üzerine yazılır.

```objc
-(void)uploadBlobToContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists)
        {
            if (error){
                NSLog(@"Error in creating container.");
            }
            else{
                // Create a local blob object
                AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

                // Upload blob to Storage
                [blockBlob uploadFromText:@"This text will be uploaded to Blob Storage." completionHandler:^(NSError *error) {
                    if (error){
                        NSLog(@"Error in creating blob.");
                    }
                }];
            }
        }];
}
```

Bu bakarak çalıştığını onaylayın [Microsoft Azure Storage Gezgini](http://storageexplorer.com) ve olmadığını doğrulayarak kapsayıcı *containerpublic*, blob'un bulunduğu *sampleblob*. Bu örnekte, bu uygulama BLOB URI'si giderek çalışılan doğrulayabilmeniz için ortak bir kapsayıcı kullandık:

    https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob

Bir NSString blok blobundan karşıya yanı sıra, NSData, NSInputStream veya yerel bir dosya için benzer yöntemler mevcut.

## <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme
Aşağıdaki örnek, bir kapsayıcıdaki tüm blob'lara listesinde gösterilmiştir. Bu işlemi gerçekleştirirken, aşağıdaki parametreleri dikkatli olun:     

* **continuationToken** -listeleme işlemi burada başlamalıdır devamlılık belirteci temsil eder. Herhangi bir belirteci sağlanırsa, başlangıçtan itibaren BLOB'ları listeler. En fazla kümesi kadar sıfırdan herhangi bir sayıda BLOB listelenebilir. Bu yöntem, sıfır sonuçları döndürür olsa bile `results.continuationToken` nil, olmayan değil listelenen başka BLOB hizmeti üzerinde olabilir.
* **önek** -blob listesi için kullanılacak öneki belirtin. Bu önek ile başlayan BLOB'lar listelenir.
* **Listblobs** - bölümünde belirtildiği gibi [adlandırma ve kapsayıcılar ve bloblar başvuran](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) bölümünde, bir düz depolama düzeni Blob hizmeti olmasına rağmen oluşturabilirsiniz sanal bir hiyerarşi BLOB'lar yolu ile adlandırılmasıyla bilgi. Ancak, düz olmayan listeleme şu anda desteklenmiyor. Bu özellik yakında çıkıyor. Şimdilik, bu değer olmalıdır **Evet**.
* **blobListingDetails** -BLOB'lar listelerken dahil edilecek maddeleri belirtebilirsiniz
  * _AZSBlobListingDetailsNone_: kaydedilmiş BLOB'ları yalnızca listelemek ve blob verilerini döndürmenizi değil.
  * _AZSBlobListingDetailsSnapshots_: kaydedilmiş BLOB'ları listeler ve blob anlık görüntüler.
  * _AZSBlobListingDetailsMetadata_: döndürülen listesindeki her bir blob için blob meta veri alınamadı.
  * _AZSBlobListingDetailsUncommittedBlobs_: kaydedilmiş ve kaydedilmemiş BLOB'ları listeler.
  * _AZSBlobListingDetailsCopy_: kopyalama dahil listenin özelliklerinde.
  * _AZSBlobListingDetailsAll_: tüm kullanılabilir kaydedilmiş BLOB'ları, kaydedilmemiş BLOB'ları ve anlık görüntüleri listesi ve bu BLOB'lar için tüm meta veri ve kopyalama durumu döndürür.
* **maxResults** -bu işlem için döndürülecek sonuç sayısı. Bir sınır ayarlamamayı -1 kullanın.
* **completionHandler** - listeleme işlemi sonuçlarıyla yürütülecek kod bloğu.

Bu örnekte, bir yardımcı yöntemi için kullanılan yinelemeli olarak çağrı listesi BLOB'devamlılık belirteci döndürülen her zaman yöntemi.

```objc
-(void)listBlobsInContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    //List all blobs in container
    [self listBlobsInContainerHelper:blobContainer continuationToken:nil prefix:nil blobListingDetails:AZSBlobListingDetailsAll maxResults:-1 completionHandler:^(NSError *error) {
        if (error != nil){
            NSLog(@"Error in creating container.");
        }
    }];
}

//List blobs helper method
-(void)listBlobsInContainerHelper:(AZSCloudBlobContainer *)container continuationToken:(AZSContinuationToken *)continuationToken prefix:(NSString *)prefix blobListingDetails:(AZSBlobListingDetails)blobListingDetails maxResults:(NSUInteger)maxResults completionHandler:(void (^)(NSError *))completionHandler
{
    [container listBlobsSegmentedWithContinuationToken:continuationToken prefix:prefix useFlatBlobListing:YES blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:^(NSError *error, AZSBlobResultSegment *results) {
        if (error)
        {
            completionHandler(error);
        }
        else
        {
            for (int i = 0; i < results.blobs.count; i++) {
                NSLog(@"%@",[(AZSCloudBlockBlob *)results.blobs[i] blobName]);
            }
            if (results.continuationToken)
            {
                [self listBlobsInContainerHelper:container continuationToken:results.continuationToken prefix:prefix blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:completionHandler];
            }
            else
            {
                completionHandler(nil);
            }
        }
    }];
}
```

## <a name="download-a-blob"></a>Blob indirme
Aşağıdaki örnek, bir NSString nesnesine bir blob indirmek gösterilmiştir.

```objc
-(void)downloadBlobToString{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Create a local blob object
    AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

    // Download blob
    [blockBlob downloadToTextWithCompletionHandler:^(NSError *error, NSString *text) {
        if (error) {
            NSLog(@"Error in downloading blob");
        }
        else{
            NSLog(@"%@",text);
        }
    }];
}
```

## <a name="delete-a-blob"></a>Blob silme
Aşağıdaki örnek, bir blobu silmek gösterilmektedir.

```objc
-(void)deleteBlob{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Create a local blob object
    AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob1"];

    // Delete blob
    [blockBlob deleteWithCompletionHandler:^(NSError *error) {
        if (error) {
            NSLog(@"Error in deleting blob.");
        }
    }];
}
```

## <a name="delete-a-blob-container"></a>Bir blob kapsayıcısından silin
Aşağıdaki örnek, bir kapsayıcı silmek gösterilmiştir.

```objc
-(void)deleteContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Delete container
    [blobContainer deleteContainerIfExistsWithCompletionHandler:^(NSError *error, BOOL success) {
        if(error){
            NSLog(@"Error in deleting container");
        }
    }];
}
```

## <a name="next-steps"></a>Sonraki adımlar
Artık iOS Blob Storage kullanma öğrendiğinize göre iOS kitaplığı ve depolama hizmeti hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin.

* [İOS için Azure Storage istemci kitaplığı](https://github.com/azure/azure-storage-ios)
* [Azure depolama iOS başvuru belgeleri](http://azure.github.io/azure-storage-ios/)
* [Azure Depolama Hizmetleri REST API'si](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Azure Depolama Ekibi Blog’u](http://blogs.msdn.com/b/windowsazurestorage)

Bu kitaplığı ile ilgili sorularınız varsa, gönderilecek çekinmeyin bizim [MSDN Azure Forumu](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) veya [yığın taşması](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files).
Azure Storage için özellik önerileri varsa, lütfen deftere [Azure depolama geri bildirim](https://feedback.azure.com/forums/217298-storage/).

