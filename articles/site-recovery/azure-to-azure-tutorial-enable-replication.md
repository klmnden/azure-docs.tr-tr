---
title: Azure Site Recovery ile Azure VM’leri için ikincil Azure bölgesine olağanüstü durum kurtarma ayarlama
description: Azure Site Recovery (Önizleme) ile Azure VM’leri için ikincil Azure bölgesine olağanüstü durum kurtarmanın nasıl ayarlanacağını öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 588fe452473ddc2434d92f90afbf8a0e1bc8c275
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65795760"
---
# <a name="set-up-disaster-recovery-for-azure-vms"></a>Azure Vm'leri için olağanüstü durum kurtarmayı ayarlayın

[Azure Site Recovery](site-recovery-overview.md) Hizmet Yönetimi ve çoğaltma, yük devretme ve şirket içi makinelerin ve Azure sanal makineleri (VM) yeniden çalışma işlemlerini olağanüstü durum kurtarma stratejinize katkıda bulunur.

Bu öğreticide, bunları bir Azure bölgesinden diğerine çoğaltılması yoluyla Azure Vm'leri için olağanüstü durum kurtarmayı ayarlayın gösterilmektedir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Kurtarma Hizmetleri kasası oluşturma
> * Hedef kaynak ayarlarını doğrulama
> * VM'ler için giden ağ bağlantısı ayarlayın
> * VM için çoğaltmayı etkinleştirme

