---
title: Linux Vm'lerinizi Azure Site Recovery ile Azure Premium Depolama'ya geçiş | Microsoft Docs
description: Mevcut sanal makinelerinizi, Site Recovery kullanarak Azure Premium Depolama'ya geçirin. Premium depolama, Azure sanal makinelerinde çalışan g/Ç açısından yoğun iş yükleri için yüksek performanslı, düşük gecikme süreli disk desteği sunar.
services: virtual-machines-linux,storage
cloud: Azure
author: luywang
ms.service: virtual-machines-linux
ms.tgt_pltfrm: linux
ms.topic: article
ms.date: 08/15/2017
ms.author: luywang
ms.subservice: disks
ms.openlocfilehash: ffcc2f46a30569979879ff302cde1e3b146d3b50
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60543692"
---
# <a name="migrate-to-premium-storage-by-using-azure-site-recovery"></a>Azure Site Recovery kullanarak Premium depolamaya geçiş

[Azure premium SSD](disks-types.md) , g/Ç açısından yoğun iş yüklerini çalıştıran sanal makineler (VM) için yüksek performanslı, düşük gecikme süreli disk desteği sunar. Bu kılavuz kullanarak sanal makine disklerinizi bir standart depolama hesabından premium depolama hesabına geçirmenize yardımcı olur. [Azure Site Recovery](../../site-recovery/site-recovery-overview.md).

Site Recovery, şirket içi fiziksel sunucuları ve Vm'leri buluta (Azure) veya ikincil veri merkezine çoğaltılmasını düzenleyerek iş sürekliliği ve olağanüstü durum kurtarma stratejiniz için katkıda bulunan bir Azure hizmetidir. Kesinti birincil konumunuzda meydana gelirse uygulamaları ve iş yüklerini kullanılabilir durumda tutmak için ikincil konuma yük devredersiniz. Normal çalışmasına geri döndüğünde birincil konumunuzda başarısız. 

