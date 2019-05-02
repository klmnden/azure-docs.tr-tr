---
title: Kapasite planlama ve Vmware'den azure'a olağanüstü durum kurtarma için Azure Site Recovery kullanarak ölçeklendirme | Microsoft Docs
description: Bu makale size gibi kapasite ve Azure Site Recovery kullanılarak Azure'da VMware vm'lerinin olağanüstü durum kurtarma oluşturduğunuzda ölçeklendirme planı.
author: nsoneji
manager: garavd
ms.service: site-recovery
ms.date: 4/9/2019
ms.topic: conceptual
ms.author: ramamill
ms.openlocfilehash: 9a77b3982d8aed6ae694c32baecd7ae194c51724
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64924838"
---
# <a name="plan-capacity-and-scaling-for-vmware-disaster-recovery-to-azure"></a>Kapasite ve ölçeklendirme Vmware'den azure'a olağanüstü durum kurtarma için planlama

Kapasite ve şirket içi VMware Vm'leri ve fiziksel sunucuları Azure'a kullanarak çoğalttığınızda ölçeklendirme planlamak için bu makaleyi kullanın [Azure Site Recovery](site-recovery-overview.md).

## <a name="how-do-i-start-capacity-planning"></a>Kapasite planlaması nasıl başlarım?

