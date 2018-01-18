---
title: "Oluşturma sırasında Azure VM yedeklemeyi etkinleştirme | Microsoft Docs"
description: "Azure sanal makine yedeklemesi oluşturma işlemi sırasında etkinleştirme adımları bakın."
services: backup, virtual-machines
documentationcenter: 
author: markgalioto
manager: carmonm
tags: azure-resource-manager, virtual-machine-backup
ms.assetid: 
ms.service: backup, virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 01/08/2018
ms.author: trinadhk
ms.openlocfilehash: 4041fc555fe4b61d10f84236dcae5156c6282fd3
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="enable-backup-during-azure-virtual-machine-creation"></a>Azure sanal makine oluşturma sırasında yedeklemeyi etkinleştirme 

Azure Backup hizmeti oluşturmak ve bulut yedeklemeler yapılandırmak için bir arabirim sağlar. Yedeklemeler, Kurtarma noktaları, düzenli aralıklarla adlı gerçekleştirerek verilerinizi koruyun. Azure Backup, coğrafi olarak yedekli kurtarma kasalarında saklanabilecek kurtarma noktaları oluşturur. Bu makalede, Azure portalında yedekleme bir sanal makine (VM) oluşturulurken etkinleştirme ayrıntıları verilmektedir.  

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

