---
title: "Bir yük devretme sınaması için (System Center VMM olmadan) Hyper-V çoğaltma Azure için Çalıştır | Microsoft Docs"
description: "Hyper-V Vm'lerini Azure Site Recovery hizmetini kullanarak Azure'a çoğaltma için bir yük devretme testi çalıştırmak için gereken adımları özetler."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c9a4c3ca-84a0-4668-b700-9b0046202299
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: 0974b9eda2cb7e3ba54a4a0fad0a768db644caf9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="step-11-run-a-test-failover-for-hyper-v-replication-to-azure"></a>11. adım: bir yük devretme sınaması için Hyper-V çoğaltma Azure için çalıştırın.

Bu makalede yük devretme testi şirket içi Hyper-V sanal makineleri (System Center VMM tarafından yönetilmediğinden) Azure'a nasıl çalıştırılacağını açıklar kullanarak [Azure Site Recovery](site-recovery-overview.md) Azure portalında hizmet.

POST açıklamaları ve soruları alt bu makalenin veya üzerinde [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="before-you-start"></a>Başlamadan önce

VM özelliklerini doğrulayın ve herhangi bir değişiklik öneririz yük devretme testi çalıştırmadan önce gerekir. VM Özellikleri'nde erişebilirsiniz **öğeleri çoğaltılan**. **Essentials** dikey makineler ayarlarını ve durumu hakkında bilgileri gösterir.

## <a name="managed-disk-considerations"></a>Yönetilen disk değerlendirmeleri

[Yönetilen diskleri](../virtual-machines/windows/managed-disks-overview.md) VM diskleri ile ilişkilendirilmiş depolama hesaplarını yöneterek Azure VM'ler için disk yönetimi kolaylaştırabilir. 

- Yönetilen diskleri oluşturulur ve yalnızca azure'a yük devretme oluştuğunda VM'ye bağlı. Korumayı etkinleştirdiğinizde, depolama hesapları için şirket içi Vm'lerden gelen verilerini çoğaltır.
- Yönetilen diskleri Resource manager dağıtım modeli kullanılarak dağıtılan VM'ler için oluşturulabilir.
- Azure'dan şirket içi Hyper-V ortamında, yönetilen disklerle makineler için şu anda desteklenmiyor. Yalnızca ayarlamalısınız **yönetilen diskleri kullanımını** için **Evet** yaptığınız geçiş tek (yük devretme Azure geri dönme olmadan),
- Bu ayar etkinse, yalnızca kullanılabilirlik sahip kaynak gruplarını ayarlar **yönetilen diskleri kullanımını** etkin seçilebilir. Yönetilen bir diske sahip sanal makineleri kullanılabilirlik kümeleri ile birlikte olmalıdır **yönetilen diskleri kullanımını** kümesine **Evet**. Ayar VM'ler için etkin değil, yalnızca kaynak gruplarındaki kullanılabilirlik kümelerini yönetilen diski etkin olmayan seçilebilir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set).
- - Çoğaltma için kullandığınız depolama hesabı depolama hizmeti şifrelemesi ile şifrelenmiş yönetilen diskleri yük devretme sırasında oluşturulamıyor. Bu durumda yok yönetilen diskleri kullanımını etkinleştirmek veya sanal makine için korumayı devre dışı bırakın hem şifreleme etkin olmayan bir depolama hesabı kullanacak şekilde yeniden etkinleştirin. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption).

 
## <a name="network-considerations"></a>Ağ konuları
    
- Yük devretme sonrasında Azure VM için kullanılacak hedef IP adresini ayarlayabilirsiniz. Bir IP adresi sağlamazsanız yük devredilen makine DHCP kullanır. Yük devretmede kullanılamayan bir adres ayarlarsanız yük devretme işlemi başarısız olur. Hedef IP adresi, yük devretme ağı testinde kullanılabilirse aynı IP adresi yük devretme sınamasında da kullanılabilir.
- Ağ bağdaştırıcılarının sayısı, hedef sanal makine için sizin belirlediğiniz boyuta göre aşağıdaki gibi belirlenmiştir:
    - Kaynak makinedeki ağ bağdaştırıcılarının sayısı, hedef makine boyutu için verilen ağ bağdaştırıcısı sayısına eşitse veya daha azsa hedef makine kaynakla aynı sayıda bağdaştırıcıya sahip olur.
    - Kaynak sanal makinenin bağdaştırıcı sayısı, hedef boyut için izin verilen sayıyı aşarsa maksimum hedef boyutu kullanılır.
    - Örneğin, kaynak makinenin iki bağdaştırıcısı varsa ve hedef makine boyutu dört adet bağdaştırıcıyı destekliyorsa hedef makinenin iki bağdaştırıcısı olur. Kaynak makinenin iki bağdaştırıcısı varken hedef boyut yalnızca bir bağdaştırıcıyı destekliyorsa hedef makinenin bir bağdaştırıcısı olur.     
