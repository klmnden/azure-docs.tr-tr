---
title: "Azure Site Recovery ile şirket içi VMware VM’leri için Azure’da olağanüstü durum kurtarma ayarlama | Microsoft Docs"
description: "Azure Site Recovery ile şirket içi VMware VM’leri için Azure’da olağanüstü durum kurtarma ayarlamayı öğrenin."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/15/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 3d9248d2501c7fea0492bad2687b6bdfb0b903e8
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2018
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Şirket içi VMware VM’leri için Azure’da olağanüstü durum kurtarmayı ayarlama

Bu öğreticide, Windows çalıştıran VMware VM’leri için Azure’da olağanüstü durum kurtarmanın nasıl ayarlanacağı gösterilmektedir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Çoğaltma kaynağını ve hedefi belirtin.
> * Kaynak çoğaltma ortamını, şirket içi Site Recovery bileşenleri ve hedef çoğaltma ortamı dahil olmak üzere ayarlayın.
> * Çoğaltma ilkesi oluşturma
> * VM için çoğaltmayı etkinleştirme

Bu, serideki üçüncü öğreticidir. Bu öğreticide, önceki öğreticilerdeki adımları zaten tamamladığınız varsayılır:

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
2. [Şirket içi VMware’leri hazırlama](tutorial-prepare-on-premises-vmware.md)

Başlamadan önce olağanüstü durum kurtarma senaryosu için [mimariyi gözden geçirmeniz](concepts-vmware-to-azure-architecture.md) yararlı olabilir.


## <a name="select-a-replication-goal"></a>Çoğaltma hedefi seçme

1. **Kurtarma Hizmetleri kasalarında** kasa adına, **ContosoVMVault**, tıklayın.
2. **Kullanmaya Başlama** bölümünde Site Recovery’ye tıklayın. Daha sonra **Altyapıyı Hazırla**’ya tıklayın.
3. **Koruma hedefi** > **Makineleriniz nerede** bölümünde **Şirket içi** seçeneğini belirleyin.
4. **Makinelerinizi nereye çoğaltmak istiyorsunuz** bölümünde **Azure’a** seçeneğini belirleyin.
5. **Makineleriniz sanallaştırıldı mı** bölümünde **Evet, VMware vSphere Hypervisor ile** seçeneğini belirleyin. Daha sonra, **Tamam**'a tıklayın.

## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Kaynak ortamı ayarlamak için, şirket içi Site Recovery bileşenlerini barındıracak yüksek kullanılabilirliğe sahip tek bir şirket içi makine gerekir. Bileşenler yapılandırma sunucusu, işlem sunucusu ve ana hedef sunucudur.

- Yapılandırma sunucusu yerinde bileşenler ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir.
- İşlem sunucusu, çoğaltma ağ geçidi olarak davranır. Çoğaltma verilerini alıp bu verileri önbelleğe alma, sıkıştırma ve şifreleme işlemleriyle iyileştirir ve Azure depolama alanına gönderir. Ayrıca işlem sunucusu, çoğaltmak istediğiniz VM’lere Mobility hizmetini yükler ve şirket içi VMware VM’lerini otomatik olarak bulur.
- Ana hedef sunucu, Azure'dan yeniden çalışma sırasında çoğaltma verilerini işler.

Yapılandırma sunucusunu yüksek kullanılabilirliğe sahip VMware VM’si olarak ayarlamak için hazır bir OVF şablonu indirin ve VM’yi oluşturmak üzere şablonu VMware’e aktarın. Yapılandırma sunucusunu ayarladıktan sonra kasaya kaydedin. Kayıt işleminden sonra Site Recovery, şirket içi VMware VM’lerini bulur.

### <a name="download-the-vm-template"></a>VM şablonunu indirme

