---
title: "Azure Site Recovery ile AWS’den Azure’a sanal makineleri geçirme | Microsoft Docs"
description: "Bu makalede, Azure Site Recovery kullanılarak, Amazon Web Services’te (AWS) çalıştırılan sanal makinelerin Azure’a nasıl geçirileceği açıklanmaktadır."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ddb412fd-32a8-4afa-9e39-738b11b91118
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/27/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 9b1aabcd88ce93ff9316d3ee99619eccaeb6a38e
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="migrate-amazon-web-services-aws-vms-to-azure"></a>Amazon Web Services (AWS) sanal makinelerini Azure’a geçirme

Bu öğretici, Site Recovery’yi kullanarak, Amazon Web Services (AWS) sanal makinelerini Azure sanal makinelerine nasıl geçireceğinizi öğretir. Azure, fiziksel, oldukları gibi kabul VMsare EC2 örnekleri geçirilirken bilgisayarların şirket içi. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure kaynaklarını hazırlama
> * AWS EC2 örneklerini geçiş için hazırlama
> * Yapılandırma sunucusunu dağıtma
> * Sanal makineler için çoğaltmayı etkinleştirme
> * Her şeyin çalıştığından emin olmak için yük devretme testi çalıştırma
> * Azure’a bir defalık yük devretme çalıştırma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.


## <a name="prepare-azure-resources"></a>Azure kaynaklarını hazırlama 

Geçirilen EC2 örneklerinin kullanılması için Azure’da birkaç kaynağın hazır olması gerekir. Bunlar arasında, depolama hesabı, kasa ve sanal ağ yer alır.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Çoğaltılan makinelerin görüntüleri Azure depolama alanında tutulur. Şirket içinden Azure’a yük devretme çalıştırdığınızda depolama alanından Azure sanal makineleri oluşturulur.

