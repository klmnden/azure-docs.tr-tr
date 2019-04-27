---
title: Azure Site Recovery ile şirket içi VMware VM’leri için Azure’da olağanüstü durum kurtarma ayarlama | Microsoft Docs
description: Azure Site Recovery ile şirket içi VMware VM’leri için Azure’da olağanüstü durum kurtarmayı nasıl ayarlayacağınızı öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 4/08/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 9e8f450825b7b4ad0402b8976d68bc23c18ce855
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60565672"
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Şirket içi VMware VM’leri için Azure’da olağanüstü durum kurtarmayı ayarlama

Bu makalede aracılığıyla Azure'a olağanüstü durum kurtarma için şirket içi VMware Vm'leri için çoğaltmayı etkinleştirme [Azure Site Recovery](site-recovery-overview.md) hizmeti.

Bu, şirket içi VMware Vm'leri için Azure'da olağanüstü durum kurtarma ayarlama gösteren serideki üçüncü öğreticidir. Önceki öğreticide, biz [şirket içi VMware ortamınızı hazırladığınız](vmware-azure-tutorial-prepare-on-premises.md) azure'a olağanüstü durum kurtarma için.


Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Kaynak çoğaltma ayarları ve şirket içi Site Recovery yapılandırma sunucusu ayarlarsınız.
> * Çoğaltma hedef ayarlarını yapın.
> * Bir çoğaltma ilkesi oluşturma.
> * Bir VMware sanal makine için çoğaltmayı etkinleştirin.

> [!NOTE]
> Öğreticiler bir senaryo için en basit dağıtım yolu gösterir. Mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı yönergeler için Site Recovery İçindekiler bölümünde nasıl yapılır makalesine gözden geçirin.

## <a name="before-you-start"></a>Başlamadan önce

