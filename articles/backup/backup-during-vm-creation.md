---
title: Oluşturma sırasında Azure VM yedeklemeyi etkinleştirme
description: Oluşturma işlemi sırasında Azure sanal makine yedeklemeyi etkinleştirmek için adımlara bakın.
services: backup, virtual-machines
author: rayne-wiselman
manager: carmonm
tags: azure-resource-manager, virtual-machine-backup
ms.service: backup, virtual-machines
ms.topic: conceptual
ms.date: 01/08/2018
ms.author: trinadhk
ms.openlocfilehash: 518d171c96b9c4f9bf3e195a7130f4c022b7ad07
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52879885"
---
# <a name="enable-backup-during-azure-virtual-machine-creation"></a>Azure sanal makine oluşturma işlemi sırasında yedeklemeyi etkinleştirme 

Azure Backup hizmeti oluşturmak ve bulutta yedeklemeleri yapılandırmak için bir arabirim sağlar. Yedekleme, Kurtarma noktalarının düzenli aralıklarla adlı yararlanarak verilerinizi koruyun. Azure Backup, coğrafi olarak yedekli kurtarma kasalarında saklanabilecek kurtarma noktaları oluşturur. Bu makalede, Azure portalında sanal makine (VM) oluştururken yedeklemeyi etkinleştirmek nasıl açıklanmaktadır.  

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

