---
title: "Site Recovery’de Azure’a Hyper-V çoğaltması nasıl işliyor? | Microsoft Belgeleri"
description: "Bu makalede, Hyper-V çoğaltmasının Azure Site Recovery’de nasıl çalıştığına dair genel bir bakış sağlanmaktadır"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/05/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: d9dad6cff80c1f6ac206e7fa3184ce037900fc6b
ms.openlocfilehash: 9adf266c6a2ac00c3aaa34e2a29aefe34abe2871
ms.lasthandoff: 04/18/2017


---
# <a name="how-does-hyper-v-replication-to-azure-work"></a>Azure’a Hyper-V çoğaltması nasıl işler?

Bu makaleyi, [Azure Site Recovery](site-recovery-overview.md) hizmeti kullanılarak Azure’a Hyper-V çoğaltması için iş akışlarını ve mimariyi anlamak üzere okuyun.

Tüm yorumlarınızı bu makalenin alt kısmında veya [Azure Kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)'nda paylaşabilirsiniz.

Azure'a aşağıdakileri çoğaltabilirsiniz:
- **VMM ile Hyper-V**: System Center Virtual MAchine Manager (VMM) bulutlarında yönetilen şirket içi Hyper-V ana bilgisayarlarında yer alan VM'ler. Ana bilgisayarlar, [desteklenen herhangi bir işletim sistemini](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers) çalıştırıyor olabilir. [Hyper-V ve Azure tarafından desteklenen](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) herhangi bir konuk işletim sistemini çalıştıran Hyper-V VM’lerini çoğaltabilirsiniz.
- **VMM olmadan Hyper-V**: VMM bulutlarında yönetilmeyen Hyper-V ana bilgisayarlarında bulunan şirket içi VM’ler. Ana bilgisayarlar [desteklenen herhangi bir işletim sisteminde](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) çalışabilir. [Hyper-V ve Azure tarafından desteklenen](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) herhangi bir konuk işletim sistemini çalıştıran Hyper-V VM’lerini çoğaltabilirsiniz.


## <a name="architectural-components"></a>Mimari bileşenler

**Alan** | **Bileşen** | **Ayrıntılar**
--- | --- | ---
**Azure** | Azure’da bir Microsoft Azure hesabına, bir Azure depolama hesabına ve bir Azure ağına ihtiyacınız vardır. | Depolama ve ağ, Kaynak Yöneticisi tabanlı veya klasik hesaplar olabilir.<br/><br/> Çoğaltılan veriler depolama hesabında depolanır ve şirket içi sitenizden yük devretme gerçekleştiğinde çoğaltılan verilerle Azure VM’leri oluşturulur.<br/><br/> Azure VM’leri oluşturulduğunda Azure sanal ağına bağlanır.
**VMM sunucusu** | VMM bulutlarında bulunan Hyper-V ana bilgisayarları | Hyper-V ana bilgisayarları VMM bulutlarında yönetiliyorsa, VMM sunucusunu Kurtarma Hizmetleri kasasına kaydedin.<br/><br/> VMM sunucusuna, Azure ile çoğaltmayı düzenlemek için Site Recovery Sağlayıcısı’nı yükleyin.<br/><br/> Ağ eşlemeyi yapılandırmak için, mantıksal ağlar ve VM ağları kurulumu gerekir. VM ağının da bulutla ilişkilendirilen bir mantıksal ağ ile bağlantılı olması gerekir.
**Hyper-V konağı** | Hyper-V sunucuları VMM sunucusu ile veya sunucu olmadan dağıtılabilir. | VMM sunucusu yoksa, İnternet’ten Site Recovery ile çoğaltmayı düzenlemek için ana bilgisayara Site Recovery Sağlayıcısı yüklenir. Bir VMM sunucusu varsa, Sağlayıcı ana bilgisayara değil bu sunucuya yüklenir.<br/><br/> Kurtarma Hizmetleri aracısı, veri çoğaltmayı işlemek için ana bilgisayara yüklenir.<br/><br/> Sağlayıcı ve aracı arasındaki iletişimler şifrelenir ve güvence altına alınır. Azure depolama alanında çoğaltılan veriler de şifrelenir.
**Hyper-V VM’leri** | Hyper-V konak sunucusunda bir veya daha fazla VM’niz olmalıdır. | Sanal makinelere açıkça bir şey yüklenmesi gerekmez

## <a name="deployment-steps"></a>Dağıtım adımları

