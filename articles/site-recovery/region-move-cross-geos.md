---
title: Azure kamu ve Azure Site Recovery hizmeti ile genel bölgeler arasında Azure Iaas Vm'leri Taşı | Microsoft Docs
description: Azure kamu ve genel bölgeler arasında Azure Iaas sanal makineleri taşımak için Azure Site RECOVERY'yi kullanın.
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: fe2620c7a07389b2a86d36420eadd2ef5883f5da
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60013236"
---
# <a name="move-azure-vms-between-azure-government-and-public-regions"></a>Azure kamu ve genel bölgeler arasında Azure Vm'leri Taşı 

İdare nedeniyle, ayrıntılı olarak veya mevcut Vm'lerinizin kullanılabilirliğini artırmak, yönetilebilirlik artırmak için Azure kamu ve genel bölgeler arasında Iaas Vm'lerinizi taşımak isteyebilirsiniz [burada](azure-to-azure-move-overview.md).

Kullanmanın yanı sıra [Azure Site Recovery](site-recovery-overview.md) yönetmek ve iş sürekliliği ve olağanüstü durum kurtarma (BCDR) amacıyla şirket içi makinelerin ve Azure Vm'lerinde olağanüstü durum kurtarma düzenlemek için hizmet sitesini de kullanabilirsiniz Kurtarma yönetmek için Azure Vm'leri için ikincil bir bölgeye taşıyın.       

Bu öğreticide Azure Vm'leri, Azure kamu ve Azure Site Recovery kullanarak genel bölgeler arasında taşımak gösterilmektedir. Aynı aynı coğrafi kümede olmayan bölge çiftleri arasında sanal makineleri taşımak için genişletilebilir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Önkoşulları doğrulama
> * Kaynak Vm'leri hazırlama
> * Hedef bölge hazırlama
> * Hedef bölge için veri kopyalama
> * Test yapılandırması
> * Taşımayı gerçekleştirmek
> * Kaynak bölgedeki kaynakları AT

> [!IMPORTANT]
> Bu öğreticide Azure Vm'leri tarafından normal olağanüstü durum kurtarma çözümü, Azure Vm'leri için desteklenmeyen bölge çiftleri veya Azure devlet kurumları ve genel bölgeler arasında taşımak gösterilmektedir. Kaynak ve hedef bölgeler çiftlerinizi olması durumunda, [desteklenen](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support), lütfen şuna bakın [belge](azure-to-azure-tutorial-migrate.md) taşımak için. Bir kullanılabilirlik bölgeye sabitlenmiş Vm'leri için farklı bir bölgede kümesindeki sanal makinelerin taşınmasında kullanılabilirliği geliştirmek için gereksinim ise Öğreticisine bakın [burada](move-azure-VMs-AVset-Azone.md).

> [!IMPORTANT]
> DR senaryosu için kritik olan aklınızda veri gecikme süresinin tutma çiftleri tanımlandığı gibi desteklenmeyen bölge çiftleri DR yapılandırmak için bu yöntemi kullanmak için önerilmez.

## <a name="verify-prerequisites"></a>Önkoşulları doğrulama

> [!NOTE]
> Anladığınızdan emin olun [mimarisini ve bileşenlerini](physical-azure-architecture.md) bu senaryo için. Bu mimari, Azure Vm'leri taşımak için kullanılacak **Vm'leri fiziksel sunucuları olarak davranılması tarafından**.

