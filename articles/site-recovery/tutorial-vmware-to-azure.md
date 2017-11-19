---
title: "Azure Site Recovery ile şirket içi VMware VM'ler için olağanüstü durum kurtarma Azure ayarlama | Microsoft Docs"
description: "Olağanüstü durum kurtarma Azure Azure Site Recovery hizmeti ile şirket içi VMware Vm'leri için nasıl ayarlanacağını öğrenin."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 461feb952f7e2eddba9c7218b3463868e8cb7965
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Şirket içi VMware Vm'leri için olağanüstü durum kurtarma Azure ayarlama

Bu öğretici, olağanüstü durum kurtarma için Azure şirket içi VMware VM çalıştıran Windows nasıl ayarlanacağını gösterir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Site kurtarma için bir kurtarma Hizmetleri kasası oluşturma
> * Kaynağı ayarlama ve çoğaltma ortamları hedef
> * Çoğaltma ilkesi oluşturma
> * Bir sanal makine için çoğaltmayı etkinleştirme

Bir dizi üçüncü öğreticide budur. Bu öğreticinin önceki eğitimlerine görevleri önceden tamamlamış varsayılır:

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
2. [Şirket içi VMware’leri hazırlama](tutorial-prepare-on-premises-vmware.md)

Başlamadan önce için yararlı [mimarisi gözden](concepts-vmware-to-azure-architecture.md) olağanüstü durum kurtarma senaryosuna.

## <a name="configure-vmware-account-permissions"></a>VMware hesap izinlerini yapılandırma

1. Bir rolü vCenter düzeyinde oluşturun. Rol adını verin **Azure_Site_Recovery**.
2. Aşağıdaki izinleri atamak **Azure_Site_Recovery** rol.

   **Görev** | **Rol/izinleri** | **Ayrıntılar**
   --- | --- | ---
   **VM bulma** | Veri Merkezi Nesne –> Propagate alt nesne için rol = salt okunur | En az bir salt okunur kullanıcı.<br/><br/> Kullanıcı veri merkezi düzeyde atanan ve veri merkezinde tüm nesnelere erişimi vardır.<br/><br/> Erişimi kısıtlamak için Ata **erişim yok** rolüyle **alt Propagate** (vSphere ana bilgisayarları, datastores, sanal makineleri ve ağları) alt nesneleri için nesne.
   **Tam çoğaltma, yük devretme, yeniden çalışma** |  Veri Merkezi Nesne –> Propagate alt nesne için rol Azure_Site_Recovery =<br/><br/> Veri deposu alanı Ayır ->, veri deposu, alt düzey dosya işlemleri göz atın, dosyayı kaldırmak, sanal makine dosyalarını güncelleştir<br/><br/> Ağ -> Ağ atama<br/><br/> Kaynak VM atamak için kaynak havuzu ->, VM güç beslemeli geçirmek, VM güç beslemeli geçirme<br/><br/> Görevler oluşturma görevi, güncelleştirme görevi -><br/><br/> Sanal Makine Yapılandırma -><br/><br/> Sanal makine -> etkileşimde bulunma yanıt soru, cihaz bağlantısı ->, CD ortamı yapılandırmak, disket ortamı, kapatma, açma, VMware araçları yükleme yapılandırın<br/><br/> Sanal makine -> Stok Oluştur ->, kaydetme, kaydı<br/><br/> Sanal makine sağlama -> izin sanal makine indirme ->, sanal makine dosyalarını karşıya yükleme izin ver<br/><br/> Sanal makine anlık görüntüleri -> Kaldır anlık görüntüleri -> | Kullanıcı veri merkezi düzeyde atanan ve veri merkezinde tüm nesnelere erişimi vardır.<br/><br/> Erişimi kısıtlamak için Ata **erişim yok** rolüyle **alt Propagate** (vSphere ana bilgisayarları, datastores, sanal makineleri ve ağları) alt nesneleri için nesne.

3. VCenter sunucusu veya vSphere ana bilgisayarda bir kullanıcı oluşturun. Kullanıcıya rol atayın.

## <a name="specify-what-you-want-to-replicate"></a>Çoğaltmak istediğiniz belirtin