- VM'nin birden çok bağdaştırıcısı varsa bunların tümü aynı ağa bağlanır.
- Sanal makine birden çok ağ bağdaştırıcısına sahip sonra listede gösterilen birinci hale *varsayılan* Azure sanal makine ağ bağdaştırıcısı.


## <a name="view-and-manage-vm-settings"></a>VM ayarlarını görüntülemek ve yönetmek

Bir yük devretme çalıştırmadan önce kaynak makinenin özelliklerini doğrulamanızı öneririz.

1. İçinde **korunan öğeler**, tıklatın **çoğaltılan öğeler**, VM'a tıklayın.

    ![Çoğaltmayı etkinleştirme](./media/hyper-v-site-walkthrough-test-failover/test-failover1.png)
2. İçinde **yinelenmiş öğesi** bölmesinde, VM bilgileri, sistem durumu ve en son kullanılabilir kurtarma noktalarını özetini görebilirsiniz. Tıklatın **özellikleri** daha fazla ayrıntı görüntülemek için.

    ![Çoğaltmayı etkinleştirme](./media/hyper-v-site-walkthrough-test-failover/test-failover2.png)
3. İçinde **işlem ve ağ**, şunları yapabilirsiniz:
    - Azure VM adını değiştirin. Adı karşılamalıdır [Azure gereksinimleri](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
    - Bir yük devretme sonrasında [kaynak grubu] belirtin.
    - Azure VM için bir hedef boyutu belirtin
    - Seçin bir [kullanılabilirlik kümesi](../virtual-machines/windows/tutorial-availability-sets.md).
    - Kullanıp kullanmayacağınızı belirtin [yönetilen diskleri](#managed-disk-considerations). Seçin **Evet**, Azure geçiş makinenizde yönetilen diskleri eklemek istiyorsanız.
    - Görüntülemek veya, Azure VM yük devretme ve kendisine atanmış IP adresi sonra yer alacağı ağını/alt ağını dahil olmak üzere ağ ayarlarını değiştirin.

    ![Çoğaltmayı etkinleştirme](./media/hyper-v-site-walkthrough-test-failover/test-failover4.png)
4. İçinde **diskleri**, VM işletim sistemi ve veri diskleri hakkındaki bilgileri görebilirsiniz.


## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Şimdi, her şeyin beklendiği gibi çalıştığından emin olmak için yük devretme testi çalıştırın.

- Yük devretmeden sonra RDP kullanarak Azure vm'lerine bağlanmak isterseniz [bağlamaya hazırlanmak](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
 - Tam anlamıyla test etmek için Active Directory ve DNS'in bir kopyasını test ortamınızda bulundurmanız gerekir. [Daha fazla bilgi edinin](site-recovery-active-directory.md#test-failover-considerations).
 - Yük devretme testi hakkında tam bilgi için okuma [bu makalede](site-recovery-test-failover-to-azure.md) makalesi.
 
 Artık bir yük devretme çalıştırın:

1. İçinde tek bir makineye vermesine **çoğaltılan öğeler**, VM tıklayın > **+ yük devretme testi** simgesi.
2. Kurtarma planında yük devretme için **Kurtarma Planları** seçeneklerinde plana sağ tıklayıp **Yük Devretme Testi**'ne tıklayın. Kurtarma planı oluşturmak için [aşağıdaki yönergeleri uygulayın](site-recovery-create-recovery-plans.md).
3. İçinde **yük devretme testi**, Azure Vm'lerinin Azure ağa bağlı olacak yük devretme gerçekleştikten sonra seçin.
4. Yük devretmeyi başlatmak için **Tamam**'a tıklayın. Özelliklerini açmak için VM'ye veya üzerinde tıklayarak ilerleme durumunu izleyebilirsiniz **yük devretme testi** kasa adı işinde > **işleri** > **Site Recovery işleri**.
5. Yük devretme tamamlandıktan sonra çoğaltılan makineyi Azure portalı > **Sanal Makineler** kısmında da görmeniz gerekir. VM'nin uygun boyutta olduğundan, uygun bir ağa bağlandığından ve çalıştığından emin olmanız gerekir.
6. Yük devretme sonrasındaki bağlantılar için hazırlık yaptıysanız Azure VM'ye bağlanabilmeniz gerekir.
7. Bitirdikten sonra tıklayın **temizleme yük devretme testi** kurtarma planı üzerinde. Yük devretme testiyle ilişkili gözlemlerinizi kaydetmek ve saklamak için **Notlar**'a tıklayın. Bunun yapılması, yük devretme testi sırasında oluşturulan sanal makineleri siler.



## <a name="next-steps"></a>Sonraki adımlar

- [Daha fazla bilgi edinin](site-recovery-failover.md) farklı türdeki yük devretmeler ve bunları çalıştırma hakkında.
- [Yeniden çalışma hakkında okuyun](site-recovery-failback-from-azure-to-hyper-v.md), geri dönecek ve Azure Vm'lerini geri birincil şirket içi Hyper-V siteye çoğaltma.

