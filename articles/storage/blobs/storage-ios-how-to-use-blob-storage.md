---
title: İOS - Azure nesne (Blob) depolama kullanma | Microsoft Docs
description: Azure Blob Storage (nesne depolama) ile bulutta yapılandırılmamış veri depolayın.
services: storage
author: mhopkins-msft
ms.service: storage
ms.devlang: objective-c
ms.topic: article
ms.date: 11/20/2018
ms.author: mhopkins
ms.reviewer: seguler
ms.subservice: blobs
ms.openlocfilehash: 87651aa1fd44a831e94a00b5871faaae51f2f6a7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65148302"
---
# <a name="how-to-use-blob-storage-from-ios"></a>BLOB depolama alanından iOS kullanma

Bu makalede, Microsoft Azure Blob depolamayı kullanarak yaygın senaryoları gerçekleştirmek gösterilmektedir. Objective-C ve kullanım örnekleri yazılır [iOS için Azure depolama istemci Kitaplığı](https://github.com/Azure/azure-storage-ios). Kapsanan senaryolar, karşıya yükleme, listeleme, indirme ve BLOB'ları silmeden içerir. BLOB'ları hakkında daha fazla bilgi için bkz. [sonraki adımlar](#next-steps) bölümü. Ayrıca yükleyebilirsiniz [örnek uygulaması](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) hızlıca bir iOS uygulamasına Azure depolama kullanımını görmek için.

Blob Depolama hakkında daha fazla bilgi için bkz: [Azure Blob depolamaya giriş](storage-blobs-introduction.md).

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="import-the-azure-storage-ios-library-into-your-application"></a>Azure depolama iOS kitaplığı uygulamanın içine alma
Azure depolama iOS kitaplığı uygulamanın içine kullanılarak alabileceğiniz [Azure depolama CocoaPod](https://cocoapods.org/pods/AZSClient) veya alarak **Framework** dosya. Kolay mevcut projeniz için daha az müdahale eden ancak framework dosyasından içeri aktarma kitaplığı tümleştirme getirir CocoaPod önerilen yoludur.

Bu kitaplığı kullanmak için aşağıdakiler gerekir:
- iOS 8+
- Xcode 7+

## <a name="cocoapod"></a>CocoaPod
1. Bunu henüz yapmadıysanız [CocoaPods yükleme](https://guides.cocoapods.org/using/getting-started.html#toc_3) bilgisayarda bir terminal penceresi açıp ve aşağıdaki komutu çalıştırarak
    
    ```shell   
    sudo gem install cocoapods
    ```

2. Ardından, proje dizininde (, .xcodeproj dosyasını içeren dizin) adlı yeni bir dosya oluşturun _Podfile_(dosya uzantısı). Ekleyin _Podfile_ ve kaydedin.

    ```ruby
    platform :ios, '8.0'

    target 'TargetName' do
      pod 'AZSClient'
    end
    ```

3. Terminal penceresinde, proje dizinine gidin ve aşağıdaki komutu çalıştırın

    ```shell    
    pod install
    ```

4. Xcode'da, .xcodeproj açıksa, kapatın. Proje dizininizde .xcworkspace uzantısı olan yeni oluşturulan proje dosyasını açın. Bu, için artık üzerinde çalıştığınızın dosyasıdır.

## <a name="framework"></a>Framework
Kitaplığı kullanmak için başka bir şekilde framework el ile oluşturmaktır:

1. İlk olarak, indirin veya kopyalayın [azure-storage-ios depo](https://github.com/azure/azure-storage-ios).
2. Na *azure-storage-ios* -> *LIB* -> *Azure depolama istemci Kitaplığı*açın `AZSClient.xcodeproj` xcode'da.
3. Üst sol xcode, "Azure depolama istemci kitaplığı" "Framework" etkin düzeni değiştirin.
4. (⌘ + B) projeyi derleyin. Bu oluşturacak bir `AZSClient.framework` masaüstünüzde dosya.

Aşağıdakileri yaparak uygulamanıza sonra framework dosyasını içe aktarabilirsiniz:

1. Yeni bir proje oluşturun veya mevcut projenizi xcode'da açın.
2. Sürükle ve bırak `AZSClient.framework` , Xcode Proje Gezgini içinde.
3. Seçin *gerekirse öğeleri Kopyala*, tıklayın *son*.
4. Sol gezintide projenize tıklayarak *genel* proje düzenleyicisinden üst kısmındaki sekme.
5. Altında *bağlantılı çerçeveler ve kitaplıklar* bölümünde, Ekle (+) düğmesine tıklayın.
6. İçin zaten sağlanan kitaplıklara listesinde arama `libxml2.2.tbd` ve projenize ekleyin.

## <a name="import-the-library"></a>İçeri aktarma kitaplığı 
```objc
// Include the following import statement to use blob APIs.
#import <AZSClient/AZSClient.h>
```

Swift kullanıyorsanız, bir köprü oluşturma üst bilgisi oluşturma ve yok < AZSClient/AZSClient.h > içeri aktarma gerekir:

1. Üst bilgi dosyası oluştur `Bridging-Header.h`, yukarıdaki import deyimini ekleyin.
2. Git *Build Settings* sekmesinde ve arama *Objective-C köprü oluşturma üst bilgisi*.
3. Alanına çift *Objective-C köprü oluşturma üst bilgisi* ve yolun üst bilgisi dosyanıza ekleyin: `ProjectName/Bridging-Header.h`
4. Köprü oluşturma üst bilgisi Xcode tarafından toplanmış olduğunu doğrulamak için projeyi (⌘ + B) oluşturun.
5. Doğrudan bir Swift dosyasında kitaplığı kullanmaya başlamak, içeri aktarma deyimlerini için gerek yoktur.

[!INCLUDE [storage-mobile-authentication-guidance](../../../includes/storage-mobile-authentication-guidance.md)]

## <a name="asynchronous-operations"></a>Zaman uyumsuz işlemler
> [!NOTE]
> Bir hizmet isteği gerçekleştiren tüm yöntemler, zaman uyumsuz işlemlerdir. Kod örnekleri, bu yöntemler bir tamamlama işleyicisi olduğunu bulabilirsiniz. Kod tamamlama işleyicisine içinde çalıştırılacağı **sonra** isteğini tamamladı. Tamamlama işleyicisine çalışacak sonra kod **sırada** isteği yapılır.
> 
> 

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma
Azure storage'da her blob bir kapsayıcıda yer almalıdır. Aşağıdaki örnekte adlı bir kapsayıcı oluşturma işlemi gösterilmektedir *newcontainer*, zaten yoksa, depolama hesabınızdaki. Kapsayıcı için bir ad seçerken, yukarıda belirtilen adlandırma kurallarının oluşturduğunu unutmayın.

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

Bu durum bakarak çalıştığını doğrulayabilirsiniz [Microsoft Azure Depolama Gezgini](https://storageexplorer.com) ve doğrulanıyor *newcontainer* depolama hesabınız için kapsayıcıları listesi bulunmaktadır.

## <a name="set-container-permissions"></a>Kapsayıcı izinleri ayarlama
Bir kapsayıcının izinleri için yapılandırılmış olan **özel** varsayılan olarak erişim. Ancak, kapsayıcılar, kapsayıcı erişim için birkaç farklı seçenekler sunar:

* **Özel**: Kapsayıcı ve blob verileri yalnızca hesap sahibi tarafından okunabilir.
* **BLOB**: Anonim İstek bu kapsayıcıdaki BLOB verilerini okuyabilir, ancak kapsayıcı verileri mevcut değil. İstemcilerin anonim isteğiyle kapsayıcı içindeki blobları listelenemiyor.
* **kapsayıcı**: Kapsayıcı ve blob verilerini anonim istek okunabilir. İstemcilerin anonim isteğiyle kapsayıcı içindeki blobları listeleme, ancak depolama hesabında kapsayıcıları numaralandırılamıyor.

Aşağıdaki örnek ile bir kapsayıcı oluşturma işlemi gösterilmektedir **kapsayıcı** erişim izinlerini Internet üzerindeki tüm kullanıcılar için genel ve salt okunur erişim sağlar:

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
Blob hizmeti kavramları bölümünde belirtildiği gibi Blob Depolama farklı üç tür BLOB sunar: blok blobları, ekleme blobları ve sayfa blobları. Azure depolama iOS kitaplığı, üç tür BLOB'ları destekler. Çoğu durumda, kullanılması önerilen blob türü blok blobudur.

Aşağıdaki örnek, bir NSString blok blobu karşıya yükleme işlemini gösterir. Bu blob içeriğini, bu kapsayıcı içinde aynı ada sahip bir blob zaten varsa üzerine yazılır.

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

Bu durum bakarak çalıştığını doğrulayabilirsiniz [Microsoft Azure Depolama Gezgini](https://storageexplorer.com) ve doğrulanıyor kapsayıcı *containerpublic*, blob içeren *sampleblob*. Bu örnekte, bu uygulama bir BLOB URI'si giderek çalıştığını doğrulayabilmeniz için bir ortak kapsayıcı kullandık:

    https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob

Bir NSString blok blobu karşıya ek olarak, benzer yöntemler NSData, NSInputStream veya bir yerel dosya yok.

## <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme
Aşağıdaki örnek, bir kapsayıcıdaki tüm blobları listelemek gösterilmektedir. Bu işlemi gerçekleştirirken, aşağıdaki parametreleri oluşturduğunu unutmayın:     

* **continuationToken** -listeleme işlemi burada başlamalıdır devamlılık belirteci temsil eder. Hiçbir belirteç sağlanmazsa, baştan blobları listeler. En fazla kümesi kadar sıfırdan herhangi bir sayıda BLOB listelenebilir. Bile, bu yöntem sıfır sonuçları döndürür `results.continuationToken` nil, olmayan listede olmayan daha fazla hizmet bloblarda olabilir.
* **önek** -blob listesi için kullanılacak öneki belirtebilirsiniz. Bu ön Ekle başlayan blobları listelenir.
* **Listblobs** - belirtildiği gibi [adlandırma ve kapsayıcılar ve bloblar başvuran](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) bölümünde, Blob hizmeti, bir düz depolama düzeni olmasına rağmen oluşturabileceğiniz sanal bir hiyerarşi yol ile BLOB'ları adlandırarak bilgiler. Ancak, olmayan düz liste şu anda desteklenmiyor. Bu özellik yakında kullanıma sunulacaktır. Şimdilik, bu değer olmalıdır **Evet**.
* **blobListingDetails** -blobları listeleme dahil edilecek öğeleri belirtebilirsiniz.
  * _AZSBlobListingDetailsNone_: Yalnızca kaydedilmiş blobları listelemek ve blob meta verilerini döndürmüyor.
  * _AZSBlobListingDetailsSnapshots_: Taahhüt edilen blobları listelemek ve blob anlık görüntüleri.
  * _AZSBlobListingDetailsMetadata_: Her blob için blob meta veri alınamadı, liste döndürdü.
  * _AZSBlobListingDetailsUncommittedBlobs_: İşlenmiş ve kaydedilmemiş blobları listeleyin.
  * _AZSBlobListingDetailsCopy_: Özellikleri Kopyala listesindeki içerir.
  * _AZSBlobListingDetailsAll_: Tüm kullanılabilir kaydedilmiş BLOB'ları, işlenmemiş BLOB'ları ve anlık görüntüleri listeleyin ve bu bloblar için tüm meta verileri ve kopyalama durumu döndürür.
* **maxresults bağımsız değişkenini** -bu işlem için döndürülecek sonuç sayısı. Olmayan bir sınır ayarlamak için-1 değerini kullanın.
* **completionHandler** - listeleme işlemi sonuçlarını yürütmek için kod bloğu.

Bu örnekte, bir yardımcı yöntem için kullanılan yinelemeli olarak çağrı listesi blobları yöntemi her zaman bir devamlılık belirteci döndürülür.

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
Aşağıdaki örnek, bir NSString nesnesine bir blobu indirmek gösterilmektedir.

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
Aşağıdaki örnek, bir blobun silinmesi gösterilmektedir.

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

## <a name="delete-a-blob-container"></a>Bir blob kapsayıcısını Sil
Aşağıdaki örnek, bir kapsayıcıyı silme gösterilmektedir.

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
Blob Depolama iOS kullanma öğrendiniz, iOS kitaplığı ve depolama hizmeti hakkında daha fazla bilgi için bu bağlantıları izleyin.

* [İOS için Azure depolama istemci kitaplığı](https://github.com/azure/azure-storage-ios)
* [Azure depolama iOS başvuru belgeleri](https://azure.github.io/azure-storage-ios/)
* [Azure Depolama Hizmetleri REST API'si](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Azure Depolama Ekibi Blog’u](https://blogs.msdn.com/b/windowsazurestorage)

Bu kitaplığı ile ilgili sorularınız varsa şuraya gönder: çekinmeyin bizim [Azure MSDN Forumu](https://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) veya [Stack Overflow](https://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files).
Azure depolama için özellik önerileriniz varsa, lütfen deftere [Azure depolama geri bildirim](https://feedback.azure.com/forums/217298-storage/).

