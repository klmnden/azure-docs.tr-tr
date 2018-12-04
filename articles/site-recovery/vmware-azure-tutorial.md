---
title: Azure Site Recovery ile şirket içi VMware VM’leri için Azure’da olağanüstü durum kurtarma ayarlama | Microsoft Docs
description: Azure Site Recovery ile şirket içi VMware VM’leri için Azure’da olağanüstü durum kurtarmayı nasıl ayarlayacağınızı öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 11/27/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 18675737dd03dd6c95a8f57a8f8bcdaed6c8b93d
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52837333"
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Şirket içi VMware VM’leri için Azure’da olağanüstü durum kurtarmayı ayarlama

[Azure Site Recovery](site-recovery-overview.md), planlı ve plansız kesintiler sırasında iş uygulamalarınızı çalışır durumda tutarak, iş sürekliliğinize ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur. Site Recovery, şirket içi makinelerin ve Azure sanal makinelerinin çoğaltma, yük devretme ve kurtarma gibi olağanüstü durum kurtarma işlemlerini yönetir ve düzenler.


Bu öğreticide, size Azure Site Recovery kullanarak bir VMware VM’sinin Azure’a çoğaltılmasını ayarlamayı ve etkinleştirmeyi göstereceğiz. Öğreticiler size Site Recovery’i temel ayarlarla nasıl dağıtacağınızı göstermek için tasarlanmıştır. En basit yolu kullanır ve tüm seçenekleri göstermezler. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Çoğaltma kaynağını ve hedefini girme.
> * Şirket içi Azure Site Recovery bileşenleri de dahil olmak üzere kaynak çoğaltma ortamını ve hedef çoğaltma ortamını ayarlayın.
> * Bir çoğaltma ilkesi oluşturma.
> * Sanal makine için çoğaltmayı etkinleştirme.

## <a name="before-you-start"></a>Başlamadan önce

Başlamadan önce şunların yapılması yararlıdır:

- Bu olağanüstü durum kurtarma senaryosu için [mimariyi gözden geçirin](vmware-azure-architecture.md).
- VMware VM’leri için olağanüstü durum kurtarmayı ayarlama hakkında daha ayrıntılı bilgiler için aşağıdaki kaynakları gözden geçirin ve kullanın:
    - VMware için olağanüstü durum kurtarma hakkında [sık sorulan soruları okuyun](vmware-azure-common-questions.md).
    - VMware için desteklenen ve zorunlu bileşenleri [öğrenin](vmware-physical-azure-support-matrix.md).
-  VMware için tüm dağıtım seçeneklerini kapsayan ayrıntılı yönergeler için **Nasıl Yapılır kılavuzlarımızı** okuyun:
    - [Yineleme kaynağı](vmware-azure-set-up-source.md) ve [yapılandırma sunucusunu](vmware-azure-deploy-configuration-server.md) ayarlayın.
    - [Çoğaltma hedefini](vmware-azure-set-up-target.md) ayarlayın.
    - Bir [çoğaltma ilkesi](vmware-azure-set-up-replication.md) yapılandırın ve [çoğaltmayı etkinleştirin](vmware-azure-enable-replication.md).


## <a name="select-a-protection-goal"></a>Koruma hedefi seçme

1. **Kurtarma Hizmetleri kasaları** bölümünde kasa adını seçin. Bu senaryo için**ContosoVMVault**’u kullanıyoruz.
2. **Başlarken** bölümünde Site Recovery’yi seçin. Ardından **Altyapıyı Hazırla** seçeneğini belirleyin.
3. **Koruma hedefi** > **Makineleriniz nerede** bölümünde **Şirket içi** seçeneğini belirleyin.
4. **Makinelerinizi nereye çoğaltmak istiyorsunuz** bölümünde **Azure’a** seçeneğini belirleyin.
5. **Makineleriniz sanallaştırıldı mı** bölümünde **Evet, VMware vSphere Hypervisor ile** seçeneğini belirleyin. Sonra **Tamam**’ı seçin.


## <a name="plan-your-deployment"></a>Dağıtımınızı planlama

