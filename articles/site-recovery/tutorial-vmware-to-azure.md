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
ms.date: 12/11/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 5810ff908d48fc4ff742d734e7c2457fdfe8cb03
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Şirket içi VMware Vm'leri için olağanüstü durum kurtarma Azure ayarlama

Bu öğretici Windows çalıştıran Azure olağanüstü durum kurtarma şirket içi VMware Vm'leri için nasıl ayarlanacağını gösterir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Çoğaltma kaynağını ve hedef belirtin.
> * Şirket içi Site Recovery bileşenleri de dahil olmak üzere kaynak çoğaltma ortamı ve hedef çoğaltma ortamı ayarlayın.
> * Çoğaltma ilkesi oluşturma
> * Bir sanal makine için çoğaltmayı etkinleştirme

Bir dizi üçüncü öğreticide budur. Bu öğreticinin önceki eğitimlerine görevleri önceden tamamlamış varsayılır:

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
2. [Şirket içi VMware’leri hazırlama](tutorial-prepare-on-premises-vmware.md)

Başlamadan önce için yararlı [mimarisi gözden](concepts-vmware-to-azure-architecture.md) olağanüstü durum kurtarma senaryosuna.


## <a name="select-a-replication-goal"></a>Çoğaltma hedefi seçin

1. İçinde **kurtarma Hizmetleri kasaları**, kasa adını tıklatın **ContosoVMVault**.
2. İçinde **Başlarken**, Site Kurtarma'yı tıklatın. Ardından **altyapıyı hazırlama**.
3. İçinde **koruma hedefi** > **bulunan makinelerinizi nerede**seçin **şirket içi**.
4. İçinde ** istediğiniz makinelerinizi çoğaltmak seçin **için Azure**.
5. İçinde **sanallaştırılmış, makine**seçin **Evet, VMware vSphere hiper yönetici ile**. Daha sonra, **Tamam**'a tıklayın.

## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Kaynak ortamı ayarlamak için Site Recovery birleşik Kurulumu dosyasını indirin. Şirket içi Site Recovery bileşenlerini yüklemek, VMware sunucuları kasaya kaydetmek ve şirket içi sanal makineleri bulmak için Kurulumu çalıştırın.

### <a name="verify-on-premises-site-recovery-requirements"></a>Şirket içi Site kurtarma gereksinimlerini doğrulayın

Ana bilgisayar şirket içi Site Recovery bileşenleri için tek, yüksek oranda kullanılabilir, şirket içi VMware VM gerekir. Yapılandırma sunucusu, işlem sunucusu ve ana hedef sunucusu bileşenleri içerir.

- Yapılandırma sunucusu yerinde bileşenler ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir.
- İşlem sunucusu çoğaltma ağ geçidi olarak davranır. Çoğaltma verilerini alıp bu verileri önbelleğe alma, sıkıştırma ve şifreleme işlemleriyle iyileştirir ve Azure depolama alanına gönderir. İşlem sunucusu çoğaltmak istediğiniz sanal makinelerin de Mobility hizmeti yükler ve şirket içi VMware vm'lerinin otomatik bulma işlemini gerçekleştirir.
- Ana hedef sunucusu azure'dan yeniden çalışma sırasında çoğaltma verilerini işler.

VM aşağıdaki gereksinimleri karşılaması.

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

Buna ek olarak: 
- VM üzerindeki sistem saatinin bir saat sunucusuyla eşitlendiğinden emin olun. Saat 15 dakika içinde eşitlenmelidir. Büyük olan kurulum başarısız olur.
Kurulum başarısız olur.
- Yapılandırma sunucusu VM bu URL'leri erişebildiğinden emin olun:

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- IP adresi tabanlı güvenlik duvarı kuralları Azure ile iletişim kurmaya izin verdiğinden emin olun.
    - İzin [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653)443 (HTTPS) bağlantı noktası ve 9443 (veri çoğaltma) bağlantı noktası.
    - IP adres aralıklarını aboneliğinizin Azure bölgesi ve Batı ABD (erişim denetimi ve kimlik yönetimi için kullanılan) izin verir.


### <a name="download-the-site-recovery-unified-setup-file"></a>Site Recovery birleşik Kurulumu dosyasını indirin

1. Kasadaki > **altyapıyı hazırlama**, tıklatın **kaynak**.
1. İçinde **kaynağı**, tıklatın **+ yapılandırma sunucusu**.
2. İçinde **Sunucu Ekle**, denetleyin **yapılandırma sunucusu** görünür **sunucu türü**.
3. Site Recovery birleşik Kurulumu yükleme dosyasını indirin.
4. Kasa kayıt anahtarını indir Birleşik Kurulum'u çalıştırdığınızda bu gerekir. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

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

Vm'leri bulma şirket içi VMware sunucularına bağlanmak yapılandırma sunucusu gerekir. Bu öğreticinin amaçları doğrultusunda vCenter sunucusu veya vSphere ana bilgisayarları, sunucu üzerinde yönetici ayrıcalıklarına sahip bir hesabı kullanarak ekleyin. Bu hesap içinde oluşturduğunuz [önceki Öğreticisi](tutorial-prepare-on-premises-vmware.md). 

Hesap eklemek için:

1. Yapılandırma sunucusunda VM başlatma **CSPSConfigtool.exe**. Masaüstünde kısayol olarak bulunur ve *yükleme konumu*\home\svsystems\bin klasöründedir.

2. **Hesapları Yönet** > **Hesap Ekle**’ye tıklayın.

   ![Hesap ekleme](./media/tutorial-vmware-to-azure/credentials1.png)

3. **Hesap Ayrıntıları** menüsünde otomatik bulma için kullanılacak hesabı ekleyin.

   ![Ayrıntılar](./media/tutorial-vmware-to-azure/credentials2.png)

VMware server eklemek için:

1. Açık [Azure portal](https://portal.azure.com) ve tıklayın **tüm kaynakları**.
2. Adlı kurtarma hizmeti kasaya tıklayın **ContosoVMVault**.
3. Tıklatın **Site kurtarma** > **altyapıyı hazırlama** > **kaynağı**
4. Seçin **+ vCenter**, bir vCenter sunucusu veya vSphere ESXi ana bilgisayara bağlanmak için.
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
