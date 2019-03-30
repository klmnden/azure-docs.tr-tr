---
title: Yeniden koruma başarısız Azure Vm'leri üzerinde Azure Site Recovery ile Birincil Azure bölgesine geri dön | Microsoft Docs
description: Azure Site Recovery kullanarak birincil bir bölgeden yük devretme sonrasında Azure Vm'lerini ikincil bir bölgede yeniden korumak nasıl açıklar.
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: bd65b1479ace1a51087836eb8032f16fd10dc119
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58648911"
---
# <a name="reprotect-failed-over-azure-vms-to-the-primary-region"></a>Yeniden koruma birincil bölgeye Azure Vm'leri üzerinde başarısız oldu


Olduğunda, [yük devretme](site-recovery-failover.md) başka bir kullanarak Azure Vm'leri bir bölgeden [Azure Site Recovery](site-recovery-overview.md), VM'lerin önyükleme ikincil bölgede, korumasız bir durumda. Başarısız Vm'lere birincil bölgeye gönderir, aşağıdakileri yapmanız gerekir:

- Böylece birincil bölgeye çoğaltmak başlatmaları ikincil bölgedeki Vm'leri yeniden koruyun.
- Yeniden koruma tamamlandıktan sonra Vm'leri çoğaltma yapıyorsanız, siz bunları ikincil birincil bölgeye devredebilir.

## <a name="prerequisites"></a>Önkoşullar
1. Birincil VM yük devretme ikincil bölgeye kaydedilmiş olması gerekir.
2. Birincil hedef siteye kullanılabilir olması gerekir ve erişmek veya bu bölgede kaynakları oluşturmak mümkün olması gerekir.

## <a name="reprotect-a-vm"></a>Bir VM'yi yeniden koruma

