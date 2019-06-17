---
title: Azure'da olağanüstü durum kurtarma için fiziksel şirket içi sunucuları Azure Site Recovery ile ayarlama | Microsoft Docs
description: Azure Site Recovery hizmeti ile şirket içi Windows ve Linux sunucuları için azure'a olağanüstü durum kurtarma ayarlamayı öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 05/30/2019
ms.author: raynew
ms.openlocfilehash: c3b9aa6fcf5cf96e3ef1f3bdd76e9f1d19be5c5c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66400112"
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-physical-servers"></a>Şirket içi fiziksel sunucuların azure'a olağanüstü durum kurtarmayı ayarlama

[Azure Site Recovery](site-recovery-overview.md) hizmeti, şirket içi makinelerin ve Azure sanal makinelerinin (VM) çoğaltma, yük devretme ve yeniden çalışma işlemlerini yöneterek ve düzenleyerek olağanüstü durum kurtarma stratejinize katkı sağlar.

Bu öğreticide, şirket içi fiziksel Windows ve Linux sunucularını azure'a olağanüstü durum kurtarma ayarlama işlemini göstermektedir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure ve şirket içi önkoşulları ayarlama
> * Site Recovery için Kurtarma Hizmetleri kasası oluşturma 
> * Hedef çoğaltma ortamları ve kaynağını Ayarla
> * Çoğaltma ilkesi oluşturma
> * Bir sunucu için çoğaltmayı etkinleştirme

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