Bu öğreticide size tek bir VM’yi çoğaltmayı göstereceğiz ve **Dağıtım Planlama**’da **Evet, yaptım** seçeneğini belirleyeceğiz. Birden çok VM dağıtıyorsanız bu adımı atlamamanızı öneririz. Size yardımcı olmak için [Dağıtım Planlayıcısı Aracı](https://aka.ms/asr-deployment-planner)’nı sağlarız. Bu araç hakkında [daha fazla bilgi edinin](site-recovery-deployment-planner.md).

## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

İlk dağıtım adımı olarak, kaynak ortamınızı ayarlarsınız. Şirket içi Site Recovery bileşenlerini barındıracak, yüksek kullanılabilirliğe sahip tek bir şirket içi makineye ihtiyacınız vardır. Bileşenler yapılandırma sunucusunu, işlem sunucusunu ve ana hedef sunucuyu içerir:

- Yapılandırma sunucusu, şirket içi bileşenler ile Azure arasındaki iletişimleri koordine eder ve veri çoğaltma işlemini yönetir.
- İşlem sunucusu, çoğaltma ağ geçidi olarak davranır. Çoğaltma verilerini alıp bunları önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir ve Azure depolamaya gönderir. İşlem sunucusu aynı zamanda çoğaltmak istediğiniz VM’lere Mobility Hizmetini yükler ve şirket içi VMware VM’lerinin otomatik olarak bulunmasını sağlar.
- Ana hedef sunucu, Azure'dan yeniden çalışma sırasında çoğaltma verilerini işler.

Yapılandırma sunucusunu yüksek kullanılabilirliğe sahip bir VMware VM olarak ayarlamak için, hazırlanmış bir Open Virtualization Application (OVA) şablonu indirin ve VM’yi oluşturmak üzere şablonu VMware’e aktarın. Yapılandırma sunucusunu ayarladıktan sonra kasaya kaydedin. Kayıt işleminden sonra Site Recovery, şirket içi VMware VM’lerini bulur.

> [!TIP]
> Bu öğreticide, yapılandırma sunucusu VMware sanal makinesi oluşturmak için bir OVA şablonu kullanılmaktadır. Bunu yapamıyorsanız, [yapılandırma sunucusunu el ile ayarlayın](physical-manage-configuration-server.md).

> [!TIP]
> Bu öğreticide, Site Recovery yapılandırma sunucusuna MySQL indirir ve yükler. Site Recovery’nin bunu yapmasını istemiyorsanız, el ile ayarlayabilirsiniz. [Daha fazla bilgi edinin](vmware-azure-deploy-configuration-server.md#configure-settings).


### <a name="download-the-vm-template"></a>VM şablonunu indirme

1. Kasada **Altyapıyı Hazırlama** > **Kaynak** seçeneğine gidin.
2. **Kaynağı hazırla** bölümünde **+Yapılandırma sunucusu**’nu seçin.
3. **Sunucu Ekle** bölümünde **Sunucu türü**’nde **VMware için yapılandırma sunucusu**’nun görüntülenip görüntülenmediğini kontrol edin.
4. Yapılandırma sunucusu için OVF şablonunu indirin.

 > [!TIP]
 >Yapılandırma sunucusu şablonunun en son sürümünü doğrudan [Microsoft Yükleme Merkezi](https://aka.ms/asrconfigurationserver)’nden indirebilirsiniz.

>[!NOTE]
OVF şablonuyla sunulan lisans, 180 günlük bir değerlendirme lisansıdır. Müşterinin Windows'u satın lisansla etkinleştirmesi gerekir.

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
  Bir NIC daha eklemek istiyorsanız **Dağıtımdan sonra aç** > **Son** seçeneğinin işaretini kaldırın. Varsayılan olarak, şablon tek bir NIC içerir. Dağıtımdan sonra daha fazla NIC ekleyebilirsiniz.

## <a name="add-an-additional-adapter"></a>Ek bağdaştırıcı ekleme

Yapılandırma sunucusuna bir NIC daha eklemek için, ekleme işlemini sunucuyu kasaya kaydetmeden önce gerçekleştirin. Kayıt işleminden sonra ek sunucu eklenemez.

1. vSphere Client envanterinde VM’ye sağ tıklayın ve **Ayarları Düzenle**’yi seçin.
2. **Donanım** bölümünde **Ekle** > **Ethernet Bağdaştırıcısı** seçeneğini belirleyin. Sonra **İleri**’yi seçin.
3. Bir bağdaştırıcı türü ve ağ seçin. 
4. VM açıldığında sanal NIC’ye bağlanmak için **Açıldığında bağlan**’ı seçin. **İleri** > **Son** seçeneğini belirleyin. Sonra **Tamam**’ı seçin.


## <a name="register-the-configuration-server"></a>Yapılandırma sunucusunu kaydetme 

1. VMWare vSphere Client konsolundan VM’yi açın.
2. VM’de Windows Server 2016 yükleme deneyimi önyüklemesi yapılır. Lisans sözleşmesini kabul edin ve bir yönetici parolası girin.
3. Yükleme tamamlandıktan sonra VM’de yönetici olarak oturum açın.
4. İlk oturum açma işleminizde Azure Site Recovery Yapılandırma Aracı birkaç saniye içinde başlatılır.
5. Yapılandırma sunucusunu Site Recovery’ye kaydetmek için kullanılacak bir ad girin. Sonra **İleri**’yi seçin.
6. Araç, VM’nin Azure bağlanıp bağlanamadığını denetler. Bağlantı kurulduktan sonra Azure aboneliğinizde oturum açmak için **Oturum aç** seçeneğini belirleyin. Kimlik bilgilerinin, yapılandırma sunucusunu kaydetmek istediğiniz kasaya erişim izni olmalıdır.
7. Araç bazı yapılandırma görevleri gerçekleştir ve sonra yeniden başlatılır.
8. Makinede tekrar oturum açın. Birkaç saniye içinde, Yapılandırma Sunucusu Yönetim Sihirbazı otomatik olarak başlar.

### <a name="configure-settings-and-add-the-vmware-server"></a>Ayarları yapılandırma ve VMware sunucusunu ekleme

1. Yapılandırma sunucusu yönetim sihirbazında **Bağlantı kurma** seçeneğini belirleyip, VM’lerden çoğaltma trafiğini almak için işlem sunucusunun kullandığı NIC’yi seçin. Daha sonra **Kaydet**’e tıklayın. Bu ayar yapılandırıldıktan sonra değiştirilemez.
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


Site Recovery, belirtilen ayarları kullanarak VMware sunucularına bağlanır ve VM’leri bulur.

> [!NOTE]
> Hesap adının portalda görünmesi 15 dakika veya daha fazla sürebilir. Hemen güncelleştirme yapmak için **Yapılandırma Sunucuları** > ***sunucu adı*** > **Sunucuyu Yenile** seçeneğini belirleyin.

## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Hedef kaynaklarını seçin ve doğrulayın.

1. **Altyapıyı hazırla** > **Hedef** seçeneğini belirleyin. Kullanmak istediğiniz Azure aboneliğini seçin. Bir Kaynak Yöneticisi modeli kullanıyoruz.
2. Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler. Bu öğretici serisindeki [ilk öğreticide](tutorial-prepare-azure.md) Azure bileşenlerini ayarladığınızda bunları edinmeniz gerekir.

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

Çoğaltmayı etkinleştirme şu şekilde gerçekleştirilebilir:

1. **Uygulamayı çoğalt** > **Kaynak** seçeneğini belirleyin.
2. **Kaynak** içinde, **Şirket içi**’ni seçin ve **Kaynak konumu** içinde yapılandırma sunucusunu seçin.
3. **Makine türü** bölümünde **Sanal Makineler**’i seçin.
4. **vCenter/vSphere Hypervisor** bölümünde vSphere konağını veya konağı yöneten vCenter sunucusunu seçin.
5. İşlem sunucusunu seçin (varsayılan olarak yapılandırma sunucusu VM’sine yüklenir). Sonra **Tamam**’ı seçin.
6. **Hedef** bölümünde, yükü devredilen VM’leri oluşturmak istediğiniz aboneliği ve kaynak grubunu seçin. Kaynak Yöneticisi dağıtım modelini kullanacağız. 
7. Verileri çoğaltmak için kullanmak istediğiniz Azure hesabını ve yük devretme işleminden sonra oluşturulan Azure VM’lerin bağlandığı Azure ağını ve alt ağını seçin.
8. Çoğaltmayı etkinleştirdiğiniz tüm VM’lere ağ ayarını uygulamak için **Seçili makineler için şimdi yapılandır**’ı seçin. Makineler için Azure ağını ayrı ayrı seçmek için **Daha sonra yapılandır**'ı seçin.
9. **Sanal Makineler** > **Sanal makineleri seçin** bölümünde, çoğaltmak istediğiniz her makineyi seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. Sonra **Tamam**’ı seçin.
10. **Özellikler** > **Özellikleri yapılandırma** bölümünde, Mobility Hizmeti’nin makineye otomatik olarak yüklenmesi için işlem sunucusu tarafından kullanılacak hesabı seçin.
11. **Çoğaltma ayarları** > **Çoğaltma ayarlarını yapılandırma** bölümünde doğru çoğaltma ilkesinin seçilip seçilmediğini doğrulayın.
12. **Çoğaltmayı Etkinleştir** seçeneğini belirleyin. Bir VM için çoğaltma etkinleştirildiğinde Site Recovery, Mobility Hizmeti’ni yükler.
13. **Ayarlar** > **İşler** > **Site Recovery İşleri** bölümünden **Korumayı Etkinleştir** işinin ilerleme durumunu izleyebilirsiniz. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.
- Değişikliklerin geçerli olması ve portalda görüntülenmesi 15 dakika veya daha uzun sürebilir.
- Eklediğiniz VM’leri izlemek için **Configuration Servers** > **Last Contact At** bölümünde VM’lerin son bulunma zamanını kontrol edin. Zamanlanan bulma işlemini beklemeden VM’leri eklemek için yapılandırma sunucusunu vurgulayın (seçmeyin) ve **Yenile**’yi seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma tatbikatı çalıştırma](site-recovery-test-failover-to-azure.md)
