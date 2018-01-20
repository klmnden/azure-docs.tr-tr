---
title: "Azure Site Recovery ile Azure Premium Depolama'ya Windows sanal makineleri geçirme | Microsoft Docs"
description: "Varolan sanal makinelerinizi Site RECOVERY'yi kullanarak Azure Premium depolama alanına geçirin. Premium Storage, Azure sanal makinelerde çalışan g/Ç kullanımı yoğun iş yükleri için yüksek performanslı, düşük gecikmeli disk desteği sağlar."
services: virtual-machines-windows
cloud: Azure
documentationcenter: na
author: luywang
manager: jeconnoc
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: luywang
ms.openlocfilehash: 66e0e7a1fc620d0be9bd7057be4e04d628da174e
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="migrate-to-premium-storage-by-using-azure-site-recovery"></a>Premium depolama için Azure Site Recovery kullanarak geçirme

[Azure Premium Storage](premium-storage.md) g/Ç kullanımı yoğun iş yükleri çalıştıran sanal makineler (VM'ler) için yüksek performanslı, düşük gecikmeli disk desteği sunar. Bu kılavuz, VM diskleri bir standart depolama hesabından bir premium depolama hesabı kullanarak yükseltmenize yardımcı olur [Azure Site Recovery](../../site-recovery/site-recovery-overview.md).

Site Recovery, şirket içi fiziksel sunucuların ve sanal makineleri buluta (Azure'a) veya ikincil veri merkezine çoğaltılmasını düzenleyerek iş sürekliliği ve olağanüstü durum kurtarma stratejiniz için katkı sağlayan bir Azure hizmetidir. Kesinti birincil konumunuzda meydana gelirse uygulamaları ve iş yüklerini kullanılabilir durumda tutmak için ikincil konuma yük devredersiniz. Normal çalışmasına geri döndüğünde birincil konumunuz başarısız. 

Site Recovery, üretim ortamlarını etkilemeden olağanüstü durum kurtarma ayrıntılarını desteklemek için yük devretme testlerini sağlar. Beklenmeyen olağanüstü için en düşük düzeyde veri kaybı (bağlı olarak çoğaltma sıklığı) ile yük devretmeler çalıştırabilirsiniz. Premium depolama alanına geçiş senaryosunda, kullandığınız [Site Recovery yük](../../site-recovery/site-recovery-failover.md) hedef diskler bir premium depolama hesabına geçirmek için.

Bu seçenek en az kapalı kalma sağladığından Site RECOVERY'yi kullanarak Premium depolama alanına geçirme öneririz. Bu seçenek ayrıca diskleri kopyalama ve yeni VM oluşturma el ile yürütme önler. Site Recovery sistematik olarak disklerinizi kopyalayın ve yük devretme sırasında yeni VM'ler oluşturun. 

Site Recovery türdeki yük devretme kümelemesiyle en az bir sayı veya kapalı kalma süresi destekler. Kapalı kalma süresini ayarlamanıza ve veri kaybı tahmin etmek için bkz: [yük devretme Site kurtarma türlerini](../../site-recovery/site-recovery-failover.md). Varsa, [yük devretme sonrasında Azure Vm'lerine bağlanmak hazırlama](../../site-recovery/vmware-walkthrough-overview.md), yük devretme işleminden sonra RDP kullanarak Azure VM'ye bağlanabilmeniz gerekir.

![Olağanüstü durum kurtarma diyagramı][1]

## <a name="azure-site-recovery-components"></a>Azure Site Recovery bileşenleri

Bu Site Recovery bileşenleri bu geçiş senaryosu için uygun olan:

* **Yapılandırma sunucusu** iletişimi düzenler ve veri çoğaltma ve kurtarma işlemleri yöneten bir Azure VM. Bu VM, yapılandırma sunucusu ve bir ek bileşeni yüklemek için bir tek kurulum dosyasını çalıştırın, bir işlem sunucusu çoğaltma ağ geçidi olarak çağrılır. Hakkında bilgi edinin [yapılandırma sunucusu önkoşulları](../../site-recovery/vmware-walkthrough-overview.md). Yapılandırma sunucusu yalnızca bir kez ayarlamanız ve aynı bölgeye tüm geçişler için kullanın.

* **İşlem sunucusu** çoğaltma ağ geçidi: 

  1. Çoğaltma verilerinin VM'ler kaynağından alır.
  2. Veri önbelleğe alma, sıkıştırma ve şifreleme ile en iyi duruma getirir.
  3. Verileri bir depolama hesabına gönderir. 

  Ayrıca kaynak VM'ler mobilite hizmetinin göndermeli yüklemesi işler ve VM'ler kaynağının otomatik bulma işlemini gerçekleştirir. Varsayılan işlem sunucusu yapılandırma sunucusuna yüklenir. Dağıtımınız ölçeklendirmek için ek tek başına işlem sunucuları dağıtabilirsiniz. Hakkında bilgi edinin [işlem sunucusu dağıtımı için en iyi uygulamaları](https://azure.microsoft.com/blog/best-practices-for-process-server-deployment-when-protecting-vmware-and-physical-workloads-with-azure-site-recovery/) ve [ek işlem sunucuları dağıtma](../../site-recovery/site-recovery-plan-capacity-vmware.md#deploy-additional-process-servers). İşlem sunucusu yalnızca bir kez ayarlamanız ve aynı bölgeye tüm geçişler için kullanın.

* **Mobility hizmeti** çoğaltmak istediğiniz her standart VM üzerinde dağıtılan bir bileşenidir. Yakalar ve standart VM'de veri yazar ve işlem sunucusuna gönderir. Hakkında bilgi edinin [makine önkoşulları çoğaltılan](../../site-recovery/vmware-walkthrough-overview.md).

Bu grafik, bu bileşenlerin nasıl etkileşim gösterir:

![Site Recovery bileşenlerini etkileşim][15]

> [!NOTE]
> Site Recovery, depolama alanları disklerini geçişini desteklemez.

Diğer senaryolar için ek bileşenler için bkz: [senaryo mimarisinin](../../site-recovery/vmware-walkthrough-overview.md).

## <a name="azure-essentials"></a>Azure temelleri

Bu geçiş senaryosu için Azure gereksinimleri şunlardır:

* Azure aboneliği.
* Çoğaltılan verileri depolamak için bir Azure premium depolama hesabı.
* Yük devretme sırasında oluşturulduğunda VM'ler bağlanacağı bir Azure sanal ağı. Azure sanal ağı, Site Recovery çalıştığı biri ile aynı bölgede olması gerekir.
* Çoğaltma günlüklerini depolamak için Azure standard storage hesabı. Bu Geçirilmekte olan VM diskleri için aynı depolama hesabı olabilir.

## <a name="prerequisites"></a>Önkoşullar

* Önceki bölümde ilgili geçiş senaryosu bileşenleri anlayın.
* Hakkında bilgi edinerek, kesinti planlama [Site Recovery yük](../../site-recovery/site-recovery-failover.md).

## <a name="setup-and-migration-steps"></a>Kurulum ve geçiş adımları

Azure Iaas Vm'leri aynı bölgedeki veya bölgeler arasında geçirmek için Site Recovery kullanabilirsiniz. Bu makalede geçiş senaryosu için aşağıdaki yönergeleri uyarlanabilir [çoğaltmak VMware Vm'lerini veya fiziksel sunucuları azure'a](../../site-recovery/vmware-walkthrough-overview.md). Bu makaledeki yönergeleri yanı sıra ayrıntılı adımlar için bağlantıları izleyin.

### <a name="step-1-create-a-recovery-services-vault"></a>1. adım: bir kurtarma Hizmetleri kasası oluşturma

1. [Azure portalı](https://portal.azure.com) açın.
2. Seçin **yeni** > **Yönetim** > **yedekleme** ve **konum Kurtarma (OMS)**. Alternatif olarak, seçebileceğiniz **Gözat** > **kurtarma Hizmetleri kasası** > **Ekle**. 
3. VM'ler için yinelenen bir bölge belirtin. Aynı bölgede geçiş amacıyla, kaynak VM'ler ve kaynak depolama hesaplarının bulunduğu bölgeyi seçin. 

### <a name="step-2-choose-your-protection-goals"></a>2. adım: koruma hedeflerinizi seçme 

1. Yapılandırma sunucusu yüklemek istediğiniz VM üzerinde açmak [Azure portal](https://portal.azure.com).
2. Git **kurtarma Hizmetleri kasaları** > **ayarları** > **Site Recovery** > **1. adım: hazırlama Altyapı** > **koruma hedefi**.

   ![Koruma hedefi bölmesine göz atma][2]

3. Altında **koruma hedefi**, ilk açılan listeden seçin **için Azure**. İkinci aşağı açılan listesinde seçin **değil sanallaştırılmış / diğer**ve ardından **Tamam**.

   ![Doldurulmuş kutuları ile koruma hedefi bölmesi][3]

### <a name="step-3-set-up-the-source-environment-configuration-server"></a>3. adım: kaynak ortamını (yapılandırma sunucusu) ayarlama

1. Karşıdan **Azure Site Recovery birleşik Kurulumu** ve kasa kayıt anahtarı'na giderek **altyapıyı hazırlama** > **kaynağı**  >  **Sunucu Ekle** bölmeleri. 
 
   Birleşik Kur'u çalıştırmak için kasa kayıt anahtarı gerekir. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

   ![Sunucu Ekle bölmesine göz atma][4]

2. İçinde **Sunucu Ekle** bölmesinde, bir yapılandırma sunucusu ekleyin.

   ![Seçili yapılandırma sunucusuyla sunucu bölmesi ekleme][5]

3. Yapılandırma sunucusu olarak kullandığınız VM birleşik yapılandırma sunucusu ve işlem sunucusu yüklemek için Kurulumu çalıştırın. Yapabilecekleriniz [ekran görüntüleri yol](../../site-recovery/vmware-walkthrough-overview.md) yüklemeyi tamamlamak için. Bu geçiş senaryosu için belirtilen adımları için aşağıdaki ekran görüntüleri başvurabilirsiniz.

   1. İçinde **başlamadan önce**seçin **işlem sunucusu ve yapılandırma sunucusu yüklemek**.

      ![Başlamadan önce sayfası][6]

   2. İçinde **kayıt**göz atın ve kasadan indirdiğiniz kayıt anahtarı seçin.

      ![Kayıt sayfası][7]

   3. **Ortam Ayrıntıları**’nda VMware sanal makinelerini çoğaltıp çoğaltmayacağınızı seçin. Bu geçiş senaryosu için seçme **Hayır**.

      ![Ortam Ayrıntıları sayfası][8]

4. Yükleme tamamlandıktan sonra aşağıdakileri **Microsoft Azure Site kurtarma yapılandırma sunucusu** penceresi:
 
   1. Kullanmak **hesaplarını yönetme** sekmesi, Site kurtarma hesabı oluşturmak için otomatik bulma için kullanabilirsiniz. (Fiziksel makineleri koruma hakkında senaryosu, hesap kurma ilgili değildir ancak aşağıdaki adımlardan birini etkinleştirmek için en az bir hesabı gerekir. Bu durumda, tüm parola ve hesap adı verebilirsiniz.) 
   2. Kullanım **kasa kayıt** için kasa kimlik bilgilerini karşıya yüklemek için sekmesi.

      ![Kasa kayıt sekmesi][9]

### <a name="step-4-set-up-the-target-environment"></a>Adım 4: hedef ortamını ayarlama

Seçin **altyapıyı hazırlama** > **hedef**ve yük devretme sonrasında VM'ler için kullanmak istediğiniz dağıtım modelini belirtin. Seçebileceğiniz **Klasik** veya **Resource Manager**senaryonuza bağlı olarak.

![Hedef bölmesi][10]

Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler. 

> [!NOTE]
> Çoğaltılan veriler için bir premium depolama hesabı kullanıyorsanız, çoğaltma günlüklerini depolamak için bir ek bir standart depolama hesabı ayarlamanız gerekir.

### <a name="step-5-set-up-replication-settings"></a>5. adım: çoğaltma ayarlarını belirleme

Yapılandırma sunucusu oluşturduğunuz çoğaltma ilkesiyle başarıyla ilişkilendirildi olduğunu doğrulamak için izleyin [çoğaltma ayarlarını belirleme](../../site-recovery/vmware-walkthrough-overview.md).

### <a name="step-6-plan-capacity"></a>6. adım: Planı kapasite

1. Kullanım [kapasite Planlayıcısı](../../site-recovery/site-recovery-capacity-planner.md) ağ bant genişliği, depolama ve diğer gereksinimler, çoğaltma karşılamak için doğru şekilde tahmin gerekiyor. 
2. İşiniz bittiğinde seçin **Evet, yaptım** içinde **kapasite planlamasını tamamladınız?**.

   ![Kapasite planlama tamamlandığını onaylayan kutusu][11]

### <a name="step-7-install-the-mobility-service-and-enable-replication"></a>7. adım: mobilite hizmetinin yüklenmesi ve çoğaltmayı etkinleştirme

1. Seçebileceğiniz [anında yükleme](../../site-recovery/vmware-walkthrough-overview.md) kaynak Vm'leriniz veya çok [mobility hizmeti el ile yüklemek](../../site-recovery/site-recovery-vmware-to-azure-install-mob-svc.md) kaynağınız VM'ler üzerinde. Yükleme ve el ile yükleyici yolu sağlanan bağlantıyı Ftp'den zorunluluğu bulabilirsiniz. El ile yükleme yaptığınız, yapılandırma sunucusu bulmak için bir iç IP adresi kullanmanız gerekebilir.

   ![Yapılandırma sunucusu Ayrıntıları sayfası][12]

   Başarısız üzerinden VM iki geçici disk olacaktır: birincil VM ve diğer VM kurtarma bölgede hazırlama sırasında oluşturulan birinden. Geçici disk çoğaltma önce dışlamak için çoğaltma etkinleştirmeden önce mobility hizmetini yükleyin. Geçici disk hariç hakkında daha fazla bilgi için bkz: [Çoğaltmada diskleri Dışla](../../site-recovery/vmware-walkthrough-overview.md).

2. Şekilde çoğaltmayı etkinleştirin:
   1. Seçin **uygulama çoğaltma** > **kaynak**. Çoğaltma ilk kez etkinleştirdikten sonra seçin **+ Çoğalt** ilave makineler için çoğaltmayı etkinleştirmek için kasada.
   2. 1. adımda ayarladığınız **kaynak** işlem sunucusu olarak.
   3. Adım 2'de, yük devretme sonrası dağıtım modeli, geçirmek için premium depolama hesabı, günlükleri ve başarısız bir sanal ağı kaydetmek için bir standart depolama hesabı belirtin.
   4. 3. adımda IP adresine göre korumalı sanal makineleri ekleyin. (Bunları bulmak için bir iç IP adresi gerekebilir.)
   5. 4. adımda, daha önce işlem sunucusu ayarladığınız hesap seçerek özelliklerini yapılandırın.
   6. 5. adımda, daha önce oluşturduğunuz çoğaltma ilkesi seçin "5. adım: çoğaltma ayarlarını belirleme."
   7. **Tamam**’ı seçin.

   > [!NOTE]
   > Bir Azure VM serbest ve yeniden başlatıldığı zaman aynı IP adresi al garantisi yoktur. IP adresi yapılandırması sunucu/işlem sunucusunun veya korumalı Azure sanal makinelerini değişirse, bu senaryoda çoğaltma düzgün çalışmayabilir.

   ![Seçili kaynağı ile çoğaltması bölmesini etkinleştirme][13]

Azure depolama ortamınızı tasarlarken, bir kullanılabilirlik kümesindeki her bir VM için ayrı bir depolama hesapları kullanmanızı öneririz. Depolama katmanı için en iyi uygulamada izlemenizi öneririz [her kullanılabilirlik kümesi için birden çok depolama hesabı kullanmak](../linux/manage-availability.md). Birden çok depolama hesabı VM diskleri dağıtma, depolama alanı kullanılabilirliği artırmaya yardımcı olur ve g/ç Azure depolama altyapınız genelinde dağıtır.

Diskleri tüm VM'lerin bir storage hesabınıza çoğaltmak yerine bir kullanılabilirlik kümesinde Vm'leriniz olması durumunda birden çok kez birden çok VM geçirme öneririz. Bu şekilde, sanal makineleri aynı kullanılabilirlik kümesindeki tek bir depolama hesabına paylaşmayın. Kullanım **çoğaltmayı etkinleştirme** bölmesinde her sanal makine için bir kerede bir hedef depolama hesabı ayarlama.
 
Gereksiniminize uygun bir yük devretme sonrası dağıtım modeli seçebilirsiniz. Azure Resource Manager yük devretme sonrası dağıtım modeli olarak seçerseniz, bir VM (Resource Manager) (Resource Manager) VM devredebildiğini veya VM'ye (Klasik) bir VM (Resource Manager) devredebilir.

### <a name="step-8-run-a-test-failover"></a>8. adım: yük devretme testi çalıştırma

Çoğaltma tam olup olmadığını denetlemek için Site Recovery örneğini seçin ve ardından **ayarları** > **çoğaltılan öğeler**. Çoğaltma işleminizi yüzdesi ve durumu görürsünüz. 

İlk çoğaltma sonrasında, çoğaltma stratejinizi doğrulamak için bir yük devretme testi çalıştırmak, tamamlanır. Yük devretme testi ayrıntılı adımlar için bkz: [Site kurtarma için yük devretme testi çalıştırmak](../../site-recovery/vmware-walkthrough-overview.md). 

> [!NOTE]
> Tüm yük devretme çalıştırmadan önce VM'ler ve çoğaltma stratejisi gereksinimleri karşıladığından emin olun. Yük devretme testi çalıştırma hakkında daha fazla bilgi için bkz: [Test yük devretme Azure Site Recovery](../../site-recovery/site-recovery-test-failover-to-azure.md).

Test yük devretme kümesinde durumunu görebilirsiniz **ayarları** > **işleri** > *YOUR_FAILOVER_PLAN_NAME*. Bölmesinde adımlar ve başarı/hata sonuçları dökümünü görebilirsiniz. Yük devretme sınaması herhangi bir adımda başarısız olursa, hata iletisi denetlemek için adım seçin. 

### <a name="step-9-run-a-failover"></a>9. adım: yük devretme gerçekleştirme

Test sonra disklerinizi Premium depolama alanına geçirmek ve VM örnekleri çoğaltmak için yük devretme gerçekleştirme yük devretme tamamlanır. Ayrıntılı adımları [yük devretme gerçekleştirme](../../site-recovery/site-recovery-failover.md#run-a-failover). 

Seçtiğinizden emin olun **sanal makineleri kapatın ve en son verileri eşitleyin**. Bu seçenek, Site Recovery korumalı sanal makineleri kapatın ve böylece verilerin en son sürümünü devredilir verileri eşitlemek denemelisiniz belirtir. Bu seçeneği seçmeyin veya denemesi başarılı değil, yük devretme VM için en son kullanılabilir kurtarma noktası arasında olacaktır. 

Site Recovery, aynı veya benzer bir Premium depolama özellikli VM türü olan bir VM örneği oluşturur. Giderek çeşitli VM örnekleri fiyat ve performans denetleyebilirsiniz [Windows sanal makineler fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) veya [Linux sanal makineleri fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

## <a name="post-migration-steps"></a>Geçiş sonrası adımlar

1. **Çoğaltılmış VM'ler varsa kullanılabilirlik için yapılandırma**. Site Recovery kullanılabilirlik kümesi ile birlikte geçirme sanal makineleri desteklemez. Çoğaltılmış VM'yi dağıtım bağlı olarak aşağıdakilerden birini yapın:
   * Bir VM Klasik dağıtım modeli aracılığıyla oluşturulan için: VM kullanılabilirlik Azure portalında kümesini ekleyin. Ayrıntılı adımlar için Git [var olan bir sanal makine bir kullanılabilirlik kümesine ekleme](../linux/classic/configure-availability-classic.md).
   * Bir VM Resource Manager dağıtım modeli aracılığıyla oluşturulan için: VM yapılandırmanızı kaydetmek ve ardından silin ve kullanılabilirlik kümesindeki sanal makineleri yeniden oluşturun. Bunu yapmak için komut kullanın [Azure Kaynak Yöneticisi'ni VM kullanılabilirlik Set](https://gallery.technet.microsoft.com/Set-Azure-Resource-Manager-f7509ec4). Bu komut dosyasını çalıştırmadan önce kısıtlamalarını denetleyin ve, kapalı kalma süresi planlayın.

2. **Eski VM'ler ve diskleri silme**. Yeni VM'ler VM'ler kaynak ile aynı işlevi gerçekleştirir ve Premium disklerin kaynak diskler ile tutarlı olduğundan emin olun. VM ve Azure portalındaki kaynak depolama hesaplarınızdan diskleri silin. Bir sorun varsa disk olmayan silinmiş VM silinmiş olsa bile, bkz: [VHD'ler sildiğinizde hatalarında sorun giderme](../../storage/common/storage-resource-manager-cannot-delete-storage-account-container-vhd.md).

3. **Azure Site Recovery altyapısı temiz**. Site Recovery artık gereksinim duyulmuyorsa, kendi altyapısını temizleyebilirsiniz. Çoğaltılan öğe, yapılandırma sunucusunu ve kurtarma ilkesi silin ve sonra Azure Site Recovery kasası silin.

## <a name="troubleshooting"></a>Sorun giderme

* [İzleme ve sanal makineleri ve fiziksel sunucuları için koruması sorunlarını giderme](../../site-recovery/site-recovery-monitoring-and-troubleshooting.md)
* [Microsoft Azure Site Recovery Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr)

## <a name="next-steps"></a>Sonraki adımlar

Sanal makineleri geçirmek için belirli senaryolar için aşağıdaki kaynaklara bakın:

* [Azure sanal makineleri depolama hesapları arasında geçiş](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [Oluşturma ve Windows Server VHD Azure'a yükleyin](classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Oluşturma ve Linux işletim sistemini içeren bir sanal sabit disk karşıya yükleme](../linux/classic/create-upload-vhd-classic.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Geçirme sanal makinelerden Amazon AWS Microsoft Azure](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Ayrıca, Azure Storage ve Azure sanal makineler hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure Depolama](https://azure.microsoft.com/documentation/services/storage/)
* [Azure Sanal Makineler](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Premium Depolama: Azure sanal makine iş yükleri için yüksek performanslı depolama](premium-storage.md)

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