Mobility hizmetinin çoğaltmak istediğiniz her bir VM üzerinde yüklü olmalıdır. Sanal makine için çoğaltmayı etkinleştirdiğinizde, site kurtarma Bu hizmeti otomatik olarak yüklenir. Otomatik yükleme için Site Recovery VM erişmek için kullanacağı bir hesap hazırlamanız gerekir.

Bir etki alanı veya yerel hesabı kullanabilirsiniz. Linux VM'ler için hesabın kaynak Linux sunucusu kökünde olması gerekir. Windows etki alanı hesabı, yerel makinede uzak kullanıcı erişim denetimi devre dışı bırak kullanmıyorsanız VM'ler için:

  - Kayıt defteri içinde altında **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, DWORD girdisi eklemek **LocalAccountTokenFilterPolicy** ve değerini 1 olarak ayarlayın.

## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Kaynak ortamını ayarlama, Site Recovery Kurulumu birleşik karşıdan yükleme, yapılandırma sunucusu kurma ve kasaya kaydetmeyi ve VM'ler keşfetme oluşur.

Yapılandırma, tek bir VMware VM'ın tüm Site Recovery bileşenlerini barındırmak için şirket içi sunucusudur. Bu VM yapılandırması sunucu, işlem sunucusu ve ana hedef sunucusunda çalışır.

- Yapılandırma sunucusu yerinde bileşenler ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir.
- İşlem sunucusu çoğaltma ağ geçidi olarak davranır. Çoğaltma verilerini alıp bu verileri önbelleğe alma, sıkıştırma ve şifreleme işlemleriyle iyileştirir ve Azure depolama alanına gönderir. İşlem sunucusu çoğaltmak istediğiniz sanal makinelerin de Mobility hizmeti yükler ve sanal makineleri otomatik olarak bulmayı şirket içi VMware sunucularda gerçekleştirir.
- Ana hedef sunucusu azure'dan yeniden çalışma sırasında çoğaltma verilerini işler.

Yapılandırma sunucusu VM aşağıdaki gereksinimleri karşıladığından yüksek oranda kullanılabilir bir VMware VM olmalıdır:

| **Gereksinim** | **Ayrıntılar** |
|-----------------|-------------|
| CPU çekirdeği sayısı| 8 |
| RAM | 12 GB |
| Disk sayısı | 3 - işletim sistemi diski, işlem sunucusu önbellek disk, bekletme sürücüsü (için yeniden çalışma) |
| Boş disk alanı (işlem sunucusu önbelleği) | 600 GB |
| Boş disk alanı (bekletme diski) | 600 GB |
| İşletim sistemi sürümü | Windows Server 2012 R2 |
| İşletim sistemi yerel ayarı | İngilizce (en-us) |
| VMware vSphere PowerCLI sürümü | [PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "PowerCLI 6.0") |
| Windows Server rolleri | Bu rolleri etkinleştirmezseniz: Active Directory etki alanı Hizmetleri, Internet Information Services, Hyper-V |
| NIC türü | VMXNET3 |
| IP adresi türü | Statik |
| Bağlantı Noktaları | 443 (Denetim kanalı düzenleme)<br/>9443 (Veri aktarımı)|

Yapılandırma sunucusunda VM sistem saati bir saat sunucusuyla eşitlendiğinden emin olun.
Saat 15 dakika içinde eşitlenmelidir. Zaman farkı 15 dakikadan fazla ise, kurulum başarısız olur.

