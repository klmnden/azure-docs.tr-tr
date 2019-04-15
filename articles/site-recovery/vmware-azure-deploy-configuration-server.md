---
title: Azure Site Recovery ile VMware olağanüstü durum kurtarması için yapılandırma sunucusunu dağıtma | Microsoft Docs
description: Bu makalede Azure Site Recovery ile VMware olağanüstü durum kurtarması için yapılandırma sunucusunu dağıtma
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/06/2019
ms.author: ramamill
ms.openlocfilehash: c25ca8c27b84f34b025ec5abce00c8d8c70e5df6
ms.sourcegitcommit: b8a8d29fdf199158d96736fbbb0c3773502a092d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59565133"
---
# <a name="deploy-a-configuration-server"></a>Yapılandırma sunucusunu dağıtma

Kullanırken bir şirket içi yapılandırma sunucusunu dağıtma [Azure Site Recovery](site-recovery-overview.md) VMware Vm'lerini ve fiziksel sunucuları azure'a olağanüstü durum kurtarma için. Yapılandırma sunucusu koordinatları iletişimleri arasında şirket içi VMware ve Azure. Veri çoğaltma işlemlerini yönetir. Bu makalede, VMware Vm'lerini Azure'a çoğaltırken, yapılandırma sunucusu dağıtmak için gereken adımlar anlatılmaktadır. [Bu makaleyi takip](physical-azure-set-up-source.md) fiziksel sunucu çoğaltması için yapılandırma sunucusunu ayarlamak gerekiyorsa.

> [!TIP]
> Yapılandırma sunucusu rolü parçası olan Azure Site Recovery mimarisi hakkında bilgi edinebilirsiniz [burada](vmware-azure-architecture.md).

## <a name="deployment-of-configuration-server-through-ova-template"></a>OVA şablon aracılığıyla yapılandırma sunucusu dağıtımı

Yapılandırma sunucusu belirli en düşük donanım ve boyutlandırma gereksinimleri ile yüksek oranda kullanılabilir bir VMware VM olarak ayarlanmalıdır. Rahat ve kolay dağıtımı için Site Recovery, aşağıda listelenen mandated gereksinimleriyle uyumlu yapılandırma sunucusunu ayarlamak için indirilebilir bir OVA (Open Virtualization uygulaması) şablonunu sunar.

## <a name="prerequisites"></a>Önkoşullar

Yapılandırma sunucusu için en düşük donanım gereksinimleri aşağıdaki tabloda özetlenmiştir.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="azure-active-directory-permission-requirements"></a>Azure Active Directory izin gereksinimleri

Bir kullanıcıyla gerektiren **aşağıdakilerden birini** AAD (Azure Active Directory) Azure Site Recovery hizmetleriyle yapılandırma sunucusunu kaydetmek için ayarlanan izinler.