Site Recovery, üretim ortamlarını etkilemeden olağanüstü durum kurtarma ayrıntılarını desteklemek için yük devretme testleri sunar. Beklenmeyen olağanüstü durumlar için en düşük düzeyde veri kaybıyla sonuçlanan (çoğaltma sıklığı) bağlı olarak yük devretmeler çalıştırabilirsiniz. Premium depolamaya geçiş senaryosunda, kullandığınız [Site recovery'de yük devretme](../../site-recovery/site-recovery-failover.md) hedef diskler bir premium depolama hesabına geçirmek için.

Bu seçenek en düşük kapalı kalma süresi sunduğundan, Site Recovery kullanarak Premium depolamaya geçiş öneririz. Bu seçenek ayrıca diskleri kopyalama ve yeni VM'ler oluşturma el ile yürütülmesine önler. Site Recovery, sistematik olarak disklerinizi kopyalayın ve yük devretme sırasında yeni bir VM oluşturun. 

Site Recovery, yük devretme en az bir tür bir sayı veya kapalı kalma süresi olmadan destekler. Planlamak, kapalı kalma süresi ve veri kaybı tahmin etmek için bkz: [Site recovery'de yük devretme türleri](../../site-recovery/site-recovery-failover.md). Varsa, [yük devretmeden sonra Azure Vm'lerine bağlanmak için hazırlık yapma](../../site-recovery/vmware-walkthrough-overview.md), Azure VM'ye yük devretmeden sonra RDP kullanarak bağlanabiliyor olmanız gerekir.

![Olağanüstü durum kurtarma diyagramı][1]

## <a name="azure-site-recovery-components"></a>Azure Site Recovery bileşenleri

Site Recovery'nin şu bileşenlerini bu geçiş senaryosu için uygundur:

* **Yapılandırma sunucusu** iletişimi düzenler ve veri çoğaltma ve kurtarma işlemlerini yöneten bir Azure VM. Bu sanal makinede, yapılandırma sunucusu ve ek bir bileşeni yüklemek için bir tek kurulum dosyasını çalıştırın, bir işlem sunucusu çoğaltma ağ geçidi çağrılır. Hakkında bilgi edinin [yapılandırma sunucusu önkoşulları](../../site-recovery/vmware-walkthrough-overview.md). Yapılandırma sunucusunu yalnızca bir kez ayarlayın ve aynı bölgeye tüm geçişler için kullanabilirsiniz.

* **İşlem sunucusu** çoğaltma ağ geçidi: 

  1. Çoğaltma verilerinin kaynak Vm'leri alır.
  2. Veri önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir.
  3. Verileri bir depolama hesabına gönderir. 

  Ayrıca mobility hizmetinin kaynak VM'lerin göndermeli yüklemesini ele alır ve kaynak Vm'leri otomatik olarak bulunmasını gerçekleştirir. Varsayılan işlem sunucusu yapılandırma sunucusuna yüklenir. Dağıtımınız ölçeklendirmek için ek bağımsız işlem sunucularını dağıtabilirsiniz. Hakkında bilgi edinin [işlem sunucusu dağıtımı için en iyi yöntemler](https://azure.microsoft.com/blog/best-practices-for-process-server-deployment-when-protecting-vmware-and-physical-workloads-with-azure-site-recovery/) ve [ek işlem sunucusu dağıtımı](../../site-recovery/site-recovery-plan-capacity-vmware.md#deploy-additional-process-servers). İşlem sunucusu yalnızca bir kez ayarlama ve aynı bölgeye tüm geçişler için kullanabilirsiniz.

* **Mobility hizmeti** çoğaltmak istediğiniz her standart sanal makineye dağıtılan bir bileşendir. Standart VM üzerindeki veri yazma yakalar ve bunları işlem sunucusuna gönderir. Hakkında bilgi edinin [makine önkoşulları çoğaltılan](../../site-recovery/vmware-walkthrough-overview.md).

Bu grafik, bu bileşenlerin nasıl etkileşim kurduklarını gösterir:

![Site Recovery bileşenlerinin etkileşimi][15]

> [!NOTE]
> Site Recovery, depolama alanları disklerini geçişini desteklemez.

Diğer senaryolar için ek bileşenler için bkz: [senaryo mimarisini](../../site-recovery/vmware-walkthrough-overview.md).

## <a name="azure-essentials"></a>Azure temel bileşenleri

Bu geçiş senaryosu için Azure gereksinimleri şunlardır:

* Azure aboneliği.
* Çoğaltılan verileri depolamak için bir Azure premium depolama hesabı.
* Yük devretme sırasında oluşturulduğunda VM'lerin bağlanacağı bir Azure sanal ağı. Azure sanal ağı olarak Site Recovery çalıştığı aynı bölgede olması gerekir.
* Çoğaltma günlüklerini depolamak için bir Azure standart depolama hesabı. Bu, Geçirilmekte olan VM diskleri için aynı depolama hesabı olabilir.

## <a name="prerequisites"></a>Önkoşullar

* Önceki bölümde ilgili geçiş senaryosu bileşenlerini anlayın.
* Uygulamanızı kapalı kalma süresi hakkında bilgi edinerek planlama [Site recovery'de yük devretme](../../site-recovery/site-recovery-failover.md).

## <a name="setup-and-migration-steps"></a>Kurulum ve geçiş adımları

Aynı bölge içinde veya bölgeler arasında Azure Iaas Vm'lerine geçirmek için Site RECOVERY'yi kullanabilirsiniz. Aşağıdaki yönergeler bu makaleden geçiş senaryosu için uyarlanmış [çoğaltmak VMware Vm'lerini veya fiziksel sunucuları azure'a](../../site-recovery/vmware-walkthrough-overview.md). Lütfen bu makaledeki yönergeleri yanı sıra ayrıntılı adımlar için bağlantıları izleyin.

### <a name="step-1-create-a-recovery-services-vault"></a>1. Adım: Kurtarma Hizmetleri kasası oluşturma

1. [Azure portalı](https://portal.azure.com) açın.
2. Seçin **kaynak Oluştur** > **Yönetim** > **yedekleme** ve **Site Recovery (OMS)**. Alternatif olarak, seçebileceğiniz **Gözat** > **kurtarma Hizmetleri kasası** > **Ekle**. 
3. VM'ler için çoğaltılacak bir bölge belirtin. Aynı bölgede geçiş amacıyla, kaynak depolama hesabı ve kaynak VM'lerin bulunduğu bölgeyi seçin. 

### <a name="step-2-choose-your-protection-goals"></a>2. Adım: Koruma hedeflerinizi seçme 

1. Açmak istediğiniz yapılandırma sunucusunu yüklemek için sanal makinede [Azure portalında](https://portal.azure.com).
2. Git **kurtarma Hizmetleri kasaları** > **ayarları** > **Site Recovery** > **1. adım: Altyapıyı hazırlama** > **koruma hedefi**.

   ![Koruma hedefi bölmesine gözatma][2]

3. Altında **koruma hedefi**, ilk açılan listeden seçin **Azure'a**. İkinci açılan listesinde seçin **sanallaştırılmamış / diğer**ve ardından **Tamam**.

   ![Koruma hedefi bölmesinde doldurulmuş kutuları][3]

### <a name="step-3-set-up-the-source-environment-configuration-server"></a>3. Adım: Kaynak ortamı ayarlamak (yapılandırma sunucusu)

1. İndirme **Azure Site Recovery birleşik Kurulumu** ve giderek kasa kayıt anahtarını **altyapıyı hazırlama** > **kaynağı hazırla**  >  **Sunucusu Ekle** bölmeleri. 
 
   Birleşik kurulumu çalıştırmak için kasa kayıt anahtarı gerekir. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

   ![Sunucu Ekle bölmesine gözatma][4]

2. İçinde **Sunucusu Ekle** bölmesinde, yapılandırma sunucusu ekleyin.

   ![Seçili yapılandırma sunucusu ile sunucu bölmesi ekleme][5]

3. Yapılandırma sunucusu olarak kullandığınız sanal makinede yapılandırma sunucusu ve işlem sunucusu yüklemek için birleşik Kurulum'u çalıştırın. Yapabilecekleriniz [ekran görüntüleri ile yol](../../site-recovery/vmware-walkthrough-overview.md) yüklemeyi tamamlamak için. Bu geçiş senaryosu için belirtilen adımları için aşağıdaki ekran görüntüleri başvurabilirsiniz.

   1. İçinde **başlamadan önce**seçin **yapılandırma sunucusu ve işlem sunucusunu yükle**.

      ![Başlamadan önce sayfası][6]

   2. İçinde **kayıt**göz atın ve kasadan indirdiğiniz kayıt anahtarı seçin.

      ![Kayıt sayfası][7]

   3. **Ortam Ayrıntıları**’nda VMware sanal makinelerini çoğaltıp çoğaltmayacağınızı seçin. Bu geçiş senaryosu için seçin **Hayır**.

      ![Ortam Ayrıntıları sayfası][8]

4. Yükleme tamamlandıktan sonra aşağıdakileri yapabilirsiniz **Microsoft Azure Site Recovery yapılandırma sunucusunun** penceresi:
 
   1. Kullanma **hesaplarını yönetme** otomatik bulma için sekmesinde, Site Recovery hesabı oluşturmak için kullanabilirsiniz. (Fiziksel makineler koruma hakkında senaryosu, hesabı ayarlama geçerli değildir, ancak aşağıdaki adımlardan birini etkinleştirmek için en az bir hesabınızın olması gerekir. Bu durumda, hesap ve parola herhangi biri olarak adlandırabilirsiniz.) 
   2. Kullanım **kasa kaydı** kasa kimlik bilgilerini karşıya yüklemek için sekmesinde.

      ![Kasa kaydı sekmesini][9]

### <a name="step-4-set-up-the-target-environment"></a>4. Adım: Hedef ortamı ayarlama

Seçin **altyapıyı hazırlama** > **hedef**ve yük devretmenin ardından VM'ler için kullanmak istediğiniz dağıtım modelini belirtin. Seçebileceğiniz **Klasik** veya **Resource Manager**senaryonuza bağlı olarak.

![Hedef bölmesi][10]

Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler. 

> [!NOTE]
> Çoğaltılan veriler için bir premium depolama hesabı kullanıyorsanız, çoğaltma günlüklerini depolamak için bir ek standart depolama hesabı ayarlamanız gerekir.

### <a name="step-5-set-up-replication-settings"></a>5. Adım: Çoğaltma ayarlarını belirleme

Yapılandırma sunucunuzda oluşturduğunuz çoğaltma ilkesiyle başarıyla ilişkilendirildi olduğunu doğrulamak için izleyin [çoğaltma ayarlarını belirleme](../../site-recovery/vmware-walkthrough-overview.md).

### <a name="step-6-plan-capacity"></a>6. Adım: Kapasiteyi planlama

1. Kullanım [kapasite Planlayıcısı](../../site-recovery/site-recovery-capacity-planner.md) ağ bant genişliği, depolama ve diğer gereksinimler, çoğaltma karşılamak için doğru bir şekilde tahmin gerekiyor. 
2. İşiniz bittiğinde **Evet, yaptım** içinde **kapasite planlamasını tamamladınız mı?**.

   ![Kapasite planlaması tamamlandığını onaylayan kutusu][11]

### <a name="step-7-install-the-mobility-service-and-enable-replication"></a>7. Adım: Mobility hizmetini yükleme ve çoğaltmayı etkinleştirin

1. Seçebileceğiniz [gönderme yüklemesi](../../site-recovery/vmware-walkthrough-overview.md) kaynak vm'lerinize veya çok [mobility hizmetini el ile yükleme](../../site-recovery/site-recovery-vmware-to-azure-install-mob-svc.md) kaynak VM'lerin üzerinde. Yükleme ve el ile yükleyici yolu, sağlanan bağlantıyı gönderme gereksiniminin bulabilirsiniz. El ile yükleme yapıyorsanız yapılandırma sunucusunu bulmak için iç IP adresi kullanmak gerekebilir.

   ![Yapılandırma sunucusu Ayrıntıları sayfası][12]

   İki geçici diskler devredilen VM'nin olacaktır: biri birincil VM ve kurtarma bölgesinde bir VM sağlama sırasında oluşturulan diğer. Çoğaltılmadan önce geçici disk tutmak için çoğaltmayı etkinleştirmeden önce mobility hizmetini yükleyin. Geçici disk dışlama hakkında daha fazla bilgi için bkz: [diskleri çoğaltmadan hariç](../../site-recovery/vmware-walkthrough-overview.md).

2. Aşağıda belirtilen şekilde çoğaltmayı etkinleştirin:
   1. Seçin **uygulama çoğaltma** > **kaynak**. Çoğaltmayı ilk kez etkinleştirdikten sonra seçip **+ Çoğalt** ek makineler için çoğaltma işlemini etkinleştirmek istiyorsanız kasada.
   2. 1. adımda ayarladığınız **kaynak** işlem sunucunuzu olarak.
   3. Adım 2'de, yük devretme sonrası dağıtım modeli, geçirmek için bir premium depolama hesabı, günlükleri ve başarısız için sanal ağ kaydetmek için bir standart depolama hesabı belirtin.
   4. 3. adımda, korumalı VM'lerin IP adresine göre ekleyin. (Bunları bulmak için bir dahili IP adresine ihtiyacınız.)
   5. 4. adımda, işlem sunucusu daha önce ayarlamış hesaplar'ı seçerek özelliklerini yapılandırın.
   6. 5. adımda daha önce oluşturduğunuz çoğaltma ilkesini seçin "5. adım: Çoğaltma ayarlarını yapın."
   7. **Tamam**’ı seçin.

   > [!NOTE]
   > Bir Azure VM serbest bırakılmış ve yeniden başlatıldı, aynı IP adresini alma garantisi yoktur. Yapılandırma sunucusu/işlem sunucusu veya korumalı Azure VM'lerin IP adresi değişirse, bu senaryoda çoğaltma düzgün çalışmayabilir.

   ![Seçilen kaynak ile çoğaltma bölmesini etkinleştirme][13]

Azure depolama ortamınızı tasarlarken, bir kullanılabilirlik kümesindeki her VM için ayrı bir depolama hesabı kullanmanızı öneririz. Depolama katmanı için en iyi izlemenizi öneririz [her kullanılabilirlik kümesi için birden fazla depolama hesabı kullanma](../linux/manage-availability.md). Birden çok depolama hesabı için VM disklerini dağıtma, depolama kullanılabilirliğini artırmaya yardımcı olur ve Azure depolama altyapıda g/ç dağıtır.

Bir depolama hesabına, tüm sanal makinelerin diskleri çoğaltmak yerine bir kullanılabilirlik kümesindeki sanal makineleriniz varsa birden çok kez birden çok VM geçirme önemle öneririz. Böylece, aynı kullanılabilirlik kümesindeki Vm'leri tek bir depolama hesabında paylaşmayın. Kullanım **çoğaltmayı etkinleştirme** bölmesinde her bir VM için bir kerede bir hedef depolama hesabı ayarlama.
 
Gereksinimlerinize göre bir yük devretme sonrası dağıtım modeli seçebilirsiniz. Azure Resource Manager, yük devretme sonrası dağıtım modeli seçerseniz, bir VM'yi (Resource Manager) sanal makinesine (Resource Manager) devredebilir veya VM'ye (Klasik) bir VM'yi (Resource Manager) devredebilir.

### <a name="step-8-run-a-test-failover"></a>8. adım: Yük devretme testi çalıştırma

Çoğaltma tam olup olmadığını denetlemek için Site Recovery örneğinizi seçin ve ardından **ayarları** > **çoğaltılan öğeler**. Çoğaltma işleminizi yüzdesi ve durumu görürsünüz. 

İlk çoğaltma sonrasında, çoğaltma stratejinizi doğrulamak için yük devretme testi çalıştırma, tamamlanmıştır. Yük devretme testi ayrıntılı adımlar için bkz: [Site Recovery'de yük devretme testi çalıştırma](../../site-recovery/vmware-walkthrough-overview.md). 

> [!NOTE]
> Tüm yük devretmeyi çalıştırmadan önce Vm'lerinizin ve çoğaltma stratejinizi gereksinimleri karşıladığından emin olun. Yük devretme testi çalıştırma hakkında daha fazla bilgi için bkz. [Site Recovery'de azure'a yük devretme testi](../../site-recovery/site-recovery-test-failover-to-azure.md).

İçinde yük devretme testi durumunu görebilirsiniz **ayarları** > **işleri** > *YOUR_FAILOVER_PLAN_NAME*. Bölmede bir dökümünü adımları ve başarı/hata sonuçları görebilirsiniz. Yük devretme testi sırasında herhangi bir adım başarısız olursa hata iletisini kontrol etmek için adımı seçin. 

### <a name="step-9-run-a-failover"></a>9. adım: Yük devretme çalıştırma

Sonra test disklerinizi Premium depolamaya geçiş ve sanal makine örneklerine çoğaltmak için yük devretme yük devretme tamamlanır. Ayrıntılı adımları [yük devretme çalıştırma](../../site-recovery/site-recovery-failover.md#run-a-failover). 

Seçtiğinizden emin olun **Vm'lerini kapatır ve en son verileri eşitleyin**. Bu seçenek, Site Recovery korumalı VM'lerin kapatın ve verileri eşitleyerek veri en son sürümünü yük devretme denemesi gerektiğini belirtir. Bu seçeneği seçmezseniz veya denemesi başarılı olmazsa, yük devretme sanal makine için en son kullanılabilir kurtarma noktasından olacaktır. 

Site Recovery, aynı veya benzer bir Premium depolama özelliğine sahip VM türü olan bir sanal makine örneği oluşturur. Performans ve çeşitli sanal makine örneği fiyatı giderek denetleyebilirsiniz [Windows sanal makineler fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) veya [Linux sanal makineleri fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

## <a name="post-migration-steps"></a>Geçiş sonrası adımlar

1. **Çoğaltılan VM'ler için kullanılabilirlik varsa kümesini yapılandırma**. Site kurtarma, kullanılabilirlik kümesinin yanı sıra geçirme Vm'leri desteklemez. Çoğaltılmış sanal makinenizin dağıtım bağlı olarak aşağıdakilerden birini yapın:
   * Klasik dağıtım modeliyle oluşturulan bir VM için: VM, Azure portaldaki kullanılabilirlik kümesi ekleyin. Ayrıntılı adımlar için Git [mevcut bir sanal makine bir kullanılabilirlik kümesine ekleme](../linux/classic/configure-availability-classic.md).
   * Resource Manager dağıtım modeliyle oluşturulan bir VM için: VM'nin yapılandırmanızı kaydetmek ve silin ve yeniden kullanılabilirlik kümesindeki VM'ler oluşturun. Bunu yapmak için komut kullanmak [Azure Resource Manager VM kullanılabilirlik Set](https://gallery.technet.microsoft.com/Set-Azure-Resource-Manager-f7509ec4). Bu betiği çalıştırmadan önce kısıtlamalarını denetleyin ve planlamak, kapalı kalma süresi.

2. **Eski Vm'leri ve diskleri silmek**. Premium disklerin kaynak disk ile tutarlı olduğunu ve yeni sanal makinelerin kaynak Vm'leri aynı işlevi gerçekleştirmek emin olun. VM'yi silin ve Azure portalında kaynak depolama hesabından disklerin silin. Bir sorun varsa, disk silinmiş VM silinmiş olsa bile, bkz: [depolama kaynağı silme hatalarını giderme](storage-resource-deletion-errors.md).

3. **Azure Site Recovery altyapısı temiz**. Site Recovery artık gerekli değilse, temel altyapıyla temizleyebilirsiniz. Çoğaltılan öğeler, yapılandırma sunucusunu ve kurtarma ilkesi silin ve sonra Azure Site Recovery kasayı silin.

## <a name="troubleshooting"></a>Sorun giderme

* [İzleme ve sorun giderme sanal makineleri ve fiziksel sunucular için koruma](../../site-recovery/site-recovery-monitoring-and-troubleshooting.md)
* [Microsoft Azure Site Recovery Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr)

## <a name="next-steps"></a>Sonraki adımlar

Sanal makineleri geçirmek için belirli senaryolar için aşağıdaki kaynaklara bakın:

* [Azure sanal makineleri depolama hesapları arasında geçirme](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [Bir Linux sanal sabit disk yükleyin](upload-vhd.md)
* [Amazon AWS geçirme sanal makineleri için Microsoft Azure](https://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Ayrıca, Azure depolama ve Azure sanal makineler hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure Depolama](https://azure.microsoft.com/documentation/services/storage/)
* [Azure Sanal Makineler](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Iaas VM'ler için bir disk türü seçin](disks-types.md)

[1]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-1.png
[2]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-2.png
[3]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-3.png
[4]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-4.png
[5]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-5.png
[6]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-6.PNG
[7]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-7.PNG
[8]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-8.PNG
[9]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-9.PNG
[10]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-10.png
[11]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-11.PNG
[12]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-12.PNG
[13]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-13.png
[14]:../site-recovery/media/site-recovery-vmware-to-azure/v2a-architecture-henry.png
[15]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-14.png