- Anladığınızdan emin olun [mimarisini ve bileşenlerini](physical-azure-architecture.md) bu senaryo için.
- Tüm bileşenler için [destek gereksinimlerini](vmware-physical-secondary-support-matrix.md) gözden geçirin.
- Çoğaltmak istediğiniz sunucuları ile uyumlu olduğundan emin [Azure VM gereksinimleri](vmware-physical-secondary-support-matrix.md#replicated-vm-support).
- Azure'ı hazırlama. Bir Azure aboneliği, bir Azure sanal ağı ve bir depolama hesabı gerekir.
- Çoğaltmak istediğiniz her sunucuda Mobility hizmetinin otomatik olarak yüklemek için bir hesap hazırlama.

Başlamadan önce dikkat edin:

- Azure'a yük devretme işleminden sonra fiziksel sunucuları şirket içi fiziksel makinelerine devredilemez. Yalnızca VMware sanal makinelerini de başarısız olabilir. 
- Bu öğreticide basit ayarlarla azure'a fiziksel sunucu olağanüstü durum kurtarma ayarlar. Diğer seçenekler hakkında bilgi edinmek istiyorsanız, bizim nasıl yapılır kılavuzları okuyun:
    - Ayarlanan [çoğaltma kaynağı](physical-azure-set-up-source.md), Site Recovery yapılandırma sunucusu da dahil olmak üzere.
    - [Çoğaltma hedefini](physical-azure-set-up-target.md) ayarlayın.
    - Bir [çoğaltma ilkesi](vmware-azure-set-up-replication.md) yapılandırın ve [çoğaltmayı etkinleştirin](vmware-azure-enable-replication.md).


### <a name="set-up-an-azure-account"></a>Bir Azure hesabı ayarlama

Bir Microsoft alma [Azure hesabı](https://azure.microsoft.com/).

- [Ücretsiz deneme sürümüyle](https://azure.microsoft.com/pricing/free-trial/) başlayabilirsiniz.
- Hakkında bilgi edinin [Site Recovery fiyatlandırma](site-recovery-faq.md#pricing)ve [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/).
- Hangi [bölgeleri desteklenir](https://azure.microsoft.com/pricing/details/site-recovery/) Site Recovery için.

### <a name="verify-azure-account-permissions"></a>Azure hesap izinlerini doğrulama

Azure hesabınızı Vm'lerini azure'a çoğaltma için izinleri olduğundan emin olun.

- Gözden geçirme [izinleri](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) makinelerin Azure'a çoğaltılması gerekir.
- Doğrulama ve değiştirme [rol tabanlı erişim](../role-based-access-control/role-assignments-portal.md) izinleri. 



### <a name="set-up-an-azure-network"></a>Azure ağı ayarlama

Ayarlanmış bir [Azure ağı](../virtual-network/quick-create-portal.md).

- Yük devretme sonrasında oluşturulan azure VM'ler bu ağda yerleştirilir.
- Ağın kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir


## <a name="set-up-an-azure-storage-account"></a>Azure depolama hesabı ayarlama

Ayarlanmış bir [Azure depolama hesabı](../storage/common/storage-quickstart-create-account.md).

- Site Recovery, şirket içi makinelerin Azure depolama alanına çoğaltır. Yük devretme gerçekleştikten sonra depolamadan azure sanal makineleri oluşturulur.
- Depolama hesabının, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.


### <a name="prepare-an-account-for-mobility-service-installation"></a>Bir hesabı Mobility hizmeti yüklemesi için hazırlama

Çoğaltmak istediğiniz her sunucuda Mobility hizmetinin yüklenmesi gerekir. Sunucu için çoğaltmayı etkinleştirdiğinizde site Recovery bu hizmeti otomatik olarak yükler. Otomatik olarak yüklemek için Site Recovery'nin sunucuya erişmek için kullanacağı bir hesap hazırlamanız gerekir.

- Bir etki alanı veya yerel hesap kullanabilirsiniz
- Windows yerel makinede uzak kullanıcı erişim denetimini devre dışı bir etki alanı hesabı kullanmıyorsanız VM'ler için. Kayıt altında Bunu yapmak için **hkey_local_machıne\software\microsoft\windows\currentversion\policies\system**, DWORD girişini ekleyin **LocalAccountTokenFilterPolicy**, 1 değerine sahip.
- CLI ayarını devre dışı bırakmak için kayıt defteri girdisini eklemek için şunu yazın:       ``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
- Linux için hesabın kaynak Linux sunucusunda kök olması gerekir.


## <a name="create-a-vault"></a>Kasa oluşturma

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>Koruma hedefi seçme

Neleri çoğaltmak ve kendisine çoğaltmak için seçin.

1. **Kurtarma Hizmetleri kasaları** > kasa öğesine tıklayın.
2. Kaynak Menüsünde, **Site Recovery** > **Altyapıyı Hazırlama** > **Koruma hedefi** seçeneklerine tıklayın.
3. İçinde **koruma hedefi**seçin **Azure'a** > **sanallaştırılmamış/diğer**.

## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Yapılandırma sunucusu ayarlarsınız, kasaya kaydedin ve Vm'leri keşfedin.

1. Tıklayın **Site kurtarma** > **altyapıyı hazırlama** > **kaynak**.
2. Yapılandırma sunucusu yoksa, tıklayın **+ yapılandırma sunucusu**.
3. İçinde **Sunucusu Ekle**, kontrol **yapılandırma sunucusu** görünür **sunucu türü**.
4. Site Recovery birleşik Kurulumu yükleme dosyasını indirin.
5. Kasa kayıt anahtarını indir Birleşik Kurulumu çalıştırırken buna ihtiyacınız olur. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

   ![Kaynağı ayarlama](./media/physical-azure-disaster-recovery/source-environment.png)


### <a name="register-the-configuration-server-in-the-vault"></a>Yapılandırma sunucusunu kasaya kaydedin.

Başlamadan önce aşağıdakileri yapın: 

#### <a name="verify-time-accuracy"></a>Zaman hassasiyeti doğrulayın
Configuration server makinesinde sistem saatinin eşitlendiğinden emin olun bir [saat sunucusu](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Eşleşmelidir. 15 dakika önde ise veya arkasında kurulum başarısız olabilir.

#### <a name="verify-connectivity"></a>Bağlantısını doğrula
Makinenin ortamınıza bağlı olarak bu URL'lere erişebildiğinden emin olun: 

[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]  

IP adresi tabanlı güvenlik duvarı kurallarına yukarıda listelenen Azure URL'lerini tüm iletişim HTTPS (443) bağlantı noktası üzerinden izin vermelidir. Basitleştirmek ve IP aralıkları sınırlamak için URL filtreleme yapılması önerilir.

- **Ticari IP'ler** -izin [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653)ve HTTPS (443) bağlantı noktası. AAD, yedekleme, çoğaltma ve depolama URL'leri desteklemek için aboneliğinizin Azure bölgesi için IP adresi aralıklarına izin verin.  
- **Kamu IP'ler** -izin [Azure devlet kurumları veri merkezi IP aralıkları](https://www.microsoft.com/en-us/download/details.aspx?id=57063)ve HTTPS (443) bağlantı noktası (Virginia, Texas, Arizona ve Iowa) AAD, yedekleme, çoğaltma ve depolama URL'leri desteklemek için tüm USGov bölgeler için.  

#### <a name="run-setup"></a>Kurulumu çalıştırma
Birleşik Kurulum bir yerel yapılandırma sunucusunu yüklemek için yönetici olarak çalıştırın. Ayrıca işlem sunucusu ve ana hedef sunucusu yapılandırma sunucusunda varsayılan olarak yüklenir.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

Kayıt tamamlandıktan sonra yapılandırma sunucusu görüntülenen **ayarları** > **sunucuları** kasadaki sayfası.

## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Hedef kaynaklarını seçin ve doğrulayın.

1. **Altyapıyı hazırlama** > **Hedef** seçeneklerine tıklayıp kullanmak istediğiniz Azure aboneliğini seçin.
2. Hedef dağıtım modelini belirtin.
3. Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.

   ![Hedef](./media/physical-azure-disaster-recovery/network-storage.png)


## <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

1. Yeni bir çoğaltma ilkesi oluşturmak için **Site Recovery altyapısı** > **Çoğaltma İlkeleri** >  **+Çoğaltma İlkesi**’ne tıklayın.
2. **Çoğaltma ilkesi oluştur** bölümünde bir ilke adı belirtin.
3. **RPO eşiği** bölümünde kurtarma noktası hedefi (RPO) sınırını belirtin. Bu değer, ne sıklıkta belirtir veri kurtarma noktaları oluşturulur. Devamlı çoğaltma bu sınırı aşarsa bir uyarı oluşturulur.
4. **Kurtarma noktası bekletme** bölümünde, her kurtarma noktası için bekletme süresinin ne kadar olacağını (saat) belirtin. Çoğaltılan VM’ler bir aralıktaki herhangi bir noktaya kurtarılabilir. Premium depolama alanına çoğaltılan makineler için 24 saate, standart depolama için de 72 saate kadar bekletme desteklenir.
5. İçinde **uygulamayla tutarlı anlık görüntü sıklığı**, belirtin uygulamayla tutarlı anlık görüntüleri içeren kurtarma noktalarının ne sıklıkta (dakika cinsinden) oluşturulur. İlkeyi oluşturmak için **Tamam**’a tıklayın.

    ![Çoğaltma ilkesi](./media/physical-azure-disaster-recovery/replication-policy.png)


İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir. Eşleşen bir ilke, varsayılan olarak yeniden çalışma için otomatik oluşturulur. Örneğin, çoğaltma ilkesi **rep-policy** sonra bir yeniden çalışma ilkesi **rep-policy-failback** oluşturulur. Bu ilke Azure’dan bir yeniden çalışma başlatılana kadar kullanılmaz.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Her sunucu için çoğaltmayı etkinleştirin.

- Çoğaltma etkinleştirildiğinde site Recovery Mobility hizmetini yükler.
- Bir sunucu için çoğaltmayı etkinleştirdiğinizde, 15 dakika alabilir veya değişikliklerin etkili olması ve portalda görüntülenmesi daha uzun.

1. **Uygulama çoğaltma** > **Kaynak** seçeneğine tıklayın.
2. **Kaynak** bölümünde yapılandırma sunucusunu seçin.
3. İçinde **makine türü**seçin **fiziksel makineler**.
4. İşlem sunucusunu (yapılandırma sunucusu) seçin. Daha sonra, **Tamam**'a tıklayın.
5. İçinde **hedef**, abonelik ve yük devretme sonrasında Azure Vm'leri oluşturmak istediğiniz kaynak grubunu seçin. (Klasik veya kaynak yönetimi) Azure'da kullanmak istediğiniz dağıtım modelini seçin.
6. Verileri çoğaltmak için kullanmak istediğiniz Azure depolama hesabını seçin. 
7. Yük devretme sonrasında oluşturulan Azure VM'lerinin bağlanacağı Azure ağını ve alt ağını seçin.
8. Koruma için seçtiğiniz tüm makinelere ağ ayarını uygulamak için **Seçili makineler için şimdi yapılandır**’ı seçin. Makineler için Azure ağını ayrı ayrı seçmek için **Daha sonra yapılandır**'ı seçin. 
9. İçinde **fiziksel makineler**, tıklatıp **+ fiziksel makine**. Adı ve IP adresi belirtin. Çoğaltmak istediğiniz makinenin işletim sistemini seçin. Bulunan ve listelenen sunucuları için birkaç dakika sürer. 
10. İçinde **özellikleri** > **özelliklerini yapılandırmak**, Mobility hizmetini makineye otomatik olarak yüklemeniz için işlem sunucusu tarafından kullanılacak hesabı seçin.
11. **Çoğaltma ayarları** > **Çoğaltma ayarlarını yapılandırma** bölümünde doğru çoğaltma ilkesinin seçilip seçilmediğini doğrulayın. 
12. **Çoğaltmayı Etkinleştir**’e tıklayın. **Ayarlar** > **İşler** > **Site Recovery İşleri** bölümünden **Korumayı Etkinleştir** işinin ilerleme durumunu izleyebilirsiniz. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.


Eklediğiniz sunucuları izlemek için bunlar için son bulunma zamanını kontrol edebilirsiniz **Configuration Servers** > **Last Contact At**. Bir zamanlanmış bulma süresi beklenmeden makineler eklemek için yapılandırma sunucusunu vurgulayın (tıklamayın), tıklatıp **Yenile**.

## <a name="next-steps"></a>Sonraki adımlar

[Bir olağanüstü durum kurtarma tatbikatı çalıştırma](tutorial-dr-drill-azure.md).
