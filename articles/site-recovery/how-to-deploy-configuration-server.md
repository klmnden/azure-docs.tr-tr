---
title: " Azure Site Recovery ile VMware olağanüstü durum kurtarma için yapılandırma sunucusu dağıtma | Microsoft Docs"
description: "Bu makalede, Azure Site Recovery ile VMware olağanüstü durum kurtarma için bir yapılandırma sunucusu dağıtmayı açıklar"
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 01/15/2018
ms.author: anoopkv
ms.openlocfilehash: e257ede08ac46ad863b4883b10399058e6f59f1f
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="deploy-a-configuration-server"></a>Yapılandırma sunucusu dağıtma

Kullanırken bir şirket içi yapılandırma sunucusu dağıtmak [Azure Site Recovery](site-recovery-overview.md) VMware Vm'lerini ve fiziksel sunucuların azure'a olağanüstü durum kurtarma için hizmet. WThe yapılandırma sunucusu arasındaki iletişimi düzenler VMware ve Azure şirket içi ve veri çoğaltma yönetir. Bu makalede yapılandırma sunucusu dağıtmak için gereken adımlarda size yol gösterir.

## <a name="prerequisites"></a>Önkoşullar

Yüksek oranda kullanılabilir bir VMware VM olarak yapılandırma sunucusu dağıtmanızı öneririz. Fiziksel sunucu çoğaltma için bir fiziksel makine yapılandırma sunucusu ayarlanabilir. En düşük donanım gereksinimleri aşağıdaki tabloda özetlenmiştir.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]




## <a name="capacity-planning"></a>Kapasite planlaması

Yapılandırma sunucusu için boyutlandırma gereksinimleri olası veri değişikliği oranına bağlıdır. Bu tablo bir kılavuz olarak kullanın.

| **CPU** | **Bellek** | **Önbellek disk boyutu** | **Veri değişikliği oranı** | **Korumalı makineler** |
| --- | --- | --- | --- | --- |
| 8 Vcpu'lar (2 yuva * @ 2,5 GHz 4 çekirdek) |16 GB |300 GB |500 GB veya daha az |100'den az makineler çoğaltılır. |
| 12 Vcpu'lar (2 yuva * 2,5 GHz @ 6 çekirdek) |18 GB |600 GB |1 TB ' 500 GB |100-150 makineler arasında çoğaltılır. |
| 16 Vcpu (2 yuva * 2,5 GHz @ 8 çekirdek) |32 GB |1 TB |1 TB ile 2 TB |150-200 makineler arasında çoğaltılır. |


VMware sanal makinelerini çoğaltıyorsanız daha fazla bilgi edinin [kapasite planlama konuları](/site-recovery-plan-capacity-vmware.md), çalıştırıp [dağıtım planlayıcısı aracı](site-recovery-deployment-planner.md) VMWare çoğaltma için.



## <a name="download-the-template"></a>Şablonu İndirme

Site Recovery yapılandırma sunucusu yüksek oranda kullanılabilir bir VMware VM olarak ayarlamak için indirilebilir bir şablonu sağlar. 

1. Kasasına gidin **altyapıyı hazırlama** > **kaynak**.
2. İçinde **kaynağı**, tıklatın **+ yapılandırma sunucusu**.
3. İçinde **Sunucu Ekle**, denetleyin **VMware yapılandırma sunucusu** görünür **sunucu türü**.
4. Yapılandırma sunucusu için açık sanallaştırma biçimi (OVF) şablonu indirin.

  > [!TIP]
  Yapılandırma sunucusu şablonu en son sürümünü doğrudan indirilebilir [Microsoft Download Center](https://aka.ms/asrconfigurationserver)


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

### <a name="configure-settings"></a>Ayarları yapılandırma

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


## <a name="troubleshoot-deployment-issues"></a>Dağıtım sorunlarını giderme

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]



## <a name="next-steps"></a>Sonraki adımlar

Olağanüstü durum kurtarma ayarlamak için öğreticileri gözden [VMware Vm'lerini](tutorial-vmware-to-azure.md) ve [fiziksel sunucuları](tutorial-physical-to-azure.md) Azure.
