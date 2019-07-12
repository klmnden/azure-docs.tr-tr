---
title: Şirket içi fiziksel makineler veya sanal makineleri Azure geçişi sunucusu geçişi ile Azure'a geçiş | Microsoft Docs
description: Bu makalede, şirket içi fiziksel makineler veya sanal makineleri Azure geçişi sunucusu geçişi ile Azure'a geçiş açıklar.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/09/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 8dc3b485d83f7bc55ca913e2c706164d4953ee61
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809606"
---
# <a name="migrate-physical-or-virtualized-servers-to-azure"></a>Fiziksel veya sanallaştırılmış sunucuları Azure'a geçirme 

Bu makalede, fiziksel veya sanallaştırılmış sunucuları Azure'a geçirme işlemini göstermektedir. Aracı tabanlı çoğaltmayı kullanarak, fiziksel ve sanallaştırılmış sunucuların geçişi, Azure geçişi Server Geçiş Aracı sunar. Bu Aracı'nı kullanarak çok çeşitli makineleri Azure'a geçirebilirsiniz:

- Şirket içi fiziksel sunucuları geçirme.
- Vm'leri Xen, KVM gibi platformları tarafından sanallaştırılmış geçirin.
- Hyper-V veya VMware Vm'lerini geçirme. Herhangi bir nedenden dolayı Azure geçişi sunucusu geçişi için sunan standart geçiş akışı kullanmak zamanınız yoksa kullanışlıdır [Hyper-V](tutorial-migrate-hyper-v.md), [VMware aracısız](tutorial-migrate-vmware.md) geçiş veya [VMware Aracı tabanlı](tutorial-migrate-vmware-agent.md) geçiş.
- Özel bulutlarında çalışan Vm'leri geçirme.
- Amazon Web Services (AWS) gibi genel Bulutlar veya Google Cloud Platform (GCP) çalışan Vm'leri geçirme.


[Azure geçişi](migrate-services-overview.md) bulma, değerlendirme ve şirket içi uygulamalar ve iş yüklerinin geçişini izlemek ve VM bulut için merkezi bir nokta örnekler, Azure'a sağlar. Hub araçları Azure geçişi, değerlendirme ve geçiş, ek olarak üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri sağlar.


Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Azure, Azure geçişi Server Geçiş Aracı ile geçiş için hazırlayın.
> * Geçirmek istediğiniz makineler için gereksinimler kontrol edin ve bir makine için keşfetmek ve makineleri Azure'a geçirmek için kullanılan Azure geçişi çoğaltma Gereci hazırlayın.
> * Azure geçişi Server Geçiş Aracı, Azure geçişi hub'ında ekleyin.
> * Çoğaltma gereç ayarlayın.
> * Mobility hizmeti, geçirmek istediğiniz makinelerde yükleyin.
> * Çoğaltmayı etkinleştirin.
> * Her şeyin beklendiği gibi çalıştığından emin olmak için geçiş testi çalıştırma.
> * Çalıştıran bir Azure tam geçişi.

> [!NOTE]
> Öğreticiler hızlı bir kavram kanıtı ayarlayabilirsiniz bir senaryo için en basit dağıtım yolu gösterir. Öğreticiler, mümkün olduğunda varsayılan seçenekleri kullanın ve tüm olası ayarları ve yol gösterme. Ayrıntılı yönergeler için Azure geçişi için bilgi belgeleri gözden geçirin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.


## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce karşılamanız gereken ön koşullar şunlardır:

1. [Gözden geçirme](migrate-architecture.md) geçiş mimarisi.
2. İzinlerine sahip olduğunu Azure hesabınızı kullanarak sanal makine Katılımcısı rolü atanmış olduğundan emin olun:

    - Seçilen kaynak grubunda sanal makine oluşturma.
    - Seçilen sanal ağda sanal makine oluşturma.
    - Yazma için bir Azure yönetilen disk. 

