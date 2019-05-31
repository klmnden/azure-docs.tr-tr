---
title: Azure Stack Vm'lerini Azure Site Recovery kullanarak Azure'a çoğaltma | Microsoft Docs
description: Azure'da olağanüstü durum kurtarma için Azure Stack Vm'leri Azure Site Recovery hizmeti ile nasıl ayarlanacağını öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.topic: conceptual
ms.service: site-recovery
ms.date: 05/30/2019
ms.author: raynew
ms.openlocfilehash: 11d409f904c43c0df4bbbd44fdb24531f2f989f6
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399589"
---
# <a name="replicate-azure-stack-vms-to-azure"></a>Azure Stack sanal makinelerini Azure'a çoğaltma

Bu makalede, Azure, Azure Stack Vm'leri olağanüstü durum kurtarma ayarlama işlemini göstermektedir kullanarak [Azure Site Recovery hizmeti](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview).

Site Recovery, iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkı sağlar. VM iş yüklerinizi beklendiği zaman kullanılabilir durumda kalır ve beklenmeyen kesintiler hizmet sağlar.

- Site Recovery, düzenler ve Azure depolama için sanal makinelerin çoğaltma işlemlerini yönetir.
- Birincil sitenizde bir kesinti oluştuğunda, Azure'a yük devri için Site RECOVERY'yi kullanın.
- Yük devretmede, depolanan sanal makine verileri Azure Vm'leri oluşturulur ve kullanıcıların bu Azure Vm'lerinde çalışan iş yükleri erişmeye devam.
- Her şeyi yeniden çalışır olduğunda, birincil sitenize geri Azure Vm'leri başarısız ve Azure depolama için yeniden çoğaltmaya başlayın.


Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * **1. adım: Azure stack sanal makineleri çoğaltma için hazırlanamadı**. Vm'lere Site Recovery gereksinimlerine uygun ve Site Recovery Mobility hizmeti yüklemesi için hazırlama denetleyin. Bu hizmet, çoğaltmak istediğiniz her sanal makinede yüklü.
> * **2. adım: Bir kurtarma Hizmetleri kasası ayarlama**. Site Recovery için bir kasası ayarlama ve neleri çoğaltmak istediğinizi belirtin. Site Recovery bileşenleri ve eylemleri yapılandırılır ve Kasası'nda yönetilen.
> * **3. adım: Kaynak çoğaltma ortamını ayarlama**. Bir Site Recovery yapılandırma sunucusunu ayarlayın. Site Recovery tarafından gereken tüm bileşenleri çalıştıran tek bir Azure Stack sanal makine yapılandırma sunucusudur. Yapılandırma sunucusunu ayarladıktan sonra kasaya kaydedin.
> * **4. adım: Hedef çoğaltma ortamı ayarlama**. Azure hesabınızı ve Azure depolama hesabı ve kullanmak istediğiniz ağ seçin. Çoğaltma sırasında sanal makine verilerini Azure depolama alanına kopyalanır. Yük devretme sonrasında Azure Vm'lerini belirtilen ağa katılır.
> * **5. adım: Çoğaltmayı etkinleştirme**. Çoğaltma ayarlarını yapılandırma ve VM'ler için çoğaltmayı etkinleştirin. Çoğaltma etkinleştirildiğinde Mobility hizmeti VM üzerinde yüklenir. Site Recovery, sanal makinenin ilk çoğaltma gerçekleştirir ve ardından devam eden çoğaltma başlar.
> * **6. adım: Bir olağanüstü durum kurtarma tatbikatı çalıştırma**: Çoğaltma çalışır duruma geldikten sonra Yük devretme tatbikat çalıştırarak beklendiği gibi çalıştığından doğrulayın. Ayrıntıya başlatmak için Site Recovery'de yük devretme testi çalıştırın. Yük devretme testi üretim ortamınızı etkilemez.

Adımları ile tam olarak azure'a ve gerektiğinde bir tam yük devretme çalıştırabilirsiniz.

## <a name="architecture"></a>Mimari

![Mimari](./media/azure-stack-site-recovery/architecture.png)

