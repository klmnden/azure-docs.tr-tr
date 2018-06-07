---
title: Olağanüstü durum kurtarma Azure fiziksel şirket içi sunucular için Azure Site Recovery ile ayarlama | Microsoft Docs
description: Azure Site Recovery hizmeti ile şirket içi Windows ve Linux sunucuları için olağanüstü durum kurtarma Azure ayarlanacağını öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 05/23/2018
ms.author: raynew
ms.openlocfilehash: a4c83e495e269cdca35844a699d714b55cf1f500
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34643320"
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-physical-servers"></a>Şirket içi fiziksel sunucuların azure'a olağanüstü durum kurtarma ayarlama

[Azure Site Recovery](site-recovery-overview.md) hizmeti, şirket içi makinelerin ve Azure sanal makinelerinin (VM) çoğaltma, yük devretme ve yeniden çalışma işlemlerini yöneterek ve düzenleyerek olağanüstü durum kurtarma stratejinize katkı sağlar.

Bu öğretici, şirket içi fiziksel Windows ve Linux sunucularının Azure olağanüstü durum kurtarma ayarlama gösterilmiştir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure ve şirket içi önkoşulları ayarlama
> * Site kurtarma için bir kurtarma Hizmetleri kasası oluşturma 
> * Kaynağı ayarlama ve çoğaltma ortamları hedef
> * Çoğaltma ilkesi oluşturma
> * Bir sunucu için çoğaltmayı etkinleştirme

[Mimari gözden](concepts-hyper-v-to-azure-architecture.md) bu olağanüstü durum kurtarma senaryosuna.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

