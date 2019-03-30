---
title: Azure anında geri yükleme özelliği
description: Yedekleme yığını, Resource Manager dağıtım modelinde Azure anında geri yükleme özelliği ve VM için sık sorulan sorular
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: sogup
ms.openlocfilehash: 1f96c47e993e9b3d123972aba8eefc54b1d5cdfa
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58652680"
---
# <a name="get-improved-backup-and-restore-performance-with-azure-backup-instant-restore-capability"></a>Geliştirilmiş yedeği almak ve performansı Azure Backup anında geri yükleme özelliğine sahip geri yükleme

> [!NOTE]
> Biz yeniden adlandırma kullanıcılar görüşlerine dayalı **VM yedek yığını V2** için **anında geri yükleme** Karışıklığı önlemek için Azure Stack işlevsellikle azaltmak için.<br/><br/> Tüm Azure yedekleme kullanıcılar için artık yükseltilmiş **anında geri yükleme**.

Yeni model anlık geri yüklemek için aşağıdaki özellik geliştirmeleri sağlar:

* Veri aktarımı tamamlamak için kasaya beklemeden kurtarma için kullanılabilir olan bir yedekleme işi kapsamında alınan anlık görüntülere kullanabilme özelliği. Bunu geri yüklemeyi tetikleme önce kasaya kopyalamak anlık görüntüler için bekleme süresini azaltır.
* Varsayılan olarak iki gün için yerel anlık görüntüleri koruyarak yedekleme ve geri yükleme süresi kısalır. Bu varsayılan kasa 1-5 gün arasında herhangi bir değer için yapılandırılabilir.
* 4 TB'a kadar destekler disk boyutları.
* Standart SSD disk yanı sıra diskleri HDD standart ve Premium SSD diskleri destekler.
*   Yönetilmeyen bir sanal makinenin özgün depolama hesaplarına (disk başına), kullanma yeteneğini geri yüklerken. Depolama hesabı arasında dağıtılmış diskleri VM olsa bile bu özelliği var. Çok çeşitli sanal makine yapılandırmaları için geri yükleme işlemlerini hızlandırır.


## <a name="whats-new-in-this-feature"></a>Bu özelliği yenilikler nelerdir?

Şu anda, yedekleme işini iki aşamadan oluşur:

1.  VM anlık görünüm alınıyor.
2.  VM anlık görüntüsü, Azure kurtarma Hizmetleri Kasası'na aktarma.

Aşama 1 ve 2 yalnızca tamamlandıktan sonra oluşturulan bir kurtarma noktası olarak kabul edilir. Bu yükseltme kapsamında, bu kurtarma noktası anlık görüntü türü, aynı geri yükleme akışı kullanarak bir geri yüklemeyi gerçekleştirmek için kullanılabilir ve anlık görüntü tamamlandıktan hemen sonra bir kurtarma noktası oluşturulur. Kurtarma noktası türü olarak "snapshot"'ı kullanarak Azure portalında bu kurtarma noktasını tanımlayabilirsiniz ve anlık görüntü Kasası'na aktarıldıktan sonra "anlık görüntü ve kasa" kurtarma noktası türünü değiştirir.

![Yedekleme işini VM yedek yığını Resource Manager dağıtım modelinde--depolama ve kasa](./media/backup-azure-vms/instant-rp-flow.png)

Varsayılan olarak, iki gün için anlık görüntüleri korunur. Bu özellik, geri yükleme sürelerini keserek geri yükleme işlemi var. Bu anlık görüntülerden sağlar. Bu, dönüştürme ve verileri kasadan kopyalama için gereken süreyi azaltır.

## <a name="feature-considerations"></a>Özellik konuları

