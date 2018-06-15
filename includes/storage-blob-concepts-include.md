---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 04/09/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 203f5a766c4c8a8f1e577f6be1e18d0f9ac95403
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31432781"
---
Azure Blob storage Microsoft'un nesne depolama için bulut çözümüdür. BLOB Depolama oldukça büyük miktardaki metin veya ikili veriler gibi yapılandırılmamış verileri depolamak için en iyi duruma getirilmiştir.

BLOB Depolama için idealdir:

* Görüntülerin veya belgelerin doğrudan bir tarayıcıya hizmet.
* Dağıtılan erişim için dosyaların depolanması.
* Video ve ses akışları.
* Günlük dosyalarına yazma.
* Yedekleme ve geri yükleme, olağanüstü durum kurtarma ve arşivleme için verilerin depolanması.
* Bir şirket içi tarafından analiz için verilerin depolanması veya Azure barındırılan hizmet.

Blob depolama alanındaki nesnelerin herhangi bir yere HTTP veya HTTPS aracılığıyla dünyanın erişilebilir. Kullanıcılar ya da istemci uygulamalar blobları URL'ler aracılığıyla erişebilir [Azure Storage REST API'sini](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api), [Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage), [Azure CLI](https://docs.microsoft.com/cli/azure/storage), veya bir Azure Storage istemci kitaplığı. Depolama istemcisi kitaplıklarını dahil olmak üzere birden çok dil için kullanılabilir olan [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage/client), [Java](https://docs.microsoft.com/java/api/overview/azure/storage/client), [Node.js](http://azure.github.io/azure-storage-node), [Python](https://azure-storage.readthedocs.io/en/latest/index.html), [PHP](http://azure.github.io/azure-storage-php/), ve [Ruby](http://azure.github.io/azure-storage-ruby).

## <a name="blob-service-concepts"></a>Blob hizmeti kavramları

BLOB storage üç kaynakları gösterir: depolama hesabınız, hesaptaki kapsayıcılar ve bloblar bir kapsayıcıda. Aşağıdaki diyagramda, bu kaynakları arasındaki ilişkiyi gösterir.

![Blob (nesne) depolama mimarisi diyagramı](./media/storage-blob-concepts-include/blob1.png)

### <a name="storage-account"></a>Depolama Hesabı

Azure Storage veri nesneleri tüm erişimi bir depolama hesabıyla yapılır. Daha fazla bilgi için bkz: [Azure storage hesapları hakkında](../articles/storage/common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="container"></a>Kapsayıcı

Bir kapsayıcı, BLOB'ları, bir klasöre dosya sistemindeki benzer birtakım düzenler. Tüm bloblar bir kapsayıcıda yer alır. Bir depolama hesabı sınırsız sayıda kapsayıcı içerebilir ve bir kapsayıcıda sınırsız sayıda BLOB depolayabilirsiniz. Kapsayıcı adındaki harflerin küçük harf olması gerektiğini unutmayın.

### <a name="blob"></a>Blob
 
Azure Storage sunar üç türde BLOB--blok blobları, ekleme blobları ve [sayfa blobları](../articles/storage/blobs/storage-blob-pageblob-overview.md) (VHD dosyaları için kullanılan).

* Blok blobları, metin ve yaklaşık 4.7 TB kadar ikili veri depolayın. Blok blobları, ayrı ayrı yönetilen veri bloklarını yapılır.
* Ekleme blobları yapılma bloklarını blok blobları gibi çalışır, ancak için iyileştirilmiş ekleme işlemleri. Ekleme blobları sanal makinelerden verileri günlüğe kaydetmeye gibi senaryolar için idealdir.
* Sayfa BLOB deposu rasgele erişim boyutu 8 TB'ye kadar dosyaları. Sayfa bloblarını VM'ler geri VHD dosyalarını depolar.

Tüm bloblar bir kapsayıcıda yer alır. Bir kapsayıcı bir dosya sisteminde bir klasöre benzer. Daha fazla sanal dizinlere BLOB'lar düzenlemek ve bir dosya sistemi gibi bunları çapraz geçiş. 

Ağ kısıtlamalarının kablo üzerinden Blob depolamaya veri yükleme veya indirme yapmayı kullanışsız hale getirdiği çok büyük veri kümelerinde verileri doğrudan veri merkezinden içeri veya dışarı aktarmak için Microsoft’a sabit sürücüler gönderebilirsiniz. Daha fazla bilgi için bkz: [Blob depolama alanına veri aktarmak için Microsoft Azure içeri/dışarı aktarma hizmeti kullanma](../articles/storage/common/storage-import-export-service.md).
  
Kapsayıcıları ve blobları adlandırma hakkında ayrıntılı bilgi için bkz. [Kapsayıcıları, Blobları ve Meta Verileri Adlandırma ve Bunlara Başvurma](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).
