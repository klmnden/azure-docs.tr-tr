---
title: Hyper-V için Azure Site kurtarma Azure çoğaltma mimarisi | Microsoft Docs
description: Bu makalede, Azure Site Recovery hizmeti ile şirket içi Hyper-V VM'leri Azure'a çoğaltma işleminde kullanılan bileşenlere ve mimariye ilişkin genel bir bakış sunulmaktadır.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 05/02/2018
ms.author: raynew
ms.openlocfilehash: 11ecc341977efb5c815581a77383d1c8cae51ed6
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="hyper-v-to-azure-replication-architecture"></a>Hyper-V çoğaltma Azure Mimarisi


Bu makalede mimari ve çoğaltma, yük devri ve Hyper-V sanal makineleri (VM'ler) şirket içi Hyper-V konakları ve Azure arasında Kurtarma sırasında kullanılan işlemleri kullanarak [Azure Site Recovery](site-recovery-overview.md) hizmet.

Hyper-V konakları System Center Virtual Machine Manager (VMM) özel bulutlara isteğe bağlı olarak yönetilebilir.



## <a name="architectural-components---hyper-v-without-vmm"></a>Mimari Bileşenleri - VMM olmadan Hyper-V

Aşağıdaki tablo ve grafik Hyper-V konakları VMM tarafından yönetilmeyen Hyper-V çoğaltma Azure için kullanılan bileşenler üst düzey bir görünümünü sağlar.

**Bileşen** | **Gereksinim** | **Ayrıntılar**
--- | --- | ---
**Azure** | Bir Azure aboneliği, Azure depolama hesabı ve Azure ağı. | Şirket içi VM iş yüklerini çoğaltılmış verileri depolama hesabında depolanır. Şirket içi sitenizdeki yük devretme durumunda azure Vm'leri çoğaltılmış iş yükü verilerle oluşturulur.<br/><br/> Azure VM’leri oluşturulduğunda Azure sanal ağına bağlanır.
**Hyper-V** | Site Recovery dağıtımı sırasında Hyper-V siteye Hyper-V konakları ve kümeleri toplayın. Azure Site Recovery sağlayıcısı ve kurtarma Hizmetleri Aracısı, her tek başına Hyper-V konağı veya her bir Hyper-V küme düğümüne yükleyin. | Sağlayıcı, İnternet üzerinden Site Recovery hizmetiyle gerçekleştirilen çoğaltma işlemini düzenler ve yönetir. Kurtarma Hizmetleri aracısı, veri çoğaltma işlemini gerçekleştirir.<br/><br/> Sağlayıcı ve aracı arasındaki iletişimler şifrelenir ve güvence altına alınır. Azure depolama alanında çoğaltılan veriler de şifrelenir.
**Hyper-V VM’leri** | Hyper-V üzerinde çalışan bir veya daha fazla VM. | Hiçbir şey Vm'lerinde açıkça yüklü olması gerekir.


**Hyper-V (VMM olmadan) Azure Mimarisi**

![Mimari](./media/hyper-v-azure-architecture/arch-onprem-azure-hypervsite.png)



## <a name="architectural-components---hyper-v-with-vmm"></a>Mimari Bileşenleri - VMM ile Hyper-V

Aşağıdaki tablo ve grafik Hyper-V konakları VMM bulutlarında yönetilen Hyper-V çoğaltma Azure için kullanılan bileşenler üst düzey bir görünümünü sağlar.

**Bileşen** | **Gereksinim** | **Ayrıntılar**
--- | --- | ---
**Azure** | Bir Azure aboneliği, Azure depolama hesabı ve Azure ağı. | Şirket içi VM iş yüklerini çoğaltılmış verileri depolama hesabında depolanır. Şirket içi sitenizdeki yük devretme durumunda azure Vm'leri çoğaltılan veriler ile oluşturulur.<br/><br/> Azure VM’leri oluşturulduğunda Azure sanal ağına bağlanır.
**VMM sunucusu** | VMM sunucusu, Hyper-V konakları içeren bir veya daha fazla bulut içerir. | Site Recovery ile çoğaltmayı düzenlemek için VMM sunucusundaki Site kurtarma Sağlayıcısı'nı yükleyin ve sunucunun kurtarma Hizmetleri kasasına kaydedin.
**Hyper-V konağı** | VMM tarafından yönetilen bir veya daha fazla Hyper-V konağı/kümesi. |  Her Hyper-V ana bilgisayar veya küme düğümünde kurtarma Hizmetleri aracısını yükleyin.
**Hyper-V VM’leri** | Hyper-V konağı sunucusunda çalışan bir veya daha fazla VM. | VM'lere açıkça bir şey yüklenmesi gerekmez.
**Ağ** | VMM sunucusunda ayarlanmış mantıksal ağlar ve VM ağları. VM ağının da bulutla ilişkilendirilen bir mantıksal ağ bağlantılı olması gerekir. | VM ağları için Azure sanal ağlar eşlenir. Yük devretme sonrasında Azure Vm'lerinin oluşturduğunuzda, VM ağı'na eşlenen Azure ağına eklenir.

**(VMM ile) Hyper-V için Azure Mimarisi**

![Bileşenler](./media/hyper-v-azure-architecture/arch-onprem-onprem-azure-vmm.png)



## <a name="replication-process"></a>Çoğaltma işlemi

![Hyper-V Azure çoğaltma](./media/hyper-v-azure-architecture/arch-hyperv-azure-workflow.png)

**Çoğaltma ve kurtarma işlemi**


### <a name="enable-protection"></a>Korumayı etkinleştir

1. Azure portalında veya şirket içinde bir Hyper-V VM’si için koruma etkinleştirdikten sonra, **Korumayı etkinleştir** başlatılır.
2. İş, makinenin önkoşullarla uyumlu olup olmadığını denetler, ardından, çoğaltmayı daha önce yapılandırdığınız ayarları uygulamak üzere [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) çağırır.
3. İş, tam bir VM çoğaltması başlatmak için [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) yöntemini çağırarak ilk çoğaltmayı başlatır ve VM’lerin sanal disklerini Azure’a gönderir.
4. **İşler** sekmesinde işi izleyebilirsiniz.      ![İşler listesi](media/hyper-v-azure-architecture/image1.png) ![Korumayı etkinleştir ayrıntıları](media/hyper-v-azure-architecture/image2.png)


### <a name="initial-data-replication"></a>İlk veri çoğaltma

1. İlk çoğaltma tetiklendiğinde bir [Hyper-V VM anlık görüntü](https://technet.microsoft.com/library/dd560637.aspx) anlık görüntü alınmaz.
2. Tüm Azure'a kopyalanacak kadar VM sanal sabit disklerde çoğaltılmış tek tek ' dir. Bu VM boyutuna bağlı olarak uzun sürebilir ve ağ bant genişliği olabilir. [Bilgi nasıl](https://support.microsoft.com/kb/3056159) ağ bant genişliğini artırmak için.
3. İlk çoğaltma işlemi devam ederken disk değişimi meydana gelirse Hyper-V çoğaltma çoğaltma İzleyicisi değişiklikleri Hyper-V çoğaltma günlükleri (.hrl) izler. Bu günlük dosyaları disklerle aynı klasörde yer alır. Her diskin ikincil depolamaya gönderilen bir ilişkili .hrl dosyası vardır. İlk çoğaltma sırasında anlık görüntü ve günlük dosyaları disk kaynaklarını kullanır.
4. İlk çoğaltma tamamlandığında, VM anlık görüntüsü silinir.
5. Günlükteki değişim disk değişiklikleri eşitlenir ve üst diske birleştirilir.


### <a name="finalize-protection-process"></a>Koruma İşlemi Sonlandır

1. İlk çoğaltma sonlandırıldıktan sonra **sanal makinede korumayı Sonlandır** işi çalıştırır. Böylece VM korumalı ağ ve diğer çoğaltma sonrası ayarlarını yapılandırır.
2. Bu aşamada yük devretme için hazır olduğundan emin olmak için VM ayarlarını denetleyebilirsiniz. Bir olağanüstü durum kurtarma ayrıntısı seçeneğini (yük devretme testi) üzerinde beklendiği gibi başarısız olduğunu denetlemek için VM için çalıştırabilirsiniz. 


## <a name="delta-replication"></a>Değişim çoğaltması

1. İlk çoğaltmadan sonra değişim çoğaltması, çoğaltma ilkesiyle uygun şekilde başlar.
2. Hyper-V çoğaltma çoğaltma İzleyicisi değişiklikleri .hrl dosyası bir sanal sabit disk izler. Çoğaltma için yapılandırılmış her diskin ilişkili bir .hrl dosyası vardır.
3. Günlüğü müşterinin depolama hesabına gönderilir. Bir günlük Azure geçişte olduğunda, birincil disk değişiklikleri aynı klasörde başka bir günlük dosyasında izlenir.
4. Başlangıç ve değişim çoğaltma sırasında Azure portalında VM izleyebilirsiniz.

### <a name="resynchronization-process"></a>Yeniden eşitleme işlemi

1. Değişim çoğaltması başarısız olursa ve bant genişliği ile zaman açısından tam çoğaltma maliyetli olacaksa, bir VM yeniden eşitleme için işaretlenir.
    - Örneğin, .hrl dosyası disk boyutunun %50'sine ulaşırsa VM, yeniden eşitleme için işaretlenir.
    -  Varsayılan olarak yeniden eşitleme ofis saatleri dışında otomatik olarak çalışacak şekilde zamanlanır.
1.  Yeniden eşitleme yalnızca değişim verilerini gönderir.
    - Kaynak ve hedef VM'lerin sağlama bilgi işlem tarafından gönderilen veri miktarını azaltır.
    - Burada kaynak ve hedef dosya sabit parçalara bölünür Sabit blok kümeleme algoritması kullanır.
    - Her bir öbeğin sağlama üretilir. Bu hedefe uygulanacak kaynağından hangi blokları gerektiğini belirlemek için karşılaştırılır.
2. Yeniden eşitleme sona erdikten sonra, normal değişim çoğaltması devam edecektir.
3. Varsayılan eşitleme saatleri dışında beklemek istemiyorsanız, bir VM el ile eşitleyebilirsiniz. Örneğin, bir kesinti oluşursa. Bu, Azure portalında yapmak için VM seçin > **eşitlemek**.

    ![El ile yeniden eşitleme](./media/hyper-v-azure-architecture/image4-site.png)


### <a name="retry-process"></a>İşlemi yeniden deneyin

Bir çoğaltma hatası meydana gelirse, yerleşik yeniden deneme işlevi vardır. Yeniden deneme tabloda açıklandığı şekilde sınıflandırılır.

**Kategori** | **Ayrıntılar**
--- | ---
**Kurtarılamaz hatalar** | Yeniden deneme yapılmaya çalışılmaz. VM durumu **Kritik** olacaktır ve yönetici müdahalesi gerekir.<br/><br/> Kopuk bir VHD zinciri, VM, ağ kimlik doğrulama hataları, yetkilendirme hataları, çoğaltma için geçersiz bir durum bu hataları örneklerindendir ve VM hataları (tek başına Hyper-V sunucuları için. bulunamadı.
**Kurtarılabilir hatalar** | Yeniden deneme aralığını ilk denemenin başlangıcına göre 1, 2, 4, 8 ve 10 dakika artıran bir üstel geri çekme kullanılarak her çoğaltma aralığında yeniden denemeler gerçekleşir. Hata devam ederse, 30 dakikada bir yeniden deneyin. Bunlara örnek olarak, ağ hataları, düşük disk hatalarını ve yetersiz bellek koşulları içerir.



## <a name="failover-and-failback-process"></a>Yük devretme ve yeniden çalışma işlemi

1. Azure için şirket içi Hyper-V Vm'lerden planlanmış veya planlanmamış bir yük devretme çalıştırabilirsiniz. Planlı bir yük devretme çalıştırırsanız, veri kaybı olmaması için kaynak VM’ler kapatılır. Birincil sitenizi erişilebilir durumda değilse, planlanmamış bir yük devretme çalıştırın.
2. Üzerinde tek bir makine başarısız veya birden çok makinelerin yük devretme düzenlemek için kurtarma planları oluşturabilirsiniz.
3. Bir yük devretme çalıştırın. Yük devretme ilk aşaması tamamlandıktan sonra oluşturulan çoğaltma sanal makineleri azure'da görmeye olmalıdır. Gerekli olursa VM’ye genel bir IP adresi atayabilirsiniz.
4. Ardından, iş yükü Azure VM çoğaltmasından erişme başlatmak için yük devretme de uygulayın.

Şirket içi altyapınızı yeniden çalışır olduktan sonra geri başarısız olabilir. Yeniden çalışma üç aşamada gerçekleşir:

1. Planlanmış bir yük devretme azure'dan şirket içi site dışı kazandırın:
    - **Kapalı kalma süresini en aza**: Bu seçeneği kullanırsanız, Site Recovery yük devretme önce verileri eşitler. Değiştirilen veri blokları denetler ve bunları şirket içi siteye sırasında Azure VM tutar çalışıyor, kapalı kalma süresini en aza indirir. Yük devretmeyi tamamlamanız el ile belirttiğinizde Azure VM'yi kapatın, son delta değişiklikleri kopyalanır ve yük devretme başlatır.
    - **Tam yükleme**: Bu seçenek ile yük devretme sırasında veriler eşitlenir. Bu seçenek, tüm disk yükler. Hiçbir sağlama hesaplanır, ancak daha fazla kapalı kalma süresi daha hızlıdır. Bu seçenek, Azure sanal makinelerini çoğaltma bazı süredir çalışmakta veya şirket içi VM silindiyse kullanın.
    - **VM oluşturma**: geri aynı VM veya alternatif bir VM başarısız seçeneğini belirleyebilirsiniz. Zaten yoksa Site Recovery VM oluşturacağını belirtebilirsiniz.

2. İlk eşitleme tamamlandıktan sonra yük devretmeyi tamamlamak için seçin. İşlem tamamlandıktan sonra her şeyin beklendiği gibi çalıştığını denetlemek için şirket içi VM oturum açabilir. Azure portalında Azure sanal makinelerini durdurulan görebilirsiniz.
3.  Ardından, işlemi sonlandırın ve iş yükü erişimi şirket içi sanal makineden yeniden başlatmak için yük devretme uygulayın.
4. İş yükleri geri başarısız olmuş sonra böylece şirket içi sanal makineleri yeniden azure'a çoğaltmayı tersine çevirme etkinleştirin.



## <a name="next-steps"></a>Sonraki adımlar


İzleyin [Bu öğretici](tutorial-prepare-azure.md) Hyper-V ile Azure çoğaltma başlamak için.


