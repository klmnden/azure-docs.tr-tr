---
title: Azure Site Recovery ile VMware olağanüstü durum kurtarma için yapılandırma sunucusu dağıtma | Microsoft Docs
description: Bu makalede, Azure Site Recovery ile VMware olağanüstü durum kurtarma için bir yapılandırma sunucusu dağıtmayı açıklar
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: raynew
ms.openlocfilehash: 4d944bec9d1cf73263d9deb54d0d9b3f6fc5532f
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36285042"
---
# <a name="deploy-a-configuration-server"></a>Yapılandırma sunucusunu dağıtma

Kullanırken bir şirket içi yapılandırma sunucusu dağıtmak [Azure Site Recovery](site-recovery-overview.md) VMware Vm'lerini ve fiziksel sunucuların azure'a olağanüstü durum kurtarma. Yapılandırma sunucusu koordinatları iletişimleri arasında şirket içi VMware ve Azure. Ayrıca, veri çoğaltma yönetir. Bu makalede, VMware Vm'lerini Azure'a çoğaltırken yapılandırma sunucusu dağıtmak için gereken adımlar anlatılmaktadır. [Bu makalede izleyin](physical-azure-set-up-source.md) fiziksel sunucu çoğaltma için yapılandırma sunucusu kurma gerekiyorsa.

## <a name="prerequisites"></a>Önkoşullar

Yüksek oranda kullanılabilir bir VMware VM olarak yapılandırma sunucusu dağıtmanızı öneririz. Yapılandırma sunucusu gereksinimleri aşağıdaki tabloda özetlenmiştir.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]



### <a name="prepare-for-mysql-installation"></a>MySQL yükleme için hazırlama

MySQL yapılandırma sunucusunda yüklü olmalıdır. Aşağıdaki yöntemlerden birini kullanarak bunu yapabilirsiniz:

