---
title: Azure Blobları, Azure dosyaları veya Azure diskleri ne zaman kullanılacağını belirleme
description: Kullanılacak teknolojileri karar depolamak ve verilere Yardımı azure'da erişmek için farklı yollarını öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 11/28/2018
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 869d2105ccf635a46a21e9b7f382ddbef713d68b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61483426"
---
# <a name="deciding-when-to-use-azure-blobs-azure-files-or-azure-disks"></a>Azure Blobları, Azure dosyaları veya Azure diskleri ne zaman kullanılacağını belirleme
Microsoft Azure, verilerinizi bulutta erişmek ve depolamak için Azure Depolama'da birçok özellik sunar. Bu makalede, Azure dosyaları, Blobları ve diskleri kapsar ve bu özellikler arasında seçmenize yardımcı olmak için tasarlanmıştır.

## <a name="scenarios"></a>Senaryolar
Aşağıdaki tabloda, dosyaları, Blobları ve diskleri karşılaştırır ve her biri için uygun örnek senaryoları gösterir.

| Özellik | Açıklama | Kullanılması gereken durumlar |
|--------------|-------------|-------------|
| **Azure dosyaları** | İstemci kitaplıkları, bir SMB arabirim sağlar ve bir [REST arabirimi](/rest/api/storageservices/file-service-rest-api) her yerden erişim sağlayan depolanan dosyalar için. | "Kaldırma ve kaydırma" istediğiniz zaten ve Azure'da çalışan diğer uygulamalar arasında veri paylaşımı için yerel dosya sistemi API kullanan bulut için bir uygulama.<br/><br/>Geliştirme ve hata ayıklama çok sayıda sanal makinelerden erişilmesi gereken araçları depolamak istiyorsanız. |
| **Azure BLOB'ları** | İstemci kitaplıkları sağlar ve bir [REST arabirimi](/rest/api/storageservices/blob-service-rest-api) depolanabilir ve blok blobları, büyük bir ölçekte erişilen yapılandırılmamış verileri sağlar.<br/><br/>Ayrıca destekler [Azure Data Lake depolama Gen2](../blobs/data-lake-storage-introduction.md) Kurumsal büyük veri analizi çözümleri. | Rastgele erişim senaryoları ve akış'ı desteklemek üzere uygulama istediğiniz.<br/><br/>Uygulama verilerine her yerden erişebilir olmasını istediğiniz.<br/><br/>Bir kurumsal veri gölü, Azure üzerinde oluşturmayı ve büyük veri analizi gerçekleştirmek istiyorsunuz. |
| **Azure diskleri** | İstemci kitaplıkları sağlar ve bir [REST arabirimi](/rest/api/compute/manageddisks/disks/disks-rest-api) kalıcı olarak depolanır ve bir bağlı sanal sabit diskten erişilen verileri sağlar. | Lift- and -shift okumak ve kalıcı diske veri yazmak için yerel dosya sistemi API kullanan uygulamaları istiyorsunuz.<br/><br/>Disk bağlı olduğu sanal makinenin dışında erişilebilir için gerekli olmayan verileri depolamak istediğiniz. |

## <a name="comparison-files-and-blobs"></a>Karşılaştırma: Dosyalar ve Bloblar
Aşağıdaki tabloda, Azure BLOB'ları ile Azure dosyaları karşılaştırır.  
  
