---
title: Kapasite planlama ve azure'a Azure Site Recovery ile VMware olağanüstü durum kurtarma için ölçeklendirme | Microsoft Docs
description: Azure Site Recovery ile VMware vm'lerinin olağanüstü durum kurtarmayı ayarlarken kapasiteyi planlama ve ölçek için bu makaleyi kullanın
author: nsoneji
manager: garavd
ms.service: site-recovery
ms.date: 12/12/2018
ms.topic: conceptual
ms.author: mayg
ms.openlocfilehash: 6f644416a9e56009aadd0f8e1b217402d625af84
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2018
ms.locfileid: "53788744"
---
# <a name="plan-capacity-and-scaling-for-vmware-disaster-recovery-to-azure"></a>Kapasite ve ölçeklendirme Vmware'den azure'a olağanüstü durum kurtarma için planlama

Kapasite planlaması ve ölçeklendirme, şirket içi VMware Vm'leri ve fiziksel sunucuları azure'a çoğaltırken anlamak için bu makaleyi kullanın [Azure Site Recovery](site-recovery-overview.md).

## <a name="how-do-i-start-capacity-planning"></a>Kapasite planlaması nasıl başlarım?

Azure Site Recovery altyapısı gereksinimleri bilmeniz çalıştırarak çoğaltma ortamınız hakkında bilgi toplayın [Azure Site Recovery dağıtım Planlayıcısı](https://aka.ms/asr-deployment-planner-doc) VMware çoğaltması için. Bu araç hakkında [daha fazla bilgi edinin](site-recovery-deployment-planner.md). Bu araç, uyumlu ve uyumsuz VM'ler, VM başına disk üzerindeki tüm bilgileri içeren bir rapor sağlar ve disk başına veri değişim sıklığı. Araç ayrıca hedef RPO ve başarılı çoğaltma ve yük devretme testi için gereken Azure altyapısını karşılamak için ağ bant genişliği gereksinimlerini özetler.

## <a name="capacity-considerations"></a>Kapasite konuları

**Bileşen** | **Ayrıntılar** |
--- | --- | ---
**Çoğaltma** | **Maksimum günlük değişikliği oranı:** Korumalı makine yalnızca bir işlem sunucusu kullanabilirsiniz ve tek işlem sunucusu günlük işleyebilir değişiklik hızı 2 TB'a kadar artırın. Maksimum günlük veri değişikliği bir korumalı makine için desteklenen oranı böylece 2 TB'tır.<br/><br/> **En fazla aktarım hızı:** Çoğaltılmış bir makineden, azure'da bir depolama hesabına ait olabilir. Bir standart depolama hesabı en fazla saniye başına 20.000 istekleri işleyebilir ve 20. 000'için bir kaynak makine arasında (IOPS) saniyede giriş/çıkış işlemi sayısı tutmanızı öneririz. Örneğin, kaynak makinenin 5 disklerle olan ve her disk 120 IOPS (8 K boyut) kaynak makinede oluşturur, ardından bu azure'da 500 IOPS sınırı disk başına olacaktır. (Gerekli depolama hesabı sayısı 20,000 tarafından ayrılmış toplam kaynak makine IOPS, eşittir.)
**Yapılandırma sunucusu** | Yapılandırma sunucusu, korumalı makineler üzerinde çalışan tüm iş yükleri arasında günlük değişiklik hızı kapasitesi işleyebilmesi ve verileri Azure depolama alanına sürekli olarak çoğaltmak için yeterli bant genişliği gerekiyor.<br/><br/> En iyi uygulama, aynı ağ ve LAN kesimi yapılandırma sunucusunda, korumak istediğiniz makinelere bulun. Farklı bir ağda bulunan, ancak korumak istediğiniz makinelere Katman 3 ağ görünürlüğü ona sahip olmalıdır.<br/><br/> Yapılandırma sunucusu için boyut önerileri, aşağıdaki bölümde bulunan tablodaki özetlenmiştir.
**İşlem sunucusu** | İlk işlem sunucusu yapılandırma sunucusunda varsayılan olarak yüklenir. Ortamınızı ölçeklendirme için ek işlem sunucuları dağıtabilirsiniz. <br/><br/> İşlem sunucusu, korumalı makinelerden çoğaltma verilerini alıp ve önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir. Daha sonra Azure'a veri gönderir. İşlem sunucusu makinesi, bu görevleri gerçekleştirmek için yeterli kaynak olması gerekir.<br/><br/> İşlem sunucusu, disk tabanlı bir önbelleği kullanır. Bir ağ sorunu ya da kesinti olması durumunda depolanan veri değişikliklerini işlemek için 600 GB veya üzeri bir ayrı önbellek diski kullanın.

## <a name="size-recommendations-for-the-configuration-serverin-built-process-server"></a>Yapılandırma sunucusu/yerleşik işlem sunucusu için boyut önerileri

Her yapılandırma sunucusu aracılığıyla dağıtılan [OVF şablonunu](vmware-azure-deploy-configuration-server.md#deployment-of-configuration-server-through-ova-template) bir yerleşik bir işlem sunucusuna sahiptir. Yerleşik bir işlem sunucusu sanal makineleri korumak için kullanılırken, yapılandırma sunucusunun CPU, bellek, boş alan gibi kaynakların farklı bir fiyat karşılığında kullanılır. Bu nedenle, yerleşik bir işlem sunucusu yararlanıldığında gereksinimleri farklılık gösterir.
Yerleşik bir işlem sunucusu iş yükünü korumak için kullanıldığı bir yapılandırma sunucusu 200'e kadar sanal makineler aşağıdaki yapılandırmalarına göre işleyebilir.

**CPU** | **Bellek** | **Önbellek diski boyutu** | **Veri değişiklik oranı** | **Korumalı makineler**
--- | --- | --- | --- | ---
8 Vcpu (2 yuva * 4 çekirdek \@ 2,5 gigahertz [GHz]) | 16 GB | 300 GB | 500 GB veya daha az | 100 makineleri çoğaltabilir.
12 Vcpu (2 yuva * 6 çekirdek \@ 2.5 GHz) | 18 GB | 600 GB | 500 GB ila 1 TB | 100-150 makineler arasında çoğaltılır.
16 Vcpu (2 yuva * 8 çekirdek \@ 2.5 GHz) | 32 GB | 1 TB | 1 TB ile 2 TB | 150-200 makineler arasında çoğaltılır.
Başka bir yapılandırma sunucusunu dağıtma [OVF şablonu](vmware-azure-deploy-configuration-server.md#deployment-of-configuration-server-through-ova-template) | | | | 200'den fazla makineler çoğaltma yapıyorsanız, yeni yapılandırma sunucusu dağıtır.
Başka bir dağıtma [işlem sunucusu](vmware-azure-set-up-process-server-scale.md#download-installation-file) | | | &GT; 2 TB| Genel günlük veri değişikliği hızını 2 TB aşarsa, yeni bir genişleme işlem sunucusu dağıtın.

Konumlar:

* Her kaynak makine, 100 GB'lık 3 diskleri ile yapılandırıldı.
* 10 k RPM, 8 SAS sürücüleri Kıyaslama depolama RAID 10 ile önbellek diski ölçümleri için kullandık.

## <a name="size-recommendations-for-the-configuration-server"></a>Yapılandırma sunucusu için boyut önerileri

İşlem sunucusu olarak yapılandırma sunucusu kullanmayı planladığınız değil izlenmesi yapılandırma en fazla 650 sanal makineler işlemek için aşağıda verilen.

**CPU** | **RAM** | **İşletim sistemi disk boyutu** | **Veri değişiklik oranı** | **Korumalı makineler**
--- | --- | --- | --- | ---
24 Vcpu (2 yuva * 12 çekirdek \@ 2,5 gigahertz [GHz])| 32GB | 80 GB | Uygulanamaz | En fazla 650 VM'ler

Burada, her bir kaynak makine 100 GB'lık 3 diskle yapılandırılır.

Bu yana, işlem sunucusu işlevselliği kullanılmadı, veri değişikliği hızını geçerli değildir. Kapasite sağlamak için başka bir genişleme işlem yerleşik işlem sunucusundan iş yükünüze yönergeleri izleyerek geçebilirsiniz [burada](vmware-azure-manage-process-server.md#balance-the-load-on-process-server).

## <a name="size-recommendations-for-the-process-server"></a>İşlem sunucusu için boyut önerileri

İşlem sunucusu, Azure Site recovery'de veri çoğaltma işlemini gerçekleştirir bileşendir. Günlük değişim hızı 2 TB'den büyük ise, çoğaltma yükünü işlemek için bir genişleme işlem sunucusu eklemeniz gerekir. Ölçeği genişletmek için şunları yapabilirsiniz:

* Yapılandırma sunucusu arasında dağıtarak sayısını [OVF şablonunu](vmware-azure-deploy-configuration-server.md#deployment-of-configuration-server-through-ova-template). Örneğin, 400 makineleri iki yapılandırma sunucusu ile en fazla koruyabilirsiniz.
* Ekleme [genişleme işlem sunucusu](vmware-azure-set-up-process-server-scale.md#download-installation-file)ve bunlar yerine (veya ek olarak) çoğaltma trafiğini işlemek için yapılandırma sunucusu.

Aşağıdaki tabloda, bir senaryoda açıklanmaktadır:

* Bir genişleme işlem sunucusu ayarlamadıysanız ayarladınız.
* Korunan sanal makinelerin genişleme işlem sunucusu kullanacak şekilde yapılandırdınız.
* Her bir korumalı kaynak makine, 100 GB'lık üç diskleri ile yapılandırıldı.

**Ek işlem sunucusu** | **Önbellek diski boyutu** | **Veri değişiklik oranı** | **Korumalı makineler**
--- | --- | --- | ---
4 Vcpu (2 yuva * 2 Çekirdek \@ 2.5 GHz), 8 GB bellek | 300 GB | 250 GB veya daha az | 85 veya daha az makineleri çoğaltabilir.
8 Vcpu (2 yuva * 4 çekirdek \@ 2.5 GHz), 12 GB bellek | 600 GB | 250 GB ila 1 TB | 85 150 makineler arasında çoğaltılır.
12 Vcpu (2 yuva * 6 çekirdek \@ 2.5 GHz) 24 GB bellek | 1 TB | 1 TB ile 2 TB | 150-225 makineler arasında çoğaltılır.

Bir ölçek büyütme veya ölçek genişletme modeli için tercihinizi sunucularınızın ölçeği şekilde bağlıdır.  Bazı gelişmiş yapılandırma ve işlem sunucusu dağıtarak ölçeği büyütün veya daha az kaynak ile daha fazla sunucu dağıtarak ölçeği genişletme. Örneğin, genel veri değişikliği hızını günlük 1,5 TB'lık birlikte için 200 makine korumanız gerekiyorsa, şunlardan birini yapabilirsiniz:

* 16 vCPU, 24 GB RAM ile tek bir işlem sunucusu ayarlayın.
* İki işlem sunucularını (2 x 8 vCPU, 2 * 12 GB RAM) ayarlayın.

## <a name="control-network-bandwidth"></a>Ağ bant genişliğini denetlemek

Kullandığınız sonra [dağıtım Planlayıcısı aracını](site-recovery-deployment-planner.md) (ilk çoğaltma ve ardından değişim) çoğaltma için gereken bant genişliğini hesaplamak için birkaç seçenek kullanarak çoğaltma için kullanılan bant genişliği miktarını kontrol edebilirsiniz:

* **Bant genişliğini kısıtlama**: Azure'a çoğaltılan VMware trafiği belirli bir işlem sunucusu üzerinden gider. İşlem sunucusu çalışan makineler üzerinde bant genişliğini kısıtlayabilirsiniz.
* **Bant genişliği üzerinde etki**: Birkaç kayıt defteri anahtarlarını kullanarak çoğaltma için kullanılan bant genişliği üzerinde etki oluşturabilirsiniz:
  * **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM** kayıt defteri değeri, veri aktarımı bir disk için (başlangıç ve değişim çoğaltması) kullanılan iş parçacığı sayısını belirtir. Daha yüksek bir değer, çoğaltma işlemi için kullanılan ağ bant genişliğini artırır.
  * **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\DownloadThreadsPerVM** yeniden çalışma sırasında veri aktarımı için kullanılan iş parçacıklarının sayısını belirtir.

### <a name="throttle-bandwidth"></a>Bant genişliğini kısıtlama

1. İşlem sunucusu davranan makineye Azure Backup MMC ek bileşenini açın. Varsayılan olarak, bir kısayol yedekleme için masaüstünde veya aşağıdaki klasörde bulunur: C:\Program Files\Microsoft Azure Recovery Services Agent\bin.
2. Ek bileşende **Özellikleri Değiştir**'e tıklayın.

    ![Özellikleri değiştirmek için ekran Azure Backup MMC ek bileşenini seçeneği](./media/site-recovery-vmware-to-azure/throttle1.png)
3. Üzerinde **azaltma** sekmesinde **yedekleme işlemleri için internet bant genişliği kullanımını azaltmayı etkinleştir**. Ve çalışma dışı saatler için sınırları ayarlayın. Geçerli aralıklar 1023 MB/sn saniye başına 512 Kbps ila üzeresiniz.

    ![Azure yedekleme özellikleri ekran iletişim kutusu](./media/site-recovery-vmware-to-azure/throttle2.png)

Ayrıca, azaltma ayarı için [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet'ini de kullanabilirsiniz. Bu ayara ilişkin örneği aşağıda bulabilirsiniz:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting - NoThrottle** hiçbir azaltmanın gerekli olmadığını gösterir.

### <a name="influence-network-bandwidth-for-a-vm"></a>Bir VM için ağ bant genişliğini etkiler

1. Sanal makinenin kayıt defterine gidin **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * Çoğaltılan disk üzerindeki bant genişliği trafiğini etkilemek için değerini değiştirmek **UploadThreadsPerVM**, veya anahtar yoksa anahtar oluşturun.
   * Azure'dan yeniden çalışma trafiği için bant genişliği etkilemek için değerini değiştirmek **DownloadThreadsPerVM**.
2. Varsayılan değer 4'tür. "Fazla sağlanan" bir ağda, bu kayıt defteri anahtarlarının varsayılan değerlerinin değiştirilmesi gerekir. Maksimum değer 32'dir. Değeri iyileştirmek için trafiği izleyin.

## <a name="setup-azure-site-recovery-infrastructure-to-protect-more-than-500-virtual-machines"></a>500'den fazla sanal makineyi korumak için Azure Site Recovery altyapı Kurulumu

Azure Site Recovery altyapısı ayarlama önce aşağıdaki faktörleri ölçmek için ortamı erişmeniz gerekir: uyumlu sanal makineler, günlük veri RPO, Azure site kurtarma sayısı için gerekli ağ bant genişliği Hızı değiştirin gerekli bileşenler, vb. ilk çoğaltmayı tamamlamak için geçen süre.,

1. Dağıtım Planlayıcısını çalıştırın, ortamınıza paylaşılan yönergeleri, Yardım için bu parametreleri ölçmek için olun [burada](site-recovery-deployment-planner.md).
2. Belirtilen gereksinimleri ile bir yapılandırma sunucusunu dağıtma [burada](site-recovery-plan-capacity-vmware.md#size-recommendations-for-the-configuration-server). Üretim iş yükünüz 650 sanal makineler aşarsa, başka bir yapılandırma sunucusu dağıtır.
3. Ölçülen günlük veri değişikliği hızınıza göre dağıtma [genişleme işlem sunucusu](vmware-azure-set-up-process-server-scale.md#download-installation-file) belirtilen boyutu yönergeler yardımıyla [burada](site-recovery-plan-capacity-vmware.md#size-recommendations-for-the-process-server).
4. Bir disk sanal makineye 2 MB/sn aşacağı için veri değişim oranı bekliyorsanız, emin olmak için [bir premium depolama hesabı ayarlama](tutorial-prepare-azure.md#create-a-storage-account). Belirli bir süre için dağıtım Planlayıcısı çalıştırıldıktan sonra veri yükündeki hızını nokta raporda yakalanan değil başka bir zaman sırasında değiştirin.
5. İstenen RPO'ya göre [ağ bant genişliğini](site-recovery-plan-capacity-vmware.md#control-network-bandwidth).
6. Altyapıyı ayarladıktan sonra altında yayımlanan faydalandığından [yapılır bölümü](vmware-azure-set-up-source.md) yükünüze olağanüstü durum kurtarma sağlamak.

## <a name="deploy-additional-process-servers"></a>Ek işlem sunucusu dağıtma

Varsa ölçeklendirmek için çıkış dağıtımınızı 200 kaynak makineler ötesinde veya günlük 2 TB'den fazla oranını karmaşıklığı toplam sahip, trafik hacmini işlemeye yetecek ek işlem sunucularının gerekir. Üzerinde verilen yönergeleri izleyerek [bu makalede](vmware-azure-set-up-process-server-scale.md) işlem sunucusu ayarlanamıyor. Sunucuyu ayarladıktan sonra bunu kullanmak için kaynak makineleri geçirebilirsiniz.

### <a name="migrate-machines-to-use-the-new-process-server"></a>Yeni işlem sunucusu kullanabilmek için makineleri geçirme

1. İçinde **ayarları** > **Site Recovery sunucuları**, yapılandırma sunucusuna tıklayın ve ardından **işlem sunucuları**.

    ![İşlem sunucusu ekran iletişim kutusu](./media/site-recovery-vmware-to-azure/migrate-ps2.png)
2. İşlem sunucusu şu anda kullanımda sağ tıklayın ve **anahtar**.

    ![Ekran görüntüsü, yapılandırma sunucusu iletişim kutusu](./media/site-recovery-vmware-to-azure/migrate-ps3.png)
3. İçinde **Select hedef işlem sunucusunu**, kullanmak istediğiniz yeni işlem sunucusunu seçin ve ardından sunucu işleyecek olan sanal makineleri seçin. Sunucu hakkındaki bilgileri almak için bilgi simgesine tıklayın. Yük kararları vermenize yardımcı olmak için seçilen her bir sanal makine için yeni işlem sunucusu çoğaltmak için gereken ortalama alanı görüntülenir. Yeni işlem sunucusuna çoğaltmaya başlamak için onay işaretine tıklayın.

## <a name="deploy-additional-master-target-servers"></a>Ek ana hedef sunucuları dağıtma

Aşağıdaki senaryolarda sırasında ek ana hedef sunucu gerekir

1. Linux tabanlı bir sanal makine korumaya çalışıyorsanız.
2. Ana hedef sunucusu yapılandırma sunucusunda kullanılabilir VM veri deposuna erişimi yok
3. Disk yöneticisinde toplam sayısı (Hayır. sunucu hedefliyorsanız Sunucu üzerindeki yerel disklerden) + korunacak disklerin 60 diskleri aşıyor.

İçin yeni bir ana hedef sunucusu eklemek için **Linux tabanlı sanal makine**, [Buraya](vmware-azure-install-linux-master-target.md).

İçin **Windows tabanlı sanal makine**, izleyerek aşağıdaki yönergeler verilir.

1. Gidin **kurtarma Hizmetleri kasası** > **Site Recovery altyapısı** > **yapılandırma sunucusu**.
2. Gerekli yapılandırma sunucusunda tıklayın > **+ ana hedef sunucu**.![ Ekle-master-target-server.png](media/site-recovery-plan-capacity-vmware/add-master-target-server.png)
3. Birleşik Kurulum indirme ve çalıştırma, ana hedef sunucusu'kurmak için VM üzerinde.
4. Seçin **yükleme ana hedef** > **sonraki**. ![MT.PNG seçin](media/site-recovery-plan-capacity-vmware/choose-MT.PNG)
5. Varsayılan yükleme konumu seçin > tıklatın **yükleme**. ![MT yüklemesi](media/site-recovery-plan-capacity-vmware/MT-installation.PNG)
6. Tıklayarak **yapılandırmasına devam** ana hedef yapılandırma sunucusu ile kayıt. ![MT devam configuration.PNG](media/site-recovery-plan-capacity-vmware/MT-proceed-configuration.PNG)
7. Yapılandırma sunucusu ve parola IP adresini girin. [Buraya](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase) parola oluşturmayı öğrenin.![ cs IP parola](media/site-recovery-plan-capacity-vmware/cs-ip-passphrase.PNG)
8. Tıklayın **kaydetme** post kayıt tıklatıp **son**.
9. Başarılı kayıt sırasında bu sunucu altında portalında listelenen **kurtarma Hizmetleri kasası** > **Site Recovery altyapısı** > **yapılandırma sunucuları** > ana hedef sunucular ilgili yapılandırma sunucusu.

 >[!NOTE]
 >Ayrıca ana en son sürümünü indirebilirsiniz Windows için hedef sunucu birleşik Kurulum [burada](https://aka.ms/latestmobsvc).

## <a name="next-steps"></a>Sonraki adımlar

İndirme ve çalıştırma [Azure Site Recovery dağıtım Planlayıcısı](https://aka.ms/asr-deployment-planner)
