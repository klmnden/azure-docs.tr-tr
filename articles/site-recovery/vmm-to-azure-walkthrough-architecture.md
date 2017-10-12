---
title: "Azure Site Recovery ile Hyper-V'den Azure'a çoğaltma işleminin (System Center VMM ile) yapısını inceleme | Microsoft Docs"
description: "Bu makalede, Azure Site Recovery hizmeti ile şirket içi VMM bulutlarındaki Hyper-V VM'lerini Azure'a çoğaltma işleminde kullanılan bileşenlere ve mimariye ilişkin genel bir bakış sunulmaktadır."
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
ms.date: 07/24/2017
ms.author: raynew
ms.openlocfilehash: df4e227d02901153d3cfcfd4dfd4f11de180763a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="step-1-review-the-architecture"></a>1. Adım: Mimariyi gözden geçirme


Bu makalede, System Center Virtual Machine Manager (VMM) bulutlarındaki şirket içi Hyper-V sanal makinelerini [Azure Site Recovery](site-recovery-overview.md) hizmeti aracılığıyla Azure'a çoğaltma işleminde kullanılan bileşenler ve işlemler açıklanmaktadır.

Tüm yorumlarınızı bu makalenin alt kısmında veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda paylaşabilirsiniz.



## <a name="architectural-components"></a>Mimari bileşenler

VMM bulutlarındaki Hyper-V VM'leri Azure'a çoğaltırken kullanılan çeşitli bileşenler vardır.

**Bileşen** | **Gereksinim** | **Ayrıntılar**
--- | --- | ---
**Azure** | Azure’da bir Microsoft Azure hesabına, bir Azure depolama hesabına ve bir Azure ağına ihtiyacınız vardır. | Çoğaltılan veriler depolama hesabında depolanır ve şirket içi sitenizden yük devretme gerçekleştiğinde çoğaltılan verilerle Azure VM’leri oluşturulur.<br/><br/> Azure VM’leri oluşturulduğunda Azure sanal ağına bağlanır.
**VMM sunucusu** | VMM sunucusu, Hyper-V konakları içeren bir veya daha fazla bulut içerir. | VMM sunucusunda Site Recovery ile çoğaltmayı düzenlemek için Site Recovery Sağlayıcısı'nı yükleyin ve sunucuyu Kurtarma Hizmetleri kasasına kaydedin.
**Hyper-V konağı** | VMM tarafından yönetilen bir veya daha fazla Hyper-V konağı/kümesi. |  Her konak veya küme üyesinde Kurtarma Hizmetleri aracısını yükleyin.
**Hyper-V VM’leri** | Hyper-V konağı sunucusunda çalışan bir veya daha fazla VM. | VM'lere açıkça bir şey yüklenmesi gerekmez.
**Ağ** |VMM sunucusunda ayarlanmış mantıksal ağlar ve VM ağları. VM ağının da bulutla ilişkilendirilen bir mantıksal ağ ile bağlantılı olması gerekir. | VM ağları, yük devretme sonrasında oluşturulan Azure sanal makinelerinin bir ağda olması için Azure sanal ağlarına eşlenir.

[Destek matrisinde](site-recovery-support-matrix-to-azure.md), bu bileşenlerden her birine ilişkin dağıtım önkoşulları ve gereksinimler hakkında bilgi edinin.


**Şekil 1: VMM bulutlarındaki Hyper-V konaklarında bulunan VM’leri-Azure’a çoğaltma**

![Bileşenler](./media/vmm-to-azure-walkthrough-architecture/arch-onprem-onprem-azure-vmm.png)


## <a name="replication-process"></a>Çoğaltma işlemi

**Şekil 2: Hyper-V'den Azure'a çoğaltma için çoğaltma ve kurtarma işlemi**

![iş akışı](./media/vmm-to-azure-walkthrough-architecture/arch-hyperv-azure-workflow.png)

### <a name="enable-protection"></a>Korumayı etkinleştir

1. Azure portalında veya şirket içinde bir Hyper-V VM’si için koruma etkinleştirdikten sonra, **Korumayı etkinleştir** başlatılır.
2. İş, makinenin önkoşullarla uyumlu olup olmadığını denetler, ardından, çoğaltmayı daha önce yapılandırdığınız ayarları uygulamak üzere [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) çağırır.
3. İş, tam bir VM çoğaltması başlatmak için [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) yöntemini çağırarak ilk çoğaltmayı başlatır ve VM’lerin sanal disklerini Azure’a gönderir.
4. **İşler** sekmesinde işi izleyebilirsiniz.      ![İşler listesi](media/vmm-to-azure-walkthrough-architecture/image1.png) ![Korumayı etkinleştir ayrıntıları](media/vmm-to-azure-walkthrough-architecture/image2.png)

