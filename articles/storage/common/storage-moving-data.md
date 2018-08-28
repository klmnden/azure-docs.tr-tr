---
title: Büyük miktarda veriyi Azure bulut depolama içine/dışına taşıma | Microsoft Docs
description: Genel bakış için ve Azure Depolama'dan veri taşıma için farklı yöntemler.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 08/26/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: 1df237a65a8b5312b20de19a99399b3a3dd075ff
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43049619"
---
# <a name="moving-data-to-and-from-azure-storage"></a>Azure Storage’a ve Azure Storage’da veri taşıma
Şirket içi verilerinizi Azure Depolama'ya (veya tersi) taşımak istiyorsanız, bunu yapmak için yol çeşitli vardır. Sizin için en uygun yaklaşımı, senaryoya bağlıdır. Bu makalede farklı senaryolar ve her biri için uygun teklifleri hızlı bir genel bakış sağlar.

## <a name="building-applications"></a>Uygulamaları oluşturma
Bir uygulama oluşturuyorsanız, REST API veya bizim birçok istemci kitaplıklarından birini karşı geliştirmek için ve Azure Depolama'dan veri taşımak için harika bir yoludur.

Azure depolama, .NET, Java, Android, Go, Xamarin, C++, Node.JS, PHP, Ruby, Python ve iOS gibi birçok popüler diller için zengin istemci kitaplıkları sağlar. İstemci kitaplıkları yeniden deneme mantığı, günlüğe kaydetme ve paralel karşıya yüklemeler gibi gelişmiş özellikler sunar. HTTP/HTTPS istekleri yapan herhangi bir dil tarafından çağrılabilen REST API’sine karşı doğrudan da geliştirebilirsiniz.

Bkz: [Azure Blob Depolama ile çalışmaya başlama](../blobs/storage-dotnet-how-to-use-blobs.md) daha fazla bilgi için.

Ayrıca, ayrıca sunuyoruz [Azure Storage veri hareketi Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement) yüksek performans verileri azure'a veya azure'dan kopyalanması için tasarlanmış bir kitaplık olan. Lütfen bizim veri taşıma Kitaplığı'na başvurun [belgeleri](https://github.com/Azure/azure-storage-net-data-movement) daha fazla bilgi için. 

## <a name="quickly-viewinginteracting-with-your-data"></a>Verilerinizi hızla görüntüleme/verilerinizle etkileşim kurma
Karşıya yükleme ve verilerinizi indirme olanağı da yaparken, Azure depolama veri görüntülemek için kolay bir yol istiyorsanız, bir Azure Depolama Gezgini'ni kullanarak düşünün.

Listemize denetleyin [Azure depolama gezginleri](../storage-explorers.md) daha fazla bilgi için.

## <a name="system-administration"></a>Sistem Yönetimi
Gerekli veya bir komut satırı yardımcı programı ile (örneğin, sistem yöneticileri) daha deneyimliyseniz, dikkate alınması gereken bazı seçenekler şunlardır:

### <a name="azcopy"></a>AzCopy
AzCopy, yüksek performans verilerini ve Azure Depolama'dan kopyalamak için tasarlanan bir komut satırı yardımcı programıdır. Ayrıca, bir depolama hesabının içinde veya farklı depolama hesapları arasında verileri kopyalayabilirsiniz. AzCopy, üzerinde kullanılabilir [Windows](storage-use-azcopy.md) ve [Linux](storage-use-azcopy-linux.md).

Bkz: [AzCopy komut satırı yardımcı programı ile veri aktarma](storage-use-azcopy.md) veya [Linux üzerinde AzCopy ile veri aktarımı](storage-use-azcopy-linux.md) daha fazla bilgi için.

### <a name="azure-powershell"></a>Azure PowerShell
Azure PowerShell, Azure hizmetlerini yönetmek için cmdlet’ler sağlayan bir modüldür. Sistem yönetimi için özel olarak tasarlanan görev tabanlı bir komut satırı kabuğu ve betik dili olarak tanımlanır.

Bkz: [Azure PowerShell kullanarak Azure depolama ile](storage-powershell-guide-full.md) daha fazla bilgi için.

### <a name="azure-cli"></a>Azure CLI
Azure CLI, bir dizi açık kaynaklı, Azure hizmetleriyle çalışmak için platformlar arası komut sunar. Windows, OSX ve Linux'ta Azure CLI kullanılabilir.

Bkz: [Azure depolama ile Azure CLI kullanarak](../storage-azure-cli.md) daha fazla bilgi için.

## <a name="moving-large-amounts-of-data-with-a-slow-network"></a>Büyük miktarlarda yavaş bir ağ ile veri taşıma
Büyük miktarlarda veri taşıma ile ilişkili en büyük zorluklardan biri aktarımı zamandır. Ardından Azure içeri/dışarı aktarma ağ maliyetleri hakkında endişelenmeden veya kod yazma, verileri Azure depolama içine/dışına almak istiyorsanız, uygun bir çözümdür.

Bkz: [Azure içeri/dışarı aktarma](../storage-import-export-service.md) daha fazla bilgi için.

## <a name="backing-up-your-data"></a>Verilerinizi yedekleme
Verilerinizi Azure Depolama'ya yedeklemek istiyorsanız, Azure Backup Git yoludur. Bu, şirket içi veri ve Azure sanal makinelerini yedeklemek için güçlü bir çözümdür.

Bkz: [Azure Backup](../../backup/backup-introduction-to-azure-backup.md) daha fazla bilgi için.

## <a name="accessing-your-data-on-premises-and-from-the-cloud"></a>Verileri şirket içi erişme ve buluttan
Ardından, verileri şirket içi Exchange'e erişimini ve buluttan bir çözüm gerekiyorsa, Azure'nın hibrit bulut depolaması çözümü, StorSimple kullanmayı düşünmeniz gerekir. Bu çözüm, fiziksel StorSimple cihazı akıllıca depoları sık veri SSD'ler üzerinde kullanılan, HDD'ler ve etkin olmayan/yedekleme/arşiv verileri Azure depolama üzerinde verileri bazen kullanılan, oluşur.

Bkz: [StorSimple](../../storsimple/storsimple-overview.md) daha fazla bilgi için.

## <a name="recovering-your-data"></a>Veri Kurtarma
Şirket içi iş yükleri ve uygulamalar varsa, işletmenizi olağanüstü bir çalıştırmaya devam etmesine izin veren bir çözüm gerekir. Çoğaltma, yük devretme ve kurtarma sanal makinelerin ve fiziksel sunucuları Azure Site Recovery işler. Çoğaltılan veriler, ikincil bir veri merkezlerinde gerekmemesi olanak tanıyan Azure Depolama'da depolanır.

Bkz: [Azure Site Recovery](../../site-recovery/site-recovery-overview.md) daha fazla bilgi için.
### <a name="moving-data-faq"></a>SSS veri taşıma:
## <a name="can-i-migrate-vhds-from-one-region-to-another-without-copying"></a>VHD bir bölgeden diğerine kopyalama olmadan geçişini sağlayabilir miyim?
Bölge arasında VHD'lerin kopyalanacağı tek her bir bölgeye depolama hesapları arasında veri kopyalamak için yoludur. Bunun için AZCopy kullanabilirsiniz. AzCopy komut satırı daha fazla bilgi için yardımcı programı ile veri aktarımı bakın. Çok büyük miktarlarda veri için Azure içeri/dışarı aktarma da kullanabilirsiniz. Bkz: [Azure içeri/dışarı aktarma](https://docs.microsoft.com/azure/storage/storage-import-export-service) daha fazla bilgi için.