1. Kullanıcı, uygulama oluşturmak için "Uygulama geliştiricisi" rolüne sahip olmalıdır.
   1. Doğrulamak için Azure portalında oturum açın</br>
   1. Azure Active Directory'ye gidin > Roller ve Yöneticiler</br>
   1. "Uygulama geliştiricisi" rolü kullanıcıya atanıp atanmadığını doğrulayın. Aksi takdirde, bir kullanıcının bu izinle kullanın veya ulaşın [Yöneticisi izni](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal#assign-roles).
    
1. "Uygulama geliştiricisi" rolü atanmış, "Kullanıcı uygulama kaydedebilir" bayrak kullanıcının kimliği oluşturmak true olarak ayarlandığından emin olun. İzinleri etkinleştirmek için
   1. Azure portalda oturum açın
   1. Azure Active Directory'ye gidin > kullanıcı ayarları
   1. Altında ** uygulama kayıtları ","Kullanıcılar uygulamaları kaydedebilir","Evet"olarak seçilmelidir.

      ![AAD_application_permission](media/vmware-azure-deploy-configuration-server/AAD_application_permission.png)

> [!NOTE]
> Active Directory Federasyon Services(ADFS) olduğu **desteklenmiyor**. Lütfen üzerinden yönetilen bir hesap kullanın [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis).

## <a name="capacity-planning"></a>Kapasite planlaması

Yapılandırma sunucusu boyutlandırma gereksinimlerini olası veri değişikliği hızınıza bağlıdır. Bu tabloyu kılavuz olarak kullanın.

| **CPU** | **Bellek** | **Önbellek diski boyutu** | **Veri değişiklik oranı** | **Korumalı makineler** |
| --- | --- | --- | --- | --- |
| 8 Vcpu (2 yuva * 4 çekirdek \@ 2.5 GHz) |16 GB |300 GB |500 GB veya daha az |100'den az makineleri çoğaltabilir. |
| 12 Vcpu (2 yuva * 6 çekirdek \@ 2.5 GHz) |18 GB |600 GB |500 GB ila 1 TB |100-150 makineleri çoğaltabilir. |
| 16 Vcpu (2 yuva * 8 çekirdek \@ 2.5 GHz) |32 GB |1 TB |1 TB ile 2 TB |150-200 makineleri çoğaltabilir. |

Birden fazla VMware VM çoğaltma yapıyorsanız okuma [kapasite planlaması konuları](site-recovery-plan-capacity-vmware.md). Çalıştırma [dağıtım Planlayıcısı aracını](site-recovery-deployment-planner.md) VMWare çoğaltması için.

## <a name="download-the-template"></a>Şablonu indirme

1. Kasada **Altyapıyı Hazırlama** > **Kaynak** seçeneğine gidin.
2. **Kaynağı hazırla** bölümünde **+Yapılandırma sunucusu**’nu seçin.
3. **Sunucu Ekle** bölümünde **Sunucu türü**’nde **VMware için yapılandırma sunucusu**’nun görüntülenip görüntülenmediğini kontrol edin.
4. Yapılandırma sunucusu için Open Virtualization uygulaması (OVA) şablonunu indirin.

   > [!TIP]
   >Ayrıca doğrudan yapılandırma sunucusu şablonunun en son sürümünü indirebilirsiniz [Microsoft Download Center](https://aka.ms/asrconfigurationserver).

> [!NOTE]
> OVA şablonu ile sağlanan lisans 180 gün boyunca geçerli bir değerlendirme lisanstır. POST bu dönem, tedarik edilen lisansa sahip windows etkinleştirmek müşteri gerekir.

## <a name="import-the-template-in-vmware"></a>VMware’de şablonu içeri aktarma

1. VMWare vSphere İstemcisi’ni kullanarak VMware vCenter sunucusunda veya vSphere ESXi konağında oturum açın.
2. **Dosya** menüsünde **OVF Şablonunu Dağıt** seçeneğini belirleyerek OVF Şablonu Dağıtma sihirbazını başlatın.

     ![OVF şablonu](./media/vmware-azure-deploy-configuration-server/vcenter-wizard.png)

3. İçinde **Kaynak Seç**, indirilen OVF'nin konumunu girin.
4. İçinde **gözden geçirme ayrıntıları**seçin **sonraki**.
5. İçinde **ad ve klasör seçin** ve **yapılandırma seçin**, varsayılan ayarları kabul edin.
6. **Depolama seçin** bölümünde en iyi performans için **Sanal disk biçimini seçin** bölümden **Thick Provision Eager Zeroed** seçeneğini belirleyin. Ölçülü kaynak sağlama seçeneği kullanımı, yapılandırma sunucusunun performansını etkileyebilir.
7. Kalan sihirbaz sayfalarında varsayılan ayarları kabul edin.
8. **Tamamlanmaya hazır** bölümünde:

    * VM’yi varsayılan ayarlarla kurmak için **Dağıtımdan sonra aç** > **Son** seçeneğini belirleyin.

    * Ek bir ağ arabirimi eklemek, temizlemek **dağıtımdan sonra Aç**ve ardından **son**. Varsayılan olarak, yapılandırma sunucusu şablonu tek bir NIC ile dağıtılır. Dağıtımdan sonra daha fazla NIC ekleyebilirsiniz.

> [!IMPORTANT]
> Dağıtımdan sonra kaynak yapılandırmaları (bellek/Çekirdek/CPU sınırlama), hizmetleri değiştirmek/yüklü silin veya yapılandırma sunucusundaki dosyaları değiştirmeyin. Bu, Azure hizmetleriyle yapılandırma sunucusunun kaydı ve yapılandırma sunucusunun performansını etkiler.

## <a name="add-an-additional-adapter"></a>Ek bağdaştırıcı ekleme

Yapılandırma sunucusuna Ek NIC eklemek istiyorsanız, sunucuyu kasaya kaydetmeden önce ekleyin. Kayıt işleminden sonra ek sunucu eklenemez.

1. vSphere Client envanterinde VM’ye sağ tıklayın ve **Ayarları Düzenle**’yi seçin.
2. **Donanım** bölümünde **Ekle** > **Ethernet Bağdaştırıcısı** seçeneğini belirleyin. Sonra **İleri**’yi seçin.
3. Bir bağdaştırıcı türü ve ağ seçin.
4. VM açıldığında sanal NIC'ye bağlanmak için seçin **Connect güç açma sırasında**. Ardından **sonraki** > **son** > **Tamam**.

## <a name="register-the-configuration-server-with-azure-site-recovery-services"></a>Azure Site Recovery hizmetleriyle yapılandırma sunucusuna kaydedin

1. VMWare vSphere Client konsolundan VM’yi açın.
2. VM’de Windows Server 2016 yükleme deneyimi önyüklemesi yapılır. Lisans sözleşmesini kabul edin ve bir yönetici parolası girin.
3. Yükleme tamamlandıktan sonra VM’de yönetici olarak oturum açın.
4. İlk kez oturum açarken, birkaç saniye içinde Azure Site Recovery Configuration Tool başlatılır.
5. Yapılandırma sunucusunu Site Recovery’ye kaydetmek için kullanılacak bir ad girin. Sonra **İleri**’yi seçin.
6. Araç, VM’nin Azure bağlanıp bağlanamadığını denetler. Bağlantı kurulduktan sonra Azure aboneliğinizde oturum açmak için **Oturum aç** seçeneğini belirleyin.</br>
    a. Kimlik bilgilerinin, yapılandırma sunucusunu kaydetmek istediğiniz kasaya erişim izni olmalıdır.</br>
    b. Seçilen kullanıcı hesabı, Azure'da uygulama oluşturma izni olduğundan emin olun. Gerekli izinleri etkinleştirmek için verilen yönergeleri izleyin [burada](#azure-active-directory-permission-requirements).
7. Araç birkaç yapılandırma görevi gerçekleştirir ve yeniden başlatır.
8. Makinede tekrar oturum açın. Yapılandırma sunucusu yönetim Sihirbazı başlar **otomatik olarak** birkaç saniye içinde.

### <a name="configure-settings"></a>Ayarları yapılandırma

1. Yapılandırma sunucusu yönetim Sihirbazı'nda seçin **bağlantı kurma**. Pencerelerden ilk Keşif ve anında iletme kaynak makinede mobility hizmeti yüklemesi için yerleşik bir işlem sunucusunu kullanan NIC'yi seçin ve ardından Azure ile bağlantı için yapılandırma sunucusunu kullanan NIC'yi seçin. Daha sonra **Kaydet**’e tıklayın. Yapılandırıldıktan sonra bu ayarı değiştiremezsiniz. Yapılandırma sunucusunun IP adresini değiştirmek için değil kesinlikle önerilir. STATİK IP ve DHCP IP yapılandırma sunucusuna atanan IP olduğundan emin olun.
2. İçinde **kurtarma Hizmetleri kasasını seçin**, Microsoft Azure'da oturum aç için kullanılan kimlik bilgileriyle **6. adım** , "[kayıt yapılandırma sunucusu Azure Site kurtarma Hizmetleri ile](#register-the-configuration-server-with-azure-site-recovery-services)" .
3. Oturum açma işleminden sonra Azure aboneliğinizi ve ilgili kaynak grubu ve kasayı seçin.

    > [!NOTE]
    > Kaydedildikten sonra kurtarma Hizmetleri kasası değiştirmek için hiçbir esneklik yoktur.
    > Değişen kurtarma Hizmetleri kasası geçerli kasasından yapılandırma sunucusunun ilişkisi kaldırma gerektirecek ve yapılandırma sunucusu altında tüm korumalı sanal makinelerin çoğaltma durdurulur. [Daha fazla bilgi](vmware-azure-manage-configuration-server.md#register-a-configuration-server-with-a-different-vault) edinin.

4. İçinde **üçüncü taraf yazılım Yükle**,

    |Senaryo   |İzlemeniz gereken adımlar  |
    |---------|---------|
    |İndirmek & MySQL el ile yükleme?     |  Evet. MySQL uygulama indirme ve bu klasöre yerleştirin **C:\Temp\ASRSetup**, el ile yükleyin. Şimdi ne zaman kabul etmiş olursunuz. koşulları > tıklayarak **indirme ve yükleme**, portalı söylüyor *zaten yüklü*. Sonraki adıma geçebilirsiniz.       |
    |MySQL çevrimiçi indirilmesini engelleyebilirim?     |   Evet. MySQL yükleyici uygulamanızı klasörüne yerleştirin **C:\Temp\ASRSetup**. Koşullarını kabul edin > tıklayarak **indirme ve yükleme**, portal sizin tarafınızdan eklenen yükleyici kullanır ve uygulama yüklenir. Sonraki adım post yüklemeye devam edebilirsiniz.    |
    |İndirme ve Azure Site Recovery aracılığıyla MySQL yükleme yapmak istiyorum     |  Lisans sözleşmesini kabul & tıklayın **yükleyip**. Sonraki adım post yüklemeye devam edebilirsiniz.       |

5. **Gereç yapılandırmasını doğrulama** bölümünde ön koşullar, siz devam etmeden önce doğrulanır.
6. **vCenter Server/vSphere ESXi sunucusu yapılandırma** bölümünde, çoğaltmak istediğiniz VM’lerin bulunduğu vCenter sunucusunun veya vSphere konağının FQDN’sini ya da IP adresini girin. Sunucunun dinleme gerçekleştirdiği bağlantı noktasını girin. Kasadaki VMware sunucusu için kullanılacak bir kolay ad girin.
7. VMware sunucusu ile bağlantı için yapılandırma sunucusu tarafından kullanılacak kimlik bilgilerini girin. Site Recovery, bu kimlik bilgilerini çoğaltma için kullanılabilen VMware VM’lerini otomatik olarak bulmak üzere kullanır. Seçin **ekleme**, ardından **devam**. Buraya girdiğiniz kimlik bilgileri yerel olarak kaydedilir.
8. İçinde **sanal makine kimlik bilgilerini yapılandırma**, kullanıcı adı ve sanal makinelerin otomatik olarak çoğaltma sırasında Mobility hizmetini yüklemek için parolayı girin. İçin **Windows** makineler, hesabın, çoğaltmak istediğiniz makinelerde yerel yönetici ayrıcalıkları gerekir. İçin **Linux**, kök hesap için ayrıntıları sağlayın.
9. Kaydı tamamlamak için **Yapılandırmayı son haline getir** seçeneğini belirleyin.
10. Kayıt tamamlandıktan sonra Azure portalını açın, yapılandırma sunucusunun ve VMware sunucusunun listelendiğini doğrulayın **kurtarma Hizmetleri kasası** > **Yönet**  >  **Site Recovery altyapısı** > **yapılandırma sunucusu**.

## <a name="upgrade-the-configuration-server"></a>Yapılandırma sunucusunu yükseltme

Yapılandırma sunucusunu en son sürüme yükseltmek için aşağıdaki adımları [adımları](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server). Tüm Site Recovery bileşenlerini yükseltme hakkında ayrıntılı yönergeler için tıklayın [burada](service-updates-how-to.md).

## <a name="manage-the-configuration-server"></a>Yapılandırma sunucusunu yönetme

Devam eden çoğaltma kesintileri önlemek için yapılandırma sunucusunu bir kasaya kaydedildikten sonra yapılandırma sunucusunun IP adresi değişmez emin olun. Genel yapılandırma sunucusu yönetim görevleri hakkında daha fazla bilgi [burada](vmware-azure-manage-configuration-server.md).

## <a name="faq"></a>SSS

1. Ne kadar süreyle OVF dağıtılan yapılandırma sunucusunda sağlanan lisans geçerli değil mi? Lisans etkinleştirilmiyor ne olur?

    OVA şablonu ile sağlanan lisans 180 gün boyunca geçerli bir değerlendirme lisanstır. Geçerlilik süresi dolmadan lisans etkinleştirmeniz gerekir. Aksi takdirde, bu yapılandırma sunucusunun sık kapatma neden ve bu nedenle çoğaltma etkinliklere hinderance neden.

2. Yapılandırma sunucusu, farklı amaçlara yönelik yüklendiği VM kullanabilir miyim?

    **Hayır**, yapılandırma sunucusunun amacı VM kullanmanızı öneririz. Belirtilen tüm belirtimleri izleyin olun [önkoşulları](#prerequisites) verimli olağanüstü durum kurtarma yönetimi için.
3. Yeni oluşturulan bir kasa yapılandırma sunucusuyla zaten kayıtlı kasa geçiş yapabilir miyim?

    **Hayır**, yapılandırma sunucusu ile bir kasaya kaydedildikten sonra değiştirilemez.
4. Aynı yapılandırma sunucusuna hem fiziksel hem de sanal makineleri korumak için kullanabilir miyim?

    **Evet**, aynı yapılandırma sunucusuna, fiziksel ve sanal makineleri çoğaltmak için kullanılabilir. Ancak, fiziksel makine başarısız geriye yalnızca bir VMware VM.
5. Yapılandırma sunucusunun amacı ve kullanıldığı nedir?

    Başvurmak [Vmware'den Azure'a çoğaltma mimarisi](vmware-azure-architecture.md) yapılandırma sunucusu ve onun işlevler hakkında daha fazla bilgi edinmek için.
6. Yapılandırma sunucusunun en son sürümünü nereden bulabilirim?

    Portal üzerinden yapılandırma sunucusunu yükseltmek adımlar için bkz: [yapılandırma sunucusunu yükseltme](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server). Tüm Site Recovery bileşenlerini yükseltme hakkında ayrıntılı yönergeler için bkz [burada](https://aka.ms/asr_how_to_upgrade).
7. Yapılandırma sunucusu için parola nereden indirebilirim?

    Başvurmak [bu makalede](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase) parola indirilemedi.
8. Parola değiştirebilirim?

    **Hayır**, işiniz **parolayı değiştiremez önemle önerilir** yapılandırma sunucusu. Parola değişikliği korunan makinelerin çoğaltılması keser ve kritik sistem durumuna yol açıyor.
9. Kasa kayıt anahtarlarının nereden indirebilirim?

    İçinde **kurtarma Hizmetleri kasası**, **yönetme** > **Site Recovery altyapısı** > **yapılandırmasunucusu**. Sunucularda seçin **indirme kayıt anahtarı** kasa kimlik bilgileri dosyası indirilemedi.
10. Var olan bir yapılandırma sunucusu kopyalayın ve miyim çoğaltma düzenleme işlemi için kullanılmakta?

    **Hayır**, klonlanmış bir yapılandırma sunucusu bileşeni kullanımı desteklenmiyor. Genişleme işlem sunucusu bir kopyasını da desteklenmeyen bir senaryodur. Site Recovery kopyalama bileşenleri Süren çoğaltmaları etkiler.

11. Yapılandırma sunucusu IP'si değiştirebilirim?

    **Hayır**, yapılandırma sunucusunun IP adresini değiştirmek için kesinlikle önerilir. Yapılandırma sunucusuna atanan tüm IP'lere statik IP ve değil DHCP IP'ler olduğundan emin olun.
12. Yapılandırma sunucusu azure'da ayarlayabilir miyim?

    Yapılandırma sunucusu ile doğrudan satır görüş v-Center ile şirket içi ortamda ayarlama ve veri aktarımı gecikme süreleri en aza indirmek için önerilir. Zamanlanmış yedeklemeleri yapılandırma sunucusu için uygulayabileceğiniz [yeniden çalışma amacıyla](vmware-azure-manage-configuration-server.md#failback-requirements).

Yapılandırma sunucusu üzerinde daha fazla SSS için başvurmak bizim [configuration server sık sorulan sorular hakkındaki belgeleri](vmware-azure-common-questions.md#configuration-server) .

## <a name="troubleshoot-deployment-issues"></a>Dağıtım sorunlarını giderme

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]



## <a name="next-steps"></a>Sonraki adımlar

Olağanüstü durum kurtarma ayarlama [VMware Vm'lerini](vmware-azure-tutorial.md) azure'a.