- Tüm bileşenler için [destek gereksinimlerini](vmware-physical-secondary-support-matrix.md) gözden geçirin.
- Çoğaltmak istediğiniz sunucuları ile uyumlu olduğundan emin [Azure VM gereksinimleri](vmware-physical-secondary-support-matrix.md#replicated-vm-support).
- Çoğaltmak istediğiniz her sunucuda Mobility hizmetinin otomatik olarak yüklemek için bir hesap hazırlama.

- Sonra Azure hedef bölgede hatalarını, doğrudan yük devretme Kaynak bölgesi dön gerçekleştiremeyeceğinizi unutmayın. Çoğaltmayı yeniden hedeflemek için yeniden ayarlamanız gerekir.

### <a name="verify-azure-account-permissions"></a>Azure hesap izinlerini doğrulama

Azure hesabınızı Vm'lerini azure'a çoğaltma için izinleri olduğundan emin olun.

- Gözden geçirme [izinleri](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) makinelerin Azure'a çoğaltılması gerekir.
- Doğrulama ve değiştirme [rol tabanlı erişim](../role-based-access-control/role-assignments-portal.md) izinleri. 

### <a name="set-up-an-azure-network"></a>Azure ağı ayarlama

Ayarlanmış bir hedef [Azure ağı](../virtual-network/quick-create-portal.md).

- Yük devretme sonrasında oluşturulan azure VM'ler bu ağda yerleştirilir.
- Ağın kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir


### <a name="set-up-an-azure-storage-account"></a>Azure depolama hesabı ayarlama

Ayarlanmış bir [Azure depolama hesabı](../storage/common/storage-quickstart-create-account.md).

- Site Recovery, şirket içi makinelerin Azure depolama alanına çoğaltır. Yük devretme gerçekleştikten sonra depolamadan azure sanal makineleri oluşturulur.
- Depolama hesabının, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.


## <a name="prepare-the-source-vms"></a>Kaynak Vm'leri hazırlama

### <a name="prepare-an-account-for-mobility-service-installation"></a>Bir hesabı Mobility hizmeti yüklemesi için hazırlama

Çoğaltmak istediğiniz her sunucuda Mobility hizmetinin yüklenmesi gerekir. Sunucu için çoğaltmayı etkinleştirdiğinizde site Recovery bu hizmeti otomatik olarak yükler. Otomatik olarak yüklemek için Site Recovery'nin sunucuya erişmek için kullanacağı bir hesap hazırlamanız gerekir.

- Bir etki alanı veya yerel hesap kullanabilirsiniz
- Windows yerel makinede uzak kullanıcı erişim denetimini devre dışı bir etki alanı hesabı kullanmıyorsanız VM'ler için. Kayıt altında Bunu yapmak için **hkey_local_machıne\software\microsoft\windows\currentversion\policies\system**, DWORD girişini ekleyin **LocalAccountTokenFilterPolicy**, 1 değerine sahip.
- CLI ayarını devre dışı bırakmak için kayıt defteri girdisini eklemek için şunu yazın:       ``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
- Linux için hesabın kaynak Linux sunucusunda kök olması gerekir.


## <a name="prepare-the-target-region"></a>Hedef bölge hazırlama

1. Azure aboneliğinizin, olağanüstü durum kurtarma için kullanılan hedef bölgede VM’ler oluşturmanıza izin verdiğinden emin olun. Gerekli kotayı sağlamak için desteğe başvurun.

2. Aboneliğinizin, kaynak VM’lerinize uygun boyutlardaki VM’leri desteklemek için yeterli kaynakları içerdiğinden emin olun. Hedef veri kopyalamak için Site Recovery kullanıyorsanız, aynı boyutta veya hedef sanal makine için olası en yakın boyutu seçer.

3. Kaynak ağ düzeninde tanımlanan her bileşeni için bir hedef kaynak oluşturduğundan emin olun. Bu olduğundan emin olun, hedef bölge için kesme sonrası önemlidir, sanal makinelerinizin tüm işlevleri ve kaynak olan özellikleri vardır.

    > [!NOTE]
    > Azure Site Recovery otomatik olarak bulur ve kaynak VM için çoğaltmayı etkinleştirin veya Ayrıca bir ağı önceden oluşturun ve VM için çoğaltmayı etkinleştirme kullanıcı akışında atama bir sanal ağ oluşturur. Ancak diğer kaynaklar için aşağıda belirtildiği gibi bunları hedef bölgede el ile oluşturmanız gerekir.

     En sık kullanılan ağ kaynakları, kaynak VM yapılandırmasına bağlı olarak ilgili oluşturmak için aşağıdaki belgelere bakın.

    - [Ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)
    - [Yük dengeleyiciler](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
    - [Genel IP](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
    
    Tüm diğer ağ bileşenleri için ağ ile başvurmak [belgeleri.](https://docs.microsoft.com/azure/#pivot=products&panel=network) 

4. El ile [üretim dışı ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/quick-create-portal) son kesme üzerinden hedef bölgeye gerçekleştirmeden önce yapılandırmayı test etmek isterseniz, hedef bölgede. Bu, üretim ile en az bir girişim oluşturacak ve önerilir.

## <a name="copy-data-to-the-target-region"></a>Hedef bölge için veri kopyalama
Aşağıdaki adımların Azure Site Recovery hedef bölgeye veri kopyalamak için nasıl kullanılacağını size yol gösterir.

### <a name="create-the-vault-in-any-region-except-the-source-region"></a>Kasayı, kaynak bölgesi dışında herhangi bir bölgede oluşturun.

1. [Azure Portal](https://portal.azure.com) > **Kurtarma Hizmetleri**’nde oturum açın.
2. Tıklayın **kaynak Oluştur** > **Yönetim Araçları** > **Backup ve Site Recovery**.
3. **Ad** bölümünde **ContosoVMVault** kolay adını belirtin. Birden fazla aboneliğiniz varsa bir. Abonelik, uygun olanı seçin.
4. Bir **ContosoRG** kaynak grubu oluşturun.
5. Bir Azure bölgesi belirtin. Desteklenen bölgeleri kontrol etmek için [Azure Site Recovery Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/) bölümündeki coğrafi kullanılabilirlik kısmına bakın.
6. Kurtarma Hizmetleri kasalarında tıklayın **genel bakış** > **ConsotoVMVault** > **+ Çoğalt**
7. Seçin **Azure'a** > **sanallaştırılmamış/diğer**.

### <a name="set-up-the-configuration-server-to-discover-vms"></a>Vm'leri bulmak için yapılandırma sunucusunu ayarlayın.


Yapılandırma sunucusu ayarlarsınız, kasaya kaydedin ve Vm'leri keşfedin.

1. Tıklayın **Site kurtarma** > **altyapıyı hazırlama** > **kaynak**.
2. Yapılandırma sunucusu yoksa, tıklayın **+ yapılandırma sunucusu**.
3. İçinde **Sunucusu Ekle**, kontrol **yapılandırma sunucusu** görünür **sunucu türü**.
4. Site Recovery birleşik Kurulumu yükleme dosyasını indirin.
5. Kasa kayıt anahtarını indirin. Birleşik Kurulumu çalıştırırken buna ihtiyacınız olur. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

   ![Kaynağı ayarlama](./media/physical-azure-disaster-recovery/source-environment.png)


### <a name="register-the-configuration-server-in-the-vault"></a>Yapılandırma sunucusunu kasaya kaydedin.

Başlamadan önce aşağıdakileri yapın: 

#### <a name="verify-time-accuracy"></a>Zaman hassasiyeti doğrulayın
Configuration server makinesinde sistem saatinin eşitlendiğinden emin olun bir [saat sunucusu](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Eşleşmelidir. 15 dakika önde ise veya arkasında kurulum başarısız olabilir.

#### <a name="verify-connectivity"></a>Bağlantısını doğrula
Makinenin ortamınıza bağlı olarak bu URL'lere erişebildiğinden emin olun: 

[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]  

IP adresi tabanlı güvenlik duvarı kurallarına yukarıda listelenen Azure URL'lerini tüm iletişim HTTPS (443) bağlantı noktası üzerinden izin vermelidir. Basitleştirmek ve IP aralıkları sınırlamak için URL filtreleme yapılması önerilir.

- **Ticari IP'ler** -izin [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653)ve HTTPS (443) bağlantı noktası. AAD, yedekleme, çoğaltma ve depolama URL'leri desteklemek için aboneliğinizin Azure bölgesi için IP adresi aralıklarına izin verin.  
- **Kamu IP'ler** -izin [Azure devlet kurumları veri merkezi IP aralıkları](https://www.microsoft.com/en-us/download/details.aspx?id=57063)ve HTTPS (443) bağlantı noktası (Virginia, Texas, Arizona ve Iowa) AAD, yedekleme, çoğaltma ve depolama URL'leri desteklemek için tüm USGov bölgeler için.  

#### <a name="run-setup"></a>Kurulumu çalıştırma
Birleşik Kurulum bir yerel yapılandırma sunucusunu yüklemek için yönetici olarak çalıştırın. Ayrıca işlem sunucusu ve ana hedef sunucusu yapılandırma sunucusunda varsayılan olarak yüklenir.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

Kayıt tamamlandıktan sonra yapılandırma sunucusu görüntülenen **ayarları** > **sunucuları** kasadaki sayfası.

### <a name="configure-target-settings-for-replication"></a>Çoğaltma için hedef ayarlarını yapılandırma

Hedef kaynaklarını seçin ve doğrulayın.

1. **Altyapıyı hazırlama** > **Hedef** seçeneklerine tıklayıp kullanmak istediğiniz Azure aboneliğini seçin.
2. Hedef dağıtım modelini belirtin.
3. Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.

   ![Hedef](./media/physical-azure-disaster-recovery/network-storage.png)


### <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

1. Yeni bir çoğaltma ilkesi oluşturmak için **Site Recovery altyapısı** > **Çoğaltma İlkeleri** > **+Çoğaltma İlkesi**’ne tıklayın.
2. **Çoğaltma ilkesi oluştur** bölümünde bir ilke adı belirtin.
3. **RPO eşiği** bölümünde kurtarma noktası hedefi (RPO) sınırını belirtin. Bu değer, ne sıklıkta belirtir veri kurtarma noktaları oluşturulur. Devamlı çoğaltma bu sınırı aşarsa bir uyarı oluşturulur.
4. **Kurtarma noktası bekletme** bölümünde, her kurtarma noktası için bekletme süresinin ne kadar olacağını (saat) belirtin. Çoğaltılan VM’ler bir aralıktaki herhangi bir noktaya kurtarılabilir. Premium depolama alanına çoğaltılan makineler için 24 saate, standart depolama için de 72 saate kadar bekletme desteklenir.
5. İçinde **uygulamayla tutarlı anlık görüntü sıklığı**, belirtin uygulamayla tutarlı anlık görüntüleri içeren kurtarma noktalarının ne sıklıkta (dakika cinsinden) oluşturulur. İlkeyi oluşturmak için **Tamam**’a tıklayın.

    ![Çoğaltma ilkesi](./media/physical-azure-disaster-recovery/replication-policy.png)


İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir. Eşleşen bir ilke, varsayılan olarak yeniden çalışma için otomatik oluşturulur. Örneğin, çoğaltma ilkesi **rep-policy** sonra bir yeniden çalışma ilkesi **rep-policy-failback** oluşturulur. Bu ilke Azure’dan bir yeniden çalışma başlatılana kadar kullanılmaz.

### <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

- Çoğaltma etkinleştirildiğinde site Recovery Mobility hizmetini yükler.
- Bir sunucu için çoğaltmayı etkinleştirdiğinizde, 15 dakika alabilir veya değişikliklerin etkili olması ve portalda görüntülenmesi daha uzun.

1. **Uygulama çoğaltma** > **Kaynak** seçeneğine tıklayın.
2. **Kaynak** bölümünde yapılandırma sunucusunu seçin.
3. İçinde **makine türü**seçin **fiziksel makineler**.
4. İşlem sunucusunu (yapılandırma sunucusu) seçin. Daha sonra, **Tamam**'a tıklayın.
5. İçinde **hedef**, abonelik ve yük devretme sonrasında Azure Vm'leri oluşturmak istediğiniz kaynak grubunu seçin. (Klasik veya kaynak yönetimi) Azure'da kullanmak istediğiniz dağıtım modelini seçin.
6. Verileri çoğaltmak için kullanmak istediğiniz Azure depolama hesabını seçin. 
7. Yük devretme sonrasında oluşturulan Azure VM'lerinin bağlanacağı Azure ağını ve alt ağını seçin.
8. Koruma için seçtiğiniz tüm makinelere ağ ayarını uygulamak için **Seçili makineler için şimdi yapılandır**’ı seçin. Makineler için Azure ağını ayrı ayrı seçmek için **Daha sonra yapılandır**'ı seçin. 
9. İçinde **fiziksel makineler**, tıklatıp **+ fiziksel makine**. Adı ve IP adresi belirtin. Çoğaltmak istediğiniz makinenin işletim sistemini seçin. Bulunan ve listelenen sunucuları için birkaç dakika sürer. 

   > [!WARNING]
   > Taşımak istediğiniz Azure VM'nin IP adresini girmeniz gerekir

10. İçinde **özellikleri** > **özelliklerini yapılandırmak**, Mobility hizmetini makineye otomatik olarak yüklemeniz için işlem sunucusu tarafından kullanılacak hesabı seçin.
11. **Çoğaltma ayarları** > **Çoğaltma ayarlarını yapılandırma** bölümünde doğru çoğaltma ilkesinin seçilip seçilmediğini doğrulayın. 
12. **Çoğaltmayı Etkinleştir**’e tıklayın. **Ayarlar** > **İşler** > **Site Recovery İşleri** bölümünden **Korumayı Etkinleştir** işinin ilerleme durumunu izleyebilirsiniz. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.


Eklediğiniz sunucuları izlemek için bunlar için son bulunma zamanını kontrol edebilirsiniz **Configuration Servers** > **Last Contact At**. Bir zamanlanmış bulma süresi beklenmeden makineler eklemek için yapılandırma sunucusunu vurgulayın (tıklamayın), tıklatıp **Yenile**.

## <a name="test-the-configuration"></a>Test yapılandırması


1. Kasası'na gidebilirsiniz **ayarları** > **çoğaltılan öğeler**, sanal makine için hedef bölgede taşımak için istediğinize tıklayın **+ yük devretme testi** simge.
2. **Yük Devretme Testi** bölümünde, yük devretmede kullanılması için bir kurtarma noktası seçin:

   - **En son işlenen**: VM'nin Site Recovery hizmeti tarafından işlenen en son kurtarma noktasına devreder. Zaman damgası gösterilir. Bu seçenekle veri işlemeye zaman harcanmadığından düşük RTO sağlanılır (Kurtarma Süresi Hedefi)
   - **Uygulamayla tutarlı olan sonuncu**: Bu seçenek tüm sanal makineler en son uygulamayla tutarlı kurtarma noktasına devreder. Zaman damgası gösterilir.
   - **Özel**: Herhangi bir kurtarma noktasını seçin.

3. ' % S'hedef Azure seçin yapılandırmasını test etmek için Azure Vm'lerini taşımak istediğiniz sanal ağ. 

   > [!IMPORTANT]
   > Test yük devretmesi ve değil çoğaltmayı etkinleştirdiğinizde ayarlanmış, Vm'leri sonunda taşımak istediğiniz üretim ağı için ayrı bir Azure VM ağını kullanmanızı öneririz.

4. Taşıma testi başlatmak için tıklatın **Tamam**. İlerleme durumunu izlemek için, VM’ye tıklayarak özelliklerini açın. Ya da kasa adı > **Ayarlar** > **İşler** > **Site Recovery işleri** bölümünde **Yük Devretme Testi** işine tıklayabilirsiniz.
5. Yük devretme bittikten sonra, çoğaltma Azure VM, Azure portalı > **Sanal Makineler** bölümünde görünür. VM’nin çalıştığından, uygun şekilde boyutlandırıldığından ve uygun ağa bağlı olduğundan emin olun.
6. Silmek istediğiniz taşıma testin bir parçası olarak oluşturulan VM, tıklayın **yük devretme testini Temizle** çoğaltılan öğe üzerinde. İçinde **notları**testiyle ilişkili gözlemlerinizi kaydetmek ve kaydedin.

## <a name="perform-the-move-to-the-target-region-and-confirm"></a>Taşımak için hedef bölgede gerçekleştirir ve onaylayın.

1. Kasası'na gidebilirsiniz **ayarları** > **çoğaltılan öğeler**, sanal makineye tıklayın ve ardından **yük devretme**.
2. **Yük devretme** bölümünde **En geç** seçeneğini belirleyin. 
3. **Yük devretmeyi başlatmadan önce makineyi kapatın** seçeneğini belirleyin. Site Recovery, yük devretmeyi tetiklemeden önce kaynak sanal makineyi kapatmaya çalışır. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz. 
4. İş tamamlandıktan sonra sanal Makinenin hedef Azure bölgeniz beklendiği gibi görüntülenip görüntülenmediğini denetleyin.
5. **Çoğaltılan öğeler** bölümünde VM’ye sağ tıklayıp **Yürüt**’e tıklayın. Bu, hedef bölge için taşıma işlemi tamamlanır. İşleme iş tamamlanana kadar bekleyin.

## <a name="discard-the-resource-in-the-source-region"></a>Kaynak bölgedeki kaynak atma 

- Sanal Makineye gidin.  Tıklayarak **çoğaltma devre dışı bırakma**.  Bu VM için veri kopyalama işlemini durdurur.  

   > [!IMPORTANT]
   > ASR çoğaltma için ücret önlemek için bu adımı gerçekleştirmek önemlidir.

Lütfen kaynak kaynaklardan herhangi birini yeniden kullanmak için herhangi bir plan varsa sonraki adım kümesini ile devam edin.

1. Adım 4'te bir parçası olarak kullanıma listelenen kaynak bölgedeki tüm ilgili ağ kaynakları silmek için devam [kaynak Vm'leri hazırlama](#prepare-the-source-vms) 
2. Karşılık gelen bir depolama hesabı kaynak bölgede silin.



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir Azure sanal makinesi farklı bir Azure bölgesine taşındı. Şimdi taşınan sanal makine için olağanüstü durum kurtarmayı yapılandırabilirsiniz.

> [!div class="nextstepaction"]
> [Geçişten sonra olağanüstü durum kurtarmayı ayarlama](azure-to-azure-quickstart.md)
