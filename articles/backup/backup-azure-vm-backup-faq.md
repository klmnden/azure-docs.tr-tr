---
title: Azure VM yedeklemesi hakkında SSS
description: Azure VM yedeklemesinin çalışması, sınırlamalar ve ilkede değişiklikler yapıldığında ne olacağı hakkındaki yaygın soruların yanıtları
services: backup
author: trinadhk
manager: shreeshd
keywords: azure vm yedeklemesi, azure vm geri yükleme, yedekleme ilkesi
ms.service: backup
ms.topic: conceptual
ms.date: 8/16/2018
ms.author: trinadhk
ms.openlocfilehash: ba77ec34e7887f676ea3df101e87c1ea80fceec5
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50414803"
---
# <a name="questions-about-the-azure-vm-backup-service"></a>Azure VM Yedeklemesi hizmetiyle ilgili sorular
Bu makalede Azure VM Yedeklemesi bileşenlerini kısa süre içinde anlamanıza yardımcı olacak yaygın soruların yanıtları bulunur. Bazı yanıtlarda, kapsamlı bilgiler içeren makalelerin bağlantıları vardır. Ayrıca Azure Backup hizmeti ile ilgili sorularınızı [tartışma forumunda](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup) paylaşabilirsiniz.

## <a name="configure-backup"></a>Yedeklemeyi yapılandırma
### <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Kurtarma Hizmetleri kasaları, klasik VM’leri mi Resource Manager tabanlı VM’leri mi destekler? <br/>
Kurtarma Hizmetleri kasaları iki modeli de destekler.  Klasik VM'yi ya bir Resource Manager VM bir kurtarma Hizmetleri kasasına yedekleyebilirsiniz.