Zaten hesabınızda oturum içinde değilse, oturum [Azure portal](http://portal.azure.com).
 
## <a name="create-virtual-machine-with-backup-configured"></a>Yedekleme ile yapılandırılmış sanal makine oluşturma 

1. Azure portalında sol üst köşesinde tıklatın **yeni**. 

2. Seçin **işlem**ve ardından sanal makine görüntüsü seçin.   

3. Sanal makine bilgilerini girin. Kullanıcı adı ve parolası, sanal makinede oturum açmak için kullanılır. İşlem tamamlandığında **Tamam**’a tıklayın. 

4. VM için bir boyut seçin.  

5. Altında **ayarlar > Yedekleme**, tıklatın **etkin** yedekleme yapılandırma ayarlarını getirmek için. Varsayılan değerleri kabul edip tıklatın **Tamam** VM'yi oluşturmak için Özet sayfasına gitmek için Ayarları sayfasında. Değerleri değiştirmek istiyorsanız, sonraki adımları izleyin.  

6. Oluşturun veya sanal makine yedeklerini tutan bir kurtarma Hizmetleri kasası seçin. Bir kurtarma Hizmetleri kasası oluşturuyorsanız, kasa için bir kaynak grubu seçebilirsiniz.  

    ![Vm yedekleme yapılandırmasında sayfa oluşturma](./media/backup-during-vm-creation/create-vm-backup-config.png) 

    > [!NOTE] 
    > Kaynak grubu için kurtarma Hizmetleri kasası kaynak grubu sanal makine için farklı olabilir.  
    > 
    > 

7. Varsayılan olarak, bir yedekleme İlkesi VM hızlı bir şekilde korumak için seçilir. Bir yedekleme İlkesi sık yedekleme anlık görüntüsünü nasıl belirtir ve bu yedek kopyaları korumak için ne kadar süre. Varsayılan ilkeyi kabul edebilir veya oluşturun veya farklı bir yedekleme ilkesi seçin. Yedekleme ilkesini düzenlemek için seçin **yedekleme İlkesi** ve ilke değerleri değiştirin.  

8. Yedekleme yapılandırması değerlerle ayar sayfasında, hazır olduğunuzda tıklatın **Tamam**.  

9. Özet sayfasında, doğrulamayı geçtikten sonra tıklayın **oluşturma** yapılandırılmış yedekleme ayarları kullanan bir sanal makine oluşturmak için. 

## <a name="initiate-a-backup-after-creating-the-vm"></a>VM oluşturduktan sonra bir yedekleme başlatmak 

Yedekleme İlkesi oluşturuldu ancak ilk yedeği oluşturmak iyi bir uygulamadır. Sanal bir kez VM oluşturma şablonu sonlandığında gelen makine için yedekleme ayrıntılarını görüntülemek için **Operations** sol menüde ayarı tıklatın **yedekleme**. Bu, bir isteğe bağlı yedeklemeyi tetikleyin, tam bir VM veya tüm diskleri geri yüklemek, dosyaları VM yedekten geri yükleyin veya sanal makine ile ilişkili yedekleme ilkesini değiştirmek için kullanabilirsiniz.  

## <a name="frequently-asked-questions"></a>Sık sorulan sorular 

### <a name="which-vm-images-enable-backup-at-the-time-of-vm-creation"></a>Hangi VM görüntüleri VM oluşturma sırasında yedekleme etkinleştirilsin mi? 

Microsoft tarafından yayımlanan çekirdek görüntülerinin aşağıdaki listede, VM oluşturma sırasında yedekleme etkinleştirmek için desteklenir. Diğer VM'ler için VM oluşturulduktan sonra yedekleme etkinleştirebilirsiniz. Daha fazla bilgi edinin [VM oluşturulduktan sonra yedeklemeyi etkinleştir](quick-backup-vm-portal.md) 

- **Windows** -Windows Server 2016 veri merkezi, Windows Server 2016 veri merkezi çekirdek, Windows Server 2012 DataCenter, Windows Server 2012 R2 DataCenter, Windows Server 2008 R2 SP1 
- **Ubuntu** -Ubuntu Server 1710, Ubuntu Server 1704, UUbuntu sunucu 1604(LTS), Ubuntu Server 1404(LTS) 
- **RedHat** -RHEL 6.7, 6,8 6.9, 7.2, 7.3, 7.4 
- **SUSE** -SUSE Linux Enterprise Server 11 SP4'ü, 12 SP2, 12 SP3 
- **Debian** -Debian 8, Debian 9 
- **CentOS** -CentOS 6.9, CentOS 7.3 
- **Oracle Linux** -Oracle Linux 6.7, 6,8 6.9, 7.2, 7.3 
 
### <a name="is-backup-cost-included-in-the-vm-cost"></a>VM maliyet dahil yedekleme Maliyet mi? 

Hayır, yedekleme maliyetleri ayrı ya da ayrı, sanal makineleri maliyetleri. Yedekleme fiyatlandırma hakkında daha fazla bilgi için bkz: [yedekleme fiyatlandırma site](https://azure.microsoft.com/pricing/details/backup/).
 
### <a name="which-permissions-are-required-to-enable-backup-on-a-vm"></a>Hangi izinlerin bir VM üzerinde yedekleme etkinleştirmek için gerekli olan? 

Bir sanal makine Katılımcısı olmanız durumunda VM yedekleme etkinleştirebilirsiniz. Özel bir rol kullanıyorsanız, VM Yedekleme başarıyla etkinleştirmek için aşağıdaki izinler gerekir. 

- Microsoft.RecoveryServices/Vaults/write 
- Microsoft.RecoveryServices/Vaults/read 
- Microsoft.RecoveryServices/locations/* 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write 
- Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write 
- Microsoft.RecoveryServices/Vaults/backupPolicies/read 
- Microsoft.RecoveryServices/Vaults/backupPolicies/write 
 
Kurtarma Hizmetleri kasası ve sanal makineyi farklı bir kaynak gruplarınız varsa, Kurtarma Hizmetleri kasası kaynak grubunda yazma izinlerine sahip olduğundan emin olun.  

## <a name="next-steps"></a>Sonraki adımlar 

VM korumalı, VM yönetim görevleri ve sanal makineleri geri yükleme hakkında bilgi edinmek için aşağıdaki makalelere bakın. 

- [Sanal makinelerinizi yönetme ve izleme](backup-azure-manage-vms.md) 
- [Sanal makineleri geri yükleme](backup-azure-arm-restore-vms.md) 
