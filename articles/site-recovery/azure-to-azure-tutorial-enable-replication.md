---
title: Azure Site Recovery ile Azure VM’leri için ikincil Azure bölgesine olağanüstü durum kurtarma ayarlama
description: Azure Site Recovery (Önizleme) ile Azure VM’leri için ikincil Azure bölgesine olağanüstü durum kurtarmanın nasıl ayarlanacağını öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 11/27/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 0aa7b7f3558bab7f3553527e03c44d71dd5a87ac
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52833151"
---
# <a name="set-up-disaster-recovery-for-azure-vms-to-a-secondary-azure-region"></a>Azure VM’leri için ikincil Azure bölgesine olağanüstü durum kurtarma ayarlama

[Azure Site Recovery](site-recovery-overview.md) Hizmet Yönetimi ve çoğaltma, yük devretme ve şirket içi makinelerin ve Azure sanal makineleri (VM) yeniden çalışma işlemlerini olağanüstü durum kurtarma stratejinize katkıda bulunur.

Bu öğreticide Azure VM’leri için ikincil Azure bölgesine olağanüstü durum kurtarmayı nasıl ayarlayacağını açıklanmaktadır. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Kurtarma Hizmetleri kasası oluşturma
> * Hedef kaynak ayarlarını doğrulama
> * VM’ler için giden erişim ayarlama
> * VM için çoğaltmayı etkinleştirme

> [!NOTE]
> Bu makalede olağanüstü durum kurtarma ile basit ayarları dağıtmak için yönergeler sağlar. Özelleştirilmiş ayarlar hakkında bilgi edinmek istiyorsanız, altında makaleleri inceleyin [nasıl için bölüm](azure-to-azure-how-to-enable-replication.md). o

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

- [Senaryo mimarisini ve bileşenlerini ](concepts-azure-to-azure-architecture.md) anladığınızdan emin olun.
- Tüm bileşenler için [destek gereksinimlerini](site-recovery-support-matrix-azure-to-azure.md) gözden geçirin.

## <a name="create-a-vault"></a>Kasa oluşturma

Kasayı, kaynak bölgesi dışında herhangi bir bölgede oluşturun.