Önceki öğreticilerde tamamlayın:
1. Seçtiğiniz emin [Azure yedekleme](tutorial-prepare-azure.md) Azure'da şirket içi VMware olağanüstü durum kurtarma için.
2. İzleyin [adımları](vmware-azure-tutorial-prepare-on-premises.md) şirket içi VMware dağıtım azure'a olağanüstü durum kurtarma için hazırlamak için.
3. Bu öğreticide size, tek bir VM'yi çoğaltma gösterilmektedir. Birden çok VMware Vm'sini dağıtıyorsanız, kullanmanız gereken [dağıtım Planlayıcısı aracını](https://aka.ms/asr-deployment-planner). Bu araç hakkında [daha fazla bilgi edinin](site-recovery-deployment-planner.md).
4. Bu öğreticide bir dizi seçenek farklı yapmak isteyebilirsiniz:
    - Öğreticide OVA şablon yapılandırma sunucusu VMware VM'sini oluşturmak için kullanılır. Bazı nedenlerden dolayı bu yapamazsanız, izleyin [bu yönergeleri](physical-manage-configuration-server.md) el ile yapılandırma sunucusu ayarlanamıyor.
    - Bu öğreticide, Site Recovery otomatik olarak indirir ve MySQL yapılandırma sunucusuna yükler. İsterseniz, bunu el ile bunun yerine ayarlayabilirsiniz. [Daha fazla bilgi edinin](vmware-azure-deploy-configuration-server.md#configure-settings).




## <a name="select-a-protection-goal"></a>Koruma hedefi seçme

1. **Kurtarma Hizmetleri kasaları** bölümünde kasa adını seçin. Bu senaryo için**ContosoVMVault**’u kullanıyoruz.
2. **Başlarken** bölümünde Site Recovery’yi seçin. Ardından **Altyapıyı Hazırla** seçeneğini belirleyin.
3. **Koruma hedefi** > **Makineleriniz nerede** bölümünde **Şirket içi** seçeneğini belirleyin.
4. **Makinelerinizi nereye çoğaltmak istiyorsunuz** bölümünde **Azure’a** seçeneğini belirleyin.
5. **Makineleriniz sanallaştırıldı mı** bölümünde **Evet, VMware vSphere Hypervisor ile** seçeneğini belirleyin. Sonra **Tamam**’ı seçin.



## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Kaynak ortamınız bu şirket içi Site Recovery bileşenlerini barındıracak bir tek, yüksek oranda kullanılabilir şirket içi makine gerekir:

- **Yapılandırma sunucusu**: Yapılandırma sunucusu yerinde bileşenler ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir.
- **İşlem sunucusu**: İşlem sunucusu, çoğaltma ağ geçidi olarak davranır. Bu çoğaltma verilerini alıp; önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir ve Azure önbellek depolama hesabına gönderir. İşlem sunucusu, çoğaltmak istediğiniz Vm'lere Mobility hizmeti aracısı ayrıca yükler ve şirket içi VMware Vm'lerini otomatik olarak bulunmasını gerçekleştirir.
- **Ana hedef sunucusu**: Ana hedef sunucu, Azure'dan yeniden çalışma sırasında çoğaltma verilerini işler.


Bu bileşenlerin tümünü birlikte olarak bilinen tek şirket içi makinelere yüklenen *yapılandırma sunucusu*. VMware olağanüstü durum kurtarma için varsayılan yapılandırma sunucusunu yüksek oranda kullanılabilir bir VMware VM olarak ayarladık. Bunu yapmak için hazırlanmış bir Open Virtualization uygulaması (OVA) şablonu indirin ve şablonu bir VM oluşturmak için Vmware'e aktarın. 

- Yapılandırma sunucusunu en son sürümünü portalda kullanılabilir. Ayrıca doğrudan indirebilirsiniz [Microsoft Download Center](https://aka.ms/asrconfigurationserver).
- Herhangi bir nedenden dolayı OVA şablon bir VM ayarlama için kullanamazsınız, izleyin [bu yönergeleri](physical-manage-configuration-server.md) el ile yapılandırma sunucusu ayarlanamıyor.
- OVF şablonu ile sağlanan lisans 180 gün boyunca geçerli bir değerlendirme lisanstır. Windows VM'de çalışan gerekli lisansa etkinleştirilmiş olması gerekir. 


### <a name="download-the-vm-template"></a>VM şablonunu indirme

1. Kasada **Altyapıyı Hazırlama** > **Kaynak** seçeneğine gidin.
2. **Kaynağı hazırla** bölümünde **+Yapılandırma sunucusu**’nu seçin.
3. **Sunucu Ekle** bölümünde **Sunucu türü**’nde **VMware için yapılandırma sunucusu**’nun görüntülenip görüntülenmediğini kontrol edin.
4. Yapılandırma sunucusu için OVF şablonunu indirin.



## <a name="import-the-template-in-vmware"></a>VMware’de şablonu içeri aktarma


1. VMWare vSphere İstemcisi ile VMware vCenter sunucusunda veya vSphere ESXi konağında oturum açın.
2. **Dosya** menüsünde **OVF Şablonunu Dağıt** seçeneğini belirleyerek **OVF Şablonu Dağıtma Sihirbazı**’nı başlatın. 

     ![OVF şablonu](./media/vmware-azure-tutorial/vcenter-wizard.png)

3. **Kaynak seçin** bölümünde, indirilen OVF’nin konumunu girin.
4. **Değerlendirme ayrıntıları** bölümünde **İleri** seçeneğini belirleyin.
5. **Ad ve klasör seçin** ve **Yapılandırma seçin** bölümünde varsayılan ayarları kabul edin.
6. **Select storage** bölümünde, en iyi performans için **Select virtual disk format** bölümünden **Thick Provision Eager Zeroed** seçeneğini belirleyin.
7. Sihirbaz sayfalarının geri kalan kısmında varsayılan ayarları kabul edin.
8. **Tamamlanmak için hazır** durumunda, VM’yi varsayılan ayarlarla kurmak için **Dağıtımdan sonra aç** > **Son** seçeneğini belirleyin.

   > [!TIP]
   > Bir NIC daha eklemek istiyorsanız **Dağıtımdan sonra aç** > **Son** seçeneğinin işaretini kaldırın. Varsayılan olarak, şablon tek bir NIC içerir. Dağıtımdan sonra daha fazla NIC ekleyebilirsiniz.

## <a name="add-an-additional-adapter"></a>Ek bağdaştırıcı ekleme

Yapılandırma sunucusuna Ek NIC eklemek istiyorsanız, sunucuyu kasaya kaydetmeden önce ekleyin. Kayıt işleminden sonra ek sunucu eklenemez.

1. vSphere Client envanterinde VM’ye sağ tıklayın ve **Ayarları Düzenle**’yi seçin.
2. **Donanım** bölümünde **Ekle** > **Ethernet Bağdaştırıcısı** seçeneğini belirleyin. Sonra **İleri**’yi seçin.
3. Bir bağdaştırıcı türü ve ağ seçin. 
4. VM açıldığında sanal NIC’ye bağlanmak için **Açıldığında bağlan**’ı seçin. **İleri** > **Son** seçeneğini belirleyin. Sonra **Tamam**’ı seçin.


## <a name="register-the-configuration-server"></a>Yapılandırma sunucusunu kaydetme 

Yapılandırma sunucusunu ayarladıktan sonra kasaya kaydedin.

1. VMWare vSphere Client konsolundan VM’yi açın.
2. VM’de Windows Server 2016 yükleme deneyimi önyüklemesi yapılır. Lisans sözleşmesini kabul edin ve bir yönetici parolası girin.
3. Yükleme tamamlandıktan sonra VM’de yönetici olarak oturum açın.
4. İlk oturum açma işleminizde Azure Site Recovery Yapılandırma Aracı birkaç saniye içinde başlatılır.
5. Yapılandırma sunucusunu Site Recovery’ye kaydetmek için kullanılacak bir ad girin. Sonra **İleri**’yi seçin.
6. Araç, VM’nin Azure bağlanıp bağlanamadığını denetler. Bağlantı kurulduktan sonra Azure aboneliğinizde oturum açmak için **Oturum aç** seçeneğini belirleyin. Kimlik bilgilerinin, yapılandırma sunucusunu kaydetmek istediğiniz kasaya erişim izni olmalıdır.
7. Araç bazı yapılandırma görevleri gerçekleştir ve sonra yeniden başlatılır.
8. Makinede tekrar oturum açın. Birkaç saniye içinde, Yapılandırma Sunucusu Yönetim Sihirbazı otomatik olarak başlar.


### <a name="configure-settings-and-add-the-vmware-server"></a>Ayarları yapılandırma ve VMware sunucusunu ekleme

Ayarlama ve yapılandırma sunucusunu kaydetme tamamlayın. 


1. Yapılandırma sunucusu yönetim Sihirbazı'nda seçin **bağlantı kurma**. Pencerelerden ilk Keşif ve anında iletme kaynak makinede mobility hizmeti yüklemesi için yerleşik bir işlem sunucusunu kullanan NIC'yi seçin ve ardından Azure ile bağlantı için yapılandırma sunucusunu kullanan NIC'yi seçin. Daha sonra **Kaydet**’e tıklayın. Yapılandırıldıktan sonra bu ayarı değiştiremezsiniz.
2. **Recovery Services kasasını seçin** bölümünde Azure aboneliğinizi, ilgili kaynak grubunu ve kasayı seçin.
3. **Üçüncü taraf yazılımı yükleyin** bölümünde lisans sözleşmesini kabul edin. MySQL Server’ı yüklemek için **İndir ve Yükle** seçeneğini belirleyin. Yola MySQL eklediyseniz bu adım atlanır.
4. **VMware PowerCLI’yi Yükle** seçeneğini belirleyin. Bunu yapmadan önce tüm tarayıcı pencerelerinin kapalı olduğundan emin olun. Daha sonra **Devam** seçeneğini belirleyin.
5. **Gereç yapılandırmasını doğrulama** bölümünde ön koşullar, siz devam etmeden önce doğrulanır.
6. **vCenter Server/vSphere ESXi sunucusu yapılandırma** bölümünde, çoğaltmak istediğiniz VM’lerin bulunduğu vCenter sunucusunun veya vSphere konağının FQDN’sini ya da IP adresini girin. Sunucunun dinleme gerçekleştirdiği bağlantı noktasını girin. Kasadaki VMware sunucusu için kullanılacak bir kolay ad girin.
7. VMware sunucusu ile bağlantı için yapılandırma sunucusu tarafından kullanılacak kullanıcı kimlik bilgilerini girin. Girdiğiniz kullanıcı adı ve parola bilgilerinin doğru ve korunacak sanal makinenin Yöneticiler grubuna ait olduğuna emin olun. Site Recovery, bu kimlik bilgilerini çoğaltma için kullanılabilen VMware VM’lerini otomatik olarak bulmak üzere kullanır. **Ekle** seçeneğini ve sonra **Devam** seçeneğini belirleyin.
8. **Sanal makine kimlik bilgilerini yapılandırma** bölümünde, çoğaltma etkinleştirildiğinde VM’lere Mobility Hizmetinin otomatik olarak yüklenmesi için kullanılacak kullanıcı adını ve parolayı girin.
    - Windows makinelerinde hesap için, çoğaltmak istediğiniz makinelerde yerel yönetici ayrıcalıkları gerekir.
    - Linux’ta kök hesap için bilgileri sağlayın.
9. Kaydı tamamlamak için **Yapılandırmayı son haline getir** seçeneğini belirleyin.
10. Kayıt bittikten sonra Azure portalında, yapılandırma sunucusunun ve VMware sunucusunun kasadaki **Kaynak** sayfasında listelendiğinden emin olun. Daha sonra hedef ayarlarını yapılandırmak için **Tamam**’ı seçin.


Yapılandırma sunucusunu kaydettikten sonra Site Recovery belirtilen ayarları kullanarak VMware sunucularına bağlanır ve Vm'leri bulur.

> [!NOTE]
> Hesap adının portalda görünmesi 15 dakika veya daha fazla sürebilir. Hemen güncelleştirme yapmak için **Yapılandırma Sunucuları** > ***sunucu adı*** > **Sunucuyu Yenile** seçeneğini belirleyin.

## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Hedef kaynaklarını seçin ve doğrulayın.

1. **Altyapıyı hazırla** > **Hedef** seçeneğini belirleyin. Kullanmak istediğiniz Azure aboneliğini seçin. Bir Kaynak Yöneticisi modeli kullanıyoruz.
2. Site Recovery, bir veya daha fazla sanal ağ olup olmadığını denetler. Bu öğretici serisindeki [ilk öğreticide](tutorial-prepare-azure.md) Azure bileşenlerini ayarladığınızda bunları edinmeniz gerekir.

   ![Hedef sekmesi](./media/vmware-azure-tutorial/storage-network.png)

## <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

1. [Azure portalını](https://portal.azure.com) açın ve **Tüm kaynaklar**’ı seçin.
2. Kurtarma Hizmetleri kasasını (bu öğreticide **ContosoVMVault**) seçin.
3. Bir çoğaltma ilkesi oluşturmak için **Site Recovery altyapısı** > **Çoğaltma İlkeleri** > **+Çoğaltma İlkesi** seçeneğini belirleyin.
4. **Çoğaltma ilkesi oluştur** seçeneğinde, ilke adını girin. **VMwareRepPolicy** ilkesini kullanıyoruz.
5. **RPO eşiği** bölümünde varsayılan 60 dakika seçeneğini kullanın. Bu değer kurtarma noktalarının hangi sıklıkta oluşturulacağını tanımlar. Devamlı çoğaltma bu sınırı aşarsa bir uyarı oluşturulur.
6. **Kurtarma noktası tutma** içinde, her bir kurtarma noktasının ne kadar tutulacağını belirtin. Bu öğretici için 72 saat değerini kullanıyoruz. Çoğaltılan VM’ler bir tutma penceresindeki herhangi bir noktaya kurtarılabilir.
7. **Uygulamayla tutarlı anlık görüntü sıklığı** içinde, uygulamayla tutarlı anlık görüntülerin oluşturulma sıklığını belirtin. Varsayılan 60 dakikayı kullanıyoruz. İlkeyi oluşturmak için **Tamam**’ı seçin.

   ![Çoğaltma ilkesi oluşturma](./media/vmware-azure-tutorial/replication-policy.png)

- İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir.
- Yeniden çalışma için varsayılan olarak, eşleşen bir ilke otomatik olarak oluşturulur. Örneğin, çoğaltma ilkesi **rep-policy** şeklindeyse yeniden çalışma ilkesi **rep-policy-failback** şeklindedir. Bu ilke Azure’dan bir yeniden çalışma başlatılana kadar kullanılmaz.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Şekilde, sanal makineler için çoğaltmayı etkinleştirin:

1. **Uygulamayı çoğalt** > **Kaynak** seçeneğini belirleyin.
1. **Kaynak** içinde, **Şirket içi**’ni seçin ve **Kaynak konumu** içinde yapılandırma sunucusunu seçin.
1. **Makine türü** bölümünde **Sanal Makineler**’i seçin.
1. **vCenter/vSphere Hypervisor** bölümünde vSphere konağını veya konağı yöneten vCenter sunucusunu seçin.
1. İşlem sunucusunu seçin (varsayılan olarak yapılandırma sunucusu VM’sine yüklenir). Sonra **Tamam**’ı seçin.
1. **Hedef** bölümünde, yükü devredilen VM’leri oluşturmak istediğiniz aboneliği ve kaynak grubunu seçin. Kaynak Yöneticisi dağıtım modelini kullanacağız. 
1. Yük devretme işleminden sonra oluşturulan Azure VM’lerin bağlandığı Azure ağını ve alt ağını seçin.
1. Çoğaltmayı etkinleştirdiğiniz tüm VM’lere ağ ayarını uygulamak için **Seçili makineler için şimdi yapılandır**’ı seçin. Makineler için Azure ağını ayrı ayrı seçmek için **Daha sonra yapılandır**'ı seçin.
1. **Sanal Makineler** > **Sanal makineleri seçin** bölümünde, çoğaltmak istediğiniz her makineyi seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. Sonra **Tamam**’ı seçin. Görünüm/herhangi belirli bir sanal makineyi seçmek mümkün değilse [daha fazla bilgi edinin](https://aka.ms/doc-plugin-VM-not-showing) sorunu çözme konusunda.
1. **Özellikler** > **Özellikleri yapılandırma** bölümünde, Mobility Hizmeti’nin makineye otomatik olarak yüklenmesi için işlem sunucusu tarafından kullanılacak hesabı seçin.
1. **Çoğaltma ayarları** > **Çoğaltma ayarlarını yapılandırma** bölümünde doğru çoğaltma ilkesinin seçilip seçilmediğini doğrulayın.
1. **Çoğaltmayı Etkinleştir** seçeneğini belirleyin. Bir VM için çoğaltma etkinleştirildiğinde Site Recovery, Mobility Hizmeti’ni yükler.
1. **Ayarlar** > **İşler** > **Site Recovery İşleri** bölümünden **Korumayı Etkinleştir** işinin ilerleme durumunu izleyebilirsiniz. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.
1. Değişikliklerin geçerli olması ve portalda görüntülenmesi 15 dakika veya daha uzun sürebilir.
1. Eklediğiniz VM’leri izlemek için **Configuration Servers** > **Last Contact At** bölümünde VM’lerin son bulunma zamanını kontrol edin. Zamanlanan bulma işlemini beklemeden VM’leri eklemek için yapılandırma sunucusunu vurgulayın (seçmeyin) ve **Yenile**’yi seçin.

## <a name="next-steps"></a>Sonraki adımlar
Çoğaltmayı etkinleştirdikten sonra her şeyin beklendiği gibi çalıştığından emin olmak için bir tatbikatı çalıştırma.
> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma tatbikatı çalıştırma](site-recovery-test-failover-to-azure.md)
