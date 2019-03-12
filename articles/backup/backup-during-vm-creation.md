---
title: Oluşturma sırasında Azure VM yedeklemeyi etkinleştirme
description: Azure sanal makine yedekleme oluşturma işlemi sırasında elverişli hale getirme.
services: backup, virtual-machines
author: rayne-wiselman
manager: carmonm
tags: azure-resource-manager, virtual-machine-backup
ms.service: backup
ms.topic: conceptual
ms.date: 01/08/2018
ms.author: trinadhk
ms.openlocfilehash: fd2beaa39f03d4f2342c94bf1cd8b8aea7440e62
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57780452"
---
# <a name="enable-backup-when-you-create-an-azure-virtual-machine"></a>Bir Azure sanal makine oluşturduğunuzda, yedeklemeyi etkinleştirme

Azure Backup hizmeti, Azure sanal makinelerini (VM) yedekleme için kullanın. Vm'leri yedekleme ilkesinde belirtilen bir zamanlamaya göre yedeklenir ve yedeklerinden kurtarma noktaları oluşturulur. Kurtarma noktaları, Kurtarma Hizmetleri kasalarında depolanır.

Bu makalede, Azure portalında sanal makine (VM) oluştururken yedeklemeyi etkinleştirmek nasıl açıklanmaktadır.  

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Hesabınıza zaten oturum açmadıysanız, oturum [Azure portalında](https://portal.azure.com).
 
## <a name="create-a-vm-with-backup-configured"></a>Yedekleme yapılandırıldı ile VM oluşturma 

1. Azure portalının sol üst köşedeki seçin **yeni**.

1. Seçin **işlem**ve ardından VM görüntüsünü seçin.

1. VM bilgilerini girin. Kullanıcı adı ve parola sağlayan, VM'de oturum açmak için kullanılır. İşlemi tamamladığınızda, seçin **Tamam**. 

1. VM için bir boyut seçin.  

1. Altında **ayarları** > **yedekleme**seçin **etkin** yedekleme yapılandırma ayarlarını açın.

   - Varsayılan değerleri kabul etmek için işaretleyin **Tamam** üzerinde **ayarları** sayfası. Ardından gideceğiniz **özeti** VM oluşturmak için sayfa. Adım 6-8 atlayın.
   - Yedekleme yapılandırması değerleri değiştirmek için sonraki adımları izleyin.  

1. Oluşturun veya sanal makine yedeklerini tutmak için bir kurtarma Hizmetleri Kasası'nı seçin. Kurtarma Hizmetleri kasası oluşturuyorsanız bu kasa için bir kaynak grubu seçebilirsiniz.  

    ![Yedekleme yapılandırması ayarlarında VM oluşturma sayfası](./media/backup-during-vm-creation/create-vm-backup-config.png) 

    > [!NOTE] 
    > Kurtarma Hizmetleri kasasının kaynak grubu, sanal makine için kaynak grubu farklı olabilir.  

1. Varsayılan olarak, sanal Makineyi korumak için bir yedekleme İlkesi seçilir. Sık sık yedekleme anlık görüntüleri almak nasıl bir yedekleme İlkesi belirler ve bu yedekleme kopyası tutmak için ne kadar. 

   - Varsayılan ilkeyi kabul edebilir veya oluşturabilir veya farklı bir yedekleme İlkesi'ni seçin. 
   - Yedekleme ilkesini düzenlemek için seçin **yedekleme İlkesi** ve değerlerini değiştirin.  

1. Yedekleme yapılandırması değerlerini ayarlamayı tamamladığınızda seçin **Tamam** üzerinde **ayarları** sayfası.  

1. Üzerinde **özeti** sayfasında doğrulamayı geçtikten sonra seçin **Oluştur** yapılandırılan yedekleme ayarları kullanan bir VM oluşturmak için. 

## <a name="start-a-backup-after-creating-the-vm"></a>VM oluşturduktan sonra bir yedeklemeyi başlatın. 

Sanal Makineniz için bir yedekleme İlkesi yapılandırılmış olsa da, ilk yedekleme oluşturmak için iyi bir uygulamadır. 

VM şablonu oluşturma tamamlandıktan sonra Git **işlemleri** sol menüsünü seçip **yedekleme** sanal makine için yedekleme ayrıntılarını görüntülemek için. Bu sayfaya kullanabilirsiniz:

- İsteğe bağlı yedekleme tetikleyin.
- Tam bir VM veya tüm diskleri geri yükleyin.
- Dosyaları bir VM yedekten geri yükleyin.
- Sanal makine ile ilişkilendirilmiş yedekleme ilkesini değiştirin.  

## <a name="use-a-resource-manager-template-to-deploy-a-protected-vm"></a>Korunan bir sanal makine dağıtmak için bir Resource Manager şablonu kullanma

Önceki adımlarda, bir sanal makine oluşturun ve bir kurtarma Hizmetleri kasasında korumak için Azure portalını kullanma açıklanmaktadır. Hızlı bir şekilde bir veya daha fazla sanal makineleri dağıtmak ve bir kurtarma Hizmetleri kasasında korumak için şablonu görmek [Windows VM dağıtma ve yedeklemeyi etkinleştirme](https://azure.microsoft.com/resources/templates/101-recovery-services-create-vm-and-configure-backup/).

## <a name="frequently-asked-questions"></a>Sık sorulan sorular 

### <a name="which-vm-images-support-backup-configuration-during-vm-creation"></a>Yedekleme yapılandırması VM oluşturma sırasında hangi VM görüntülerini destekler?

Microsoft tarafından yayımlanan aşağıdaki çekirdek görüntüler, yedekleme sırasında VM oluşturmayı etkinleştirmek için desteklenir. VM oluşturulduktan sonra diğer sanal makineler için yedeklemeyi etkinleştirebilirsiniz. Daha fazla bilgi için bkz. [VM oluşturulduktan sonra yedeklemeyi etkinleştir](quick-backup-vm-portal.md).

- **Windows** -Windows Server 2016 Datacenter, Windows Server 2016 Datacenter Core, Windows Server 2012 Datacenter, Windows Server 2012 R2 Datacenter, Windows Server 2008 R2 SP1 
- **Ubuntu** -Ubuntu Server 17.10, Ubuntu Server 17.04, Ubuntu Server 16.04 (LTS), Ubuntu Server 14.04 (LTS) 
- **Red Hat** -RHEL 6.7, 6,8 6.9, 7.2 7.3, 7.4 
- **SUSE** -SUSE Linux Enterprise Server 11 SP4, 12 SP2, 12 SP3 
- **Debian** -Debian 8, Debian 9 
- **CentOS** - CentOS 6.9, CentOS 7.3 
- **Oracle Linux** - Oracle Linux 6.7, 6.8, 6.9, 7.2, 7.3 
 
### <a name="is-the-backup-cost-included-in-the-vm-cost"></a>VM maliyeti dahil yedekleme maliyet mevcut mu? 

Hayır. Yedekleme maliyetleri ayrı bir sanal makinenin maliyetleri aşağıda sunulmuştur. Yedekleme fiyatlandırması hakkında daha fazla bilgi için bkz. [Azure Yedekleme'nin fiyatlandırma](https://azure.microsoft.com/pricing/details/backup/).
 
### <a name="which-permissions-are-required-to-enable-backup-on-a-vm"></a>Bir sanal makinede yedeklemeyi etkinleştirmek için hangi izinler gereklidir? 

Bir VM katkıda bulunanı olduğunuz, sanal makine yedeklemeyi etkinleştirebilirsiniz. Özel bir rol kullanıyorsanız, sanal makine yedeklemeyi etkinleştirmek için aşağıdaki izinlere ihtiyacınız vardır: 

- Microsoft.RecoveryServices/Vaults/write 
- Microsoft.RecoveryServices/Vaults/read 
- Microsoft.RecoveryServices/locations/* 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write 
- Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write 
- Microsoft.RecoveryServices/Vaults/backupPolicies/read 
- Microsoft.RecoveryServices/Vaults/backupPolicies/write 
 
Farklı kaynak gruplarında VM ve kurtarma Hizmetleri kasası varsa, kaynak grubunda bir kurtarma Hizmetleri kasası için yazma izinlerine sahip olduğunuzdan emin olun.  

## <a name="next-steps"></a>Sonraki adımlar 

Sanal makinenizin koruma uyguladım, yönetme ve Vm'leri geri yükleme hakkında bilgi edinmek için aşağıdaki makalelere bakın:

- [Sanal makinelerinizi yönetme ve izleme](backup-azure-manage-vms.md) 
- [Sanal makineleri geri yükleme](backup-azure-arm-restore-vms.md) 
