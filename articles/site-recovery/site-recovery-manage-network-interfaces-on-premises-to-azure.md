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
ms.date: 02/27/2018
ms.author: manayar
ms.openlocfilehash: ab8582d9c32cf13bd7b21a59031af8fde58effbf
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="manage-virtual-machine-network-interfaces-for-on-premises-to-azure-scenarios"></a>Şirket içi Azure senaryoları için sanal makine ağ arabirimleri yönetme

Azure'da sanal makine (VM) ekli en az bir ağ arabirimine sahip olmalıdır. Birçok ağ arabirimleri VM boyutu destekler bağlı olarak sağlayabilirsiniz.

Varsayılan olarak, bir Azure sanal makinesine bağlı ilk ağ arabirimi birincil ağ arabirimi olarak tanımlanır. Sanal makineyi diğer tüm ağ arabirimlerini ikincil ağ arabirimlerine ' dir. Ayrıca varsayılan olarak, sanal makineden giden tüm trafiği birincil ağ arabirimi birincil IP yapılandırması için atanan IP adresi çıkışı gönderilir.

Bir şirket içi ortamda ortamda farklı ağlar için birden çok ağ arabirimi sanal makine ya da sunucular olabilir. Farklı ağlarda genellikle yükseltmeler, Bakım ve Internet erişimi gibi belirli işlemler gerçekleştirmek için kullanılır. Geçiş ya da Azure'a bir şirket içi ortamından devretmek aynı sanal makine ağ arabirimleri tüm aynı sanal ağa bağlı olması gerekir olduğunu aklınızda bulundurun.

Birçok şirket içi sunucuya bağlı olarak bir Azure sanal makinesi arabirimlerde ağ olarak varsayılan olarak, Azure Site Recovery oluşturur. Ağ arabirimi ayarları altında çoğaltılmış sanal makine ayarlarını düzenleyerek geçişi veya yük devretme sırasında yedek ağ arabirimleri oluşturma önleyebilirsiniz.

## <a name="select-the-target-network"></a>Hedef ağ seçin

VMware ve fiziksel makineler için ve Hyper-V (olmadan System Center Virtual Machine Manager) sanal makineler için tek tek sanal makineleri için hedef sanal ağ belirtebilirsiniz. Virtual Machine Manager ile yönetilen Hyper-V sanal makineleri için kullanmak [ağ eşlemesi](site-recovery-network-mapping.md) kaynak Sanal Makine Yöneticisi sunucunun VM ağlarında eşlemeniz ve hedef Azure ağları.

1. Altında **öğeleri çoğaltılan** bir kurtarma Hizmetleri kasasına çoğaltılmış öğenin ayarlarına erişmek için herhangi bir çoğaltılmış öğeyi seçin.

2. Seçin **işlem ve ağ** çoğaltılan öğe ağ ayarlarına erişmek için sekme.

3. Altında **ağ özellikleri**, kullanılabilir ağ arabirimleri listesinden bir sanal ağ seçin.

    ![Ağ ayarları](./media/site-recovery-manage-network-interfaces-on-premises-to-azure/compute-and-network.png)

Hedef ağ değiştirme, belirli bir sanal makine için tüm ağ arabirimleri etkiler.

Virtual Machine Manager Bulutları için Ağ eşlemesi değiştirmek için tüm sanal makineleri ve ağ arabirimlerini etkiler.

## <a name="select-the-target-interface-type"></a>Hedef arabirim türü seçin

Altında **ağ arabirimleri** bölümünü **işlem ve ağ** bölmesinde, görüntülemek ve ağ arabirimi ayarları düzenleyin. Hedef ağ arabirimi türünü de belirtebilirsiniz.

- A **birincil** ağ arabirimi yük devretme için gereklidir.
- Tüm seçili ağ arabirimleri, varsa **ikincil** ağ arabirimleri.
- Seçin **kullanmayın** bir ağ arabirimi yük devretme sırasında oluşturma hariç tutulacak.

Çoğaltma, etkinleştirirken varsayılan olarak, Site Recovery şirket içi sunucu üzerindeki tüm algılanan ağ arabirimleri seçer. Tek olarak işaretler **birincil** ve tüm diğer olarak **ikincil**. Şirket içi sunucuda eklenen sonraki arabirimlerden işaretlenmiş **kullanmayın** varsayılan olarak. Daha fazla ağ arabirimleri eklerken doğru Azure sanal makine hedef boyutu tüm gerekli ağ arabirimleri uyum sağlamak için seçili olduğundan emin olun.

## <a name="modify-network-interface-settings"></a>Ağ arabirimi ayarlarını değiştirme

Çoğaltılmış bir öğenin ağ arabirimleri için IP adresi ve alt ağ değiştirebilirsiniz. Bir IP adresi belirtilmezse, Site Recovery ağ arabirimi yük devretme sırasında alt ağdan sonraki kullanılabilir IP adresi atar.

1. Ağ arabirimi ayarları'nı açmak için tüm kullanılabilir ağ arabirimi seçin.

2. İstenen alt ağ kullanılabilir alt ağlar listesinden seçin.

3. İstenen IP adresi (gerektiği gibi) girin.

    ![Ağ arabirimi ayarları](./media/site-recovery-manage-network-interfaces-on-premises-to-azure/network-interface-settings.png)

4. Seçin **Tamam** düzenlenmesini tamamlamayı ve dönmek için **işlem ve ağ** bölmesi.

5. Diğer ağ arabirimleri için 1-4 arası adımları yineleyin.

6. Seçin **kaydetmek** tüm değişiklikleri kaydetmek için.

## <a name="next-steps"></a>Sonraki adımlar
  [Daha fazla bilgi edinin](../virtual-network/virtual-network-network-interface-vm.md) Azure sanal makineleri için ağ arabirimleri hakkında.
