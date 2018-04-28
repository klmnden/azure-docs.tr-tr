---
title: Azure Site Recovery ile şirket içi VMware VM’leri için Azure’da olağanüstü durum kurtarma ayarlama | Microsoft Docs
description: Azure Site Recovery ile şirket içi VMware VM’leri için Azure’da olağanüstü durum kurtarma ayarlamayı öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 04/08/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 6c86a98dd819b91608be04f1466dc1e6764ee4b9
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Şirket içi VMware VM’leri için Azure’da olağanüstü durum kurtarmayı ayarlama

Bu öğreticide, Windows çalıştıran VMware VM’leri için Azure’da olağanüstü durum kurtarmanın nasıl ayarlanacağı gösterilmektedir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Çoğaltma kaynağı ve hedefini girin.
> * Kaynak çoğaltma ortamını, şirket içi Azure Site Recovery bileşenleri ve hedef çoğaltma ortamı dahil olmak üzere ayarlayın.
> * Bir çoğaltma ilkesi oluşturma.
> * Sanal makine için çoğaltmayı etkinleştirme.

Bu, serideki üçüncü öğreticidir. Bu öğreticide, önceki öğreticilerdeki görevleri tamamladığınız varsayılır:

* [Azure’ı hazırlama](tutorial-prepare-azure.md). Bu öğreticide, bir Azure depolama hesabını ve ağı ayarlama, Azure hesabınızın doğru izinlere sahip olduğundan emin olma ve Kurtarma Hizmetleri kasası oluşturma işlemlerinin nasıl yapılacağı açıklanmaktadır.
* [Şirket içi VMware’i hazırlama](vmware-azure-tutorial-prepare-on-premises.md). Bu öğreticide, Site Recovery’nin sanal makineleri bulmak ve sanal makineye yönelik çoğaltmayı etkinleştirdiğinizde Site Recovery Mobility hizmeti bileşenlerinin göndererek yüklemesini gerçekleştirmek için VMware sunucularına erişebilmesi için hesapları hazırlarsınız. Ayrıca VMware sunucularınızın ve sanal makinelerinizin Site Recovery gereksinimlerine uygun olduğundan da emin olursunuz.

Başlamadan önce olağanüstü durum kurtarma senaryoları için [mimariyi gözden geçirmeniz](vmware-azure-architecture.md) yararlı olabilir.


## <a name="select-a-replication-goal"></a>Çoğaltma hedefi seçme

1. **Kurtarma Hizmetleri kasalarında** **ContosoVMVault** kasa adını seçin.
2. **Başlarken** bölümünde Site Recovery’yi seçin. Daha sonra **Altyapıyı Hazırlama**’yı seçin.
3. **Koruma hedefi** > **Makineleriniz nerede** bölümünde **Şirket içi** seçeneğini belirleyin.
4. **Makinelerinizi nereye çoğaltmak istiyorsunuz** bölümünde **Azure’a** seçeneğini belirleyin.
5. **Makineleriniz sanallaştırıldı mı** bölümünde **Evet, VMware vSphere Hypervisor ile** seçeneğini belirleyin. Sonra **Tamam**’ı seçin.

## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama


Kaynak ortamı ayarlamak için, şirket içi Site Recovery bileşenlerini barındıracak, yüksek kullanılabilirliğe sahip tek bir şirket içi makineye ihtiyacınız vardır. Bileşenler yapılandırma sunucusunu, işlem sunucusunu ve ana hedef sunucuyu içerir:

- Yapılandırma sunucusu, şirket içi bileşenler ile Azure arasındaki iletişimleri koordine eder ve veri çoğaltma işlemini yönetir.
- İşlem sunucusu, çoğaltma ağ geçidi olarak davranır. Çoğaltma verilerini alıp bunları önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir ve Azure depolamaya gönderir. İşlem sunucusu aynı zamanda çoğaltmak istediğiniz VM’lere Mobility Hizmetini yükler ve şirket içi VMware VM’lerinin otomatik olarak bulunmasını sağlar.
- Ana hedef sunucu, Azure'dan yeniden çalışma sırasında çoğaltma verilerini işler.

