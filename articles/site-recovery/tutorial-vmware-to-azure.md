---
title: "Azure Site Recovery ile şirket içi VMware VM'ler için olağanüstü durum kurtarma Azure ayarlama | Microsoft Docs"
description: "Olağanüstü durum kurtarma Azure Azure Site Recovery hizmeti ile şirket içi VMware Vm'leri için nasıl ayarlanacağını öğrenin."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/15/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 8acc8deff8b635c97e8722d65a728aebf0e49bb3
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Şirket içi VMware Vm'leri için olağanüstü durum kurtarma Azure ayarlama

Bu öğretici Windows çalıştıran Azure olağanüstü durum kurtarma şirket içi VMware Vm'leri için nasıl ayarlanacağını gösterir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Çoğaltma kaynağını ve hedef belirtin.
> * Şirket içi Site Recovery bileşenleri de dahil olmak üzere kaynak çoğaltma ortamı ve hedef çoğaltma ortamı ayarlayın.
> * Çoğaltma ilkesi oluşturma
> * Bir sanal makine için çoğaltmayı etkinleştirme

Bir dizi üçüncü öğreticide budur. Bu öğreticinin önceki eğitimlerine görevleri önceden tamamlamış varsayılır:

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
2. [Şirket içi VMware’leri hazırlama](tutorial-prepare-on-premises-vmware.md)

Başlamadan önce için yararlı [mimarisi gözden](concepts-vmware-to-azure-architecture.md) olağanüstü durum kurtarma senaryosuna.


## <a name="select-a-replication-goal"></a>Çoğaltma hedefi seçin

1. İçinde **kurtarma Hizmetleri kasaları**, kasa adını tıklatın **ContosoVMVault**.
2. İçinde **Başlarken**, Site Kurtarma'yı tıklatın. Ardından **altyapıyı hazırlama**.
3. İçinde **koruma hedefi** > **bulunan makinelerinizi nerede**seçin **şirket içi**.
4. İçinde ** istediğiniz makinelerinizi çoğaltmak seçin **için Azure**.
5. İçinde **sanallaştırılmış, makine**seçin **Evet, VMware vSphere hiper yönetici ile**. Daha sonra, **Tamam**'a tıklayın.

## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Kaynak ortamı ayarlamak için tek bir yüksek oranda kullanılabilir, ana bilgisayar şirket içi Site Recovery bileşenlerini şirket içi makineye gerekir. Yapılandırma sunucusu, işlem sunucusu ve ana hedef sunucusu bileşenleri içerir.

- Yapılandırma sunucusu yerinde bileşenler ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir.
- İşlem sunucusu çoğaltma ağ geçidi olarak davranır. Çoğaltma verilerini alıp bu verileri önbelleğe alma, sıkıştırma ve şifreleme işlemleriyle iyileştirir ve Azure depolama alanına gönderir. İşlem sunucusu çoğaltmak istediğiniz sanal makinelerin de Mobility hizmeti yükler ve şirket içi VMware vm'lerinin otomatik bulma işlemini gerçekleştirir.
- Ana hedef sunucusu azure'dan yeniden çalışma sırasında çoğaltma verilerini işler.

Yapılandırma sunucusu yüksek oranda kullanılabilir bir VMware VM olarak ayarlamak için hazırlanmış bir OVF şablonu indirin ve şablonu oluşturmak için VMware içeri aktarın. Yapılandırma sunucusunu ayarlamayı ayarladıktan sonra bu kasaya kaydedin. Kayıttan sonra şirket içi VMware sanal makinelerini Site Recovery bulur.

### <a name="download-the-vm-template"></a>VM şablonunu yükle