3. [Bir Azure ağı ayarlama](../virtual-network/manage-virtual-network.md#create-a-virtual-network). Azure'a çoğalttığınızda, Azure Vm'leri oluşturulur ve geçişi belirlediğiniz bir Azure ağına katılmış.


## <a name="prepare-azure"></a>Azure’u hazırlama

Azure geçişi sunucusu geçişi ile geçirmeden önce Azure izinlerini ayarlayabilirsiniz.

- **Proje oluşturma**: Azure hesabınızı bir Azure geçişi projesi oluşturmak için izinler gerekiyor. 
- **Azure geçişi çoğaltma Gereci kaydetme**: Çoğaltma Gereci oluşturur ve Azure hesabınızda bir Azure Active Directory uygulaması kaydeder. Bu izinleri verin.
- **Anahtar kasası oluşturma**: Makineleri geçirmek için Azure geçişi bir Key Vault aboneliğinizdeki çoğaltma depolama hesabı erişim anahtarlarını yönetmek için kaynak grubu oluşturur. Kasa oluşturmak için Azure geçişi projesi içinde bulunduğu kaynak grubunun üzerinde rol atama izinleri gerekir. 


### <a name="assign-permissions-to-create-project"></a>Proje oluşturma izinleri atama

1. Azure portalında abonelik açın ve seçin **erişim denetimi (IAM)** .
2. İçinde **denetleyin erişim**, ilgili hesabını bulun ve izinleri görüntülemek için tıklayın.
3. Olmalıdır **katkıda bulunan** veya **sahibi** izinleri.
    - Ücretsiz Azure hesabı oluşturduysanız, aboneliğinizin sahibine demektir.
    - Abonelik sahibi değilseniz, rol atamak için sahibi ile çalışır.

### <a name="assign-permissions-to-register-the-replication-appliance"></a>Çoğaltma gereç kaydetmek için izinler atama

Aracı tabanlı geçiş için geçiş sunucusu geçişi oluşturma ve hesabınızı bir Azure AD uygulamasını kaydetme Azure izinlerini verin. Aşağıdaki yöntemlerden birini kullanarak izinleri atayabilirsiniz:

- Kiracı/genel yönetici, oluşturmak ve Azure AD uygulamaları kaydetmek için kiracıdaki kullanıcılar izin verebilirsiniz.
- Kiracı/genel yönetici hesabına (izinleri) Uygulama geliştirici rolünü atayabilirsiniz.

Bu, hatalarının ayıklanabileceğini belirtmekte yarar:

- Uygulamalar, abonelikte yukarıda açıklanan dışındaki herhangi bir erişim izinleri yok.
- Yeni bir çoğaltma Gereci kaydettiğinizde yalnızca bu izinleri gerekir. Çoğaltma gereç ayarlandıktan sonra izinleri kaldırabilir. 


#### <a name="grant-account-permissions"></a>Hesabı izinleri verme

Kiracı/genel yönetici gibi izinleri verebilir

1. Kiracı/genel yönetici Azure AD'de gidin **Azure Active Directory** > **kullanıcılar** > **kullanıcı ayarları**.
2. Yönetici ayarlamalısınız **uygulama kayıtları** için **Evet**.

    ![Azure AD izinleri](./media/tutorial-migrate-physical-virtual-machines/aad.png)

> [!NOTE]
> Bu hassas olmayan varsayılan ayardır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).

#### <a name="assign-application-developer-role"></a>Uygulama geliştiricisi rolü atama 