1. **Azure**: Azure bileşenlerini ayarlayın. Site Recovery dağıtımına başlamadan önce depolama ve ağ hesapları ayarlamanızı öneririz.
2. **Kasa**: Site Recovery için bir Kurtarma Hizmetleri kasası oluşturun ve kaynak ile hedef ayarlarını yapılandırma, bir çoğaltma ilkesi ayarlama ve çoğaltmayı etkinleştirilme de dahil olmak üzere kasa ayarlarını yapılandırın.
3. **Kaynak ve hedef**:
    - **VMM bulutlarındaki Hyper-V ana bilgisayarları**: Kaynak ayarlarını belirtmenin parçası olarak, VMM sunucusuna Azure Site Recovery Sağlayıcısını ve her Hyper-V ana bilgisayarında Azure Kurtarma Hizmetleri aracısını indirip yükleyin. Kaynak, VMM sunucusu olacaktır. Hedef, Azure’dur.
    - VMM olmadan Hyper-V ana bilgisayarları: Kaynak ayarlarını belirttiğinizde, her Hyper-V ana bilgisayarına Sağlayıcıyı ve aracıyı indirip yükleyin. Dağıtım esnasında, ana bilgisayarları bir Hyper-V sitesinde toplayın ve bunu kaynak olarak belirtin. Hedef, Azure’dur.

    ![Azure’a Hyper-V/VMM çoğaltma](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)
    ![Azure’a Hyper-V sitesi çoğaltma](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)


4. **Çoğaltma ilkesi**: Hyper-V sitesi veya VMM bulutu için bir çoğaltma ilkesi oluştur. İlke, site veya buluttaki tüm konaklarda yer alan tüm VM’lere uygulanır.
5. **Çoğaltmayı etkinleştir**: Hyper-V VM’leri için çoğaltmayı etkinleştir. İlk çoğaltma, çoğaltma ilkesi ayarlarına uygun şekilde gerçekleştirilir. Veri değişiklikleri izlenir ve delta değişikliklerinin Azure’a çoğaltılması ilk çoğaltma işlemi tamamlandıktan sonra başlar. Bir öğe için izlenen değişiklikler bir .hrl dosyasında saklanır.
6. **Test yük devretme**:  Her şeyin çalıştığından emin olmak için bir Yük devretme testi çalıştır.

Dağıtım hakkında daha fazla bilgi edinin:
- [Hyper-V VM’lerini Azure’a çoğaltmaya başlayın - VMM ile](site-recovery-vmm-to-azure.md)
- [Hyper-V VM’lerini Azure’a çoğaltmaya başlayın - VMM olmadan](site-recovery-hyper-v-site-to-azure.md)

## <a name="hyper-v-replication-workflow"></a>Hyper-V çoğaltma iş akışı

### <a name="enable-protection"></a>Korumayı etkinleştir

1. Azure portalında veya şirket içinde bir Hyper-V VM’si için koruma etkinleştirdikten sonra, **Korumayı etkinleştir** başlatılır.
2. İş, makinenin önkoşullarla uyumlu olup olmadığını denetler, ardından, çoğaltmayı daha önce yapılandırdığınız ayarları uygulamak üzere [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) çağırır.
3. İş, tam bir VM çoğaltması başlatmak için [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) yöntemini çağırarak ilk çoğaltmayı başlatır ve VM’lerin sanal disklerini Azure’a gönderir.
4. **İşler** sekmesinde işi izleyebilirsiniz.
        ![İşler listesi](media/site-recovery-hyper-v-azure-architecture/image1.png)
        ![Koruma etkinleştir detayına git](media/site-recovery-hyper-v-azure-architecture/image2.png)

### <a name="initial-replication"></a>İlk çoğaltma

