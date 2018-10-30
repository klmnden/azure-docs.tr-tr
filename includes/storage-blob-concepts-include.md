---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/17/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 7889fbc9373cbdfdfab891bf8b1cd610523c7032
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50088100"
---
Azure Blob depolama, Microsoft’un buluta yönelik nesne depolama çözümüdür. Blob depolama, metin veya ikili veri gibi çok miktarda yapılandırılmamış veriyi depolamak için iyileştirilmiştir.

Blob depolama aşağıdaki durumlar için idealdir:

* Görüntülerin veya belgelerin doğrudan bir tarayıcıya sunulması.
* Dağıtılan erişim için dosyaların depolanması.
* Video ve ses akışları.
* Günlük dosyalarına yazma.
* Yedekleme ve geri yükleme, olağanüstü durum kurtarma ve arşivleme için verilerin depolanması.
* Şirket içi veya Azure’da barındırılan bir hizmetle analiz için verilerin depolanması.

Kullanıcılar veya istemci uygulamalar, bloblara HTTP veya HTTPS, URL’ler, &mdash;Azure Depolama REST API’si&mdash;, [Azure PowerShell](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api), [Azure CLI](https://docs.microsoft.com/powershell/module/azure.storage) veya bir Azure Depolama istemci kitaplığı aracılığıyla [dünyanın her yerinden](https://docs.microsoft.com/cli/azure/storage) erişebilir. Depolama istemci kitaplıkları [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage/client), [Java](https://docs.microsoft.com/java/api/overview/azure/storage/client), [Node.js](http://azure.github.io/azure-storage-node), [Python](https://docs.microsoft.com/python/azure/), [PHP](http://azure.github.io/azure-storage-php/) ve [Ruby](http://azure.github.io/azure-storage-ruby) dahil olmak üzere çeşitli dillerle kullanılabilir.

## <a name="blob-service-concepts"></a>Blob hizmeti kavramları

Blob depolama üç kaynağı kullanır: depolama hesabınız, hesaptaki kapsayıcılar ve kapsayıcı içindeki bloblar. Aşağıdaki diyagramda bu kaynaklar arasındaki ilişki gösterilmektedir.

![Blob (nesne) depolama mimarisi diyagramı](./media/storage-blob-concepts-include/blob1.png)

### <a name="storage-account"></a>Depolama hesabı

Azure Depolama içindeki veri nesnelerine erişim bir depolama hesabıyla sağlanır. Daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../articles/storage/common/storage-account-overview.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="container"></a>Kapsayıcı

Kapsayıcı, dosya sistemindeki klasörlere benzer ve bir blob kümesini düzenler. Tüm bloblar bir kapsayıcı içinde bulunur. Depolama hesabında sınırsız sayıda kapsayıcı olabilir ve her kapsayıcı sınırsız sayıda blob depolayabilir. 

  > [!NOTE]
  > Kapsayıcı adındaki harfler küçük harf olmalıdır.

### <a name="blob"></a>Blob
 
Azure Depolama üç tür blob sunar: &mdash;blok blobları, ekleme blobları ve [sayfa blobları](../articles/storage/blobs/storage-blob-pageblob-overview.md) (VHD dosyaları için kullanılır).

* Blok blobları en fazla 4,7 TB olmak üzere metin ve ikili veri depolar. Blok blobları, ayrı ayrı yönetilebilen veri bloklarından oluşur.
* Ekleme blobları blok bloblarına benzer bloklardan oluşur ancak ekleme işlemleri için en iyi duruma getirilmiştir. Ekleme blobları sanal makine verilerini günlüğe alma gibi senaryolar için idealdir.
* Sayfa blobları, 8 TB’a kadar boyutta rastgele erişimli dosyaları depolar. Sayfa blobları, VM'leri içeren VHD dosyalarını depolar.

Tüm bloblar bir kapsayıcı içinde bulunur. Kapsayıcı, dosya sistemindeki klasöre benzer. Blobları sanal dizinler halinde de düzenleyebilir ve dosya sisteminde olduğu gibi farklı dizinlerdekilerde gezinti yapabilirsiniz. 

Büyük veri kümelerinin ve ağ kısıtlamalarının kablo üzerinden Blob depolama alanına veri yükleme işlemini kullanışsız hale getirdiği durumlar olabilir. Microsoft’tan katı hal sürücüleri (SSD’ler) istemek için [Azure Data Box Disk](../articles/databox/data-box-disk-overview.md)’i kullanabilirsiniz. Daha sonra verilerinizi bu disklere kopyalayabilir ve Blob depolama alanında yüklenmelerini sağlamak için Microsoft’a geri gönderebilirsiniz.

Depolama hesabınızdan büyük miktarlarda veriyi dışarı aktarmanız gerekiyorsa bkz. [Blob depolama alanına veri aktarmak için Microsoft Azure İçeri/Dışarı Aktarma hizmetini kullanma](../articles/storage/common/storage-import-export-service.md).
  
Kapsayıcıları ve blobları adlandırma hakkında ayrıntılı bilgi için bkz. [Kapsayıcıları, blobları ve meta verileri adlandırma ve bunlara başvurma](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).
