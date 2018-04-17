---
title: Azure VM yedekleme yığını V2'ye yükseltin | Microsoft Docs
description: VM yedekleme yığını V2 yükseltme işlemi ve sık sorulan sorular
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
ms.openlocfilehash: 4bdbf48030dda18e6698a7731989ec2de2319b35
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="upgrade-to-vm-backup-stack-v2"></a>VM yedek yığını V2’ye yükseltme
Sanal makine (VM) yedekleme yığını V2 yükseltme aşağıdaki özellik geliştirmeleri sağlar:
* Veri aktarımı için beklemeden kurtarma için kullanılabilir olması için yedekleme işinin parçası olarak alınan anlık görüntü görmek yeteneği.
Anlık görüntü geri yükleme tetiklemeden kasa kopyalanması için bekleme azaltır. Ayrıca, bu ilk yedekleme dışında premium sanal makineleri yedeklemek için ek depolama alanı gereksinimini ortadan kaldırır.  

* Anlık görüntüler için yerel olarak 7 gün koruyarak yedekleme ve geri yükleme sürelerini düşüş. 

* Disk desteği en fazla 4 TB boyutları.  

* (VM depolama hesaplarında dağıtılmış diskleri olsa bile) özgün depolama hesapları kullandığınızda yeteneği bir geri yükleme yönetilmeyen bir VM'nin yapılıyor. Bu geri yüklemeler daha hızlı çok çeşitli VM yapılandırmaları için yapar. 
    > [!NOTE] 
    > Bu orijinal VM geçersiz kılma olarak aynı değildir. 
    > 
    >

## <a name="what-is-changing-in-the-new-stack"></a>Yeni yığınında ne değişiyor?
Bugün, yedekleme işi iki aşamadan oluşur:
1.  VM anlık görüntü almak. 
2.  VM anlık görüntü Azure yedekleme Kasası'na aktarılıyor. 

Aşama 1 ve 2 yalnızca tamamladıktan sonra oluşturulmuş bir kurtarma noktası olarak kabul edilir. Anlık görüntü tamamlandıktan hemen sonra yeni yığını bir parçası olarak, bir kurtarma noktası oluşturulur. Ayrıca, aynı geri yükleme akış kullanarak bu kurtarma noktasından geri yükleyebilirsiniz. Bu kurtarma noktasını "anlık görüntü" kurtarma noktası türü olarak kullanarak Azure portalında tanımlayabilirsiniz. Anlık görüntü kasa transfer sonra "Anlık görüntü ve kasaya" kurtarma noktası türünü değiştirir. 

![VM yedekleme yığınında V2 yedekleme işi](./media/backup-azure-vms/instant-rp-flow.jpg) 

Varsayılan olarak, anlık görüntüleri yedi gün boyunca saklanır. Bu müşteri depolama hesabına geri kasadan veri kopyalamak için gerekli olan zamanı azaltarak bu anlık görüntülerden daha hızlı tamamlanması geri yükleme sağlar. 

## <a name="considerations-before-upgrade"></a>Yükseltmeden önce konuları
* Tek yönlü yükseltme VM yedekleme yığınının budur. Bu nedenle, gelecekteki tüm yedeklemeler bu akışına geçer. Bu yana **abonelik düzeyinde etkinleştirildiğinde, tüm sanal makineleri bu akışı gider**. Tüm yeni özellik eklemeler aynı yığında dayalı olarak belirlenir. Bu ilke düzeyinde gelecekte geliyor denetleme olanağı serbest bırakır. 
* Premium diskler, ilk yedekleme sırasında olan VM'ler için ilk yedekleme tamamlanana kadar depolama alanı boyutu eşdeğer VM depolama hesabında kullanılabilir olduğundan emin olun. 
* Anlık görüntüler yerel olarak kurtarma noktası oluşturma artırmak ve geri yükleme hızlandırmak için depolandığından, anlık görüntüler için yedi günlük dönem boyunca karşılık gelen depolama maliyetleri görürsünüz.
* Artımlı anlık görüntüleri, sayfa blobları depolanır. Tüm yönetilmeyen diskleri kullanan müşteriler 7 gün anlık görüntüler için sizden ücret alınır, Müşteri'nin yerel depolama hesabında depolanır. Geçerli fiyatlandırma modeli göredir yönetilen disklerde müşteriler için hiçbir ücret yoktur.
* Premium VM için anlık görüntü kurtarma noktasından geri yükleme yapıyorsanız, geri yüklemenin bir parçası olarak VM oluşturulurken kullanılan geçici depolama konumu görürsünüz. 
* Premium depolama hesapları durumunda anlık kurtarma için alınan anlık görüntü premium bir depolama hesabında ayrılan 10 TB alanı kaplar.

## <a name="how-to-upgrade"></a>Yükseltme için nasıl?
### <a name="the-azure-portal"></a>Azure portalı
Azure portalını kullanarak kullanıyorsanız, büyük disk desteği ile ilgili kasa panosunda bir bildirim görür ve yedekleme ve hız artırmaları geri yükleme.

![VM yedekleme yığınında V2 yedekleme işi](./media/backup-azure-vms/instant-rp-banner.png) 

Yeni yığını yükseltme ekranını açmak için başlık seçin. 

![VM yedekleme yığınında V2 yedekleme işi](./media/backup-azure-vms/instant-rp.png) 

### <a name="powershell"></a>PowerShell
Aşağıdaki cmdlet bir yükseltilmiş PowerShell terminal yürütün:
1.  Azure hesabınızda oturum açın. 

```
PS C:> Login-AzureRmAccount
```

2.  Önizleme için kaydetmek istediğiniz aboneliği seçin:

```
PS C:>  Get-AzureRmSubscription –SubscriptionName "Subscription Name" | Select-AzureRmSubscription
```

3.  Bu abonelik özel önizlemesi için kaydolun:

```
PS C:>  Register-AzureRmProviderFeature -FeatureName “InstantBackupandRecovery” –ProviderNamespace Microsoft.RecoveryServices
```

## <a name="verify-whether-the-upgrade-is-complete"></a>Yükseltme tamamlandıktan olup olmadığını doğrulayın
Yükseltilmiş bir PowerShell terminal durumundan aşağıdaki cmdlet'i çalıştırın:

```
Get-AzureRmProviderFeature -FeatureName “InstantBackupandRecovery” –ProviderNamespace Microsoft.RecoveryServices
```

Kayıtlı diyorsa, aboneliğinizi VM yedekleme yığını V2 yükseltilir. 



