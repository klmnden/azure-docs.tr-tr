---
title: "BLOB storage (nesne depolama) C++ içinden kullanma | Microsoft Docs"
description: "Azure Blob Storage (nesne depolama) ile bulutta yapılandırılmamış veri depolayın."
services: storage
documentationcenter: .net
author: MichaelHauss
manager: vamshik
editor: tysonn
ms.assetid: 53844120-1c48-4e2f-8f77-5359ed0147a4
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: michaelhauss
ms.openlocfilehash: 9fe2112370f7d29eb0fde856995768660f9871e6
ms.sourcegitcommit: d6ad3203ecc54ab267f40649d3903584ac4db60b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2017
---
# <a name="how-to-use-blob-storage-from-c"></a>C++ içinden BLOB Storage kullanma
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Genel Bakış
Azure Blob Storage, bulutta nesne/blob olarak yapılandırılmamış veri depolayan bir hizmettir. Blob Storage belge, medya dosyası veya uygulama yükleyici gibi her tür metin veya ikili veri depolayabilir. Blob Storage aynı zamanda nesne depolama olarak adlandırılır.

Bu kılavuz Azure Blob Depolama hizmeti kullanılarak yaygın senaryolar gerçekleştirme gösterilmektedir. C++ ve kullanım örnekleri yazılır [C++ için Azure Storage istemci Kitaplığı](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). Kapsamdaki senaryolar dahil **karşıya**, **listeleme**, **indirme**, ve **silme** BLOB'lar.  