Azure Site Recovery altyapı gereksinimleri hakkında bilgi edinmek için çalıştırarak çoğaltma ortamınız hakkında bilgi toplayın [Azure Site Recovery dağıtım Planlayıcısı](https://aka.ms/asr-deployment-planner-doc) VMware çoğaltması için. Daha fazla bilgi için [hakkında Site Recovery dağıtım Planlayıcısı vmware'den azure'a](site-recovery-deployment-planner.md). 

Site Recovery dağıtım Planlayıcısı uyumlu ve uyumsuz VM'ler, VM başına disk sayısı ile ilgili eksiksiz bilgi sahip bir rapor sağlar ve disk başına veri değişim sıklığı. Araç ayrıca hedef RPO ve başarılı çoğaltma ve yük devretme testi için gereken Azure altyapısını karşılamak için ağ bant genişliği gereksinimlerini özetler.

## <a name="capacity-considerations"></a>Kapasite konuları

Bileşen | Ayrıntılar
--- | ---
**Çoğaltma** | **Maksimum günlük değişim hızı**: Korumalı makine yalnızca bir işlem sunucusunu kullanabilirsiniz. Bir tek işlem sunucusu günlük işleyebilir değişiklik hızı 2 TB'a kadar artırın. Bu nedenle, en fazla günlük veri değişikliği bir korumalı makine için desteklenen oranı 2 TB olduğu.<br /><br /> **En yüksek aktarım**: Çoğaltılmış bir makineden, azure'da bir depolama hesabına ait olabilir. Standart Azure depolama hesabı, en fazla saniye başına 20.000 istekleri işleyebilir. 20.000 için bir kaynak makine arasında giriş/çıkış işlemi (IOPS) saniyede sayısını sınırlayın öneririz. Örneğin, beş diskleri olan bir kaynak makine varsa ve her disk 120 IOPS (8 K boyutu) kaynak makinede oluşturur. kaynak makine 500 Azure disk başına IOPS sınırı içinde olur. (Gerekli depolama hesabı sayısı 20. 000'ile ayrılmış IOPS toplam kaynak makineye eşittir.)
**Yapılandırma sunucusu** | Yapılandırma sunucusu korumalı makineler üzerinde çalışan tüm iş yükleri arasında günlük değişiklik hızı kapasitesi işleyebilir olması gerekir. Makine yapılandırma verilerini Azure Depolama'ya sürekli olarak çoğaltmak için yeterli bant genişliği olması gerekir.<br /><br /> Yapılandırma sunucusu korumak istediğiniz makinelere LAN kesimi ve aynı ağ üzerinde yerleştirmek iyi bir uygulamadır. Yapılandırma sunucusunu farklı bir ağda koyabilirsiniz ancak korumak istediğiniz makinelere Katman 3 ağ görünürlüğü olması gerekir.<br /><br /> Yapılandırma sunucusu için boyut önerileri, aşağıdaki bölümde bulunan tablodaki özetlenmiştir.
**İşlem sunucusu** | İlk işlem sunucusu yapılandırma sunucusunda varsayılan olarak yüklenir. Ortamınızı ölçeklendirme için ek işlem sunucuları dağıtabilirsiniz. <br /><br /> İşlem sunucusu, korumalı makinelerden çoğaltma verilerini alır. İşlem sunucusu, önbelleğe alma, sıkıştırma ve şifreleme kullanarak verileri iyileştirir. Ardından, işlem sunucusu verileri Azure'a gönderir. İşlem sunucusu makinesi, bu görevleri gerçekleştirmek için yeterli kaynak olması gerekir.<br /><br /> İşlem sunucusu, disk tabanlı bir önbelleği kullanır. Bir ağ sorununu veya kesinti oluşursa, depolanan veri değişikliklerini işlemek için 600 GB veya üzeri bir ayrı önbellek diski kullanın.

## <a name="size-recommendations-for-the-configuration-server-and-inbuilt-process-server"></a>Boyut önerileri ve yerleşik yapılandırma sunucusu için sunucu işlemleri

İş yükünü korumak için bir yerleşik işlem sunucusunu kullanan bir yapılandırma sunucusu, aşağıdaki yapılandırmalarına göre 200'e kadar sanal makineler işleyebilir:

CPU | Bellek | Önbellek diski boyutu | Veri değişiklik oranı | Korumalı makineler
--- | --- | --- | --- | ---
8 Vcpu (2 yuva * 4 çekirdek \@ 2.5 GHz) | 16 GB | 300 GB | 500 GB veya daha az | 100'den az makineleri çoğaltmak için kullanın.
12 Vcpu (2 yuva * 6 çekirdek \@ 2.5 GHz) | 18 GB | 600 GB | 501 GB ila 1 TB | 100-150 makineleri çoğaltmak için kullanın.
16 Vcpu (2 yuva * 8 çekirdek \@ 2.5 GHz) | 32 GB | 1 TB | > 1 TB ile 2 TB | 200 151 makineleri çoğaltmak için kullanın.
Kullanarak başka bir yapılandırma sunucusunu dağıtma bir [OVF şablonunu](vmware-azure-deploy-configuration-server.md#deployment-of-configuration-server-through-ova-template). | | | | 200'den fazla makineler çoğaltma yapıyorsanız, yeni bir yapılandırma sunucusu dağıtın.
Başka bir dağıtma [işlem sunucusu](vmware-azure-set-up-process-server-scale.md#download-installation-file). | | | &GT; 2 TB| Genel günlük veri değişikliği hızınıza 2 TB'den büyük ise, yeni bir genişleme işlem sunucusu dağıtın.

Bu yapılandırmalarda:

* Her kaynak makinenin, üç disk 100 GB'lık vardır.
* 10 k RPM sekiz paylaşılan erişim imzası sürücülerin Kıyaslama depolama RAID 10 ile önbellek diski ölçümleri için kullandık.

## <a name="size-recommendations-for-the-process-server"></a>İşlem sunucusu için boyut önerileri

İşlem sunucusu, Azure Site recovery'de veri çoğaltma işlemini gerçekleştirir bileşendir. Günlük değişim hızı 2 TB'den büyük ise, çoğaltma yükünü işlemek için genişleme işlem sunucusu eklemeniz gerekir. Ölçeği genişletmek için şunları yapabilirsiniz:

* Yapılandırma sunucusu kullanarak dağıtarak sayısını bir [OVF şablonunu](vmware-azure-deploy-configuration-server.md#deployment-of-configuration-server-through-ova-template). Örneğin, 400 makineler, iki yapılandırma sunucusu kullanarak koruyabilirsiniz.
* Ekleme [genişleme işlem sunucusu](vmware-azure-set-up-process-server-scale.md#download-installation-file). Çoğaltma trafiği yerine (veya ek olarak) işlemek için ölçeği genişletilmiş işlem sunucularını kullanın yapılandırma sunucusu.

Aşağıdaki tabloda bu senaryo anlatılmaktadır:

* Bir genişleme işlem sunucusu ayarlayın.
* Korunan sanal makinelerin genişleme işlem sunucusu kullanacak şekilde yapılandırılmış.
* Her korumalı kaynak makinenin, üç disk 100 GB vardır.

Ek işlem sunucusu | Önbellek diski boyutu | Veri değişiklik oranı | Korumalı makineler
--- | --- | --- | ---
4 Vcpu (2 yuva * 2 Çekirdek \@ 2.5 GHz), 8 GB bellek | 300 GB | 250 GB veya daha az | 85 veya daha az makineleri çoğaltmak için kullanın.
8 Vcpu (2 yuva * 4 çekirdek \@ 2.5 GHz), 12 GB bellek | 600 GB | 251 GB ila 1 TB | -86 150 makineleri çoğaltmak için kullanın.
12 Vcpu (2 yuva * 6 çekirdek \@ 2.5 GHz) 24 GB bellek | 1 TB | > 1 TB ile 2 TB | 151 için 225 makineleri çoğaltmak için kullanın.

Bir ölçek büyütme veya ölçek genişletme modeli için tercihinizi sunucularınızın nasıl ölçekleme bağlıdır. Ölçeği artırma işleminin bazı gelişmiş yapılandırma sunucusu dağıtma ve sunucuların işleme. Ölçeği genişletmek için daha az kaynak sahip daha fazla sunucu dağıtın. Örneğin, 200 makine korumak istiyorsanız ile bir genel günlük veri değişim oranı 1,5 TB'lık, aşağıdaki eylemlerden birini gerçekleştirebilir:

* Bir tek işlem sunucusunu (16 vCPU, 24 GB RAM) ayarlayın.
* İki işlem sunucularını (2 x 8 vCPU, 2 * 12 GB RAM) ayarlayın.

## <a name="control-network-bandwidth"></a>Ağ bant genişliğini denetlemek

Kullandıktan sonra [Site Recovery dağıtım Planlayıcısı](site-recovery-deployment-planner.md) (ilk çoğaltma ve ardından değişim) çoğaltma için gereken bant genişliğini hesaplamak için kullanılan bant genişliği miktarını denetleme seçenekleri birkaç vardır. çoğaltma:

* **Bant genişliğini kısıtlama**: Azure'a çoğaltılan VMware trafiği belirli bir işlem sunucusu üzerinden gider. İşlem sunucusu çalışan sanal makinelerin bant genişliğini kısıtlayabilirsiniz.
* **Bant genişliği üzerinde etki**: Birkaç kayıt defteri anahtarlarını kullanarak çoğaltma için kullanılan bant genişliği üzerinde etki oluşturabilirsiniz:
  * **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM** kayıt defteri değeri, veri aktarımı bir disk için (başlangıç ve değişim çoğaltması) kullanılan iş parçacığı sayısını belirtir. Daha yüksek bir değer, çoğaltma için kullanılan ağ bant genişliğini artırır.
  * **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\DownloadThreadsPerVM** kayıt defteri değeri, yeniden çalışma sırasında veri aktarımı için kullanılan iş parçacığı sayısını belirtir.

### <a name="throttle-bandwidth"></a>Bant genişliğini kısıtlama

1. İşlem sunucusu kullandığınız makineye Azure Backup MMC ek bileşenini açın. Varsayılan olarak, bir kısayol yedekleme için masaüstünde veya aşağıdaki klasörde bulunur: C:\Program Files\Microsoft Azure Recovery Services Agent\bin.
2. Ek bileşeninde, seçin **özelliklerini değiştirme**.

    ![Özelliklerini değiştirmek için Azure Backup MMC ek bileşenini seçeneğinin ekran görüntüsü](./media/site-recovery-vmware-to-azure/throttle1.png)
3. Üzerinde **azaltma** sekmesinde **yedekleme işlemleri için internet bant genişliği kullanımını azaltmayı etkinleştir**. Ve çalışma dışı saatler için sınırları ayarlayın. Geçerli aralıklar 512 Kbps ila 1,023 MB/sn olan.

    ![Azure yedekleme Özellikleri iletişim kutusunun ekran görüntüsü](./media/site-recovery-vmware-to-azure/throttle2.png)

Ayrıca, azaltma ayarı için [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet'ini de kullanabilirsiniz. Bir örneği aşağıda verilmiştir:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting - NoThrottle** hiçbir azaltmanın gerekli olmadığını gösterir.

### <a name="alter-the-network-bandwidth-for-a-vm"></a>Bir VM için ağ bant genişliğini değiştirme

1. Sanal makinenin kayıt defterine gidin **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * Çoğaltılan disk üzerindeki bant genişliği trafiğini değiştirmek için değerini değiştirmek **UploadThreadsPerVM**. Yoksa anahtar oluşturun.
   * Azure'dan yeniden çalışma trafiği için bant genişliği değiştirmek için değerini değiştirmek **DownloadThreadsPerVM**.
2. Her anahtar için varsayılan değer **4**. "Fazla sağlanan" bir ağda, bu kayıt defteri anahtarlarının varsayılan değerlerinin değiştirilmesi gerekir. Kullanabileceğiniz maksimum değer **32**. Değeri iyileştirmek için trafiği izleyin.

## <a name="set-up-the-site-recovery-infrastructure-to-protect-more-than-500-vms"></a>Site Recovery altyapı Kurulumu 500'den fazla Vm'leri korumak için ayarlayın

Site Recovery altyapı Kurulumu ayarlamadan önce aşağıdaki etmenlere ölçmek için ortam erişim: uyumlu sanal makinelerin günlük veri değişiklik oranı, gerekli ağ bant genişliği elde etmek için Site Recovery sayısını istediğiniz RPO için gerekli bileşenleri ve ilk çoğaltmayı tamamlamak için gereken süreyi. Gerekli bilgileri toplamak için aşağıdaki adımları tamamlayın:

1. Bu parametreleri ölçmek için Site Recovery dağıtım Planlayıcısı ortamınızda çalıştırın. Faydalı yönergeleri için bkz. [hakkında Site Recovery dağıtım Planlayıcısı vmware'den azure'a](site-recovery-deployment-planner.md).
2. Uygun bir yapılandırma sunucusunu dağıtma [yapılandırma sunucusuna yönelik boyut önerileri](site-recovery-plan-capacity-vmware.md#size-recommendations-for-the-configuration-server-and-inbuilt-process-server). Üretim iş yükünüz 650 sanal makineler aşarsa, başka bir yapılandırma sunucusu dağıtır.
3. Ölçülen günlük veri değişikliği hızınıza göre dağıtma [genişleme işlem sunucusu](vmware-azure-set-up-process-server-scale.md#download-installation-file) yardımıyla [boyut yönergeleri](site-recovery-plan-capacity-vmware.md#size-recommendations-for-the-process-server).
4. Veri değişikliği oranı 2 MB/sn aşmayı disk sanal makineye ilişkin bekliyorsanız, premium yönetilen diskler kullandığınızdan emin olun. Site Recovery dağıtım Planlayıcısı, belirli bir süre için çalışır. Diğer zamanlarda veri değişim hızı yükündeki raporda yakalanan değil.
5. [Ağ bant genişliğini](site-recovery-plan-capacity-vmware.md#control-network-bandwidth) elde etmek istediğiniz RPO üzerinde temel.
6. Altyapısını ayarlarken, olağanüstü durum kurtarma iş yükünüz için etkinleştirin. Bilgi edinmek için bkz. nasıl [Azure'a çoğaltma için VMware için kaynak ortamı ayarlama](vmware-azure-set-up-source.md).

## <a name="deploy-additional-process-servers"></a>Ek işlem sunucusu dağıtma

Dağıtımınız kaynak için 200 makine ötesinde ölçeği veya günlük 2 TB'den fazla oranını karmaşıklığı toplam varsa, trafik hacmini işlemeye yetecek işlem sunucusu eklemeniz gerekir. Ürün sağlamak 9.24 sürümünde geliştirdik [işlem sunucu uyarılarını](vmware-physical-azure-monitor-process-server.md#process-server-alerts) ne zaman bir genişleme işlem sunucusu kurma hakkında. [İşlem sunucusu kurma](vmware-azure-set-up-process-server-scale.md) yeni kaynak makineyi korumak için veya [Yük Dengelemesi](vmware-azure-manage-process-server.md#move-vms-to-balance-the-process-server-load).

### <a name="migrate-machines-to-use-the-new-process-server"></a>Yeni işlem sunucusu kullanabilmek için makineleri geçirme

1. Seçin **ayarları** > **Site Recovery sunucuları**. Yapılandırma sunucusunu seçin ve ardından **işlem sunucuları**.

    ![İşlem Sunucusu iletişim kutusunun ekran görüntüsü](./media/site-recovery-vmware-to-azure/migrate-ps2.png)
2. Şu anda kullanımda işlem sunucusuna sağ tıklayın ve ardından **anahtar**.

    ![Yapılandırma sunucusu iletişim kutusunun ekran görüntüsü](./media/site-recovery-vmware-to-azure/migrate-ps3.png)
3. İçinde **Select hedef işlem sunucusunu**, kullanmak istediğiniz yeni işlem sunucusunu seçin. Ardından sunucu işleyecek olan sanal makineleri seçin. Sunucu hakkındaki bilgileri almak için bilgi simgesini seçin. Yük kararları vermenize yardımcı olmak için seçilen her bir sanal makine için yeni işlem sunucusu çoğaltmak için gereken ortalama alanı gösterilir. Yeni işlem sunucusuna çoğaltmaya başlamak için onay işaretini seçin.

## <a name="deploy-additional-master-target-servers"></a>Ek ana hedef sunucuları dağıtma

Aşağıdaki senaryolarda, birden fazla ana hedef sunucusunda gereklidir:

*   Linux tabanlı bir sanal makine korumak istiyorsunuz.
*   Ana hedef sunucusu yapılandırma sunucusunda kullanılabilir VM veri deposuna erişimi yok.
*   Disk (sunucu üzerindeki yerel disklerden sayısı) yanı sıra korunacak disklerin ana hedef sunucusunda toplam sayısını 60 disklerden daha büyüktür.

Linux tabanlı bir sanal makine için bir ana hedef sunucusu ekleme konusunda bilgi edinmek için [bir Linux ana hedef sunucusu yeniden çalışma için yükleme](vmware-azure-install-linux-master-target.md).

Windows tabanlı bir sanal makine için bir ana hedef sunucusu eklemek için:

1. Git **kurtarma Hizmetleri kasası** > **Site Recovery altyapısı** > **yapılandırma sunucusu**.
2. Gerekli yapılandırma sunucusunu seçin ve ardından **ana hedef sunucu**.

    ![Ana hedef Sunucusu Ekle düğmesini gösteren ekran görüntüsü](media/site-recovery-plan-capacity-vmware/add-master-target-server.png)
3. Birleşik kurulum dosyasını indirirsiniz ve ardından dosya ana hedef sunucusu kurmak için VM üzerinde çalıştırın.
4. Seçin **yükleme ana hedef** > **sonraki**.

    ![Ana hedef yükleme seçeneğini gösteren ekran görüntüsü](media/site-recovery-plan-capacity-vmware/choose-MT.PNG)
5. Varsayılan yükleme konumu seçin ve ardından **yükleme**.

     ![Varsayılan yükleme konumu gösteren ekran görüntüsü](media/site-recovery-plan-capacity-vmware/MT-installation.PNG)
6. Yapılandırma sunucusu ile ana hedef kaydetmek için işaretleyin **yapılandırma devam**.

    ![Devam etmek için Yapılandır düğmesini gösteren ekran görüntüsü](media/site-recovery-plan-capacity-vmware/MT-proceed-configuration.PNG)
7. Yapılandırma sunucusunun IP adresini girin ve parolayı girin. Bir parola oluşturmak nasıl öğrenmek için bkz. [yapılandırma sunucusu parolası oluşturma](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase). 

    ![Yapılandırma sunucusunun IP adresini ve parolayı girin nerede gösteren ekran görüntüsü](media/site-recovery-plan-capacity-vmware/cs-ip-passphrase.PNG)
8. **Kaydol**’u seçin. Kayıt tamamlandığında seçin **son**.

Kayıt başarıyla tamamlandığında sunucu adresindeki Azure portalında listelenir **kurtarma Hizmetleri kasası** > **Site Recovery altyapısı**  >   **Yapılandırma sunucusu**, yapılandırma sunucusunun ana hedef sunucular.

 > [!NOTE]
 > En son sürümünü indirin [Windows için ana hedef sunucusu birleşik kurulum dosyasını](https://aka.ms/latestmobsvc).

## <a name="next-steps"></a>Sonraki adımlar

İndirme ve çalıştırma [Site Recovery dağıtım Planlayıcısı](https://aka.ms/asr-deployment-planner).