* Anlık görüntüler, disk kurtarma noktası oluşturma artırın ve geri yükleme işlemlerini hızlandırmak için birlikte depolanır. Sonuç olarak, bu süre boyunca alınan anlık görüntülere karşılık gelen depolama maliyetini görürsünüz.
* Artımlı anlık görüntüleri, sayfa blobları depolanır. Yönetilmeyen diskler kullanan tüm kullanıcılar, kendi yerel depolama hesabında depolanan anlık görüntüler için ücretlendirilir. Yönetilen VM yedeklemeleri tarafından kullanılan geri yükleme noktası koleksiyonları temel alınan depolama düzeyinde blob anlık görüntüleri kullandığından, yönetilen diskler için anlık görüntü fiyatlandırma blob karşılık gelen maliyetleri görürsünüz ve bunların artımlı.
* Premium depolama hesapları için anında kurtarma noktalarının sayısı 10 TB sınırını doğrultusunda için alınan anlık görüntülere ayrılmış alanı.
* Geri yükleme gereksinimlerini temel alan anlık görüntü saklama yapılandırma yeteneği sahip olursunuz. Gereksinim bağlı olarak, anlık görüntü saklama en az bir gün aşağıda açıklandığı gibi yedekleme İlkesi dikey penceresinde ayarlayabilirsiniz. Bu, sık geri yükleme gerçekleştirme, anlık görüntü saklama için maliyet tasarruf etmenize yardımcı olabilir.
* Bu kez anında geri yükleme için yükseltilmiş tek yönlü bir yükseltme, geri dönemezsiniz.

>[!NOTE]
>Yükseltmeden sonra tüm müşterilere ait anlık görüntü saklama süresi ile bu anında geri yükleme (**yeni ve mevcut her ikisi de dahil**) iki gün varsayılan değerine ayarlanır. Ancak, gereksinim 1-5 gün arasında herhangi bir değere göre süresini ayarlayabilirsiniz.

## <a name="cost-impact"></a>Maliyet etkisi

Artımlı anlık anında kurtarma için kullanılan sanal makinenin depolama hesabında depolanır. Artımlı anlık görüntü, bir anlık görüntü tarafından kaplanan alanı anlık görüntü oluşturulduktan sonra yazılan sayfa kapladığı alanı eşittir anlamına gelir. Faturalandırma hala içindir anlık görüntü ve GB başına fiyat kapladığı kullanılan GB başına belirtildiği gibi aynı [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/managed-disks/).

>[!NOTE]
> Anlık görüntü saklama, haftalık ilkeleri için 5 gün olarak sabitlenmiştir.

## <a name="configure-snapshot-retention"></a>Anlık görüntü saklama yapılandırma

### <a name="using-azure-portal"></a>Azure portalını kullanma

Azure portalında, eklenen bir alan gördüğünüz **VM yedekleme İlkesi** altındaki dikey penceresinde **anında geri yükleme** bölümü. Anlık görüntü saklama süresinden değiştirebilirsiniz **VM yedekleme İlkesi** dikey penceresinde tüm sanal makineler için belirli bir yedekleme ilkesiyle ilişkili.

![Anında geri yükleme özelliği](./media/backup-azure-vms/instant-restore-capability.png)

### <a name="using-powershell"></a>PowerShell’i kullanma

>[!NOTE]
> Az Powershell'den sürüm 1.6.0 ve sonraki sürümlerde, PowerShell kullanarak ilkesinde anında geri yükleme anlık görüntü saklama süresi güncelleştirebilirsiniz.

```powershell
PS C:\> $bkpPol = Get-AzureRmRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureVM"
$bkpPol.SnapshotRetentionInDays=5
PS C:\> Set-AzureRmRecoveryServicesBackupProtectionPolicy -policy $bkpPol
```
Her ilke için varsayılan anlık görüntü saklama 2 gün olarak ayarlanır. Kullanıcı, en az 1 ve en fazla 5 gün değeri değiştirebilirsiniz. Haftalık ilkeleri için anlık görüntü saklama 5 gün için sabit.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="what-are-the-cost-implications-of-instant-restore"></a>Anında geri yükleme maliyet etkileri nelerdir?
Anlık görüntüler, kurtarma noktası oluşturma hızlandırmak ve geri yükleme işlemleri için diskleri ile birlikte saklanır. Sonuç olarak, VM yedekleme ilkesinin bir parçası olarak seçilen anlık görüntü saklama karşılık gelen depolama maliyetini görürsünüz.

