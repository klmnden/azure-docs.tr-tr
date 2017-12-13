---
title: "Azure Site Recovery ile azure'a AWS Vm'leri geçirme | Microsoft Docs"
description: "Bu makalede, Amazon Web Hizmetleri (AWS) için Azure çalışan, Azure Site Recovery kullanarak sanal makineleri geçirmek açıklar."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ddb412fd-32a8-4afa-9e39-738b11b91118
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/01/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 814d8ee4952dd08707849eadc1e4e97ab6087da0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="migrate-amazon-web-services-aws-vms-to-azure"></a>Amazon Web Hizmetleri (AWS) Vm'leri Azure'a geçirme

Bu öğretici, Amazon Web Hizmetleri (AWS) sanal makineleri (VM'ler) geçirmek Azure Site Recovery kullanarak sanal makineleri öğretir. Azure'a geçirme EC2 örnekleri, sanal makineleri fiziksel olmaları durumunda olarak kabul edilir, şirket içi bilgisayarları. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure kaynaklarını hazırlama
> * AWS EC2 örnekleri geçiş için hazırlama
> * Yapılandırma sunucusu dağıtma
> * VM'ler için çoğaltmayı etkinleştirme
> * Her şeyi emin olmak için yük devretme testi çalışma
> * Azure için tek seferlik bir yük devretmeyi çalıştırma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.


## <a name="prepare-azure-resources"></a>Azure kaynaklarını hazırlama 

Birkaç kaynakları kullanmak için Azure içinde geçirilen EC2 örnekleri için hazır olması gerekir. Bunlar, bir depolama hesabı, bir kasa ve sanal ağ içerir.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Azure depolama alanında çoğaltılan makinelerin görüntüleri tutulur. Azure VM'ler, yük devretmeyi Azure için şirket içi, depolama biriminden oluşturulur.

1. İçinde [Azure portal](https://portal.azure.com) menüsünde tıklatın **yeni** -> **depolama** -> **depolama hesabı**.
2. Depolama hesabınız için bir ad girin. Bu öğreticileri için kullanırız adı **awsmigrated2017**. Adı Azure içinde benzersiz olması ve 3 ile 24 karakter, yalnızca sayılar ve küçük harfler arasında olması gerekir.
3. Varsayılanları tutun **dağıtım modeli**, **tür hesap**, **performans**, ve **güvenli aktarımı gerekli**.
5. Varsayılan seçmek **RA-GRS** için **çoğaltma**.
6. Bu öğretici için kullanmak istediğiniz aboneliği seçin.
7. İçin **kaynak grubu**seçin **Yeni Oluştur**. Bu örnekte, kullandığımız **migrationRG** adı.
8. Seçin **Batı Avrupa** konumu olarak. 
9. Depolama hesabını oluşturmak için **Oluştur**’a tıklayın.

### <a name="create-a-vault"></a>Bir kasa oluşturun

1. İçinde [Azure portal](https://portal.azure.com), sol gezinti bölmesinde tıklatın **daha fazla hizmet** arayın ve seçin **kurtarma Hizmetleri kasaları**.
2. Kurtarma Hizmetleri kasaları sayfasını tıklatın **+ Ekle** sayfasının sol üst köşedeki.
3. İçin **adı**, türü *myVault*. 
4. İçin **abonelik**, ilgili aboneliğini seçin.
4. İçin **kaynak grubu**seçin **var olanı kullan** seçip *migrationRG*. 
5. İçinde **konumu**seçin *Batı Avrupa*. 
5. Yeni kasa panodan hızlı bir şekilde erişmek için seçin **panoya Sabitle**.
7. İşiniz bittiğinde tıklatın **oluşturma**.

Yeni kasa kasasındaki **Pano** > **tüm kaynakları**ve ana **kurtarma Hizmetleri kasaları** sayfası.

### <a name="set-up-an-azure-network"></a>Azure ağı ayarlama

Azure sanal makinelerini (yük devretme) geçişten sonra oluşturduğunuzda, bu ağa alanına.

1. İçinde [Azure portal](https://portal.azure.com), tıklatın **yeni** > **ağ** >
    **sanal ağ**
3. İçin **adı**, türü *myMigrationNetwork*.
4. Varsayılan değeri bırakın **adres alanı**.
5. İçin **abonelik**, ilgili aboneliğini seçin.
6. İçin **kaynak grubu**seçin **kullanım varolan** ve *migrationRG* açılan gelen.
7. İçin **konumu**seçin **Batı Avrupa**. 
8. Varsayılan değerleri bırakın **alt**, her iki **adı** ve **IP aralığı**.
9. Bırakın **hizmet uç noktaları** devre dışı.
10. İşiniz bittiğinde tıklatın **oluşturma**.


## <a name="prepare-the-ec2-instances"></a>EC2 örnekleri hazırlama

Geçirmek istediğiniz bir veya daha fazla VM gerekir. Bu EC2 örneği 64 bit sürümü Windows Server 2008 R2 SP1 veya sonraki sürümleri, Windows Server 2012, Windows Server 2012 R2 veya Red Hat Enterprise Linux 6.7 (yalnızca HVM sanallaştırılmış örnekleri) çalıştırması gerekir. Sunucu yalnızca Citrix PV veya AWS gd sürücüleri yüklü olmalıdır. RedHat PV sürücüleri çalışan örnekleri desteklenmez.

Mobility hizmetinin çoğaltmak istediğiniz her bir VM üzerinde yüklü olmalıdır. Sanal makine için çoğaltmayı etkinleştirdiğinizde, site kurtarma Bu hizmeti otomatik olarak yüklenir. Otomatik yükleme için bir hesap Site Recovery VM erişmek için kullanacağı EC2 örneklerinde hazırlamanız gerekir.

Bir etki alanı veya yerel hesabı kullanabilirsiniz. Linux VM'ler için hesabın kaynak Linux sunucusu kökünde olması gerekir. Windows etki alanı hesabı, yerel makinede uzak kullanıcı erişim denetimi devre dışı bırak kullanmıyorsanız VM'ler için:

  - Kayıt defterinde altında **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, DWORD girdisi eklemek **LocalAccountTokenFilterPolicy** ve değerini 1 olarak ayarlayın.
    
Ayrıca Site Recovery yapılandırma sunucusu olarak kullanabilmeniz için ayrı bir EC2 örneği gerekir. Bu örnek, Windows Server 2012 R2 çalıştırması gerekir. 
    

## <a name="prepare-the-infrastructure"></a>Altyapıyı hazırlama 

Kasanız için portal sayfasında seçin **Site Recovery** gelen **Başlarken** bölümünde ve ardından **altyapıyı hazırlama**.

### <a name="1-protection-goal"></a>1 koruma hedefi

Aşağıdaki değerleri seçin **koruma hedefi** sayfa:
   
|    |  | 
|---------|-----------|
| Makinelerinizi bulunduğu? | **Şirket içi**|
| Nereye makinelerinizi çoğaltmak istiyorsunuz? |**Azure'a**|
| Makineleriniz sanallaştırılmış mı? | **Değil sanallaştırılmış / diğer**|

İşiniz bittiğinde tıklatın **Tamam** sonraki bölüme taşımak için.

### <a name="2-source-prepare"></a>2 kaynak hazırlama 

Üzerinde **kaynağı** sayfasında, **+ yapılandırma sunucusu**. 

1. Yapılandırma sunucusu oluşturmak ve kurtarma kasası ile kaydetmek için Windows Server 2012 R2 çalıştıran bir EC2 örneği kullanın.

2. Bunu erişebilmesi için yapılandırma sunucusu olarak kullandığınız VM EC2 örneğinde proxy yapılandırma [hizmeti URL'leri](site-recovery-support-matrix-to-azure.md).

3. Karşıdan [Microsoft Azure Site Recovery birleşik Kurulumu](http://aka.ms/unifiedinstaller_wus) program. Yerel makinenize indirmek ve ardından üzerinden yapılandırma sunucusu olarak kullanıyorsanız VM kopyalayın.

4. Tıklayın **karşıdan** düğmesi kasa kayıt anahtarını indirin. Yapılandırma sunucusu olarak kullanıyorsanız VM üzerinden indirilen dosya kopyalayın.

5. İndirdiğiniz için yükleyici VM üzerinde sağ **Microsoft Azure Site Recovery birleşik Kurulumu** seçip **yönetici olarak çalıştır**. 

    1. İçinde **başlamadan önce**seçin **işlem sunucusu ve yapılandırma sunucusu yüklemek** ve ardından **sonraki**.
    2. İçinde **üçüncü taraf yazılım lisansı**seçin **üçüncü taraf lisans sözleşmesini kabul ediyorum.** ve ardından **sonraki**.
    3. İçinde **kayıt**, Gözat'ı tıklatın ve burada kasa kayıt anahtarı dosyasını alın ve ardından gidin **sonraki**.
    4. İçinde **Internet ayarlarını**seçin **bir proxy sunucu olmadan Azure Site Recovery bağlanın.** ve ardından **sonraki**.
    5. İçinde **Önkoşul denetimi** sayfasında, birden çok öğe için denetimleri çalıştırır. Bu tamamlandığında tıklatın **sonraki**. 
    6. İçinde **MySQL yapılandırma**, gerekli parola sağlayın ve ardından **sonraki**.
    7. İçinde **ortam ayrıntıları**seçin **Hayır**, VMware makineleri korumak ve ardından gerekmez **sonraki**.
    8. İçinde **yükleme konumu**, tıklatın **sonraki** varsayılanı kabul etmek için.
    9. İçinde **Ağ Seçimi**, tıklatın **sonraki** varsayılanı kabul etmek için.
    10. İçinde **Özet** tıklatın **yükleme**.
    11. **Yükleme ilerleme durumu** yükleme sürecinde olduğu hakkında bilgi gösterir. Bu tamamlandığında tıklatın **son**. Olası bir yeniden başlatma gerektiren hakkında bir açılır pencere almak, tıklatın **Tamam**. Ayrıca yapılandırma sunucusunun bağlantı parolası hakkında bir açılır pencere alma, parola panonuza kopyalayın ve güvenli bir yere kaydedin.
    
6. VM üzerinde çalışan **cspsconfigtool.exe** yapılandırma sunucusunda bir veya daha fazla yönetim hesabı oluşturmak için. Yönetim hesapları, geçirmek istediğiniz EC2 örneklerinde yönetici izinlerine sahip olduğunuzdan emin olun. 

İşiniz bittiğinde yapılandırma sunucuyu kurma, portalına geri dönün ve yeni oluşturduğunuz için sunucuyu seçin **yapılandırma sunucusu** tıklatıp *Tamam** 3 hedef hazırlama adım geçmek için.

### <a name="3-target-prepare"></a>3 hedef hazırlama 

Bu bölümdeki kaynaklar hakkında bilgi olduğunuzda oluşturduğunuz girin [hazırlama Azure kaynakları](#prepare-azure-resources) bölümüne, Bu öğretici.

1. İçinde **abonelik**, için kullanılan Azure aboneliğini seçin [hazırlama Azure](tutorial-prepare-azure.md) Öğreticisi.
2. Dağıtım modeli olarak **Kaynak Yöneticisi**’ni seçin.
3. Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler. Bunlar olduğunuzda oluşturduğunuz kaynaklar olmalıdır [hazırlama Azure kaynakları](#prepare-azure-resources) bölümünde, bu öğreticide
4. İşiniz bittiğinde **Tamam**’a tıklayın.


### <a name="4-replication-settings-prepare"></a>4 çoğaltma ayarları hazırla 

Çoğaltma etkinleştirebilmeniz için önce bir çoğaltma ilkesi oluşturmanız gerekir

1. Tıklatın **+ çoğaltabilir ve ilişkilendirmek**.
2. İçinde **adı**, türü **myReplicationPolicy**.
3. Geri kalan varsayılan ayarları bırakın ve tıklatın **Tamam** bir ilke oluşturmak için. Yeni ilke yapılandırma sunucusuyla otomatik olarak ilişkilendirilir. 

### <a name="5-deployment-planning-select"></a>5 dağıtım planlama seçin 

İçinde **dağıtım planlamasını tamamladınız?**seçin **ı daha sonra yapacağınız** açılan gelen ve ardından **Tamam**.

Tüm tamamladığınızda tüm 5 bölümlerle **altyapıyı hazırlama**, tıklatın **Tamam**.


## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Geçirmek istediğiniz her sanal makine için çoğaltmayı etkinleştirin. Çoğaltma etkin olduğunda, Site Recovery Mobility hizmeti otomatik olarak yüklenir. 

1. [Azure portalı](htts://portal.azure.com) açın.
1. Kasa adınız sayfasında altında **Başlarken**, tıklatın **Site Recovery**.
2. Altında **şirket içi makineler ve Azure Vm'leri için**, tıklatın **adım 1:Replicate uygulama**. Aşağıdaki bilgileri ve tıklatın sihirbaz sayfalarını tamamlamak **Tamam** bittiğinde her sayfada:
    - 1 kaynak yapılandırın:
      
    |  |  |
    |-----|-----|
    | Kaynak: | **Şirket içinde**|
    | Kaynak konumu:| Yapılandırma sunucusu EC2 örneğinizi adı.|
    |Makine türü: | **Fiziksel makineleri**|
    | İşlem sunucusu: | Yapılandırma sunucusu aşağı açılan listeden seçin.|
    
    - 2 hedefini yapılandırın
        
    |  |  |
    |-----|-----|
    | Hedef: | Varsayılan adı bırakın.|
    | Abonelik: | Kullanmakta olduğunuz abonelik seçin.|
    | Yük devretme sonrasında kaynak grubu:| Oluşturduğunuz kaynak grubunu kullanmak [hazırlama Azure kaynakları](#prepare-azure-resources) bölümü.|
    | Yük devretme sonrası dağıtım modeli: | Seçin **Kaynak Yöneticisi**|
    | Depolama hesabı: | Depolama hesap içinde oluşturduğunuz seçin [hazırlama Azure kaynakları](#prepare-azure-resources) bölümü.|
    | Azure ağ: | Seçin **seçili makineler için Şimdi Yapılandır**|
    | Yük devretme sonrasında Azure ağ: | Oluşturduğunuz ağ seçin [hazırlama Azure kaynakları](#prepare-azure-resources) bölümü.|
    | Alt ağ: | Seçin **varsayılan** gelen açılır.|
    
    - 3 fiziksel makineleri seçin
        
        Tıklatın **+ fiziksel makine** yazıp enter **adı**, **IP adresi** ve **işletim sistemi türü** geçirmek istediğiniz EC2 örneğinin ve ardından **Tamam**.
        
    - 4 özellikler özelliklerini yapılandırın
        
        Aşağı açılan yapılandırma sunucusundan oluşturduğunuz hesabı seçin ve tıklatın **Tamam**.
        
    - 5 çoğaltma ayarları çoğaltma ayarlarını yapılandır
    
        Açılan seçili çoğaltma ilkesi olduğundan emin olun **myReplicationPolicy** ve ardından **Tamam**.
        
3. Sihirbaz tamamlandığında tıklatın **çoğaltmasını etkinleştir**.
        

İlerleme durumunu izleyebilirsiniz **korumayı etkinleştir** iş **izleme ve Raporlama** > **işleri** > **Site Recovery işleri** . **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.        
        
Bir sanal makine için çoğaltmayı etkinleştirdiğinizde, 15 dakika alabilir veya artık değişikliklerinin için efekt ve portalda görüntülenir.

## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Yük devretme testi çalıştırdığınızda, şunlar olur:

1. Tüm yük devretme için gereken koşulların karşılandığından emin olmak için çalışır bir ön koşullarını denetleyin.
2. Bir Azure VM oluşturulan yük devretme verileri işler. En son kurtarma noktası, bir kurtarma noktası seçin, verilerden oluşturulur.
3. Bir Azure VM, önceki adımda işlenen veriler kullanılarak oluşturulur.

Portalda, yük devretme sınaması aşağıdaki gibi çalıştırın:

1. Kasanız için sayfada Git **korunan öğeler** > **çoğaltılan öğeler**> VM tıklayın > **+ yük devretme testi**.

2. Yük devretme için kullanılacak bir kurtarma noktası seçin:
    - **En son işlenen**: VM üzerinden Site Recovery tarafından işlenen en son kurtarma noktası başarısız olur. Zaman damgası gösterilir. Bu seçenek ile düşük RTO (Kurtarma süresi hedefi) sağlayan verileri işleme hiçbir zaman harcanmaktadır.
    - **Son uygulama tutarlı**: tüm sanal makineleri en son uygulamayla tutarlı kurtarma noktası için bu seçeneği yöneltilir. Zaman damgası gösterilir.
    - **Özel**: herhangi bir kurtarma noktası seçin.
3. İçinde **yük devretme testi**, hedef Azure seçin Azure Vm'lerinin bir ağa bağlı olacak yük devretme gerçekleştikten sonra. Bu, oluşturduğunuz ağ olmalıdır [hazırlama Azure kaynakları](#prepare-azure-resources) bölümü.
4. Yük devretmeyi başlatmak için **Tamam**'a tıklayın. VM özelliklerini açmak için tıklayarak ilerleme durumunu izleyebilirsiniz. Veya tıklatabilirsiniz **yük devretme testi** , kasaya sayfasında iş **izleme ve Raporlama** > **işleri** >
   **Site Kurtarma işleri**.
5. Yük devretme işlemi tamamlandıktan sonra çoğaltma Azure VM Azure portalında görünür. > **sanal makineleri**. VM sağ ağa bağlı uygun boyutta olduğundan ve çalışıp çalışmadığını denetleyin.
6. Artık Azure çoğaltılmış VM'yi bağlanabiliyor olması gerekir.
7. Yük devretme testi sırasında oluşturulan Azure VM'ler silmek için tıklatın **temizleme yük devretme testi** kurtarma planı üzerinde. İçinde **notları**, kaydetme ve yük devretme testiyle ilişkili gözlemlerinizi kaydetmek.

Bazı senaryolarda, yük devretme tamamlamak için yaklaşık sekiz ile on dakika sürer ek işleme gerektirir. 


## <a name="migrate-to-azure"></a>Azure’a geçiş

Azure Vm'lerine geçirmek EC2 örnekleri için gerçek bir yük devretme çalıştırın. 

1. İçinde **korunan öğeler** > **öğeleri çoğaltılan** AWS örnekleri > **yük devretme**.
2. İçinde **yük devretme** seçin bir **kurtarma noktası** yük. En son kurtarma noktası seçin.
3. Seçin **yük devretme işlemine başlamadan önce makineyi kapatın** Site Kurtarma'nın bir kapanma kaynak sanal makinelerin yük devretme tetiklemeden yapmaya istiyorsanız. Kapatma başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleyişini izleyin **işleri** sayfası.
4. VM görünür onay **öğeleri çoğaltılan**. 
5. Her VM'ye sağ tıklayın > **tamamlamak geçiş**. Bu geçiş işlemini tamamlar, AWS sanal makine için çoğaltmayı durdurur ve VM için Site Recovery Faturalaması durdurulur.

    ![Geçişi tamamlama](./media/tutorial-migrate-aws-to-azure/complete-migration.png)

> [!WARNING]
> **Bir yük devretme devam ediyor iptal etme**: yük devretme başlatılmadan önce VM çoğaltma durduruldu. Bir yük devretme devam ediyor, yük devretme durduruyor, iptal ancak VM yeniden çoğaltma olmaz  


    

## <a name="next-steps"></a>Sonraki adımlar

Bu konuda, AWS EC2 örneklerini Azure Vm'lerine geçirmek nasıl öğrendiniz. Azure VM'ler hakkında daha fazla bilgi için Windows VM'ler için öğreticileri için devam edin.

> [!div class="nextstepaction"]
> [Azure Windows sanal makine öğreticileri](../virtual-machines/windows/tutorial-manage-vm.md)
