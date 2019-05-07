---
title: C++ - Azure nesne (Blob) depolama kullanma nasıl | Microsoft Docs
description: Azure Blob (nesne) depolama ile bulutta yapılandırılmamış veri Store.
services: storage
author: mhopkins-msft
ms.service: storage
ms.topic: conceptual
ms.date: 03/21/2018
ms.author: mhopkins
ms.reviewer: seguler
ms.subservice: blobs
ms.openlocfilehash: 519190b6aeb313f25eddd717bce1a72148c8c518
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148459"
---
# <a name="how-to-use-blob-storage-from-c"></a>BLOB depolama alanından C++ kullanma

Bu kılavuz, yaygın senaryoları Azure Blob depolamayı kullanarak nasıl gerçekleştireceğinizi gösterir. Örnekleri karşıya yükleme, listesinde, indirmek ve blobları silme işlemini göstermektedir. Örnekler C++ dilinde yazılmıştır ve [C++ için Azure Depolama İstemci Kitaplığı](https://github.com/Azure/azure-storage-cpp/blob/master/README.md)’nı kullanır.   

Blob Depolama hakkında daha fazla bilgi için bkz: [Azure Blob depolamaya giriş](storage-blobs-introduction.md).

> [!NOTE]
> Bu kılavuz C++ için Azure Depolama İstemci Kitaplığı sürüm 1.0.0 ve üzerini hedefler. Microsoft öneriyor aracılığıyla kullanılabilir olan C++ için depolama istemci Kitaplığı'nın en son sürümünü kullanarak [NuGet](https://www.nuget.org/packages/wastorage) veya [GitHub](https://github.com/Azure/azure-storage-cpp).

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>C++ uygulaması oluşturma
Bu kılavuzda, bir C++ uygulaması içinde çalışabilen depolama özelliklerini kullanır.  

Bunu yapmak için, C++ için Azure Depolama İstemci Kitaplığı’nı yüklemeniz ve Azure aboneliğinizde bir Azure depolama hesabı oluşturmanız gerekir.   

C++ için Azure Depolama İstemci Kitaplığı’nı aşağıdaki yöntemleri kullanarak yükleyebilirsiniz:

* **Linux:** Verilen yönergeleri izleyerek [C++ Benioku için Azure depolama istemci Kitaplığı](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) sayfası.  
* **Windows:** Visual Studio'da, **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**’na tıklayın. Aşağıdaki komutu yazın [NuGet Paket Yöneticisi Konsolu](https://docs.nuget.org/docs/start-here/using-the-package-manager-console) basın **ENTER**.  
  
     Install-Package wastorage

## <a name="configure-your-application-to-access-blob-storage"></a>BLOB depolama alanına erişmek için uygulamanızı yapılandırma
Bloblara erişmek için Azure depolama API'leri kullanmak istediğiniz C++ dosyasının en üstüne deyimleriyle şunlardır ekleyin:  

```cpp
#include <was/storage_account.h>
#include <was/blob.h>
#include <cpprest/filestream.h>  
#include <cpprest/containerstream.h> 
```

## <a name="setup-an-azure-storage-connection-string"></a>Bir Azure depolama bağlantı dizesi ayarlama
Azure depolama istemcisi, veri yönetimi hizmetlerine erişmek üzere uç noktaları ve kimlik bilgilerini depolamak için bir depolama bağlantı dizesi kullanır. Bir istemci uygulamasında çalışırken, listelenen depolama hesabı için depolama hesabınızın ve depolama erişim anahtarı adını kullanarak depolama bağlantı dizesi şu biçimde sağlamanız gereken [Azure portalı](https://portal.azure.com) için *AccountName* ve *AccountKey* değerleri. Depolama hesapları ve erişim anahtarları hakkında daha fazla bilgi için bkz. [Azure Storage hesapları hakkında](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json). Bu örnekte bağlantı dizesini tutmak için nasıl statik bir alan bildirebileceğiniz gösterilmektedir:  

```cpp
// Define the connection-string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

Yerel Windows bilgisayarınızda uygulamanızı test etmek için Microsoft Azure kullanabilirsiniz [depolama öykünücüsü](../storage-use-emulator.md) ile yüklenen [Azure SDK'sı](https://azure.microsoft.com/downloads/). Depolama öykünücüsü, yerel geliştirme makinenizde Azure'da kullanıma sunulan Blob, kuyruk ve Tablo Hizmetleri taklit eden bir yardımcı programdır. Aşağıdaki örnekte bağlantı dizesini yerel depolama öykünücünüzde tutmak için nasıl statik bir alan bildirebileceğiniz gösterilmektedir:

```cpp
// Define the connection-string with Azure Storage Emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

Azure depolama öykünücüsü'nü başlatmak için **Başlat** düğme veya basın **Windows** anahtarı. Yazmaya başlayın **Azure Storage öykünücüsü**seçip **Microsoft Azure depolama öykünücüsü** uygulamalar listesinden.  

Aşağıdaki örnekler, depolama bağlantı dizesini almak için bu iki yöntemden birini kullandığınızı varsayar.  

## <a name="retrieve-your-connection-string"></a>Bağlantı dizenizi alma
Kullanabileceğiniz **cloud_storage_account** , depolama hesabı bilgileri temsil eden sınıf. Depolama bağlantı dizesinden depolama hesabı bilgilerini almak için **parse** yöntemini kullanabilirsiniz.  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

Ardından, bir başvuru almak bir **cloud_blob_client** kapsayıcılar ve bloblar Blob Depolama içinde depolanan temsil eden nesneleri almak sağladığından sınıfı. Aşağıdaki kod oluşturur bir **cloud_blob_client** biz alınan yukarıda depolama hesabı nesnesini kullanarak nesne:  

```cpp
// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();  
```

## <a name="how-to-create-a-container"></a>Nasıl yapılır: Bir kapsayıcı oluşturma
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

Bu örnek, zaten yoksa, nasıl bir kapsayıcı oluşturulacağını gösterir:  

```cpp
try
{
    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Create the container if it doesn't already exist.
    container.create_if_not_exists();
}
catch (const std::exception& e)
{
    std::wcout << U("Error: ") << e.what() << std::endl;
}  
```

Varsayılan olarak yeni kapsayıcı özeldir ve Bu kapsayıcıdan BLOB indirmek için depolama erişim anahtarınızı belirtmeniz gerekir. ' % S'dosya (BLOB) kapsayıcı içinde herkes tarafından kullanılabilmesini sağlamak istiyorsanız, kapsayıcı aşağıdaki kodu kullanarak genel olarak ayarlayabilirsiniz:  

```cpp
// Make the blob container publicly accessible.
azure::storage::blob_container_permissions permissions;
permissions.set_public_access(azure::storage::blob_container_public_access_type::blob);
container.upload_permissions(permissions);  
```

Internet'teki herkes ortak bir kapsayıcıdaki blobları görebilir ancak değiştirdiğinizde ya da yalnızca uygun erişim anahtarınız varsa bunları silin.  

## <a name="how-to-upload-a-blob-into-a-container"></a>Nasıl yapılır: Bir kapsayıcıya bir blob yükleme
Azure Blob Depolama destekler, blobları ve sayfa blobları engelleyin. Çoğu durumda kullanılması önerilen blob türü blok blobudur.  

Bir dosyayı bir blok blobuna yüklemek için bir kapsayıcı başvurusu alın ve blok blob başvurusu almak için kullanın. Bir blob başvurusunu aldıktan sonra herhangi bir veri akışı için çağırarak yükleyebilirsiniz **upload_from_stream** yöntemi. Bu işlemle, eğer önceden oluşturulmadıysa bir blob oluşturulacaktır, aksi takdirde üzerine yazılacaktır. Aşağıdaki örnek kapsayıcının önceden oluşturulduğunu varsayarak bir blobun bir kapsayıcıya nasıl yükleneceğini gösterir.  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference to a previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference to a blob named "my-blob-1".
azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

// Create or overwrite the "my-blob-1" blob with contents from a local file.
concurrency::streams::istream input_stream = concurrency::streams::file_stream<uint8_t>::open_istream(U("DataFile.txt")).get();
blockBlob.upload_from_stream(input_stream);
input_stream.close().wait();

// Create or overwrite the "my-blob-2" and "my-blob-3" blobs with contents from text.
// Retrieve a reference to a blob named "my-blob-2".
azure::storage::cloud_block_blob blob2 = container.get_block_blob_reference(U("my-blob-2"));
blob2.upload_text(U("more text"));

// Retrieve a reference to a blob named "my-blob-3".
azure::storage::cloud_block_blob blob3 = container.get_block_blob_reference(U("my-directory/my-sub-directory/my-blob-3"));
blob3.upload_text(U("other text"));  
```

Alternatif olarak, **upload_from_file** bir dosyayı bir blok blobuna yüklemek için yöntemi.

## <a name="how-to-list-the-blobs-in-a-container"></a>Nasıl yapılır: Blob’ları bir kapsayıcıda listeleme
Blob’ları bir kapsayıcıda listelemek için ilk olarak bir kapsayıcı başvurusu edinin. Daha sonra kapsayıcının kullanabilirsiniz **list_blobs** blobları ve/veya dizinleri içine almak için yöntemi. Zengin özellik ve yöntem dönen erişmeye **list_blob_item**, çağırmalısınız **list_blob_item.as_blob** almak için yöntemi bir **cloud_blob** nesnesi veya **list_blob.as_directory** cloud_blob_directory nesne almak için yöntemi. Aşağıdaki kod, almak ve her nesnenin URI çıkış gösterilmiştir **örnek kapsayıcı my** kapsayıcı:

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference to a previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Output URI of each item.
azure::storage::list_blob_item_iterator end_of_results;
for (auto it = container.list_blobs(); it != end_of_results; ++it)
{
    if (it->is_blob())
    {
        std::wcout << U("Blob: ") << it->as_blob().uri().primary_uri().to_string() << std::endl;
    }
    else
    {
        std::wcout << U("Directory: ") << it->as_directory().uri().primary_uri().to_string() << std::endl;
    }
}
```

İşlemlerini listeleyen hakkında ayrıntılı bilgi için bkz: [listesi Azure depolama kaynaklarını C++ dilinde](../storage-c-plus-plus-enumeration.md).

## <a name="how-to-download-blobs"></a>Nasıl yapılır: Blob’ları indirme
Blobları indirmek için önce bir blob başvurusu alın ve sonra çağrı **download_to_stream** yöntemi. Aşağıdaki örnekte **download_to_stream** blob içeriklerini, ardından yerel bir dosyaya kalıcı bir akış nesnesine aktarmak için yöntemi.  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference to a previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference to a blob named "my-blob-1".
azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

// Save blob contents to a file.
concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
concurrency::streams::ostream output_stream(buffer);
blockBlob.download_to_stream(output_stream);

std::ofstream outfile("DownloadBlobFile.txt", std::ofstream::binary);
std::vector<unsigned char>& data = buffer.collection();

outfile.write((char *)&data[0], buffer.size());
outfile.close();  
```

Alternatif olarak, **download_to_file** bir dosyaya bir blobun içeriklerini indirmek için yöntemi.
Ayrıca, ayrıca kullanabileceğiniz **download_text** bir metin dizesi olarak bir blobun içeriklerini indirmek için yöntemi.  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference to a previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference to a blob named "my-blob-2".
azure::storage::cloud_block_blob text_blob = container.get_block_blob_reference(U("my-blob-2"));

// Download the contents of a blog as a text string.
utility::string_t text = text_blob.download_text();
```

## <a name="how-to-delete-blobs"></a>Nasıl yapılır: Blob’ları silme
Bir blobu silmek için önce bir blob başvurusu alın ve sonra çağrı **delete_blob** yöntemini.  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

// Retrieve a reference to a previously created container.
azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

// Retrieve reference to a blob named "my-blob-1".
azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

// Delete the blob.
blockBlob.delete_blob();
```

## <a name="next-steps"></a>Sonraki adımlar
Blob storage'nın temellerini öğrendiğinize göre Azure depolama hakkında daha fazla bilgi için bu bağlantıları izleyin.  

* [Kuyruk Depolama'yı C++ kullanma](../storage-c-plus-plus-how-to-use-queues.md)
* [Tablo depolama'yı C++ kullanma](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [Liste Azure depolama kaynaklarını C++ dilinde](../storage-c-plus-plus-enumeration.md)
* [C++ başvurusu için depolama istemcisi kitaplığı](https://azure.github.io/azure-storage-cpp)
* [Azure Depolama Belgeleri](https://azure.microsoft.com/documentation/services/storage/)
* [AzCopy komut satırı yardımcı programı ile veri aktarımı](../storage-use-azcopy.md)

