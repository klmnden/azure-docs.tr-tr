---
title: Azure Site Recovery ile VMware olağanüstü durum kurtarma için yapılandırma sunucusu dağıtma | Microsoft Docs
description: Bu makalede, Azure Site Recovery ile VMware olağanüstü durum kurtarma için bir yapılandırma sunucusu dağıtmayı açıklar
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 05/06/2018
ms.author: raynew
ms.openlocfilehash: 841176d8c5f215d18edf25b1f191792b37555fa9
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36318128"
---
# <a name="deploy-a-configuration-server"></a>Yapılandırma sunucusunu dağıtma

Kullanırken bir şirket içi yapılandırma sunucusu dağıtmak [Azure Site Recovery](site-recovery-overview.md) VMware Vm'lerini ve fiziksel sunucuların azure'a olağanüstü durum kurtarma. Yapılandırma sunucusu koordinatları iletişimleri arasında şirket içi VMware ve Azure. Ayrıca, veri çoğaltma yönetir. Bu makalede, VMware Vm'lerini Azure'a çoğaltırken yapılandırma sunucusu dağıtmak için gereken adımlar anlatılmaktadır. [Bu makalede izleyin](physical-azure-set-up-source.md) fiziksel sunucu çoğaltma için yapılandırma sunucusu kurma gerekiyorsa.

>[!TIP]
Yapılandırma sunucusu rolü Azure Site Recovery mimarisi bir parçası olarak öğrenebilirsiniz [burada](vmware-azure-architecture.md).

## <a name="deployment-of-configuration-server-through-ova-template"></a>Yapılandırma sunucusu üzerinden OVA şablon dağıtımı

Yapılandırma sunucusu belirli en düşük donanım ve boyutlandırma gereksinimleri ile yüksek oranda kullanılabilir bir VMware VM olarak ayarlanması gerekir. Rahat ve kolay dağıtımı için aşağıda listelenen tüm mandated gereksinimleri ile uyumludur yapılandırma sunucusu ayarlamak üzere indirilebilir bir OVA (açık sanallaştırma uygulama) şablonu Site kurtarma sağlar.

## <a name="prerequisites"></a>Önkoşullar

Yapılandırma sunucusu için en düşük donanım gereksinimleri aşağıdaki tabloda özetlenmiştir.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="capacity-planning"></a>Kapasite planlaması

Yapılandırma sunucusu için boyutlandırma gereksinimleri olası veri değişikliği oranına bağlıdır. Bu tablo bir kılavuz olarak kullanın.

| **CPU** | **Bellek** | **Önbellek disk boyutu** | **Veri değişikliği oranı** | **Korumalı makineler** |
| --- | --- | --- | --- | --- |
| 8 Vcpu'lar (2 yuva * @ 2,5 GHz 4 çekirdek) |16 GB |300 GB |500 GB veya daha az |100'den az makineler çoğaltılır. |
| 12 Vcpu'lar (2 yuva * 2,5 GHz @ 6 çekirdek) |18 GB |600 GB |1 TB ' 500 GB |100-150 makineler çoğaltılır. |
| 16 Vcpu (2 yuva * 2,5 GHz @ 8 çekirdek) |32 GB |1 TB |1 TB ile 2 TB |150-200 makineler çoğaltılır. |

Birden fazla VMware VM çoğaltıyorsanız okuma [kapasite planlama konuları](/site-recovery-plan-capacity-vmware.md). Çalıştırma [dağıtım planlayıcısı aracı](site-recovery-deployment-planner.md) VMWare çoğaltma için.

## <a name="download-the-template"></a>Şablonu İndirme

1. Kasada **Altyapıyı Hazırlama** > **Kaynak** seçeneğine gidin.
2. **Kaynağı hazırla** bölümünde **+Yapılandırma sunucusu**’nu seçin.
3. **Sunucu Ekle** bölümünde **Sunucu türü**’nde **VMware için yapılandırma sunucusu**’nun görüntülenip görüntülenmediğini kontrol edin.
4. Yapılandırma sunucusu için açık sanallaştırma uygulama (OVA) şablonu indirin.

  > [!TIP]