1. [Azure portalı](https://portal.azure.com) menüsünde **Kaynak oluştur** -> **Depolama** -> **Depolama hesabı**’na tıklayın.
2. Depolama hesabınız için bir ad girin. Bu öğreticiler için **awsmigrated2017** adını kullanırız. Ad, Azure içinde benzersiz olmalı, 3 ila 24 karakter uzunluğunda olmalı ve yalnızca rakamlar ve küçük harfler içermelidir.
3. **Dağıtım modeli**, **Hesap türü**, **Performans** ve **Güvenli aktarım gereklidir** için varsayılanları değiştirmeyin.
5. **Çoğaltma** için varsayılan **RA-GRS** seçeneğini belirleyin.
6. Bu öğretici için kullanmak istediğiniz aboneliği seçin.
7. **Kaynak grubu** için **Yeni oluştur**’u seçin. Bu örnekte, ad olarak **migrationRG** adını kullanırız.
8. Konum olarak **Batı Avrupa**’yı seçin. 
9. Depolama hesabını oluşturmak için **Oluştur**’a tıklayın.

### <a name="create-a-vault"></a>Kasa oluşturma

1. [Azure portalı](https://portal.azure.com)’nda sol gezinme bölmesinde **Diğer hizmetler**’e tıklayın ve **Kurtarma Hizmetleri kasaları**’nı bulup seçin.
2. Kurtarma Hizmetleri kasaları sayfasında sol üst kısımdaki **+ Ekle**’ye tıklayın.
3. **Ad** için *myVault* yazın. 
4. **Abonelik** için uygun aboneliği seçin.
4. **Kaynak Grubu** için **Var olanı kullan**’ı ve *migrationRG* adını seçin. 
5. **Konum** bölümünde *Batı Avrupa*’yı seçin. 
5. Panodan yeni kasaya hızlı şekilde erişmek için **Panoya sabitle**’yi seçin.
7. İşiniz bittiğinde **Oluştur**’a tıklayın.

Yeni kasa, **Pano** > **Tüm kaynaklar** bölümünde ve ana **Kurtarma Hizmetleri kasaları** sayfasında görüntülenir.

### <a name="set-up-an-azure-network"></a>Azure ağı ayarlama

Geçişten (yük devretme) sonra Azure sanal makineleri oluşturulduğunda bu ağa katılırlar.

1. İçinde [Azure portal](https://portal.azure.com), tıklatın **kaynak oluşturma** > **ağ** >
    **sanal ağ**
3. **Ad** için *myMigrationNetwork* yazın.
4. **Adres alanı** için varsayılan değeri değiştirmeyin.
5. **Abonelik** için uygun aboneliği seçin.
6. **Kaynak grubu** için **Var olanı kullan** seçeneğini belirleyin ve açılır listeden *migrationRG* adını seçin.
7. **Konum** için **Batı Avrupa**’yı seçin. 
8. **Alt Ağ** için hem **Ad** hem de **IP aralığı** varsayılanlarını değiştirmeyin.
9. **Hizmet Uç Noktaları**’nı devre dışı bırakın.
10. İşiniz bittiğinde **Oluştur**’a tıklayın.


## <a name="prepare-the-ec2-instances"></a>EC2 örneklerini hazırlama

Geçirmek istediğiniz bir veya daha fazla sanal makine olması gerekir. Bu EC2 örneği; 64 bit Windows Server 2008 R2 SP1 veya sonrası, Windows Server 2012, Windows Server 2012 R2 ya da Red Hat Enterprise Linux 6.7 (yalnızca HVM sanallaştırılmış örnekleri) sürümünü çalıştırıyor olmalıdır. Sunucuda yalnızca Citrix PV veya AWS PV sürücüler bulunmalıdır. RedHat PV sürücüleri çalıştıran örnekler desteklenmez.

Çoğaltmak istediğiniz her sanal makinede Mobility hizmeti yüklü olmalıdır. Sanal makine için çoğaltmayı etkinleştirdiğinizde Site Recovery bu hizmeti otomatik olarak yükler. Otomatik yükleme için, Site Recovery’nin sanal makineye erişmek için kullanacağı EC2 örneklerinde bir hesap hazırlamanız gerekir.

Bir etki alanı veya yerel hesap kullanabilirsiniz. Linux sanal makineleri için hesap, kaynak Linux sunucusu üzerindeki kök olmalıdır. Windows sanal makineleri içinse, bir etki alanı hesabı kullanmıyorsanız yerel makinede Uzak Kullanıcı Erişim denetimini devre dışı bırakın:

  - Kayıt defterinde **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System** altına **LocalAccountTokenFilterPolicy** DWORD girişini ekleyin ve değeri 1 olarak ayarlayın.
    
Ayrıca Site Recovery yapılandırma sunucusu olarak kullanabileceğiniz ayrı bir EC2 örneği de gerekir. Bu örnek, Windows Server 2012 R2’yi çalıştırıyor olmalıdır. 
    

## <a name="prepare-the-infrastructure"></a>Altyapıyı hazırlama 

Kasanızın portal sayfasında, **Başlarken** bölümünden **Site Recovery** seçeneğini belirleyin ve **Altyapıyı Hazırlama**’ya tıklayın.

### <a name="1-protection-goal"></a>1 Koruma hedefi

**Koruma Hedefi** sayfasından aşağıdaki değerleri seçin:
   
|    |  | 
|---------|-----------|
| Makineleriniz nerede bulunuyor? | **Şirket içi**|
| Makinelerinizi nereye çoğaltmak istiyorsunuz? |**Azure’a**|
| Makineleriniz sanallaştırılmış mı? | **Sanallaştırılmamış / Diğer**|

İşiniz bittiğinde, sonraki bölüme geçmek için **Tamam**’a tıklayın.

### <a name="2-source-prepare"></a>2 Kaynak Hazırlama 

**Kaynağı hazırla** sayfasında **+ Yapılandırma Sunucusu** seçeneğine tıklayın. 

1. Yapılandırma sunucusu oluşturmak ve kurtarma kasası ile kaydetmek için Windows Server 2012 R2 çalıştıran bir EC2 örneği kullanın.

2. [Hizmet URL’lerine](../site-recovery-support-matrix-to-azure.md) erişebilmesi için yapılandırma sunucusu olarak kullandığınız EC2 örneği sanal makinesindeki ara sunucuyu yapılandırın.

3. [Microsoft Azure Site Recovery Birleşik Kurulumu](http://aka.ms/unifiedinstaller_wus) programını indirin. Yerel makinenize indirebilir ve yapılandırma sunucusu olarak kullandığınız sanal makineye kopyalayabilirsiniz.

4. Kasa kayıt anahtarını indirmek için **İndir** düğmesine tıklayın. İndirilen dosyayı, yapılandırma sunucusu olarak kullandığınız sanal makineye kopyalayın.

5. Sanal makinede, **Microsoft Azure Site Recovery Birleşik Kurulumu** için indirdiğiniz yükleyiciye sağ tıklayın ve **Yönetici olarak çalıştır**’ı seçin. 

    1. **Başlamadan Önce** bölümünde **Yapılandırma sunucusunu ve işlem sunucusunu yükleme**’yi seçin ve ardından **İleri**’ye tıklayın.
    2. **Üçüncü Taraf Yazılım Lisansı** bölümünde **Üçüncü taraf lisans sözleşmesini kabul ediyorum**’u seçin ve ardından **İleri**’ye tıklayın.
    3. **Kayıt** bölümünde, kasa kayıt anahtarı dosyasını koyduğunuz yere göz atıp gidin ve **İleri**’ye tıklayın.
    4. **Internet Ayarları** bölümünde **Ara sunucu olmadan Azure Site Recovery’ye bağlan** seçeneğini belirleyin ve ardından **İleri**’ye tıklayın.
    5. **Önkoşul Denetimi** sayfasında bu birkaç öğe için denetimler çalıştırır. İşlem tamamlandığında **İleri**’ye tıklayın. 
    6. **MySQL Yapılandırması** bölümünde gerekli parolaları sağlayın ve **İleri**’ye tıklayın.
    7. **Ortam Ayrıntıları** bölümünde **Hayır**, VMware makinelerini korumanız gerekmez seçeneğini belirleyin ve ardından **İleri**’ye tıklayın.
    8. **Yükleme Konumu** bölümünde **İleri**’ye tıklayarak varsayılanı kabul edin.
    9. **Ağ Seçimi** bölümünde **İleri**’ye tıklayarak varsayılanı kabul edin.
    10. **Özet** bölümünde **Yükle**’ye tıklayın.
    11. **Yükleme İlerleme Durumu**, size yükleme sürecinin neresinde olduğunuza dair bilgileri gösterir. İşlem tamamlandığında **Son**’a tıklayın. Olası bir yeniden başlatma gerektiğine dair açılır pencereyle karşılaşırsınız; bu durumda **Tamam**’a tıklayın. Yapılandırma Sunucusu Bağlantı Şifresi ile ilgili bir açılır pencereyle de karşılaşırsınız; bu durumda şifreyi panonuza kopyalayıp güvenli bir yerde saklayın.
    
6. Sanal makinede, yapılandırma sunucusunda bir veya daha fazla yönetim hesabı oluşturmak için **cspsconfigtool.exe** dosyasını çalıştırın. Yönetim hesaplarının, geçirmek istediğiniz EC2 örneklerinde yönetici iznine sahip olduğundan emin olun. 

Yapılandırma sunucusunu ayarlama işiniz bittiğinde portala geri dönün, **Yapılandırma Sunucusu** için oluşturmuş olduğunuz sunucuyu seçin ve *Tamam** seçeneğine tıklayarak 3. adım olan Hedef Hazırlama adımına ilerleyin.

### <a name="3-target-prepare"></a>3 Hedef Hazırlama 

Bu bölümde, bu öğreticinin önceki kısımlarındaki [Azure kaynaklarını hazırlama](#prepare-azure-resources) bölümündeyken oluşturduğunuz kaynaklar hakkında bilgi girersiniz.

1. **Abonelik** bölümünde, [Azure’u Hazırlama](../tutorial-prepare-azure.md) öğreticisi için kullandığınız Azure aboneliğini seçin.
2. Dağıtım modeli olarak **Kaynak Yöneticisi**’ni seçin.
3. Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler. Bunlar, bu öğreticinin önceki kısımlarındaki [Azure kaynaklarını hazırlama](#prepare-azure-resources) bölümündeyken oluşturduğunuz kaynaklar olmalıdır
4. İşiniz bittiğinde **Tamam**’a tıklayın.


### <a name="4-replication-settings-prepare"></a>4 Çoğaltma ayarlarını Hazırlama 

Çoğaltmayı etkinleştirebilmeniz için önce bir çoğaltma ilkesi oluşturmanız gerekir

1. **+ Çoğalt ve İlişkilendir**’e tıklayın.
2. **Ad** bölümüne **myReplicationPolicy** yazın.
3. Geri kalan varsayılan ayarları değiştirmeyin ve **Tamam**’a tıklayarak ilkeyi oluşturun. Yeni ilke otomatik olarak yapılandırma sunucusu ile ilişkilendirilir. 

### <a name="5-deployment-planning-select"></a>5 Dağıtım planlaması Seçme 

**Dağıtım planlamasını tamamladınız mı?** bölümünde, açılır listeden **Daha sonra yapacağım**’ı seçin ve **Tamam**’a tıklayın.

**Altyapıyı hazırlama** ile ilgili 5 bölümü de tamamladığınızda **Tamam**’a tıklayın.


## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Geçirmek istediğiniz her sanal makine için çoğaltmayı etkinleştirin. Çoğaltma etkinleştirildiğinde Site Recovery otomatik olarak Mobility hizmetini yükler. 

1. [Azure portalı](htts://portal.azure.com) açın.
1. Kasanızın sayfasındaki **Başlarken** bölümünde **Site Recovery** seçeneğine tıklayın.
2. **Şirket içi makineler ve Azure VM’ler için** bölümünde **1. Adım: Uygulamayı Çoğaltma** seçeneğine tıklayın. Sihirbaz sayfalarını aşağıdaki bilgilerle doldurun ve tamamladığınız her sayfada **Tamam**’a tıklayın:
    - 1 Kaynak Yapılandırma:
      
    |  |  |
    |-----|-----|
    | Kaynak: | **Şirket İçi**|
    | Kaynak konumu:| Yapılandırma sunucusu EC2 örneğinizin adı.|
    |Makine türü: | **Fiziksel makineler**|
    | İşlem sunucusu: | Açılır listeden yapılandırma sunucusunu seçin.|
    
    - 2 Hedef Yapılandırma
        
    |  |  |
    |-----|-----|
    | Hedef: | Varsayılanı değiştirmeyin.|
    | Abonelik: | Kullanmakta olduğunuz aboneliği seçin.|
    | Yük devretme sonrası kaynak grubu:| [Azure kaynaklarını hazırlama](#prepare-azure-resources) bölümünde oluşturduğunuz kaynak grubunu kullanın.|
    | Yük devretme sonrası dağıtım modeli: | **Resource Manager**’ı seçin|
    | Depolama hesabı: | [Azure kaynaklarını hazırlama](#prepare-azure-resources) bölümünde oluşturduğunuz depolama hesabını seçin.|
    | Azure ağı: | **Seçili makineler için şimdi yapılandırın**’ı seçin|
    | Yük devretme sonrası Azure ağı: | [Azure kaynaklarını hazırlama](#prepare-azure-resources) bölümünde oluşturduğunuz ağı seçin.|
    | Alt ağ: | Açılır listeden **varsayılan** seçeneğini belirleyin.|
    
    - 3 Fiziksel Makineleri Seçme
        
        **+ Fiziksel makine** seçeneğine tıklayın ve ardından geçirmek istediğiniz EC2 örneğinin **Ad**, **IP Adresi** ve **İşletim Sistemi Türü** bilgilerini girip **Tamam**’a tıklayın.
        
    - 4 Özellikler Özellikleri Yapılandırma
        
        Açılır listeden yapılandırma sunucusunda oluşturduğunuz hesabı seçin ve **Tamam**’a tıklayın.
        
    - 5 Çoğaltma Ayarları Çoğaltma ayarlarını yapılandırma
    
        Açılır listede seçilen çoğaltma ilkesinin **myReplicationPolicy** olduğundan emin olun ve **Tamam**’a tıklayın.
        
3. Sihirbaz tamamlandığında **Çoğaltmayı etkinleştir**’e tıklayın.
        

**İzleme ve raporlar** > **İşler** > **Site Recovery işleri** bölümünde **Korumayı Etkinleştir** işinin ilerleme durumunu izleyebilirsiniz. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.        
        
Bir sanal makine için çoğaltmayı etkinleştirdiğinizde, değişikliklerin geçerli olması ve portalda görüntülenmesi 15 dakika veya daha uzun sürebilir.

## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Yük devretme testi çalıştırdığınızda şunlar olur:

1. Yük devretme için gerekli tüm koşulların karşılandığından emin olmak için bir önkoşul denetimi çalıştırılır.
2. Yük devretme, bir Azure sanal makinesi oluşturulabilmesi için verileri işler. En son kurtarma noktasını seçerseniz verilerden bir kurtarma noktası oluşturulur.
3. Önceki adımda işlenen veriler kullanılarak bir Azure sanal makinesi oluşturulur.

Portalda şu şekilde yük devretme testini çalıştırın:

1. Kasanızın sayfasında **Korumalı öğeler** > **Çoğaltılan Öğeler** bölümüne gidin > VM > **+ Yük Devretme Testi** seçeneklerine tıklayın.

2. Yük devretme için kullanılacak bir kurtarma noktası seçin:
    - **En son işlenen**: VM yükü, Site Recovery tarafından işlenen en son kurtarma noktasına devredilir. Zaman damgası gösterilir. Bu seçenekte, verileri işlemek için zaman harcanmadığından düşük bir RTO (kurtarma süresi hedefi) sağlanır.
    - **En son uygulamayla tutarlı**: Bu seçenekte tüm VM yükü, uygulamayla tutarlı en son kurtarma noktasına devredilir. Zaman damgası gösterilir.
    - **Özel**: Herhangi bir kurtarma noktası seçin.
3. **Yük Devretme Testi** bölümünde, yük devretme gerçekleştikten sonra Azure VM’lerinin bağlanacağı hedef Azure ağını seçin. Bu, [Azure kaynaklarını hazırlama](#prepare-azure-resources) bölümünde oluşturduğunuz ağ olmalıdır.
4. Yük devretmeyi başlatmak için **Tamam**'a tıklayın. Sanal makine özelliklerini açmak için sanal makineye tıklayarak ilerleme durumunu izleyebilirsiniz. Veya **İzleme ve raporlar** > **İşler** >
   **Site Recovery işleri** bölümündeki kasanızın sayfasında **Yük Devretme Testi** işine tıklayabilirsiniz.
5. Yük devretme işlemi tamamlandıktan sonra çoğaltma Azure sanal makinesi, Azure portalı > **Sanal Makineler** bölümünde görüntülenir. Sanal makinenin uygun boyutta olduğundan, doğru ağa bağlandığından ve çalıştığından emin olun.
6. Şimdi Azure’da çoğaltılan sanal makineye bağlanabiliyor olmanız gerekir.
7. Yük devretme testi sırasında oluşturulan Azure sanal makinelerini silmek için, kurtarma planında **Yük devretme testini temizle**’ye tıklayın. **Notlar**’da, yük devretme testiyle ilişkili gözlemlerinizi kaydedin ve saklayın.

Bazı senaryolarda yük devretme için sekiz ila on dakikada tamamlanan ek işlem gerekir. 


## <a name="migrate-to-azure"></a>Azure’a geçiş

EC2 örneklerinin Azure sanal makinelerine geçişi için gerçek bir yük devretme çalıştırın. 

1. **Korumalı öğeler** > **Çoğaltılan öğeler** bölümünde AWS örnekleri > **Yük devretme** seçeneklerine tıklayın.
2. **Yük devretme** bölümünde yük devretmenin yapılacağı bir **Kurtarma Noktası** seçin. En son kurtarma noktasını seçin.
3. Site Recovery’nin yük devretmeyi tetiklemeden önce kaynak sanal makineleri kapatmayı denemesini istiyorsanız **Yük devretmeye başlamadan önce makineyi kapat** seçeneğini belirleyin. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.
4. Sanal makinenin, **Çoğaltılan öğeler** bölümünde görüntülenip görüntülenmediğini denetleyin. 
5. Her bir sanal makineye sağ tıklayın > **Geçişi Tamamla**’ya tıklayın. Böylece geçiş işlemi tamamlanır, AWS VM için çoğaltma durdurulur ve sanal makine için Site Recovery faturalaması durdurulur.

    ![Geçişi tamamlama](./media/tutorial-migrate-aws-to-azure/complete-migration.png)

> [!WARNING]
> **Devam eden yük devretme işlemini iptal etmeyin**: Yük devretme başlatılmadan önce VM çoğaltma durdurulur. Devam eden bir yük devretme işlemini iptal ederseniz yük devretme durdurulur, ancak VM yeniden çoğaltılmaz.  


    

## <a name="next-steps"></a>Sonraki adımlar

Bu konuda, AWS EC2 örneklerinin Azure sanal makinelerine nasıl geçirileceğini öğrendiniz. Azure sanal makineleri hakkında daha fazla bilgi için Windows sanal makinelerine yönelik öğreticilere geçin.

> [!div class="nextstepaction"]
> [Azure Windows sanal makine öğreticileri](../../virtual-machines/windows/tutorial-manage-vm.md)
