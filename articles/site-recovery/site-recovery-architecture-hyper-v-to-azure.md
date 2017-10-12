---
title: "Azure'a Hyper-V çoğaltması Azure Site Recovery'de nasıl çalışır? | Microsoft Docs"
description: "Bu makalede, Azure Site Recovery hizmeti ile şirket içi Hyper-V VM'lerini Azure'a çoğaltma işleminde kullanılan bileşenlere ve mimariye ilişkin genel bir bakış sunulmaktadır."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/23/2017
ms.author: raynew
ms.openlocfilehash: 28f775afaf72b11eec0c22f755e4dbd6a485c895
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-does-hyper-v-replication-to-azure-work-in-site-recovery"></a>Site Recovery’de Azure’a Hyper-V çoğaltması nasıl işliyor?


Bu makalede [Azure Site Recovery](site-recovery-overview.md) hizmeti aracılığıyla şirket içi Hyper-V sanal makinelerini Azure'a çoğaltma işleminde kullanılan bileşenler ve işlemler açıklanmıştır.

Site Recovery, System Center Virtual Machine Manager (VMM) ile veya VMM olmadan yönetilen Hyper-V kümelerindeki ve bağımsız konaklardaki Hyper-V VM'lerini çoğaltabilir.

Tüm yorumlarınızı bu makalenin alt kısmında veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda paylaşabilirsiniz.



## <a name="architectural-components"></a>Mimari bileşenler

Hyper-V VM'lerini Azure'a çoğaltırken kullanılan çeşitli bileşenler vardır.

**Bileşen** | **Konum** | **Ayrıntılar**
--- | --- | ---
**Azure** | Azure’da bir Microsoft Azure hesabına, bir Azure depolama hesabına ve bir Azure ağına ihtiyacınız vardır. | Çoğaltılan veriler depolama hesabında depolanır ve şirket içi sitenizden yük devretme gerçekleştiğinde çoğaltılan verilerle Azure VM’leri oluşturulur.<br/><br/> Azure VM’leri oluşturulduğunda Azure sanal ağına bağlanır.
**VMM sunucusu** | Hyper-V konakları VMM bulutlarında bulunur | Hyper-V ana bilgisayarları VMM bulutlarında yönetiliyorsa, VMM sunucusunu Kurtarma Hizmetleri kasasına kaydedin.<br/><br/> VMM sunucusuna, Azure ile çoğaltmayı düzenlemek için Site Recovery Sağlayıcısı’nı yükleyin.<br/><br/> Ağ eşlemeyi yapılandırmak için, mantıksal ağlar ve VM ağları kurulumu gerekir. VM ağının da bulutla ilişkilendirilen bir mantıksal ağ ile bağlantılı olması gerekir.
**Hyper-V konağı** | Hyper-V konakları ve kümeleri, VMM sunucusu ile veya sunucu olmadan dağıtılabilir. | VMM sunucusu yoksa, İnternet’ten Site Recovery ile çoğaltmayı düzenlemek için ana bilgisayara Site Recovery Sağlayıcısı yüklenir. Bir VMM sunucusu varsa, Sağlayıcı ana bilgisayara değil bu sunucuya yüklenir.<br/><br/> Kurtarma Hizmetleri aracısı, veri çoğaltmayı işlemek için ana bilgisayara yüklenir.<br/><br/> Sağlayıcı ve aracı arasındaki iletişimler şifrelenir ve güvence altına alınır. Azure depolama alanında çoğaltılan veriler de şifrelenir.
**Hyper-V VM’leri** | Hyper-V konak sunucusunda çalışan bir veya daha fazla VM'nizin olması gerekir. | VM'lere açıkça bir şey yüklenmesi gerekmez.

[Destek matrisinde](site-recovery-support-matrix-to-azure.md), bu bileşenlerden her birine ilişkin dağıtım önkoşulları ve gereksinimler hakkında bilgi edinin.

**Şekil 1: Hyper-V sitesinden Azure'a çoğaltma**

![Bileşenler](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)

**Şekil 2: VMM bulutlarındaki Hyper-V'den Azure'a çoğaltma**

![Bileşenler](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)


## <a name="replication-process"></a>Çoğaltma işlemi

**Şekil 3: Hyper-V'den Azure'a çoğaltma için çoğaltma ve kurtarma işlemi**

