---
title: Azure VM yedeklemesi hakkında SSS
description: Azure VM yedeklemesinin çalışması, sınırlamalar ve ilkede değişiklikler yapıldığında ne olacağı hakkındaki yaygın soruların yanıtları
services: backup
author: trinadhk
manager: shreeshd
ms.service: backup
ms.topic: conceptual
ms.date: 8/16/2018
ms.author: trinadhk
ms.openlocfilehash: 31a708f3a0da76ab13e789b099f312cca1f86e08
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54332260"
---
# <a name="frequently-asked-questions-azure-backup"></a>Sık sorulan sorular Azure Backup

Bu makalede hakkında sık sorulan soruları yanıtlar [Azure Backup](backup-introduction-to-azure-backup.md) hizmeti.

## <a name="general-questions"></a>Genel sorular


### <a name="what-azure-vms-can-you-back-up-using-azure-backup"></a>Hangi Azure Vm'leri Azure Backup kullanarak yedekleyebilir?
[Gözden geçirme](backup-azure-arm-vms-prepare.md#before-you-start) desteklenen işletim sistemleri ve sınırlamaları.



## <a name="backup"></a>Backup

### <a name="does-an-on-demand-backup-job-use-the-same-retention-schedule-as-scheduled-backups"></a>İsteğe bağlı yedekleme işi zamanlanmış yedeklemeler gibi aynı saklama zamanlaması kullanıyor mu?
Hayır. İsteğe bağlı yedekleme işi için bekletme aralığı belirtmeniz gerekir. Varsayılan olarak 30 gün boyunca tutulur portaldan tetiklendiğinde.

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-to-work"></a>Kısa süre önce bazı sanal makinelerde Azure Disk Şifrelemesi'ni etkinleştirdim. Yedeklemelerim çalışmaya devam edecek mi?
Azure Backup'ın anahtar kasasına erişmek gerekli izinleri sağlamanız gerekir. İzinler açıklandığı PowerShell'de belirtin **yedeklemeyi etkinleştir** konusundaki [Azure Backup PowerShell](backup-azure-vms-automation.md) belgeleri.

### <a name="i-migrated-vm-disks-to-managed-disks-will-my-backups-continue-to-work"></a>Ben VM diskleri yönetilen disklere geçişi. Yedeklemelerim çalışmaya devam edecek mi?
Evet, yedeklemeler sorunsuz çalışır. Her şeyi yeniden yapılandırmak için gerek yoktur.

### <a name="why-cant-i-see-my-vm-in-the-configure-backup-wizard"></a>Yedeklemeyi Yapılandır Sihirbazı'nı sanal makinemi neden göremiyorum?
Sihirbaz yalnızca kasasıyla aynı bölgede Vm'leri listeler ve, zaten yedeklenmeyen.


### <a name="my-vm-is-shut-down-will-an-on-demand-or-a-scheduled-backup-work"></a>Sanal Makinem kapatılır. İsteğe bağlı veya zamanlanmış bir yedekleme iş olacak mı?
Evet. Bir makine kapatıldığında yedeklemeleri çalıştırın. Kurtarma noktası kilitlenme tutarlı olarak işaretlenir.

### <a name="can-i-cancel-an-in-progress-backup-job"></a>Devam eden yedekleme işi iptal edebilir miyim?
Evet. Yedekleme işi iptal edebilirsiniz bir **anlık görüntü alınıyor** durumu. Anlık görüntüden veri aktarımı sürüyorsa bir iş iptal edilemiyor.

### <a name="i-enabled-resource-group-lock-on-my-backed-up-managed-disk-vms-will-my-backups-continue-to-work"></a>Ben my yedeklenen yönetilen disk Vm'leri üzerinde kaynak grubu kilidi etkin. Yedeklemelerim çalışmaya devam edecek mi?
Azure Backup hizmeti, kaynak grubunu kilitlerseniz, eski geri yükleme noktalarını silemezsiniz.
- Yeni yedeklemeler, 18 geri yükleme noktaları üst sınırına olduğundan başarısız olmaya başlar.
- Yedeklemeleri kilitlendikten sonra bir iç hata ile başarısız olursa [adımları](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#clean-up-restore-point-collection-from-azure-portal) geri yükleme noktası koleksiyonu kaldırmak için.

### <a name="does-the-backup-policy-consider-daylight-saving-time-dst"></a>Yedekleme İlkesi Yaz Saati (DST) göz önünde bulundurmaz?
Hayır. Tarih ve saat yerel bilgisayarınızdaki yerel geçerli gün ışığından tasarruf uygulanır. Zamanlanmış yedeklemeler için ayarlanan saate DST nedeniyle yerel zamandan farklı olabilir.

### <a name="how-many-data-disks-can-i-attach-to-a-vm-backed-up-by-azure-backup"></a>Azure Backup tarafından yedeklenen VM'ye kaç veri diskinin ekleyebilir miyim?
Azure yedekleme ile 16 adede kadar disk sanal makinelerini yedekleyebilirsiniz. 16 diskler için destek sağlanır [anında geri yükleme](backup-instant-restore-capability.md).

### <a name="does-azure-backup-support-standard-ssd-managed-disk"></a>Yoksa Azure yedekleme desteği standart SSD yönetilen disk?
Azure Backup'ın destekledikleri [SSD standart yönetilen diskler](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/). SSD yönetilen diskler, Azure Vm'leri için yeni bir tür dayanıklı depolama sağlar. SSD yönetilen diskler için destek sağlanır [anında geri yükleme](backup-instant-restore-capability.md).

### <a name="can-we-back-up-a-vm-with-a-write-accelerator-wa-enabled-disk"></a>Yazma Hızlandırıcı WA etkinleştirilmiş bir disk sahip bir VM yedekleyebilir miyim?
Anlık görüntüleri WA özellikli diskte alınamaz. Ancak, Azure Backup hizmeti yedekleme dosyasından WA etkin disk hariç tutabilirsiniz. WA etkinleştirilmiş disklerle sanal makineler için disk dışlama, yalnızca anlık geri yüklemek için yükseltilmiş abonelikleri için desteklenir.

### <a name="i-have-a-vm-with-write-accelerator-wa-disks-and-sap-hana-installed-how-do-i-back-up"></a>Yazma Hızlandırıcı (WA) disklerle bir VM sahibim ve SAP HANA yüklenir. Nasıl yapılır? yedekleyin
Azure yedekleme WA etkin disk yedekleyemezsiniz, ancak yedeklemeden hariç tutabilirsiniz. Ancak, WA özellikli diskteki bilgileri yedeklenmediğini nedeniyle yedekleme veritabanı tutarlılığını sağlamaz. İşletim sistemi diskini yedekleme ve WA etkin olmayan disk yedekleme istiyorsanız bu yapılandırmayı disklerle yedekleyebilirsiniz.

15 dakikalık bir RPO ile bir SAP HANA yedekleme için özel bir önizleme sahibiz. SQL DB yedekleme benzer bir şekilde oluşturulmuştur ve üçüncü taraf çözümleri ile SAP HANA sertifikalı backInt arabirim kullanır. Özel olarak incelenmektedir ilgileniyorsanız adresinden bize e-posta ` AskAzureBackupTeam@microsoft.com ` konu ile **Azure vm'lerde SAP HANA yedeklemesi için özel Önizleme için kaydolun**.


## <a name="restore"></a>Geri Yükleme

### <a name="how-do-i-decide-whether-to-restore-disks-only-or-a-full-vm"></a>Yalnızca diskleri geri yükle verilip verilmeyeceğini veya tam bir VM nasıl karar verebilirim?
Bir VM geri yüklemesi için bir Azure VM hızlı oluşturma seçeneği olarak düşünün. Bu seçenek, disk adları, diskleri, genel IP adresleri ve ağ arabirimi adlarını tarafından kullanılan kapsayıcıları değiştirir. Bir VM oluşturulduğunda değişiklik benzersiz kaynakları tutar. Sanal Makineyi bir kullanılabilirlik kümesine eklenmez.

İsterseniz geri yükleme disk seçeneği kullanabilirsiniz:
  * Oluşturulan VM özelleştirin. Örnek boyutunu değiştirin.
  * Yedekleme sırasında var olmayan yapılandırma ayarları Ekle
  * Oluşturulan kaynakların adlandırma kuralını denetleme.
  * Sanal Makineyi bir kullanılabilirlik kümesine ekleyin.
  * PowerShell ya da bir şablon kullanılarak yapılandırılması gerekir her bir ayar ekleyin.  w

### <a name="can-i-restore-backups-of-unmanaged-vm-disks-after-i-upgrade-to-managed-disks"></a>Yönetilmeyen sanal makine disklerinin yedeklerini, yönetilen disklere yükselttiğimde geri yükleyebilirim?
Evet, disk alanından yönetilene geçirilen önce alınan yedeklemeler kullanabilirsiniz.
- Varsayılan olarak, bir geri yükleme VM iş yönetilmeyen VM oluşturur.
- Ancak, diskleri geri yükle ve bunları yönetilen bir sanal makine oluşturmak için kullanın.

### <a name="how-do-i-restore-a-vm-to-a-restore-point-before-the-vm-was-migrated-to-managed-disks"></a>VM'yi yönetilen disklere geçirilmeden önce nasıl bir VM için bir geri yükleme noktası geri yükleyebilirim?
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
3. Aynı ya da yeni kasaya yeniden iler hale yedekleme.

VM taşıma işleminden önce oluşturulan mevcut geri yükleme noktalarından geri yükleyebilirsiniz.
