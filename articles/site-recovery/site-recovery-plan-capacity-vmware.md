---
title: "Kapasite planlama ve Azure Site Recovery ile Azure'da VMware çoğaltma işlemini ölçeklendirme | Microsoft Docs"
description: "VMware Vm'leri Azure Site Recovery ile azure'a çoğaltırken planı kapasite ve ölçek bu makaleyi kullanın"
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 02/07/2018
ms.author: rayne
ms.openlocfilehash: 02f5a7270b5d8b7657a585fce99946cff8ed8d67
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="plan-capacity-and-scaling-for-vmware-replication-with-azure-site-recovery"></a>Kapasite ve Azure Site Recovery ile VMware çoğaltma için ölçeklendirme planı

Bu makale için kapasite planlaması ve ölçeklendirme, şirket içi VMware Vm'lerini ve fiziksel sunucuları ile azure'a çoğaltırken ekleneceğini kullanmak [Azure Site Recovery](site-recovery-overview.md).

## <a name="how-do-i-start-capacity-planning"></a>Kapasite planlama nasıl başlamanız gerekir?

Çalıştırarak çoğaltma ortamınız hakkında bilgi toplayın [Azure Site Recovery dağıtımı Planlayıcısı](https://aka.ms/asr-deployment-planner-doc) VMware çoğaltma için. [Daha fazla bilgi edinin](site-recovery-deployment-planner.md) bu araç hakkında. Uyumlu ve uyumsuz VM'ler, VM başına disk hakkında bilgi toplamanız ve disk başına veri karmaşıklığı. Aracı, ağ bant genişliği gereksinimlerini ve başarılı çoğaltma ve test yük devretme için gereken Azure altyapı da kapsar.

## <a name="capacity-considerations"></a>Kapasite dikkat edilecek noktalar

**Bileşen** | **Ayrıntılar** |
--- | --- | ---
**Çoğaltma** | **Maksimum günlük değişim oranı:** korumalı makine yalnızca bir işlem sunucusunu kullanabilir ve tek işlem sunucusu günlük işleyebilir değişiklik 2 TB oranı. Bu nedenle 2 TB maksimum günlük veri korumalı bir makine için desteklenen oranı değiştirmektir.<br/><br/> **En yüksek verimlilik:** çoğaltılmış bir makineden azure'da bir depolama hesabına ait olabilir. Standart depolama hesabı 20.000 istekleri saniye başına maksimum işleyebilir ve 20. 000'için kaynak makine arasında girdi/çıktı işlemleri (IOPS) saniye başına sayısı tutmanızı öneririz. Örneğin, kaynak makine 5 disklerle varsa ve her disk 120 IOPS (8 K boyut) kaynak makinede oluşturur, ardından onu Azure başına 500 disk IOPS sınırı içinde olacaktır. (Gerekli depolama hesabı sayısı tarafından 20.000 bölünmüş toplam kaynak makine IOPS, eşittir.)
**Yapılandırma sunucusu** | Yapılandırma sunucusu günlük değişikliği oranı kapasite korumalı makinelerde çalışan tüm iş yükleri arasında olması ve sürekli olarak Azure Storage veri çoğaltmak için yeterli bant genişliği gerekiyor.<br/><br/> En iyi uygulama, aynı ağ ve LAN kesimi yapılandırma sunucusunda korumak istediğiniz makinelere bulun. Farklı bir ağda bulunan, ancak korumak istediğiniz makinelere Katman 3 ağ görünürlüğünü ona sahip olmalıdır.<br/><br/> Boyutu önerileri yapılandırma sunucusu için aşağıdaki bölümde tabloda özetlenmiştir.
**İşlem sunucusu** | İlk işlem sunucusunu yapılandırma sunucusuna varsayılan olarak yüklenir. Ortamınızı ölçeklendirmek için ek işlem sunucuları dağıtabilirsiniz. <br/><br/> İşlem sunucusu korunan makinelerden çoğaltma verilerini alıp ve önbelleğe alma, sıkıştırma ve şifreleme iyileştirir. Ardından verileri Azure'a gönderir. İşlem sunucusu makine bu görevleri gerçekleştirmek için yeterli kaynak olması gerekir.<br/><br/> İşlem sunucusu disk tabanlı önbelleği kullanır. Bir ağ sorununu veya kesinti durumunda depolanan veri değişikliklerini işlemek için ayrı önbelleği disk 600 GB veya daha fazla kullanır.

## <a name="size-recommendations-for-the-configuration-server"></a>Yapılandırma sunucusu için boyutu önerileri

**CPU** | **Bellek** | **Önbellek disk boyutu** | **Veri değişikliği oranı** | **Korumalı makineler**
--- | --- | --- | --- | ---
8 Vcpu'lar (2 yuva * 2,5 gigahertz [GHz] @ 4 çekirdek) | 16 GB | 300 GB | 500 GB veya daha az | 100'den az makineler çoğaltılır.
12 Vcpu'lar (2 yuva * 2,5 GHz @ 6 çekirdek) | 18 GB | 600 GB | 1 TB ' 500 GB | 100-150 makineler arasında çoğaltılır.
16 Vcpu (2 yuva * 2,5 GHz @ 8 çekirdek) | 32 GB | 1 TB | 1 TB ile 2 TB | 150-200 makineler arasında çoğaltılır.
Başka bir işlem sunucusu Dağıt | | | > 2 TB | Ek işlem sunucusu 200'den fazla makineleri çoğaltma yapıyorsanız ya da günlük verileri değiştirirseniz oranı 2 TB aştığında dağıtın.

Konumlar:

* Her kaynak makine 3 100 GB disk ile yapılandırılır.
* 10 bin RPM, 8 SAS sürücüleri Kıyaslama depolanmasını RAID 10 ile için önbellek disk ölçümleri kullandık.

## <a name="size-recommendations-for-the-process-server"></a>İşlem sunucusu için boyut önerileri

200'den fazla makineler korumanız gerekir ya da günlük değişim oranı 2 TB'den büyük ise, çoğaltma yükü işlemek üzere işlem sunucuları ekleyebilirsiniz. Genişletmek için şunları yapabilirsiniz:

* Yapılandırma sunucularına sayısını artırın. Örneğin, iki yapılandırma sunucularına 400 makinelerle kadar koruyabilirsiniz.
* Daha fazla işlem sunucusu ekleyebilir ve bunlar yerine (veya ek olarak) trafiği işlemeye kullanabilirsiniz yapılandırma sunucusu.

Aşağıdaki tabloda bir senaryoda açıklanmaktadır:

* İşlem sunucusu olarak yapılandırma sunucusu kullanmayı planlıyorsanız değil.
* Bir ek işlem sunucusu ayarlamadıysanız ayarladınız.
* Ek işlem sunucusu kullanabilmek için korumalı sanal makineleri yapılandırdıktan.
* Her korumalı kaynak makine üç 100 GB disk ile yapılandırılır.

**Yapılandırma sunucusu** | **Ek işlem sunucusu** | **Önbellek disk boyutu** | **Veri değişikliği oranı** | **Korumalı makineler**
--- | --- | --- | --- | ---
8 Vcpu'lar (2 yuva * @ 2,5 GHz 4 çekirdek), 16 GB bellek | 4 Vcpu (2 yuva * 2,5 GHz @ 2 Çekirdek), 8 GB bellek | 300 GB | 250 GB veya daha az | 85 veya daha az makineler çoğaltılır.
8 Vcpu'lar (2 yuva * @ 2,5 GHz 4 çekirdek), 16 GB bellek | 8 Vcpu'lar (2 yuva * @ 2,5 GHz 4 çekirdek), 12 GB bellek | 600 GB | 1 TB ' 250 GB | 85 150 makineler arasında çoğaltılır.
12 Vcpu'lar (2 yuva * 2,5 GHz @ 6 çekirdek), 18 GB bellek | 12 Vcpu'lar (2 yuva * 2,5 GHz @ 6 çekirdek) 24 GB bellek | 1 TB | 1 TB ile 2 TB | 150-225 makineler arasında çoğaltılır.

Sunucularınızın ölçeklendirme şekilde büyütme veya genişleme modeli için tercihinizi bağlıdır.  Bazı gelişmiş yapılandırma ve işlem sunucuları dağıtarak ölçeği veya daha az kaynak ile daha fazla sunucu dağıtarak ölçeğini. Örneğin, 220 makinelerin korunmasına ihtiyacınız varsa, aşağıdakilerden birini yapabilirsiniz:

* 12 vCPU, 18 GB bellek, yapılandırma sunucusuyla ve ek işlem sunucusu 12 vCPU, 24 GB bellek ile ayarlayın. Korumalı makineler, yalnızca ek işlem sunucusu kullanacak şekilde yapılandırın.
* İki yapılandırma sunucularına (2 x 8 vCPU, 16 GB RAM) ve iki ek işlem sunucusu (1 x 8 vCPU ve 4 vCPU x 1 135 + 85 [220] işlenecek makineler) ayarlayın. Korumalı makineler, yalnızca ek işlem sunucularını kullanacak şekilde yapılandırın.


## <a name="control-network-bandwidth"></a>Ağ bant genişliğini denetlemek

Kullandığınız sonra [dağıtım planlayıcısı aracı](site-recovery-deployment-planner.md) (ilk çoğaltma ve ardından değişim) çoğaltma için gereken bant genişliğini hesaplamak için birkaç seçenekleri kullanarak çoğaltma için kullanılan bant genişliği miktarını kontrol edebilirsiniz:

* **Bant genişliğini kısıtlama**: Azure'a çoğaltılan VMware trafiği belirli işlem sunucusu üzerinden gider. İşlem sunucusu olarak çalışan makineler üzerinde bant genişliğini kısıtlayabilirsiniz.
* **Bant genişliği üzerinde etki**: birkaç kayıt defteri anahtarlarını kullanarak çoğaltma için kullanılan bant genişliği etkileyebilir:
  * **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM** kayıt defteri değeri veri aktarımını bir disk için (başlangıç ve değişim çoğaltması) kullanılan iş parçacığı sayısını belirtir. Daha yüksek bir değer, çoğaltma işlemi için kullanılan ağ bant genişliğini artırır.
  * **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\DownloadThreadsPerVM** yeniden çalışma sırasındaki veri aktarımı için kullanılan iş parçacığı sayısını belirtir.

### <a name="throttle-bandwidth"></a>Bant genişliğini kısıtlama

1. İşlem sunucusu olarak işlev gören makinedeki Azure Backup MMC ek bileşenini açın. Varsayılan olarak, bir kısayol yedekleme için masaüstünde veya aşağıdaki klasörde kullanılabilir: C:\Program Files\Microsoft Azure Recovery Services agent\bin\wabadmin yolunda.
2. Ek bileşende **Özellikleri Değiştir**'e tıklayın.

    ![Özelliklerini değiştirmek için ekran Azure Backup MMC ek bileşenini seçeneği](./media/site-recovery-vmware-to-azure/throttle1.png)
3. Üzerinde **azaltma** sekmesine **yedekleme işlemleri için Internet bant genişliği kullanımı daraltmayı etkinleştir**. İş için sınırları ayarlayın ve saat İş dışı. Geçerli aralıklar saniye başına 512 Kbps ila 102 Mbps arasındadır.

    ![Ekran Azure yedekleme Özellikleri iletişim kutusu](./media/site-recovery-vmware-to-azure/throttle2.png)

Ayrıca, azaltma ayarı için [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet'ini de kullanabilirsiniz. Bu ayara ilişkin örneği aşağıda bulabilirsiniz:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting - NoThrottle** hiçbir azaltmanın gerekli olmadığını gösterir.

### <a name="influence-network-bandwidth-for-a-vm"></a>Bir VM için ağ bant genişliği üzerinde etki

1. VM'ın kayıt defterinde gidin **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * Bir çoğaltma diskte bant genişliği trafiği etkilemek için değerini değiştirin **UploadThreadsPerVM**, veya yoksa anahtar oluşturun.
   * Azure'dan yeniden çalışma trafiği için bant genişliği etkilemek için değerini değiştirin **DownloadThreadsPerVM**.
2. Varsayılan değer 4'tür. "Fazla sağlanan" bir ağda, bu kayıt defteri anahtarlarının varsayılan değerlerinin değiştirilmesi gerekir. Maksimum değer 32'dir. Değeri iyileştirmek için trafiği izleyin.


## <a name="deploy-additional-process-servers"></a>Ek işlem sunucusu dağıtın

Varsa ölçeklendirmek için out dağıtımınızı 200 kaynak makinelerden ötesinde veya günlük 2 TB'den fazla oranını karmaşıklığı toplam sahip, trafik hacmi işlemek için ek işlem sunucuları gerekir. İşlem sunucusu kurmak için bu yönergeleri izleyin. Sunucuyu ayarladıktan sonra bunu kullanmak için kaynak makineleri geçirin.

1. İçinde **Site Recovery sunucuları**yapılandırma sunucuya tıklayın ve ardından **işlem sunucusu**.

    ![Bir işlem sunucusu eklemek için sunucuları seçeneği Site Recovery ekran görüntüsü](./media/site-recovery-vmware-to-azure/migrate-ps1.png)
2. İçinde **sunucu türü**, tıklatın **işlem sunucusu (şirket içi)**.

    ![İşlem sunucusu ekran iletişim kutusu](./media/site-recovery-vmware-to-azure/migrate-ps2.png)
3. Site Recovery birleşik Kurulumu dosyasını indirin ve işlem sunucusu yüklemek için çalıştırın. Bu ayrıca, kasaya kaydeder.
4. **Başlamadan önce** kısmında **Dağıtımı genişletmek için ek işlem sunucuları ekleyin**’i seçin.
5. Ne zaman yaptığınız gibi Sihirbazı tamamlayın, [ayarlanan](#step-2-set-up-the-source-environment) yapılandırma sunucusu.

    ![Ekran görüntüsü, Azure Site kurtarma birleşik Kurulum Sihirbazı](./media/site-recovery-vmware-to-azure/add-ps1.png)
6. İçinde **yapılandırma sunucusu ayrıntılarını**, IP adresi yapılandırma sunucusu ve parola belirtin. Parola elde etmek için Çalıştır **[SiteRecoveryInstallationFolder]\home\sysystems\bin\genpassphrase.exe – n** yapılandırma sunucusundaki.

    ![Ekran yapılandırması sunucu Ayrıntıları sayfası](./media/site-recovery-vmware-to-azure/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Yeni işlem sunucusu kullanabilmek için makineleri geçirin
1. İçinde **ayarları** > **Site Recovery sunucuları**yapılandırma sunucusu tıklayın ve ardından **işlem sunucuları**.

    ![İşlem sunucusu ekran iletişim kutusu](./media/site-recovery-vmware-to-azure/migrate-ps2.png)
2. İşlem sunucusu şu anda kullanımda sağ tıklayın ve **anahtar**.

    ![Ekran görüntüsü, yapılandırma sunucusu iletişim kutusu](./media/site-recovery-vmware-to-azure/migrate-ps3.png)
3. İçinde **Select hedef işlem sunucusunu**, kullanmak istediğiniz yeni işlem sunucusunu seçin ve ardından sunucu işleyecek sanal makineleri seçin. Sunucu hakkında bilgi almak için bilgi simgesine tıklayın. Yük kararları vermenize yardımcı olmak için her seçili bir sanal makine için yeni işlem sunucusu çoğaltmak için gerekli olan ortalama alanı görüntülenir. Yeni işlem sunucusu için çoğaltma başlatmak için onay işaretine tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

İndirme ve çalıştırma [Azure Site Recovery dağıtımı Planlayıcısı](https://aka.ms/asr-deployment-planner)
