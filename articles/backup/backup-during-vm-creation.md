---
title: Azure Backup ile bir Azure VM oluşturduğunuzda, yedeklemeyi etkinleştirme
description: Azure Backup ile bir Azure VM oluşturduğunuzda, yedeklemeyi etkinleştirmek açıklar.
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 06/13/2019
ms.author: raynew
ms.openlocfilehash: a19653f7ae3900fd7999f347ef4d3ef710be1430
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67436347"
---
# <a name="enable-backup-when-you-create-an-azure-vm"></a>Azure VM’sini oluşturduğunuz sırada yedeklemeyi etkinleştirme

Azure Backup hizmeti, Azure sanal makinelerini (VM) yedekleme için kullanın. Vm'leri yedekleme ilkesinde belirtilen bir zamanlamaya göre yedeklenir ve yedeklerinden kurtarma noktaları oluşturulur. Kurtarma noktaları, Kurtarma Hizmetleri kasalarında depolanır.

Bu makalede, Azure portalında sanal makine (VM) oluşturduğunuzda, yedeklemeyi etkinleştirmek nasıl açıklanmaktadır.  

## <a name="before-you-start"></a>Başlamadan önce

- [Denetleme](backup-support-matrix-iaas.md#supported-backup-actions) bir VM oluşturduğunuzda, yedekleme etkinleştirirseniz, hangi işletim sistemleri desteklenir.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Hesabınıza zaten oturum açmadıysanız, oturum [Azure portalında](https://portal.azure.com).

## <a name="create-a-vm-with-backup-configured"></a>Yedekleme yapılandırıldı ile VM oluşturma

1. Azure portalında **kaynak Oluştur**.

2. Azure Market'te tıklayın **işlem**ve ardından VM görüntüsünü seçin.

3. VM ile uyumlu olarak ayarlama [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal) veya [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal) yönergeleri.

4. Üzerinde **Yönetim** sekmesinde **yedeklemeyi etkinleştir**, tıklayın **üzerinde**.
5. Bir kurtarma Hizmetleri kasasına Azure yedekleme yedeklemeler. Tıklayın **Yeni Oluştur** mevcut bir kasayı yoksa.
6. Önerilen kasa adını kabul edin veya kendi belirtin.
7. Kasa bulunacağı kaynak grubu oluşturun veya belirtin. Kaynak grubu kasa, VM kaynak grubundan farklı olabilir.

    ![Bir VM için yedeklemeyi etkinleştirme](./media/backup-during-vm-creation/enable-backup.png)

8. Varsayılan yedekleme ilkesini kabul edin ya da ayarları değiştirin.
    - Yedekleme anlık görüntüleri, sanal makinenin ve bu yedekleme kopyası tutmak için ne kadar sık sık almak nasıl bir yedekleme ilkesi belirtir.
    - Varsayılan ilke günde bir kez VM'yi yedekler.
    - Günlük veya haftalık yedekleme bir Azure VM yedekleme kendi ilkenizi özelleştirebilirsiniz.
    - [Daha fazla bilgi edinin](backup-azure-vms-introduction.md#backup-and-restore-considerations) Azure Vm'leri için yedekleme konusunda.
    - [Daha fazla bilgi edinin](backup-instant-restore-capability.md) hakkında anında geri yükleme işlevselliğinin.

      ![Varsayılan yedekleme İlkesi](./media/backup-during-vm-creation/daily-policy.png)


> [!NOTE]
> Azure Backup hizmeti adlandırma biçimi ile anlık görüntü deposu için ayrı bir kaynak grubu (dışında VM kaynak grubu) oluşturur **AzureBackupRG_geography_number** (örnek: AzureBackupRG_northeurope_1). Bu kaynak grubundaki veriler, belirtilen gün sayısı süresince korunur *tut anında kurtarma anlık görüntü* Azure sanal makine yedekleme ilkesinin bir bölümü.  Bu kaynak grubu için bir kilit uygulama, yedekleme hatalarına neden olabilir.<br>
Kısıtlama İlkesi yeniden yedekleme hatalarına neden olduğu kaynak noktası koleksiyonları oluşturulmasını engeller olarak bu kaynak grubu adı/etiketi kısıtlamalar da bırakılmalıdır.


## <a name="start-a-backup-after-creating-the-vm"></a>VM oluşturduktan sonra bir yedeklemeyi başlatın.

VM yedekleme, yedekleme ilkenize uygun şekilde çalışır. Ancak, ilk yedekleme çalıştırmanızı öneririz.

VM oluşturulduktan sonra aşağıdakileri yapın:

1. VM Özellikleri'nde tıklayın **yedekleme**. İlk yedekleme çalıştırılana kadar sanal Makinenin durumu ilk yedekleme bekliyor.
2. Tıklayın **Şimdi Yedekle** isteğe bağlı yedekleme çalıştırmak için.

    ![Bir talep üzerine yedekleme gerçekleştirin](./media/backup-during-vm-creation/run-backup.png)

## <a name="use-a-resource-manager-template-to-deploy-a-protected-vm"></a>Korunan bir sanal makine dağıtmak için bir Resource Manager şablonu kullanma

Önceki adımlarda, bir sanal makine oluşturun ve bir kurtarma Hizmetleri kasasında korumak için Azure portalını kullanma açıklanmaktadır. Hızlı bir şekilde bir veya daha fazla Vm'leri dağıtma ve bir kurtarma Hizmetleri kasasında korumak için şablonu görmek [Windows VM dağıtma ve yedeklemeyi etkinleştirme](https://azure.microsoft.com/resources/templates/101-recovery-services-create-vm-and-configure-backup/).



## <a name="next-steps"></a>Sonraki adımlar

Sanal makinenizin koruma uyguladım, yönetmek ve bunları geri yükleme hakkında bilgi edinin.

- [Vm'leri yönetme ve izleme](backup-azure-manage-vms.md)
- [Sanal makineyi geri yükleme](backup-azure-arm-restore-vms.md)

Herhangi bir sorunla karşılaşırsanız [gözden](backup-azure-vms-troubleshoot.md) sorun giderme kılavuzu.