### <a name="what-configurations-are-not-supported-by-azure-vm-backup"></a>Azure VM yedeklemesinde hangi yapılandırmalar desteklenmez?
Git aracılığıyla [desteklenen işletim sistemleri](backup-azure-arm-vms-prepare.md#supported-operating-systems-for-backup) ve [VM yedeklemesinin sınırları](backup-azure-arm-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm)

### <a name="why-cant-i-see-my-vm-in-configure-backup-wizard"></a>Yedeklemeyi yapılandırma sihirbazında sanal makinemi neden göremiyorum?
Yedeklemeyi yapılandırma sihirbazında, Azure Backup yalnızca VM'ler listeler:
  * Zaten korumalı olmayan VM dikey penceresine gidip ayarlar menüsünde yedekleme durumunu denetleme, sanal Makinenin yedekleme durumunu doğrulayabilirsiniz. [Sanal makinenin yedekleme durumunu denetleme](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-vm-operations-menu) hakkında daha fazla bilgi edinin.
  * Sanal makine ile aynı bölgeye ait olan

## <a name="backup"></a>Backup
### <a name="will-on-demand-backup-job-follow-same-retention-schedule-as-scheduled-backups"></a>İsteğe bağlı yedekleme işi zamanlanmış yedeklemelerle aynı bekletme zamanlamasına mı uyar?
Hayır. İsteğe bağlı yedekleme işi için bekletme aralığı belirtmeniz gerekir. Varsayılan olarak 30 gün boyunca tutulur portaldan tetiklendiğinde.

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-to-work"></a>Kısa süre önce bazı sanal makinelerde Azure Disk Şifrelemesi'ni etkinleştirdim. Yedeklemelerim çalışmaya devam edecek mi?
Azure Backup hizmetinin Key Vault'a erişmesi için izin vermeniz gerekir. [PowerShell](backup-azure-vms-automation.md) belgelerinin *Yedeklemeyi Etkinleştirme* bölümünde belirtilen adımları kullanarak, PowerShell'de bu izinleri sağlayabilirsiniz.

### <a name="i-migrated-disks-of-a-vm-to-managed-disks-will-my-backups-continue-to-work"></a>Bir sanal makinenin disklerini yönetilen disklere geçirdim. Yedeklemelerim çalışmaya devam edecek mi?
Evet, yedeklemeler sorunsuz çalışır ve yedeklemeyi yeniden yapılandırmanız gerekmez.

### <a name="my-vm-is-shut-down-will-an-on-demand-or-a-scheduled-backup-work"></a>Sanal Makinem kapatılır. İsteğe bağlı veya zamanlanmış bir yedekleme iş olacak mı?
Evet. Hatta bir makine kapatıldığında yedeklemelerin gerçekleştirildiğinden ve kurtarma noktası kilitlenme tutarlı işaretlenir. Veri tutarlılığı bölümünde daha fazla ayrıntı için bkz [bu makalede](backup-azure-vms-introduction.md#how-does-azure-back-up-virtual-machines)

### <a name="can-i-cancel-an-in-progress-backup-job"></a>Devam eden yedekleme işi iptal edebilir miyim?
Evet. "Anlık görüntü almak" aşamasında olan, bir yedekleme işi iptal edebilirsiniz. **Anlık görüntüden veri aktarımı sürüyorsa bir iş iptal edilemiyor**.

### <a name="i-enabled-resource-group-lock-on-my-backed-up-managed-disk-vms-will-my-backups-continue-to-work"></a>Ben kaynak grubu kilit my yedeklenen yönetilen disk Vm'leri üzerinde etkin. Yedeklemelerim çalışmaya devam edecek mi?
Kullanıcının kaynak grubunu kilitler, Backup hizmeti daha eski geri yükleme noktalarını silmek mümkün değil. Bu nedenle, yeni yedeklemeleri'nin arka ucundan uygulanan en fazla 18 geri yükleme noktası sınırı olduğundan başarısız olmaya başlar. RG kilitlendikten sonra Yedeklemelerinizin bir iç hata ile başarısız oluyorsa, aşağıdaki adımları [geri yüklemeyi kaldırma adımları işaret koleksiyonu](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#clean-up-restore-point-collection-from-portal-created-by-backup-service).

### <a name="does-backup-policy-take-daylight-saving-timedst-into-account"></a>Yedekleme İlkesi hesaba Yaz Time(DST) kaydetme sürüyor?
Hayır. Tarih ve saat, yerel bilgisayarınızda görüntülenir, yerel saat ve geçerli gün ışığından yararlanma saatine göre olduğunu unutmayın. Bu nedenle zamanlanmış yedeklemeler için yapılandırılan süre, yerel saat DST nedeniyle farklı olabilir.

### <a name="maximum-of-how-many-data-disks-can-i-attach-to-a-vm-to-be-backed-up-by-azure-backup"></a>Kaç tane veri diskleri en fazla bir VM, Azure Backup tarafından yedeklenmek üzere ekleme?
Azure Backup artık 32 adede kadar disklere sahip sanal makinelerin yedeklenmesini destekler. 32 disk desteği almak için [Azure VM yedekleme yığını v2'ye yükseltme](backup-upgrade-to-vm-backup-stack-v2.md). 24 Eylül 2018 tarihinden itibaren korunmasını tüm VM'lerin desteklenen.

### <a name="does-azure-backup-support-standard-ssd-managed-disk"></a>Mu Azure yedekleme desteği standart SSD disk yönetiliyor?
Azure Backup'ın destekledikleri [SSD standart yönetilen diskler](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/), Microsoft Azure sanal makineler için kalıcı depolama için yeni bir tür. Üzerinde yönetilen diskler için desteklenir [Azure VM yedekleme yığını v2'ye](backup-upgrade-to-vm-backup-stack-v2.md).

## <a name="restore"></a>Geri Yükleme
### <a name="how-do-i-decide-between-restoring-disks-versus-full-vm-restore"></a>Diskleri geri yüklemekle tam sanal makine geri yüklemesi yapmak arasında nasıl seçim yapabilirim?
Azure tam sanal makine geri yüklemesini hızlı oluşturma seçeneği olarak düşünün. VM seçeneği değişiklikleri diskler, bu diskler, genel IP adresleri ve ağ arabirimi adlarını tarafından kullanılan kapsayıcıları adlarını geri yükleyin. Değişiklik oluşturulduğunda, VM oluşturma sırasında oluşturulan kaynakları korumak için gereklidir. Ancak, sanal Makineyi kullanılabilirlik kümesine eklemez.

Aşağıdakileri yapmak için diskleri geri yükleme seçeneğini kullanın:
  * Boyutu değiştirme gibi yapılandırma zaman noktasından oluşturulan VM özelleştirme
  * Yedekleme sırasında mevcut olmayan yapılandırmalar ekleme
  * Oluşturulan kaynakların adlandırma kuralını denetleme
  * Kullanılabilirlik kümesine sanal makine ekleme
  * Herhangi bir yapılandırma için yalnızca bir PowerShell/bildirim temelli bir şablon tanımı kullanılarak sağlanabilir.

### <a name="can-i-use-backups-of-unmanaged-disk-vm-to-restore-after-i-upgrade-my-disks-to-managed-disks"></a>Disklerim yönetilen disklere yükselttiğinizde geri yüklemek için yönetilmeyen disk VM yedekleme kullanabilir miyim?
Evet, geçirme disklerden yönetilmeyen-yönetilen önce geçen yedeklerini kullanabilirsiniz. Varsayılan olarak, yönetilmeyen disklerle bir VM geri yükleme VM işi oluşturun. Diskleri geri yükle ve bunları yönetilen diskler üzerindeki bir VM oluşturmak için geri yükleme diskler işlevini kullanabilirsiniz.

### <a name="what-is-the-procedure-to-restore-a-vm-to-a-restore-point-taken-before-the-conversion-from-unmanaged-to-managed-disks-was-done-for-a-vm"></a>Bir VM için yönetilen diskleri dönüştürme yönetilmeyenden yapıldığı önce gerçekleştirilen bir geri yükleme noktası için bir VM geri yükleme işlemini nedir?
Bu senaryoda, varsayılan olarak, geri yükleme VM işi yönetilmeyen disklerle bir VM oluşturma. Yönetilen disklerle bir VM oluşturmak için:
1. [Yönetilmeyen diskleri geri yükle](tutorial-restore-disk.md#restore-a-vm-disk)
2. [Geri yüklenen disklerden yönetilen disklere dönüştürme](tutorial-restore-disk.md#convert-the-restored-disk-to-a-managed-disk)
3. [Yönetilen disklerle bir VM oluşturma](tutorial-restore-disk.md#create-a-vm-from-the-restored-disk) <br>
PowerShell cmdlet'leri için başvuru [burada](backup-azure-vms-automation.md#restore-an-azure-vm).

### <a name="can-i-restore-the-vm-if-my-vm-is-deleted"></a>VM, sanal Makinem silinirse geri yükleyebilirim?
Evet. VM yaşam döngüsü ve kendi karşılık gelen yedekleme öğesi farklıdır. Böylece VM silseniz bile, karşılık gelen öğe kurtarma Hizmetleri kasasında yedekleme ve kurtarma noktalarından birini kullanarak bir geri yüklemeyi tetikleyecek gidebilirsiniz.

## <a name="manage-vm-backups"></a>VM yedeklemelerini yönetme
### <a name="what-happens-when-i-change-a-backup-policy-on-vms"></a>Sanal makinelerde yedekleme ilkesini değiştirdiğimde ne olur?
Yeni bir ilke uygulandığında uygulandığında yeni ilkenin zamanlama ve ardından. Bekletme süresi uzatıldıysa, yeni ilkeye göre tutulması için mevcut kurtarma noktaları işaretlenir. Bekletme süresi kısaltıldıysa, bunlar sonraki temizleme işleminde kesilmek üzere işaretlenir ve sonra silinir.

### <a name="how-can-i-move-a-vm-enrolled-in-azure-backup-between-resource-groups"></a>Azure backup kaynak grupları arasında bir sanal makine kaydedilmiş nasıl taşıyabilirim?
İzleyin başarılı bir şekilde hedef kaynak grubu için yedeklenen VM'ye taşımak için aşağıdaki adımları
1. Geçici olarak yedeklemeyi Durdur ve yedekleme verilerini koru
2. Hedef kaynak grubu için VM'yi taşıma
3. Aynı/yeni kasaya yeniden koruma

Kullanıcılar, taşıma işleminden önce oluşturulan mevcut geri yükleme noktalarından geri yükleyebilirsiniz.