1. Kasasına gidin **altyapıyı hazırlama** > **kaynak**.
2. İçinde **kaynağı**, tıklatın **+ yapılandırma sunucusu**.
3. İçinde **Sunucu Ekle**, denetleyin **VMware yapılandırma sunucusu** görünür **sunucu türü**.
4. Yapılandırma sunucusu için açık sanallaştırma biçimi (OVF) şablonu indirin.

  > [!TIP]
  Yapılandırma sunucusu şablonu en son sürümünü doğrudan indirilebilir [Microsoft Download Center](https://aka.ms/asrconfigurationserver).

## <a name="import-the-template-in-vmware"></a>VMware şablonunda alma

1. VMWare vSphere istemci kullanarak VMware vCenter server veya vSphere ESXi konağına, oturum açın.
2. Üzerinde **dosya** menüsünde, select **OVF şablon dağıtma**, OVF şablon Dağıtma Sihirbazı'nı başlatmak için.  

     ![OVF şablonu](./media/tutorial-vmware-to-azure/vcenter-wizard.png)

3. İçinde **kaynak seçme**, indirilen OVF konumunu belirtin.
4. İçinde **ayrıntıları inceleyin**, tıklatın **sonraki**.
5. İçinde **seçin adı ve klasör**, ve **Select configuration**, varsayılan ayarları kabul edin.
6. İçinde **seçin depolama**, en iyi performansı seçmek için **kalın sağlama istekli Sıfırlı** içinde **Select sanal disk biçimi**.
4. Sihirbazın sonraki sayfalarında kalan varsayılan ayarları kabul edin.
5. İçinde **tamamlamak hazır**:
  - Varsayılan ayarlarla VM ayarlamak için seçin **dağıtımdan sonra güç üzerinde** > **son**.
  - Ağ arabirimi eklemek istiyorsanız, temizleyin **dağıtımdan sonra güç üzerinde**ve ardından **son**. Varsayılan olarak, yapılandırma sunucusu şablonu tek bir NIC ile dağıtılır, ancak ek NIC'lerin dağıtımdan sonra ekleyebilirsiniz.

  
## <a name="add-an-additional-adapter"></a>Ek bir bağdaştırıcı Ekle

Ek bir NIC yapılandırma sunucusuna eklemek istiyorsanız, sunucuyu kasaya kaydetmek önce bunu yapar. Ek bağdaştırıcılar ekleme kayıttan sonra desteklenmiyor.

1. VSphere istemci envanteri VM'ye sağ tıklayın ve seçin **ayarlarını Düzenle**.
2. İçinde **donanım**, tıklatın **Ekle** > **Ethernet bağdaştırıcısı**. Ardından **İleri**'ye tıklayın.
3. Seçin ve bağdaştırıcı türü ve bir ağ. 
4. VM açıldığında sanal NIC bağlanmak için **CONNECT'te güç**. Tıklatın **sonraki** > **son**ve ardından **Tamam**.


## <a name="register-the-configuration-server"></a>Yapılandırma sunucusuna kaydedin 

1. VMWare vSphere istemci konsolundan VM açın.
2. VM oluşturan bir Windows Server 2016 yükleme deneyimi önyüklenir. Lisans Sözleşmesi'ni kabul edin ve bir yönetici parolası belirtin.
3. Yükleme tamamlandıktan sonra VM yönetici olarak oturum açın.
4. Azure Site kurtarma yapılandırma aracı ilk kez oturum başlatır.
5. Site Recovery ile yapılandırma sunucusunu kaydetmek için kullanılan bir ad belirtin. Ardından **İleri**'ye tıklayın.
6. Aracı için Azure VM bağlanıp bağlanamadığını denetler. Bağlantı kurulduktan sonra tıklayın **oturum**, Azure aboneliğinizde oturum. Kimlik bilgileri, yapılandırma sunucusunu kaydetmek istediğiniz kasaya erişimi olmalıdır.
7. Aracı, bazı yapılandırma görevleri gerçekleştirir ve ardından yeniden başlatır.
8. Makine yeniden oturum açın. Yapılandırma sunucusu yönetim Sihirbazı'nı otomatik olarak başlatır.

### <a name="configure-settings-and-connect-to-vmware"></a>Ayarlarını yapılandırmak ve VMware bağlanın

1. Yapılandırma sunucusu yönetim Sihirbazı'nda > **Kurulum Bağlantı**, çoğaltma trafiğini alacak NIC'i seçin. Daha sonra **Kaydet**'e tıklayın. Yapılandırması tamamlandıktan sonra bu ayarı değiştiremezsiniz.
2. İçinde **seçin kurtarma Hizmetleri kasası**, Azure aboneliğiniz ile ilgili kaynak grubu seçin ve kasa.
3. İçinde **üçüncü taraf yazılımlarını yüklemek için**lisans agreeemtn kabul edin ve tıklayın **karşıdan yükleyip**, MySQL Server yüklemek için.
4. Tıklatın **VMware PowerLCI yükleme**. Bunu yapmadan önce tüm tarayıcı pencerelerini kapalı olduğundan emin olun. Ardından **devam et**
5. İçinde **doğrulama Gereci yapılandırma**, Önkoşullar doğrulandı devam etmeden önce.
6. İçinde **yapılandırma vCenter sunucusu/vSphere ESXi sunucusunda**, vCenter sunucusunun FQDN veya IP adresi belirtin veya vSphere ana bilgisayar, çoğaltmak istediğiniz hangi sanal makinelerin bulunduğu. Sunucunun dinlediği bağlantı noktasını ve kasa VMware sunucusu için kullanılacak bir kolay ad belirtin.
7. Yapılandırma sunucusu tarafından VMware sunucuya bağlanmak için kullanılacak kimlik bilgilerini belirtin. Site kurtarma, çoğaltma için kullanılabilir olan VMware VM'ler otomatik olarak bulmak üzere bu kimlik bilgilerini kullanır. Tıklatın **Ekle**ve ardından **devam**.
8. İçinde **sanal makine kimlik bilgileri yapılandırma**, kullanıcı adı ve çoğaltma etkin olduğunda Mobility hizmeti makinelerde otomatik olarak yüklemek için kullanılan parolayı belirtin. Windows makineler için hesap çoğaltmak istediğiniz makinelerde yerel yönetici ayrıcalıkları gerekiyor. Linux için kök hesabının ayrıntılarını verin.
9. Tıklatın **Finalize yapılandırma** kayıt işlemini tamamlamak için. 
10. Kayıt tamamlandığında Azure portalında VMware sunucusu ve yapılandırma sunucusu üzerinde listelendiğini doğrulayın **kaynak** kasası sayfasında. Ardından **Tamam** hedef ayarlarını yapılandırmak için.


Site Recovery belirtilen ayarları kullanarak VMware sunucularına bağlanır ve sanal makineleri bulur.

> [!NOTE]
> Hesap adının portalda görünmesi 15 dakika veya daha fazla sürebilir. Hemen güncelleştirmek için **Yapılandırma Sunucuları** > ***sunucu adı*** > **Sunucuyu Yenile**’ye tıklayın.

## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Seçin ve hedef kaynaklarını doğrulayın.

1. **Altyapıyı hazırlama** > **Hedef** seçeneklerine tıklayıp kullanmak istediğiniz Azure aboneliğini seçin.
2. Belirtin, hedef dağıtım modeli Resource Manager tabanlı veya Klasik olup.
3. Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.

   ![Hedef](./media/tutorial-vmware-to-azure/storage-network.png)

## <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

1. Açık [Azure portal](https://portal.azure.com) ve tıklayın **tüm kaynakları**.
2. Adlı kurtarma hizmeti kasaya tıklayın **ContosoVMVault**.
3. Bir çoğaltma ilkesi oluşturmak için tıklatın **Site Recovery altyapısı** > **çoğaltma ilkeleri** > **+ Çoğaltma İlkesi**.
4. İçinde **çoğaltma ilkesi oluşturun**, ilke adını belirtebilir **VMwareRepPolicy**.
5. İçinde **RPO eşik**, varsayılan değer 60 dakika kullanın. Bu değer ne sıklıkta tanımlar kurtarma noktaları oluşturulur. Sürekli çoğaltma bu sınırı aşarsa bir uyarı üretilir.
6. İçinde **kurtarma noktası bekletme**, varsayılan 24 saatlik ne kadar süreyle bekletme penceresi için her kurtarma noktası için kullanın. Bu öğretici için 72 saat seçin. Çoğaltılan VM'ler penceresinde herhangi bir noktaya kurtarılabilir.
7. İçinde **uygulamayla tutarlı anlık görüntü sıklığı**, uygulamayla tutarlı anlık görüntüleri oluşturulur sıklığı için varsayılan değer 60 dakika kullanın. Tıklatın **Tamam** bir ilke oluşturmak için.

   ![İlke](./media/tutorial-vmware-to-azure/replication-policy.png)

İlke yapılandırma sunucusuyla otomatik olarak ilişkilendirilir. Varsayılan olarak, eşleşen bir ilkesi yeniden çalışma için otomatik olarak oluşturulur. Örneğin, çoğaltma ilkesi ise **rep İlkesi** geri dönme ilkesini sonra **rep İlkesi yeniden**. Bir azure'dan başlatana kadar bu ilkeyi kullanılmaz.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Çoğaltma için bir VM etkinleştirildiğinde, site Recovery Mobility hizmetini yükler. Bu 15 dakika alabilir veya artık değişikliklerinin için efekt ve portalda görüntülenir.

Şekilde çoğaltmayı etkinleştirin:

1. Tıklatın **uygulama çoğaltma** > **kaynak**.
2. İçinde **kaynak**, yapılandırma sunucusu seçin.
3. İçinde **makine türü**seçin **sanal makineleri**.
4. İçinde **vCenter/vSphere hiper yönetici**vSphere ana yöneten vCenter sunucusu seçin veya konağı seçin.
5. İşlem Sunucusu (yapılandırma sunucusu) seçin. IThen tıklatın **Tamam**.
6. İçinde **hedef**, abonelik ve başarısız VM'ler üzerinde oluşturmak istediğiniz kaynak grubunu seçin. Devredilen sanal makineleri için Azure (Yönetim), Klasik veya resource kullanmak istediğiniz dağıtım modelini seçin.
7. Veri çoğaltmak için kullanmak istediğiniz Azure depolama hesabı seçin.
8. Yük devretme sonrasında oluşturulan Azure VM'lerinin bağlanacağı Azure ağını ve alt ağını seçin.
9. Koruma için seçtiğiniz tüm makinelere ağ ayarını uygulamak için **Seçili makineler için şimdi yapılandır**’ı seçin. Makineler için Azure ağını ayrı ayrı seçmek için **Daha sonra yapılandır**'ı seçin.
10. **Sanal Makineler** > **Sanal makine seçin** seçeneklerine tıklayın ve çoğaltmak istediğiniz makineleri seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. Daha sonra, **Tamam**'a tıklayın.
11. İçinde **özellikleri** > **özelliklerini yapılandırma**, işlem sunucusu tarafından otomatik olarak makinede Mobility hizmeti yüklemek için kullanılacak hesabı seçin.
12. İçinde **çoğaltma ayarları** > **çoğaltma ayarlarını yapılandırın**, doğru Çoğaltma İlkesi'nin seçili olduğunu doğrulayın.
13. Tıklatın **çoğaltmasını etkinleştir**.

İlerleme durumunu izleyebilirsiniz **korumayı etkinleştir** iş **ayarları** > **işleri** > **Site Recovery işleri**. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.

Eklediğiniz sanal makineleri izlemek için son keşfedilen zamanı Vm'lerde denetleyebilirsiniz **yapılandırma sunucuları**
> **Son ilgili kişi**. VM'ler için zamanlanmış bulma beklemeden eklemek için yapılandırma sunucusu vurgulayın (tıklatın yok), tıklatıp **yenileme**.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma tatbikatı çalıştırma](site-recovery-test-failover-to-azure.md)
