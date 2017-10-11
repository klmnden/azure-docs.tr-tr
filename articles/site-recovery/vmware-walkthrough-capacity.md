---
title: "Kapasite planlama ve Azure Site Recovery ile Azure'da VMware çoğaltma işlemini ölçeklendirme | Microsoft Docs"
description: "VMware Vm'leri Azure Site Recovery ile azure'a çoğaltırken planı kapasite ve ölçek bu makaleyi kullanın"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 0a1cd8eb-a8f7-4228-ab84-9449e0b2887b
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: rayne
ms.openlocfilehash: f5b334e594e3d002e1862b25c4faba7163efa7d4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="step-3-plan-capacity-and-scaling-for-vmware-to-azure-replication"></a>3. adım: kapasite ve VMware için Azure çoğaltma ölçeklendirme planlama

Bu makale için kapasite planlaması ve ölçeklendirme, şirket içi VMware Vm'lerini ve fiziksel sunucuları ile azure'a çoğaltırken ekleneceğini kullanmak [Azure Site Recovery](site-recovery-overview.md).

POST açıklamaları ve soruları alt bu makalenin veya üzerinde [Azure kurtarma Hizmetleri Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="how-do-i-start-capacity-planning"></a>Kapasite planlama nasıl başlamanız gerekir?

Çoğaltma ortamınız hakkında bilgi toplamak ve bu makalede vurgulanmış konuları birlikte bu bilgileri kullanarak kapasite planlama.


## <a name="gather-information"></a>Bilgileri toplayın

1. Karşıdan [dağıtım planlayıcısı aracı](https://aka.ms/asr-deployment-planner) VMware çoğaltma için.
2. [Bu makaleyi okuyun](site-recovery-deployment-planner.md) aracı çalıştırmak nasıl anlamak için.
3. Aracını kullanarak, uyumlu ve uyumsuz VM'ler, VM başına disk hakkında bilgi toplamak ve disk başına veri karmaşıklığı. Aracı, ağ bant genişliği gereksinimlerini ve başarılı çoğaltma ve test yük devretme için gereken Azure altyapı da kapsar.

## <a name="replication-considerations"></a>Çoğaltma konuları

Dağıtıma başlamadan önce bu noktalar göz önünde bulundurun:

**Bileşen** | **Ayrıntılar** |
--- | --- | ---
**Çoğaltma** | **Maksimum günlük değişim oranı:** korumalı makine yalnızca bir işlem sunucusunu kullanabilir ve tek işlem sunucusu günlük işleyebilir değişiklik 2 TB oranı. Bu nedenle 2 TB maksimum günlük veri korumalı bir makine için desteklenen oranı değiştirmektir.<br/><br/> **En yüksek verimlilik:** çoğaltılmış bir makineden azure'da bir depolama hesabına ait olabilir. Standart depolama hesabı 20.000 istekleri saniye başına maksimum işleyebilir ve 20. 000'için kaynak makine arasında girdi/çıktı işlemleri (IOPS) saniye başına sayısı tutmanızı öneririz. Örneğin, kaynak makine 5 disklerle varsa ve her disk 120 IOPS (8 K boyut) kaynak makinede oluşturur, ardından onu Azure başına 500 disk IOPS sınırı içinde olacaktır. (Gerekli depolama hesabı sayısı tarafından 20.000 bölünmüş toplam kaynak makine IOPS, eşittir.)

## <a name="configuration-server-capacity"></a>Yapılandırma sunucusu kapasite

Yapılandırma sunucusu günlük değişikliği oranı kapasite korumalı makinelerde çalışan tüm iş yükleri arasında olması ve sürekli olarak Azure Storage veri çoğaltmak için yeterli bant genişliği gerekiyor.

En iyi uygulama, aynı ağ ve LAN kesimi yapılandırma sunucusunda korumak istediğiniz makinelere bulun. Farklı bir ağda bulunan, ancak korumak istediğiniz makinelere Katman 3 ağ görünürlüğünü ona sahip olmalıdır.

## <a name="sizing-recommendations"></a>Boyutlandırma önerileri

Tablo CPU üzerinde göre boyutlandırma önerileri özetler.

**CPU** | **Bellek** | **Önbellek disk boyutu** | **Veri değişikliği oranı** | **Korumalı makineler**
--- | --- | --- | --- | ---
8 Vcpu'lar (2 yuva * 2,5 gigahertz [GHz] @ 4 çekirdek) | 16 GB | 300 GB | 500 GB veya daha az | 100'den az makineler çoğaltılır.
12 Vcpu'lar (2 yuva * 2,5 GHz @ 6 çekirdek) | 18 GB | 600 GB | 1 TB ' 500 GB | 100-150 makineler arasında çoğaltılır.
16 Vcpu (2 yuva * 2,5 GHz @ 8 çekirdek) | 32 GB | 1 TB | 1 TB ile 2 TB | 150-200 makineler arasında çoğaltılır.
Başka bir işlem sunucusu Dağıt | | | > 2 TB | Ek işlem sunucusu 200'den fazla makineleri çoğaltma yapıyorsanız ya da günlük verileri değiştirirseniz oranı 2 TB aştığında dağıtın.

Konumlar:

* Her kaynak makine 3 100 GB disk ile yapılandırılır.
* 10 bin RPM, 8 SAS sürücüleri Kıyaslama depolanmasını RAID 10 ile için önbellek disk ölçümleri kullandık.

## <a name="process-server-capacity"></a>İşlem sunucusu kapasite


İşlem sunucusu korunan makinelerden çoğaltma verilerini alıp ve önbelleğe alma, sıkıştırma ve şifreleme iyileştirir. Ardından verileri Azure'a gönderir.

- İşlem sunucusu makine bu görevleri gerçekleştirmek için yeterli kaynak olması gerekir.
- İlk işlem sunucusunu yapılandırma sunucusuna varsayılan olarak yüklenir. Ortamınızı ölçeklendirmek için ek işlem sunucuları dağıtabilirsiniz.
- İşlem sunucusu disk tabanlı önbelleği kullanır. Bir ağ sorununu veya kesinti durumunda depolanan veri değişikliklerini işlemek için ayrı önbelleği disk 600 GB veya daha fazla kullanır.
- 200'den fazla makineler korumanız gerekir ya da günlük değişim oranı 2 TB'den büyük ise, çoğaltma yükü işlemek üzere işlem sunucuları ekleyebilirsiniz. Genişletmek için şunları yapabilirsiniz:
    - Yapılandırma sunucularına sayısını artırın. Örneğin, iki yapılandırma sunucularına 400 makinelerle kadar koruyabilirsiniz.
    - Daha fazla işlem sunucusu ekleyebilir ve bunlar yerine (veya ek olarak) trafiği işlemeye kullanabilirsiniz yapılandırma sunucusu.


### <a name="example-process-server-scaling"></a>Örnek sunucu işlemi ölçeklendirme

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

## <a name="deploy-additional-process-servers"></a>Ek işlem sunucusu dağıtın

Bir ek işlem sunucusu kurmak için bu yönergeleri izleyin. Sunucuyu ayarladıktan sonra bunu kullanmak için kaynak makineleri geçirin.

1. İçinde **Site Recovery sunucuları**, yapılandırma sunucusu tıklayın > **+ işlem sunucusu**.
2. İçinde **sunucu türü**, tıklatın **işlem sunucusu (şirket içi)**.

    ![İşlem sunucusu](./media/vmware-walkthrough-capacity/migrate-ps2.png)
3. Site kurtarma birleşik kurulum dosyasını karşıdan yükleyin.
4. İşlem sunucusu yüklemek için Kurulumu çalıştırın ve kasaya kaydedin.
5. **Başlamadan önce** kısmında **Dağıtımı genişletmek için ek işlem sunucuları ekleyin**’i seçin.
6. İçinde **yapılandırma sunucusu ayrıntılarını**, IP adresi yapılandırma sunucusu ve parola belirtin. Parola yoksa, çalıştırarak alın **[SiteRecoveryInstallationFolder]\home\sysystems\bin\genpassphrase.exe – n** yapılandırma sunucusundaki.

    ![Yapılandırma sunucusu](./media/vmware-walkthrough-capacity/add-ps2.png)
7. Yapılandırma sunucuyu ayarladığınızda yaptığınız aynı şekilde Kurulum kalanını tamamlayın.

### <a name="migrate-machines-to-use-the-process-server"></a>İşlem sunucusu kullanabilmek için makineleri geçirin

1. İçinde **ayarları** > **Site Recovery sunucuları**, yapılandırma sunucusu tıklayın > **işlem sunucuları**.
2. Şu anda kullanımda işlem sunucusunu sağ tıklatın > **anahtar**.

    ![Anahtar işlem sunucusu](./media/vmware-walkthrough-capacity/migrate-ps3.png)
3. İçinde **Select hedef işlem sunucusunu**, kullanmak istediğiniz işlem sunucusunu seçin ve sunucu işleyecek sanal makineleri seçin.
4. Bilgi simgesine tıklayın. Yük kararları vermenize yardımcı olmak için her seçili VM yeni işlem sunucusu çoğaltmak için gerekli olan ortalama alanı görüntülenir.
5. Yeni işlem sunucusu için çoğaltma başlatma onay işaretine tıklayın.

## <a name="control-network-bandwidth"></a>Ağ bant genişliğini denetlemek

Çalıştırdıktan sonra [dağıtım planlayıcısı aracı](site-recovery-deployment-planner.md) (ilk çoğaltma ve ardından değişim) çoğaltma için gereken bant genişliğini hesaplamak için birkaç seçenekleri kullanarak çoğaltma için kullanılan bant genişliği miktarını kontrol edebilirsiniz:

* **Bant genişliğini kısıtlama**: Azure'a çoğaltılan VMware trafiği belirli işlem sunucusu üzerinden gider. İşlem sunucusu olarak çalışan makineler üzerinde bant genişliğini kısıtlayabilirsiniz.
* **Bant genişliği üzerinde etki**: birkaç kayıt defteri anahtarlarını kullanarak çoğaltma için kullanılan bant genişliği etkileyebilir:
  * **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\UploadThreadsPerVM** kayıt defteri değeri veri aktarımını bir disk için (başlangıç ve değişim çoğaltması) kullanılan iş parçacığı sayısını belirtir. Daha yüksek bir değer, çoğaltma işlemi için kullanılan ağ bant genişliğini artırır.
  * **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DownloadThreadsPerVM** yeniden çalışma sırasındaki veri aktarımı için kullanılan iş parçacığı sayısını belirtir.

### <a name="throttle-bandwidth"></a>Bant genişliğini kısıtlama

1. İşlem sunucusu olarak işlev gören makinedeki Azure Backup MMC ek bileşenini açın. Varsayılan olarak, bir kısayol yedekleme için masaüstünde veya aşağıdaki klasörde kullanılabilir: C:\Program Files\Microsoft Azure Recovery Services agent\bin\wabadmin yolunda.
2. Ek bileşende **Özellikleri Değiştir**'e tıklayın.
3. Üzerinde **azaltma** sekmesine **yedekleme işlemleri için Internet bant genişliği kullanımı daraltmayı etkinleştir**.
4. İş için sınırları ayarlayın ve saat İş dışı. Geçerli aralıklar saniye başına 512 Kbps ila 102 Mbps arasındadır.

    ![Kısıtlama](./media/vmware-walkthrough-capacity/throttle2.png)

Ayrıca, azaltma ayarı için [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet'ini de kullanabilirsiniz. Bu ayara ilişkin örneği aşağıda bulabilirsiniz:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting - NoThrottle** hiçbir azaltmanın gerekli olmadığını gösterir.

### <a name="influence-network-bandwidth-for-a-vm"></a>Bir VM için ağ bant genişliği üzerinde etki

1. VM kayıt defterinde Git **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * Bir çoğaltma diskte bant genişliği trafiği etkilemek için değerini değiştirin **UploadThreadsPerVM**, veya yoksa anahtar oluşturun.
   * Azure'dan yeniden çalışma trafiği için bant genişliği etkilemek için değerini değiştirin **DownloadThreadsPerVM**.
2. Varsayılan değer 4'tür. Fazla sağlanan bir ağda, bu kayıt defteri anahtarları değiştirilmesi gerekir. Maksimum değer 32'dir. Değeri iyileştirmek için trafiği izleyin.




## <a name="next-steps"></a>Sonraki adımlar

Git [4. adım: ağ planlama](vmware-walkthrough-network.md).
