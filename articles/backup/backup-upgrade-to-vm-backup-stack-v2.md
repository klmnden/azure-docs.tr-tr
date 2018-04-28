---
title: Azure VM yedekleme yığını için Azure Resource Manager dağıtım modeline yükseltme | Microsoft Docs
description: VM yedekleme yığını için Resource Manager dağıtım modeli yükseltme işlemi ve sık sorulan sorular
services: backup, virtual-machines
documentationcenter: ''
author: trinadhk
manager: vijayts
tags: azure-resource-manager, virtual-machine-backup
ms.assetid: ''
ms.service: backup, virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 03/08/2018
ms.author: trinadhk, sogup
ms.openlocfilehash: 224cd365e6b3ca4fd963b530dbaa289b763d53ee
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="upgrade-to-the-azure-resource-manager-deployment-model-for-azure-vm-backup-stack"></a>Azure VM yedekleme yığını için Azure Resource Manager dağıtım modeline yükseltme
Resource Manager dağıtım modeli için sanal makine (VM) yedekleme yığını yükseltme aşağıdaki özellik geliştirmeleri sağlar:
* Veri aktarımı bitmesini beklemeden kurtarma için kullanılabilir olan bir yedekleme işinin parçası olarak alınan anlık görüntülerini görmek yeteneği. Geri yükleme tetiklemeden önce kasaya kopyalamak anlık görüntüler için bekleme süresini azaltır. Ayrıca, bu özelliği ilk yedek dışında premium sanal makineleri yedeklemek için ek depolama alanı gereksinimini ortadan kaldırır.  

* Yerel olarak yedi gün için anlık görüntü koruyarak yedekleme ve geri yükleme sürelerini düşüş.

* Disk desteği en fazla 4 TB boyutları.

* Yönetilmeyen bir sanal makinenin özgün depolama hesapları geri yüklerken kullanma olanağı. VM depolama hesaplarında dağıtılmış diskleri olsa bile bu özelliği mevcut. Geri yüklemeler daha hızlı çok çeşitli VM yapılandırmaları için kolaylaştırır.
    > [!NOTE] 
    > Bu özelliği geçersiz kılma orijinal VM ile aynı değil. 
    >

## <a name="whats-changing-in-the-new-stack"></a>Yeni yığınında ne değişiyor?
Şu anda, yedekleme işi iki aşamadan oluşur:
1.  VM anlık görünüm alınıyor. 
2.  VM anlık Azure yedekleme Kasası'na aktarılıyor. 

Aşama 1 ve 2 yalnızca tamamladıktan sonra oluşturulmuş bir kurtarma noktası olarak kabul edilir. Anlık görüntü tamamlandıktan hemen sonra yeni yığını bir parçası olarak, bir kurtarma noktası oluşturulur. Aynı geri yükleme akış kullanarak, bu kurtarma noktasından geri yükleyebilirsiniz. Kurtarma noktası türü olarak "anlık görüntü" kullanarak, bu kurtarma noktasını Azure portalında tanımlayabilirsiniz. Anlık görüntü kasaya aktarıldıktan sonra "anlık görüntü ve kasa." kurtarma noktası türünü değiştirir 

![VM yedekleme yığını Resource Manager dağıtım modelinde--depolama ve kasa yedekleme işi](./media/backup-azure-vms/instant-rp-flow.jpg) 

Varsayılan olarak, anlık görüntüleri yedi gün boyunca tutulur. Bu özellik, bu anlık görüntülerden daha hızlı tamamlanması geri yüklemeyi sağlar. Verileri geri kasadan müşterinin depolama hesabına kopyalamak için gerekli olan süreyi azaltır. 

## <a name="considerations-before-upgrade"></a>Yükseltmeden önce konuları
* VM yedekleme yığınının bir yükseltmedir yön. Bu nedenle tüm yedeklemeler bu akışına gidin. Abonelik düzeyinde etkinleştirilmiş olduğundan, tüm sanal makineleri bu akışına gidin. Tüm yeni özellik eklemeler aynı yığında temel alır. Bu ilke düzeyinde gelecekte geliyor denetleme olanağı serbest bırakır.

* VM premium diskler, yedekleme sırasında ve ilk kadar tamamlandıktan, depolama hesabında yeterli depolama alanı olduğundan emin olun. VM boyutuna eşit olması gerekir.

* Anlık görüntüler, kurtarma noktası oluşturma artırmak ve geri yükleme hızlandırmak için yerel olarak depolanır. Bu nedenle, anlık görüntüler için yedi günlük dönem boyunca karşılık gelen depolama maliyetleri bakın.

* Artımlı anlık görüntüleri, sayfa blobları depolanır. Yönetilmeyen diskler kullanan tüm müşterilerin anlık görüntüleri Müşteri'nin yerel depolama hesabında depolanan yedi gün için sizden ücret kesilir. Geçerli fiyatlandırma modeli göre yönetilen disklerde müşteriler için hiçbir ücret yoktur.

* Premium VM için bir anlık görüntü kurtarma noktasından geri yükleme yaparsanız, geri yüklemenin bir parçası olarak VM oluşturulurken kullanılan bir geçici depolama alanı konumunda bakın.

* Premium depolama hesapları için hızlı kurtarma için gerçekleştirilecek anlık görüntüleri 10 TB ayrılmış alanı kaplar.

## <a name="upgrade"></a>Yükseltme
### <a name="the-azure-portal"></a>Azure portalı
Azure Portalı'nı kullanırsanız, kasa Panosu üzerinde bir bildirim görür. Bu bildirim büyük disk desteğini ve yedekleme ve geri yükleme hız artırmaları ilgilidir.

![VM yedekleme yığını Resource Manager dağıtım modelinde--destek bildirim yedekleme işi](./media/backup-azure-vms/instant-rp-banner.png) 

Yeni yığını yükseltme ekranını açmak için başlık seçin. 

![Yükseltme VM yedekleme yığınında Resource Manager dağıtım modeli--yedekleme işi](./media/backup-azure-vms/instant-rp.png) 

### <a name="powershell"></a>PowerShell
Bir yükseltilmiş PowerShell terminal aşağıdaki cmdlet'leri çalıştırın:
1.  Azure hesabınızda oturum açın: 

    ```
    PS C:> Connect-AzureRmAccount
    ```

2.  Önizleme için kaydetmek istediğiniz aboneliği seçin:

    ```
    PS C:>  Get-AzureRmSubscription –SubscriptionName "Subscription Name" | Select-AzureRmSubscription
    ```

3.  Bu abonelik özel önizlemesi için kaydolun:

    ```
    PS C:>  Register-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" –ProviderNamespace Microsoft.RecoveryServices
    ```

## <a name="verify-that-the-upgrade-is-finished"></a>Yükseltme tamamlandığını doğrulayın
Yükseltilmiş bir PowerShell terminal durumundan aşağıdaki cmdlet'i çalıştırın:

```
Get-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" –ProviderNamespace Microsoft.RecoveryServices
```

"Kaydedildi" diyorsa, aboneliğinizi VM yedekleme yığını Resource Manager dağıtım modeline yükseltilir.