>Ayrıca doğrudan yapılandırma sunucusu şablonu en son sürümünü indirebilirsiniz [Microsoft Download Center](https://aka.ms/asrconfigurationserver).

>[!NOTE]
OVA şablonu ile sağlanan lisans 180 gün için geçerli bir değerlendirme Lisans ' dir. POST bu süre, müşteri tedarik edilen bir lisans ile windows etkinleştirmesi gerekir.

## <a name="import-the-template-in-vmware"></a>VMware’de şablonu içeri aktarma

1. VMWare vSphere İstemcisi’ni kullanarak VMware vCenter sunucusunda veya vSphere ESXi konağında oturum açın.
2. **Dosya** menüsünde **OVF Şablonunu Dağıt** seçeneğini belirleyerek OVF Şablonu Dağıtma sihirbazını başlatın.

     ![OVF şablonu](./media/vmware-azure-deploy-configuration-server/vcenter-wizard.png)

3. İçinde **kaynak seçme**, indirilen OVF konumu girin.
4. İçinde **ayrıntıları inceleyin**seçin **sonraki**.
5. İçinde **seçin adı ve klasör** ve **Select configuration**, varsayılan ayarları kabul edin.
6. **Depolama seçin** bölümünde en iyi performans için **Sanal disk biçimini seçin** bölümden **Thick Provision Eager Zeroed** seçeneğini belirleyin.
7. Kalan sihirbaz sayfalarında varsayılan ayarları kabul edin.
8. **Tamamlanmaya hazır** bölümünde:

    * VM’yi varsayılan ayarlarla kurmak için **Dağıtımdan sonra aç** > **Son** seçeneğini belirleyin.

    * Ağ arabirimi eklemek için temizleyin **dağıtımdan sonra güç üzerinde**ve ardından **son**. Varsayılan olarak, yapılandırma sunucusu şablonu tek bir NIC ile dağıtılır. Dağıtımdan sonra daha fazla NIC ekleyebilirsiniz.

## <a name="add-an-additional-adapter"></a>Ek bağdaştırıcı ekleme

Ek bir NIC yapılandırma sunucusuna eklemek istiyorsanız, sunucuyu kasaya kaydetmek önce ekleyin. Kayıt işleminden sonra ek sunucu eklenemez.

1. vSphere Client envanterinde VM’ye sağ tıklayın ve **Ayarları Düzenle**’yi seçin.
2. **Donanım** bölümünde **Ekle** > **Ethernet Bağdaştırıcısı** seçeneğini belirleyin. Sonra **İleri**’yi seçin.
3. Bir bağdaştırıcı türü ve ağ seçin. 
4. VM açıldığında sanal NIC bağlanmak için **Bağlan çalıştırma sırasında**. Ardından **sonraki** > **son** > **Tamam**.

## <a name="register-the-configuration-server-with-azure-site-recovery-services"></a>Yapılandırma sunucusu Azure Site Recovery services ile kaydetme

1. VMWare vSphere Client konsolundan VM’yi açın.
2. VM’de Windows Server 2016 yükleme deneyimi önyüklemesi yapılır. Lisans sözleşmesini kabul edin ve bir yönetici parolası girin.
3. Yükleme tamamlandıktan sonra VM’de yönetici olarak oturum açın.
4. İçinde birkaç saniye içinde Azure Site kurtarma yapılandırma aracı oturum ilk kez başlatır.
5. Yapılandırma sunucusunu Site Recovery’ye kaydetmek için kullanılacak bir ad girin. Sonra **İleri**’yi seçin.
6. Araç, VM’nin Azure bağlanıp bağlanamadığını denetler. Bağlantı kurulduktan sonra Azure aboneliğinizde oturum açmak için **Oturum aç** seçeneğini belirleyin. Kimlik bilgilerinin, yapılandırma sunucusunu kaydetmek istediğiniz kasaya erişim izni olmalıdır.
7. Araç birkaç yapılandırma görevi gerçekleştirir ve yeniden başlatır.
8. Makinede tekrar oturum açın. Yapılandırma sunucusu yönetim Sihirbazı'nı başlatır **otomatik olarak** birkaç saniye cinsinden.

### <a name="configure-settings"></a>Ayarları yapılandırma

1. Yapılandırma sunucusu yönetim sihirbazında **Bağlantı kurma** seçeneğini belirleyip, VM’lerden çoğaltma trafiğini almak için işlem sunucusunun kullandığı NIC’yi seçin. Daha sonra **Kaydet**’e tıklayın. Bunu yapılandırıldıktan sonra bu ayarı değiştiremezsiniz.
2. İçinde **seçin kurtarma Hizmetleri kasası**, Microsoft Azure oturumu açın, Azure aboneliğinize ve ilgili kaynak grubu ve kasa seçin.
    >[!NOTE]
    > Kayıttan sonra kurtarma Hizmetleri kasası değiştirmek için hiçbir esneklik yoktur.
3. İçinde **üçüncü taraf yazılımlarını yüklemek**,

    |Senaryo   |İzlenecek adımlar  |
    |---------|---------|
    |Karşıdan yüklemek & MySQL el ile yüklemeniz?     |  Evet. MySQL uygulamayı indirmek ve bu klasöre yerleştirin **C:\Temp\ASRSetup**, el ile yükleyin. Şimdi, ne zaman, koşullarını kabul > tıklayın **yükleyip**, portal diyor *zaten yüklü*. Sonraki adıma geçebilirsiniz.       |
    |Çevrimiçi MySQL yüklenmesini önlemek?     |   Evet. MySQL yükleyici uygulamanız bu klasöre yerleştirin **C:\Temp\ASRSetup**. Koşulları kabul > tıklayın **yükleyip**, portal sizin tarafınızdan eklenen yükleyici kullanır ve uygulamayı yükler. Sonraki adım post yüklemeye devam edebilirsiniz.    |
    |Karşıdan yükle ve Azure Site Recovery aracılığıyla MySQL yüklemek ister misiniz     |  Lisans sözleşmesini kabul & tıklayın **yükleyip**. Ardından bir sonraki adım post yüklemeye devam edebilirsiniz.       |
4. **Gereç yapılandırmasını doğrulama** bölümünde ön koşullar, siz devam etmeden önce doğrulanır.
5. **vCenter Server/vSphere ESXi sunucusu yapılandırma** bölümünde, çoğaltmak istediğiniz VM’lerin bulunduğu vCenter sunucusunun veya vSphere konağının FQDN’sini ya da IP adresini girin. Sunucunun dinleme gerçekleştirdiği bağlantı noktasını girin. Kasadaki VMware sunucusu için kullanılacak bir kolay ad girin.
6. VMware sunucusu ile bağlantı için yapılandırma sunucusu tarafından kullanılacak kimlik bilgilerini girin. Site Recovery, bu kimlik bilgilerini çoğaltma için kullanılabilen VMware VM’lerini otomatik olarak bulmak üzere kullanır. Seçin **ekleme**ve ardından **devam**. Buraya girilen kimlik bilgileri yerel olarak kaydedilir.
7. İçinde **sanal makine kimlik bilgileri yapılandırma**, kullanıcı adı ve parola Mobility hizmeti çoğaltma sırasında otomatik olarak yüklemek için sanal makinelerin girin. İçin **Windows** makineler, hesap çoğaltmak istediğiniz makinelerde yerel yönetici ayrıcalıkları gerekiyor. İçin **Linux**, kök hesabının ayrıntılarını verin.
8. Kaydı tamamlamak için **Yapılandırmayı son haline getir** seçeneğini belirleyin.
9. Kayıt tamamlandıktan sonra Azure portalını açın, VMware sunucusu ve yapılandırma sunucusu listelendiğini doğrulayın **kurtarma Hizmetleri kasası** > **Yönet**  >  **Site Recovery altyapısı** > **yapılandırma sunucularına**.

## <a name="faq"></a>SSS

1. Yapılandırma sunucusu farklı amaçlar için yüklendiği VM kullanabilir miyim? **Hayır**, yapılandırma sunucusu, tek amaçlı bir sunucu olmalı ve paylaşılan bir sunucu kullanma desteklenmiyor.
2. Yeni oluşturulan kasası ile yapılandırma sunucusu zaten kayıtlı kasası geçiş yapabilirim? **Hayır**, bir kasa yapılandırma sunucusuna kayıtlı sonra değiştirilemez.
3. Aynı yapılandırma sunucusuna hem fiziksel hem de sanal makineleri korumak için kullanabilir miyim? **Evet**, aynı yapılandırma sunucusuna fiziksel ve sanal makineleri çoğaltmak için kullanılabilir. Ancak, bir fiziksel makine için yeniden çalışma desteklenmez.
4. Yapılandırma sunucusu nerede kullanılacak? Azure Site Recovery mimarimizin başvuran [burada](vmware-azure-architecture.md) yapılandırma sunucusu ve onun işlevler hakkında daha fazla bilgi edinmek için.
5. Yapılandırma sunucusunun en son sürümünü nereden bulabilirim? Doğrudan buradan indirebilirsiniz [Microsoft Download Center](https://aka.ms/asrconfigurationserver). Yükseltme yapılandırma sunucusuna adımlar makalesine başvurun [burada](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server).

## <a name="upgrade-the-configuration-server"></a>Yapılandırma sunucusu yükseltme

Yapılandırma sunucusunun en son sürüme yükseltmek için verilen adımları okuyun [burada](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server)

## <a name="troubleshoot-deployment-issues"></a>Dağıtım sorunlarını giderme

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]



## <a name="next-steps"></a>Sonraki adımlar

Olağanüstü durum kurtarma ayarlamak [VMware Vm'lerini](vmware-azure-tutorial.md) azure'a.