### <a name="in-premium-storage-accounts-do-the-snapshots-taken-for-instant-recovery-point-occupy-the-10-tb-snapshot-limit"></a>Premium depolama hesaplarında, 10 TB anlık görüntü sınırı için anında kurtarma noktası alınan anlık görüntülere kaplayabilir?
Evet, premium depolama hesapları için anında kurtarma noktası için alınan anlık görüntülere 10 TB ayrılmış anlık görüntü alanı kaplar.

### <a name="how-does-the-snapshot-retention-work-during-the-five-day-period"></a>Anlık görüntü saklama beş günlük süre içinde nasıl çalışır?
Eğer beş ayrı artımlı anlık her gün yeni bir anlık görüntü, alınır. Çoğu durumda yaklaşık 2-%7 veri değişim sıklığı anlık görüntü boyutuna bağlıdır.

### <a name="is-an-instant-restore-snapshot-an-incremental-snapshot-or-full-snapshot"></a>Anlık görüntü anlık geri bir artımlı anlık görüntü veya tam bir anlık görüntü mi?
Anında geri yükleme özelliği bir parçası olarak alınan anlık görüntülere artımlı anlık görüntüleridir.

### <a name="how-can-i-calculate-the-approximate-cost-increase-due-to-instant-restore-feature"></a>Anında geri yükleme özelliğine nedeniyle maliyet artışı nasıl hesaplar?
Bu VM üzerindeki karmaşıklığı bağlıdır. Bir kararlı durumda, kabul edilebilir maliyet artışı anlık görüntü saklama dönemi günlük değişim sıklığı GB başına VM depolama maliyeti =.

### <a name="if-the-recovery-type-for-a-restore-point-is-snapshot-and-vault-and-i-perform-a-restore-operation-which-recovery-type-will-be-used"></a>Kurtarma için bir geri yükleme noktası "Anlık görüntü ve kasa" türdür ve bir geri yükleme işlemi, hangi kurtarma türünü kullanılacak mı?
Kurtarma türünü "anlık görüntü ve kasa" ise, kasadan bitti geri yüklemeyi çok daha hızlı Karşılaştırılacak yerel anlık görüntüden geri yükleme otomatik olarak gerçekleştirilir.

### <a name="what-happens-if-i-select-retention-period-of-restore-point-tier-2-less-than-the-snapshot-tier1-retention-period"></a>Geri yükleme noktası (Katman 2) (Katman1) anlık görüntü saklama süresinden daha az bekletme süresi seçtiğim ne olur?
Yeni model (Katman1) anlık görüntüsü silinir sürece geri yükleme noktası (Katman2) silinmesine izin vermiyor. Geri yükleme noktası (Katman2) saklama süresi için anlık görüntü saklama süresinden daha fazla zamanlama öneririz.

### <a name="why-is-my-snapshot-existing-even-after-the-set-retention-period-in-backup-policy"></a>Neden benim anlık görüntü mevcut sonra bile ayarlanan Bekletme dönemi yedekleme ilkesinde?
Kurtarma noktası anlık görüntü ve kullanılabilir en son RP olan, bir sonraki başarılı yedekleme zamana kadar bekletilir. Sanal makinede bir sorun nedeniyle hakkında daha fazla tüm yedeklemeler başarısız durumunda her zaman mevcut olacak şekilde en az bir en son RP taahhütlerin tasarlanmış GC ilkeyi hemen göre budur. Normal senaryolarda RPs Temizlenen en fazla 24 saat sonra kendi süre sonu içinde.
