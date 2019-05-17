---
title: Blob depolamaya giriş - Azure’da nesne depolama
description: Azure Blob depolama, metin veya ikili veri gibi çok miktarda yapılandırılmamış nesne verilerini depolar. Azure Blob depolama, yüksek oranda ölçeklenebilir ve kullanılabilir. İstemciler, REST kullanarak veya Azure Depolama istemci kitaplıkları aracılığıyla programlama yoluyla Azure CLI’den veya PowerShell’den Blob depolamadaki veri nesnelerine erişebilir.
services: storage
author: tamram
ms.service: storage
ms.topic: overview
ms.date: 01/03/2019
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 498dd3be75cb27263c811d9db541b2e040599156
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65788014"
---
# <a name="introduction-to-azure-blob-storage"></a>Azure Blob depolamaya giriş

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

## <a name="blob-storage-resources"></a>BLOB depolama kaynaklarını

BLOB Depolama üç tür kaynak sunar:

- **Depolama hesabı**. 
- A **kapsayıcı** depolama hesabındaki
- A **blob** bir kapsayıcıda 

Aşağıdaki diyagramda bu kaynaklar arasındaki ilişki gösterilmektedir.

![Blob Depolama mimarisi diyagramı](./media/storage-blob-introduction/blob1.png)

### <a name="storage-accounts"></a>Depolama hesapları

Bir depolama hesabı, Azure, verileriniz için benzersiz bir ad sağlar. Azure Storage'a depoladığınız her nesnenin benzersiz hesabınızın adını içeren bir adresi vardır. Hesap adı ve Azure depolama blob uç noktası birleşimi, depolama hesabınızdaki nesneler için taban adresi oluşturur.

Örneğin, depolama hesabınızın adı *mystorageaccount*, Blob Depolama için varsayılan uç nokta ise:

```
http://mystorageaccount.blob.core.windows.net 
```

Bir depolama hesabı oluşturmak için bkz: [depolama hesabı oluşturma](../common/storage-quickstart-create-account.md). Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../common/storage-account-overview.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="containers"></a>Kapsayıcılar

Bir kapsayıcı, BLOB, dosya sistemindeki bir dizin için benzer bir dizi düzenler. Depolama hesabında sınırsız sayıda kapsayıcı olabilir ve her kapsayıcı sınırsız sayıda blob depolayabilir. 

  > [!NOTE]
  > Kapsayıcı adındaki harfler küçük harf olmalıdır. Adlandırma kapsayıcıları hakkında daha fazla bilgi için bkz. [adlandırma ve başvuran kapsayıcıları, Blobları ve meta verileri](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).

### <a name="blobs"></a>Bloblar
 
Azure depolama üç tür BLOB destekler:

* **Blok blobları** yaklaşık 4,7 TB'a kadar metin ve ikili verileri depolayın. Blok blobları, ayrı ayrı yönetilebilen veri bloklarından oluşur.
* **Ekleme blobları** bloklarını yapılan blok blobları gibi çalışır, ancak için optimize edilmiş ekleme işlemleri. Ekleme blobları sanal makine verilerini günlüğe alma gibi senaryolar için idealdir.
* **Sayfa blobları** deposu rastgele erişim boyutu 8 TB'a kadar dosyaları. Sayfa blobları Azure sanal makineler için diskler olarak sanal sabit disk (VHD) dosyaları işleme depolar. Sayfa BLOB'ları hakkında daha fazla bilgi için bkz: [genel bakış, Azure sayfa blobları](storage-blob-pageblob-overview.md)

BLOB'ları farklı türleri hakkında daha fazla bilgi için bkz. [anlama blok Blobları, ekleme Blobları ve sayfa Blobları](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs).

## <a name="move-data-to-blob-storage"></a>Blob depolamaya veri taşıma

Blob depolama alanına geçirme var olan veriler için birkaç çözüm mevcuttur:

- **AzCopy** Windows ve Linux kapsayıcılar arasında veya depolama hesaplarınız arasında verileri ve Blob depolama alanından kopyalar kullanımı kolay bir komut satırı aracı içindir. AzCopy hakkında daha fazla bilgi için bkz: [AzCopy v10 ile veri aktarma (Önizleme)](../common/storage-use-azcopy-v10.md). 
- **Azure depolama veri taşıma Kitaplığı** Azure depolama hizmetleri arasında verileri taşımak için bir .NET kitaplığı. AzCopy yardımcı programı ile veri taşıma kitaplığı yerleşik olarak bulunur. Daha fazla bilgi için [başvuru belgeleri](/dotnet/api/microsoft.azure.storage.datamovement) veri taşıma kitaplığı. 
- **Azure Data Factory** paylaşılan erişim imzası hesap anahtarını kullanarak verileri ve Blob depolama alanından kopyalama destekler hizmet sorumlusu veya yönetilen kimliklerini Azure kaynaklarında kimlik doğrulama için. Daha fazla bilgi için [veri kopyalama ya da Azure Blob depolamadan Azure Data Factory kullanarak](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage?toc=%2fazure%2fstorage%2fblobs%2ftoc.json). 
- **Blobfuse** bir Azure Blob Depolama için sanal dosya sistemi sürücüsüdür. Blobfuse mevcut blok blobu verileriniz depolama hesabınızda Linux dosya sistemi üzerinden erişmek için kullanabilirsiniz. Daha fazla bilgi için [bağlama Blob depolamaya bir dosya sistemi ile blobfuse olarak nasıl](storage-how-to-mount-container-linux.md).
- **Azure Data Box Disk** aktarma verileri Blob Depolama için büyük veri kümeleri veya ağ kısıtlamaları veri yüklemeyi kablo üzerinden kullanışsız değişiklik yaptığınızda şirket için bir hizmettir. Microsoft’tan katı hal sürücüleri (SSD’ler) istemek için [Azure Data Box Disk](../../databox/data-box-disk-overview.md)’i kullanabilirsiniz. Daha sonra verilerinizi bu disklere kopyalayabilir ve Blob depolama alanında yüklenmelerini sağlamak için Microsoft’a geri gönderebilirsiniz.
- **Azure içeri/dışarı aktarma hizmeti** sağladığınız ve sizin verilerinizi Microsoft ardından birlikte gelen sabit sürücüleri depolama hesabınızdan büyük miktarlarda veri aktarmak için bir yol sağlar. Daha fazla bilgi için [Blob depolama alanına veri aktarmak için Microsoft Azure içeri/dışarı aktarma hizmetini kullanmak](../common/storage-import-export-service.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama hesabı oluşturma](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Azure depolama ölçeklenebilirlik ve performans hedefleri](../common/storage-scalability-targets.md)