- Anladığınızdan emin olun [mimarisi ve bileşenleri](physical-azure-architecture.md) bu senaryo için.
- Tüm bileşenler için [destek gereksinimlerini](vmware-physical-secondary-support-matrix.md) gözden geçirin.
- Çoğaltmak istediğiniz sunucuları ile uyumlu olduğundan emin olun [Azure VM gereksinimleri](vmware-physical-secondary-support-matrix.md#replicated-vm-support).
- Azure hazırlayın. Bir Azure aboneliği, Azure sanal ağı ve bir depolama hesabı gerekir.
- Mobility hizmetinin çoğaltmak istediğiniz her bir sunucuda otomatik yüklemesi için bir hesap hazırlayın.

Başlamadan önce dikkat edin:

- Azure yük devretme işleminden sonra fiziksel sunucuları şirket içi fiziksel makinelerine başarısız olamaz. Yalnızca VMware sanal makinelerini başarısız olabilir. 
- Bu öğretici Azure fiziksel sunucu olağanüstü durum kurtarma basit ayarlarla ayarlar. Diğer seçenekler hakkında bilgi edinmek istiyorsanız, nasıl yapılır kılavuzlarımız okuyun:
    - Ayarlanan [çoğaltma kaynağı](physical-azure-set-up-source.md), Site Recovery yapılandırma sunucusu da dahil olmak üzere.
    - Ayarlanan [çoğaltma hedefi](physical-azure-set-up-target.md).
    - Yapılandırma bir [Çoğaltma İlkesi](vmware-azure-set-up-replication.md), ve [çoğaltmayı etkinleştirme](vmware-azure-enable-replication.md).


### <a name="set-up-an-azure-account"></a>Bir Azure hesabı ayarlama

Bir Microsoft alma [Azure hesabı](http://azure.microsoft.com/).

- [Ücretsiz deneme sürümüyle](https://azure.microsoft.com/pricing/free-trial/) başlayabilirsiniz.
- Hakkında bilgi edinin [Site Recovery fiyatlandırma](site-recovery-faq.md#pricing)ve alma [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/).
- Hangi bulma [bölgeleri desteklenir](https://azure.microsoft.com/pricing/details/site-recovery/) Site kurtarma.

### <a name="verify-azure-account-permissions"></a>Azure hesabı izinleri doğrulayın

Azure hesabınız Azure VM'ler, çoğaltma için izinleri olduğundan emin olun.

- Gözden geçirme [izinleri](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) makineleri azure'a gerekir.
- Doğrulama ve değiştirme [rol tabanlı erişim](../role-based-access-control/role-assignments-portal.md) izinleri. 



### <a name="set-up-an-azure-network"></a>Azure ağı ayarlama

Ayarlanmış bir [Azure ağ](../virtual-network/quick-create-portal.md).

- Yük devretme sonrasında oluşturulduğunda azure sanal makineleri bu ağda yerleştirilir.
- Ağın kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir


## <a name="set-up-an-azure-storage-account"></a>Azure depolama hesabı ayarlama

Ayarlanmış bir [Azure depolama hesabı](../storage/common/storage-create-storage-account.md#create-a-storage-account).

- Site Recovery, şirket içi makineler Azure depolama alanına çoğaltır. Yük devretme gerçekleştikten sonra azure VM'ler depolama biriminden oluşturulur.
- Depolama hesabının, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.
- Depolama hesabı standart olabilir veya [premium](../virtual-machines/windows/premium-storage.md).
- Premium hesabınızı ayarlarsanız, ek bir standart hesap için günlük verileri gerekir.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Bir hesabı Mobility hizmeti yüklemesi için hazırlama

Mobility hizmetinin çoğaltmak istediğiniz her sunucuda yüklenmesi gerekir. Çoğaltma sunucusu için etkinleştirdiğinizde, site kurtarma Bu hizmeti otomatik olarak yüklenir. Otomatik olarak yüklemek için Site Recovery sunucuya erişmek için kullanacağı bir hesabı hazırlamanız gerekir.

- Bir etki alanı veya yerel hesabı kullanın
- Windows etki alanı hesabı, yerel makinede uzak kullanıcı erişim denetimi devre dışı bırak kullanmıyorsanız VM'ler için. Bu, altında kayıttaki yapmak için **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, DWORD girdisi ekleyin **LocalAccountTokenFilterPolicy**, 1 değerine sahip.
- CLI ayarından devre dışı bırakmak için kayıt defteri girdisini eklemek için şunu yazın:       ``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
- Linux için hesabın kaynak Linux sunucusu kökünde olması gerekir.


## <a name="create-a-vault"></a>Kasa oluşturma

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>Koruma hedefi seçin

Neleri çoğaltmak için ve kendisine çoğaltmak için seçin.

1. **Kurtarma Hizmetleri kasaları** > kasa öğesine tıklayın.
2. Kaynak Menüsünde, **Site Recovery** > **Altyapıyı Hazırlama** > **Koruma hedefi** seçeneklerine tıklayın.
3. İçinde **koruma hedefi**seçin **için Azure** > **değil sanallaştırılmış/diğer**.

## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Yapılandırma sunucusunu ayarlamayı, kasaya kaydetmek ve Vm'leri bulma.

1. Tıklatın **Site kurtarma** > **altyapıyı hazırlama** > **kaynak**.
2. Yapılandırma sunucusu yoksa, tıklatın **+ yapılandırma sunucusu**.
3. İçinde **Sunucu Ekle**, denetleyin **yapılandırma sunucusu** görünür **sunucu türü**.
4. Site Recovery birleşik Kurulumu yükleme dosyasını indirin.
5. Kasa kayıt anahtarını indirin. Birleşik Kurulum'u çalıştırdığınızda bu gerekir. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

   ![Kaynağı ayarlama](./media/physical-azure-disaster-recovery/source-environment.png)


### <a name="register-the-configuration-server-in-the-vault"></a>Yapılandırma sunucusunu kasaya kaydetmek

Başlamadan önce aşağıdakileri yapın: 

- Yapılandırma sunucusu makinede sistem saatinin eşitlendiğinden emin olun bir [saat sunucusu](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Eşleşmelidir. Önde 15 dakika ise veya arkasında kurulum başarısız olabilir.
- Makinenin bu URL'leri erişebildiğinden emin olun:       [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]

- IP adresi tabanlı güvenlik duvarı kuralları Azure ile iletişim kurmaya izin vermelidir.
- [Azure Veri Merkezi IP Aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653)'na ve HTTPS (443) bağlantı noktasına izin verin.
- Aboneliğinizin Azure bölgesi ve Batı ABD (Access Control ve Identity Management için kullanılır) için IP adresi aralıklarına izin verin.

Bir yerel yapılandırma sunucusu yüklemek için yönetici olarak, birleştirilmiş Kurulumu çalıştırın. İşlem sunucusu ve ana hedef sunucusu yapılandırma sunucusuna varsayılan olarak yüklenir.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

Kayıt tamamlandıktan sonra yapılandırma sunucusu görüntülenen **ayarları** > **sunucuları** kasası sayfasında.


## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Hedef kaynaklarını seçin ve doğrulayın.

1. **Altyapıyı hazırlama** > **Hedef** seçeneklerine tıklayıp kullanmak istediğiniz Azure aboneliğini seçin.
2. Hedef dağıtım modelini belirtin.
3. Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.

   ![Hedef](./media/physical-azure-disaster-recovery/network-storage.png)


## <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

1. Yeni bir çoğaltma ilkesi oluşturmak için tıklatın **Site Recovery altyapısı** > **çoğaltma ilkeleri** > **+ Çoğaltma İlkesi**.
2. İçinde **çoğaltma ilkesi oluşturun**, bir ilke adı belirtin.
3. İçinde **RPO eşik**, kurtarma noktası hedefi (RPO) sınırları belirtin. Bu değer, ne sıklıkta belirtir veri kurtarma noktaları oluşturulur. Devamlı çoğaltma bu sınırı aşarsa bir uyarı oluşturulur.
4. İçinde **kurtarma noktası bekletme**, (saat) cinsinden ne kadar süreyle bekletme penceresi için her kurtarma noktası belirtin. Çoğaltılan VM’ler bir aralıktaki herhangi bir noktaya kurtarılabilir. 24 saat bekletme için premium depolama ve standart depolama için 72 saat çoğaltılan makineler için desteklenir.
5. İçinde **uygulamayla tutarlı anlık görüntü sıklığı**, belirtin ne sıklıkta (dakika cinsinden) uygulamayla tutarlı anlık görüntüleri içeren kurtarma noktaları oluşturulur. İlkeyi oluşturmak için **Tamam**’a tıklayın.

    ![Çoğaltma ilkesi](./media/physical-azure-disaster-recovery/replication-policy.png)


İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir. Eşleşen bir ilke, varsayılan olarak yeniden çalışma için otomatik oluşturulur. Örneğin, çoğaltma ilkesi ise **rep İlkesi** sonra bir yeniden çalışma ilkesi **rep İlkesi yeniden** oluşturulur. Bu ilke Azure’dan bir yeniden çalışma başlatılana kadar kullanılmaz.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Her sunucu için çoğaltma etkinleştirin.

- Çoğaltma etkin olduğunda site Recovery Mobility hizmetini yükleyin.
- Bir sunucu için çoğaltma etkinleştirdiğinizde, 15 dakika alabilir veya daha uzun değişikliklerin etkili olması ve Portalı'nda görünür.

1. **Uygulama çoğaltma** > **Kaynak** seçeneğine tıklayın.
2. **Kaynak** bölümünde yapılandırma sunucusunu seçin.
3. İçinde **makine türü**seçin **fiziksel makineleri**.
4. İşlem Sunucusu (yapılandırma sunucusu) seçin. Daha sonra, **Tamam**'a tıklayın.
5. İçinde **hedef**, abonelik ve yük devretme sonrasında Azure sanal makinelerini oluşturmak istediğiniz kaynak grubunu seçin. Azure (Klasik veya resource management) kullanmak istediğiniz dağıtım modelini seçin.
6. Verileri çoğaltmak için kullanmak istediğiniz Azure depolama hesabını seçin. 
7. Yük devretme sonrasında oluşturulan Azure VM'lerinin bağlanacağı Azure ağını ve alt ağını seçin.
8. Koruma için seçtiğiniz tüm makinelere ağ ayarını uygulamak için **Seçili makineler için şimdi yapılandır**’ı seçin. Makineler için Azure ağını ayrı ayrı seçmek için **Daha sonra yapılandır**'ı seçin. 
9. İçinde **fiziksel makineleri**, tıklatıp **+ fiziksel makine**. Ad ve IP adresi belirtin. Çoğaltmak istediğiniz makinenin işletim sistemini seçin. Bulunan ve listelenen sunucuları için birkaç dakika sürer. 
10. İçinde **özellikleri** > **özelliklerini yapılandırma**, işlem sunucusu tarafından otomatik olarak makinede Mobility hizmeti yüklemek için kullanılacak hesabı seçin.
11. **Çoğaltma ayarları** > **Çoğaltma ayarlarını yapılandırma** bölümünde doğru çoğaltma ilkesinin seçilip seçilmediğini doğrulayın. 
12. **Çoğaltmayı Etkinleştir**’e tıklayın. **Ayarlar** > **İşler** > **Site Recovery İşleri** bölümünden **Korumayı Etkinleştir** işinin ilerleme durumunu izleyebilirsiniz. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.


Eklediğiniz sunucularını izlemek için bunları son keşfedilen zamanını kontrol edebilirsiniz **yapılandırma sunucularına** > **en son kişi**. Makineler için zamanlanmış bulma süresini beklemeden eklemek için yapılandırma sunucusu vurgulayın (tıklatın yok), tıklatıp **yenileme**.

## <a name="next-steps"></a>Sonraki adımlar

[Bir olağanüstü durum kurtarma ayrıntıya çalıştırmak](tutorial-dr-drill-azure.md).
