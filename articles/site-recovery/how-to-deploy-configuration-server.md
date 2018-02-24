---
title: "Azure Site Recovery ile VMware olağanüstü durum kurtarma için yapılandırma sunucusu dağıtma | Microsoft Docs"
description: "Bu makalede, Azure Site Recovery ile VMware olağanüstü durum kurtarma için bir yapılandırma sunucusu dağıtmayı açıklar"
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 01/15/2018
ms.author: anoopkv
ms.openlocfilehash: 3b09c11d76d5c88b904afa3c6d256bc475992339
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
---
# <a name="deploy-a-configuration-server"></a>Yapılandırma sunucusunu dağıtma

Kullanırken bir şirket içi yapılandırma sunucusu dağıtmak [Azure Site Recovery](site-recovery-overview.md) VMware Vm'lerini ve fiziksel sunucuların azure'a olağanüstü durum kurtarma. Yapılandırma sunucusu koordinatları iletişimleri arasında şirket içi VMware ve Azure. Ayrıca, veri çoğaltma yönetir. Bu makalede yapılandırma sunucusu dağıtmak için gereken adımlarda size yol gösterir.

## <a name="prerequisites"></a>Önkoşullar

Yüksek oranda kullanılabilir bir VMware VM olarak yapılandırma sunucusu dağıtmanızı öneririz. Fiziksel sunucu çoğaltma için bir fiziksel makine yapılandırma sunucusu ayarlanabilir. En düşük donanım gereksinimleri aşağıdaki tabloda özetlenmiştir.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]




## <a name="capacity-planning"></a>Kapasite planlaması

Yapılandırma sunucusu için boyutlandırma gereksinimleri olası veri değişikliği oranına bağlıdır. Bu tablo bir kılavuz olarak kullanın.

| **CPU** | **Bellek** | **Önbellek disk boyutu** | **Veri değişikliği oranı** | **Korumalı makineler** |
| --- | --- | --- | --- | --- |
| 8 Vcpu'lar (2 yuva * @ 2,5 GHz 4 çekirdek) |16 GB |300 GB |500 GB veya daha az |100'den az makineler çoğaltılır. |
| 12 Vcpu'lar (2 yuva * 2,5 GHz @ 6 çekirdek) |18 GB |600 GB |1 TB ' 500 GB |100-150 makineler çoğaltılır. |
| 16 Vcpu (2 yuva * 2,5 GHz @ 8 çekirdek) |32 GB |1 TB |1 TB ile 2 TB |150-200 makineler çoğaltılır. |


VMware sanal makinelerini çoğaltıyorsanız daha fazla bilgi edinin [kapasite planlama konuları](/site-recovery-plan-capacity-vmware.md). Çalıştırma [dağıtım planlayıcısı aracı](site-recovery-deployment-planner.md) VMWare çoğaltma için.



## <a name="download-the-template"></a>Şablonu İndirme

Site Recovery yapılandırma sunucusu yüksek oranda kullanılabilir bir VMware VM olarak ayarlamak için indirilebilir bir şablonu sağlar. 

