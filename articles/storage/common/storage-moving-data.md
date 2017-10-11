---
title: "Büyük miktarlarda veri taşıma/Azure içinde bulut depolama biriminden | Microsoft Docs"
description: "Azure Storage gelen ve verilerin taşınması için farklı yöntemler genel bakış."
services: storage
documentationcenter: 
author: JarrettRenshaw
manager: msmets
editor: tysonn
ms.assetid: 5e3947a9-d99b-4108-9d57-3eb67c03e7ba
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: jarrettr
ms.openlocfilehash: ba390a5973ad33405f1d4217d60d7989f04db3b4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="moving-data-to-and-from-azure-storage"></a>Azure Storage’a ve Azure Storage’da veri taşıma
Şirket içi verileri Azure Storage (veya tersi) taşımak istiyorsanız, çeşitli yollarla Bunu yapmak için vardır. En uygun bir yaklaşım, senaryoya bağlıdır. Bu makalede farklı senaryolar ve her biri için uygun teklifleri hızlı bir genel bakış sağlar.

## <a name="building-applications"></a>Uygulamaları oluşturma
Bir uygulama oluşturuyorsanız, REST API veya bizim birçok istemci kitaplıkları karşı geliştirmek için ve Azure Storage veri taşımak için harika bir yoludur.

Azure depolama .NET, iOS, Java, Android, Evrensel Windows Platformu (UWP), Xamarin, C++, Node.JS, PHP, Ruby ve Python için zengin istemci kitaplıkları sağlar. İstemci kitaplıkları yeniden deneme mantığı, günlüğe kaydetme ve paralel karşıya yüklemeler gibi gelişmiş özellikler sunar. HTTP/HTTPS istekleri yapan herhangi bir dil tarafından çağrılabilen REST API’sine karşı doğrudan da geliştirebilirsiniz.

Bkz: [Azure Blob Storage ile çalışmaya başlama](../blobs/storage-dotnet-how-to-use-blobs.md) daha fazla bilgi için.

Ayrıca, aynı zamanda sunuyoruz [Azure Storage veri hareketi Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement) yüksek performans verilerini ve azure'dan kopyalama için tasarlanmış bir kitaplık değil. Lütfen bizim veri hareketi Kitaplığı'na başvurun [belgelerine](https://github.com/Azure/azure-storage-net-data-movement) daha fazla bilgi için. 

## <a name="quickly-viewinginteracting-with-your-data"></a>Verilerinizi hızla görüntüleme/verilerinizle etkileşim kurma
Ardından, karşıya yükleme ve verilerinizi indirme olanağı da yaparken Azure Storage verilerinizin görüntülemek için kolay bir yol istiyorsanız, bir Azure Storage Gezgini kullanmayı düşünün.

Listemiz denetleyin [Azure depolama gezginleri](../storage-explorers.md) daha fazla bilgi için.

## <a name="system-administration"></a>Sistem Yönetimi
Gerektiren veya bir komut satırı yardımcı programı ile (örneğin, sistem yöneticileri) daha rahat, dikkate almanız gereken bazı seçenekler şunlardır:

### <a name="azcopy"></a>AzCopy
AzCopy, verilerin Azure Storage’a ve Azure Storage’dan yüksek performansla kopyalanması için tasarlanmış bir Windows komut satırı yardımcı programıdır. Ayrıca, verileri bir depolama hesabı içinde veya farklı depolama hesapları arasında kopyalayabilirsiniz.

Bkz: [AzCopy komut satırı yardımcı programı ile veri aktarma](storage-use-azcopy.md) daha fazla bilgi için.

### <a name="azure-powershell"></a>Azure PowerShell
Azure PowerShell, Azure hizmetlerini yönetmek için cmdlet’ler sağlayan bir modüldür. Sistem yönetimi için özel olarak tasarlanan görev tabanlı bir komut satırı kabuğu ve betik dili olarak tanımlanır.

Bkz: [Azure Storage ile Azure PowerShell'i kullanma](storage-powershell-guide-full.md) daha fazla bilgi için.

### <a name="azure-cli"></a>Azure CLI
Azure CLI, Azure Hizmetleri ile çalışmak için platformlar arası komutları açık kaynak kümesi sağlar. Azure CLI, Windows, OSX ve Linux üzerinde kullanılabilir.

Bkz: [Azure Storage ile Azure CLI kullanarak](../storage-azure-cli.md) daha fazla bilgi için.

## <a name="moving-large-amounts-of-data-with-a-slow-network"></a>Büyük miktarlarda verinin yavaş bir ağ ile taşıma
Büyük miktarlarda veri taşıma ile ilişkili en büyük zorluklardan biri aktarımı saattir. Ardından Azure içeri/dışarı aktarma hakkında ağları maliyetleri kaygı veya kod yazmadan denetleyicisinden Azure Storage veri almak istiyorsanız, uygun bir çözümdür.

Bkz: [Azure içeri/dışarı aktarma](../storage-import-export-service.md) daha fazla bilgi için.

## <a name="backing-up-your-data"></a>Verilerinizi yedekleme
Verilerinizi Azure Depolama'ya yedekleme gerekiyorsa, Azure Backup Git yoludur. Bu, şirket içi veri ve Azure sanal makineleri yedeklemek için güçlü bir çözümdür.

Bkz: [Azure Backup](../../backup/backup-introduction-to-azure-backup.md) daha fazla bilgi için.

## <a name="accessing-your-data-on-premises-and-from-the-cloud"></a>Verileri şirket içi erişme ve bulutta
Verileri şirket içi erişmek ve bulutta bir çözüm gerekiyorsa, Azure'nın karma bulut depolama çözümü, StorSimple kullanmayı düşünmeniz gerekir. Bu çözüm fiziksel StorSimple cihazı akıllıca sık depoları verileri SSD üzerinde kullanılan, bazen HDD ve etkin olmayan/yedekleme/arşiv verileri Azure depolama birimindeki verileri kullanılan olduğunu oluşur.

Bkz: [StorSimple](../../storsimple/storsimple-overview.md) daha fazla bilgi için.

## <a name="recovering-your-data"></a>Veri Kurtarma
Şirket içi iş yüklerini ve uygulamaları varsa, işinizin olağanüstü bir durumda olması durumunda çalışmaya devam etmesini sağlayan bir çözüme ihtiyacınız vardır. Azure Site Recovery, çoğaltma, yük devretme ve sanal makinelerin ve fiziksel sunucuları kurtarma işler. Çoğaltılan veriler Azure depolama, yerinde ikincil veri merkezine gereksinimini ortadan kaldırmak sağlayarak depolanır.

Bkz: [Azure Site Recovery](../../site-recovery/site-recovery-overview.md) daha fazla bilgi için.
### <a name="moving-data-faq"></a>Veri SSS taşıma:
## <a name="can-i-migrate-vhds-from-one-region-to-another-without-copying"></a>VHD bir bölgesinden diğerine kopyalamadan geçişini sağlayabilir miyim?
VHD'ler bölge arasında kopyalamak için yalnızca her bölge depolama hesapları arasında veri kopyalamak için yoludur. AZCopy için bunu kullanabilirsiniz. AzCopy komut satırı daha fazla bilgi için yardımcı programı ile veri aktarımı bakın. İçin çok büyük miktarlarda verinin Azure içeri/dışarı aktarma de kullanabilirsiniz. Bkz: [Azure içeri/dışarı aktarma](https://docs.microsoft.com/en-us/azure/storage/storage-import-export-service) daha fazla bilgi için.
