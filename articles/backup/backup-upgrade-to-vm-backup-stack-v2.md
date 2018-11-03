---
title: Azure VM yedekleme yığını V2'ye yükseltin
description: VM yedekleme yığını için Resource Manager dağıtım modeli yükseltme işlemi ve SSS
services: backup
author: trinadhk
manager: vijayts
tags: azure-resource-manager, virtual-machine-backup
ms.service: backup
ms.topic: conceptual
ms.date: 10/3/2018
ms.author: trinadhk
ms.openlocfilehash: 20c1606d4d6a1ddd43426731e5498d1bee47f2e3
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50962546"
---
# <a name="upgrade-to-azure-vm-backup-stack-v2"></a>Azure VM yedekleme yığını v2'ye yükseltme

Resource Manager dağıtım modeli sanal makine (VM) yedekleme yığını yükseltmek için aşağıdaki özellik geliştirmeleri sağlar:

* Veri aktarımı tamamlamak beklemenize gerek kalmadan kurtarma için kullanılabilir olan bir yedekleme işi kapsamında alınan anlık görüntülere görüntüleme olanağı. Bunu geri yüklemeyi tetikleme önce kasaya kopyalamak anlık görüntüler için bekleme süresini azaltır. Ayrıca, bu özellik, ilk yedekleme dışında premium Vm'lerini yedekleme için ek depolama alanı gereksinimini ortadan kaldırır.  

* Yerel anlık görüntüleri yedi gün boyunca koruyarak yedekleme ve geri yükleme süresi kısalır.

* Disk desteği için 4 TB'a kadar boyutları.

* Yönetilmeyen bir sanal makinenin özgün depolama hesaplarına geri yüklerken kullanabilme özelliği. Depolama hesabı arasında dağıtılmış diskleri VM olsa bile bu özelliği var. Çok çeşitli sanal makine yapılandırmaları için geri yükleme işlemlerini hızlandırır.
    > [!NOTE]
    > Bu özellik, veri kurtarma noktasından sanal makinenin diskleri yerine olarak aynı değil.


## <a name="whats-changing-in-the-new-stack"></a>Yeni yığın içinde değişen nedir?
Şu anda, yedekleme işini iki aşamadan oluşur:
1.  VM anlık görünüm alınıyor.
2.  VM anlık görüntüsü, Azure Backup Kasası'na aktarma.

Yalnızca 1 ve 2 aşamalarını tamamladıktan sonra oluşturulan bir kurtarma noktası olarak kabul edilir. Anlık görüntü tamamlandıktan hemen sonra yeni yığınının bir parçası, bir kurtarma noktası oluşturulur. Aynı geri yükleme akışı kullanarak, bu kurtarma noktasından geri yükleyebilirsiniz. Bu kurtarma noktasını Azure portalında kurtarma noktası türü olarak "snapshot" kullanarak tanımlayabilirsiniz. Anlık görüntü Kasası'na aktarıldıktan sonra "anlık görüntü ve kasa." kurtarma noktası türünü değiştirir

![Yedekleme işini VM yedek yığını Resource Manager dağıtım modelinde--depolama ve kasa](./media/backup-azure-vms/instant-rp-flow.jpg)

Varsayılan olarak, anlık görüntüler yedi gün boyunca tutulur. Bu özellik, bu anlık görüntülerden daha hızlı tamamlamak geri yükleme sağlar. Bu, verileri kasadan geri müşterinin depolama hesabına kopyalamak için gereken süreyi azaltır.

## <a name="considerations-before-upgrade"></a>Yükseltmeden önce dikkat edilmesi gerekenler

* VM yedekleme yığını yükseltmesini biridir yönlü, tüm yedeklemeler bu akışına gidin. Değişiklik abonelik düzeyinde oluştuğundan, tüm sanal makineler bu akışına gidin. Tüm yeni özellik eklemeleri aynı yığınına dayalıdır. Şu anda İlkesi düzeyinde yığın denetleyemezsiniz.

* Anlık görüntüler, kurtarma noktası oluşturma artırın ve geri yükleme işlemlerini hızlandırmak için yerel olarak depolanır. Sonuç olarak, yedi günlük süre içinde alınan anlık görüntülere karşılık gelen depolama maliyetini görürsünüz.

* Artımlı anlık görüntüleri, sayfa blobları depolanır. Yönetilmeyen diskler kullanan tüm müşteriler, Müşteri'nin yerel bir depolama hesabında depolanan anlık görüntüleri yedi gün için ücretlendirilirsiniz. Yönetilen diskler için yönetilen VM yedeklemeleri tarafından kullanılan geri yükleme noktası koleksiyonları temel alınan depolama düzeyinde blob anlık görüntüleri kullandığından, karşılık gelen maliyetleri görürsünüz [blob anlık görüntüsü fiyatlandırma](https://docs.microsoft.com/rest/api/storageservices/understanding-how-snapshots-accrue-charges) ve artımlı olmalıdır.

* VM arttığında bir anlık görüntü kurtarma noktasından geri yüklerseniz, geçici bir depolama konumu VM oluşturulurken kullanılır.

