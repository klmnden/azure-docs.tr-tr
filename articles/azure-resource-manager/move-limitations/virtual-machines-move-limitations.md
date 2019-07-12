---
title: Azure sanal makineler için yeni abonelik veya kaynak grubu taşıma | Microsoft Docs
description: Azure Resource Manager sanal makineleri bir yeni kaynak grubuna veya aboneliğe taşıma kullanın.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: tomfitz
ms.openlocfilehash: 095ed1c8d2328b1eb391042125526696ba8cda49
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723551"
---
# <a name="move-guidance-for-virtual-machines"></a>Sanal makineler için Taşıma Kılavuzu

Bu makalede, şu anda desteklenmeyen senaryolar ve yedekleme ile sanal makineleri taşımak için gereken adımlar açıklanmaktadır.

## <a name="scenarios-not-supported"></a>Desteklenmeyen senaryolar

Henüz, aşağıdaki senaryolar desteklenmez:

* Kullanılabilirlik alanına yönetilen diskler, farklı bir aboneliğe taşınamaz.
* Key Vault'ta depolanan bir sertifika ile sanal makineler için yeni bir kaynak grubu ile aynı abonelikte ancak değil, abonelikler arasında taşınabilir.
* Standart SKU yük Dengeleyicide veya standart SKU genel IP ile sanal makine ölçek kümeleri taşınamaz.
* Market kaynaklardan bağlı planlar ile oluşturulan sanal makineler, kaynak grubu veya abonelik arasında taşınamaz. Geçerli Abonelikteki sanal makine sağlamasını kaldırma ve yeni aboneliği yeniden dağıtın.
* Sanal makineler'de var olan bir sanal ağ, ancak tüm kaynaklar sanal ağ içinde taşıma değildir.

## <a name="virtual-machines-with-azure-backup"></a>Azure Backup ile sanal makineler

Azure Backup ile yapılandırılmış sanal makineleri taşımak için aşağıdaki geçici çözümü kullanın:

* Sanal makinenizin konumu bulun.
* Aşağıdaki adlandırma deseni ile bir kaynak grup bulunamıyor: `AzureBackupRG_<location of your VM>_1` örneğin AzureBackupRG_westus2_1
* Azure portalında, ardından onay "gizli türleri Göster ise"
* PowerShell'de varsa, kullanmak `Get-AzResource -ResourceGroupName AzureBackupRG_<location of your VM>_1` cmdlet'i
* CLI, kullanın `az resource list -g AzureBackupRG_<location of your VM>_1`
* Kaynak türü bulma `Microsoft.Compute/restorePointCollections` adlandırma deseni sahip `AzureBackup_<name of your VM that you're trying to move>_###########`
* Bu kaynağı silme. Bu işlem, yedeklenen verileri kasadaki değil yalnızca anında kurtarma noktalarını siler.
* Silme işlemi tamamlandıktan sonra hedef aboneliği için sanal makine ve kasa taşıyabilirsiniz. Taşıma sonrası veri kaybı olmadan yedeklemelerde devam edebilirsiniz.
* Taşıma kurtarma hizmeti kasası yedekleme hakkında daha fazla bilgi için bkz. [kurtarma Hizmetleri sınırlamalarını](../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json).

## <a name="next-steps"></a>Sonraki adımlar

Kaynakları taşıma komutlar için bkz [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../resource-group-move-resources.md).