1. İçinde **kasası** > **çoğaltılan öğeler**başarısız VM'ye sağ tıklayın ve seçin **yeniden koruma**. Yeniden koruma yönü, ikincil siteden göstermelidir.

   ![Yeniden koruma](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. Kaynak grubu, ağ, depolama ve kullanılabilirlik kümeleri gözden geçirin. Daha sonra, **Tamam**'a tıklayın. Yeni olarak işaretli tüm kaynaklar varsa, bunlar yeniden koruma işleminin bir parçası oluşturulur.
3. Yeniden koruma işi, hedef siteye en son verileri sağlar. Bittikten sonra değişim çoğaltması gerçekleşir. Daha sonra birincil sitenin devredebilir. Depolama hesabını seçebilir veya sırasında kullanmak istediğiniz ağ yeniden koruma, Özelleştir seçeneğini kullanarak.

   ![Seçenek özelleştirme](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

### <a name="customize-reprotect-settings"></a>Yeniden koruma ayarlarını özelleştirme

Yeniden koruma sırasında ' % s'hedef VMe aşağıdaki özelliklerini özelleştirebilirsiniz.

![Özelleştirme](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Özellik |Notes  |
|---------|---------|
|Hedef kaynak grubu     | Hedef kaynak grubu, sanal Makinenin oluşturulduğu değiştirin. Yeniden koruma bir parçası, hedef sanal makine silindi. Yük devretmeden sonra VM'ye oluşturulacağı altında yeni bir kaynak grubu seçebilirsiniz.        |
|Hedef sanal ağ     | Hedef ağ yeniden koruma işi sırasında değiştirilemez. Ağ değiştirmek için ağ eşlemesini yineler.         |
|Hedef depolama (ikincil VM'yi yönetilen disklere kullanmaz)     | Yük devretme sonrasında VM kullandığı depolama hesabı olarak değiştirebilirsiniz.         |
|Yönetilen çoğaltma diskleri (ikincil VM'yi yönetilen disklere kullanır)    | Site Recovery, ikincil sanal makinenin yönetilen disklerini yansıtmak için birincil bölgede yönetilen çoğaltma diskleri oluşturur.         |
|Önbellek depolama     | Çoğaltma sırasında kullanılacak bir önbellek depolama hesabı belirtebilirsiniz. Varsayılan olarak, yeni bir önbellek depolama hesabı olması oluşturulur, yoksa.         |
|Kullanılabilirlik Kümesi     |İkincil bölgedeki bir sanal makine bir kullanılabilirlik kümesinin parçasıysa, birincil bölgeye hedef sanal Makineyi bir kullanılabilirlik seçebilirsiniz. Varsayılan olarak, Site Recovery birincil bölgede mevcut kullanılabilirlik bulmaya çalışır ve bunu kullanın. Özelleştirme sırasında yeni bir kullanılabilirlik kümesi belirtebilirsiniz.         |


### <a name="what-happens-during-reprotection"></a>Yeniden koruma sırasında ne olacak?

Varsayılan olarak, aşağıdakiler gerçekleşir:

1. Bir önbellek depolama hesabı bölgede oluşturulur devredilen VM'nin çalıştığı.
2. Hedef depolama hesabı'nı (birincil bölgedeki özgün depolama hesabı) yoksa, yeni bir tane oluşturulur. Atanan depolama hesabı adı ile: "asr" sonekine ikincil VM tarafından kullanılan depolama hesabının adıdır.
3. Sanal makinenizin yönetilen diskleri kullanıyorsa, yönetilen çoğaltma diskleri ikincil sanal makinenin diskleri çoğaltılan verileri depolamak için birincil bölgede oluşturulur.
4. Hedef kullanılabilirlik kümesi mevcut değilse yeni bir tane gerekirse yeniden koruma işini bir parçası olarak oluşturulur. Daha sonra yeniden koruma ayarları özelleştirdiyseniz, seçili kümesi kullanılır.

Yeniden koruma işini ve VM var. hedef tetiklediğinizde aşağıdakiler gerçekleşir:

1. VM kapalıysa açık olduğundan hedef tarafta, çalışıyor.
2. VM'yi yönetilen diskleri kullanıyorsa, özgün diskleri bir kopyası oluşturulur ile '-ASRReplica' soneki. Özgün diskleri silinir. '-ASRReplica' kopyaları, çoğaltma için kullanılır.
3. VM'yi yönetilmeyen diskler kullanıyorsanız, hedef sanal makinenin veri diski kullanımdan çıkarıldı ve çoğaltma için kullanılan. İşletim sistemi diskinin bir kopyasını oluşturulur ve VM'ye bağlı. Özgün işletim sistemi diski kullanımdan çıkarıldı ve çoğaltma için kullanılan.
4. Yalnızca kaynak ve hedef disk arasındaki değişiklikleri eşitlenir. Differentials her iki diskte de karşılaştırılmasıyla hesaplanır ve daha sonra aktarılan. Bu işlemin tamamlanması birkaç saat sürebilir.
5. Eşitleme tamamlandıktan sonra değişiklik çoğaltması başlar ve bir kurtarma noktası Çoğaltma İlkesi ayarlarına uygun olarak oluşturur.

Yeniden koruma işini tetiklemek için ve diskleri ve hedef sanal Makineyi yok aşağıdakiler gerçekleşir:
1. VM'yi yönetilen diskleri kullanıyorsa, çoğaltma diskleri ile oluşturulan '-ASRReplica' soneki. '-ASRReplica' kopyaları, çoğaltma için kullanılır.
2. VM'yi yönetilmeyen diskler kullanıyorsanız, çoğaltma diskleri kullanarak hedef depolama hesabı oluşturulur.
3. Tüm diskler kopyalanır başarısız gelen yeni hedef bölgeye bölge üzerinden.
4. Eşitleme tamamlandıktan sonra değişiklik çoğaltması başlar ve bir kurtarma noktası Çoğaltma İlkesi ayarlarına uygun olarak oluşturur.

## <a name="next-steps"></a>Sonraki adımlar

Sanal makine korunduktan sonra bir yük devretme başlatabilirsiniz. Yük devretme ikincil bölgedeki VM kapatır ve oluşturur ve bazı küçük kapalı kalma süresi ile birincil bölgedeki VM önyüklenir. Bir zaman uygun şekilde seçin ve birincil site için tam bir yük devretmeyi başlatmadan önce yük devretme testi çalıştırma öneririz. [Daha fazla bilgi edinin](site-recovery-failover.md) yük devretme hakkında.