**Konum** | **Bileşen** |**Ayrıntılar**
--- | --- | ---
**Yapılandırma sunucusu** | Tek bir Azure Stack VM üzerinde çalışır. | Her abonelikte bir VM yapılandırma sunucusunu ayarlayın. Bu VM, aşağıdaki Site Recovery bileşenlerini çalıştıran:<br/><br/> -Yapılandırma sunucusu: Şirket içi ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir. -İşlem sunucusu: Çoğaltma ağ geçidi olarak davranır. Bu çoğaltma verilerini alıp, önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir. ve Azure depolamaya gönderir.<br/><br/> Çoğaltmak istediğiniz Vm'leri aşağıda belirtilen sınırları aşarsanız, ayrı bir tek başına bir işlem sunucusu ayarlamadıysanız ayarlayabilirsiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-process-server-scale).
**Mobility hizmeti** | Çoğaltmak istediğiniz her sanal makinede yüklü. | Bu makaledeki adımlarda, çoğaltma etkinleştirildiğinde Mobility hizmeti VM üzerinde otomatik olarak yüklenir, böylece biz bir hesap hazırlama. Hizmetini otomatik olarak yüklemek istemiyorsanız, bir dizi kullanabileceğiniz diğer yöntemler vardır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/site-recovery/vmware-azure-install-mobility-service).
**Azure** | Azure'da bir kurtarma Hizmetleri kasası, bir depolama hesabı ve bir sanal ağa ihtiyacınız vardır. |  Çoğaltılan veriler depolama hesabında depolanır. Azure sanal makineler, yük devretme gerçekleştiğinde Azure ağına eklenir. 


Çoğaltma gibi çalışır:

1. Kasada çoğaltma kaynağını ve hedef yapılandırma sunucusunu ayarlar, bir çoğaltma ilkesi oluşturma ve çoğaltmayı etkinleştirme belirtin.
2. Mobility hizmetinin (göndererek yükleme kullandıysanız) makineye yüklenir ve makineler çoğaltma ilkesine uygun olarak çoğaltma başlayın.
3. Sunucu verilerini bir başlangıç kopyasını Azure depolamaya çoğaltılır.
4. Delta değişikliklerinin azure'a çoğaltılması ilk çoğaltma sonlandırıldıktan sonra başlar. Bir makine için izlenen değişiklikler bir .hrl dosyasında saklanır.
5. Yapılandırma sunucusu, Azure (HTTPS 443 giden bağlantı noktası) ile çoğaltma yönetimini düzenler.
6. İşlem Sunucusu Kaynak makinelerden gelen verileri alan, en iyi duruma getirir ve şifreler ve Azure depolamaya (443 giden bağlantı noktası) gönderir.
7. Çoğaltılan makinelerin yapılandırma sunucusuna (HTTPS 443 gelen, çoğaltma yönetimi için bağlantı noktası. iletişim Makineler çoğaltma verilerini işlem sunucusuna Gönder (HTTPS 9443 gelen - bağlantı noktası değiştirilebilir).
8. Trafik İnternet üzerinden Azure depolama genel uç noktalarına çoğaltılır. Alternatif olarak, Azure ExpressRoute genel eşlemesi kullanabilirsiniz. Trafiğin siteden siteye bir VPN aracılığıyla şirket içi bir siteden Azure’a çoğaltılması desteklenmez.

## <a name="prerequisites"></a>Önkoşullar

Bu senaryoyu ayarlamak için ihtiyacınız olanlar aşağıda verilmiştir.

**Gereksinim** | **Ayrıntılar**
--- | ---
**Azure abonelik hesabı** | Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.
**Azure hesabı izinleri** | Kullandığınız bir Azure hesabı için izinler gerekir:<br/><br/> -Bir kurtarma Hizmetleri kasası oluşturma<br/><br/> -Kaynak grubunu ve sanal ağ senaryosu için kullandığınız bir sanal makine oluşturma<br/><br/> -Belirttiğiniz depolama hesabına yazma<br/><br/> Aşağıdakilere dikkat edin:<br/><br/> -Bir hesap oluşturursanız, aboneliğinizin Yöneticisi olduğunuzu ve tüm eylemleri gerçekleştirebilir.<br/><br/> -Mevcut bir abonelik kullanıyorsanız ve Yönetici değilseniz, sahibi veya katkıda bulunan izinleri atamak için yöneticiyle birlikte çalışmanız gerekiyor.<br/><br/> -Daha ayrıntılı izinler gerekiyorsa, gözden [bu makalede](https://docs.microsoft.com/azure/site-recovery/site-recovery-role-based-linked-access-control). 
**Azure Stack VM** | Site Recovery yapılandırma sunucusu olarak dağıtılacak Kiracı aboneliği bir Azure Stack sanal ihtiyacınız vardır. 


### <a name="prerequisites-for-the-configuration-server"></a>Yapılandırma sunucusu önkoşulları

[!INCLUDE [site-recovery-config-server-reqs-physical](../../includes/site-recovery-config-server-reqs-physical.md)]


 
## <a name="step-1-prepare-azure-stack-vms"></a>1. adım: Azure Stack Vm'leri hazırlama

### <a name="verify-the-operating-system"></a>İşletim sistemini doğrulayın

VM'ler tabloda özetlenen işletim sistemlerinden birini çalıştırdığından emin olun.


**İşletim sistemi** | **Ayrıntılar**
--- | ---
**64 bit Windows** | Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 (SP1)
**CentOS** | 5.2 için 5.11, 6.1 için 6.9, 7.0 için 7.3
**Ubuntu** | 14.04 server LTS, 16.04 LTS server. Gözden geçirme [desteklenen çekirdekler](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#ubuntu-kernel-versions)

### <a name="prepare-for-mobility-service-installation"></a>Mobility hizmeti yüklemesi için hazırlama

Çoğaltmak istediğiniz her sanal makine, Mobility hizmetinin yüklü olması gerekir. Çoğaltma etkinleştirildiğinde hizmeti VM üzerinde otomatik olarak yüklemek işlem sunucusu için sırada VM ayarlarını doğrulayın.

#### <a name="windows-machines"></a>Windows makineler

- Bağlantı çoğaltmayı etkinleştirmek istediğiniz sanal makine ve işlem sunucusu çalıştıran makine arasında ağ (varsayılan olarak VM'nin yapılandırma sunucusu budur).
- Makinede çoğaltmayı etkinleştirin (etki alanı veya yerel) yönetim haklarına sahip bir hesabınız olmalıdır.
    - Site RECOVERY'yi ayarladığınızda, bu hesabı belirtmeniz gerekir. Daha sonra işlem sunucusu, çoğaltma etkinleştirildiğinde Mobility hizmetini yüklemek için bu hesabı kullanır.
    - Bu hesap göndererek yükleme ve Mobility hizmetini güncelleştirmek için Site Recovery tarafından yalnızca kullanılır.
    - Bir etki alanı hesabı kullanmıyorsanız, VM üzerindeki uzak kullanıcı erişim denetimini devre dışı bırakmanız gerekir:
        - Kayıt defterindeki DWORD değeri oluşturun **LocalAccountTokenFilterPolicy** hkey_local_machıne\software\microsoft\windows\currentversion\policies\system altında.
        - Değer 1 olarak ayarlayın.
        - Komut isteminde Bunu yapmak için aşağıdaki komutu yazın: **REG ADD hkey_local_machıne\software\microsoft\windows\currentversion\policies\system /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1**.
- Windows Güvenlik Duvarı'nda, çoğaltmak istediğiniz sanal makine, dosya ve yazıcı paylaşımı ve WMI izin verin.
    - Bunu yapmak için çalıştırın **wf.msc** Windows Güvenlik Duvarı konsolunu açın. Sağ tıklayın **gelen kuralları** > **yeni kural**. Seçin **önceden tanımlanmış**ve **dosya ve Yazıcı Paylaşımı** listeden. Sihirbazı tamamlayın; bağlantısına izin vermek için seçin > **son**.
    - Etki alanı bilgisayarları için bir GPO, bunu yapmak için kullanabilirsiniz.

    
#### <a name="linux-machines"></a>Linux makineleri

- Linux bilgisayar ile işlem sunucusu arasında ağ bağlantısı bulunduğundan emin olun.
- Makinede çoğaltmayı etkinleştirin, kaynak Linux sunucusunda kök kullanıcı olan bir hesap gerekir:
    - Site RECOVERY'yi ayarladığınızda, bu hesabı belirtmeniz gerekir. Daha sonra işlem sunucusu, çoğaltma etkinleştirildiğinde Mobility hizmetini yüklemek için bu hesabı kullanır.
    - Bu hesap göndererek yükleme ve Mobility hizmetini güncelleştirmek için Site Recovery tarafından yalnızca kullanılır.
- Kaynak Linux sunucusundaki /etc/hosts dosyasında, yerel ana bilgisayar adını tüm ağ bağdaştırıcıları ile ilişkili IP adreslerine eşleyen girişler olduğunu denetleyin.
- Çoğaltmak istediğiniz bilgisayara en son openssh, openssh-server, openssl paketlerini yükleyin.
- Secure Shell’in (SSH) etkin olduğundan ve bağlantı noktası 22’de çalıştırıldığından emin olun.
- SFTP alt sistemi ve parola ile kimlik doğrulamasını sshd_config dosyasında etkinleştirin:
    1. Bunu yapmak için kök olarak oturum açın.
    2. İle başlayan satırı bulun **PasswordAuthentication**, /etc/ssh/sshd_config dosyasında. Satırı açıklama durumundan çıkarın ve değerini **yes** (evet) olarak değiştirin.
    3. **Subsystem** ile başlayan satırı bulun ve açıklama durumundan çıkarın.

        ![Linux Mobility hizmeti](./media/azure-stack-site-recovery/linux-mobility.png)

    4. Sshd hizmetini yeniden başlatın.


### <a name="note-the-vm-private-ip-address"></a>VM'nin özel IP adresini not edin

Çoğaltmak istediğiniz her makine için IP adresini bulun:

1. Azure Stack portalında sanal makinede'a tıklayın.
2. Üzerinde **kaynak** menüsünü tıklatın **ağ arabirimleri**.
3. Özel IP adresini not edin.

    ![Özel IP adresi](./media/azure-stack-site-recovery/private-ip.png)


## <a name="step-2-create-a-vault-and-select-a-replication-goal"></a>2. adım: Kasa oluşturma ve çoğaltma hedefi seçme

1. Azure portalında **kaynak Oluştur** > **Yönetim Araçları** > **Backup ve Site Recovery**.
2. **Ad** alanına kasayı tanımlamak için kolay bir ad girin. 
3. İçinde **kaynak grubu**bir kaynak grubu seçin veya oluşturun. Kullandığımız **contosoRG**.
4. İçinde **konumu**, Azure bölgesini belirtin. **Batı Avrupa** kullanacağız.
5. Panodan kasaya hızlı şekilde erişmek için **Panoya sabitle** > **Oluştur**’u seçin.

   ![Yeni kasa oluştur](./media/azure-stack-site-recovery/new-vault-settings.png)

   Yeni kasa, **Pano** > **Tüm kaynaklar** bölümünde ve ana **Kurtarma Hizmetleri kasaları** sayfasında görüntülenir.

### <a name="select-a-replication-goal"></a>Çoğaltma hedefi seçme

1. İçinde **kurtarma Hizmetleri kasaları** > Kasa adı belirtin. Kullandığımız **ContosoVMVault**.
2. **Başlarken** bölümünde Site Recovery’yi seçin. Ardından **Altyapıyı Hazırla** seçeneğini belirleyin.
3. **Koruma hedefi** > **Makineleriniz nerede** bölümünde **Şirket içi** seçeneğini belirleyin.
4. **Makinelerinizi nereye çoğaltmak istiyorsunuz** bölümünde **Azure’a** seçeneğini belirleyin.
5. İçinde **makineleriniz sanallaştırıldı mı**seçin **sanallaştırılmamış/diğer**. Sonra **Tamam**’ı seçin.

    ![Koruma hedefi](./media/azure-stack-site-recovery/protection-goal.png)

## <a name="step-3-set-up-the-source-environment"></a>3. adım: Kaynak ortamı ayarlama

Yapılandırma sunucusu makinesini ayarlama, kasaya kaydedin ve çoğaltmak istediğiniz makineleri bulma.

1. **Altyapıyı Hazırlama** > **Kaynak** seçeneklerine tıklayın.
2. **Kaynağı hazırla** bölümünde **+ Yapılandırma Sunucusu**’na tıklayın.

    ![Kaynağı ayarlama](./media/azure-stack-site-recovery/plus-config-srv.png)

3. İçinde **Sunucusu Ekle**, kontrol **yapılandırma sunucusu** görünür **sunucu türü**.
5. Site Recovery birleşik Kurulumu yükleme dosyasını indirin.
6. Kasa kayıt anahtarını indir Birleşik Kurulum'u çalıştırdığınızda, kayıt anahtarı gerekir. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

    ![Kaynağı ayarlama](./media/azure-stack-site-recovery/set-source2.png)


### <a name="run-azure-site-recovery-unified-setup"></a>Çalıştırma Azure Site Recovery birleşik Kurulumu

Yükleme ve yapılandırma sunucusunu kaydetmek için yapılandırma sunucusu için kullanmak istediğiniz VM ile RDP bağlantısı yapmak ve birleşik Kurulum'u çalıştırın.

Başlamadan önce saatin olduğundan emin olun [saat sunucusuyla eşitlenmiş](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) başlamadan önce VM üzerinde. Yerel saat kapalı beş dakikadan fazla süredir yükleme başarısız olur.

Artık yapılandırma sunucusunu yükleyin:

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Yapılandırma sunucusu, ayrıca komut satırından yüklenebilir. [Daha fazla bilgi edinin](physical-manage-configuration-server.md#install-from-the-command-line).
> 
> Hesap adının portalda görünmesi 15 dakika veya daha fazla sürebilir. Hemen güncelleştirme yapmak için **Yapılandırma Sunucuları** > ***sunucu adı*** > **Sunucuyu Yenile** seçeneğini belirleyin.

## <a name="step-4-set-up-the-target-environment"></a>4. Adım: Hedef ortamı ayarlama

Hedef kaynaklarını seçin ve doğrulayın.

1. İçinde **altyapıyı hazırlama** > **hedef**, kullanmak istediğiniz Azure aboneliğini seçin.
2. Hedef dağıtım modelini belirtin.
3. Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler. Bunları bulmuyorsa, Sihirbazı tamamlamak için en az bir depolama hesabı ve sanal ağ oluşturmak gerekir.


## <a name="step-5-enable-replication"></a>5. Adım: Çoğaltmayı etkinleştirme

### <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

1. Tıklayın **altyapıyı hazırlama** > **çoğaltma ayarları**.
2. **Çoğaltma ilkesi oluştur** bölümünde bir ilke adı belirtin.
3. **RPO eşiği** bölümünde kurtarma noktası hedefi (RPO) sınırını belirtin.
    - Çoğaltılan veriler için kurtarma noktalarını uygun şekilde zaman kümesi oluşturulur.
    - Bu ayar, çoğaltma sürekli olduğu etkilemez. Oluşturulan olmadan bir kurtarma noktası eşiği sınırına ulaşıldığında yalnızca bir uyarı verir.
4. İçinde **kurtarma noktası bekletme**, ne kadar süreyle saklanacağını her kurtarma noktası belirtin. Çoğaltılan VM'ler, belirtilen zaman penceresinde herhangi bir noktaya kurtarılabilir.
5. İçinde **uygulamayla tutarlı anlık görüntü sıklığı**, ne sıklıkta belirtin uygulamayla tutarlı anlık görüntüler oluşturulur.

    - Uygulamayla tutarlı bir anlık görüntü, VM'nin içindeki uygulama verilerinin zaman içinde nokta anlık görüntüsüdür.
    - Birim Gölge Kopyası Hizmeti (VSS) anlık görüntü alınırken VM'deki uygulamalar tutarlı bir durumda olmasını sağlar.
6. İlkeyi oluşturmak için **Tamam**’ı seçin.


### <a name="confirm-deployment-planning"></a>Dağıtım planlamasını onaylama

Şu anda bu adımı atlayabilirsiniz. İçinde **dağıtımı planlama** açılan listeyi tıklatın **Evet, yaptım**.



### <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Tüm görevleri tamamladığınızdan emin olun [1. adım: Makinesini hazırlama](#step-1-prepare-azure-stack-vms). Ardından şekilde çoğaltmayı etkinleştirin:

1. **Uygulamayı çoğalt** > **Kaynak** seçeneğini belirleyin.
2. **Kaynak** bölümünde yapılandırma sunucusunu seçin.
3. İçinde **makine türü**seçin **fiziksel makineler**.
4. İşlem sunucusunu (yapılandırma sunucusu) seçin. Daha sonra, **Tamam**'a tıklayın.
5. İçinde **hedef**, abonelik ve yük devretme sonrasında Vm'leri oluşturmak istediğiniz kaynak grubunu seçin. Devredilen VM'ler için kullanmak istediğiniz dağıtım modelini seçin.
6. Çoğaltılan verileri depolamak istediğiniz Azure depolama hesabını seçin.
7. Yük devretme işleminden sonra oluşturulan Azure VM’lerin bağlandığı Azure ağını ve alt ağını seçin.
8. Koruma için seçtiğiniz tüm makinelere ağ ayarını uygulamak için **Seçili makineler için şimdi yapılandır**’ı seçin. Seçin **daha sonra yapılandırma** Azure ağı her makine için ayrı ayrı seçmek istiyorsanız.
9. İçinde **fiziksel makineler**, tıklayın **+ fiziksel makine**. Adı, IP adresi ve çoğaltmak istediğiniz her makinede işletim sistemi türü belirtin.

    - Makinenin iç IP adresi kullanın.
    - Genel IP adresi belirtirseniz, çoğaltma beklendiği gibi çalışmayabilir.

10. İçinde **özellikleri** > **özelliklerini yapılandırmak**, işlem sunucusu otomatik olarak Mobility hizmetini makineye yüklemek için kullanacağınız hesabı seçin.
11. İçinde **çoğaltma ayarları** > **çoğaltma ayarlarını yapılandırma**, doğru çoğaltma ilkesinin seçilip seçilmediğini kontrol edin.
12. **Çoğaltmayı Etkinleştir**’e tıklayın.
13. İlerleme durumunu izlemek **korumayı etkinleştir** işinin **ayarları** > **işleri** > **Site Recovery işleri**. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.

> [!NOTE]
> Bir VM için çoğaltma etkinleştirildiğinde Site Recovery, Mobility Hizmeti’ni yükler.
> 
> Değişikliklerin geçerli olması ve portalda görüntülenmesi 15 dakika veya daha uzun sürebilir.
> 
> Eklediğiniz VM’leri izlemek için **Configuration Servers** > **Last Contact At** bölümünde VM’lerin son bulunma zamanını kontrol edin. Zamanlanan bulma işlemini beklemeden VM’leri eklemek için yapılandırma sunucusunu vurgulayın (seçmeyin) ve **Yenile**’yi seçin.


## <a name="step-6-run-a-disaster-recovery-drill"></a>6. Adım: Olağanüstü durum kurtarma tatbikatı çalıştırma

Her şeyin beklendiği gibi çalıştığından emin olmak için Azure'a yük devretme testi çalıştırın. Bu yük devretme üretim ortamınızı etkilemez.

### <a name="verify-machine-properties"></a>Makine özelliklerini doğrulayın

Yük devretme testi çalıştırmadan önce makine özelliklerini doğrulayın ve ile uyumlu olduğundan emin olun [Azure gereksinimleri](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements). Görüntüleyebilir ve özelliklerini aşağıdaki gibi değiştirin:

1. **Korunan Öğeler**’de **Çoğaltılan Öğeler** > VM seçeneğine tıklayın.
2. **Çoğaltılan öğe** bölmesinde VM bilgileri ile sistem durumunun bir özeti ve kullanılabilen son kurtarma noktaları yer alır. Daha fazla ayrıntı görüntülemek için **Özellikler**’e tıklayın.
3. İçinde **işlem ve ağ**, ayarları gerektiği gibi değiştirin.

    - Değiştirebilirsiniz Azure VM adını, kaynak grubunu, hedef boyutunu, [kullanılabilirlik kümesi](../virtual-machines/windows/tutorial-availability-sets.md)ve yönetilen disk ayarlarını.
    - Ayrıca, görüntüleyebilir ve ağ ayarlarını değiştirin. Bunlar, Azure VM yük devretme ve VM'ye atanacak IP adresini sonra katıldığı ağ/alt ağ içerir.
1. İçinde **diskleri**, VM'deki işletim sistemi ve veri diskleri ile ilgili bilgileri görüntüleyebilir.
   

### <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Yük devretme testi çalıştırdığınızda şunlar olur:

1. Yük devretme için gerekli tüm koşulların karşılandığından emin olmak için bir önkoşul denetimi çalıştırılır.
2. Yük devretme, belirtilen kurtarma noktası kullanarak verileri işler:
    - **En son işlenen**: Makinenin Site Recovery tarafından işlenen en son kurtarma noktasına devreder. Zaman damgası gösterilir. Bu seçenekte, verileri işlemek için zaman harcanmadığından düşük bir RTO (kurtarma süresi hedefi) sağlanır.
    - **Uygulamayla tutarlı olan sonuncu**: En son uygulamayla tutarlı kurtarma noktası için makine devreder.
    - **Özel**: Yük devretme için kullanılan kurtarma noktasını seçin.

3. Bir Azure sanal makinesi, işlenen verileri kullanarak oluşturulur.
4. Yük devretme testi, otomatik olarak ayrıntıya sırasında oluşturulan Azure sanal makinelerini temizleyebilirsiniz.

Yük devretme testi için sanal makine aşağıdaki gibi çalıştırın:

1. **Ayarlar** > **Çoğaltılmış Öğeler** bölümünde, CM > **+Yük Devretme Testi**’ne tıklayın.
2. Bu kılavuz için kullanılacak seçeneğini belirleyeceğiz **en son işlenen** kurtarma noktası. 
3. İçinde **yük devretme testi**, hedef Azure ağını seçin.
4. Yük devretmeyi başlatmak için **Tamam**'a tıklayın.
5. Özelliklerini açmak için VM'ye tıklayarak ilerleme durumunu izleyin. Veya tıklayın **yük devretme testi** işinin *kasa adı* > **ayarları** > **işleri**  > **Site Recovery işleri**.
6. Yük devretme bittikten sonra, çoğaltma Azure VM, Azure portalı > **Sanal Makineler** bölümünde görünür. VM'nin uygun boyutta doğru ağ ve çalışan bağlı olup olmadığını denetleyin.
7. Şimdi Azure’da çoğaltılan sanal makineye bağlanabiliyor olmanız gerekir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/site-recovery/site-recovery-test-failover-to-azure#prepare-to-connect-to-azure-vms-after-failover).
8. Yük devretme testi sırasında oluşturulan Azure sanal makinelerini silmek için, VM’de **Yük devretme testini temizle**’ye tıklayın. İçinde **notları**, yük devretme testiyle ilişkili gözlemlerinizi kaydetmek.

## <a name="fail-over-and-fail-back"></a>Yük devretme ve ilk duruma döndürme

Çoğaltma işlemini ayarladığınız ve emin olmak için bir tatbikatı çalıştırma her şeyin çalıştığından sonra makineleri Azure'a gerektiği gibi devredebilir.

Yük devretme sonrasında, sonra azure'da makinesine bağlanmak isterseniz yük devretme çalıştırmadan önce [bağlanmak için hazırlık yapma](https://docs.microsoft.com/azure/site-recovery/site-recovery-test-failover-to-azure#prepare-to-connect-to-azure-vms-after-failover) başlamadan önce.

Ardından, bir yük devretme gibi çalıştırın:


1. İçinde **ayarları** > **çoğaltılan öğeler**, makineye tıklayın > **yük devretme**.
2. Kullanmak istediğiniz kurtarma noktasını seçin.
3. İçinde **yük devretme testi**, hedef Azure ağını seçin.
4. **Yük devretmeyi başlatmadan önce makineyi kapatın** seçeneğini belirleyin. Bu ayar, Site Recovery yük devretmeyi başlatmadan önce kaynak makineyi kapatmaya çalışır. Ancak kapatma başarısız olsa bile yük devretme devam eder. 
5. Yük devretmeyi başlatmak için **Tamam**'a tıklayın. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.
6. Yük devretme bittikten sonra, çoğaltma Azure VM, Azure portalı > **Sanal Makineler** bölümünde görünür. Yük devretmeden sonra bağlanmaya hazırsanız, VM'nin uygun boyutta doğru ağa bağlı ve çalıştırma olduğunu denetleyin.
7. VM doğruladıktan sonra tıklayın **işleme** yük devretmeyi tamamlamak için. Bu, tüm kullanılabilir kurtarma noktalarını siler.

> [!WARNING]
> Devam eden bir yük devretme işlemini iptal etmeyin: Yük devretme başlatılmadan önce VM çoğaltması durdurulur. Devam eden bir yük devretme işlemini iptal ederseniz yük devretme durdurulur, ancak VM yeniden çoğaltılmaz.


### <a name="fail-back-to-azure-stack"></a>Azure Stack'e başarısız

Birincil sitenizi yeniden çalışır olduğunda, Azure başka bir Azure Stack yeniden çalıştırabilirsiniz. Bunu yapmak için Azure VM VHD'yi indirin ve Azure Stack'e karşıya gerekir.

1. Azure VM'yi, böylece VHD indirilebilir kapatın. 
2. VHD indirmeye başlamak için Yükle [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/).
3. Sanal makine (VM adı kullanarak) Azure Portal'da gidin.
4. İçinde **diskleri**disk adına tıklayın ve ayarlarını toplayın.

    - Örneğin, VHD URİ'si testimizde kullanılan: https://502055westcentralus.blob.core.windows.net/wahv9b8d2ceb284fb59287/copied-3676553984.vhd giriş VHD indirmek için kullanılan parametreleri aşağıdaki almak için ayrılabilir.
        - Depolama hesabı: 502055westcentralus
        - Kapsayıcı: wahv9b8d2ceb284fb59287
        - VHD adı: kopyalanan-3676553984.vhd

5. Şimdi, VHD indirmek için Azure Depolama Gezgini'ni kullanırsınız.
6. Azure Stack ile VHD'nizi karşıya yüklemeyi [adımları](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-manage-vm-disks#use-powershell-to-add-multiple-unmanaged-disks-to-a-vm).
7. İçinde varolan bir VM'yi veya yeni bir VM ekleme karşıya yüklenen VHD.
8. İşletim sistemi diskini doğru olduğundan emin olun ve VM'yi başlatın.


Bu aşamada yeniden çalışma işlemi tamamlanmış olur.


## <a name="conclusion"></a>Sonuç

Bu makalede size Azure Stack Vm'leri Azure'a çoğaltılır. Bir yerde daha fazla çoğaltma ile azure'a yük devretme beklendiği gibi çalıştığını emin olmak için bir olağanüstü durum kurtarma tatbikatı karşılaştık. Makale, bir tam yük devretme çalıştırılan ve Azure Stack'e başarısız adımları da dahil.

## <a name="next-steps"></a>Sonraki adımlar

İlk duruma döndürmeden sonra VM yeniden koruma ve yeniden Bunu yapmak için bu makaledeki adımları tekrarlayın azure'a çoğaltmaya başlayın.