### <a name="replicate-the-initial-data"></a>İlk verileri çoğaltma

1. İlk çoğaltma tetiklendiğinde bir [Hyper-V VM anlık görüntüsü](https://technet.microsoft.com/library/dd560637.aspx) alınır.
2. Sanal diskler, hepsi Azure'a kopyalanana kadar birer birer çoğaltılır. VM boyutuna ve ağ bant genişliğine bağlı olarak biraz uzun sürebilir. Ağ kullanımınızı iyileştirmek için, bkz: [Şirket içinden Azure'a koruma ağ bant genişliği kullanımını yönetme](https://support.microsoft.com/kb/3056159).
3. İlk çoğaltma devam ederken disk değişiklikleri meydana gelirse, Hyper-V Çoğaltma'nın Çoğaltma İzleyicisi bu değişiklikleri Hyper-V Çoğaltma Günlükleri (.hrl) olarak izler. Bu dosyalar disklerle aynı klasörde bulunur. Her diskin ikincil depolamaya gönderilecek bir ilişkili .hrl dosyası vardır.
4. İlk çoğaltma sırasında anlık görüntü ve günlük dosyaları disk kaynaklarını kullanır.
5. İlk çoğaltma tamamlandığında, VM anlık görüntüsü silinir. Günlükteki değişim disk değişiklikleri eşitlenir ve üst diske birleştirilir.


### <a name="finalize-protection"></a>Korumayı sonlandırma

1. İlk çoğaltma sona erdikten sonra, **Sanal makinede korumayı sonlandır** işi, sanal makinenin korunabilmesi adına ağı ve diğer çoğaltma sonrası ayarlarını yapılandırır.
    ![Korumayı sonlandır işi](media/vmm-to-azure-walkthrough-architecture/image3.png)
2. Azure'da çoğaltma yapıyorsanız sanal makinede ince ayar yapmanız gerekebilir. Böylece sanal makine yük devretme için hazır hale gelir. Bu noktada, her şeyin istendiği şekilde çalıştığını denetlemek için yük devretme testi çalıştırabilirsiniz.

### <a name="replicate-the-delta"></a>Değişimi çoğaltma

1. İlk çoğaltma sonrasında, çoğaltma ayarlarına uygun olarak değişim eşitlemesi başlar.
2. Bir Çoğaltma İzleyicisi olan Hyper-V Çoğaltma, bir sanal sabit diskteki değişiklikleri .hrl dosyası olarak izler. Çoğaltma için yapılandırılmış her diskin ilişkili bir .hrl dosyası vardır. İlk çoğaltma tamamlandıktan sonra bu günlük müşterinin depolama hesabına gönderilir. Bir günlük Azure’a iletilmekteyken, birincil diskteki değişiklikler aynı dizindeki başka bir günlük dosyasında izlenir.
3. İlk çoğaltma ve değişim çoğaltması esnasında, VM görünümünde VM’yi izleyebilirsiniz. [Daha fazla bilgi edinin](site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machines).  

### <a name="synchronize-replication"></a>Çoğaltmayı senkronize etme

1. Değişim çoğaltması başarısız olursa ve bant genişliği ile zaman açısından tam çoğaltma maliyetli olacaksa, bir VM yeniden eşitleme için işaretlenir. Örneğin, .hrl dosyası disk boyutunun %50'sine ulaşırsa VM, yeniden eşitleme için işaretlenir.
2.  Yeniden eşitleme, kaynak ve hedef sanal makinelerin sağlama toplamlarını hesaplayarak ve yalnızca değişim verilerini göndererek, gönderilen veri miktarını en aza indirir. Yeniden eşitleme, kaynak ve hedef dosyaların sabit öbeklere bölündüğü bir sabit blok kümeleme algoritması kullanır. Her öbek için sağlama toplamları oluşturulur ve ardından, kaynaktan hangi blokların hedefe uygulanması gerektiğini belirlemek üzere karşılaştırılır.
3. Yeniden eşitleme sona erdikten sonra, normal değişim çoğaltması devam edecektir. Varsayılan olarak, yeniden eşitleme ofis saatleri dışında otomatik olarak çalışacak şekilde planlanmıştır ancak sanal makineyi elle yeniden eşitleyebilirsiniz. Örneğin, bir ağ kesintisi veya başka bir kesinti oluşursa, yeniden eşitlemeyi devam ettirebilirsiniz. Bunu yapmak için, portalda VM’yi > **Yeniden eşitle**’yi seçin.

    ![El ile yeniden eşitleme](media/vmm-to-azure-walkthrough-architecture/image4.png)


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

[2. Adım: Dağıtım önkoşullarını inceleme](vmm-to-azure-walkthrough-prerequisites.md)