1. [Azure Portal](https://portal.azure.com) > **Kurtarma Hizmetleri**’nde oturum açın.
2. **Kaynak oluştur** > **İzleme ve Yönetim** > **Backup ve Site Recovery** öğesine tıklayın.
3. **Ad** alanında, kasayı tanımlamak için bir kolay ad belirtin. Birden fazla aboneliğiniz varsa uygun olanı seçin.
4. Kaynak grubu oluşturun veya var olan bir grubu seçin. Bir Azure bölgesi belirtin. Desteklenen bölgeleri kontrol etmek için [Azure Site Recovery Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/) bölümündeki coğrafi kullanılabilirlik kısmına bakın.
5. Panodan kasaya hızlıca erişmek için önce **Panoya sabitle** seçeneğine ve sonra **Oluştur**’a tıklayın.

   ![Yeni kasa](./media/azure-to-azure-tutorial-enable-replication/new-vault-settings.png)

   Yeni kasa, **Pano**’da **Tüm kaynaklar** bölümüne ve ana **Kurtarma Hizmetleri kasaları** sayfasına eklenir.

## <a name="verify-target-resources"></a>Hedef kaynaklarını doğrulama

1. Azure aboneliğinizi hedef bölgede VM'ler oluşturmanıza izin verdiğinden emin olun. Gerekli kotayı sağlamak için desteğe başvurun.
2. Aboneliğinizin kaynak VM'lerin aynı VM boyutlarını desteklemek için yeterli kaynakları içerdiğinden emin olun. Site Recovery, aynı boyutta veya hedef sanal makine için en yakın olası boyutu seçer.

## <a name="configure-outbound-network-connectivity"></a>Giden ağ bağlantısını yapılandırma

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

  - [Microsoft Azure Veri Merkezi IP Aralıkları](https://www.microsoft.com/en-us/download/details.aspx?id=41653)
  - [Almanya’daki Windows Azure Veri Merkezi IP Aralıkları](https://www.microsoft.com/en-us/download/details.aspx?id=54770)
  - [Çin’deki Windows Azure Veri Merkezi IP Aralıkları](https://www.microsoft.com/en-us/download/details.aspx?id=42064)
  - [Office 365 URL’leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity)
  - [Site Recovery hizmeti uç nokta IP adresleri](https://aka.ms/site-recovery-public-ips)

Bu [betik](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702) gerekli NSG kurallarını oluşturmak için.

## <a name="verify-azure-vm-certificates"></a>Azure VM sertifikalarını doğrulama

Çoğaltmak istediğiniz Vm'leri en son kök sertifikalar olup olmadığını denetleyin. VM sağlanmıyorsa olamaz güvenlik kısıtlamaları nedeniyle Site Recovery'ye kayıtlı.

- Windows VM’ler için, güvenilir kök sertifikaların tamamı makinede mevcut olacak şekilde sanal makineye en son Windows güncelleştirmelerinin tümünü yükleyin. Bağlantısı kesilmiş bir ortamda, kuruluşunuz için standart Windows Update ve sertifika güncelleştirme işlemlerini uygulayın.
- Linux VM'lerde, VM’deki en son güvenilen kök sertifikalarını ve sertifika iptal listesini almak için Linux dağıtıcınız tarafından sağlanan yönergeleri uygulayın.

## <a name="set-permissions-on-the-account"></a>Hesapta izinleri ayarlama

Azure Site Recovery, Site Recovery yönetim işlemlerini denetlemek için üç yerleşik rol sağlar.

- **Site Recovery Katkıda Bulunanı** - Bu rol, Kurtarma Hizmetleri kasasında Site Recovery işlemlerini yönetmek için gereken tüm izinlere sahiptir. Ancak bu role sahip kullanıcı, Kurtarma Hizmetleri kasasını oluşturamaz veya silemez ya da diğer kullanıcılara erişim hakkı atayamaz. Bu rol, uygulamalar veya kuruluşların tamamı için olağanüstü durum kurtarmayı etkinleştirebilen ve yönetebilen olağanüstü durum kurtarma yöneticileri için idealdir.

- **Site Recovery Operatörü** - Bu rol, Yük Devretme ve Yeniden Çalışma işlemlerini yürütme ve yönetme izinlerine sahiptir. Bu role sahip olan kullanıcı çoğaltmayı etkinleştirip devre dışı bırakabilir, kasaları oluşturup silebilir, yeni altyapılar kaydedebilir veya diğer kullanıcılara erişim hakkı atayabilir. Bu rol, uygulama sahipleri ve BT yöneticileri tarafından istendiğinde sanal makinelerin veya uygulamaların yükünü devredebilen olağanüstü durum kurtarma operatörü için idealdir. Olağanüstü durum sonrası çözümde DR operatörü sanal makineleri yeniden koruyabilir ve yeniden çalıştırabilir.

- **Site Recovery Okuyucusu** - Bu rol tüm Site Recovery yönetim işlemlerini görüntüleme iznine sahiptir. Bu rol, mevcut koruma durumunu izleyebilen ve destek biletleri oluşturabilen BT izleme yöneticisi için idealdir.

Daha fazla bilgi edinin [Azure RBAC yerleşik rolleri](../role-based-access-control/built-in-roles.md)

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

### <a name="select-the-source"></a>Kaynağı seçme

1. Kurtarma Hizmetleri kasalarında, Kasa adı > **+Çoğalt** seçeneğine tıklayın.
2. **Kaynak** bölümünde **Azure** seçeneğini belirleyin.
3. **Kaynak konumu**’nda, VM’lerinizin çalışmakta olduğu kaynak Azure bölgesini seçin.
4. Sanal makinelerin çalıştığı **Kaynak aboneliği** seçin. Bu abonelik, kurtarma hizmetleri kasanızın bulunduğu Azure Active Directory kiracısında bulunan aboneliklerden biri olabilir.
5. Kaynak Yöneticisi VM’leri için **Kaynak grubu**’nu veya klasik VM’ler için **bulut hizmeti**'ni seçin.
6. Ayarları kaydetmek için **Tamam**’a tıklayın.

### <a name="select-the-vms"></a>VM’leri seçme

Site Recovery, abonelik ve kaynak grup/bulut hizmeti ile ilişkili VM’lerin listesini alır.

1. **Sanal Makineler** bölümünde çoğaltmak istediğiniz VM’leri seçin.
2. **Tamam** düğmesine tıklayın.

### <a name="configure-replication-settings"></a>Çoğaltma ayarlarını yapılandırma

Site Recovery, hedef bölge için varsayılan ayarları ve çoğaltma ilkesini oluşturur. Ayarları gerektiği gibi değiştirebilirsiniz.

1. Hedef ve çoğaltma ayarlarını görüntülemek için **Ayarlar**’a tıklayın.
2. Varsayılan hedef ayarlarını geçersiz kılmak için tıklayın **Özelleştir** yanındaki **kaynak grubu, ağ, depolama ve kullanılabilirlik**.

  ![Ayarları yapılandırma](./media/azure-to-azure-tutorial-enable-replication/settings.png)


3. Hedef ayarlarını aşağıdaki gibi özelleştirebilirsiniz:

    - **Hedef abonelik**: Olağanüstü durum kurtarma için kullanılan hedef abonelik. Hedef abonelik varsayılan olarak kaynak abonelikle aynı olur. Aynı Azure Active Directory kiracısındaki farklı bir hedef aboneliği seçmek için 'Özelleştir'e tıklayın.
    - **Hedef konum**: Olağanüstü durum kurtarma için kullanılan hedef bölge. Hedef konumun Site Recovery kasasının konumuyla eşleşmesini öneririz.
    - **Hedef kaynak grubu**: Yük devretme işleminden sonra VM’lerin tutulduğu hedef bölgedeki kaynak grup. Site Recovery, varsayılan olarak hedef bölgede "asr" sonekine sahip yeni bir kaynak grup oluşturur. hedef kaynak grubunun kaynak grubu konumu, sanal makinelerinizin barındırıldığı bölge dışındaki herhangi bir bölge olabilir.
    - **Hedef sanal ağ**: Yük devretme işleminden sonra VM’lerin bulunduğu hedef bölgedeki ağ.
      Site Recovery varsayılan olarak hedef bölgede "asr" sonekine sahip yeni bir sanal ağ (ve alt ağlarını) oluşturur.
    - **Önbellek depolama hesapları**: Site Recovery, kaynak bölgede bir depolama hesabı kullanır. Kaynak VM’lere yönelik değişiklikler, hedef konuma çoğaltılmadan önce bu hesaba gönderilir.
      >[!NOTE]
      >Güvenlik Duvarı etkin önbellek depolama hesabı kullanıyorsanız, 'güvenilen Microsoft hizmetlerinin izin ver' emin olun. [Daha fazla bilgi edinin.](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions)
      >

    - **Hedef depolama hesapları (Kaynak sanal makine yönetilen diskleri kullanmıyorsa)**: Site Recovery varsayılan olarak kaynak sanal makine depolama hesabını yansıtmak için hedef bölgede yeni bir depolama hesabı oluşturur.
      >[!NOTE]
      >Güvenlik Duvarı etkin kaynak veya hedef depolama hesabı kullanıyorsanız, 'güvenilen Microsoft hizmetlerinin izin ver' emin olun. [Daha fazla bilgi edinin.](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions)
      >

    - **Yönetilen çoğaltma diskleri (Kaynak sanal makine yönetilen diskleri kullanıyorsa)**: Site Recovery varsayılan olarak kaynak sanal makinenin yönetilen diskiyle aynı depolama türüne (Standart veya premium) sahip kaynak sanal makinenin yönetilen disklerini yansıtmak için hedef bölgede yönetilen çoğaltma diskleri oluşturur.
    - **Hedef kullanılabilirlik kümeleri**: varsayılan olarak, Azure Site Recovery, yeni bir kullanılabilirlik Vm'leri bir kullanılabilirlik kümesi kaynak bölgede bir parçası için "asr" sonekine sahip adı ile hedef bölgede kümesi oluşturur. Kullanılabilirlik kümesi zaten Azure Site Recovery tarafından oluşturulan mevcut durumda yeniden kullanılır.
    - **Hedef kullanılabilirlik**: hedef bölge kullanılabilirlik destekliyorsa, varsayılan olarak, Site Recovery hedef bölge kaynak bölgede aynı bölge sayısına atar. 

    Hedef bölge kullanılabilirlik bölgelerini desteklemez, hedef Vm'leri varsayılan olarak tek örnekleri olarak yapılandırılır. Gerekirse, tür VM'ler 'Özelleştir' tıklayarak kullanılabilirlik kümeleri hedef bölgede bir parçası olarak yapılandırabilirsiniz.

    >[!NOTE]
    >Çoğaltmayı etkinleştirdikten sonra kullanılabilirlik türü - tek örnek, kullanılabilirlik kümesi veya kullanılabilirlik alanı değiştiremezsiniz. Devre dışı bırakın ve çoğaltma kullanılabilirlik türünü değiştirmek etkinleştirmeniz gerekir.
    >

4. Çoğaltma İlkesi ayarlarını özelleştirmek için tıklatın **Özelleştir** yanındaki **Çoğaltma İlkesi**ve aşağıdaki ayarları gerektiği gibi değiştirin:

    - **Çoğaltma ilkesi adı**: İlke adı.
    - **Kurtarma noktası bekletme**: Site Recovery varsayılan olarak kurtarma noktalarını 24 saat boyunca saklar. 1 ile 72 saat arasında bir değer yapılandırabilirsiniz.
    - **Uygulamayla tutarlı anlık görüntü sıklığı**: Site Recovery varsayılan olarak 4 saatte bir uygulamayla tutarlı bir anlık görüntü alır. 1 ile 12 saat arasında bir değer yapılandırabilirsiniz. Uygulamayla tutarlı anlık görüntü, VM’nin içindeki uygulama verilerinin belirli bir noktadaki anlık görüntüsüdür. Birim Gölge Kopyası Hizmeti (VSS), anlık görüntü alınırken VM’deki uygulamanın tutarlı bir durumda olmasını sağlar.
    - **Çoğaltma grubu**: Uygulamanız VM’ler arasında çoklu VM tutarlığına ihtiyaç duyuyorsa, bu VM’ler için bir çoğaltma grubu oluşturabilirsiniz. Seçilen VM’ler varsayılan olarak hiçbir çoğaltma grubunun parçası değildir.

5. İçinde **Özelleştir**seçin **Evet** Vm'leri yeni veya mevcut çoğaltma grubuna eklemek istiyorsanız, çoklu VM tutarlılığı için. Vm'leri yapmak için bir çoğaltma grubunun parçası. Daha sonra, **Tamam**'a tıklayın.

    - Bir çoğaltma grubundaki tüm makineler, yük devredildiğinde paylaşılan kilitlenmeyle tutarlı ve uygulamayla tutarlı kurtarma noktalarına sahip olur. Çoklu VM tutarlılığını etkinleştirmek, iş yükü performansını etkileyebileceğinden yalnızca makineler aynı iş yükünü çalıştırıyorsa ve birden çok makinede tutarlığa ihtiyaç duyuyorsanız kullanılmalıdır.
    - Çoklu VM tutarlılığını etkinleştirirseniz çoğaltma grubundaki makineler birbiriyle 20004 numaralı bağlantı noktası üzerinden iletişim kurar. VM’ler arasında 20004 numaralı bağlantı noktası üzerinden gerçekleştirilen iç iletişimi engelleyen bir güvenlik duvarı gereci olmadığından emin olun. Linux VM’lerinin çoğaltma grubunun bir parçası olmasını istiyorsanız, 20004 numaralı bağlantı noktası üzerinden giden trafiğin, belirli Linux sürümünün kılavuzuna göre el ile açıldığından emin olun.

### <a name="configure-encryption-settings"></a>Şifreleme ayarlarını yapılandırma

Kaynak sanal makinenin etkinleştirilmiş Azure disk şifrelemesi (ADE) varsa, şifreleme ayarları gösterilir:

1. Şifreleme ayarları gözden geçirin.
    - **Disk şifrelemesi anahtar kasaları**: Azure Site Recovery varsayılan olarak hedef bölgede kaynak VM disk şifrelemesi anahtarlarını temel alan "asr" son ekine sahip yeni bir anahtar kasası oluşturur. Azure Site Recovery tarafından oluşturulmuş bir anahtar kasası varsa yeniden kullanılır.
    - **Anahtar şifreleme anahtar kasaları**: Azure Site Recovery varsayılan olarak hedef bölgede kaynak VM anahtar şifreleme anahtarlarını temel alan "asr" son ekine sahip yeni bir anahtar kasası oluşturur. Azure Site Recovery tarafından oluşturulmuş bir anahtar kasası varsa yeniden kullanılır.

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