> [!NOTE]
> Bu kılavuz, c++ sürümü 1.0.0 ve yukarıda Azure Storage istemci kitaplığı hedefler. Aracılığıyla kullanılabilir olan depolama istemci kitaplığı 2.2.0, önerilen sürümüdür [NuGet](http://www.nuget.org/packages/wastorage) veya [GitHub](https://github.com/Azure/azure-storage-cpp).
> 
> 

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>C++ uygulaması oluşturma
Bu kılavuzda, C++ uygulamasında çalıştırabileceğiniz depolama özelliklerini kullanır.  

Bunu yapmak için Azure Storage istemci kitaplığı C++ için yükleme ve Azure aboneliğinizde bir Azure depolama hesabı oluşturmanız gerekir.   

C++ için Azure Storage istemci kitaplığı yüklemek için aşağıdaki yöntemleri kullanabilirsiniz:

* **Linux:** verilen yönergeleri izleyerek [C++ Benioku için Azure Storage istemci Kitaplığı](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) sayfası.  
* **Windows:** Visual Studio'da sırasıyla **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**. Aşağıdaki komutu yazın [NuGet Paket Yöneticisi Konsolu](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) ve basın **ENTER**.  
  
     Install-Package wastorage

## <a name="configure-your-application-to-access-blob-storage"></a>BLOB depolama alanına erişmek için uygulamanızı yapılandırın
Azure depolama API'leri BLOB'lar erişmek için kullanmasını istediğiniz C++ dosyanın en üstüne deyimlerini şunlar ekleyin:  

```cpp
#include <was/storage_account.h>
#include <was/blob.h>
#include <cpprest/filestream.h>  
#include <cpprest/containerstream.h> 
```

## <a name="setup-an-azure-storage-connection-string"></a>Bir Azure depolama bağlantı dizesini ayarlayın
Bir Azure storage istemci uç noktaları ve Veri Yönetimi Hizmetleri erişmek için kimlik bilgilerini depolamak için bir depolama bağlantı dizesi kullanır. Bir istemci uygulamasında çalıştırırken, depolama hesabı altında listelenen için adını depolama hesabınız ve depolama erişim tuşunu kullanarak depolama bağlantı dizesi şu biçimde sağlamalısınız [Azure Portal](https://portal.azure.com) için *AccountName* ve *AccountKey* değerleri. Depolama hesapları ve erişim anahtarları hakkında daha fazla bilgi için bkz: [Azure Storage hesapları hakkında](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json). Bu örnek, bağlantı dizesi tutmak için statik bir alana nasıl bildirebilir gösterir:  

```cpp
// Define the connection-string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

Yerel Windows bilgisayarınızda uygulamanızı test etmek için Microsoft Azure kullanabilirsiniz [depolama öykünücüsü](../storage-use-emulator.md) ile yüklü [Azure SDK'sı](https://azure.microsoft.com/downloads/). Depolama öykünücüsü Blob, kuyruk ve Tablo Hizmetleri, yerel geliştirme makinenizde Azure'da kullanılabilir benzetim yapan bir yardımcı programdır. Aşağıdaki örnek, yerel depolama öykünücüsü için bağlantı dizesi tutmak için statik bir alana nasıl bildirebilir gösterir:

```cpp
// Define the connection-string with Azure Storage Emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

Azure storage öykünücüsü başlatmak için **Başlat** düğmesini veya tuşuna **Windows** anahtarı. Yazmaya başlayın **Azure Storage öykünücüsü**seçip **Microsoft Azure Storage öykünücüsü** uygulamalar listesinden.  

Aşağıdaki örnekler, bu iki yöntemden birini depolama bağlantı dizesini almak için kullanılan olduğunu varsayalım.  

## <a name="retrieve-your-connection-string"></a>Bağlantı dizesi alma
Kullanabileceğiniz **cloud_storage_account** depolama hesabı bilgileri temsil eden sınıf. Depolama bağlantı dizesi, depolama hesabı bilgilerini almak için kullanabileceğiniz **ayrıştırma** yöntemi.  

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

Ardından, bir başvuru almak bir **cloud_blob_client** sınıfı gibi kapsayıcılar ve bloblar Blob Depolama hizmet içinde depolanan temsil eden nesneler almanıza olanak tanır. Aşağıdaki kod oluşturur bir **cloud_blob_client** biz alınan yukarıda depolama hesabı nesnesini kullanarak nesnesi:  

```cpp
// Create the blob client.
azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();  
```

## <a name="how-to-create-a-container"></a>Nasıl yapılır: bir kapsayıcı oluşturma
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

Varsayılan olarak yeni kapsayıcı özeldir ve Bu kapsayıcıdan BLOB indirmek için depolama erişim anahtarınızı belirtmeniz gerekir. Dosyaları (BLOB) kapsayıcı içinde herkes tarafından kullanılabilmesini sağlamak istiyorsanız, aşağıdaki kodu kullanarak genel kapsayıcıya ayarlayabilirsiniz:  

```cpp
// Make the blob container publicly accessible.
azure::storage::blob_container_permissions permissions;
permissions.set_public_access(azure::storage::blob_container_public_access_type::blob);
container.upload_permissions(permissions);  
```

Internet üzerinden herkes ortak bir kapsayıcıdaki blobları görebilir ancak değiştirdiğinizde ya da yalnızca uygun erişim anahtarı varsa, bunları silin.  

## <a name="how-to-upload-a-blob-into-a-container"></a>Nasıl yapılır: bir kapsayıcıya bir blob karşıya yükleme
Azure Blob Storage blok blobları ve sayfa bloblarını destekler. Çoğu durumda kullanılması önerilen blob türü blok blobudur.  

Bir dosyayı bir blok blobuna yüklemek için bir kapsayıcı başvurusu alın ve blok blob başvurusu almak için kullanın. Bir blob başvurusu edindiğinizde veri kendisine çağırarak yükleyebilirsiniz **upload_from_stream** yöntemi. Bu işlemle, eğer önceden oluşturulmadıysa bir blob oluşturulacaktır, aksi takdirde üzerine yazılacaktır. Aşağıdaki örnek kapsayıcının önceden oluşturulduğunu varsayarak bir blobun bir kapsayıcıya nasıl yükleneceğini gösterir.  

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

Alternatif olarak, kullanabileceğiniz **upload_from_file** bir dosyayı bir blok blobuna yüklemek için yöntem.

## <a name="how-to-list-the-blobs-in-a-container"></a>Nasıl yapılır: bir kapsayıcıda BLOB'ları Listele
Blob’ları bir kapsayıcıda listelemek için ilk olarak bir kapsayıcı başvurusu edinin. Kapsayıcının sonra kullanabileceğiniz **list_blobs** blobları ve/veya dizinleri içindeki alma yöntemi. Zengin özellik ve yöntem erişmek için **list_blob_item**, çağırmalısınız **list_blob_item.as_blob** almak için yöntemi bir **cloud_blob** nesnesi veya **list_blob.as_directory** cloud_blob_directory nesnesini almak için yöntemi. Aşağıdaki kodda almak ve her öğe URI'sini çıkış gösterilmiştir **örnek kapsayıcı my** kapsayıcı:

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

İşlem listeleme konusunda daha fazla ayrıntı için bkz: [listesi Azure depolama kaynaklarını c++](../storage-c-plus-plus-enumeration.md).

## <a name="how-to-download-blobs"></a>Nasıl yapılır: BLOB'ları indirme
BLOB'ları indirmek için önce bir blob başvurusu alın ve ardından çağırın **download_to_stream** yöntemi. Aşağıdaki örnek kullanır **download_to_stream** blob içeriklerini sonra yerel bir dosyaya kalıcı bir akış nesnesine aktarmak için yöntem.  

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

Alternatif olarak, kullanabileceğiniz **download_to_file** bir dosyaya bir blob içeriğini indirmek için yöntem.
Ayrıca, ayrıca kullanabileceğiniz **download_text** yöntemi bir blob'u bir metin dizesi olarak içeriğini indirmek için.  

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

## <a name="how-to-delete-blobs"></a>Nasıl yapılır: BLOB'ları silme
Bir blobu silmek için önce bir blob başvurusu alın ve ardından çağıran **delete_blob** yöntemini.  

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
Blob storage'nın öğrendiğinize göre Azure Storage hakkında daha fazla bilgi için aşağıdaki bağlantıları izleyin.  

* [C++ içinden kuyruk depolama kullanma](../storage-c-plus-plus-how-to-use-queues.md)
* [Tablo depolama C++ içinden kullanma](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [C++'ta listesi Azure Storage kaynakları](../storage-c-plus-plus-enumeration.md)
* [C++ başvurusu için depolama istemci kitaplığı](http://azure.github.io/azure-storage-cpp)
* [Azure Depolama Belgeleri](https://azure.microsoft.com/documentation/services/storage/)
* [AzCopy komut satırı yardımcı programı ile veri aktarımı](../storage-use-azcopy.md)