> [!NOTE]
> Bu makalede olağanüstü durum kurtarma ile basit ayarları dağıtmak için yönergeler sağlar. Özelleştirilmiş ayarlar hakkında bilgi edinmek istiyorsanız, altında makaleleri inceleyin [nasıl için bölüm](azure-to-azure-how-to-enable-replication.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

- [Senaryo mimarisini ve bileşenlerini ](concepts-azure-to-azure-architecture.md) anladığınızdan emin olun.
- Gözden geçirme [destek gereksinimlerini](site-recovery-support-matrix-azure-to-azure.md) başlamadan önce.

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Kasayı, kaynak bölgesi dışında herhangi bir bölgede oluşturun.

1. [Azure Portal](https://portal.azure.com) > **Kurtarma Hizmetleri**’nde oturum açın.
2. Tıklayın **kaynak Oluştur** > **Yönetim Araçları** > **Backup ve Site Recovery**.
3. **Ad** alanında, kasayı tanımlamak için bir kolay ad belirtin. Birden fazla aboneliğiniz varsa uygun olanı seçin.
4. Kaynak grubu oluşturun veya var olan bir grubu seçin. Bir Azure bölgesi belirtin. Desteklenen bölgeleri kontrol etmek için [Azure Site Recovery Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/) bölümündeki coğrafi kullanılabilirlik kısmına bakın.
5. Panodan kasaya hızlıca erişmek için önce **Panoya sabitle** seçeneğine ve sonra **Oluştur**’a tıklayın.

   ![Yeni kasa](./media/azure-to-azure-tutorial-enable-replication/new-vault-settings.png)

   Yeni kasa, **Pano**’da **Tüm kaynaklar** bölümüne ve ana **Kurtarma Hizmetleri kasaları** sayfasına eklenir.

## <a name="verify-target-resource-settings"></a>Hedef kaynak ayarlarını doğrulama

1. Azure aboneliğinizi hedef bölgede VM'ler oluşturmanıza izin verdiğinden emin olun. Gerekli kotayı sağlamak için desteğe başvurun.
2. Aboneliğinizin kaynak VM'lerin aynı VM boyutlarını desteklemek için yeterli kaynakları içerdiğinden emin olun. Site Recovery, aynı boyutta veya hedef sanal makine için en yakın olası boyutu seçer.

## <a name="set-up-outbound-network-connectivity-for-vms"></a>VM'ler için giden ağ bağlantısı ayarlayın

Site Recovery'nın beklendiği şekilde çalışması, çoğaltmak istediğiniz vm'lerden giden ağ bağlantısını değiştirmeniz gerekir.

> [!NOTE]
> Site Recovery, ağ bağlantısını denetlemek için kimlik doğrulama proxy'si kullanarak desteklememektedir.


### <a name="outbound-connectivity-for-urls"></a>URL'ler için giden bağlantı

Giden bağlantıyı denetlemek için bir URL tabanlı güvenlik duvarı proxy'si kullanıyorsanız, bu URL'ler erişime izin verin.

| **URL** | **Ayrıntılar** |
| ------- | ----------- |
| *.blob.core.windows.net | Verilerin VM’den kaynak bölgedeki önbellek depolama hesabına yazılmasına izin verir. |
| login.microsoftonline.com | Site Recovery hizmet URL’leri için yetkilendirme ve kimlik doğrulama özellikleri sağlar. |
| *.hypervrecoverymanager.windowsazure.com | VM’nin Site Recovery hizmetiyle iletişim kurmasına izin verir. |
| *.servicebus.windows.net | VM’nin Site Recovery izleme ve tanılama verilerini yazmasına izin verir. |

### <a name="outbound-connectivity-for-ip-address-ranges"></a>IP adresi aralıkları için giden bağlantı

URL'ler yerine IP adreslerini kullanarak giden bağlantıyı denetlemek istiyorsanız, IP tabanlı güvenlik duvarı, proxy veya NSG kuralları için bu adresleri sağlar.

  - [Microsoft Azure Veri Merkezi IP Aralıkları](https://www.microsoft.com/download/details.aspx?id=41653)
  - [Almanya’daki Windows Azure Veri Merkezi IP Aralıkları](https://www.microsoft.com/download/details.aspx?id=54770)
  - [Çin’deki Windows Azure Veri Merkezi IP Aralıkları](https://www.microsoft.com/download/details.aspx?id=42064)
  - [Office 365 URL’leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity)
  - [Site Recovery hizmeti uç nokta IP adresleri](https://aka.ms/site-recovery-public-ips)

NSG kullanıyorsanız, depolama hizmet etiketini Kaynak bölgesi için NSG kuralları oluşturabilirsiniz. [Daha fazla bilgi edinin](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges).

## <a name="verify-azure-vm-certificates"></a>Azure VM sertifikalarını doğrulama

Çoğaltmak istediğiniz Vm'leri en son kök sertifikalar olup olmadığını denetleyin. VM sağlanmıyorsa olamaz güvenlik kısıtlamaları nedeniyle Site Recovery'ye kayıtlı.

- Windows VM’ler için, güvenilir kök sertifikaların tamamı makinede mevcut olacak şekilde sanal makineye en son Windows güncelleştirmelerinin tümünü yükleyin. Bağlantısı kesilmiş bir ortamda, kuruluşunuz için standart Windows Update ve sertifika güncelleştirme işlemlerini uygulayın.
- Linux VM'lerde, VM’deki en son güvenilen kök sertifikalarını ve sertifika iptal listesini almak için Linux dağıtıcınız tarafından sağlanan yönergeleri uygulayın.

## <a name="set-permissions-on-the-account"></a>Hesapta izinleri ayarlama

Azure Site Recovery, Site Recovery yönetim işlemlerini denetlemek için üç yerleşik rol sağlar.

- **Site Recovery Katkıda Bulunanı** - Bu rol, Kurtarma Hizmetleri kasasında Site Recovery işlemlerini yönetmek için gereken tüm izinlere sahiptir. Ancak bu role sahip kullanıcı, Kurtarma Hizmetleri kasasını oluşturamaz veya silemez ya da diğer kullanıcılara erişim hakkı atayamaz. Bu rol, uygulamalar veya kuruluşların tamamı için olağanüstü durum kurtarmayı etkinleştirebilen ve yönetebilen olağanüstü durum kurtarma yöneticileri için idealdir.

- **Site Recovery Operatörü** - Bu rol, Yük Devretme ve Yeniden Çalışma işlemlerini yürütme ve yönetme izinlerine sahiptir. Bu role sahip olan kullanıcı çoğaltmayı etkinleştirip devre dışı bırakabilir, kasaları oluşturup silebilir, yeni altyapılar kaydedebilir veya diğer kullanıcılara erişim hakkı atayabilir. Bu rol, uygulama sahipleri ve BT yöneticileri tarafından istendiğinde sanal makinelerin veya uygulamaların yükünü devredebilen olağanüstü durum kurtarma operatörü için idealdir. Olağanüstü durum sonrası çözümde DR operatörü sanal makineleri yeniden koruyabilir ve yeniden çalıştırabilir.

- **Site Recovery Okuyucusu** - Bu rol tüm Site Recovery yönetim işlemlerini görüntüleme iznine sahiptir. Bu rol, mevcut koruma durumunu izleyebilen ve destek biletleri oluşturabilen BT izleme yöneticisi için idealdir.

Daha fazla bilgi edinin [Azure RBAC yerleşik rolleri](../role-based-access-control/built-in-roles.md).

## <a name="enable-replication-for-a-vm"></a>VM için çoğaltmayı etkinleştirme

### <a name="select-the-source"></a>Kaynağı seçme

1. Kurtarma Hizmetleri kasalarında, Kasa adı > **+Çoğalt** seçeneğine tıklayın.
2. **Kaynak** bölümünde **Azure** seçeneğini belirleyin.
3. **Kaynak konumu**’nda, VM’lerinizin çalışmakta olduğu kaynak Azure bölgesini seçin.
4. Sanal makinelerin çalıştığı **Kaynak aboneliği** seçin. Bu abonelik, kurtarma hizmetleri kasanızın bulunduğu Azure Active Directory kiracısında bulunan aboneliklerden biri olabilir.
5. Seçin **kaynak kaynak grubu**, tıklatıp **Tamam** ayarları kaydetmek için.

    ![Kaynağı ayarlama](./media/azure-to-azure-tutorial-enable-replication/source.png)

### <a name="select-the-vms"></a>VM’leri seçme

Site Recovery, abonelik ve kaynak grup/bulut hizmeti ile ilişkili VM’lerin listesini alır.

1. **Sanal Makineler** bölümünde çoğaltmak istediğiniz VM’leri seçin.
2. **Tamam**'ı tıklatın.

### <a name="configure-replication-settings"></a>Çoğaltma ayarlarını yapılandırma

Site Recovery, hedef bölge için varsayılan ayarları ve çoğaltma ilkesini oluşturur. Ayarları gerektiği gibi değiştirebilirsiniz.

1. Hedef ve çoğaltma ayarlarını görüntülemek için **Ayarlar**’a tıklayın.
2. Varsayılan hedef ayarlarını geçersiz kılmak için tıklayın **Özelleştir** yanındaki **kaynak grubu, ağ, depolama ve kullanılabilirlik**.

   ![Ayarları yapılandır](./media/azure-to-azure-tutorial-enable-replication/settings.png)


3. Hedef ayarlarını tabloda özetlendiği gibi özelleştirin.

    **Ayar** | **Ayrıntılar**
    --- | ---
    **Hedef aboneliği** | Varsayılan olarak, hedef abonelik kaynak abonelik ile aynıdır. Aynı Azure Active Directory kiracısındaki farklı bir hedef aboneliği seçmek için 'Özelleştir'e tıklayın.
    **Hedef konum** | Olağanüstü durum kurtarma için kullanılan hedef bölge.<br/><br/> Hedef konumun Site Recovery kasasının konumuyla eşleşmesini öneririz.
    **Hedef kaynak grubu** | Yük devretme sonrasında Azure Vm'leri tutan hedef bölgedeki kaynak grubu.<br/><br/> Site Recovery, varsayılan olarak hedef bölgede "asr" sonekine sahip yeni bir kaynak grup oluşturur. Hedef kaynak grubu konumunu, kaynak sanal makineleri barındırdığı bölge dışında herhangi bir bölgeyi olabilir.
    **Hedef sanal ağ** | Yük devretme işleminden sonra VM'lerin bulunduğu hedef bölgedeki ağ.<br/><br/> Site Recovery varsayılan olarak hedef bölgede "asr" sonekine sahip yeni bir sanal ağ (ve alt ağlarını) oluşturur.
    **Önbellek depolama hesapları** | Site Recovery kaynak bölgede bir depolama hesabı kullanır. Kaynak VM’lere yönelik değişiklikler, hedef konuma çoğaltılmadan önce bu hesaba gönderilir.<br/><br/> Bir güvenlik duvarı etkin önbellek depolama hesabı kullanıyorsanız, etkinleştirdiğinizden emin olun **izin güvenilen Microsoft Hizmetleri**. [Daha fazla bilgi edinin.](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions)
    **Hedef depolama hesapları (kaynak VM, yönetilmeyen diskler kullanır)** | Varsayılan olarak, Site Recovery, kaynak VM depolama hesabını yansıtmak için hedef bölgede yeni bir depolama hesabı oluşturur.<br/><br/> Etkinleştirme **izin güvenilen Microsoft Hizmetleri** bir güvenlik duvarı etkin önbellek depolama hesabı kullanıyorsanız.
    **Yönetilen çoğaltma diskleri (kaynak kullanan sanal makine yönetilen diskleri kullanmıyorsa)** | Varsayılan olarak, Site Recovery kaynak sanal makinenin yönetilen disklerle aynı depolama türüne (standart veya premium) kaynağın VM'ye yönetilen disk olarak yansıtmak için hedef bölgede yönetilen çoğaltma diskleri oluşturur. Yalnızca Disk türü özelleştirebilirsiniz 
    **Hedef kullanılabilirlik kümeleri** | Varsayılan olarak, Azure Site Recovery, yeni bir kullanılabilirlik Vm'leri bir kullanılabilirlik kümesi kaynak bölgede bir parçası için "asr" sonekine sahip adı ile hedef bölgede kümesi oluşturur. Kullanılabilirlik kümesi zaten Azure Site Recovery tarafından oluşturulan mevcut durumda yeniden kullanılır.
    **Hedef kullanılabilirlik alanları** | Kullanılabilirlik alanları hedef bölge destekliyorsa, varsayılan olarak, Site Recovery hedef bölge kaynak bölgede aynı bölge sayısına atar.<br/><br/> Hedef bölge kullanılabilirlik desteklemiyorsa, hedef Vm'leri varsayılan olarak tek örnekleri olarak yapılandırılır.<br/><br/> Tıklayın **Özelleştir** Vm'leri bir kullanılabilirlik kümesi hedef bölgede bir parçası olarak yapılandırılır.<br/><br/> (Tek örnek, kullanılabilirlik kümesi veya kullanılabilirlik alanı) kullanılabilirlik türü değiştirilemiyor. çoğaltmayı etkinleştirdikten sonra. Devre dışı bırakın ve çoğaltma kullanılabilirlik türünü değiştirmek etkinleştirmeniz gerekir.

4. Çoğaltma İlkesi ayarlarını özelleştirmek için tıklatın **Özelleştir** yanındaki **Çoğaltma İlkesi**ve ayarları gerektiği gibi değiştirin.

    **Ayar** | **Ayrıntılar**
    --- | ---
    **Çoğaltma İlkesi adı** | İlke adı.
    **Kurtarma noktası bekletme** | Varsayılan olarak, Site Recovery kurtarma noktalarını 24 saat boyunca saklar. 1 ile 72 saat arasında bir değer yapılandırabilirsiniz.
    **Uygulamayla tutarlı anlık görüntü sıklığı** | Varsayılan olarak, Site Recovery, 4 saatte bir uygulamayla tutarlı anlık görüntüsünü alır. 1 ile 12 saat arasında bir değer yapılandırabilirsiniz.<br/><br/> Uygulamayla tutarlı bir anlık görüntü, VM'nin içindeki uygulama verilerinin zaman içinde nokta anlık görüntüsüdür. Birim Gölge Kopyası Hizmeti (VSS), anlık görüntü alınırken VM’deki uygulamanın tutarlı bir durumda olmasını sağlar.
    **Çoğaltma grubu** | Uygulamanız VM'ler arasında çoklu VM tutarlılığı gerekiyorsa, bu sanal makineler için çoğaltma grubu oluşturabilirsiniz. Seçilen VM’ler varsayılan olarak hiçbir çoğaltma grubunun parçası değildir.

5. İçinde **Özelleştir**seçin **Evet** Vm'leri yeni veya mevcut çoğaltma grubuna eklemek istiyorsanız, çoklu VM tutarlılığı için. Daha sonra, **Tamam**'a tıklayın. 

    >[!NOTE]
    >- Bir çoğaltma grubundaki tüm makineler, yük devredildiğinde kilitlenme tutarlı ve uygulamayla tutarlı kurtarma noktalarını paylaştığı.
    >- Çoklu VM tutarlılığını etkinleştirmek, (CPU kullanımı yoğun olduğu) iş yükü performansını etkileyebilir. Yalnızca makineler aynı iş yükünü çalıştırıyorsa ve birden fazla makine arasında tutarlılık ihtiyacınız varsa kullanılmalıdır.
    >- Bir çoğaltma grubunda en fazla 16 sanal makinelerinin olabilir.
    >- Çoklu VM tutarlılığını etkinleştirirseniz çoğaltma grubundaki makineler birbiriyle 20004 numaralı bağlantı noktası üzerinden iletişim kurar. Bu bağlantı noktası üzerinden sanal makineler arasında iç iletişimi engelleyen güvenlik duvarı olduğundan emin olun.
    >- Çoğaltma grubunda Linux VM'ler için 20004 numaralı bağlantı noktasında giden trafik Kılavuzu Linux sürümü için uygun olarak el ile açıldığından emin olun.



### <a name="configure-encryption-settings"></a>Şifreleme ayarlarını yapılandırma

Kaynak VM, Azure disk şifrelemesi (ADE) etkin varsa, ayarları gözden geçirin.

1. Ayarları doğrulayın:
    - **Disk şifreleme anahtar kasalarını**: Varsayılan olarak, Site Recovery, kaynak VM disk şifreleme anahtarları, bir "asr" sonekine sahip yeni bir anahtar kasası oluşturur. Anahtar kasası zaten varsa, yeniden kullanılır.
    - **Anahtar şifreleme anahtar kasalarını**: Varsayılan olarak, Site Recovery, hedef bölgede yeni bir anahtar kasası oluşturur. Adı "asr" sonekine sahip ve kaynak VM anahtar şifreleme anahtarlarını alır. Site Recovery tarafından önceden oluşturulmuş bir anahtar kasası zaten varsa, yeniden kullanılır.

2. Tıklayın **Özelleştir** özel anahtar Kasası'nı seçin.

>[!NOTE]
>Yalnızca Windows işletim sistemlerini çalıştıran Azure Vm'leri ve [şifrelemesi ile Azure AD uygulaması için etkin](https://aka.ms/ade-aad-app) şu anda Azure Site Recovery tarafından desteklenir.
>

### <a name="track-replication-status"></a>Çoğaltma durumunu izleme

1. En son durumu görüntülemek için **Ayarlar**’da **Yenile**’ye tıklayın.
2. İlerleme ve durumu aşağıdaki gibi izleyin:
    - İlerleme durumunu izlemek **korumayı etkinleştir** işinin **ayarları** > **işleri** > **Site Recovery işleri**.
    - **Ayarlar** > **Çoğaltılan Öğeler** bölümünde VM’lerin durumunu ve ilk çoğaltma ilerleme durumunu görüntüleyebilirsiniz. Ayarlarının detayına gitmek için VM’ye tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure VM’si için olağanüstü durum kurtarmayı yapılandırdınız. Artık bu yük devretme beklendiği gibi çalıştığını denetlemek için olağanüstü durum kurtarma tatbikatı başlatabilirsiniz.

> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma tatbikatı çalıştırma](azure-to-azure-tutorial-dr-drill.md)