Zaten hesabınızda oturum içinde kök kullanıcı değilseniz, oturum [Azure portalında](http://portal.azure.com).
 
## <a name="create-virtual-machine-with-backup-configured"></a>Yedekleme ile yapılandırılmış sanal makine oluşturma 

1. Azure portalının sol alt köşesinde, tıklayın **yeni**. 

2. Seçin **işlem**ve ardından sanal makinenin bir görüntüsünü seçin.   

3. Sanal makine bilgilerini girin. Kullanıcı adı ve parolası, sanal makineye oturum açmak için kullanılır. İşlem tamamlandığında **Tamam**’a tıklayın. 

4. VM için bir boyut seçin.  

5. Altında **ayarlar > Yedekleme**, tıklayın **etkin** yedekleme yapılandırma ayarlarını getirilecek. Varsayılan değerleri kabul edin ve tıklayın **Tamam** VM oluşturmak için Özet sayfasına gitmek için Ayarları sayfasında. Değerleri değiştirmek isterseniz, sonraki adımları izleyin.  

6. Oluşturun veya sanal makine yedeklemelerini barındıran bir kurtarma Hizmetleri Kasası'nı seçin. Bir kurtarma Hizmetleri kasası oluşturuyorsanız bu kasa için bir kaynak grubu seçebilirsiniz.  

    ![Sanal makinede yedekleme yapılandırması oluşturma sayfası](./media/backup-during-vm-creation/create-vm-backup-config.png) 

    > [!NOTE] 
    > Kurtarma Hizmetleri kasasının kaynak grubu, kaynak grubunu sanal makine için farklı olabilir.  
    > 
    > 

7. Varsayılan olarak, bir yedekleme İlkesi hızlı bir şekilde sanal Makineyi korumak için seçilir. Sık sık yedekleme anlık görüntüleri almak nasıl bir yedekleme İlkesi belirler ve bu yedek kopyaların ne kadar. Varsayılan ilkeyi kabul edebilir veya oluşturabilir veya farklı bir yedekleme İlkesi'ni seçin. Yedekleme ilkesini düzenlemek için seçin **yedekleme İlkesi** ve ilke değerlerini değiştirin.  

8. Yedekleme yapılandırması değerlerle ayar sayfasında hazır olduğunuzda tıklayın **Tamam**.  

9. Özet sayfasında, doğrulamayı geçtikten sonra tıklayın **Oluştur** yapılandırılan yedekleme ayarları kullanan bir sanal makine oluşturmak için. 

## <a name="initiate-a-backup-after-creating-the-vm"></a>VM oluşturduktan sonra bir yedekleme başlatmak 

Yedekleme İlkesi oluşturuldu ancak ilk yedekleme oluşturmak iyi bir uygulamadır. Sanal bir kez VM oluşturma şablon tamamlandığında, gelen makine için yedekleme ayrıntılarını görüntülemek için **işlemleri** sol taraftaki menüden ayarını tıklayın **yedekleme**. İsteğe bağlı bir yedeklemeyi tetikleyin, tam bir VM veya tüm diskleri geri yükleme, dosyaları VM yedekten geri yükleyin veya sanal makineyle ilişkilendirilmiş yedekleme ilkesini değiştirmek için kullanabilirsiniz.  

## <a name="using-a-resource-manager-template-to-deploy-a-protected-vm"></a>Korunan bir sanal makine dağıtmak için Resource Manager şablonu kullanma

Önceki adımlarda, bir sanal makine oluşturun ve bir kurtarma Hizmetleri kasasına korumak için Azure portalını kullanma açıklanmaktadır. Hızlı bir şekilde bir veya daha fazla sanal makine dağıtma ve bir kurtarma Hizmetleri kasasına korumak istiyorsanız, şablonu görmek [Windows VM dağıtma ve yedeklemeyi etkinleştirme](https://azure.microsoft.com/resources/templates/101-recovery-services-create-vm-and-configure-backup/).

## <a name="frequently-asked-questions"></a>Sık sorulan sorular 

### <a name="which-vm-images-enable-backup-at-the-time-of-vm-creation"></a>Hangi VM görüntüleri, VM oluşturma sırasında yedekleme etkinleştirilsin mi? 

Aşağıdaki listede yer alan Microsoft tarafından yayımlanan temel görüntüleri, yedekleme sırasında VM oluşturmayı etkinleştirmek için desteklenir. VM oluşturulduktan sonra diğer sanal makineler için yedeklemeyi etkinleştirebilirsiniz. Daha fazla bilgi edinin [VM oluşturulduktan sonra yedeklemeyi etkinleştir](quick-backup-vm-portal.md) 

- **Windows** -Windows Server 2016 veri merkezi, Windows Server 2016 veri merkezi core, Windows Server 2012 DataCenter, Windows Server 2012 R2 DataCenter, Windows Server 2008 R2 SP1 
- **Ubuntu** -Ubuntu Server 1710, Ubuntu Server 1704, UUbuntu sunucu 1604(LTS) Ubuntu Server 1404(LTS) 
- **Red Hat** -RHEL 6.7, 6,8 6.9, 7.2 7.3, 7.4 
- **SUSE** -SUSE Linux Enterprise Server 11 SP4, 12 SP2, 12 SP3 
- **Debian** -Debian 8, Debian 9 
- **CentOS** -CentOS 6.9, CentOS 7.3 
- **Oracle Linux** -Oracle Linux 6.7, 6,8, 6.9, 7.2, 7.3 
 
### <a name="is-backup-cost-included-in-the-vm-cost"></a>Yedekleme maliyeti VM maliyeti dahil mi? 

Hayır, ayrı ve farklı, sanal makineler maliyetleri yedekleme maliyetleri aşağıda sunulmuştur. Yedekleme fiyatlandırması hakkında daha fazla bilgi için bkz. [site yedekleme fiyatlandırma](https://azure.microsoft.com/pricing/details/backup/).
 
### <a name="which-permissions-are-required-to-enable-backup-on-a-vm"></a>Bir sanal makinede yedeklemeyi etkinleştirmek için hangi izinler gereklidir? 

Bir sanal makine Katılımcısı, sanal makine yedeklemeyi etkinleştirebilirsiniz. Özel bir rol kullanıyorsanız, VM Yedekleme başarıyla etkinleştirmek için aşağıdaki izinler gerekir. 

- Microsoft.RecoveryServices/Vaults/write 
- Microsoft.RecoveryServices/Vaults/read 
- Microsoft.RecoveryServices/locations/* 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write 
- Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write 
- Microsoft.RecoveryServices/Vaults/backupPolicies/read 
- Microsoft.RecoveryServices/Vaults/backupPolicies/write 
 
Kurtarma Hizmetleri kasası ve sanal makine farklı kaynak grupları varsa, Kurtarma Hizmetleri kasası kaynak grubu içinde yazma izinlerine sahip olduğunuzdan emin olun.  

## <a name="next-steps"></a>Sonraki adımlar 

Sanal makinenizin koruduktan sonra VM yönetimi görevlerini ve Vm'leri geri yükleme hakkında bilgi edinmek için aşağıdaki makalelere bakın. 

- [Sanal makinelerinizi yönetme ve izleme](backup-azure-manage-vms.md) 
- [Sanal makineleri geri yükleme](backup-azure-arm-restore-vms.md) 