Yapılandırma sunucusunu yüksek kullanılabilirliğe sahip bir VMware VM olarak ayarlamak için, hazırlanmış bir Open Virtualization Format (OVF) şablonu indirin ve VM’yi oluşturmak üzere şablonu VMware’e aktarın. Yapılandırma sunucusunu ayarladıktan sonra kasaya kaydedin. Kayıt işleminden sonra Site Recovery, şirket içi VMware VM’lerini bulur.

> [!TIP]
> Bu öğreticide, yapılandırma sunucusu VMware sanal makinesi oluşturmak için bir OVF şablonu kullanılmaktadır. Bunu yapamıyorsanız, [El ile kurulum](physical-manage-configuration-server.md) çalıştırabilirsiniz. 


### <a name="download-the-vm-template"></a>VM şablonunu indirme

1. Kasada **Altyapıyı Hazırlama** > **Kaynak** seçeneğine gidin.
2. **Kaynağı hazırla** bölümünde **+Yapılandırma sunucusu**’nu seçin.
3. **Sunucu Ekle** bölümünde **Sunucu türü**’nde **VMware için yapılandırma sunucusu**’nun görüntülenip görüntülenmediğini kontrol edin.
4. Yapılandırma sunucusu için OVF şablonunu indirin.

  > [!TIP]
  Yapılandırma sunucusu şablonunun en son sürümünü doğrudan [Microsoft Yükleme Merkezi](https://aka.ms/asrconfigurationserver)’nden indirebilirsiniz.

## <a name="import-the-template-in-vmware"></a>VMware’de şablonu içeri aktarma

1. VMWare vSphere İstemcisi’ni kullanarak VMware vCenter sunucusunda veya vSphere ESXi konağında oturum açın.
2. **Dosya** menüsünde **OVF Şablonunu Dağıt** seçeneğini belirleyerek OVF Şablonu Dağıtma sihirbazını başlatın. 

     ![OVF şablonu](./media/vmware-azure-tutorial/vcenter-wizard.png)

3. **Kaynak seçin** bölümünde, indirilen OVF’nin konumunu girin.
4. **Değerlendirme ayrıntıları** bölümünde **İleri** seçeneğini belirleyin.
5. **Ad ve klasör seçin** ve **Yapılandırma seçin** bölümünde varsayılan ayarları kabul edin.
6. **Select storage** bölümünde, en iyi performans için **Select virtual disk format** bölümünden **Thick Provision Eager Zeroed** seçeneğini belirleyin.
7. Sihirbaz sayfalarının geri kalan kısmında varsayılan ayarları kabul edin.
8. **Tamamlanmaya hazır** olduğunda:

    * VM’yi varsayılan ayarlarla kurmak için **Dağıtımdan sonra aç** > **Son** seçeneğini belirleyin.

    * Bir ağ arabirimi daha eklemek istiyorsanız **Dağıtımdan sonra aç** seçeneğinin işaretini kaldırın. Ardından **Son**’u seçin. Varsayılan olarak, yapılandırma sunucusu şablonu tek bir NIC ile dağıtılır. Dağıtımdan sonra daha fazla NIC ekleyebilirsiniz.

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
4. İlk oturum açma işleminizde Azure Site Recovery Yapılandırma Aracı başlar.
5. Yapılandırma sunucusunu Site Recovery’ye kaydetmek için kullanılacak bir ad girin. Sonra **İleri**’yi seçin.
6. Araç, VM’nin Azure bağlanıp bağlanamadığını denetler. Bağlantı kurulduktan sonra Azure aboneliğinizde oturum açmak için **Oturum aç** seçeneğini belirleyin. Kimlik bilgilerinin, yapılandırma sunucusunu kaydetmek istediğiniz kasaya erişim izni olmalıdır.
7. Araç bazı yapılandırma görevleri gerçekleştir ve sonra yeniden başlatılır.
8. Makinede tekrar oturum açın. Yapılandırma sunucusu yönetim sihirbazı otomatik olarak başlar.

### <a name="configure-settings-and-add-the-vmware-server"></a>Ayarları yapılandırma ve VMware sunucusunu ekleme

1. Yapılandırma sunucusu yönetim sihirbazında **Bağlantı kurma** seçeneğini belirleyip, çoğaltma trafiğini almak için NIC’yi seçin. Daha sonra **Kaydet**’e tıklayın. Bu ayar yapılandırıldıktan sonra değiştirilemez.
2. **Recovery Services kasasını seçin** bölümünde Azure aboneliğinizi, ilgili kaynak grubunu ve kasayı seçin.
3. **Üçüncü taraf yazılımı yükleyin** bölümünde lisans sözleşmesini kabul edin. MySQL Server’ı yüklemek için **İndir ve Yükle** seçeneğini belirleyin.
4. **VMware PowerCLI’yi Yükle** seçeneğini belirleyin. Bunu yapmadan önce tüm tarayıcı pencerelerinin kapalı olduğundan emin olun. Daha sonra **Devam** seçeneğini belirleyin.
5. **Gereç yapılandırmasını doğrulama** bölümünde ön koşullar, siz devam etmeden önce doğrulanır.
6. **vCenter Server/vSphere ESXi sunucusu yapılandırma** bölümünde, çoğaltmak istediğiniz VM’lerin bulunduğu vCenter sunucusunun veya vSphere konağının FQDN’sini ya da IP adresini girin. Sunucunun dinleme gerçekleştirdiği bağlantı noktasını girin. Kasadaki VMware sunucusu için kullanılacak bir kolay ad girin.
7. VMware sunucusu ile bağlantı için yapılandırma sunucusu tarafından kullanılacak kimlik bilgilerini girin. Site Recovery, bu kimlik bilgilerini çoğaltma için kullanılabilen VMware VM’lerini otomatik olarak bulmak üzere kullanır. **Ekle** seçeneğini ve sonra **Devam** seçeneğini belirleyin.
8. **Sanal makine kimlik bilgilerini yapılandırma** bölümünde, çoğaltma etkinleştirildiğinde makinelere Mobility Hizmetinin otomatik olarak yüklenmesi için kullanılacak kullanıcı adını ve parolayı girin. Windows makinelerinde hesap için, çoğaltmak istediğiniz makinelerde yerel yönetici ayrıcalıkları gerekir. Linux’ta kök hesap için bilgileri sağlayın.
9. Kaydı tamamlamak için **Yapılandırmayı son haline getir** seçeneğini belirleyin. 
10. Kayıt bittikten sonra Azure portalında, yapılandırma sunucusunun ve VMware sunucusunun kasadaki **Kaynak** sayfasında listelendiğinden emin olun. Daha sonra hedef ayarlarını yapılandırmak için **Tamam**’ı seçin.


Site Recovery, belirtilen ayarları kullanarak VMware sunucularına bağlanır ve VM’leri bulur.

> [!NOTE]
> Hesap adının portalda görünmesi 15 dakika veya daha fazla sürebilir. Hemen güncelleştirme yapmak için **Yapılandırma Sunucuları** > ***sunucu adı*** > **Sunucuyu Yenile** seçeneğini belirleyin.

## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Hedef kaynaklarını seçin ve doğrulayın.

1. **Altyapıyı hazırla** > **Hedef** seçeneğini belirleyin. Kullanmak istediğiniz Azure aboneliğini seçin.
2. Hedef dağıtım modelinizin Azure Resource Manager’a bağlı mı olacağını yoksa klasik mi olacağını belirtin.
3. Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.

   ![Hedef sekmesi](./media/vmware-azure-tutorial/storage-network.png)

## <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

1. [Azure portalını](https://portal.azure.com) açın ve **Tüm kaynaklar**’ı seçin.
2. **ContosoVMVault** adlı Kurtarma Hizmeti kasasını seçin.
3. Bir çoğaltma ilkesi oluşturmak için **Site Recovery altyapısı** > **Çoğaltma İlkeleri** > **+Çoğaltma İlkesi** seçeneğini belirleyin.
4. **Çoğaltma ilkesi oluştur** seçeneğinde, **VMwareRepPolicy** ilke adını girin.
5. **RPO eşiği** bölümünde varsayılan 60 dakika seçeneğini kullanın. Bu değer kurtarma noktalarının hangi sıklıkta oluşturulacağını tanımlar. Devamlı çoğaltma bu sınırı aşarsa bir uyarı oluşturulur.
6. **Kurtarma noktası bekletme** bölümünde her bir kurtarma noktasının bekletme aralığı için varsayılan 24 saat seçeneğini kullanın. Bu öğretici için 72 saat seçeneğini kullanın. Çoğaltılan VM’ler bir aralıktaki herhangi bir noktaya kurtarılabilir.
7. **Uygulamayla tutarlı anlık görüntü sıklığı** bölümünde uygulamayla tutarlı anlık görüntülerin oluşturulma sıklığı için varsayılan 60 dakika seçeneğini kullanın. İlkeyi oluşturmak için **Tamam**’ı seçin.

   ![Çoğaltma ilkesi oluşturma](./media/vmware-azure-tutorial/replication-policy.png)

İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir. Yeniden çalışma için varsayılan olarak, eşleşen bir ilke otomatik olarak oluşturulur. Örneğin, çoğaltma ilkesi **rep-policy** şeklindeyse yeniden çalışma ilkesi **rep-policy-failback** şeklindedir. Bu ilke Azure’dan bir yeniden çalışma başlatılana kadar kullanılmaz.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Bir VM için çoğaltma etkinleştirildiğinde Site Recovery, Mobility Hizmeti’ni yükler. Değişikliklerin geçerli olması ve portalda görüntülenmesi 15 dakika veya daha uzun sürebilir.

Aşağıda belirtilen şekilde çoğaltmayı etkinleştirin:

1. **Uygulamayı çoğalt** > **Kaynak** seçeneğini belirleyin.
2. **Kaynak** bölümünde yapılandırma sunucusunu seçin.
3. **Makine türü** bölümünde **Sanal Makineler**’i seçin.
4. **vCenter/vSphere Hypervisor** bölümünde vSphere konağını yöneten vCenter sunucusunu veya konağı seçin.
5. İşlem sunucusunu (yapılandırma sunucusu) seçin. Sonra **Tamam**’ı seçin.
6. **Hedef** bölümünde, yükü devredilen VM’leri oluşturmak istediğiniz aboneliği ve kaynak grubunu seçin. Yükü devredilen VM’ler için Azure (klasik veya Resource Manager) üzerinde kullanmak istediğiniz dağıtım modelini seçin.
7. Verileri çoğaltmak için kullanmak istediğiniz Azure depolama hesabını seçin.
8. Yük devretme işleminden sonra oluşturulan Azure VM’lerin bağlandığı Azure ağını ve alt ağını seçin.
9. Koruma için seçtiğiniz tüm makinelere ağ ayarını uygulamak için **Seçili makineler için şimdi yapılandır**’ı seçin. Makineler için Azure ağını ayrı ayrı seçmek için **Daha sonra yapılandır**'ı seçin.
10. **Sanal Makineler** > **Sanal makineleri seçin** bölümünde, çoğaltmak istediğiniz her makineyi seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. Sonra **Tamam**’ı seçin.
11. **Özellikler** > **Özellikleri yapılandırma** bölümünde, Mobility Hizmeti’nin makineye otomatik olarak yüklenmesi için işlem sunucusu tarafından kullanılacak hesabı seçin.
12. **Çoğaltma ayarları** > **Çoğaltma ayarlarını yapılandırma** bölümünde doğru çoğaltma ilkesinin seçilip seçilmediğini doğrulayın.
13. **Çoğaltmayı Etkinleştir** seçeneğini belirleyin.

**Ayarlar** > **İşler** > **Site Recovery İşleri** bölümünden **Korumayı Etkinleştir** işinin ilerleme durumunu izleyebilirsiniz. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.

Eklediğiniz VM’leri izlemek için **Configuration Servers** > **Last Contact At** bölümünde VM’lerin son bulunma zamanını kontrol edin. Zamanlanan bulma işlemini beklemeden VM’leri eklemek için yapılandırma sunucusunu vurgulayın (seçmeyin) ve **Yenile**’yi seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma tatbikatı çalıştırma](site-recovery-test-failover-to-azure.md)
