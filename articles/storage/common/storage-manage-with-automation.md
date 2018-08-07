---
title: Azure depolama, Azure Otomasyonu ile yönetme
description: Nasıl Azure Otomasyonu hizmetini Azure Depolama'ya ölçekli olarak yönetmek için kullanılabilir hakkında bilgi edinin.
services: storage, automation
author: jodoglevy
ms.service: storage
ms.topic: article
ms.date: 05/23/2016
ms.author: eamono
ms.component: common
ms.openlocfilehash: 82ec929c8d3055187a83179432fc601baa191cc4
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39526377"
---
# <a name="managing-azure-storage-using-azure-automation"></a>Azure depolama, Azure Otomasyonu ile yönetme
Bu kılavuzda Azure Otomasyonu ve nasıl Azure depolama blobları, tablolar ve Kuyruklar yönetimini basitleştirmek için kullanılabilmesi için kullanıma sunacak.

## <a name="what-is-azure-automation"></a>Azure Otomasyonu Nedir?
[Azure Otomasyonu](https://azure.microsoft.com/services/automation/) bulut yönetimini işlem Otomasyonu ile basitleştirmeyi için bir Azure hizmetidir. Azure otomasyonu kullanarak, uzun süre çalışan, el ile hata yapmaya açık ve sık sık tekrarlanan görevleri, güvenilirliği ve verimliliği artırmak için otomatik ve kuruluşunuz için değer süresini azaltabilir.

Azure Otomasyonu, kuruluşunuz büyüdükçe, gereksinimlerinizi karşılayacak şekilde ölçeklendirilen bir yüksek oranda güvenilir ve yüksek oranda kullanılabilir iş akışı yürütme altyapısı sağlar. Görevleri tam olarak gerekli olduğunda gerçekleşir. böylece Azure Automation'da işlemleri el ile 3. taraf sistemleri tarafından veya zamanlanan aralıklarla çalıştırmasının.

Alt işlemsel yük ve boş BT / iş kazandıran işe odaklanmak için DevOps personeli, Azure Otomasyonu tarafından otomatik olarak çalıştırılmak için bulut Yönetimi görevlerinizi taşıyarak değer.

## <a name="how-can-azure-automation-help-manage-azure-storage"></a>Azure Otomasyonu Azure depolama yönetmenize nasıl yardımcı olabilir?
Azure depolama yönetilen Azure Automation'da kullanılabilen PowerShell cmdlet'lerini kullanarak [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx). Azure Otomasyonu bu depolama PowerShell cmdlet'leri kullanılabilir yepyeni sahiptir; böylece tüm blob, tablo ve kuyruk hizmetinde yönetim görevlerini gerçekleştirebilirsiniz. Ayrıca, Azure automation'da bu cmdlet'ler Azure Hizmetleri ve 3. taraf sistemleri arasında karmaşık görevleri otomatikleştirmek için diğer Azure Hizmetleri için cmdlet'leri eşleşebileceğini denetleyebilmesi.

[Azure Otomasyonu runbook Galerisi](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) Azure depolama, diğer Azure Hizmetleri ve 3. taraf sistemleri yönetimini otomatik hale getirmeye başlamanın ürün ekibi ve topluluk runbook'lar çeşitli içerir. Galerideki runbook'ları şunları içerir:

* [Belirli gün eski kullanarak otomasyon hizmeti olan Azure depolama Blobları Kaldır](https://gallery.technet.microsoft.com/scriptcenter/Remove-Storage-Blobs-that-aae4b761)
* [Azure depolama Blob indirme](https://gallery.technet.microsoft.com/scriptcenter/a-Blob-from-Azure-Storage-6bc13745)
* [Tüm diskler için tek bir Azure VM veya Bulut hizmetindeki tüm sanal makineler için yedekleme](https://gallery.technet.microsoft.com/scriptcenter/Backup-all-disks-for-a-ede940d5)

## <a name="next-steps"></a>Sonraki Adımlar
Azure Otomasyonu ve nasıl Azure depolama blobları, tablolar ve Kuyruklar'ı yönetmek için kullanılmadan ilişkin temel bilgileri öğrendiğinize göre Azure Otomasyonu hakkında daha fazla bilgi edinmek için bu bağlantıları izleyin.

Azure Otomasyonu bkz [oluşturma veya içeri aktarma Azure automation'da bir runbook](../../automation/automation-creating-importing-runbook.md).