1. Kasada **Altyapıyı Hazırlama** > **Kaynak** seçeneğine gidin.
2. İçinde **kaynağı**seçin **+ yapılandırma sunucusu**.
3. **Sunucu Ekle** bölümünde **Sunucu türü**’nde **VMware için yapılandırma sunucusu**’nun görüntülenip görüntülenmediğini kontrol edin.
4. Yapılandırma sunucusu için Open Virtualization Format (OVF) şablonunu indirin.

  > [!TIP]
  Yapılandırma sunucusu şablondan doğrudan en son sürümünü indirebilirsiniz [Microsoft Download Center](https://aka.ms/asrconfigurationserver).


## <a name="import-the-template-in-vmware"></a>VMware’de şablonu içeri aktarma


1. VMware vCenter sunucusu veya vSphere ESXi ana bilgisayar için VMWare vSphere istemci kullanarak oturum açın.
2. Üzerinde **dosya** menüsünde, select **OVF şablon dağıtma** OVF şablon Dağıtma Sihirbazı'nı başlatın.

     ![OVF şablonu](./media/tutorial-vmware-to-azure/vcenter-wizard.png)

3. İçinde **kaynak seçme**, indirilen OVF konumu girin.
4. İçinde **ayrıntıları inceleyin**seçin **sonraki**.
5. İçinde **seçin adı ve klasör** ve **Select configuration**, varsayılan ayarları kabul edin.
6. **Depolama seçin** bölümünde en iyi performans için **Sanal disk biçimini seçin** bölümden **Thick Provision Eager Zeroed** seçeneğini belirleyin.
4. Kalan sihirbaz sayfalarında varsayılan ayarları kabul edin.
5. **Tamamlanmaya hazır** bölümünde:

    * VM’yi varsayılan ayarlarla kurmak için **Dağıtımdan sonra aç** > **Son** seçeneğini belirleyin.

    * Ağ arabirimi eklemek için temizleyin **dağıtımdan sonra güç üzerinde**ve ardından **son**. Varsayılan olarak, tek bir NIC ile yapılandırma sunucusu şablonu dağıtılır Dağıtımdan sonra ek NIC'lerin ekleyebilirsiniz.


## <a name="add-an-additional-adapter"></a>Ek bağdaştırıcı ekleme

Ek bir NIC yapılandırma sunucusuna eklemek istiyorsanız, sunucuyu kasaya kaydetmek önce ekleyin. Kayıt işleminden sonra ek sunucu eklenemez.

1. vSphere Client envanterinde VM’ye sağ tıklayın ve **Ayarları Düzenle**’yi seçin.
2. İçinde **donanım**seçin **Ekle** > **Ethernet bağdaştırıcısı**. Sonra **İleri**’yi seçin.
3. Bir bağdaştırıcı türü ve bir ağ seçin. 
4. VM açıldığında sanal NIC’ye bağlanmak için **Açıldığında bağlan**’ı seçin. Ardından **sonraki** > **son** > **Tamam**.
 

## <a name="register-the-configuration-server"></a>Yapılandırma sunucusunu kaydetme 

1. VMWare vSphere Client konsolundan VM’yi açın.
2. VM’de Windows Server 2016 yükleme deneyimi önyüklemesi yapılır. Lisans Sözleşmesi'ni kabul edin ve bir yönetici parolası girin.
3. Yükleme tamamlandıktan sonra VM yönetici olarak oturum açın.
4. İlk kez oturum açma, Azure Site kurtarma yapılandırma aracı başlatır.
5. Site Recovery ile yapılandırma sunucusunu kaydetmek için kullanılan bir ad girin. Sonra **İleri**’yi seçin.
6. Araç, VM’nin Azure bağlanıp bağlanamadığını denetler. Bağlantı kurulduktan sonra seçin **oturum** Azure aboneliğinizde oturum açmak için. Kimlik bilgilerinin, yapılandırma sunucusunu kaydetmek istediğiniz kasaya erişim izni olmalıdır.
7. Araç birkaç yapılandırma görevi gerçekleştirir ve yeniden başlatır.
8. Makine için yeniden oturum açın. Yapılandırma sunucusu yönetim Sihirbazı'nı otomatik olarak başlar.

### <a name="configure-settings"></a>Ayarları yapılandırma

1. Yapılandırma sunucusu yönetim Sihirbazı'nda seçin **Kurulum Bağlantı**. Çoğaltma trafiğini almak ve daha sonra seçmek için NIC seçin **kaydetmek**. Bu ayarı yapılandırdıktan sonra değiştiremezsiniz.
2. İçinde **seçin kurtarma Hizmetleri kasası**, Azure aboneliğiniz ile ilgili kaynak grubu seçin ve kasa.
3. İçinde **üçüncü taraf yazılımlarını yüklemek**, lisans sözleşmesini kabul edin. Seçin **yükleyip** MySQL Server yüklemek için.
4. Seçin **VMware PowerLCI yükleme**. Bu adımı uygulamadan önce tüm tarayıcı pencerelerini kapalı olduğundan emin olun. Ardından **devam**.
5. İçinde **doğrulama Gereci yapılandırma**, önkoşulları devam etmeden önce doğrulandı.
6. İçinde **yapılandırma vCenter sunucusu/vSphere ESXi sunucusunda**, çoğaltmak istediğiniz sanal makineleri bulunduğu vCenter sunucusu ya da vSphere ana bilgisayar FQDN veya IP adresini girin. Bağlantı noktası girin, sunucunun dinlediği ve kasadaki VMware sunucusu için bir kolay ad.
7. VMware sunucuya bağlanmak için yapılandırma sunucusu tarafından kullanılacak kimlik bilgilerini girin. Site Recovery, bu kimlik bilgilerini çoğaltma için kullanılabilen VMware VM’lerini otomatik olarak bulmak üzere kullanır. Seçin **Ekle**ve ardından **devam**.
8. İçinde **sanal makine kimlik bilgileri yapılandırma**, çoğaltma etkinleştirildiğinde, Azure Site Recovery Mobility hizmeti makinelerde otomatik olarak yüklemek için kullanılacak parola ve kullanıcı adı girin. Windows makinelerinde hesap için, çoğaltmak istediğiniz makinelerde yerel yönetici ayrıcalıkları gerekir. Linux’ta kök hesap için bilgileri sağlayın.
9. Seçin **Finalize yapılandırma** kaydı tamamlamak için. 
10. Kayıt bittikten sonra Azure portalında, yapılandırma sunucusunun ve VMware sunucusunun kasadaki **Kaynak** sayfasında listelendiğinden emin olun. Ardından **Tamam** hedef ayarlarını yapılandırmak için.


## <a name="troubleshoot-deployment-issues"></a>Dağıtım sorunlarını giderme

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]



## <a name="next-steps"></a>Sonraki adımlar

Öğreticiler olağanüstü durum kurtarma ayarlama konusunda gözden [VMware Vm'lerini](tutorial-vmware-to-azure.md) ve [fiziksel sunucuları](tutorial-physical-to-azure.md) Azure.
