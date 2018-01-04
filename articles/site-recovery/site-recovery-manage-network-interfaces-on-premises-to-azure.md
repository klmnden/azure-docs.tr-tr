---
title: "Şirket içi Azure senaryoları için ağ arabirimleri Azure Site kurtarma yönetme | Microsoft Docs"
description: "Şirket içi Azure Site Recovery ile Azure senaryoları için ağ arabirimleri açıklar"
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/22/2017
ms.author: manayar
ms.openlocfilehash: b60c746ae6c243d2b448056f8768df3baaed4cc1
ms.sourcegitcommit: 4256ebfe683b08fedd1a63937328931a5d35b157
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/23/2017
---
# <a name="manage-virtual-machine-network-interfaces-for-on-premises-to-azure-scenarios"></a>Şirket içi Azure senaryoları için sanal makine ağ arabirimleri yönetme

Azure'da sanal makine ekli en az bir ağ arabirimine sahip olmalıdır ve sayıda ağ arabirimleri için VM boyutu destekleyen eklenebilir. Varsayılan olarak, bir Azure sanal makinesine bağlı ilk ağ arabirimi birincil ağ arabirimi olarak tanımlanır. Sanal makineyi diğer tüm ağ arabirimlerini ikincil ağ arabirimlerine ' dir. Varsayılan olarak, sanal makineden giden tüm trafiği birincil ağ arabirimi birincil IP yapılandırması için atanan IP adresi çıkışı gönderilir.

Bir şirket içi ortamda sanal makineleri/sunucuları ortamda farklı ağlarda birden çok ağ arabirimleri olabilir. Farklı ağlarda genellikle yükseltmeler, bakım, internet erişimi vb. gibi belirli işlemler gerçekleştirmek için kullanılır. Geçiş veya Azure'a bir şirket içi ortamından devretmek, aynı sanal makine ağ arabirimleri tüm aynı sanal ağa bağlı olması gerekir olduğunu aklınızda bulundurun.

Birçok şirket içi sunucuya bağlı olarak bir Azure sanal makinesi arabirimlerde ağ olarak varsayılan olarak, Site Recovery oluşturur. Çoğaltılmış sanal makine için ayarlar altında ağ arabirimi ayarlarını düzenleyerek geçişi veya yük devretme sırasında yedek ağ arabirimleri oluşturma önleyebilirsiniz.

## <a name="select-the-target-network"></a>Hedef ağ seçin

VMware ve fiziksel makineler için ve (VMM olmadan) Hyper-V sanal makineleri için tek tek sanal makineleri için hedef sanal ağ belirtebilirsiniz. VMM ile yönetilen Hyper-V sanal makineleri için [ağ eşlemesi](site-recovery-network-mapping.md) bir kaynak VMM sunucusunda VM ağları eşlemek ve hedef Azure ağları için kullanılır.

1. 'Çoğaltılan öğeler altında' bir kurtarma Hizmetleri kasasına, çoğaltılmış öğenin ayarlarına erişmek için herhangi bir çoğaltılmış öğeyi tıklayın.

2. Çoğaltılan öğe ağ ayarlarına erişmek için 'İşlem ve ağ' sekmesini tıklatın.

3. 'Ağ özellikleri' altında kullanılabilir ağ arabirimleri listesinden bir sanal ağ seçin.

    ![İşlem ve ağ](./media/site-recovery-manage-network-interfaces-on-premises-to-azure/compute-and-network.png)

Hedef ağ değiştirme, belirli bir sanal makine için tüm ağ arabirimleri etkiler.

VMM Bulutları için Ağ eşlemesi değiştirmek için tüm sanal makineleri ve ağ arabirimlerini etkiler.

## <a name="select-the-target-interface-type"></a>Hedef arabirim türü seçin

'İşlem ve ağ' dikey 'Ağ arabirimi' bölümü altında görüntülemek ve ağ arabirimi ayarları düzenleyin ve hedef ağ arabirim türü belirtin.

- A **birincil** ağ arabirimi yük devretme için gereklidir.
- Tüm seçili ağ arabirimleri, varsa **ikincil** ağ arabirimleri.
- Seçin **kullanmayın** bir ağ arabirimi yük devretme sırasında oluşturma hariç tutulacak.

Etkinleştirme çoğaltma, Site Recovery seçtiğinde varsayılan olarak, tüm ağ arabirimleri 'İkincil' ağ arabirimleri gibi 'Primary' ve diğerleri de bir işaretleme şirket içi sunucuda algıladı. Şirket içi sunucuda eklenen sonraki arabirimlerden 'varsayılan olarak kullanmayın' işaretlenir. Daha fazla ağ arabirimleri eklerken, doğru Azure sanal makine hedef boyutu tüm gerekli ağ arabirimleri uyum sağlamak için seçili olduğundan emin olun.

## <a name="modifying-network-interface-settings"></a>Ağ arabirimi ayarlarını değiştirme

IP ve alt ağ için bir çoğaltılmış öğenin ağ arabirimleri değiştirebilirsiniz. Bir IP belirtilmezse, Site Recovery sonraki kullanılabilir IP alt ağ üzerinden Yük Devretme ağ arabiriminin atar.

1. Ağ arabirimi ayarları dikey penceresini açmak için tüm kullanılabilir ağ arabirimi'ı tıklatın.

2. İstenen alt ağ kullanılabilir alt ağlar listesinden seçin.

3. İstenen IP (gerektiği gibi) girin.

    ![Ağ arabirimi ayarları](./media/site-recovery-manage-network-interfaces-on-premises-to-azure/network-interface-settings.png)

4. Düzenlenmesini tamamlamayı ve 'İşlem ve ağ' dikey penceresine dönmek için Tamam'ı tıklatın.

5. Diğer ağ arabirimleri için 1-4 arası adımları yineleyin.

6. 'Kaydet' tüm değişiklikleri kaydetmek için tıklatın.

## <a name="next-steps"></a>Sonraki adımlar
  [Daha fazla bilgi edinin](../virtual-network/virtual-network-network-interface-vm.md) Azure sanal makineleri için ağ arabirimleri hakkında.
