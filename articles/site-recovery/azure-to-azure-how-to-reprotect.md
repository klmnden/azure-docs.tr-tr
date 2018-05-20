---
title: Yeniden koruma başarısız oldu Azure Vm'leri üzerinde Azure Site Recovery Birincil Azure bölgesiyle dön | Microsoft Docs
description: Azure Site Recovery kullanan bir birincil bölge'ndan yük devretme sonrasında Azure Vm'lerinin ikincil bir bölgede koruyun açıklar.
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 05/15/2018
ms.author: rajanaki
ms.openlocfilehash: ccec4262297314bad261a852bb5db25c428ce0a0
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="reprotect-failed-over-azure-vms-to-the-primary-region"></a>Yeniden koruma birincil bölge Azure Vm'leri üzerinde başarısız oldu


>[!NOTE]
>
> Azure VM’ler için Site Recovery çoğaltma şu anda önizlemede.



Olduğunda, [yük devri](site-recovery-failover.md) bir bölge başka bir kullanarak Azure VM'lerin [Azure Site Recovery](site-recovery-overview.md), korumasız bir durumda ikincil bölge içindeki VM'ler önyüklemesi. Başarısız birincil bölge VM'ler yedeklerseniz, aşağıdakileri yapmanız gerekir:

- Böylece birincil bölge çoğaltmak başlatmaları ikincil bölge içindeki Vm'leri koruyun. 
- Yükü tamamlandıktan ve sanal makineleri çoğaltmak sonra bunları üzerinden ikincil birincil bölge başarısız olabilir.

> [!WARNING]
> Seçtiğiniz varsa [geçirilen](migrate-overview.md#what-do-we-mean-by-migration) birincil makinelerden ikincil bölge VM başka bir kaynak grubuna taşınmış veya Azure VM silinmiş, VM koruyun veya geri dönecek değiştiremezsiniz.


## <a name="prerequisites"></a>Önkoşullar
1. Birincil VM yük devretmeyi ikincil bölge için kaydedilmiş olmalıdır.
2. Birincil hedef sitede kullanılabilir olması gerekir ve erişmek veya bu bölgede kaynak oluşturmak mümkün olması gerekir.

## <a name="reprotect-a-vm"></a>Bir VM koruyun

1. İçinde **kasa** > **öğeleri çoğaltılan**başarısız VM üzerinde sağ tıklatın ve seçin **yeniden koruma**. Yükü yönü birincil ikincil gelen göstermesi gerekir. 

  ![Koruyun](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. Kaynak grubu, ağ, depolama ve kullanılabilirlik kümeleri gözden geçirin. Daha sonra, **Tamam**'a tıklayın. Yeni olarak işaretlenmiş tüm kaynakları varsa, bunlar yükü işleminin bir parçası oluşturulur.
3. Yükü iş hedef sitenin en son verilerle çekirdeğini oluşturur. Bittikten sonra değişim çoğaltması gerçekleşir. Daha sonra birincil sitenin yerine çalıştırabilir. Sırasında kullanmak istediğiniz ağ koruyun, Özelleştir seçeneğini kullanarak veya depolama hesabı seçebilirsiniz.

  ![Seçenek özelleştirme](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

### <a name="customize-reprotect-settings"></a>Yeniden koruma ayarlarını özelleştirme

Yükü sırasında hedef VMe aşağıdaki özelliklerini özelleştirebilirsiniz.

![Özelleştir](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Özellik |Notlar  |
|---------|---------|
|Hedef kaynak grubu     | VM oluşturulacağı hedef kaynak grubu değiştirin. Yükü parçası olarak, hedef VM silinir. VM yük devretme sonrasında oluşturulacağı altında yeni bir kaynak grubu seçebilirsiniz.        |
|Hedef sanal ağ     | Hedef ağ yeniden koruma işi sırasında değiştirilemez. Ağ değiştirmek için Ağ eşlemesi yineleyin.         |
|Hedef depolama (ikincil VM yönetilen diskleri kullanmaz)     | VM yük devretme sonrasında kullandığı depolama hesabı değiştirebilirsiniz.         |
|Çoğaltma (ikincil VM yönetilen diskler kullanan) diskleri yönetilen    | Site Recovery çoğaltma yönetilen diskleri ikincil sanal makinenin yönetilen diskleri yansıtmak üzere birincil bölgede oluşturur.         | 
|Önbellek depolama     | Çoğaltma sırasında kullanılacak bir önbellek depolama hesabı belirtebilirsiniz. Varsayılan olarak, yeni bir önbellek depolama hesabı olan oluşturulabilir, yoksa.         |
|Kullanılabilirlik Kümesi     |İkincil bölge VM'yi bir kullanılabilirlik kümesinin parçası ise, birincil bölgesinde bulunan hedef VM için ayarlanmış kullanılabilirlik seçebilirsiniz. Varsayılan olarak, Site Recovery birincil bölgede var olan kullanılabilirlik bulmayı dener ve bunu kullanın. Özelleştirme sırasında yeni bir kullanılabilirlik kümesi belirtebilirsiniz.         |


### <a name="what-happens-during-reprotection"></a>Yükü sırasında ne olur?

Varsayılan olarak, aşağıdakiler gerçekleşir:

1. Birincil bölgede bir önbellek depolama hesabı oluşturulur
2. Hedef depolama hesabı (birincil bölge içinde özgün depolama hesabı) yoksa, yeni bir tane oluşturulur. Atanan depolama hesabı adı "ile asr" sonekine ikincil VM tarafından kullanılan depolama hesabının adıdır.
3. Çoğaltma, VM yönetilen diskleri kullanıyorsa, yönetilen diskleri ikincil VM diskleri çoğaltılan verileri depolamak için birincil bölge içinde oluşturulur. 
4. Hedef kullanılabilirlik kümesi yoksa, yeni bir tane gerekliyse yeniden koruma işi bir parçası olarak oluşturulur. Yükü ayarları özelleştirdiyseniz, seçili kümesini kullanılır.

Yeniden koruma işi ve VM var. hedef tetiklemek, aşağıdakiler gerçekleşir:

1. Gerekli bileşenleri yeniden koruma bir parçası olarak oluşturulur. Zaten varsa, bunlar yeniden kullanılır.
2. VM kapalıysa açık hedef tarafı çalıştığı.
3. Hedef tarafı VM disk Site Recovery tarafından bir kapsayıcıya çekirdek blob olarak kopyalanır.
4. Hedef tarafı VM sonra silinir.
5. Çekirdek blob geçerli kaynağı tarafından kullanılan tarafı çoğaltmak için (ikincil) VM. Bu, yalnızca farkları çoğaltılır sağlar.
6. Kaynak disk çekirdek blob arasındaki önemli değişiklikler eşitlenir. Bu işlemin tamamlanması biraz zaman alabilir.
7. Yeniden koruma işi tamamlandıktan sonra değişim çoğaltması başlar ve çoğaltma ilkesi uygun olarak bir kurtarma noktası oluşturur.
8. VM yeniden koruma işi başarılı olduktan sonra korumalı bir duruma girer.

## <a name="next-steps"></a>Sonraki adımlar

VM korunduktan sonra bir yük devretme işlemi başlatabilirsiniz. Yük devretme ikincil bölge VM'yi kapatır ve oluşturur ve bazı küçük kapalı kalma süresi ile birincil bölge içinde VM önyüklenir. Seçtiğiniz birer buna göre ve yük devretme sınamasını çalıştırmanızı öneririz, ancak birincil siteye tam bir yük devretme başlatılıyor. [Daha fazla bilgi edinin](site-recovery-failover.md) yük devretme hakkında.

