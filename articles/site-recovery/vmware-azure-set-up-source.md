---
title: Kaynak ortamı ve yapılandırmayı YapılandırmaWindows olağanüstü durum kurtarma VMware vm'lerinin Azure Site Recovery ile azure'a ayarlama | Microsoft Docs
description: Bu makalede, şirket içi ortamda ve Azure'da Azure Site Recovery ile VMware vm'lerinin olağanüstü durum kurtarma için yapılandırma sunucusu nasıl ayarlanacağı açıklanır.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 10/29/2018
ms.author: ramamill
ms.openlocfilehash: e09e93ef85449c51e35b8da7ad93ee7088a0e7b4
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51566321"
---
# <a name="set-up-the-source-environment-and-configuration-server"></a>Kaynak ortamı ve yapılandırma sunucusunu ayarlama

Bu makalede, Azure ile VMware vm'lerinin olağanüstü durum kurtarma için kaynak şirket içi ortamınızı ayarlama işlemi açıklanmaktadır [Azure Site Recovery](site-recovery-overview.md). Bir şirket içi makine Site Recovery yapılandırma sunucusu olarak ayarlama adımlarını içerir ve otomatik olarak keşfetme şirket içi Vm'leri. 

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdakileri yaptığınızdan emin olun.

- Olağanüstü durum kurtarma dağıtım kullanarak planlı [Azure Site Recovery dağıtım Planlayıcısı](site-recovery-deployment-planner.md). Planlayıcıyı yeterli kaynaklara sahip ve olağanüstü durum kurtarma için bant genişliği ve Kurtarma hedefi (RPO) kuruluşunuzda noktası sağlamaya yardımcı olur.
- [Azure kaynaklarını ayarlama](tutorial-prepare-azure.md) içinde [Azure portalında](http://portal.azure.com).
- [Şirket içi VMware ayarlama](vmware-azure-tutorial-prepare-on-premises.md) şirket içi VMware vm'lerinin otomatik bulma için adanmış bir hesap gibi kaynakları.
- Yapabilecekleriniz [sık sorulan soruları gözden](vmware-azure-common-questions.md#configuration-server) yapılandırma sunucusu dağıtımı ve yönetimi hakkında.


## <a name="choose-protection-goals"></a>Koruma hedeflerini seçin

1. **Kurtarma Hizmetleri kasaları** bölümünde kasa adını seçin. 
2. **Başlarken** bölümünde Site Recovery’yi seçin. Ardından **Altyapıyı Hazırla** seçeneğini belirleyin.
3. **Koruma hedefi** > **Makineleriniz nerede** bölümünde **Şirket içi** seçeneğini belirleyin.
4. **Makinelerinizi nereye çoğaltmak istiyorsunuz** bölümünde **Azure’a** seçeneğini belirleyin.
5. **Makineleriniz sanallaştırıldı mı** bölümünde **Evet, VMware vSphere Hypervisor ile** seçeneğini belirleyin. Sonra **Tamam**’ı seçin.

## <a name="about-the-configuration-server"></a>Yapılandırma sunucusu hakkında

Azure'da VMware vm'lerinin olağanüstü durum kurtarmayı ayarlarken bir şirket içi yapılandırma sunucusu dağıtır.

- Yapılandırma sunucusu koordinatları iletişimleri arasında şirket içi VMware ve Azure. Veri çoğaltma işlemlerini yönetir.
- Yapılandırma sunucusunu bir VMware VM olarak dağıtma.
- Site Recovery, Azure portalından indirin ve vCenter sunucusuna yapılandırma sunucusu sanal makine kurmak için içe OVA şablonu sağlar.
- [Daha fazla bilgi edinin](vmware-azure-architecture.md) yapılandırma sunucusu bileşenleri ve süreçleri hakkında.


>[!NOTE]
Şirket içi fiziksel makinelerin olağanüstü durum kurtarması için yapılandırma sunucusunu Azure'a dağıtıyorsanız bu yönergeleri kullanmayın. Bu senaryo için izleyin [bu makalede](physical-azure-set-up-source.md).


## <a name="before-you-deploy-the-configuration-server"></a>Yapılandırma sunucusunu dağıtmadan önce

OVA şablonu kullanarak bir VMware VM yapılandırma sunucusu yüklerseniz, VM ile tüm Önkoşullar yüklenecek. Önkoşulları gözden geçirmek istiyorsanız, aşağıdaki makaleleri kullanın.

- [Hakkında bilgi edinin](vmware-azure-configuration-server-requirements.md) donanım, yazılım ve yapılandırma sunucusu için kapasite gereksinimleri.
- Birden çok VMware sanal makinelerini çoğaltıyorsanız, gözden geçirmeniz gereken [kapasite planlaması konuları](site-recovery-plan-capacity-vmware.md), çalıştırıp [Azure Site Recovery dağıtım Planlayıcısı](site-recovery-deployment-planner.md) VMWare çoğaltması için aracı.
- OVA şablon Windows lisansının 180 gün için geçerli olan bir değerlendirme lisanstır. Bu sürenin sonunda, geçerli bir lisans ile Windows etkinleştirmeniz gerekir. 
- OVA şablon yapılandırma sunucusunu bir VMware VM'si olarak ayarlamak için basit bir yol sağlar. VMware VM şablonu olmadan ayarlamanıza gerek herhangi bir nedenden dolayı ön koşulları gözden geçirin ve el ile yönergeleri [bu makalede](physical-azure-set-up-source.md).
- Azure hesabınızı kullanarak Azure AD uygulamaları oluşturmak için izinler gerekiyor.


>[!IMPORTANT]
Bu makalede açıklandığı gibi yapılandırma sunucusu dağıtılmalıdır. Kopyalama veya var olan bir yapılandırma sunucusu kopyalama desteklenmiyor.

### <a name="set-up-azure-account-permissions"></a>Azure hesabı izinleri ayarlama

Ya da bir kiracı/genel yönetici hesabı için Azure AD uygulamaları oluşturmak için izinler atayın veya hesabına (izinleri olan) uygulama geliştiricisi rolü atayın.


Kiracı/genel yönetici hesabı için izinler gibi verebilirsiniz:

1. Kiracı/genel yönetici Azure AD'de gidin **Azure Active Directory** > **kullanıcılar** > **kullanıcı ayarları**.
2. Yönetici ayarlamalısınız **uygulama kayıtları** için **Evet**.

 > [!NOTE]
 > Bu hassas olmayan varsayılan ayardır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).

Alternatif olarak Kiracı/genel yönetici hesabı rol atama izinlerine sahiptir. [Daha fazla bilgi edinin](https://docs.microsoft.comazure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).

 


## <a name="download-the-ova-template"></a>OVA şablonunu indirme

1. Kasada **Altyapıyı Hazırlama** > **Kaynak** seçeneğine gidin.
2. **Kaynağı hazırla** bölümünde **+Yapılandırma sunucusu**’nu seçin.
3. **Sunucu Ekle** bölümünde **Sunucu türü**’nde **VMware için yapılandırma sunucusu**’nun görüntülenip görüntülenmediğini kontrol edin.
4. Yapılandırma sunucusu için Open Virtualization uygulaması (OVA) şablonunu indirin.

  > [!TIP]
>Ayrıca, yapılandırma sunucusu şablonunun en son sürümünü indirebilirsiniz [Microsoft Download Center](https://aka.ms/asrconfigurationserver).


## <a name="import-the-ova-template-in-vmware"></a>OVA Vmware'de şablonu içeri aktarma

1. VMWare vSphere İstemcisi ile VMware vCenter sunucusunda veya vSphere ESXi konağında oturum açın.
2. **Dosya** menüsünde **OVF Şablonunu Dağıt** seçeneğini belirleyerek OVF Şablonu Dağıtma sihirbazını başlatın.

     ![OVF şablonu](./media/vmware-azure-deploy-configuration-server/vcenter-wizard.png)

3. İçinde **Kaynak Seç**, indirilen OVF'nin konumunu girin.
4. İçinde **gözden geçirme ayrıntıları**seçin **sonraki**.
5. İçinde **ad ve klasör seçin** ve **yapılandırma seçin**, varsayılan ayarları kabul edin.
6. **Depolama seçin** bölümünde en iyi performans için **Sanal disk biçimini seçin** bölümden **Thick Provision Eager Zeroed** seçeneğini belirleyin.
7. Kalan sihirbaz sayfalarında varsayılan ayarları kabul edin.
8. İçinde **tamamlanmaya hazır**, VM seçme varsayılan ayarlarla kurmak **dağıtımdan sonra Aç** > **son**.
9. Varsayılan olarak tek bir NIC ile VM dağıtılır Ek NIC eklemek istiyorsanız, Temizle **dağıtımdan sonra Aç**, tıklatıp **son**. Ardından bir sonraki yordamı izleyin.

## <a name="add-an-adapter-to-the-configuration-server"></a>Bir bağdaştırıcı yapılandırma sunucusuna Ekle

Yapılandırma sunucusuna Ek NIC eklemek istiyorsanız, sunucuyu kasaya kaydetmeden önce ekleyin. Kayıt işleminden sonra ek sunucu eklenemez.

1. vSphere Client envanterinde VM’ye sağ tıklayın ve **Ayarları Düzenle**’yi seçin.
2. **Donanım** bölümünde **Ekle** > **Ethernet Bağdaştırıcısı** seçeneğini belirleyin. Sonra **İleri**’yi seçin.
3. Bir bağdaştırıcı türü ve ağ seçin.
4. VM açıldığında sanal NIC'ye bağlanmak için seçin **Connect güç açma sırasında**. Ardından **sonraki** > **son** > **Tamam**.

## <a name="register-the-configuration-server"></a>Yapılandırma sunucusunu kaydetme 
VM'yi açın ve yapılandırma sunucusunu Site Recovery kasasına kaydedin.

1. VMWare vSphere Client konsolundan VM’yi açın.
2. VM, Windows Server 2016 yükleme deneyimi önyüklenir. Lisans sözleşmesini kabul edin ve bir yönetici parolası girin.
3. Yükleme tamamlandıktan sonra VM’de yönetici olarak oturum açın.



## <a name="set-up-the-configuration-server"></a>Yapılandırma sunucusunu ayarlayın

Dağıtımının bir parçası olarak, MySQL yapılandırma sunucusunda VM yüklemeniz gerekir. Bunu çeşitli şekillerde yapabilirsiniz:

- MySQL indirip Site Recovery olanak tanır. Bu seçeneği kullanmak istiyorsanız, yapılandırma sunucusu kurulumuna başlamadan önce herhangi bir şey yapmanız gerekmez.
- MySQL el ile yükleyip: yapılandırma sunucusunu ayarladıktan ayarlamadan önce MySQL yükleyiciyi indirir ve yerleştirin **C:\Temp\ASRSetup** klasör. Artık MySQL yükleyin. 
- El ile indirin ve Site Recovery yükleme sağlar. Yapılandırma sunucusunu ayarladıktan ayarlamadan önce MySQL yükleyiciyi indirir ve bunu koymak **C:\Temp\ASRSetup** klasör.


1. İlk VM'de oturum açın, oturum açtığınızda Azure Site Recovery Configuration Tool başlatılır.
2. Site Recovery Kasası'nda yapılandırma sunucusunu kaydetmek için kullanılan bir ad belirtin. Sonra **İleri**’yi seçin.
3. Araç, VM’nin Azure bağlanıp bağlanamadığını denetler. Bağlantı kurulduktan sonra Azure aboneliğinizde oturum açmak için **Oturum aç** seçeneğini belirleyin. Hesabı yapılandırma sunucusunu kaydetmek istediğiniz kasaya erişim olması gerektiğini unutmayın.
4. Araç birkaç yapılandırma görevi gerçekleştirir ve yeniden başlatır.
5. Makinede tekrar oturum açın. Yapılandırma sunucusu yönetim Sihirbazı otomatik olarak birkaç saniye içinde başlar.
6. Sihirbazda **bağlantı kurma**.
7. Sanal makineler çoğaltma trafiği almak için (varsayılan olarak yapılandırma sunucusunda çalışan) işlem sunucusu kullanan NIC'yi seçin.
8. Daha sonra **Kaydet**’e tıklayın. Yapılandırma sunucusunun kayıtlı olduğu sonra kasa ayarları değiştiremeyeceğinizi unutmayın. 
9. İçinde **kurtarma Hizmetleri kasasını seçin**, Microsoft Azure'da oturum açın, Azure aboneliğinizi ve ilgili kaynak grubu ve kasayı seçin. 
10. İçinde **üçüncü taraf yazılım Yükle**, MySQL yükleme:
     - Site Recovery MySQL indirme ve yükleme işliyorsa, tıklayın **indirme ve yükleme**. Yüklemeyi tamamlayın ve ardından sihirbazla devam etmek bekleyin.
     - MySQL indirilir ve Site Recovery yüklenir, tıklayın **yükleyip**. Yüklemenin tamamlanmasını bekleyin ve sihirbazla devam edin.
     - El ile yüklenen ve MySQL yüklüyse tıklayın **yükleyip**. Uygulama olarak gösterilir **yüklü**. Sihirbaza devam edin.
11. İçinde **gereç yapılandırmasını doğrulama**, kurulum devam etmeden önce önkoşulları doğrular. 
12. İçinde **vCenter sunucusu/vSphere ESXi sunucusunu yapılandır**, FQDN girin veya IP adresini vCenter sunucusunun veya vSphere konağının çoğaltmak istediğiniz VM'lerin bulunduğu. Sunucunun dinlediği bağlantı noktasını girin ve kasaya VMware sunucusu için bir kolay ad belirtin.
13. Yapılandırma sunucusu tarafından otomatik olarak çoğaltma için kullanılabilen VMware Vm'lerini keşfedin ve VMware sunucusuna bağlanmak için kullanılacak kimlik bilgilerini girin. Seçin **ekleme** >  **devam**. Kimlik bilgileri yerel olarak kaydedilir.
14. İçinde **sanal makine kimlik bilgilerini yapılandırma**, Site Recovery Mobility hizmeti, bir sanal makine için çoğaltmayı etkinleştirdiğinizde otomatik olarak yüklemek için kullanacağı kimlik bilgilerini belirtin.
    - Windows makinelerinde hesap için, çoğaltmak istediğiniz makinelerde yerel yönetici ayrıcalıkları gerekir.
    - Linux’ta kök hesap için bilgileri sağlayın.
15. Kaydı tamamlamak için **Yapılandırmayı son haline getir** seçeneğini belirleyin.
16. Kayıt tamamlandıktan sonra Azure portalını açın ve yapılandırma sunucusunun ve VMware sunucusunun göründüğünü denetlemek **kurtarma Hizmetleri kasası** > **Yönet**  >   **Site Recovery altyapısı** > **yapılandırma sunucusu**.


## <a name="exclude-antivirus-on-the-configuration-server"></a>Yapılandırma sunucusunda virüsten koruma Dışla

Virüsten koruma yazılımının yapılandırma Sunucusu'nda VM çalışıyorsa, aşağıdaki klasörleri virüsten koruma işlemlerini tutulduğundan emin olun. Bu, çoğaltma ve bağlantı beklendiği gibi çalıştığını sağlar: 

- C:\Program Files\Microsoft Azure Recovery Services Agent
- C:\Program Files\Microsoft Azure Site Recovery sağlayıcısı
- C:\Program Files\Microsoft Azure Site Recovery Yapılandırma Yöneticisi
- C:\Program Files\Microsoft Azure Site Recovery hata koleksiyon aracı
- C:\thirdparty
- C:\Temp
- C:\strawberry
- C:\ProgramData\MySQL
- C:\Program dosyaları (x86) \MySQL
- C:\ProgramData\ASR
- C:\ProgramData\Microsoft Azure Site kurtarma
- C:\ProgramData\ASRLogs
- C:\ProgramData\ASRSetupLogs
- C:\ProgramData\LogUploadServiceLogs
- C:\inetpub
- Site Recovery yükleme dizini. Örneğin: E:\Program dosyaları (x86) \Microsoft Azure Site Recovery


## <a name="next-steps"></a>Sonraki adımlar
- Zorluklarla çalıştırırsanız, gözden [sık sorulan sorular](vmware-azure-common-questions.md#configuration server) ve [sorun giderme](vmware-azure-troubleshoot-configuration-server.md) yapılandırma sunucusu için.
- Şimdi, [, hedef ortamı ayarlama](./vmware-azure-set-up-target.md) azure'da.
