---
title: Hyper-V'den azure'a Azure Site recovery'de Azure'a olağanüstü durum kurtarma mimarisi | Microsoft Docs
description: Bu makalede, Azure Site Recovery hizmeti ile olağanüstü durum kurtarma için şirket içi Hyper-V Vm'lerini (VMM olmadan) azure'a dağıtırken kullanılan bileşenlere ve genel bir bakış sağlar.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: raynew
ms.openlocfilehash: f77069592fb34caf409b387f5c8452159f55e296
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60553343"
---
# <a name="hyper-v-to-azure-disaster-recovery-architecture"></a>Hyper-V'den Azure'a olağanüstü durum kurtarma mimarisi


Bu makalede mimari ve çoğaltma, yük devretme ve şirket içi Hyper-V konakları ve Azure arasında Hyper-V sanal makineleri (VM'ler) kurtarma kullanılan işlemleri kullanarak [Azure Site Recovery](site-recovery-overview.md) hizmeti.

Hyper-V konakları, System Center Virtual Machine Manager (VMM) özel bulutlarında isteğe bağlı olarak yönetilebilir.



## <a name="architectural-components---hyper-v-without-vmm"></a>Mimari bileşenler - VMM olmadan Hyper-V

Aşağıdaki tablo ve grafik azure'a, Hyper-V çoğaltma, Hyper-V konakları VMM tarafından yönetilmeyen olduğunda kullanılan bileşenleri üst düzey bir görünümünü sağlar.

**Bileşen** | **Gereksinim** | **Ayrıntılar**
--- | --- | ---
**Azure** | Bir Azure aboneliği, Azure depolama hesabını ve Azure ağı. | Çoğaltılan verileri şirket içi VM iş yükleri depolama hesabında depolanır. Şirket içi sitenizden yük devretme gerçekleştiğinde çoğaltılan iş yükü verileri azure Vm'leri oluşturulur.<br/><br/> Azure VM’leri oluşturulduğunda Azure sanal ağına bağlanır.
**Hyper-V** | Site Recovery dağıtımı sırasında Hyper-V konakları ve kümeleri Hyper-V sitelerinde bir araya toplayın. Azure Site Recovery sağlayıcısı ve kurtarma Hizmetleri Aracısı, her tek başına Hyper-V konağında veya her Hyper-V küme düğümünde yükleyin. | Sağlayıcı, İnternet üzerinden Site Recovery hizmetiyle gerçekleştirilen çoğaltma işlemini düzenler ve yönetir. Kurtarma Hizmetleri aracısı, veri çoğaltma işlemini gerçekleştirir.<br/><br/> Sağlayıcı ve aracı arasındaki iletişimler şifrelenir ve güvence altına alınır. Azure depolama alanında çoğaltılan veriler de şifrelenir.
**Hyper-V VM’leri** | Hyper-V çalıştıran bir veya daha fazla VM. | Hiçbir şey Vm'lerde açıkça yüklü olması gerekir.


**Hyper-V-Azure arası mimari (VMM olmadan)**

![Mimari](./media/hyper-v-azure-architecture/arch-onprem-azure-hypervsite.png)



## <a name="architectural-components---hyper-v-with-vmm"></a>Mimari bileşenler - VMM ile Hyper-V

Aşağıdaki tablo ve grafik Hyper-V konakları VMM bulutlarında yönetilen Hyper-V çoğaltma Azure için kullanılan bileşenleri üst düzey bir görünümünü sağlar.

**Bileşen** | **Gereksinim** | **Ayrıntılar**
--- | --- | ---
**Azure** | Bir Azure aboneliği, Azure depolama hesabını ve Azure ağı. | Çoğaltılan verileri şirket içi VM iş yükleri depolama hesabında depolanır. Şirket içi sitenizden yük devretme gerçekleştiğinde çoğaltılan verilerle azure Vm'leri oluşturulur.<br/><br/> Azure VM’leri oluşturulduğunda Azure sanal ağına bağlanır.
**VMM sunucusu** | VMM sunucusu, Hyper-V konakları içeren bir veya daha fazla bulut içerir. | Site Recovery ile çoğaltmayı düzenlemek için VMM sunucusunda Site Recovery Sağlayıcısı'nı yükleyin ve sunucuyu kurtarma Hizmetleri kasasına kaydedin.
**Hyper-V konağı** | VMM tarafından yönetilen bir veya daha fazla Hyper-V konağı/kümesi. |  Her Hyper-V konağı veya kümesi düğümüne kurtarma Hizmetleri aracısını yükleyin.
**Hyper-V VM’leri** | Hyper-V konağı sunucusunda çalışan bir veya daha fazla VM. | VM'lere açıkça bir şey yüklenmesi gerekmez.
**Ağ** | VMM sunucusunda ayarlanmış mantıksal ağlar ve VM ağları. VM ağı bulutla ilişkilendirilen bir mantıksal ağ bağlantılı olması gerekir. | VM ağları, Azure sanal ağlarına eşlenir. Yük devretme sonrasında oluşturulan Azure Vm'lerinin, VM ağına eşlenen Azure ağına eklenir.

**(VMM ile) Hyper-V-Azure arası mimari**

![Bileşenler](./media/hyper-v-azure-architecture/arch-onprem-onprem-azure-vmm.png)



## <a name="replication-process"></a>Çoğaltma işlemi

![Hyper-V'den Azure'a çoğaltma](./media/hyper-v-azure-architecture/arch-hyperv-azure-workflow.png)

**Çoğaltma ve kurtarma işlemi**


### <a name="enable-protection"></a>Korumayı etkinleştir

1. Azure portalında veya şirket içinde bir Hyper-V VM’si için koruma etkinleştirdikten sonra, **Korumayı etkinleştir** başlatılır.
2. İş, makinenin önkoşullarla uyumlu olup olmadığını denetler, ardından, çoğaltmayı daha önce yapılandırdığınız ayarları uygulamak üzere [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) çağırır.
3. İş, tam bir VM çoğaltması başlatmak için [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) yöntemini çağırarak ilk çoğaltmayı başlatır ve VM’lerin sanal disklerini Azure’a gönderir.
4. **İşler** sekmesinde işi izleyebilirsiniz.      ![İşler listesi](media/hyper-v-azure-architecture/image1.png) ![Korumayı etkinleştir ayrıntıları](media/hyper-v-azure-architecture/image2.png)


### <a name="initial-data-replication"></a>İlk veri çoğaltma

1. İlk çoğaltma tetiklendiğinde bir [Hyper-V VM anlık görüntüsü](https://technet.microsoft.com/library/dd560637.aspx) anlık görüntü alınır.
2. Tamamı Azure'a kopyalanana kadar birer birer çoğaltılır, VM üzerindeki sanal sabit diskleri olan. Bu VM boyutuna bağlı olarak biraz zaman ve ağ bant genişliği. [Bilgi nasıl](https://support.microsoft.com/kb/3056159) ağ bant genişliğini artırmak için.
3. İlk çoğaltma devam ederken disk değişiklikleri meydana gelirse, Hyper-V çoğaltma çoğaltma İzleyicisi bu değişiklikleri Hyper-V çoğaltma günlükleri (.hrl) izler. Bu günlük dosyaları disklerle aynı klasörde yer alır. Her diskin ikincil depolamaya gönderilir bir ilişkili .hrl dosyası vardır. İlk çoğaltma sırasında anlık görüntü ve günlük dosyaları disk kaynaklarını kullanır.
4. İlk çoğaltma tamamlandığında, VM anlık görüntüsü silinir.
5. Günlükteki değişim disk değişiklikleri eşitlenir ve üst diske birleştirilir.


### <a name="finalize-protection-process"></a>Koruma İşlemi Sonlandır

1. İlk çoğaltma sonlandırıldıktan sonra **sanal makinede korumayı Sonlandır** işi çalıştırır. Sanal makine korunuyor, ağ ve diğer çoğaltma sonrası ayarlarını yapılandırır.
2. Bu aşamada yük devretme için hazır olduğundan emin olmak için VM ayarlarını denetleyebilirsiniz. Üzerinde beklendiği gibi başarısız olduğunu denetlemek için VM için olağanüstü durum kurtarma tatbikatı (yük devretme testi) çalıştırabilirsiniz. 


## <a name="delta-replication"></a>Değişim çoğaltması

1. İlk çoğaltmadan sonra değişim çoğaltması, çoğaltma ilkesine uygun olarak başlar.
2. Hyper-V çoğaltma çoğaltma İzleyicisi, bir sanal sabit disk için değişiklikleri .hrl dosyası olarak izler. Çoğaltma için yapılandırılmış her diskin ilişkili bir .hrl dosyası vardır.
3. Günlük müşterinin depolama hesabına gönderilir. Bir günlük azure'a aktarım olduğunda, birincil diskteki değişiklikler aynı klasörde yer alan başka bir günlük dosyasında izlenir.
4. İlk çoğaltma ve değişim çoğaltması sırasında Azure portalında VM'yi izleyebilirsiniz.

### <a name="resynchronization-process"></a>Yeniden eşitleme işlemi

1. Değişim çoğaltması başarısız olursa ve bant genişliği ile zaman açısından tam çoğaltma maliyetli olacaksa, bir VM yeniden eşitleme için işaretlenir.
    - Örneğin, .hrl dosyası disk boyutunun %50'sine ulaşırsa VM, yeniden eşitleme için işaretlenir.
    -  Varsayılan olarak, yeniden eşitleme ofis saatleri dışında otomatik olarak çalışacak şekilde zamanlanır.
1.  Yeniden eşitleme yalnızca değişim verilerini gönderir.
    - Kaynak ve hedef sanal makinelerin sağlama toplamlarını hesaplayarak gönderilen veri miktarını en aza indirir.
    - Kaynak ve hedef dosyaların sabit öbeklere burada bölünmüş bir sabit blok kümeleme algoritması kullanır.
    - Her öbek için sağlama toplamları oluşturulur. Bunlar, kaynaktan hangi blokların hedefe uygulanması gerektiğini belirlemek üzere karşılaştırılır.
2. Yeniden eşitleme sona erdikten sonra, normal değişim çoğaltması devam edecektir.
3. Varsayılan yeniden eşitleme saatleri dışında beklemek istemiyorsanız, bir VM elle yeniden eşitleyebilirsiniz. Örneğin, bir kesinti oluşursa. Azure portalında Bunu yapmak için VM'yi seçin > **yeniden eşitleyin**.

    ![El ile yeniden eşitleme](./media/hyper-v-azure-architecture/image4-site.png)


### <a name="retry-process"></a>İşlemi yeniden deneyin

Bir çoğaltma hatası meydana gelirse, yerleşik yeniden deneme işlevi vardır. Yeniden deneme tabloda açıklandığı şekilde sınıflandırılır.

**Kategori** | **Ayrıntılar**
--- | ---
**Kurtarılamaz hatalar** | Yeniden deneme yapılmaya çalışılmaz. VM durumu **Kritik** olacaktır ve yönetici müdahalesi gerekir.<br/><br/> Bozuk bir VHD zinciri olan çoğaltma VM, ağ kimlik doğrulama hataları, yetkilendirme hataları için geçersiz bir durumda bu hataların örnekleri ve hataları (tek başına Hyper-V sunucuları için. VM bulunamadı
**Kurtarılabilir hatalar** | Yeniden deneme aralığını ilk denemenin başlangıcına göre 1, 2, 4, 8 ve 10 dakika artıran bir üstel geri çekme kullanılarak her çoğaltma aralığında yeniden denemeler gerçekleşir. Hata devam ederse, 30 dakikada bir yeniden deneyin. Buna örnek olarak, ağ hataları, düşük disk hataları ve düşük bellek koşulları içerir.



## <a name="failover-and-failback-process"></a>Yük devretme ve yeniden çalışma işlemi

1. Şirket içi Hyper-V Vm'lerinden Azure'a planlanmış veya planlanmamış bir yük devretme çalıştırabilirsiniz. Planlı bir yük devretme çalıştırırsanız, veri kaybı olmaması için kaynak VM’ler kapatılır. Birincil sitenizi erişilebilir durumda değilse, planlanmamış yük devretme çalıştırın.
2. Tek bir makine üzerinden yük devredebilir veya kurtarma planları, birden çok makinenin yük devretmelerini düzenlemek üzere oluşturun.
3. Bir yük devretme çalıştırın. Yük devretme ilk aşaması tamamlandıktan sonra oluşturulan kopya Vm'leri azure'da görebilirsiniz. Gerekli olursa VM’ye genel bir IP adresi atayabilirsiniz.
4. Daha sonra kopya Azure VM'SİNDEKİ iş yüküne erişmeye başlamak için yük devretmeyi yürütürsünüz.

Şirket içi altyapınızı yeniden çalışır duruma geldikten sonra geri dönebilirsiniz. Yeniden çalışma üç aşamada gerçekleşir:

1. Azure planlı yük devretme şirket içi siteye hız kazandırın:
    - **Kapalı kalma süresini**: Bu seçeneği kullanırsanız, Site Recovery yük devretmeden önce verileri eşitler. Bu işlem için değiştirilen veri bloklarını denetler ve şirket içi sitesine, Azure VM tutar çalışıyor, kapalı kalma süresini en aza indirir. El ile yük devretmeyi tamamlamanız gerekir belirttiğinizde, Azure VM'yi kapatın, son delta değişiklikleri kopyalanır ve yük devretme başlatır.
    - **Tam yükleme**: Bu seçenekle yük devretme sırasında veri eşitlenir. Bu seçenek, tüm diskin yükler. Hiçbir sağlama toplamları hesaplanır, ancak daha fazla kapalı kalma süresi yoktur çünkü daha hızlıdır. Azure Vm'lerini çoğaltma için biraz zaman çalıştırıyorsunuz veya şirket içi VM silinmişse bu seçeneği kullanın.
    - **VM oluşturma**: Aynı sanal makine veya alternatif bir VM başarısız seçeneğini belirleyebilirsiniz. Zaten yoksa Site Recovery VM oluşturması gerektiğini belirtebilirsiniz.

2. İlk eşitleme tamamlandıktan sonra yük devretmeyi tamamlamak için seçin. Tamamlandıktan sonra her şeyin beklendiği gibi çalıştığını denetlemek için şirket içi VM oturum açabilir. Azure portalında Azure Vm'lerini durduruldu mu olduğunu görebilirsiniz.
3.  Ardından, bitirme ve şirket içi sanal makineden yeniden iş yüküne erişmeye başlamak için yük devretmeyi yürütürsünüz.
4. İş yüklerini geri başarısız olduktan sonra yeniden şirket içi Vm'leri Azure'a çoğaltmak için çoğaltmayı tersine çevirme etkinleştirin.



## <a name="next-steps"></a>Sonraki adımlar


İzleyin [Bu öğreticide](tutorial-prepare-azure.md) Azure'a çoğaltma için Hyper-V ile başlamak için.