Kiracı/genel yönetici hesabınız için uygulama geliştiricisinin rol atayabilirsiniz. [Daha fazla bilgi edinin](https://docs.microsoft.comazure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).

## <a name="assign-permissions-to-create-key-vault"></a>Anahtar kasası oluşturmak için izinler atama

Azure geçişi projesi, şu şekilde bulunduğu kaynak grubu rol atama izinleri atayın:

1. Azure portalında kaynak grubunu seçtikten **erişim denetimi (IAM)** .
2. İçinde **denetleyin erişim**, ilgili hesabını bulun ve izinleri görüntülemek için tıklayın. Gereksinim duyduğunuz **sahibi** (veya **katkıda bulunan** ve **kullanıcı erişimi Yöneticisi**) izinleri.
3. Gerekli izinlere sahip değilseniz, bunları kaynak grubu sahibinden isteyin. 

## <a name="prepare-for-migration"></a>Geçiş için hazırlanma

### <a name="check-machine-requirements-for-migration"></a>Geçiş için makine gereksinimlerini denetleme

Makineleri Azure'a geçiş için gereksinimleriyle uyumlu olduğundan emin olun. 

> [!NOTE]
> Azure geçişi sunucusu geçişi ile geçiş aracı tabanlı özellikleri Azure Site Recovery hizmetinin temel alır. Bazı gereksinimler, Site Recovery belgeleri bağlayabilirsiniz.

1. VMware sunucusu gereksinimlerini [doğrulayın](migrate-support-matrix-vmware.md#agent-based-migration-vmware-server-requirements).
2. [Doğrulama](migrate-support-matrix-vmware.md#agent-based-migration-vmware-vm-requirements) VM geçiş için gereksinimleri destekler.
3. VM ayarlarını doğrulayın. Şirket içi Vm'leri Azure'a çoğaltma ile uyumlu olmalıdır [Azure VM gereksinimleri](migrate-support-matrix-vmware.md#azure-vm-requirements).


### <a name="prepare-a-machine-for-the-replication-appliance"></a>Bir makine için çoğaltma gereç hazırlama

Azure geçişi sunucusu geçişi, makineleri Azure'a çoğaltmak için bir çoğaltma Gereci kullanır. Aşağıdaki bileşenler çoğaltma gereç çalıştırır.

- **Yapılandırma sunucusu**: Yapılandırma sunucusu yerinde bileşenler ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir.
- **İşlem sunucusu**: İşlem sunucusu, çoğaltma ağ geçidi olarak davranır. Bu çoğaltma verilerini alıp; önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir ve Azure önbellek depolama hesabına gönderir. 

Başlamadan önce çoğaltma gereç barındırmak için bir Windows Server 2016 makinesine hazırlamanız gerekir. Makine uymanız [bu gereksinimleri](migrate-support-matrix-vmware.md#agent-based-migration-replication-appliance-requirements).


## <a name="add-the-azure-migrate-server-migration-tool"></a>Azure geçişi Server Geçiş Aracı ekleme

Azure geçişi projesini ayarlarsınız ve Azure geçişi Server Geçiş Aracı ekleyin.

1. Azure portalında > **tüm hizmetleri**, arama **Azure geçişi**.
2. Altında **Hizmetleri**seçin **Azure geçişi**.
3. İçinde **genel bakış**, tıklayın **değerlendirin ve sunucularını geçirme**.
4. Altında **keşfedin, değerlendirin ve sunucularını geçirme**, tıklayın **değerlendirin ve sunucularını geçirme**.

    ![Bulma ve değerlendirme sunucuları](./media/tutorial-migrate-physical-virtual-machines/assess-migrate.png)

5. İçinde **keşfedin, değerlendirin ve sunucularını geçirme**, tıklayın **ekleme Araçları**.
6. İçinde **geçiş projesi**, Azure aboneliğinizi seçin ve bir yoksa, bir kaynak grubu oluşturun.
7. İçinde **Project Details**, proje adı ve projeyi oluşturmak ve istediğiniz coğrafi belirtin **İleri**

    ![Bir Azure geçişi projesi oluşturun](./media/tutorial-migrate-physical-virtual-machines/migrate-project.png)

    Azure geçişi projesinde bu coğrafyalar hiçbirinde oluşturabilirsiniz.

    **Coğrafya** | **Bölge**
    --- | ---
    Asya | Güneydoğu Asya
    Avrupa | Kuzey Avrupa veya Batı Avrupa
    Amerika Birleşik Devletleri | Doğu ABD ve Batı Orta ABD

    Proje için belirtilen coğrafya yalnızca şirket içi VM’lerden toplanan meta verileri depolamak için kullanılır. Gerçek geçiş için herhangi bir hedef bölge seçebilirsiniz.
8. İçinde **Select Değerlendirme Aracı**seçin **şimdilik bir değerlendirme aracı eklemeyi atlamak mı** > **sonraki**.
9. İçinde **Select geçiş aracı**seçin **Azure geçişi: Sunucu geçiş** > **sonraki**.
10. İçinde **gözden + araçları Ekle**, ayarları gözden geçirin ve tıklayın **araçları ekleme**
11. Aracı'nı ekledikten sonra Azure geçişi projesinde görünür > **sunucuları** > **Geçiş Araçları**.

## <a name="set-up-the-replication-appliance"></a>Çoğaltma gerecini ayarlamak

Geçişin ilk adım, çoğaltma gerecini ayarlamak sağlamaktır. Gereç için yükleyici dosyasını indirin ve çalıştırın [hazırladığınız makine](#prepare-a-machine-for-the-replication-appliance). Gereç yükledikten sonra bunu Azure geçişi sunucusu geçişi ile kaydedin.


### <a name="download-the-replication-appliance-installer"></a>Çoğaltma Gereci yükleyiciyi indirin

1. Azure geçişi projesinde > **sunucuları**, ***Azure geçişi: Sunucu geçiş**, tıklayın **bulma**.

    ![VM'leri bulma](./media/tutorial-migrate-physical-virtual-machines/migrate-discover.png)

3. İçinde **makineleri keşfet** > **makineleriniz sanallaştırıldı mı?** , tıklayın **sanallaştırılmamış/diğer**.
4. İçinde **hedef bölge**, makineleri geçirmek istediğiniz Azure bölgesini seçin.
5. Seçin **geçiş için hedef bölgede bölge adı olduğundan emin olun**.
6. Tıklayın **kaynakları oluşturma**. Bu, arka planda bir Azure Site Recovery kasası oluşturur.
    - Azure geçişi sunucusu geçişi ile geçiş'kurmak zaten ayarını etkinleştirdiyseniz, kaynakları önceden ayarlanmış olduğundan hedef seçeneği yapılandırılamaz.
    - Bu düğmesine tıklandıktan sonra bu proje için hedef bölgeyi değiştiremezsiniz.
    - Sonraki tüm geçişler bu bölgeye ' dir.

7. İçinde **yeni çoğaltma Gereci yüklemek istiyor musunuz?** seçin **çoğaltma Gereci yükleme**.
9. İçinde **indirme ve yükleme çoğaltma Gereci yazılım**, gereç yükleyici ve kayıt anahtarını indirin. Gereç kaydetmek için anahtarı gerekir. Anahtarı, ancak İndirildikten sonra beş gün boyunca geçerlidir.

    ![Sağlayıcı indirin](media/tutorial-migrate-physical-virtual-machines/download-provider.png)

10. Gereç kurulum dosyası ve anahtar dosyası oluşturduğunuz gereç Windows Server 2016 makinesinde kopyalayın.
11. Sonraki yordamda açıklandığı gibi çoğaltma Gereci kurulum dosyasını çalıştırın.
12. Gereç ayarladıktan sonra yeniden başlatıldıktan sonra **makineleri keşfet**, yeni Gereci seçin **Configuration Server seçin**, tıklatıp **kayıt Sonlandır**. Sonlandırma kayıt birkaç çoğaltma gereç hazırlamak için son görevler gerçekleştirir.

    ![Kayıt Sonlandır](./media/tutorial-migrate-physical-virtual-machines/finalize-registration.png)

Bu, Azure geçişi sunucusu geçişi içinde bulunan makineler görünene kadar kayıt sonlandırılıyor sonra 15 dakika sürebilir. VM'ler bulunduktan gibi **bulunan sunucuları** sayısı yükseldiğinde.

![Bulunan sunucuları](./media/tutorial-migrate-physical-virtual-machines/discovered-servers.png)


## <a name="install-the-mobility-service"></a>Mobility hizmetini yükleme

Geçirmek istediğiniz makinelerde Mobility hizmeti aracısı yüklemeniz gerekir. Aracı yükleyici, çoğaltma gerecinde kullanılabilir. Doğru yükleyici bulun ve geçirmek istediğiniz her makinede aracıyı yükleyin. Bunu şu şekilde yapabilirsiniz:

1. Çoğaltma gerecine oturum açın.
2. Gidin **%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository**.
3. Yükleyici, makine işletim sistemi ve sürümü için bulun. Gözden geçirme [desteklenen işletim sistemleri](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines). 
4. Yükleyici dosyasını geçirmek istediğiniz bilgisayara kopyalayın.
5. Gereç dağıtıldığında, oluşturulan parola olduğundan emin olun.
    - Makine üzerinde geçici bir metin dosyasına dosya Store.
    - Parola Çoğaltma gereçte elde edebilirsiniz. Komut satırından çalıştırmak **C:\ProgramData\ASR\svsystems\bin\genpassphrase.exe - v** geçerli parolayı görüntülemek için.
    - Parolayı yeniden yok. Bu bağlantıyı keser ve çoğaltma gereç kaydettirmeyi gerekir.


### <a name="install-on-windows"></a>Windows’ta yükleme

1. Şu şekilde bir makinede yerel bir klasöre (örneğin, C:\Temp) Yükleyici dosyasının içeriğini ayıklayın:

    ```
    ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
    MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
    cd C:\Temp\Extracted
    ```
2. Mobility hizmeti yükleyicisinin çalıştırın:
    ```
   UnifiedAgent.exe /Role "MS" /Silent
    ```
3. Aracıyı çoğaltma gereciyle kaydedin:
    ```
    cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
    UnifiedAgentConfigurator.exe  /CSEndPoint <replication appliance IP address> /PassphraseFilePath <Passphrase File Path>
    ```

### <a name="install-on-linux"></a>Linux'ta yükleme

1. Makinede yerel bir klasöre (örneğin /tmp/MobSvcInstaller) Yükleyici tarball'a içeriğini gibi ayıklayın:
    ```
    mkdir /tmp/MobSvcInstaller
    tar -C /tmp/MobSvcInstaller -xvf <Installer tarball>
    cd /tmp/MobSvcInstaller
    ```
2. Yükleyici betiği çalıştırın:
    ```
    sudo ./install -r MS -q
    ```
3. Aracıyı çoğaltma gereciyle kaydedin:
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <replication appliance IP address> -P <Passphrase File Path>
    ```

## <a name="replicate-machines"></a>Makineleri çoğaltma

1. Azure geçişi projesinde > **sunucuları**, **Azure geçişi: Sunucu geçiş**, tıklayın **çoğaltmak**.

    ![Sanal makinelerini çoğaltma](./media/tutorial-migrate-physical-virtual-machines/select-replicate.png)

2. İçinde **çoğaltmak**, > **kaynağı ayarları** > **makineleriniz sanallaştırıldı mı?** seçin **Evet, VMware vSphere ile**.
3. İçinde **şirket içi cihazının**, ayarladığınız Azure geçişi gereç adını seçin.
4. İçinde **vCenter server**, Vm'leri yönetme vCenter sunucusunun veya sanal makinelerin barındırılır vSphere sunucusu adını belirtin.
5. İçinde **işlem sunucusu**, çoğaltma gereç adını seçin.
6. İçinde **Konuk kimlik**, Mobility hizmetinin gönderme yüklemesi için kullanılacak bir sanal makine yönetici hesabı belirtin. Herhangi bir işlevsiz hesap ekleyebilmek için Bu öğreticide Mobility hizmetini el ile yüklüyoruz. Ardından **sonraki: Sanal makineler**.

    ![Sanal makinelerini çoğaltma](./media/tutorial-migrate-physical-virtual-machines/source-settings.png)

7. İçinde **sanal makineler**, **bir değerlendirmeden geçiş ayarlarını içeri aktarma?** , varsayılan ayarı değiştirmeyin **Hayır, geçiş ayarları belirteceksiniz el ile**.
8. Geçirmek istediğiniz her sanal makine kontrol edin. Ardından **sonraki: Ayarları hedef**.

    ![Sanal makineleri seçin](./media/tutorial-migrate-physical-virtual-machines/select-vms.png)


9. İçinde **hedef ayarları**aboneliği seçin ve hedef bölge için geçirin ve geçiş sonrasında Azure Vm'lerini bulunacağı kaynak grubu belirtin.
10. İçinde **sanal ağ**, Azure Vm'lerini katıldığı geçişten sonra Azure VNet/alt seçin.
11. İçinde **Azure hibrit avantajı**:

    - Seçin **Hayır** Azure hibrit avantajı uygulamak istemiyorsanız. Ardından **İleri**'ye tıklayın.
    - Seçin **Evet** ile etkin Yazılım Güvencesi'ni veya Windows Server abonelikleri kapsamındaki Windows Server makineleri sahip ve geçiş yaptığınız makinelere avantajı uygulamak istediğiniz. Ardından **İleri**'ye tıklayın.

    ![Hedef ayarları](./media/tutorial-migrate-physical-virtual-machines/target-settings.png)

12. İçinde **işlem**, VM adı, boyutu, işletim sistemi disk türü ve kullanılabilirlik kümesi gözden geçirin. Vm'leri uygun olmalıdır [Azure gereksinimleri](migrate-support-matrix-vmware.md#azure-vm-requirements).

    - **VM boyutu**: Varsayılan olarak, Azure geçişi sunucusu geçişi, Azure aboneliğinde en yakın eşleşme dayalı bir boyutu seçer. Alternatif olarak, el ile boyutundaki çekme **Azure VM boyutu**. 
    - **İşletim sistemi diski**: VM için işletim sistemi (önyükleme) diskini belirtin. İşletim sistemi diski işletim sistemi önyükleme yükleyicisi ve yükleyici sahip bir disktir. 
    - **Kullanılabilirlik kümesi**: Sanal makine geçişten sonra bir Azure kullanılabilirlik gerekiyorsa, kümesi belirtin. Küme, geçiş için belirttiğiniz hedef kaynak grubunda olmalıdır.

    ![İşlem ayarları](./media/tutorial-migrate-physical-virtual-machines/compute-settings.png)

13. İçinde **diskleri**, VM diskleri Azure'a çoğaltılacak ve disk türünü (standart SSD/HDD veya premium yönetilen diskler) Azure'da seçin olup olmadığını belirtin. Ardından **İleri**'ye tıklayın.
    - Diskleri çoğaltmadan hariç tutabilirsiniz.
    - Diskleri hariç tutarsanız, geçişten sonra Azure sanal makinesinde mevcut olmaz. 

    ![Disk ayarları](./media/tutorial-migrate-physical-virtual-machines/disks.png)


14. İçinde **gözden geçirin ve çoğaltma başlangıç**, ayarları gözden geçirin ve tıklayın **çoğaltmak** sunucular için ilk çoğaltma başlatılamadı.

> [!NOTE]
> Çoğaltma ayarları çoğaltma işlemi başlamadan önce dilediğiniz zaman güncelleştirebilirsiniz **Yönet** > **makineleri çoğaltma**. Çoğaltma başladıktan sonra ayarları değiştirilemez.



## <a name="track-and-monitor"></a>İzleme ve izleme

- Tıkladığınızda **çoğaltmak** Başlat çoğaltma işi başlar. 
- Başlangıç çoğaltma işlemi başarıyla tamamlandığında, makineleri azure'a, ilk çoğaltma başlar.
- İlk çoğaltma sonlandırıldıktan sonra değişim çoğaltması başlar. Şirket içi diskler artımlı değişiklikler düzenli aralıklarla azure'da çoğaltma diskler çoğaltılır.


İş durumunu portal bildirimlerinden izleyebilirsiniz.

Tıklayarak, çoğaltma durumunu izleyebilirsiniz **sunucuları çoğaltılması** içinde **Azure geçişi: Sunucu geçiş**.
![İzleyici çoğaltma](./media/tutorial-migrate-physical-virtual-machines/replicating-servers.png)

## <a name="run-a-test-migration"></a>Geçiş testi çalıştırma


Değişim çoğaltması başladığında Azure tam geçişi çalıştırmadan önce VM'ler için bir geçiş testi çalıştırabilirsiniz. Bu geçiş yapmadan önce en az bir kez her makine için bunu yapmanızı öneririz.

- Test geçişi, geçiş, şirket içi makineleri etkilemeden beklendiği gibi çalıştığından denetimleri çalıştırılıyor kalması ve çoğaltmaya devam edin. 
- Test geçişi, çoğaltılan veriler (genellikle, Azure aboneliğinizde bir üretim dışı sanal ağa geçirme) kullanarak Azure VM oluşturarak geçiş benzetimini yapar.
- Azure VM çoğaltılmış test kullanma geçişi doğrulamak için uygulama testi gerçekleştirin ve tam geçişten önce tüm sorunları giderin.

Test geçişi şu şekilde yapabilirsiniz:


1. İçinde **geçiş hedefleri** > **sunucuları** > **Azure geçişi: Sunucu geçiş**, tıklayın **Test geçişi sunucuları**.

     ![Geçirilen sunucularını test etme](./media/tutorial-migrate-physical-virtual-machines/test-migrated-servers.png)

2. Test ve VM'ye sağ **Test geçirme**.

    ![Test geçişi](./media/tutorial-migrate-physical-virtual-machines/test-migrate.png)

3. İçinde **geçiş testi**, hangi Azure VM konumlandırılacağı geçişten sonra Azure sanal ağı seçin. Üretim dışı VNet kullanmanızı öneririz.
4. **Test geçiş** işi başlatır. Portal bildirimleri işi izleyin.
5. Geçiş tamamlandıktan sonra geçirilen Azure sanal Makinesinde görüntüleyin **sanal makineler** Azure portalında. Bir sonek makineyi adına sahip **-Test**.
6. Test tamamlandıktan sonra Azure VM'nin sağ **makineleri çoğaltma**, tıklatıp **test geçişi temiz**.

    ![Geçişi Temizle](./media/tutorial-migrate-physical-virtual-machines/clean-up.png)


## <a name="migrate-vms"></a>VM’leri geçirme

Test geçiş beklendiği gibi çalıştığını doğruladıktan sonra şirket içi makineleri geçirebilirsiniz.

1. Azure geçişi projesinde > **sunucuları** > **Azure geçişi: Sunucu geçiş**, tıklayın **sunucuları çoğaltılması**.

    ![Çoğaltma sunucuları](./media/tutorial-migrate-physical-virtual-machines/replicate-servers.png)

2. İçinde **makineleri çoğaltma**, VM'ye sağ tıklayın > **geçirme**.
3. İçinde **geçirme** > **sanal makineleri kapatmak ve veri kaybı olmadan planlanan bir geçiş gerçekleştirmek**seçin **Evet** > **Tamam** .
    - Varsayılan olarak Azure geçişi, şirket içi VM'yi kapatır ve son çoğaltma gerçekleştikten oluşan VM değişiklikleri eşitlemek için bir isteğe bağlı çoğaltma çalışır. Bu, veri kaybı olmadan sağlar.
    - Sanal makineyi istemiyorsanız seçin **yok**
4. VM için bir geçiş işi başlatır. Azure bildirimleri işi izleyin.
5. İş tamamlandıktan sonra görüntüleyebilir ve VM'den yönetme **sanal makineler** sayfası.

## <a name="complete-the-migration"></a>Geçişi tamamlama

1. Geçiş yaptıktan sonra VM'ye sağ tıklayın > **Durdur geçiş**. Bu şirket içi makine için çoğaltma durdurulur ve sanal makine için çoğaltma durumu bilgilerini temizler.
2. Azure VM yükleme [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) veya [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) geçirilen makinelerdeki aracı.
3. Veritabanı bağlantısı dizelerini ve web sunucusu yapılandırmalarını güncelleştirme gibi herhangi bir geçiş sonrası uygulama ayarı gerçekleştirin.
4. Geçirilen uygulamada son uygulama ve geçiş kabul testi gerçekleştirme işlemi şimdi Azure’da çalıştırılmaktadır.
5. Geçirilen Azure sanal makine örneği üzerinden trafiğe keser.
6. Yerel sanal makine envanterinizden şirket içi sanal makineleri kaldırın.
7. Yerel yedeklemelerden şirket içi sanal makineleri kaldırın.
8. Azure sanal makinelerinin yeni konumunu ve IP adresini göstermek için herhangi bir iç belgeyi güncelleştirin. 

## <a name="post-migration-best-practices"></a>Geçiş sonrası en iyi uygulamalar

- Daha fazla esneklik için:
    - Azure Backup hizmetini kullanarak Azure sanal makinelerini yedekleyip verileri güvende tutun. [Daha fazla bilgi edinin](../backup/quick-backup-vm-portal.md).
    - Site Recovery ile Azure sanal makinelerini ikincil bölgeye çoğaltarak iş yüklerinin çalışmaya devam etmesini ve sürekli kullanılabilir olmasını sağlayın. [Daha fazla bilgi edinin](../site-recovery/azure-to-azure-tutorial-enable-replication.md).
- Daha fazla güvenlik için:
    - Kilitleme ve gelen trafik ile erişimini [Azure Güvenlik Merkezi - tam zamanında Yönetim](https://docs.microsoft.com/azure/security-center/security-center-just-in-time).
    - [Ağ Güvenlik Grupları](https://docs.microsoft.com/azure/virtual-network/security-overview) ile ağ trafiğini yönetim uç noktaları ile kısıtlayın.
    - [Azure Disk Şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview)’ni dağıtarak disklerin güvenliğinin sağlanmasına yardımcı olun ve verileri hırsızlık ve yetkisiz erişime karşı koruyun.
    - [IaaS kaynaklarının güvenliğini sağlama](https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/) hakkında daha fazla bilgi edinin ve [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)’ni ziyaret edin.
- İzleme ve yönetim için:
    - [Azure Maliyet Yönetimi](https://docs.microsoft.com/azure/cost-management/overview)’ni dağıtarak kaynak kullanımını ve harcamayı izleyin.


## <a name="next-steps"></a>Sonraki adımlar

Araştırma [bulut geçişi Yolculuğunuzun](https://docs.microsoft.com/azure/architecture/cloud-adoption/getting-started/migrate) Azure bulut benimseme çerçevesi içinde.