1. Kasada **Altyapıyı Hazırlama** > **Kaynak** seçeneğine gidin.
2. **Kaynağı hazırla** bölümünde **+ Yapılandırma Sunucusu**’na tıklayın.
3. **Sunucu Ekle** bölümünde **Sunucu türü**’nde **VMware için yapılandırma sunucusu**’nun görüntülenip görüntülenmediğini kontrol edin.
4. Yapılandırma sunucusu için Open Virtualization Format (OVF) şablonunu indirin.

  > [!TIP]
  Yapılandırma sunucusu şablonunun en son sürümünü doğrudan [Microsoft Yükleme Merkezi](https://aka.ms/asrconfigurationserver)’nden indirebilirsiniz.

## <a name="import-the-template-in-vmware"></a>VMware’de şablonu içeri aktarma

1. VMWare vSphere Client kullanarak VMware vCenter sunucusunda veya vSphere ESXi konağında oturum açın.
2. OVF Şablonu Dağıtma sihirbazını başlatmak için **Dosya** menüsünden **OVF Şablonu Dağıt** seçeneğini belirleyin.  

     ![OVF şablonu](./media/tutorial-vmware-to-azure/vcenter-wizard.png)

3. **Kaynak seçin** bölümünde indirilen OVF’nin konumunu belirtin.
4. **Ayrıntıları gözden geçir** bölümünde **İleri**’ye tıklayın.
5. **Ad ve klasör seçin** ve **Yapılandırma seçin** bölümünde varsayılan ayarları kabul edin.
6. **Depolama seçin** bölümünde en iyi performans için **Sanal disk biçimini seçin** bölümden **Thick Provision Eager Zeroed** seçeneğini belirleyin.
4. Kalan sihirbaz sayfalarında varsayılan ayarları kabul edin.
5. **Tamamlanmaya hazır** bölümünde:
  - VM’yi varsayılan ayarlarla kurmak için **Dağıtımdan sonra aç** > **Son** seçeneğini belirleyin.
  - Ek bir ağ arabirimi eklemek isterseniz **Dağıtımdan sonra aç**’ın seçimini kaldırın ve **Son**’u seçin. Yapılandırma sunucusu şablonu varsayılan olarak tek bir NIC ile dağıtılır, ancak dağıtımdan sonra ek NIC’ler ekleyebilirsiniz.

  
## <a name="add-an-additional-adapter"></a>Ek bağdaştırıcı ekleme

Yapılandırma sunucusuna ek NIC eklemek isterseniz, bu işlemi sunucuyu kasaya kaydetmeden önce yapın. Kayıt işleminden sonra ek sunucu eklenemez.

1. vSphere Client envanterinde VM’ye sağ tıklayın ve **Ayarları Düzenle**’yi seçin.
2. **Donanım** bölümünde **Ekle** > **Ethernet Bağdaştırıcısı**’na tıklayın. Ardından **İleri**'ye tıklayın.
3. Bağdaştırıcı türü ve bir ağ seçin. 
4. VM açıldığında sanal NIC’ye bağlanmak için **Açıldığında bağlan**’ı seçin. **İleri** > **Son** seçeneğine ve ardından**Tamam**’a tıklayın.


## <a name="register-the-configuration-server"></a>Yapılandırma sunucusunu kaydetme 

1. VMWare vSphere Client konsolundan VM’yi açın.
2. VM’de Windows Server 2016 yükleme deneyimi önyüklemesi yapılır. Lisans sözleşmesini kabul edin ve bir yönetici parolası belirtin.
3. Yükleme bittikten sonra VM’de yönetici olarak oturum açın.
4. İlk kez oturum açtığınızda Azure Site Recovery Configuration Tool başlatılır.
5. Yapılandırma sunucusunu Site Recovery’ye kaydetmek için kullanılan bir ad belirtin. Ardından **İleri**'ye tıklayın.
6. Araç, VM’nin Azure bağlanıp bağlanamadığını denetler. Bağlantı kurulduktan sonra Azure aboneliğinizde oturum açmak için **Oturum aç**’a tıklayın. Kimlik bilgilerinin, yapılandırma sunucusunu kaydetmek istediğiniz kasaya erişim izni olmalıdır.
7. Araç birkaç yapılandırma görevi gerçekleştirir ve yeniden başlatır.
8. Makinede yeniden oturum açın. Yapılandırma sunucusu yönetim sihirbazı otomatik olarak başlatılır.

### <a name="configure-settings-and-connect-to-vmware"></a>Ayarları yapılandırma ve VMware’e bağlanma

1. Yapılandırma sunucusu yönetim sihirbazı > **Kurulum bağlantısı** bölümünde çoğaltma trafiğini alacak NIC’yi seçin. Daha sonra **Kaydet**'e tıklayın. Bu ayarı yapılandırdıktan sonra değiştiremezsiniz.
2. **Kurtarma Hizmetleri kasasını seçin** bölümünde Azure aboneliğinizi ve ilgili kaynak grubu ile kasayı seçin.
3. **Üçüncü taraf yazılım yükle** bölümünde lisans sözleşmesini kabul edin ve MySQL Server’ı yüklemek için **İndir ve Yükle**’ye tıklayın.
4. **VMware PowerCLI’yı yükle**’ye tıklayın. Bunu yapmadan önce tüm tarayıcı pencerelerinin kapalı olduğundan emin olun. Daha sonra **Devam**’a tıklayın
5. Devam etmeden önce **Gereç yapılandırmasını doğrula** bölümünde önkoşullar doğrulanır.
6. **vCenter Server/vSphere ESXi sunucusunu yapılandır** bölümünde çoğaltmak istediğiniz VM’lerin bulunduğu vCenter sunucusunun veya vSphere konağının FQDN’sini veya IP adresini belirtin. Sunucunun dinlediği bağlantı noktasını ve kasadaki VMware sunucusu için kullanılacak kolay adı belirtin.
7. VMware sunucusuna bağlanmak için yapılandırma sunucusu tarafından kullanılacak kimlik bilgilerini belirtin. Site Recovery, bu kimlik bilgilerini çoğaltma için kullanılabilen VMware VM’lerini otomatik olarak bulmak üzere kullanır. **Ekle**’ye ve arından **Devam**’a tıklayın.
8. **Sanal makine kimlik bilgilerini yapılandır** bölümünde, çoğaltma etkinleştirildiğinde Mobility hizmetini makinelere otomatik olarak yüklemek için kullanılacak kullanıcı adını ve parolayı belirtin. Windows makinelerinde hesap için, çoğaltmak istediğiniz makinelerde yerel yönetici ayrıcalıkları gerekir. Linux’ta kök hesap için bilgileri sağlayın.
9. Kaydı tamamlamak için **Yapılandırmayı sonlandır**’a tıklayın. 
10. Kayıt bittikten sonra Azure portalında, yapılandırma sunucusunun ve VMware sunucusunun kasadaki **Kaynak** sayfasında listelendiğinden emin olun. Daha sonra hedef ayarlarını yapılandırmak için **Tamam**’a tıklayın.


Site Recovery, belirtilen ayarları kullanarak VMware sunucularına bağlanır ve VM’leri bulur.

> [!NOTE]
> Hesap adının portalda görünmesi 15 dakika veya daha fazla sürebilir. Hemen güncelleştirmek için **Yapılandırma Sunucuları** > ***sunucu adı*** > **Sunucuyu Yenile**’ye tıklayın.

## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Hedef kaynaklarını seçin ve doğrulayın.

1. **Altyapıyı hazırlama** > **Hedef** seçeneklerine tıklayıp kullanmak istediğiniz Azure aboneliğini seçin.
2. Hedef dağıtım modelinizin Kaynak Yöneticisi tabanlı veya klasik olup olmadığını belirtin.
3. Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.

   ![Hedef](./media/tutorial-vmware-to-azure/storage-network.png)

## <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

1. [Azure portalı](https://portal.azure.com)’nı açın ve **Tüm kaynaklar**’a tıklayın.
2. **ContosoVMVault** adlı Kurtarma Hizmeti kasasına tıklayın.
3. Çoğaltma ilkesi oluşturmak için **Site Recovery altyapısı** > **Çoğaltma İlkeleri** > **+Çoğaltma İlkesi**’ne tıklayın.
4. **Çoğaltma ilkesi oluştur** bölümünde bir ilke adı, **VMwareRepPolicy**, belirtin.
5. **RPO eşiği** bölümünde varsayılan 60 dakika seçeneğini kullanın. Bu değer kurtarma noktalarının hangi sıklıkta oluşturulacağını tanımlar. Devamlı çoğaltma bu sınırı aşarsa bir uyarı oluşturulur.
6. **Kurtarma noktası bekletme** bölümünde her bir kurtarma noktasının bekletme aralığı için varsayılan 24 saat seçeneğini kullanın. Bu öğretici için değer 72 saat olarak seçildi. Çoğaltılan VM’ler bir aralıktaki herhangi bir noktaya kurtarılabilir.
7. **Uygulamayla tutarlı anlık görüntü sıklığı** bölümünde uygulamayla tutarlı anlık görüntülerin oluşturulma sıklığı için varsayılan 60 dakika seçeneğini kullanın. İlkeyi oluşturmak için **Tamam**’a tıklayın.

   ![İlke](./media/tutorial-vmware-to-azure/replication-policy.png)

İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir. Eşleşen bir ilke, varsayılan olarak yeniden çalışma için otomatik oluşturulur. Örneğin, çoğaltma ilkesi **rep-policy** ise yeniden çalışma ilkesi **rep-policy-failback** olur. Bu ilke Azure’dan bir yeniden çalışma başlatılana kadar kullanılmaz.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Bir VM için çoğaltma etkinleştirildiğinde Site Recovery, Mobility hizmeti yükler. Değişikliklerin geçerli olması ve portalda görüntülenmesi 15 dakika veya daha uzun sürebilir.

Aşağıda belirtilen şekilde çoğaltmayı etkinleştirin:

1. **Uygulama çoğaltma** > **Kaynak** seçeneğine tıklayın.
2. **Kaynak** bölümünde yapılandırma sunucusunu seçin.
3. **Makine türü** bölümünde **Sanal Makineler**’i seçin.
4. **vCenter/vSphere Hypervisor** bölümünde vSphere konağını yöneten vCenter sunucusunu veya konağı seçin.
5. İşlem sunucusunu (yapılandırma sunucusu) seçin. Daha sonra **Tamam**’a tıklayın.
6. **Hedef** bölümünde yükü devredilen VM’leri oluşturmak istediğiniz aboneliği ve kaynak grubunu seçin. Yükü devredilen VM’ler için Azure’da kullanmak istediğiniz dağıtım modelini (klasik veya kaynak yönetimi) seçin.
7. Verileri çoğaltmak için kullanmak istediğiniz Azure depolama hesabını seçin.
8. Yük devretme sonrasında oluşturulan Azure VM'lerinin bağlanacağı Azure ağını ve alt ağını seçin.
9. Koruma için seçtiğiniz tüm makinelere ağ ayarını uygulamak için **Seçili makineler için şimdi yapılandır**’ı seçin. Makineler için Azure ağını ayrı ayrı seçmek için **Daha sonra yapılandır**'ı seçin.
10. **Sanal Makineler** > **Sanal makine seçin** seçeneklerine tıklayın ve çoğaltmak istediğiniz makineleri seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. Daha sonra, **Tamam**'a tıklayın.
11. **Özellikler** > **Özellikleri yapılandır** bölümünde, Mobility hizmetini makineye otomatik olarak yüklemek için işlem sunucusu tarafından kullanılacak hesabı seçin.
12. **Çoğaltma ayarları** > **Çoğaltma ayarlarını yapılanır** bölümünde doğru çoğaltma ilkesinin seçildiğinden emin olun.
13. **Çoğaltmayı Etkinleştir**’e tıklayın.

**Ayarlar** > **İşler** > **Site Recovery İşleri** bölümünden **Korumayı Etkinleştir** işinin ilerleme durumunu izleyebilirsiniz. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.

Eklediğiniz VM’leri izlemek için, **Yapılandırma Sunucuları**’nda VM’lerin son bulunma zamanına göz atabilirsiniz
> **Son İletişim Zamanı**. Zamanlanmış bulmayı beklemeden VM’leri eklemek için yapılandırma sunucusunu vurgulayın (tıklamayın) ve **Yenile**’ye tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma tatbikatı çalıştırma](site-recovery-test-failover-to-azure.md)
