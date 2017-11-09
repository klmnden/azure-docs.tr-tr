---
title: Azure VM Yedeklemesi ile ilgili SSS | Microsoft Docs
description: "Azure VM yedeklemesinin çalışması, sınırlamalar ve ilkede değişiklikler yapıldığında ne olacağı hakkındaki yaygın soruların yanıtları"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
keywords: "azure vm yedeklemesi, azure vm geri yükleme, yedekleme ilkesi"
ms.assetid: c4cd7ff6-8206-45a3-adf5-787f64dbd7e1
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/18/2017
ms.author: trinadhk;pullabhk;
ms.openlocfilehash: 85d6ec20fb0447165c672ba267569994e3a96e45
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2017
---
# <a name="questions-about-the-azure-vm-backup-service"></a>Azure VM Yedeklemesi hizmetiyle ilgili sorular
Bu makalede Azure VM Yedeklemesi bileşenlerini kısa süre içinde anlamanıza yardımcı olacak yaygın soruların yanıtları bulunur. Bazı yanıtlarda, kapsamlı bilgiler içeren makalelerin bağlantıları vardır. Ayrıca Azure Backup hizmeti ile ilgili sorularınızı [tartışma forumunda](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup) paylaşabilirsiniz.

## <a name="configure-backup"></a>Yedeklemeyi yapılandırma
### <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Kurtarma Hizmetleri kasaları, klasik VM’leri mi Resource Manager tabanlı VM’leri mi destekler? <br/>
Kurtarma Hizmetleri kasaları iki modeli de destekler.  Klasik portalda oluşturulan bir klasik VM’yi ya da Azure portalında oluşturulan bir Resource Manager VM’sini bir Kurtarma Hizmetleri kasasına yedekleyebilirsiniz.

### <a name="what-configurations-are-not-supported-by-azure-vm-backup"></a>Hangi yapılandırmaları Azure VM yedekleme tarafından desteklenmez?
Git üzerinden [desteklenen işletim sistemleri](backup-azure-arm-vms-prepare.md#supported-operating-system-for-backup) ve [sınırlamalar, VM yedekleme](backup-azure-arm-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm)

### <a name="why-cant-i-see-my-vm-in-configure-backup-wizard"></a>Yedeklemeyi yapılandırma sihirbazında sanal makinemi neden göremiyorum?
Yapılandırma Yedekleme Sihirbazı'nda, Azure Backup yalnızca olan VM'ler listeler:
  * Zaten korumalı VM dikey penceresine gidip ayarları menüsünden yedekleme durumunu denetleme VM yedekleme durumunu doğrulayabilirsiniz. [Sanal makinenin yedekleme durumunu denetleme](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-vm-management-blade) hakkında daha fazla bilgi edinin.
  * Sanal makine ile aynı bölgeye ait olan

## <a name="backup"></a>Backup
### <a name="will-on-demand-backup-job-follow-same-retention-schedule-as-scheduled-backups"></a>İsteğe bağlı yedekleme işi zamanlanmış yedeklemelerle aynı bekletme zamanlamasına mı uyar?
Hayır. Bir talep üzerine yedekleme işi için bekletme aralığı belirtmeniz gerekir. Varsayılan olarak 30 gün boyunca tutulur portalından tetiklendiğinde. 

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-to-work"></a>Kısa süre önce bazı sanal makinelerde Azure Disk Şifrelemesi'ni etkinleştirdim. Yedeklemelerim çalışmaya devam edecek mi?
Azure Backup hizmetinin Key Vault'a erişmesi için izin vermeniz gerekir. [PowerShell](backup-azure-vms-automation.md) belgelerinin *Yedeklemeyi Etkinleştirme* bölümünde belirtilen adımları kullanarak, PowerShell'de bu izinleri sağlayabilirsiniz.

### <a name="i-migrated-disks-of-a-vm-to-managed-disks-will-my-backups-continue-to-work"></a>Bir sanal makinenin disklerini yönetilen disklere geçirdim. Yedeklemelerim çalışmaya devam edecek mi?
Evet, yedekleme sorunsuz bir şekilde çalışır ve yedeklemeyi yeniden yapılandırmanız gerekmez. 

### <a name="my-vm-is-shut-down-will-an-on-demand-or-a-scheduled-backup-work"></a>My VM'yi kapatın. İsteğe bağlı veya zamanlanmış bir yedekleme iş mı?
Evet. Bile bir makine kapatıldığında yedeklemeler çalışır ve kurtarma noktası kilitlenme tutarlı işaretlenir. Veri tutarlılığı bölümünde daha fazla ayrıntı için bkz [bu makalede](backup-azure-vms-introduction.md#how-does-azure-back-up-virtual-machines)

### <a name="can-i-cancel-an-in-progress-backup-job"></a>Devam eden yedekleme işi iptal edebilir miyim?
Evet. "Anlık görüntü almak" aşamasında ise, yedekleme işi iptal edebilirsiniz. **Devam eden anlık görüntü veri aktarımı ise, bir iş iptal edilemiyor**. 

## <a name="restore"></a>Geri Yükleme
### <a name="how-do-i-decide-between-restoring-disks-versus-full-vm-restore"></a>Diskleri geri yüklemekle tam sanal makine geri yüklemesi yapmak arasında nasıl seçim yapabilirim?
Azure tam VM geri yükleme hızlı oluşturma seçeneği olarak düşünün. VM seçeneği değişiklikleri diskler, bu diskler, genel IP adresleri ve ağ arabirimi adlarına göre kullanılan kapsayıcılar adları geri yükleyin. Değişiklik VM oluşturma sırasında oluşturulan kaynakları benzersizliğini tutmak için gereklidir. Ancak VM kullanılabilirlik kümesine eklemez. 

Aşağıdakileri yapmak için diskleri geri yükleme seçeneğini kullanın:
  * Boyutu değiştirme gibi saat yapılandırması noktasından oluşturulan VM özelleştirme
  * Yedekleme aynı anda mevcut olmayan yapılandırmalar ekleme 
  * Oluşturulan kaynakların adlandırma kuralını denetleme
  * Kullanılabilirlik kümesine sanal makine ekleme
  * Herhangi bir yapılandırma için yalnızca PowerShell/bir bildirim şablonu tanımını kullanarak elde edilebilir
  
### <a name="can-i-use-backups-of-unmanaged-disk-vm-to-restore-after-i-upgrade-my-disks-to-managed-disks"></a>Disklerim yönetilen disklere yükselttiğinizde geri yüklemek için yönetilmeyen disk VM yedeklerini kullanabilir miyim?
Evet, geçirme disklerden yönetilmeyen-yönetilen önce alınan yedeklemeler kullanabilirsiniz. Varsayılan olarak, yönetilmeyen disklerle VM geri yükleme VM işi oluşturun. Geri yükleme diskleri işlevselliğini diskleri geri yüklemek ve bunları yönetilen disklerde bir VM oluşturmak için kullanabilirsiniz. 

## <a name="manage-vm-backups"></a>VM yedeklemelerini yönetme
### <a name="what-happens-when-i-change-a-backup-policy-on-vms"></a>Sanal makinelerde yedekleme ilkesini değiştirdiğimde ne olur?
Sanal makine üzerinde yeni bir ilke uygulandığında, zamanlama ve bekletme yeni ilkenin izlendiği. Bekletme genişletilirse, var olan kurtarma noktalarının yeni İlkesi uyarınca tutmak için işaretlenir. Bekletme azaltıldığında bunlar ayıklama sonraki temizleme işi için işaretlenmiş ve daha sonra silinir. 