1. İlk çoğaltma tetiklendiğinde bir [Hyper-V VM anlık görüntüsü](https://technet.microsoft.com/library/dd560637.aspx) alınır.
2. Sanal diskler, hepsi Azure'a kopyalanana kadar birer birer çoğaltılır. VM boyutuna ve ağ bant genişliğine bağlı olarak biraz uzun sürebilir. Ağ kullanımınızı iyileştirmek için, bkz: [Şirket içinden Azure'a koruma ağ bant genişliği kullanımını yönetme](https://support.microsoft.com/kb/3056159).
3. İlk çoğaltma devam ederken disk değişiklikleri meydana gelirse, Hyper-V Çoğaltma'nın Çoğaltma İzleyicisi bu değişiklikleri Hyper-V Çoğaltma Günlükleri (.hrl) olarak izler. Bu dosyalar disklerle aynı klasörde bulunur. Her diskin ikincil depolamaya gönderilecek bir ilişkili .hrl dosyası vardır.
4. İlk çoğaltma sırasında anlık görüntü ve günlük dosyaları disk kaynaklarını kullanır.
5. İlk çoğaltma tamamlandığında, VM anlık görüntüsü silinir. Günlükteki değişim disk değişiklikleri eşitlenir ve üst diske birleştirilir.


### <a name="finalize-protection"></a>Korumayı sonlandırma

1. İlk çoğaltma sona erdikten sonra, **Sanal makinede korumayı sonlandır** işi, sanal makinenin korunabilmesi adına ağı ve diğer çoğaltma sonrası ayarlarını yapılandırır.
    ![Korumayı sonlandır işi](media/site-recovery-hyper-v-azure-architecture/image3.png)
2. Azure'da çoğaltma yapıyorsanız sanal makinede ince ayar yapmanız gerekebilir. Böylece sanal makine yük devretme için hazır hale gelir. Bu noktada, her şeyin istendiği şekilde çalıştığını denetlemek için yük devretme testi çalıştırabilirsiniz.

### <a name="delta-replication"></a>Değişim çoğaltması

1. İlk çoğaltma sonrasında, çoğaltma ayarlarına uygun olarak değişim eşitlemesi başlar.
2. Bir Çoğaltma İzleyicisi olan Hyper-V Çoğaltma, bir sanal sabit diskteki değişiklikleri .hrl dosyası olarak izler. Çoğaltma için yapılandırılmış her diskin ilişkili bir .hrl dosyası vardır. İlk çoğaltma tamamlandıktan sonra bu günlük müşterinin depolama hesabına gönderilir. Bir günlük Azure’a iletilmekteyken, birincil diskteki değişiklikler aynı dizindeki başka bir günlük dosyasında izlenir.
3. İlk çoğaltma ve değişim çoğaltması esnasında, VM görünümünde VM’yi izleyebilirsiniz. [Daha fazla bilgi edinin](site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machines).  

### <a name="replication-synchronization"></a>Çoğaltma eşitleme

1. Değişim çoğaltması başarısız olursa ve bant genişliği ile zaman açısından tam çoğaltma maliyetli olacaksa, bir VM yeniden eşitleme için işaretlenir. Örneğin, .hrl dosyası disk boyutunun %50'sine ulaşırsa VM, yeniden eşitleme için işaretlenir.
2.  Yeniden eşitleme, kaynak ve hedef sanal makinelerin sağlama toplamlarını hesaplayarak ve yalnızca değişim verilerini göndererek, gönderilen veri miktarını en aza indirir. Yeniden eşitleme, kaynak ve hedef dosyaların sabit öbeklere bölündüğü bir sabit blok kümeleme algoritması kullanır. Her öbek için sağlama toplamları oluşturulur ve ardından, kaynaktan hangi blokların hedefe uygulanması gerektiğini belirlemek üzere karşılaştırılır.
3. Yeniden eşitleme sona erdikten sonra, normal değişim çoğaltması devam edecektir. Varsayılan olarak, yeniden eşitleme ofis saatleri dışında otomatik olarak çalışacak şekilde planlanmıştır ancak sanal makineyi elle yeniden eşitleyebilirsiniz. Örneğin, bir ağ kesintisi veya başka bir kesinti oluşursa, yeniden eşitlemeyi devam ettirebilirsiniz. Bunu yapmak için, portalda VM’yi > **Yeniden eşitle**’yi seçin.

    ![El ile yeniden eşitleme](media/site-recovery-hyper-v-azure-architecture/image4.png)


### <a name="retries"></a>Yeniden deneme sayısı

Bir çoğaltma hatası meydana gelirse, yerleşik yeniden deneme işlevi vardır. Bu mantık iki kategoride sınıflandırılabilir:

**Kategori** | **Ayrıntılar**
--- | ---
**Kurtarılamaz hatalar** | Yeniden deneme yapılmaya çalışılmaz. VM durumu **Kritik** olacaktır ve yönetici müdahalesi gerekir. Bu hataların örnekleri şunlardır: kırılmış VHD zinciri; Çoğaltma VM için geçersiz durum; Ağ kimlik doğrulama hataları: yetkilendirme hataları; VM bulunamadı hataları (tek başına Hyper-V sunucuları için)
**Kurtarılabilir hatalar** | Yeniden deneme aralığını ilk denemenin başlangıcına göre 1, 2, 4, 8 ve 10 dakika artıran bir üstel geri çekme kullanılarak her çoğaltma aralığında yeniden denemeler gerçekleşir. Hata devam ederse, 30 dakikada bir yeniden deneyin. Örnekler şunlardır: ağ hataları; düşük disk hataları; yetersiz bellek durumları |

## <a name="protection-and-recovery-lifecycle"></a>Koruma ve kurtarma yaşam döngüsü

![iş akışı](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Dağıtım önkoşullarını denetleme](site-recovery-prereq.md)
- Aşağıdakilerle sorun giderme:
    - [Koruma izleme ve sorun giderme](site-recovery-monitoring-and-troubleshooting.md)
    - [Microsoft destekten yardım](site-recovery-monitoring-and-troubleshooting.md#reach-out-for-microsoft-support)
    - [Genel sorunlar ve çözümleri](site-recovery-monitoring-and-troubleshooting.md#common-azure-site-recovery-errors-and-their-resolutions)

