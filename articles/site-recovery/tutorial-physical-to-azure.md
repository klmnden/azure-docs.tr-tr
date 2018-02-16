---
title: "Olağanüstü durum kurtarma Azure fiziksel şirket içi sunucular için Azure Site Recovery ile ayarlama | Microsoft Docs"
description: "Azure Site Recovery hizmeti ile şirket içi Windows ve Linux sunucuları için olağanüstü durum kurtarma Azure ayarlanacağını öğrenin."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 02/07/2018
ms.author: raynew
ms.openlocfilehash: 01e582cb789e402496c920e4a8fe27d5c6848531
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-physical-servers"></a>Şirket içi fiziksel sunucuların azure'a olağanüstü durum kurtarma ayarlama

[Azure Site Recovery](site-recovery-overview.md) hizmeti yönetme ve çoğaltma, yük devretme ve yeniden çalışma şirket içi makineler ve Azure sanal makineleri (VM'ler) yönetme olağanüstü durum kurtarma stratejiniz katkıda bulunur.

Bu öğretici, şirket içi fiziksel Windows ve Linux sunucularının Azure olağanüstü durum kurtarma ayarlama gösterilmiştir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure ve şirket içi önkoşulları ayarlama
> * Site kurtarma için bir kurtarma Hizmetleri kasası oluşturma 
> * Kaynağı ayarlama ve çoğaltma ortamları hedef
> * Çoğaltma ilkesi oluşturma
> * Bir sunucu için çoğaltmayı etkinleştirme

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

- Anladığınızdan emin olun [senaryo mimarisi ve bileşenleri](concepts-physical-to-azure-architecture.md).
- Gözden geçirme [destek gereksinimleri](site-recovery-support-matrix-to-azure.md) tüm bileşenler için.
- Çoğaltmak istediğiniz sunucuları ile uyumlu olduğundan emin olun [Azure VM gereksinimleri](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
- Azure hazırlayın. Bir Azure aboneliği, Azure sanal ağı ve bir depolama hesabı gerekir.
- Mobility hizmetinin çoğaltmak istediğiniz her bir sunucuda otomatik yüklemesi için bir hesap hazırlayın.



### <a name="set-up-an-azure-account"></a>Bir Azure hesabı ayarlama

Bir Microsoft alma [Azure hesabı](http://azure.microsoft.com/).

- [Ücretsiz deneme sürümüyle](https://azure.microsoft.com/pricing/free-trial/) başlayabilirsiniz.
- Hakkında bilgi edinin [Site Recovery fiyatlandırma](site-recovery-faq.md#pricing)ve alma [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/).
- Hangi bulma [bölgeleri desteklenir](https://azure.microsoft.com/pricing/details/site-recovery/) Site kurtarma.

### <a name="verify-azure-account-permissions"></a>Azure hesabı izinleri doğrulayın

Azure hesabınız Azure VM'ler, çoğaltma için izinleri olduğundan emin olun.

- Gözden geçirme [izinleri](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) makineleri azure'a gerekir.
- Doğrulama ve değiştirme [rol tabanlı erişim](../active-directory/role-based-access-control-configure.md) izinleri. 



### <a name="set-up-an-azure-network"></a>Azure ağı ayarlama

Ayarlanmış bir [Azure ağ](../virtual-network/quick-create-portal.md).

- Yük devretme sonrasında oluşturulduğunda azure sanal makineleri bu ağda yerleştirilir.
- Ağın kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir


## <a name="set-up-an-azure-storage-account"></a>Azure depolama hesabı ayarlama

Ayarlanmış bir [Azure depolama hesabı](../storage/common/storage-create-storage-account.md#create-a-storage-account).

- Site Recovery, şirket içi makineler Azure depolama alanına çoğaltır. Yük devretme gerçekleştikten sonra azure VM'ler depolama biriminden oluşturulur.
- Depolama hesabı kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.
- Depolama hesabı standart olabilir veya [premium](../virtual-machines/windows/premium-storage.md).
- Premium hesabınızı ayarlarsanız, ek bir standart hesap için günlük verileri gerekir.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Mobility hizmeti yüklemesi için bir hesap hazırlama

Mobility hizmetinin çoğaltmak istediğiniz her sunucuda yüklenmesi gerekir. Çoğaltma sunucusu için etkinleştirdiğinizde, site kurtarma Bu hizmeti otomatik olarak yüklenir. Otomatik olarak yüklemek için Site Recovery sunucuya erişmek için kullanacağı bir hesabı hazırlamanız gerekir.

- Bir etki alanı veya yerel hesabı kullanın
- Windows etki alanı hesabı, yerel makinede uzak kullanıcı erişim denetimi devre dışı bırak kullanmıyorsanız VM'ler için. Bu, altında kayıttaki yapmak için **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, DWORD girdisi ekleyin **LocalAccountTokenFilterPolicy**, 1 değerine sahip.
- CLI ayarından devre dışı bırakmak için kayıt defteri girdisini eklemek için şunu yazın:``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
- Linux için hesabın kaynak Linux sunucusu kökünde olması gerekir.


## <a name="create-a-vault"></a>Bir kasa oluşturun

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>Koruma hedefi seçin

Neleri çoğaltmak için ve kendisine çoğaltmak için seçin.

1. Tıklatın **kurtarma Hizmetleri kasaları** > kasası.
2. Kaynak menüye tıklayın **Site Recovery** > **altyapıyı hazırlama** > **koruma hedefi**.
3. İçinde **koruma hedefi**seçin **için Azure** > **değil sanallaştırılmış/diğer**.

## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Yapılandırma sunucusunu ayarlamayı, kasaya kaydetmek ve Vm'leri bulma.

1. Tıklatın **Site kurtarma** > **altyapıyı hazırlama** > **kaynak**.
2. Yapılandırma sunucusu yoksa, tıklatın **+ yapılandırma sunucusu**.
3. İçinde **Sunucu Ekle**, denetleyin **yapılandırma sunucusu** görünür **sunucu türü**.
4. Site Recovery birleşik Kurulumu yükleme dosyasını indirin.
5. Kasa kayıt anahtarını indir Birleşik Kurulum'u çalıştırdığınızda bu gerekir. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

   ![Kaynağı ayarlama](./media/tutorial-physical-to-azure/source-environment.png)


### <a name="register-the-configuration-server-in-the-vault"></a>Yapılandırma sunucusunu kasaya kaydetmek

Başlamadan önce aşağıdakileri yapın: 

- Yapılandırma sunucusu makinede sistem saatinin eşitlendiğinden emin olun bir [saat sunucusu](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Eşleşmelidir. Önde 15 dakika ise veya arkasında kurulum başarısız olabilir.
- Makinenin bu URL'leri erişebildiğinden emin olun:[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]

- IP adresi tabanlı güvenlik duvarı kuralları Azure ile iletişim kurmaya izin vermelidir.
- [Azure Veri Merkezi IP Aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653)'na ve HTTPS (443) bağlantı noktasına izin verin.
- Aboneliğinizin Azure bölgesi ve Batı ABD (Access Control ve Identity Management için kullanılır) için IP adresi aralıklarına izin verin.

Bir yerel yapılandırma sunucusu yüklemek için yönetici olarak, birleştirilmiş Kurulumu çalıştırın. İşlem sunucusu ve ana hedef sunucusu yapılandırma sunucusuna varsayılan olarak yüklenir.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

Kayıt tamamlandıktan sonra yapılandırma sunucusu görüntülenen **ayarları** > **sunucuları** kasası sayfasında.


## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Seçin ve hedef kaynaklarını doğrulayın.

1. **Altyapıyı hazırlama** > **Hedef** seçeneklerine tıklayıp kullanmak istediğiniz Azure aboneliğini seçin.
2. Hedef dağıtım modelini belirtin.
3. Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.

   ![Hedef](./media/tutorial-physical-to-azure/network-storage.png)


## <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

1. Yeni bir çoğaltma ilkesi oluşturmak için tıklatın **Site Recovery altyapısı** > **çoğaltma ilkeleri** > **+ Çoğaltma İlkesi**.
2. İçinde **çoğaltma ilkesi oluşturun**, bir ilke adı belirtin.
3. İçinde **RPO eşik**, kurtarma noktası hedefi (RPO) sınırları belirtin. Bu değer, ne sıklıkta belirtir veri kurtarma noktaları oluşturulur. Sürekli çoğaltma bu sınırı aşarsa bir uyarı üretilir.
4. İçinde **kurtarma noktası bekletme**, (saat) cinsinden ne kadar süreyle bekletme penceresi için her kurtarma noktası belirtin. Çoğaltılan VM'ler penceresinde herhangi bir noktaya kurtarılabilir. 24 saat bekletme için premium depolama ve standart depolama için 72 saat çoğaltılan makineler için desteklenir.
5. İçinde **uygulamayla tutarlı anlık görüntü sıklığı**, belirtin ne sıklıkta (dakika cinsinden) uygulamayla tutarlı anlık görüntüleri içeren kurtarma noktaları oluşturulur. Tıklatın **Tamam** bir ilke oluşturmak için.

    ![Çoğaltma ilkesi](./media/tutorial-physical-to-azure/replication-policy.png)


İlke yapılandırma sunucusuyla otomatik olarak ilişkilendirilir. Varsayılan olarak, eşleşen bir ilkesi yeniden çalışma için otomatik olarak oluşturulur. Örneğin, çoğaltma ilkesi ise **rep İlkesi** sonra bir yeniden çalışma ilkesi **rep İlkesi yeniden** oluşturulur. Bir azure'dan başlatana kadar bu ilkeyi kullanılmaz.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Her sunucu için çoğaltma etkinleştirin.

- Çoğaltma etkin olduğunda site Recovery Mobility hizmetini yükleyin.
- Bir sunucu için çoğaltma etkinleştirdiğinizde, 15 dakika alabilir veya daha uzun değişikliklerin etkili olması ve Portalı'nda görünür.

1. Tıklatın **uygulama çoğaltma** > **kaynak**.
2. İçinde **kaynak**, yapılandırma sunucusu seçin.
3. İçinde **makine türü**seçin **fiziksel makineleri**.
4. İşlem Sunucusu (yapılandırma sunucusu) seçin. Daha sonra, **Tamam**'a tıklayın.
5. İçinde **hedef**, abonelik ve yük devretme sonrasında Azure sanal makinelerini oluşturmak istediğiniz kaynak grubunu seçin. Azure (Klasik veya resource management) kullanmak istediğiniz dağıtım modelini seçin.
6. Veri çoğaltmak için kullanmak istediğiniz Azure depolama hesabı seçin. 
7. Yük devretme sonrasında oluşturulan Azure VM'lerinin bağlanacağı Azure ağını ve alt ağını seçin.
8. Koruma için seçtiğiniz tüm makinelere ağ ayarını uygulamak için **Seçili makineler için şimdi yapılandır**’ı seçin. Makineler için Azure ağını ayrı ayrı seçmek için **Daha sonra yapılandır**'ı seçin. 
9. İçinde **fiziksel makineleri**, tıklatıp **+ fiziksel makine**. Ad ve IP adresi belirtin. Çoğaltmak istediğiniz makinenin işletim sistemini seçin. Bulunan ve listelenen sunucuları için birkaç dakika sürer. 
10. İçinde **özellikleri** > **özelliklerini yapılandırma**, işlem sunucusu tarafından otomatik olarak makinede Mobility hizmeti yüklemek için kullanılacak hesabı seçin.
11. İçinde **çoğaltma ayarları** > **çoğaltma ayarlarını yapılandırın**, doğru Çoğaltma İlkesi'nin seçili olduğunu doğrulayın. 
12. Tıklatın **çoğaltmasını etkinleştir**. İlerleme durumunu izleyebilirsiniz **korumayı etkinleştir** iş **ayarları** > **işleri** > **Site Recovery işleri**. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.


Eklediğiniz sunucularını izlemek için bunları son keşfedilen zamanını kontrol edebilirsiniz **yapılandırma sunucularına** > **en son kişi**. Makineler için zamanlanmış bulma süresini beklemeden eklemek için yapılandırma sunucusu vurgulayın (tıklatın yok), tıklatıp **yenileme**.

## <a name="next-steps"></a>Sonraki adımlar

[Olağanüstü durum kurtarma tatbikatı çalıştırma](tutorial-dr-drill-azure.md)
