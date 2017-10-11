---
title: "Azure Storage Azure otomasyonu kullanarak yönetme"
description: "Nasıl Azure Otomasyon hizmetine ölçekte Azure depolamayı yönetmek için kullanılabilir hakkında bilgi edinin."
services: storage, automation
documentationcenter: 
author: jodoglevy
manager: eamono
editor: 
ms.assetid: bac41c39-1d95-46aa-a481-ec17bbb21b40
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: eamono
ms.openlocfilehash: 4649e42a628307e15f8b067503e4e8e13f16f1af
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="managing-azure-storage-using-azure-automation"></a>Azure Storage Azure otomasyonu kullanarak yönetme
Bu kılavuz Azure Otomasyon hizmetine ve nasıl bu Azure depolama BLOB'lar, tablolar ve Kuyruklar yönetimini kolaylaştırmak için kullanılabilir tanıtılacaktır.

## <a name="what-is-azure-automation"></a>Azure Otomasyonu Nedir?
[Azure Otomasyonu](https://azure.microsoft.com/services/automation/) işlem Otomasyonu aracılığıyla bulut yönetimi basitleştirmek için bir Azure hizmetidir. Azure otomasyonu kullanarak, uzun süre çalışan, el ile hatasız ve sık tekrarlanan görevleri, güvenilirlik ve verimliliği artırmak için otomatik ve kuruluşunuz için değerine süresini azaltabilir.

Azure Otomasyonu, kuruluşunuz büyüdükçe gereksinimlerinizi karşılayacak şekilde ölçekler bir yüksek oranda güvenilir ve yüksek oranda kullanılabilir iş akışı yürütme altyapısı sağlar. Tam olarak gerekli görevleri ortaya böylece Azure Otomasyonu'nda işlemleri el ile 3. taraf sistemleri tarafından veya zamanlanan aralıklarla artırılabilir devre dışı.

İşletimsel ek yükü azaltmak ve boşaltmak BT / iş ekler çalışmaları odaklanmaya DevOps personel değer Azure Automation'ın otomatik olarak çalıştırılmak üzere bulut yönetim görevlerinizi taşıyarak.

## <a name="how-can-azure-automation-help-manage-azure-storage"></a>Azure Otomasyonu Azure Storage yönetmenize nasıl yardımcı olabilir?
Azure depolama yönetilebilir Azure Otomasyonu'nda kullanılabilir PowerShell cmdlet'lerini kullanarak [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx). Böylece tüm blob, tablo ve hizmet içinde sıra yönetim görevleri gerçekleştirebilir azure Otomasyonu kutudan çıktığında, bu depolama PowerShell cmdlet'leri vardır. Ayrıca Azure Automation bu cmdlet'leri Azure hizmeti ve 3. taraf sistemi arasında karmaşık görevleri otomatikleştirmek için diğer Azure Hizmetleri için cmdlet'leri ile eşleştirin.

[Azure Otomasyonu runbook Galerisi](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) Azure Storage diğer Azure Hizmetleri ve 3. taraf sistemi yönetimi otomatikleştirme başlamak için ürün ekibi ve topluluk runbook'lar çeşitli içerir. Galeri runbook'ları şunları içerir:

* [Azure Storage Bloblarında'de, belirli gün eski kullanarak otomasyon hizmet Kaldır](https://gallery.technet.microsoft.com/scriptcenter/Remove-Storage-Blobs-that-aae4b761)
* [Azure depolama biriminden bir Blob indirin](https://gallery.technet.microsoft.com/scriptcenter/a-Blob-from-Azure-Storage-6bc13745)
* [Bir bulut hizmetindeki tüm sanal makineleri veya tek bir Azure VM için tüm diskleri yedekleme](https://gallery.technet.microsoft.com/scriptcenter/Backup-all-disks-for-a-ede940d5)

## <a name="next-steps"></a>Sonraki Adımlar
Azure Otomasyonu ve nasıl Azure Storage BLOB'lar, tablolar ve Kuyruklar yönetmek için kullanılmadan öğrendiğinize göre Azure Otomasyonu hakkında daha fazla bilgi için aşağıdaki bağlantıları izleyin.

Azure Otomasyonu öğretici bkz [oluşturma veya bir Azure Otomasyonu runbook'u içeri aktarma](../../automation/automation-creating-importing-runbook.md).