- Site yükleyip yapılandırma sunucusu yönetim Sihirbazı'nı çalıştırdığında, Kurtarma sağlar. Belirli bir işlem yapmanıza gerek yoktur.
- El ile MySQL indirin ve bu klasöre C:\Temp\ASRSetup yerleştirin. Ardından yüklemesini çalıştırın. Site Kurtarma Sihirbazı'nı çalıştırdığında yüklü algılar.
- El ile MySQL indirin ve bu klasöre C:\Temp\ASRSetup yerleştirin. Sihirbaz çalıştığında kurulum dosyasını bulun ve bu konumdan yüklemesi. 


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
2. **Kaynağı hazırla** bölümünde **+Yapılandırma sunucusu**’nu seçin.
3. **Sunucu Ekle** bölümünde **Sunucu türü**’nde **VMware için yapılandırma sunucusu**’nun görüntülenip görüntülenmediğini kontrol edin.
4. Yapılandırma sunucusu için Open Virtualization Format (OVF) şablonunu indirin.

  > [!TIP]
  Yapılandırma sunucusu şablondan doğrudan en son sürümünü indirebilirsiniz [Microsoft Download Center](https://aka.ms/asrconfigurationserver).


## <a name="import-the-template-in-vmware"></a>VMware’de şablonu içeri aktarma


1. VMWare vSphere İstemcisi’ni kullanarak VMware vCenter sunucusunda veya vSphere ESXi konağında oturum açın.
2. **Dosya** menüsünde **OVF Şablonunu Dağıt** seçeneğini belirleyerek OVF Şablonu Dağıtma sihirbazını başlatın.

     ![OVF şablonu](./media/vmware-azure-deploy-configuration-server/vcenter-wizard.png)

3. İçinde **kaynak seçme**, indirilen OVF konumu girin.
4. İçinde **ayrıntıları inceleyin**seçin **sonraki**.
5. İçinde **seçin adı ve klasör** ve **Select configuration**, varsayılan ayarları kabul edin.
6. **Depolama seçin** bölümünde en iyi performans için **Sanal disk biçimini seçin** bölümden **Thick Provision Eager Zeroed** seçeneğini belirleyin.
4. Kalan sihirbaz sayfalarında varsayılan ayarları kabul edin.
5. **Tamamlanmaya hazır** bölümünde:

    * VM’yi varsayılan ayarlarla kurmak için **Dağıtımdan sonra aç** > **Son** seçeneğini belirleyin.

    * Ağ arabirimi eklemek için temizleyin **dağıtımdan sonra güç üzerinde**ve ardından **son**. Varsayılan olarak, yapılandırma sunucusu şablonu tek bir NIC ile dağıtılır. Dağıtımdan sonra daha fazla NIC ekleyebilirsiniz.


## <a name="add-an-additional-adapter"></a>Ek bağdaştırıcı ekleme

Ek bir NIC yapılandırma sunucusuna eklemek istiyorsanız, sunucuyu kasaya kaydetmek önce ekleyin. Kayıt işleminden sonra ek sunucu eklenemez.

1. vSphere Client envanterinde VM’ye sağ tıklayın ve **Ayarları Düzenle**’yi seçin.
2. **Donanım** bölümünde **Ekle** > **Ethernet Bağdaştırıcısı** seçeneğini belirleyin. Sonra **İleri**’yi seçin.
3. Bir bağdaştırıcı türü ve ağ seçin. 
4. VM açıldığında sanal NIC’ye bağlanmak için **Açıldığında bağlan**’ı seçin. Ardından **sonraki** > **son** > **Tamam**.
 

## <a name="register-the-configuration-server"></a>Yapılandırma sunucusunu kaydetme 

1. VMWare vSphere Client konsolundan VM’yi açın.
2. VM’de Windows Server 2016 yükleme deneyimi önyüklemesi yapılır. Lisans sözleşmesini kabul edin ve bir yönetici parolası girin.
3. Yükleme tamamlandıktan sonra VM’de yönetici olarak oturum açın.
4. İlk oturum açma işleminizde Azure Site Recovery Yapılandırma Aracı başlar.
5. Yapılandırma sunucusunu Site Recovery’ye kaydetmek için kullanılacak bir ad girin. Sonra **İleri**’yi seçin.
6. Araç, VM’nin Azure bağlanıp bağlanamadığını denetler. Bağlantı kurulduktan sonra Azure aboneliğinizde oturum açmak için **Oturum aç** seçeneğini belirleyin. Kimlik bilgilerinin, yapılandırma sunucusunu kaydetmek istediğiniz kasaya erişim izni olmalıdır.
7. Araç birkaç yapılandırma görevi gerçekleştirir ve yeniden başlatır.
8. Makinede tekrar oturum açın. Yapılandırma sunucusu yönetim sihirbazı otomatik olarak başlar.

### <a name="configure-settings"></a>Ayarları yapılandırma

1. Yapılandırma sunucusu yönetim Sihirbazı'nda seçin **Kurulum Bağlantı**. Çoğaltma trafiğini almak ve daha sonra seçmek için NIC seçin **kaydetmek**. Bu ayarı yapılandırdıktan sonra değiştiremezsiniz.
2. **Recovery Services kasasını seçin** bölümünde Azure aboneliğinizi, ilgili kaynak grubunu ve kasayı seçin.
3. **Üçüncü taraf yazılımı yükleyin** bölümünde lisans sözleşmesini kabul edin. MySQL ile uyumlu olarak yükleme [MySQL yüklemek için kullandığınız yöntem](#prepare-for-mysql-installation).
4. Seçin **VMware PowerLCI yükleme**. Bu adımı uygulamadan önce tüm tarayıcı pencerelerini kapalı olduğundan emin olun. Daha sonra **Devam** seçeneğini belirleyin.
5. **Gereç yapılandırmasını doğrulama** bölümünde ön koşullar, siz devam etmeden önce doğrulanır.
6. **vCenter Server/vSphere ESXi sunucusu yapılandırma** bölümünde, çoğaltmak istediğiniz VM’lerin bulunduğu vCenter sunucusunun veya vSphere konağının FQDN’sini ya da IP adresini girin. Bağlantı noktası girin, sunucunun dinlediği ve kasadaki VMware sunucusu için bir kolay ad.
7. VMware sunucusu ile bağlantı için yapılandırma sunucusu tarafından kullanılacak kimlik bilgilerini girin. Site Recovery, bu kimlik bilgilerini çoğaltma için kullanılabilen VMware VM’lerini otomatik olarak bulmak üzere kullanır. **Ekle** seçeneğini ve sonra **Devam** seçeneğini belirleyin.
8. İçinde **sanal makine kimlik bilgileri yapılandırma**, çoğaltma etkinleştirildiğinde, Azure Site Recovery Mobility hizmeti makinelerde otomatik olarak yüklemek için kullanılacak parola ve kullanıcı adı girin. Windows makinelerinde hesap için, çoğaltmak istediğiniz makinelerde yerel yönetici ayrıcalıkları gerekir. Linux’ta kök hesap için bilgileri sağlayın.
9. Seçin **Finalize yapılandırma** kaydı tamamlamak için. 
10. Kayıt bittikten sonra Azure portalında, yapılandırma sunucusunun ve VMware sunucusunun kasadaki **Kaynak** sayfasında listelendiğinden emin olun. Daha sonra hedef ayarlarını yapılandırmak için **Tamam**’ı seçin.


## <a name="upgrade-the-configuration-server"></a>Yapılandırma sunucusu yükseltme

Yapılandırma sunucusunun en son sürüme yükseltmek için verilen adımları okuyun [burada](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server)


## <a name="troubleshoot-deployment-issues"></a>Dağıtım sorunlarını giderme

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]



## <a name="next-steps"></a>Sonraki adımlar

Olağanüstü durum kurtarma ayarlamak [VMware Vm'lerini](vmware-azure-tutorial.md) azure'a.
