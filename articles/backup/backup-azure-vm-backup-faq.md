---
title: Azure sanal makinelerini Azure Backup ile yedekleme hakkında sık sorulan sorular
description: Azure sanal makinelerini Azure Backup ile yedekleme hakkında sık sorulan sorulara yanıtlar.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: sogup
ms.openlocfilehash: 0248e169f5d502cce8723f594f438b87ab088f3a
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67551607"
---
# <a name="frequently-asked-questions-back-up-azure-vms"></a>Sık sorulan sorular-Azure Vm'leri yedekleme

Bu makalede, Azure sanal makinelerini yedekleme hakkında sık sorulan sorular yanıtlanmaktadır [Azure Backup](backup-introduction-to-azure-backup.md) hizmeti.


## <a name="backup"></a>Backup

### <a name="which-vm-images-can-be-enabled-for-backup-when-i-create-them"></a>Bunları oluşturduğunuz hangi sanal makine görüntüleri için yedekleme etkin hale getirilebilir?
Bir VM oluşturduğunuzda, çalışan sanal makineler için yedeklemeyi etkinleştirebilirsiniz [desteklenen işletim sistemleri](backup-support-matrix-iaas.md#supported-backup-actions)

### <a name="is-the-backup-cost-included-in-the-vm-cost"></a>VM maliyeti dahil yedekleme maliyet mevcut mu?

Hayır. Yedekleme maliyetleri ayrı bir sanal makinenin maliyetleri aşağıda sunulmuştur. Daha fazla bilgi edinin [Azure Backup fiyatlandırma](https://azure.microsoft.com/pricing/details/backup/).

### <a name="which-permissions-are-required-to-enable-backup-for-a-vm"></a>Bir VM için yedeklemeyi etkinleştirmek için hangi izinler gereklidir?

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


### <a name="does-an-on-demand-backup-job-use-the-same-retention-schedule-as-scheduled-backups"></a>İsteğe bağlı yedekleme işi zamanlanmış yedeklemeler gibi aynı saklama zamanlaması kullanıyor mu?
Hayır. İsteğe bağlı yedekleme işi için bekletme aralığı belirtin. Varsayılan olarak 30 gün boyunca tutulur portaldan tetiklendiğinde.

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-to-work"></a>Kısa süre önce bazı sanal makinelerde Azure Disk Şifrelemesi'ni etkinleştirdim. Yedeklemelerim çalışmaya devam edecek mi?
Azure Backup'ın Key Vault'a erişmesi için izinler sağlayın. İzinler açıklandığı PowerShell'de belirtin **yedeklemeyi etkinleştir** konusundaki [Azure Backup PowerShell](backup-azure-vms-automation.md) belgeleri.

### <a name="i-migrated-vm-disks-to-managed-disks-will-my-backups-continue-to-work"></a>Ben VM diskleri yönetilen disklere geçişi. Yedeklemelerim çalışmaya devam edecek mi?
Evet, yedeklemeler sorunsuz çalışır. Her şeyi yeniden yapılandırmak için gerek yoktur.

### <a name="why-cant-i-see-my-vm-in-the-configure-backup-wizard"></a>Yedeklemeyi Yapılandır sihirbazında neden VM’mi göremiyorum?
Sihirbaz yalnızca kasasıyla aynı bölgede Vm'leri listeler ve, zaten yedeklenmeyen.

### <a name="my-vm-is-shut-down-will-an-on-demand-or-a-scheduled-backup-work"></a>Sanal Makinem kapatılır. İsteğe bağlı veya zamanlanmış bir yedekleme iş olacak mı?
Evet. Bir makine kapatıldığında yedeklemeleri çalıştırın. Kurtarma noktası kilitlenme tutarlı olarak işaretlenir.

### <a name="can-i-cancel-an-in-progress-backup-job"></a>Devam eden yedekleme işi iptal edebilir miyim?
Evet. Yedekleme işi iptal edebilirsiniz bir **anlık görüntü alınıyor** durumu. Anlık görüntüden veri aktarımı sürüyorsa bir iş iptal edilemiyor.

### <a name="i-enabled-lock-on-resource-group-created-by-azure-backup-service-ie-azurebackuprggeonumber-will-my-backups-continue-to-work"></a>Miyim (yani Azure Backup hizmeti tarafından oluşturulan kaynak grubu kilidi etkin `AzureBackupRG_<geo>_<number>`), yedeklemelerim çalışmaya devam eder mi?
Azure Backup hizmeti tarafından oluşturulan kaynak grubu kilitlerseniz, yedeklemeler 18 geri yükleme noktaları üst sınırına olduğundan başarısız olmaya başlar.

Kullanıcı gerekli kilidi kaldırın ve sonraki yedeklemelerin başarılı olmak için bu kaynak grubundaki geri yükleme noktası koleksiyonunu temizlemek [adımları](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#clean-up-restore-point-collection-from-azure-portal) geri yükleme noktası koleksiyonu kaldırmak için.


### <a name="does-azure-backup-support-standard-ssd-managed-disk"></a>Yoksa Azure yedekleme desteği standart SSD yönetilen disk?
Azure Backup'ın destekledikleri [SSD standart yönetilen diskler](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/). SSD yönetilen diskler, Azure Vm'leri için yeni bir tür dayanıklı depolama sağlar. SSD yönetilen diskler için destek sağlanır [anında geri yükleme](backup-instant-restore-capability.md).

### <a name="can-we-back-up-a-vm-with-a-write-accelerator-wa-enabled-disk"></a>Yazma Hızlandırıcı WA etkinleştirilmiş bir disk sahip bir VM yedekleyebilir miyim?
Anlık görüntüleri WA özellikli diskte alınamaz. Ancak, Azure Backup hizmeti yedekleme dosyasından WA etkin disk hariç tutabilirsiniz.

### <a name="i-have-a-vm-with-write-accelerator-wa-disks-and-sap-hana-installed-how-do-i-back-up"></a>Yazma Hızlandırıcı (WA) disklerle bir VM sahibim ve SAP HANA yüklenir. Nasıl yapılır? yedekleyin
Azure yedekleme WA etkin disk yedekleyemezsiniz, ancak yedeklemeden hariç tutabilirsiniz. Ancak, WA özellikli diskteki bilgileri yedeklenmediğini nedeniyle yedekleme veritabanı tutarlılığını sağlamaz. İşletim sistemi diskini yedekleme ve WA etkin olmayan disk yedekleme istiyorsanız bu yapılandırmayı disklerle yedekleyebilirsiniz.

Biz, 15 dakikalık bir RPO ile bir SAP HANA yedeklemesi için özel Önizleme çalıştırıyorsunuz. SQL DB yedekleme benzer bir şekilde oluşturulmuştur ve üçüncü taraf çözümleri ile SAP HANA sertifikalı backInt arabirim kullanır. İlgileniyorsanız, adresinden bize e-posta `AskAzureBackupTeam@microsoft.com` konu ile **Azure vm'lerde SAP HANA yedeklemesi için özel Önizleme için kaydolun**.

### <a name="what-is-the-maximum-delay-i-can-expect-in-backup-start-time-from-the-scheduled-backup-time-i-have-set-in-my-vm-backup-policy"></a>My VM yedekleme ilkesinde ayarlamış olmanız zamanlanmış yedekleme saatinden yedekleme başlangıç saati, bekleyebileceğiniz en büyük gecikme nedir?
Zamanlanmış yedekleme zamanlanmış yedekleme zamanını 2 saat içinde tetiklenir. İçin örn. 100 VM 2: 00'da zamanlanmış yedekleme başlangıç saati varsa, daha sonra en fazla 4: 00'da tarafından 100VMs yedekleme işi devam eden gerekir. Ardından zamanlanmış yedeklemeler sürdürüldü/denenen ve kesinti nedeniyle duraklatıldı durumunda yedekleme bu zamanlanmış 2 ik penceresi dışında başlayabilirsiniz.

### <a name="what-is-the-minimum-allowed-retention-range-for-daily-backup-point"></a>Günlük yedekleme noktası izin verilen en düşük bekletme aralığı nedir?
Azure sanal makine yedekleme ilkesi en düşük bekletme aralığı 7 gün için 9999 gün yukarı destekler. En düşük bekletme aralığı 7 günü karşılamak için bir güncelleştirme mevcut bir VM yedekleme İlkesi ile 7 günden daha az değişiklik gerektirir.

## <a name="restore"></a>Geri Yükleme

### <a name="how-do-i-decide-whether-to-restore-disks-only-or-a-full-vm"></a>Yalnızca diskleri geri yükle verilip verilmeyeceğini veya tam bir VM nasıl karar verebilirim?
Bir VM geri yüklemesi için bir Azure VM hızlı oluşturma seçeneği olarak düşünün. Bu seçenek, disk adları, diskler, genel IP adresleri ve ağ arabirimi adlarını tarafından kullanılan kapsayıcıları değiştirir. Bir VM oluşturulduğunda değişiklik benzersiz kaynakları tutar. Sanal Makineyi bir kullanılabilirlik kümesine eklenmez.

İsterseniz geri yükleme disk seçeneği kullanabilirsiniz:
  * Oluşturulan VM özelleştirin. Örneğin, boyutu değiştirin.
  * Yedekleme sırasında var olmayan yapılandırma ayarları ekleyin.
  * Oluşturulan kaynakların adlandırma kuralını denetleme.
  * Sanal Makineyi bir kullanılabilirlik kümesine ekleyin.
  * PowerShell ya da bir şablon kullanılarak yapılandırılması gerekir her bir ayar ekleyin.

### <a name="can-i-restore-backups-of-unmanaged-vm-disks-after-i-upgrade-to-managed-disks"></a>Yönetilmeyen sanal makine disklerinin yedeklerini, yönetilen disklere yükselttiğimde geri yükleyebilirim?
Evet, disk alanından yönetilene geçirilen önce alınan yedeklemeler kullanabilirsiniz.
- Varsayılan olarak, bir geri yükleme VM iş yönetilmeyen VM oluşturur.
- Ancak, diskleri geri yükle ve bunları yönetilen bir sanal makine oluşturmak için kullanın.

### <a name="how-do-i-restore-a-vm-to-a-restore-point-before-the-vm-was-migrated-to-managed-disks"></a>VM’yi nasıl yönetilen disklere geçirilmeden önceki bir geri yükleme noktasına geri yüklerim?
Varsayılan olarak, bir geri yükleme VM işi, yönetilmeyen disklerle bir VM oluşturur. Yönetilen disklerle bir VM oluşturmak için:
1. [Yönetilmeyen diskleri geri](tutorial-restore-disk.md#restore-a-vm-disk).
2. [Geri yüklenen disklerden yönetilen disklere dönüştürme](tutorial-restore-disk.md#convert-the-restored-disk-to-a-managed-disk).
3. [Yönetilen disklerle bir VM oluşturma](tutorial-restore-disk.md#create-a-vm-from-the-restored-disk).

[Daha fazla bilgi edinin](backup-azure-vms-automation.md#restore-an-azure-vm) PowerShell'de bunu hakkında.

### <a name="can-i-restore-the-vm-thats-been-deleted"></a>Silinmiş VM geri yükleme?
Evet. VM silme olsa bile, karşılık gelen yedekleme gidebilirsiniz kasaya öğe ve bir kurtarma noktasından geri yükleyin.

### <a name="how-to-restore-a-vm-to-the-same-availability-sets"></a>Bir sanal makine aynı kullanılabilirlik kümesine geri yükleme
Yönetilen Disk Azure VM için kullanılabilirlik kümeleri için geri yükleme, yönetilen diskler olarak geri yüklenirken bir seçenek şablonunda sağlayarak etkinleştirilir. Bu şablon, adlı giriş parametresinin **kullanılabilirlik kümeleri**.

### <a name="how-do-we-get-faster-restore-performances"></a>Daha hızlı geri yükleme performanslarını nasıl aldığımız?
Daha hızlı geri yükleme performansı için biz geçiş yapıyorsanız [anında geri yükleme](backup-instant-restore-capability.md) yeteneği.

## <a name="manage-vm-backups"></a>VM yedeklemelerini yönetme

### <a name="what-happens-if-i-modify-a-backup-policy"></a>Ben bir yedekleme İlkesi değiştirirseniz ne olur?
VM, değiştirilmiş veya yeni ilkenin zamanlama ve bekletme ayarları kullanılarak desteklenir.

- Bekletme süresi uzatıldıysa, bu var olan kurtarma noktalarının işaretlenmiş ve Yeni ilkeye uygun olarak tutulur.
- Bekletme, Kurtarma noktalarını ayıklama sonraki temizleme işleminde için işaretlenmiş ve sonra silinir.

### <a name="how-do-i-move-a-vm-backed-up-by-azure-backup-to-a-different-resource-group"></a>Farklı bir kaynak grubu için Azure Backup tarafından yedeklenen bir VM'ye nasıl taşıyabilirim?

1. Geçici olarak yedeklemeyi Durdur ve yedekleme verilerini koru.
2. Sanal makine için hedef kaynak grubu taşıyın.
3. Aynı ya da yeni kasa yedeklemeye tekrar etkinleştirilecektir.

VM taşıma işleminden önce oluşturulan mevcut geri yükleme noktalarından geri yükleyebilirsiniz.

### <a name="is-there-a-limit-on-number-of-vms-that-can-beassociated-with-a-same-backup-policy"></a>Aynı yedekleme İlkesi ile ilişkilendirilebilen VM sayısına bir sınır var mıdır?
Evet, aynı yedekleme ilkesine portalından ilişkilendirilebilir 100 VM sınırı yoktur. Biz, 100'den fazla VM'ler için önerilir, birden çok yedekleme İlkesi ile aynı zamanlama ya da farklı bir zamanlama oluşturun.