* Premium depolama hesapları için anında kurtarma noktalarının sayısı 10 TB sınırını doğrultusunda için alınan anlık görüntülere ayrılmış alanı.

> [!NOTE]
> Azure Backup destek almak için Azure VM yedekleme yığını v2'ye yükseltme [SSD standart yönetilen diskler](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/).

## <a name="upgrade"></a>Yükseltme
### <a name="the-azure-portal"></a>Azure portal
Azure portalını kullanıyorsanız, kasa panosunda bir bildirim görür. Bu bildirim, büyük disk desteği ve yedekleme ve geri yükleme hızı geliştirmeleri ilişkilendirir. Alternatif olarak yükseltme seçeneği almak için kasa özellikleri sayfasına gidebilirsiniz.

![Yedekleme işini VM yedek yığını Resource Manager dağıtım modelinde--destek bildirimi](./media/backup-azure-vms/instant-rp-banner.png)

Yeni yığına yükseltmek için bir ekranı açmak için başlığı seçin.

![VM yedekleme yığını Resource Manager dağıtım modeli--yedekleme işinde yükseltme](./media/backup-azure-vms/instant-rp.png)

### <a name="powershell"></a>PowerShell
Yükseltilmiş bir PowerShell üzerinden terminal aşağıdaki cmdlet'leri çalıştırın:
1.  Azure hesabınızda oturum açın:

    ```
    PS C:> Connect-AzureRmAccount
    ```

2.  Kaydetmek istediğiniz aboneliği seçin:

    ```
    PS C:>  Get-AzureRmSubscription –SubscriptionName "Subscription Name" | Select-AzureRmSubscription
    ```

3.  Bu abonelik kaydedin:

    ```
    PS C:>  Register-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" –ProviderNamespace Microsoft.RecoveryServices
    ```

## <a name="verify-that-the-upgrade-is-finished"></a>Yükseltme bittikten doğrulayın
Yükseltilmiş bir PowerShell terminalden aşağıdaki cmdlet'i çalıştırın:

```
Get-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" –ProviderNamespace Microsoft.RecoveryServices
```

"Kaydedildi" olarak görünüyorsa, aboneliğiniz VM yedek yığını Resource Manager dağıtım modeli için yükseltildi.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

Forumlar ve müşteri soruları aşağıdaki sorular ve yanıtlar toplanana.

### <a name="will-upgrading-to-v2-impact-current-backups"></a>V2'ye yükseltme geçerli yedeklemeleri etkileyecek?
V2'ye yükseltirseniz, geçerli yedeklemeleriniz için herhangi bir etkisi ve ortamınızı yeniden gerek yoktur. Yükseltme ve yedekleme ortamınız, içerdiğinden iş devam eder.

### <a name="what-does-it-cost-to-upgrade-to-azure-vm-backup-stack-v2"></a>Azure VM yedekleme yığını v2'ye yükseltmek için maliyeti nedir?
Yığını v2'ye yükseltmek için herhangi bir maliyet yoktur. Anlık görüntüler, kurtarma noktası oluşturma hızlandırmak ve geri yükleme işlemleri için yerel olarak depolanır. Sonuç olarak, yedi günlük süre içinde alınan anlık görüntülere karşılık gelen depolama maliyetini görürsünüz.

### <a name="does-upgrading-to-stack-v2-increase-the-premium-storage-account-snapshot-limit-by-10-tb"></a>V2 yığın yükseltme, 10 TB premium depolama hesabı anlık görüntü sınırı artırmak mu?
Hayır, depolama hesabının hala bırakılmalıdır 10TB başına toplam anlık görüntü sınırı.

### <a name="in-premium-storage-accounts-do-snapshots-taken-for-instant-recovery-point-occupy-the-10-tb-snapshot-limit"></a>Premium depolama hesaplarında, 10 TB anlık görüntü sınırı için anında kurtarma noktası alınan anlık görüntülere kaplayabilir?
Evet, premium depolama hesapları için anında kurtarma noktası için alınan anlık görüntülere kaplayabilir ayrılmış 10 TB alan.

### <a name="how-does-the-snapshot-work-during-the-seven-day-period"></a>Anlık görüntü yedi günlük süre içinde nasıl çalışır?
Her gün yeni bir anlık görüntü alınır. Yedi bireysel anlık görüntüleri vardır. Hizmetten **değil** ilk gününde bir kopyasını alın ve sonraki altı gündür değişiklikleri ekleyin.

### <a name="is-a-v2-snapshot-an-incremental-snapshot-or-full-snapshot"></a>V2 bir anlık görüntü, bir artımlı anlık görüntü veya tam bir anlık görüntü mi?
Artımlı anlık yönetilmeyen diskler için kullanılır. Yönetilen diskler için Azure Backup kullanan tarafından oluşturulan koleksiyon blob anlık görüntüleri ve bu nedenle, artımlı bir noktaya geri yükleme.

### <a name="how-to-get-standard-ssd-managed-disk-support-for-a-virtual-machine"></a>Standart SSD almak nasıl bir sanal makine için disk desteği yönetiliyor?
Azure Backup destek almak için Azure VM yedekleme yığını v2'ye yükseltme [SSD standart yönetilen diskler](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/).
