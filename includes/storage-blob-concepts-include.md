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
ms.openlocfilehash: 63593ff5f02f5e37fc25c988c4cef071a03a00b4
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37066042"
---
Azure Blob depolama, Microsoft’un buluta yönelik nesne depolama çözümüdür. Blob depolama, metin veya ikili veri gibi çok miktarda yapılandırılmamış veriyi depolamak için iyileştirilmiştir.

Blob depolama aşağıdaki durumlar için idealdir:

* Görüntülerin veya belgelerin doğrudan bir tarayıcıya sunulması.
* Dağıtılan erişim için dosyaların depolanması.
* Video ve ses akışları.
* Günlük dosyalarına yazma.
* Yedekleme ve geri yükleme, olağanüstü durum kurtarma ve arşivleme için verilerin depolanması.
* Şirket içi veya Azure’da barındırılan bir hizmetle analiz için verilerin depolanması.

Blob depolamadaki nesnelere, dünyanın her yerinden HTTP veya HTTPS üzerinden erişilebilir. Kullanıcılar veya istemci uygulamalar, bloblara URL’ler, [Azure Depolama REST API’si](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api), [Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage), [Azure CLI](https://docs.microsoft.com/cli/azure/storage) veya bir Azure Depolama istemci kitaplığı aracılığıyla erişebilir. Depolama istemci kitaplıkları [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage/client), [Java](https://docs.microsoft.com/java/api/overview/azure/storage/client), [Node.js](http://azure.github.io/azure-storage-node), [Python](https://docs.microsoft.com/python/azure/), [PHP](http://azure.github.io/azure-storage-php/) ve [Ruby](http://azure.github.io/azure-storage-ruby) dahil olmak üzere birden çok dil ile kullanılabilir.

## <a name="blob-service-concepts"></a>Blob hizmeti kavramları

Blob depolama üç kaynağı kullanır: depolama hesabınız, hesaptaki kapsayıcılar ve kapsayıcı içindeki bloblar. Aşağıdaki diyagramda bu kaynaklar arasındaki ilişki gösterilmektedir.

![Blob (nesne) depolama mimarisi diyagramı](./media/storage-blob-concepts-include/blob1.png)

### <a name="storage-account"></a>Depolama Hesabı

Azure Depolama içindeki veri nesnelerine erişim bir depolama hesabıyla sağlanır. Daha fazla bilgi için bkz. [Azure depolama hesapları hakkında](../articles/storage/common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="container"></a>Kapsayıcı

Kapsayıcı, dosya sistemindeki klasörlere benzer ve bir blob kümesini düzenler. Tüm bloblar bir kapsayıcı içinde bulunur. Depolama hesabında sınırsız sayıda kapsayıcı olabilir ve her kapsayıcı sınırsız sayıda blob depolayabilir. Kapsayıcı adındaki harflerin küçük harf olması gerektiğini unutmayın.

### <a name="blob"></a>Blob
 
Azure Depolama üç tür blob sunar: blok blobları, ekleme blobları ve [sayfa blobları](../articles/storage/blobs/storage-blob-pageblob-overview.md) (VHD dosyaları için kullanılır).

* Blok blobları en fazla 4,7 TB olmak üzere metin ve ikili veri depolar. Blok blobları, ayrı ayrı yönetilebilen veri bloklarından oluşur.
* Ekleme blobları blok bloblarına benzer bloklardan oluşur ancak ekleme işlemleri için en iyi duruma getirilmiştir. Ekleme blobları sanal makine verilerini günlüğe alma gibi senaryolar için idealdir.
* Sayfa blobları, 8 TB’a kadar boyutta rastgele erişimli dosyaları depolar. Sayfa blobları, VM'leri içeren VHD dosyalarını depolar.

Tüm bloblar bir kapsayıcı içinde bulunur. Kapsayıcı, dosya sistemindeki klasöre benzer. Blobları sanal dizinler halinde de düzenleyebilir ve dosya sisteminde olduğu gibi farklı dizinlerdekileri birlikte kullanabilirsiniz. 

Ağ kısıtlamalarının kablo üzerinden Blob depolamaya veri yükleme veya indirme yapmayı kullanışsız hale getirdiği çok büyük veri kümelerinde verileri doğrudan veri merkezinden içeri veya dışarı aktarmak için Microsoft’a sabit sürücüler gönderebilirsiniz. Daha fazla bilgi için bkz. [Blob Depolama'ya Veri Aktarmak için Microsoft Azure İçeri/Dışarı Aktarma Hizmetini Kullanma](../articles/storage/common/storage-import-export-service.md).
  
Kapsayıcıları ve blobları adlandırma hakkında ayrıntılı bilgi için bkz. [Kapsayıcıları, Blobları ve Meta Verileri Adlandırma ve Bunlara Başvurma](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).