Yapılandırma sunucusu bu URL'leri erişebildiğinden emin olun:

   [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
    - Herhangi bir IP adresi tabanlı güvenlik duvarı kuralı Azure ile iletişim kurmaya izin vermelidir.

- [Azure Veri Merkezi IP Aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653)'na ve HTTPS (443) bağlantı noktasına izin verin.
    - IP adres aralıklarını aboneliğinizin Azure bölgesi ve Batı ABD (erişim denetimi ve kimlik yönetimi için kullanılan) izin verir.

Herhangi bir IP adresi tabanlı güvenlik duvarı kuralı iletişimi sağlamalıdır [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653)ve bağlantı noktası 443 (HTTPS) ve 9443 (veri çoğaltma). IP adres aralıklarını aboneliğinizin Azure bölgesi ve Batı ABD (erişim denetimi ve kimlik yönetimi için kullanılan) izin verecek şekilde emin olun.

### <a name="download-the-site-recovery-unified-setup"></a>Karşıdan yükleme sitesini kurtarma birleşik Kurulumu

1. Açık [Azure portal](https://portal.azure.com) ve tıklayın **tüm kaynakları**.
2. Adlı kurtarma hizmeti kasaya tıklayın **ContosoVMVault**.
3. Tıklatın **Site Recovery** > **altyapıyı hazırlama** > **koruma hedefi**.
4. Seçin **şirket içi** makinelerinizi bulunduğu için **için Azure** makinelerinizi çoğaltmak istediğiniz için ve **Evet, VMware vSphere hiper yönetici ile**. Ardından **Tamam**.
5. Prepare kaynak bölmesinde **+ yapılandırma sunucusu**.
6. İçinde **Sunucu Ekle**, denetleyin **yapılandırma sunucusu** görünür **sunucu türü**.
7. Site Recovery birleşik Kurulumu yükleme dosyasını indirin.
8. Kasa kayıt anahtarını indir Birleşik Kurulum'u çalıştırdığınızda bu gerekir. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

   ![Kaynağı ayarlama](./media/tutorial-vmware-to-azure/source-settings.png)

### <a name="set-up-the-configuration-server"></a>Yapılandırma sunucusu kurma

1. Birleşik Kurulum yükleme dosyasını çalıştırın.
2. İçinde **başlamadan önce**seçin **işlem sunucusu ve yapılandırma sunucusu yüklemek** ardından **sonraki**.

3. İçinde **üçüncü taraf yazılım lisansı**, tıklatın **kabul ediyorum** MySQL karşıdan yüklenip kurulacak ardından **sonraki**.

4. **Kayıt** menüsünde kasadan indirdiğiniz kayıt defteri anahtarını seçin.

5. **İnternet Ayarları** alanında, yapılandırma sunucusunda çalışan Sağlayıcının Azure Site Recovery'ye İnternet üzerinden nasıl bağlanacağını belirtin.

   - Şu anda makinede select ayarlanıp proxy ile bağlanmak isterseniz **Azure Site Recovery proxy sunucu kullanma Bağlan**.
   - Sağlayıcı doğrudan bağlanmasını istiyorsanız seçin **Azure Site Recovery bir proxy sunucu olmadan doğrudan bağlan**.
   - Mevcut proxy kimlik doğrulaması gerektiriyorsa veya özel bir ara sunucu sağlayıcısı bağlantılarında kullanmak istiyorsanız, **özel proxy ayarlarıyla Bağlan**, adresini, bağlantı noktası ve kimlik bilgilerini belirtin.

   ![Güvenlik duvarı](./media/tutorial-vmware-to-azure/combined-wiz4.png)

6. **Önkoşul Denetimi** menüsünde Kurulum, yüklemenin çalışabildiğinden emin olmak üzere bir denetim gerçekleştirir. **Genel saat eşitleme denetimi** hakkında bir uyarı görünürse, sistem saatindeki zamanın (**Tarih ve Saat** ayarları) saat dilimiyle aynı olduğunu doğrulayın.

   ![Ön koşullar](./media/tutorial-vmware-to-azure/combined-wiz5.png)

7. **MySQL Yapılandırması** menüsünde, yüklü MySQL sunucu örneğinde oturum açmak için kimlik bilgileri oluşturun.

8. İçinde **ortam ayrıntıları**seçin **Evet** VMware Vm'leri korumak için. Kurulum, Powerclı 6.0 yüklü olmadığını denetler.

9. **Yükleme Konumu** alanında ikili dosyaları yüklemek ve önbelleği depolamak istediğiniz konumu seçin. Seçtiğiniz sürücü en az 5 GB kullanılabilir disk alanına sahip olmalıdır, ancak en az 600 GB boş alanı olan bir önbellek sürücüsü seçmeniz önerilir.

10. **Ağ Seçimi** menüsünde, yapılandırma sunucusunun çoğaltma verilerini gönderip aldığı dinleyiciyi (ağ bağdaştırıcısı ve SSL bağlantı noktası) seçin. Bağlantı noktası 9443, çoğaltma trafiğini gönderip almak için kullanılan varsayılan bağlantı noktasıdır, ancak bu bağlantı noktası numarasını ortamınızın gereksinimlerine uyacak şekilde değiştirebilirsiniz. Biz de çoğaltma işlemlerini düzenlemek için kullanılan bağlantı noktası 443'ü açın. Bağlantı noktası 443 gönderirken ya da çoğaltma trafiğini alırken için kullanmayın.

11. **Özet** alanındaki bilgileri gözden geçirin ve **Yükle**’ye tıklayın. Kurulum yapılandırma sunucusunu yükler ve Azure Site Recovery hizmeti ile kaydeder.

    ![Özet](./media/tutorial-vmware-to-azure/combined-wiz10.png)

    Yükleme tamamlandığında bir parola oluşturulur. Çoğaltmayı etkinleştirdiğinizde bu parola gerekli olacaktır; bu yüzden kopyalayıp güvenli bir yerde saklayın. Sunucu üzerinde görüntülenir **ayarları** > **sunucuları** kasadaki bölmesi.

### <a name="configure-automatic-discovery"></a>Otomatik bulma yapılandırma

Vm'leri bulma şirket içi VMware sunucularına bağlanmak yapılandırma sunucusu gerekir. Bu öğreticinin amaçları doğrultusunda vCenter sunucusu veya vSphere ana bilgisayarları, sunucu üzerinde yönetici ayrıcalıklarına sahip bir hesabı kullanarak ekleyin.

1. Yapılandırma sunucunuzda başlatma **CSPSConfigtool.exe**. Masaüstünde kısayol olarak bulunur ve *yükleme konumu*\home\svsystems\bin klasöründedir.

2. **Hesapları Yönet** > **Hesap Ekle**’ye tıklayın.

   ![Hesap ekleme](./media/tutorial-vmware-to-azure/credentials1.png)

3. **Hesap Ayrıntıları** menüsünde otomatik bulma için kullanılacak hesabı ekleyin.

   ![Ayrıntılar](./media/tutorial-vmware-to-azure/credentials2.png)

Bir sunucu eklemek için:

1. Açık [Azure portal](https://portal.azure.com) ve tıklayın **tüm kaynakları**.
2. Adlı kurtarma hizmeti kasaya tıklayın **ContosoVMVault**.
3. Tıklatın **Site kurtarma** > **altyapıyı hazırlama** > **kaynağı**
4. Seçin **+ vCenter** bir vCenter sunucusu veya vSphere ESXi ana bilgisayara bağlanmak için.
5. İçinde **vCenter ekleme**, sunucu için kolay bir ad belirtin. Ardından, IP adresi veya FQDN belirtin.
6. Bağlantı noktası 443'e ayarlayın, VMware sunucularınızın farklı bir bağlantı noktası isteklerini dinlemek sürece bırakın.
7. Sunucuya bağlanmak için kullanılacak hesabı seçin. **Tamam** düğmesine tıklayın.

Site Recovery belirtilen ayarları kullanarak VMware sunucularına bağlanır ve sanal makineleri bulur.

> [!NOTE]
> Hesap adının portalda görünmesi 15 dakika veya daha fazla sürebilir. Hemen güncelleştirmek için **Yapılandırma Sunucuları** > ***sunucu adı*** > **Sunucuyu Yenile**’ye tıklayın.

## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Seçin ve hedef kaynaklarını doğrulayın.

1. **Altyapıyı hazırlama** > **Hedef** seçeneklerine tıklayıp kullanmak istediğiniz Azure aboneliğini seçin.
2. Belirtin, hedef dağıtım modeli Resource Manager tabanlı veya Klasik olup.
3. Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.

   ![Hedef](./media/tutorial-vmware-to-azure/storage-network.png)

## <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

1. Açık [Azure portal](https://portal.azure.com) ve tıklayın **tüm kaynakları**.
2. Adlı kurtarma hizmeti kasaya tıklayın **ContosoVMVault**.
3. Bir çoğaltma ilkesi oluşturmak için tıklatın **Site Recovery altyapısı** > **çoğaltma ilkeleri** > **+ Çoğaltma İlkesi**.
4. İçinde **çoğaltma ilkesi oluşturun**, ilke adını belirtebilir **VMwareRepPolicy**.
5. İçinde **RPO eşik**, varsayılan değer 60 dakika kullanın. Bu değer ne sıklıkta tanımlar kurtarma noktaları oluşturulur. Sürekli çoğaltma bu sınırı aşarsa bir uyarı üretilir.
6. İçinde **kurtarma noktası bekletme**, varsayılan 24 saatlik ne kadar süreyle bekletme penceresi için her kurtarma noktası için kullanın. Bu öğretici için 72 saat seçin. Çoğaltılan VM'ler penceresinde herhangi bir noktaya kurtarılabilir.
7. İçinde **uygulamayla tutarlı anlık görüntü sıklığı**, uygulamayla tutarlı anlık görüntüleri oluşturulur sıklığı için varsayılan değer 60 dakika kullanın. Tıklatın **Tamam** bir ilke oluşturmak için.

   ![İlke](./media/tutorial-vmware-to-azure/replication-policy.png)

İlke yapılandırma sunucusuyla otomatik olarak ilişkilendirilir. Varsayılan olarak, eşleşen bir ilkesi yeniden çalışma için otomatik olarak oluşturulur. Örneğin, çoğaltma ilkesi ise **rep İlkesi** geri dönme ilkesini sonra **rep İlkesi yeniden**. Bir azure'dan başlatana kadar bu ilkeyi kullanılmaz.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Çoğaltma için bir VM etkinleştirildiğinde, site Recovery Mobility hizmetini yükler. Bu 15 dakika alabilir veya artık değişikliklerinin için efekt ve portalda görüntülenir.

Şekilde çoğaltmayı etkinleştirin:

1. Tıklatın **uygulama çoğaltma** > **kaynak**.
2. İçinde **kaynak**, yapılandırma sunucusu seçin.
3. İçinde **makine türü**seçin **sanal makineleri**.
4. İçinde **vCenter/vSphere hiper yönetici**vSphere ana yöneten vCenter sunucusu seçin veya konağı seçin.
5. İşlem Sunucusu (yapılandırma sunucusu) seçin. IThen tıklatın **Tamam**.
6. İçinde **hedef**, abonelik ve başarısız VM'ler üzerinde oluşturmak istediğiniz kaynak grubunu seçin. Devredilen sanal makineleri için Azure (Yönetim), Klasik veya resource kullanmak istediğiniz dağıtım modelini seçin.
7. Veri çoğaltmak için kullanmak istediğiniz Azure depolama hesabı seçin.
8. Yük devretme sonrasında oluşturulan Azure VM'lerinin bağlanacağı Azure ağını ve alt ağını seçin.
9. Koruma için seçtiğiniz tüm makinelere ağ ayarını uygulamak için **Seçili makineler için şimdi yapılandır**’ı seçin. Makineler için Azure ağını ayrı ayrı seçmek için **Daha sonra yapılandır**'ı seçin.
10. **Sanal Makineler** > **Sanal makine seçin** seçeneklerine tıklayın ve çoğaltmak istediğiniz makineleri seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. Daha sonra, **Tamam**'a tıklayın.
11. İçinde **özellikleri** > **özelliklerini yapılandırma**, işlem sunucusu tarafından otomatik olarak makinede Mobility hizmeti yüklemek için kullanılacak hesabı seçin.
12. İçinde **çoğaltma ayarları** > **çoğaltma ayarlarını yapılandırın**, doğru Çoğaltma İlkesi'nin seçili olduğunu doğrulayın.
13. Tıklatın **çoğaltmasını etkinleştir**.

İlerleme durumunu izleyebilirsiniz **korumayı etkinleştir** iş **ayarları** > **işleri** > **Site Recovery işleri**. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.

Eklediğiniz sanal makineleri izlemek için son keşfedilen zamanı Vm'lerde denetleyebilirsiniz **yapılandırma sunucuları**
> **Son ilgili kişi**. VM'ler için zamanlanmış bulma beklemeden eklemek için yapılandırma sunucusu vurgulayın (tıklatın yok), tıklatıp **yenileme**.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma tatbikatı çalıştırma](site-recovery-test-failover-to-azure.md)