![iş akışı](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

### <a name="enable-protection"></a>Korumayı etkinleştir

1. Azure portalında veya şirket içinde bir Hyper-V VM’si için koruma etkinleştirdikten sonra, **Korumayı etkinleştir** başlatılır.
2. İş, makinenin önkoşullarla uyumlu olup olmadığını denetler, ardından, çoğaltmayı daha önce yapılandırdığınız ayarları uygulamak üzere [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) çağırır.
3. İş, tam bir VM çoğaltması başlatmak için [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) yöntemini çağırarak ilk çoğaltmayı başlatır ve VM’lerin sanal disklerini Azure’a gönderir.
4. **İşler** sekmesinde işi izleyebilirsiniz.      ![İşler listesi](media/site-recovery-hyper-v-azure-architecture/image1.png) ![Korumayı etkinleştir ayrıntıları](media/site-recovery-hyper-v-azure-architecture/image2.png)

### <a name="replicate-the-initial-data"></a>İlk verileri çoğaltma

1. İlk çoğaltma tetiklendiğinde bir [Hyper-V VM anlık görüntüsü](https://technet.microsoft.com/library/dd560637.aspx) alınır.
2. Sanal diskler, hepsi Azure'a kopyalanana kadar birer birer çoğaltılır. VM boyutuna ve ağ bant genişliğine bağlı olarak biraz uzun sürebilir. Ağ kullanımınızı iyileştirmek için, bkz: [Şirket içinden Azure'a koruma ağ bant genişliği kullanımını yönetme](https://support.microsoft.com/kb/3056159).
3. İlk çoğaltma devam ederken disk değişiklikleri meydana gelirse, Hyper-V Çoğaltma'nın Çoğaltma İzleyicisi bu değişiklikleri Hyper-V Çoğaltma Günlükleri (.hrl) olarak izler. Bu dosyalar disklerle aynı klasörde bulunur. Her diskin ikincil depolamaya gönderilecek bir ilişkili .hrl dosyası vardır.
4. İlk çoğaltma sırasında anlık görüntü ve günlük dosyaları disk kaynaklarını kullanır.
5. İlk çoğaltma tamamlandığında, VM anlık görüntüsü silinir. Günlükteki değişim disk değişiklikleri eşitlenir ve üst diske birleştirilir.


### <a name="finalize-protection"></a>Korumayı sonlandırma

1. İlk çoğaltma sona erdikten sonra, **Sanal makinede korumayı sonlandır** işi, sanal makinenin korunabilmesi adına ağı ve diğer çoğaltma sonrası ayarlarını yapılandırır.
    ![Korumayı sonlandır işi](media/site-recovery-hyper-v-azure-architecture/image3.png)
2. Azure'da çoğaltma yapıyorsanız sanal makinede ince ayar yapmanız gerekebilir. Böylece sanal makine yük devretme için hazır hale gelir. Bu noktada, her şeyin istendiği şekilde çalıştığını denetlemek için yük devretme testi çalıştırabilirsiniz.

### <a name="replicate-the-delta"></a>Değişimi çoğaltma

1. İlk çoğaltma sonrasında, çoğaltma ayarlarına uygun olarak değişim eşitlemesi başlar.
2. Bir Çoğaltma İzleyicisi olan Hyper-V Çoğaltma, bir sanal sabit diskteki değişiklikleri .hrl dosyası olarak izler. Çoğaltma için yapılandırılmış her diskin ilişkili bir .hrl dosyası vardır. İlk çoğaltma tamamlandıktan sonra bu günlük müşterinin depolama hesabına gönderilir. Bir günlük Azure’a iletilmekteyken, birincil diskteki değişiklikler aynı dizindeki başka bir günlük dosyasında izlenir.
3. İlk çoğaltma ve değişim çoğaltması esnasında, VM görünümünde VM’yi izleyebilirsiniz. [Daha fazla bilgi edinin](site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machines).  

### <a name="synchronize-replication"></a>Çoğaltmayı senkronize etme

1. Değişim çoğaltması başarısız olursa ve bant genişliği ile zaman açısından tam çoğaltma maliyetli olacaksa, bir VM yeniden eşitleme için işaretlenir. Örneğin, .hrl dosyası disk boyutunun %50'sine ulaşırsa VM, yeniden eşitleme için işaretlenir.
2.  Yeniden eşitleme, kaynak ve hedef sanal makinelerin sağlama toplamlarını hesaplayarak ve yalnızca değişim verilerini göndererek, gönderilen veri miktarını en aza indirir. Yeniden eşitleme, kaynak ve hedef dosyaların sabit öbeklere bölündüğü bir sabit blok kümeleme algoritması kullanır. Her öbek için sağlama toplamları oluşturulur ve ardından, kaynaktan hangi blokların hedefe uygulanması gerektiğini belirlemek üzere karşılaştırılır.
3. Yeniden eşitleme sona erdikten sonra, normal değişim çoğaltması devam edecektir. Varsayılan olarak, yeniden eşitleme ofis saatleri dışında otomatik olarak çalışacak şekilde planlanmıştır ancak sanal makineyi elle yeniden eşitleyebilirsiniz. Örneğin, bir ağ kesintisi veya başka bir kesinti oluşursa, yeniden eşitlemeyi devam ettirebilirsiniz. Bunu yapmak için, portalda VM’yi > **Yeniden eşitle**’yi seçin.

    ![El ile yeniden eşitleme](media/site-recovery-hyper-v-azure-architecture/image4.png)


### <a name="retry-logic"></a>Yeniden deneme mantığı

Bir çoğaltma hatası meydana gelirse, yerleşik yeniden deneme işlevi vardır. Bu mantık iki kategoride sınıflandırılabilir:

**Kategori** | **Ayrıntılar**
--- | ---
**Kurtarılamaz hatalar** | Yeniden deneme yapılmaya çalışılmaz. VM durumu **Kritik** olacaktır ve yönetici müdahalesi gerekir. Bu hataların örnekleri şunlardır: kırılmış VHD zinciri; Çoğaltma VM için geçersiz durum; Ağ kimlik doğrulama hataları: yetkilendirme hataları; VM bulunamadı hataları (tek başına Hyper-V sunucuları için)
**Kurtarılabilir hatalar** | Yeniden deneme aralığını ilk denemenin başlangıcına göre 1, 2, 4, 8 ve 10 dakika artıran bir üstel geri çekme kullanılarak her çoğaltma aralığında yeniden denemeler gerçekleşir. Hata devam ederse, 30 dakikada bir yeniden deneyin. Örnekler şunlardır: ağ hataları; düşük disk hataları; yetersiz bellek durumları |



## <a name="failover-and-failback-process"></a>Yük devretme ve yeniden çalışma işlemi

1. Şirket içi Hyper-V VM’lerinden Azure’a planlanmış veya planlanmamış bir [yük devretme](site-recovery-failover.md) gerçekleştirebilirsiniz. Planlı bir yük devretme çalıştırırsanız, veri kaybı olmaması için kaynak VM’ler kapatılır.
2. Tek bir makine üzerinden yük devredebilir veya [kurtarma planları](site-recovery-create-recovery-plans.md) oluşturarak birden çok makinenin devredilmesini düzenleyebilirsiniz.
4. Yük devretmeyi çalıştırdıktan sonra, oluşturulan kopya VM’leri Azure’da görebiliyor olmanız gerekir. Gerekli olursa VM’ye genel bir IP adresi atayabilirsiniz.
5. Daha sonra, kopya Azure VM’sindeki iş yüküne erişmeye başlamak için yük devretmeyi yürütürsünüz.
6. Birincil yerinde siteniz yeniden kullanılabilir olduğunda siteyi [yeniden çalıştırabilirsiniz](site-recovery-failback-from-azure-to-hyper-v.md). Azure’dan birincil siteye planlanmış yük devretme işlemi başlatırsınız. Planlanmış bir yük devretme gerçekleştirmek için aynı VM’de ya da alternatif bir konumda yeniden çalışmayı seçebilir ve Azure ile şirket içi arasında değişiklikleri eşitleyerek veri kaybı olmamasını sağlayabilirsiniz. VM’ler şirket içinde oluşturulduğunda yük devretmeyi yürütürsünüz.




## <a name="next-steps"></a>Sonraki adımlar

[Destek matrisini](site-recovery-support-matrix-to-azure.md) inceleyin
