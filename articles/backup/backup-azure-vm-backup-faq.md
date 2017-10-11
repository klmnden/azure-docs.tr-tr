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
ms.openlocfilehash: 1372a9e05cb47f6c68240bffccd46b0fbebb5464
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="questions-about-the-azure-vm-backup-service"></a>Azure VM Yedeklemesi hizmetiyle ilgili sorular
Bu makalede Azure VM Yedeklemesi bileşenlerini kısa süre içinde anlamanıza yardımcı olacak yaygın soruların yanıtları bulunur. Bazı yanıtlarda, kapsamlı bilgiler içeren makalelerin bağlantıları vardır. Ayrıca Azure Backup hizmeti ile ilgili sorularınızı [tartışma forumunda](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup) paylaşabilirsiniz.

## <a name="configure-backup"></a>Yedeklemeyi yapılandırma
### <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Kurtarma Hizmetleri kasaları, klasik VM’leri mi Resource Manager tabanlı VM’leri mi destekler? <br/>
Kurtarma Hizmetleri kasaları iki modeli de destekler.  Klasik portalda oluşturulan bir klasik VM’yi ya da Azure portalında oluşturulan bir Resource Manager VM’sini bir Kurtarma Hizmetleri kasasına yedekleyebilirsiniz.

### <a name="what-configurations-are-not-supported-by-azure-vm-backup-"></a>Azure VM yedeklemesinde hangi yapılandırmalar desteklenmez?
Lütfen [Desteklenen işletim sistemleri](backup-azure-arm-vms-prepare.md#supported-operating-system-for-backup) ve [VM yedeklemesinin sınırları](backup-azure-arm-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm) konularını inceleyin.

### <a name="why-cant-i-see-my-vm-in-configure-backup-wizard"></a>Yedeklemeyi yapılandırma sihirbazında sanal makinemi neden göremiyorum?
Yedeklemeyi yapılandırma sihirbazında, Azure Backup yalnızca şu sanal makineleri listeler:
* Zaten korumalı olmayan - Sanal Makine dikey penceresine gidip pencerenin Ayarlar menüsünde Yedekleme durumunu denetleyerek sanal makinenin yedekleme durumunu doğrulayabilirsiniz. [Sanal makinenin yedekleme durumunu denetleme](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-vm-management-blade) hakkında daha fazla bilgi edinin.
* Sanal makine ile aynı bölgeye ait olan

## <a name="backup"></a>Backup
### <a name="will-on-demand-backup-job-follow-same-retention-schedule-as-scheduled-backups"></a>İsteğe bağlı yedekleme işi zamanlanmış yedeklemelerle aynı bekletme zamanlamasına mı uyar?
Hayır. İsteğe bağlı yedekleme işinde bekletme aralığını belirtmeniz gerekir. Varsayılan olarak, portaldan tetiklendiğinde 30 gün bekletilir. 

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-to-work"></a>Kısa süre önce bazı sanal makinelerde Azure Disk Şifrelemesi'ni etkinleştirdim. Yedeklemelerim çalışmaya devam edecek mi?
Azure Backup hizmetinin Key Vault'a erişmesi için izin vermeniz gerekir. [PowerShell](backup-azure-vms-automation.md) belgelerinin *Yedeklemeyi Etkinleştirme* bölümünde belirtilen adımları kullanarak, PowerShell'de bu izinleri sağlayabilirsiniz.

### <a name="i-migrated-disks-of-a-vm-to-managed-disks-will-my-backups-continue-to-work"></a>Bir sanal makinenin disklerini yönetilen disklere geçirdim. Yedeklemelerim çalışmaya devam edecek mi?
Evet, yedeklemeler sorunsuz çalışır; yedeklemeyi yeniden yapılandırmak gerekmez. 

## <a name="restore"></a>Geri Yükleme
### <a name="how-do-i-decide-between-restoring-disks-versus-full-vm-restore"></a>Diskleri geri yüklemekle tam sanal makine geri yüklemesi yapmak arasında nasıl seçim yapabilirim?
Azure tam sanal makine geri yüklemesini, geri yüklenmiş sanal makine için hızlı oluşturma seçeneği sağlayan bir yol olarak düşünün. Sanal makineyi geri yükleme seçeneği, sanal makinenin bir parçası olarak oluşturulan kaynakların benzersiz olmasını sağlamak için disklerin adını, diskler tarafından kullanılan kapsayıcıları, genel IP adreslerini ve ağ arabirimi adlarını değiştirir. Ayrıca sanal makineyi kullanılabilirlik kümesine eklemez. 

Aşağıdakileri yapmak için diskleri geri yükleme seçeneğini kullanın:
* Yedekleme yapılandırmasından boyutu değiştirme gibi belirli bir noktadaki yapılandırmayla oluşturulan sanal makineleri özelleştirme
* Yedekleme sırasında var olan yapılandırmalar ekleme 
* Oluşturulan kaynakların adlandırma kuralını denetleme
* Kullanılabilirlik kümesine sanal makine ekleme
* Yalnızca PowerShell/bildirim temelli bir şablon tanımı kullanılarak gerçekleştirilebilen bir yapılandırmanız olması durumunda

## <a name="manage-vm-backups"></a>VM yedeklemelerini yönetme
### <a name="what-happens-when-i-change-a-backup-policy-on-vms"></a>Sanal makinelerde yedekleme ilkesini değiştirdiğimde ne olur?
Sanal makinelere yeni bir ilke uygulandığında, yeni ilkenin zamanlama ve bekletme ayarları geçerli olur. Bekletme süresi uzatıldıysa, yeni ilkeye göre tutulması için mevcut kurtarma noktaları işaretlenir. Bekletme süresi kısaltıldıysa, bunlar sonraki temizleme işleminde kesilmek üzere işaretlenir ve sonra silinir. 