||||  
|-|-|-|  
|**Özniteliği**|**Azure BLOB'ları**|**Azure dosyaları**|  
|Dayanıklılık seçenekleri|LRS, ZRS, GRS, RA-GRS|LRS, ZRS, GRS|  
|Erişilebilirlik|REST API'leri|REST API'leri<br /><br /> SMB 2.1 ve SMB 3. 0'ı (standart dosya sistemi API'ları)|  
|Bağlantı|REST API'leri--dünya çapında|REST API'ler - dünya çapında<br /><br /> SMB 2.1--bölge içinde<br /><br /> SMB 3.0--dünya çapında|  
|Uç Noktalar|`http://myaccount.blob.core.windows.net/mycontainer/myblob`|`\\myaccount.file.core.windows.net\myshare\myfile.txt`<br /><br /> `http://myaccount.file.core.windows.net/myshare/myfile.txt`|  
|Dizinler|Düz ad alanı|Doğru dizin nesneleri|  
|Adları büyük/küçük harf duyarlılığı|Büyük/Küçük harfe duyarlı|/ Küçük harfe duyarlı çalışması ancak büyük/küçük harf koruma|  
|Kapasite|En fazla 2 PiB hesap sınırı |5 TiB dosya paylaşımları|  
|Aktarım hızı|Blok blobu başına en fazla 60 MiB/sn|Paylaşım başına en fazla 60 MiB/sn|  
|Nesne boyutu|Blok blobu başına yaklaşık 4,75 TiB kadar|Dosya başına en fazla 1 TiB|  
|Faturalandırılan kapasite|Yazılan bayt sayısı tabanlı|Dosya boyutuna göre|  
|İstemci kitaplıkları|Birden çok dil|Birden çok dil|  
  
## <a name="comparison-files-and-disks"></a>Karşılaştırma: Dosya ve diskleri
Azure dosyaları Azure diskleri tamamlar. Bir disk aynı anda yalnızca bir Azure sanal makinesine iliştirilebilir. Diskleri sayfa blobları Azure Depolama'da depolanan sabit biçimli VHD ve sanal makine tarafından kalıcı verileri depolamak için kullanılır. Dosya paylaşımlarını Azure dosyaları'nda yerel diske (bir yerel dosya sistemi API'ı kullanarak) erişilebilen olarak aynı şekilde erişilebilen ve çok sayıda sanal makineler arasında paylaşılabilir.  
 
Aşağıdaki tabloda, Azure dosyaları Azure diskleri ile karşılaştırır.  
 
||||  
|-|-|-|  
|**Özniteliği**|**Azure diskleri**|**Azure dosyaları**|  
|Kapsam|Tek bir sanal makine özel|Birden çok sanal makineye paylaşılan erişim|  
|Anlık görüntüler ve kopyalama|Evet|Evet|  
|Yapılandırma|Sanal makinenin başlangıçta bağlı|Sanal makine başlatıldıktan sonra bağlı|  
|Kimlik Doğrulaması|Yerleşik|NET kullanım ile ayarlama|  
|REST kullanarak erişimi|İçindeki VHD dosyaları erişilemiyor|Bir paylaşımda depolanan dosyalara erişilebilir|  
|En Yüksek Boyut|4 TiB disk|5 TiB dosya paylaşımını ve 1 TiB dosya paylaşımı içinde|  
|En fazla IOPS|500 IOPS|1000 IOPS|  
|Aktarım hızı|Disk başına en fazla 60 MiB/sn|(Daha yüksek g/ç boyutları için daha yüksek alabilirsiniz) dosya paylaşımı başına 60 MiB/sn hedefidir.|  

## <a name="next-steps"></a>Sonraki adımlar
Verilerinizin nasıl depolandığını ve erişilen ilişkin kararların, aynı zamanda maliyetleri dahil düşünmelisiniz. Daha fazla bilgi için [Azure depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).
  
SMB özelliklerinden bazıları bulut için geçerli değildir. Daha fazla bilgi için [Azure dosya hizmeti tarafından desteklenmeyen özellikler](/rest/api/storageservices/features-not-supported-by-the-azure-file-service).
  
Diskler hakkında daha fazla bilgi için bkz. bizim [yönetilen disklere giriş](../../virtual-machines/windows/managed-disks-overview.md) ve [bir Windows sanal makinesine veri diski ekleme](../../virtual-machines/windows/attach-managed-disk-portal.md).
