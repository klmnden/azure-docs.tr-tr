---
title: Geçiş şirket içi VMware Vm'lerini Azure geçişi Server Geçiş Aracı tabanlı ile azure'a | Microsoft Docs
description: Bu makalede şirket içi makinelerin Azure geçişi sunucusu geçişi ile azure'a aracı tabanlı bir geçiş gerçekleştirme
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/08/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: fe83cd5f38e8c091ee72875da370b6929a99b727
ms.sourcegitcommit: 470041c681719df2d4ee9b81c9be6104befffcea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67854150"
---
# <a name="migrate-vmware-vms-to-azure-agent-based"></a>VMware Vm'lerini Azure'a (aracı tabanlı) geçirme

Bu makalede Azure geçişi Server Geçiş Aracı ile aracı tabanlı geçiş kullanarak şirket içi VMware Vm'leri Azure'a geçirme gösterilmektedir.

[Azure geçişi](migrate-services-overview.md) bulma, değerlendirme ve şirket içi uygulamalarınızı ve iş yükleri ve AWS/GCP VM örneklerini azure'a geçişini izlemek için merkezi bir nokta sağlar. Hub araçları Azure geçişi, değerlendirme ve geçiş, ek olarak üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri sağlar.


Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Kaynak ortamı ayarlamak ve aracı tabanlı geçiş için bir Azure geçişi çoğaltma Gereci dağıtabilirsiniz.
> * Geçiş için hedef ortamı ayarlama.
> * Bir çoğaltma ilkesi ayarlayın.
> * Çoğaltmayı etkinleştirin.
> * Her şeyin beklendiği gibi çalıştığından emin olmak için geçiş testi çalıştırma.
> * Azure için tam bir geçiş çalıştırın.

> [!NOTE]
> Öğreticiler hızlı bir kavram kanıtı ayarlayabilirsiniz bir senaryo için en basit dağıtım yolu gösterir. Öğreticiler, mümkün olduğunda varsayılan seçenekleri kullanın ve tüm olası ayarları ve yol gösterme. Ayrıntılı yönergeler için VMware değerlendirme ve geçiş için bilgi belgeleri gözden geçirin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

Vm'leri Azure'a geçiş yapmadan önce Azure geçişi Server değerlendirmesi ile VMware VM değerlendirmesi denemek öneririz. Bir değerlendirmenin aşağıdaki gibi ayarlayın:

1. İçin öğreticiyi izleyebilirsiniz [Azure ile VMware hazırlama](tutorial-prepare-vmware.md) değerlendirmesi için.
2. Daha sonra izleyin [Bu öğreticide](tutorial-assess-vmware.md) değerlendirmesi için bir Azure geçişi Gereci ayarlama keşfedin ve değerlendirin.


Bir değerlendirmeyi deneyin öneririz, ancak sanal makineleri geçirmeden önce bir değerlendirme çalıştırın gerekmez.

## <a name="migration-methods"></a>Geçiş yöntemi

VMware Vm'lerini Azure geçişi Server Geçiş Aracı kullanarak Azure'a geçirebilirsiniz. Bu araç, birkaç VMware VM geçişi için seçenek sunar:

- Aracısız çoğaltma. Herhangi bir şey yükler gerek kalmadan sanal makineleri geçirin.
- Aracı tabanlı geçiş. veya çoğaltma. VM çoğaltma için bir aracı (Mobility Hizmetleri Aracısı) yükleyin.

Aracısız veya aracı tabanlı geçişi kullanmak isteyip istemediğinizi karar vermek için bu makaleleri gözden geçirin:

- [Hakkında bilgi edinin](server-migrate-overview.md) VMware geçiş seçenekleri.
- [Sınırlamalarını gözden geçirme](server-migrate-overview.md#agentless-migration-limitations) aracısız geçiş.
- [Bu makaleyi takip](tutorial-migrate-vmware.md) aracısız geçiş denemek için.



## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce karşılamanız gereken ön koşullar şunlardır:

1. [Gözden geçirme](migrate-architecture.md) VMware geçiş mimarisi.
2. İzinlerine sahip olduğunu Azure hesabınızı kullanarak sanal makine Katılımcısı rolü atanmış olduğundan emin olun:

    - Seçilen kaynak grubunda sanal makine oluşturma.
    - Seçilen sanal ağda sanal makine oluşturma.
    - Yazma için bir Azure yönetilen disk. 

3. [Bir Azure ağı ayarlama](../virtual-network/manage-virtual-network.md#create-a-virtual-network). Şirket içinde makineleri Azure'a çoğaltılan yönetilen diskler. Azure'a geçiş için yük devretme, Azure Vm'leri yönetilen bu disklerden oluşturulan ve geçişi ayarlandığında belirlediğiniz bir Azure ağına katılmış.


## <a name="prepare-azure"></a>Azure’u hazırlama

Azure geçişi Server değerlendirmesi ile bir değerlendirme zaten çalıştırdıysanız, zaten bu adımları tamamladıktan sonra bu bölümdeki yönergeleri atlayabilirsiniz. 

Değerlendirme çalıştırmadıysanız Azure izinleri ayarlama, Azure geçişi sunucusu geçişi ile geçiş yapabilmeniz gerekir.

- **Proje oluşturma**: Azure hesabınızı bir Azure geçişi projesi oluşturmak için izinler gerekiyor. 
- **Azure geçişi çoğaltma Gereci kaydetme**: Çoğaltma Gereci oluşturur ve Azure hesabınızda bir Azure Active Directory uygulaması kaydeder. Bu izin için temsilci seçme gerekir.
- **Anahtar kasası oluşturma**: VMware Vm'lerini Azure geçişi sunucusu geçişi kullanan geçirmek için Azure geçişi bir Key Vault aboneliğinizdeki çoğaltma depolama hesabı erişim anahtarlarını yönetmek için kaynak grubu oluşturur. Kasa oluşturmak için Azure geçişi projesi içinde bulunduğu kaynak grubunun üzerinde rol atama izinleri gerekir. 


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

    ![Azure AD izinleri](./media/tutorial-prepare-vmware/aad.png)

> [!NOTE]
> Bu hassas olmayan varsayılan ayardır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).

#### <a name="assign-application-developer-role"></a>Uygulama geliştiricisi rolü atama 

Kiracı/genel yönetici hesabınız için uygulama geliştiricisinin rol atayabilirsiniz. [Daha fazla bilgi edinin](https://docs.microsoft.comazure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).

## <a name="assign-permissions-to-create-key-vault"></a>Anahtar kasası oluşturmak için izinler atama

Azure geçişi projesi, şu şekilde bulunduğu kaynak grubu rol atama izinleri atayın:

1. Azure portalında kaynak grubunu seçtikten **erişim denetimi (IAM)** .
2. İçinde **denetleyin erişim**, ilgili hesabını bulun ve izinleri görüntülemek için tıklayın. Gereksinim duyduğunuz **sahibi** (veya **katkıda bulunan** ve **kullanıcı erişimi Yöneticisi**) izinleri.
3. Gerekli izinlere sahip değilseniz, bunları kaynak grubu sahibinden isteyin. 


## <a name="prepare-on-premises-vmware"></a>Şirket içi VMware’leri hazırlama

### <a name="prepare-an-account-for-automatic-discovery"></a>Otomatik bulma için bir hesap hazırlama

Azure geçişi sunucusu geçişi VMware sunucularına erişmesi gerekir:

- VM'leri otomatik olarak bulma. En az bir salt okunur hesap gereklidir.
- Çoğaltma, yük devretme ve yeniden çalıştırmayı yönetme. Diskleri oluşturma ve kaldırma ve VM’leri çalıştırma gibi işlemleri gerçekleştirebilen bir hesabınızın olması gerekir.

Hesabı aşağıdaki gibi oluşturun:

1. Ayrılmış bir hesap kullanmak için, rolü vCenter düzeyinde oluşturun. Role **Azure_Site_Recovery** gibi bir ad verin.
2. Role aşağıdaki tabloda özetlenen izinleri atayın.
3. vCenter sunucusu veya vSphere ana bilgisayarında bir kullanıcı oluşturun. Rolü kullanıcıya atayın.

#### <a name="vmware-account-permissions"></a>VMware hesap izinleri

**Görev** | **Rol/İzinler** | **Ayrıntılar**
--- | --- | ---
**VM bulma** | En az bir salt okunur kullanıcı<br/><br/> Veri Merkezi nesnesi –> Alt Nesneye Yay, role=Read-only | Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.<br/><br/> Erişimi kısıtlamak için, **Alt öğeye yay** nesnesi ile **Erişim yok** rolünü alt nesnelere (vSphere ana bilgisayarları, veri depoları, VM’ler ve ağlar) atayın.
**Tam çoğaltma, yük devretme, yeniden çalışma** |  Gerekli izinlere sahip bir rol (Azure_Site_Recovery) oluşturup rolü VMware kullanıcısı veya grubuna atayın<br/><br/> Veri Merkezi nesnesi –> Alt Nesneye Yay, role=Azure_Site_Recovery<br/><br/> Veri deposu -> Alan ayırma, veri deposuna göz atma, düşük düzeyli dosya işlemleri, dosyayı kaldırma, sanal makine dosyalarını güncelleştirme<br/><br/> Ağ -> Ağ ataması<br/><br/> Kaynak -> VM’yi kaynak havuzuna atama, kapalı VM’yi geçirme, açık VM’yi geçirme<br/><br/> Görevler -> Görev oluşturma, görevi güncelleştirme<br/><br/> Sanal makine -> Yapılandırma<br/><br/> Sanal makine -> Etkileşim -> soruyu yanıtlama, cihaz bağlantısı, CD ortamını yapılandırma, disket ortamını yapılandırma, kapatma, açma, VMware araçlarını yükleme<br/><br/> Sanal makine -> Envanter -> Oluşturma, kaydetme, kaydı kaldırma<br/><br/> Sanal makine -> Sağlama -> Sanal makine indirmeye izin verme, Sanal makine dosyalarını karşıya yüklemeye izin verme<br/><br/> Sanal makine -> Anlık görüntüler -> Anlık görüntüleri kaldırma | Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.<br/><br/> Erişimi kısıtlamak için, **Alt öğeye yay** nesnesi ile **Erişim yok** rolünü alt nesnelere (vSphere ana bilgisayarları, veri depoları, VM’ler ve ağlar) atayın.

### <a name="prepare-an-account-for-mobility-service-installation"></a>Bir hesabı Mobility hizmeti yüklemesi için hazırlama

Çoğaltmak istediğiniz makinelerde Mobility hizmeti yüklü olmalıdır.

- Azure geçişi çoğaltma Gereci bu hizmetin göndererek yükleme yapabilirsiniz, bir makineye yönelik çoğaltmayı etkinleştirmek ya da el ile yükleyebilirsiniz veya Kurulum araçlarını kullanarak.
- Bu öğreticide, Mobility hizmetini göndererek yükleme yoluyla yükleyeceğiz.
- İçin göndererek yükleme, Azure geçişi sunucusu geçişi, sanal Makineye erişmek için kullanabileceğiniz bir hesap hazırlamanız gerekir.

Hesabı aşağıdaki gibi hazırlayın:

1. VM üzerinde yükleme izinleri ile bir etki alanı veya yerel hesap hazırlayın.
2. Windows DWORD girişini ekleyerek yerel makinede uzak kullanıcı erişim denetimini devre dışı bir etki alanı hesabı kullanmıyorsanız VM'ler için **LocalAccountTokenFilterPolicy**, kayıt defterinde değeriyle altında **HKEY_ LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**
3. Linux Vm'leri için kaynak Linux sunucusunda bir kök hesabı hazırlayın.


### <a name="check-vmware-requirements"></a>VMware gereksinimlerini denetleme

Emin VMware sunucularını ve Vm'leri Azure'a geçiş için gereksinimleriyle uyumlu hale getirir. 


> [!NOTE]
> Azure geçişi sunucusu geçişi ile geçiş aracı tabanlı özellikleri Azure Site Recovery hizmetinin temel alır. Bazı gereksinimler, Site Recovery belgeleri bağlayabilirsiniz.

1. VMware sunucusu gereksinimlerini [doğrulayın](migrate-support-matrix-vmware.md#agent-based-migration-vmware-server-requirements).
2. [Doğrulama](migrate-support-matrix-vmware.md#agent-based-migration-vmware-vm-requirements) VM geçiş için gereksinimleri destekler.
3. VM ayarlarını doğrulayın. Şirket içi Vm'leri Azure'a çoğaltma ile uyumlu olmalıdır [Azure VM gereksinimleri](migrate-support-matrix-vmware.md#azure-vm-requirements).



## <a name="add-the-azure-migrate-server-migration-tool"></a>Azure geçişi Server Geçiş Aracı ekleme

VMware Vm'lerini değerlendirmek için öğreticiyi izleyebilirsiniz alamadık, bir Azure geçişi projesi ayarlayın ve ardından Azure geçişi Server Geçiş Aracı ekleyin:

1. Azure portalında > **tüm hizmetleri**, arama **Azure geçişi**.
2. Altında **Hizmetleri**seçin **Azure geçişi**.

    ![Azure Geçişi ' ayarlayın](./media/tutorial-migrate-vmware-agent/azure-migrate-search.png)

3. İçinde **genel bakış**, tıklayın **değerlendirin ve sunucularını geçirme**.
4. Altında **keşfedin, değerlendirin ve sunucularını geçirme**, tıklayın **değerlendirin ve sunucularını geçirme**.

    ![Bulma ve değerlendirme sunucuları](./media/tutorial-migrate-vmware-agent/assess-migrate.png)

1. İçinde **keşfedin, değerlendirin ve sunucularını geçirme**, tıklayın **ekleme Araçları**.
2. İçinde **geçiş projesi**, Azure aboneliğinizi seçin ve bir yoksa, bir kaynak grubu oluşturun.
3. İçinde **Project Details**, proje adı ve projeyi oluşturmak ve istediğiniz coğrafi belirtin **İleri**

    ![Bir Azure geçişi projesi oluşturun](./media/tutorial-migrate-vmware-agent/migrate-project.png)

    Azure geçişi projesinde bu coğrafyalar hiçbirinde oluşturabilirsiniz.

    **Coğrafya** | **Bölge**
    --- | ---
    Asya | Güneydoğu Asya
    Avrupa | Kuzey Avrupa veya Batı Avrupa
    Amerika Birleşik Devletleri | Doğu ABD ve Batı Orta ABD

    Proje için belirtilen coğrafya yalnızca şirket içi VM’lerden toplanan meta verileri depolamak için kullanılır. Gerçek geçiş için herhangi bir hedef bölge seçebilirsiniz.
4. İçinde **Select Değerlendirme Aracı**seçin **şimdilik bir değerlendirme aracı eklemeyi atlamak mı** > **sonraki**.
5. İçinde **Select geçiş aracı**seçin **Azure geçişi: Sunucu geçiş** > **sonraki**.
6. İçinde **gözden + araçları Ekle**, ayarları gözden geçirin ve tıklayın **araçları ekleme**
7. Aracı'nı ekledikten sonra Azure geçişi projesinde görünür > **sunucuları** > **Geçiş Araçları**.

## <a name="set-up-the-replication-appliance"></a>Çoğaltma gerecini ayarlamak

Geçişin ilk adım, çoğaltma gerecini ayarlamak sağlamaktır. Çoğaltma gereç tek, yüksek oranda kullanılabilir, şirket içinde bu bileşenlerini barındıran bir VMware VM:

- **Yapılandırma sunucusu**: Yapılandırma sunucusu yerinde bileşenler ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir.
- **İşlem sunucusu**: İşlem sunucusu, çoğaltma ağ geçidi olarak davranır. Bu çoğaltma verilerini alıp; önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir ve Azure önbellek depolama hesabına gönderir. İşlem sunucusu, çoğaltmak istediğiniz Vm'lere Mobility hizmeti aracısı ayrıca yükler ve şirket içi VMware Vm'lerini otomatik olarak bulunmasını gerçekleştirir.


Çoğaltma gerecini ayarlamak için hazırlanmış bir Open Virtualization uygulaması (OVA) şablonu indirin. Şablonu Vmware'e aktarın ve çoğaltma Gereci VM oluşturun. 

### <a name="download-the-replication-appliance-template"></a>Çoğaltma Gereci şablonunu indirme

Şablon aşağıdaki gibi indirin:

1. Azure geçişi projesinde tıklayın **sunucuları** altında **geçiş hedefleri**.
2. İçinde **Azure geçişi - sunucuları** > **Azure geçişi: Sunucu geçiş**, tıklayın **bulma**.

    ![VM'leri bulma](./media/tutorial-migrate-vmware-agent/migrate-discover.png)

3. İçinde **makineleri keşfet** > **makineleriniz sanallaştırıldı mı?** , tıklayın **Evet, VMWare vSphere hiper Yöneticisi ile**.
4. İçinde **nasıl taşımak istiyorsunuz?** seçin **aracı tabanlı çoğaltmayı kullanarak**.
5. İçinde **hedef bölge**, makineleri geçirmek istediğiniz Azure bölgesini seçin.
6. Seçin **geçiş için hedef bölgede bölge adı olduğundan emin olun**.
7. Tıklayın **kaynakları oluşturma**. Bu, arka planda bir Azure Site Recovery kasası oluşturur.
    - Bu düğmesine tıklandıktan sonra bu proje için hedef bölgeyi değiştiremezsiniz.
    - Sonraki tüm geçişler bu bölgeye ' dir.

    ![Kurtarma Hizmetleri kasası oluşturma](./media/tutorial-migrate-vmware-agent/create-resources.png)

8. İçinde **yeni çoğaltma Gereci yüklemek istiyor musunuz?** seçin **çoğaltma Gereci yükleme**.
9. Tıklayın **indirme**, çoğaltma Gereci indirin. Bu gereç çalıştıran yeni bir VMware VM oluşturmak için kullandığınız bir OVF şablonu indirir.
    ![OVA indirin](./media/tutorial-migrate-vmware-agent/download-ova.png)
10. Kaynak grubunu ve kurtarma Hizmetleri kasası adını not edin. Gereç dağıtım sırasında ihtiyacınız var.


### <a name="import-the-template-in-vmware"></a>VMware’de şablonu içeri aktarma

OVF şablonu indirdikten sonra Windows Server 2016 çalıştıran bir VMware VM çoğaltma uygulaması oluşturmak için VMware içine aktarın.

1. VMWare vSphere İstemcisi ile VMware vCenter sunucusunda veya vSphere ESXi konağında oturum açın.
2. **Dosya** menüsünde **OVF Şablonunu Dağıt** seçeneğini belirleyerek **OVF Şablonu Dağıtma Sihirbazı**’nı başlatın. 
3. İçinde **Kaynak Seç**, indirilen OVF'nin konumunu girin.
4. İçinde **gözden geçirme ayrıntıları**seçin **sonraki**.
5. İçinde **ad ve klasör seçin** ve **yapılandırma seçin**, varsayılan ayarları kabul edin.
6. İçinde **seçin depolama** > **Select sanal disk biçimi**, en iyi performansı seçin **Thick Provision Eager Zeroed**.
7. Sihirbaz sayfalarının geri kalan kısmında varsayılan ayarları kabul edin.
8. İçinde **tamamlanmaya hazır**, VM seçme varsayılan ayarlarla kurmak **dağıtımdan sonra Aç** > **son**.

   > [!TIP]
   > Bir NIC daha eklemek istiyorsanız **Dağıtımdan sonra aç** > **Son** seçeneğinin işaretini kaldırın. Varsayılan olarak, şablon tek bir NIC içerir. Dağıtımdan sonra daha fazla NIC ekleyebilirsiniz.

### <a name="kick-off-replication-appliance-setup"></a>Devre dışı çoğaltma Gereci Kurulum kazandırın

1. VMWare vSphere Client konsolundan VM’yi açın.
2. VM’de Windows Server 2016 yükleme deneyimi önyüklemesi yapılır. Lisans sözleşmesini kabul edin ve bir yönetici parolası girin.
3. Yükleme tamamlandıktan sonra VM için yönetici parolasını kullanarak yönetici olarak oturum açın.
4. İlk kez oturum açarken, çoğaltma Gereci Kurulum aracı (Azure Site Recovery Configuration Tool) birkaç saniye içinde başlar.
5. Gerecin Azure geçişi sunucusu geçişi ile kaydetmek için kullanılacak bir ad girin. Ardından **İleri**'ye tıklayın.
6. Araç, VM’nin Azure bağlanıp bağlanamadığını denetler. Bağlantı kurulduktan sonra Azure aboneliğinizde oturum açmak için **Oturum aç** seçeneğini belirleyin.
7. Aracın Gereci tanımlamak için bir Azure AD uygulamasını kaydetme tamamlamak için bekleyin. Gereç yeniden başlatır.
1. Makinede tekrar oturum açın. Birkaç saniye içinde, Yapılandırma Sunucusu Yönetim Sihirbazı otomatik olarak başlar.

### <a name="register-the-replication-appliance"></a>Çoğaltma gereç kaydetme

Ayarlama ve kaydetme çoğaltma gereç tamamlayın.

1. Yapılandırma sunucusu yönetim Sihirbazı'nda seçin **bağlantı kurma**.
2. NIC'yi seçin (varsayılan olarak var olan tek bir NIC) çoğaltma gereç VM bulma için ve kaynak makinede Mobility hizmetinin göndererek yükleme yapmak için kullanır.
3. Azure ile bağlantı için çoğaltma gereç kullanan NIC'yi seçin. Daha sonra **Kaydet**’e tıklayın. Yapılandırıldıktan sonra bu ayarı değiştiremezsiniz.
4. Gereç bir proxy sunucusu arkasında bulunuyorsa, Ara sunucu ayarlarını belirtmeniz gerekir.
    - Ara sunucu adı olarak belirtmeniz **http://ip-address** , veya **http://FQDN** . HTTPS Ara sunucular desteklenmez.
5. Abonelik, kaynak grupları ve kasa ayrıntıları istendiğinde, gereç şablon zaman indirdiğiniz ettiğiniz ayrıntılarını ekleyin.
6. **Üçüncü taraf yazılımı yükleyin** bölümünde lisans sözleşmesini kabul edin. MySQL Server’ı yüklemek için **İndir ve Yükle** seçeneğini belirleyin.
7. **VMware PowerCLI’yi Yükle** seçeneğini belirleyin. Bunu yapmadan önce tüm tarayıcı pencerelerinin kapalı olduğundan emin olun. Daha sonra **Devam** seçeneğini belirleyin.
8. **Gereç yapılandırmasını doğrulama** bölümünde ön koşullar, siz devam etmeden önce doğrulanır.
9. **vCenter Server/vSphere ESXi sunucusu yapılandırma** bölümünde, çoğaltmak istediğiniz VM’lerin bulunduğu vCenter sunucusunun veya vSphere konağının FQDN’sini ya da IP adresini girin. Sunucunun dinleme gerçekleştirdiği bağlantı noktasını girin. Kasadaki VMware sunucusu için kullanılacak bir kolay ad girin.
10. Hesap için kimlik bilgilerini girin, [oluşturulan](#prepare-an-account-for-automatic-discovery) VMware bulma. Seçin **ekleme** > **devam**.
11. İçinde **sanal makine kimlik bilgilerini yapılandırma**, kimlik bilgilerini girin, [oluşturulan](#prepare-an-account-for-mobility-service-installation) VM'ler için çoğaltmayı etkinleştirirken Mobility hizmetinin göndererek yüklenmesine ilişkin için.  
    - Windows makinelerinde hesap için, çoğaltmak istediğiniz makinelerde yerel yönetici ayrıcalıkları gerekir.
    - Linux’ta kök hesap için bilgileri sağlayın.
12. Kaydı tamamlamak için **Yapılandırmayı son haline getir** seçeneğini belirleyin.


Çoğaltma gereç kaydedildikten sonra Azure geçişi Server değerlendirmesi belirtilen ayarları kullanarak VMware sunucularına bağlanır ve Vm'leri bulur. İçinde bulunan VM'ler görüntüleyebileceğiniz **Yönet** > **bulunan öğeleri**, **diğer** sekmesi.


## <a name="replicate-vms"></a>Sanal makinelerini çoğaltma

1. Azure geçişi projesinde > **sunucuları**, **Azure geçişi: Sunucu geçiş**, tıklayın **çoğaltmak**.

    ![Sanal makinelerini çoğaltma](./media/tutorial-migrate-vmware-agent/select-replicate.png)

2. İçinde **çoğaltmak**, > **kaynağı ayarları** > **makineleriniz sanallaştırıldı mı?** seçin **Evet, VMware vSphere ile**.
3. İçinde **şirket içi cihazının**, ayarladığınız Azure geçişi gereç adını seçin.
4. İçinde **vCenter server**, Vm'leri yönetme vCenter sunucusunun veya sanal makinelerin barındırılır vSphere sunucusu adını belirtin.
5. İçinde **işlem sunucusu**, çoğaltma gereç adını seçin.
6. İçinde **Konuk kimlik**, mobilite hizmetinin gönderme yüklemesi için kullanılacak VM yönetici hesabı belirtin. Ardından **sonraki: Sanal makineler**.

    ![Sanal makinelerini çoğaltma](./media/tutorial-migrate-vmware-agent/source-settings.png)

7. İçinde **sanal makineler**, çoğaltmak istediğiniz makineleri seçin.

    - VM'ler için bir değerlendirme çalıştırın, VM boyutu ve disk türü (premium veya standart) önerileri ve değerlendirme sonuçlarda uygulayabilirsiniz. Bunu yapmak için **Azure geçişi değerlendirmesi geçiş ayarlarını içeri aktarma?** seçin **Evet** seçeneği.
    - Değerlendirme çalışmadığı ya da seçin, değerlendirme ayarlarını kullanmak istemiyorsanız **Hayır** seçenekleri.
    - Değerlendirme kullanmayı seçtiyseniz, VM grubu ve değerlendirme adını seçin.

8. Geçirmek istediğiniz her sanal makine kontrol edin. Ardından **sonraki: Ayarları hedef**.
9. İçinde **hedef ayarları**aboneliği seçin ve hedef bölge için geçirin ve geçiş sonrasında Azure Vm'lerini bulunacağı kaynak grubu belirtin.
10. İçinde **sanal ağ**, Azure Vm'lerini katıldığı geçişten sonra Azure VNet/alt seçin.
11. İçinde **Azure hibrit avantajı**:

    - Seçin **Hayır** Azure hibrit avantajı uygulamak istemiyorsanız. Ardından **İleri**'ye tıklayın.
    - Seçin **Evet** ile etkin Yazılım Güvencesi'ni veya Windows Server abonelikleri kapsamındaki Windows Server makineleri sahip ve geçiş yaptığınız makinelere avantajı uygulamak istediğiniz. Ardından **İleri**'ye tıklayın.

12. İçinde **işlem**, VM adı, boyutu, işletim sistemi disk türü ve kullanılabilirlik kümesi gözden geçirin. Vm'leri uygun olmalıdır [Azure gereksinimleri](migrate-support-matrix-vmware.md#agentless-migration-vmware-vm-requirements).

    - **VM boyutu**: Değerlendirmesi önerileri kullanıyorsanız, VM boyutu açılan önerilen boyut içerir. Aksi takdirde Azure geçişi, Azure aboneliğinde en yakın eşleşme dayalı bir boyutu seçer. Alternatif olarak, el ile boyutundaki çekme **Azure VM boyutu**. 
    - **İşletim sistemi diski**: VM için işletim sistemi (önyükleme) diskini belirtin. İşletim sistemi diski işletim sistemi önyükleme yükleyicisi ve yükleyici sahip bir disktir. 
    - **Kullanılabilirlik kümesi**: Sanal makine geçişten sonra bir Azure kullanılabilirlik gerekiyorsa, kümesi belirtin. Küme, geçiş için belirttiğiniz hedef kaynak grubunda olmalıdır.

13. İçinde **diskleri**, VM diskleri Azure'a çoğaltılacak ve disk türünü (standart SSD/HDD veya premium yönetilen diskler) Azure'da seçin olup olmadığını belirtin. Ardından **İleri**'ye tıklayın.
    - Diskleri çoğaltmadan hariç tutabilirsiniz.
    - Diskleri hariç tutarsanız, geçişten sonra Azure sanal makinesinde mevcut olmaz. 

14. İçinde **gözden geçirin ve çoğaltma başlangıç**, ayarları gözden geçirin ve tıklayın **çoğaltmak** sunucular için ilk çoğaltma başlatılamadı.

> [!NOTE]
> Çoğaltma ayarları çoğaltma işlemi başlamadan önce dilediğiniz zaman güncelleştirebilirsiniz **Yönet** > **makineleri çoğaltma**. Çoğaltma başladıktan sonra ayarları değiştirilemez.




## <a name="track-and-monitor"></a>İzleme ve izleme

- Tıkladığınızda **çoğaltmak** Başlat çoğaltma işi başlar. 
- Başlangıç çoğaltma işlemi başarıyla tamamlandığında, makineleri azure'a, ilk çoğaltma başlar.
- İlk çoğaltma sonlandırıldıktan sonra değişim çoğaltması başlar. Şirket içi diskler artımlı değişiklikler düzenli aralıklarla azure'da çoğaltma diskler çoğaltılır.


İş durumunu portal bildirimlerinden izleyebilirsiniz.

![İzleme işi](./media/tutorial-migrate-vmware-agent/jobs.png)

Tıklayarak, çoğaltma durumunu izleyebilirsiniz **sunucuları çoğaltılması** içinde **Azure geçişi: Sunucu geçiş**.
![İzleyici çoğaltma](./media/tutorial-migrate-vmware-agent/replicate-servers.png)

## <a name="run-a-test-migration"></a>Geçiş testi çalıştırma


Değişim çoğaltması başladığında Azure tam geçişi çalıştırmadan önce VM'ler için bir geçiş testi çalıştırabilirsiniz. Bu geçiş yapmadan önce en az bir kez her makine için bunu yapmanızı öneririz.

- Test geçişi, geçiş, şirket içi makineleri etkilemeden beklendiği gibi çalıştığından denetimleri çalıştırılıyor kalması ve çoğaltmaya devam edin. 
- Test geçişi, çoğaltılan veriler (genellikle, Azure aboneliğinizde bir üretim dışı sanal ağa geçirme) kullanarak Azure VM oluşturarak geçiş benzetimini yapar.
- Azure VM çoğaltılmış test kullanma geçişi doğrulamak için uygulama testi gerçekleştirin ve tam geçişten önce tüm sorunları giderin.

Test geçişi şu şekilde yapabilirsiniz:


1. İçinde **geçiş hedefleri** > **sunucuları** > **Azure geçişi: Sunucu geçiş**, tıklayın **Test geçişi sunucuları**.

     ![Geçirilen sunucularını test etme](./media/tutorial-migrate-vmware-agent/test-migrated-servers.png)

2. Test ve VM'ye sağ **Test geçirme**.

    ![Test geçişi](./media/tutorial-migrate-vmware-agent/test-migrate.png)

3. İçinde **geçiş testi**, hangi Azure VM konumlandırılacağı geçişten sonra Azure sanal ağı seçin. Üretim dışı VNet kullanmanızı öneririz.
4. **Test geçiş** işi başlatır. Portal bildirimleri işi izleyin.
5. Geçiş tamamlandıktan sonra geçirilen Azure sanal Makinesinde görüntüleyin **sanal makineler** Azure portalında. Bir sonek makineyi adına sahip **-Test**.
6. Test tamamlandıktan sonra Azure VM'nin sağ **makineleri çoğaltma**, tıklatıp **test geçişi temiz**.

    ![Geçişi Temizle](./media/tutorial-migrate-vmware-agent/clean-up.png)


## <a name="migrate-vms"></a>VM’leri geçirme

Test geçiş beklendiği gibi çalıştığını doğruladıktan sonra şirket içi makineleri geçirebilirsiniz.

1. Azure geçişi projesinde > **sunucuları** > **Azure geçişi: Sunucu geçiş**, tıklayın **sunucuları çoğaltılması**.

    ![Çoğaltma sunucuları](./media/tutorial-migrate-vmware-agent/replicate-servers.png)

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
-  [Azure Maliyet Yönetimi](https://docs.microsoft.com/azure/cost-management/overview)’ni dağıtarak kaynak kullanımını ve harcamayı izleyin.


## <a name="next-steps"></a>Sonraki adımlar

Araştırma [bulut geçişi Yolculuğunuzun](https://docs.microsoft.com/azure/architecture/cloud-adoption/getting-started/migrate) Azure bulut benimseme çerçevesi içinde.
