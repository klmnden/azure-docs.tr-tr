---
title: Azure Site Recovery ile Azure VM’leri için ikincil Azure bölgesine olağanüstü durum kurtarma ayarlama
description: Azure Site Recovery (Önizleme) ile Azure VM’leri için ikincil Azure bölgesine olağanüstü durum kurtarmanın nasıl ayarlanacağını öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 07/06/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 4638b697dcaa0d4c11bae1878a94f76f6237d4a4
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2018
ms.locfileid: "42154790"
---
# <a name="set-up-disaster-recovery-for-azure-vms-to-a-secondary-azure-region"></a>Azure VM’leri için ikincil Azure bölgesine olağanüstü durum kurtarma ayarlama

[Azure Site Recovery](site-recovery-overview.md) hizmeti, şirket içi makinelerin ve Azure sanal makinelerinin (VM) çoğaltma, yük devretme ve yeniden çalışma işlemlerini yöneterek ve düzenleyerek olağanüstü durum kurtarma stratejinize katkı sağlar.

Bu öğreticide Azure VM’leri için ikincil Azure bölgesine olağanüstü durum kurtarmayı nasıl ayarlayacağını açıklanmaktadır. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Kurtarma Hizmetleri kasası oluşturma
> * Hedef kaynak ayarlarını doğrulama
> * VM’ler için giden erişim ayarlama
> * VM için çoğaltmayı etkinleştirme


## <a name="prerequisites"></a>Ön koşullar

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

1. Azure aboneliğinizin, olağanüstü durum kurtarma için kullanılan hedef bölgede VM’ler oluşturmanıza izin verdiğinden emin olun. Gerekli kotayı sağlamak için desteğe başvurun.

2. Aboneliğinizin, kaynak VM’lerinize uygun boyutlardaki VM’leri desteklemek için yeterli kaynakları içerdiğinden emin olun. Site Recovery, hedef VM için aynı boyutu veya mümkün olan en yakın boyutu seçer.

## <a name="configure-outbound-network-connectivity"></a>Giden ağ bağlantısını yapılandırma

Site Recovery’nin beklenen şekilde çalışması için, çoğaltmak istediğiniz VM’lerden giden ağ bağlantısında bazı değişiklikler yapmanız gerekir.

- Site Recovery, ağ bağlantısını denetlemek için kimlik doğrulama proxy’si kullanımını desteklemez.
- Kimlik doğrulama proxy’sine sahipseniz çoğaltma etkinleştirilemez.

### <a name="outbound-connectivity-for-urls"></a>URL'ler için giden bağlantı

Giden bağlantıyı denetlemek için URL tabanlı bir güvenli duvarı kullanıyorsanız, Site Recovery tarafından kullanılan aşağıdaki URL’lere erişim izni verin.

| **URL** | **Ayrıntılar** |
| ------- | ----------- |
| *.blob.core.windows.net | Verilerin VM’den kaynak bölgedeki önbellek depolama hesabına yazılmasına izin verir. |
| login.microsoftonline.com | Site Recovery hizmet URL’leri için yetkilendirme ve kimlik doğrulama özellikleri sağlar. |
| *.hypervrecoverymanager.windowsazure.com | VM’nin Site Recovery hizmetiyle iletişim kurmasına izin verir. |
| *.servicebus.windows.net | VM’nin Site Recovery izleme ve tanılama verilerini yazmasına izin verir. |

### <a name="outbound-connectivity-for-ip-address-ranges"></a>IP adresi aralıkları için giden bağlantı

URL’ler yerine IP adreslerini kullanarak giden bağlantıyı denetlemek istiyorsanız, IP tabanlı güvenlik duvarları, ara sunucu veya NSG kuralları için uygun veri merkezi aralıklarını, Office 365 adreslerini ve hizmet uç noktası adreslerini beyaz listeye ekleyin.

  - [Microsoft Azure Veri Merkezi IP Aralıkları](http://www.microsoft.com/en-us/download/details.aspx?id=41653)
  - [Almanya’daki Windows Azure Veri Merkezi IP Aralıkları](http://www.microsoft.com/en-us/download/details.aspx?id=54770)
  - [Çin’deki Windows Azure Veri Merkezi IP Aralıkları](http://www.microsoft.com/en-us/download/details.aspx?id=42064)
  - [Office 365 URL’leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity)
  - [Site Recovery hizmeti uç nokta IP adresleri](https://aka.ms/site-recovery-public-ips)

Gerekli NSG kurallarını oluşturmak için bu [betiği](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702) kullanabilirsiniz.

## <a name="verify-azure-vm-certificates"></a>Azure VM sertifikalarını doğrulama

Çoğaltmak istediğiniz Windows veya Linux VM’lerinde en son kök sertifikalarının tümünün mevcut olup olmadığını kontrol edin. En son kök sertifikalar mevcut değilse sanal makine, güvenlik kısıtlamaları nedeniyle Site Recovery'ye kaydedilemez.

- Windows VM'lerde, güvenilen kök sertifikaların tamamı makinede mevcut olacak şekilde, VM’ye en son Windows güncelleştirmelerinin tümünü yükleyin. Bağlantısı kesilmiş bir ortamda, kuruluşunuz için standart Windows Update ve sertifika güncelleştirme işlemlerini uygulayın.

- Linux VM'lerde, VM’deki en son güvenilen kök sertifikalarını ve sertifika iptal listesini almak için Linux dağıtıcınız tarafından sağlanan yönergeleri uygulayın.

## <a name="set-permissions-on-the-account"></a>Hesapta izinleri ayarlama

Azure Site Recovery, Site Recovery yönetim işlemlerini denetlemek için üç yerleşik rol sağlar.

- **Site Recovery Katkıda Bulunanı** - Bu rol, Kurtarma Hizmetleri kasasında Site Recovery işlemlerini yönetmek için gereken tüm izinlere sahiptir. Ancak bu role sahip kullanıcı, Kurtarma Hizmetleri kasasını oluşturamaz veya silemez ya da diğer kullanıcılara erişim hakkı atayamaz. Bu rol, uygulamalar veya kuruluşların tamamı için olağanüstü durum kurtarmayı etkinleştirebilen ve yönetebilen olağanüstü durum kurtarma yöneticileri için idealdir.

- **Site Recovery Operatörü** - Bu rol, Yük Devretme ve Yeniden Çalışma işlemlerini yürütme ve yönetme izinlerine sahiptir. Bu role sahip olan kullanıcı çoğaltmayı etkinleştirip devre dışı bırakabilir, kasaları oluşturup silebilir, yeni altyapılar kaydedebilir veya diğer kullanıcılara erişim hakkı atayabilir. Bu rol, uygulama sahipleri ve BT yöneticileri tarafından istendiğinde sanal makinelerin veya uygulamaların yükünü devredebilen olağanüstü durum kurtarma operatörü için idealdir. Olağanüstü durum sonrası çözümde DR operatörü sanal makineleri yeniden koruyabilir ve yeniden çalıştırabilir.

- **Site Recovery Okuyucusu** - Bu rol tüm Site Recovery yönetim işlemlerini görüntüleme iznine sahiptir. Bu rol, mevcut koruma durumunu izleyebilen ve destek biletleri oluşturabilen BT izleme yöneticisi için idealdir.

[Azure RBAC yerleşik rolleri](../role-based-access-control/built-in-roles.md) hakkında daha fazla bilgi edinin

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

### <a name="select-the-source"></a>Kaynağı seçme

1. Kurtarma Hizmetleri kasalarında, Kasa adı > **+Çoğalt** seçeneğine tıklayın.
2. **Kaynak** bölümünde **Azure** seçeneğini belirleyin.
3. **Kaynak konumu**’nda, VM’lerinizin çalışmakta olduğu kaynak Azure bölgesini seçin.
4. VM’ler için **Azure sanal makine dağıtım modeli**’ni seçin: **Kaynak Yöneticisi** veya **Klasik**.
5. Sanal makinelerin çalıştığı **Kaynak aboneliği** seçin. Bu abonelik, kurtarma hizmetleri kasanızın bulunduğu Azure Active Directory kiracısında bulunan aboneliklerden biri olabilir.
6. Kaynak Yöneticisi VM’leri için **Kaynak grubu**’nu veya klasik VM’ler için **bulut hizmeti**'ni seçin.
7. Ayarları kaydetmek için **Tamam**’a tıklayın.

### <a name="select-the-vms"></a>VM’leri seçme

Site Recovery, abonelik ve kaynak grup/bulut hizmeti ile ilişkili VM’lerin listesini alır.

1. **Sanal Makineler** bölümünde çoğaltmak istediğiniz VM’leri seçin.
2. **Tamam** düğmesine tıklayın.

### <a name="configure-replication-settings"></a>Çoğaltma ayarlarını yapılandırma

Site Recovery, hedef bölge için varsayılan ayarları ve çoğaltma ilkesini oluşturur. Bu ayarları gereksinimlerinize göre değiştirebilirsiniz.

1. Hedef ve çoğaltma ayarlarını görüntülemek için **Ayarlar**’a tıklayın.
2. Varsayılan hedef ayarlarını geçersiz kılmak için **Kaynak grubu, Ağ, Depolama ve Kullanılabilirlik Kümeleri**’nin yanındaki **Özelleştir** seçeneğine tıklayın.

  ![Ayarları yapılandırma](./media/azure-to-azure-tutorial-enable-replication/settings.png)


- **Hedef abonelik**: Olağanüstü durum kurtarma için kullanılan hedef abonelik. Hedef abonelik varsayılan olarak kaynak abonelikle aynı olur. Aynı Azure Active Directory kiracısındaki farklı bir hedef aboneliği seçmek için 'Özelleştir'e tıklayın.

- **Hedef konum**: Olağanüstü durum kurtarma için kullanılan hedef bölge. Hedef konumun Site Recovery kasasının konumuyla eşleşmesini öneririz.

- **Hedef kaynak grubu**: Yük devretme işleminden sonra VM’lerin tutulduğu hedef bölgedeki kaynak grup. Site Recovery, varsayılan olarak hedef bölgede "asr" sonekine sahip yeni bir kaynak grup oluşturur. hedef kaynak grubunun kaynak grubu konumu, sanal makinelerinizin barındırıldığı bölge dışındaki herhangi bir bölge olabilir.

- **Hedef sanal ağ**: Yük devretme işleminden sonra VM’lerin bulunduğu hedef bölgedeki ağ.
  Site Recovery varsayılan olarak hedef bölgede "asr" sonekine sahip yeni bir sanal ağ (ve alt ağlarını) oluşturur.

- **Önbellek depolama hesapları**: Site Recovery, kaynak bölgede bir depolama hesabı kullanır. Kaynak VM’lere yönelik değişiklikler, hedef konuma çoğaltılmadan önce bu hesaba gönderilir.

- **Hedef depolama hesapları (Kaynak sanal makine yönetilen diskleri kullanmıyorsa)**: Site Recovery varsayılan olarak kaynak sanal makine depolama hesabını yansıtmak için hedef bölgede yeni bir depolama hesabı oluşturur.

- **Yönetilen çoğaltma diskleri (Kaynak sanal makine yönetilen diskleri kullanıyorsa)**: Site Recovery varsayılan olarak kaynak sanal makinenin yönetilen diskiyle aynı depolama türüne (Standart veya premium) sahip kaynak sanal makinenin yönetilen disklerini yansıtmak için hedef bölgede yönetilen çoğaltma diskleri oluşturur.

- **Hedef kullanılabilirlik kümeleri**: Site Recovery varsayılan olarak hedef bölgede "asr" sonekine sahip yeni bir kullanılabilirlik kümesi oluşturur. Kullanılabilirlik kümelerini yalnızca VM’ler kaynak bölgedeki bir kümenin içindeyse ekleyebilirsiniz.

Varsayılan çoğaltma ilkesi ayarlarını geçersiz kılmak için **Çoğalta ilkesi**’nin yanındaki **Özelleştir**’e tıklayın.  

- **Çoğaltma ilkesi adı**: İlke adı.

- **Kurtarma noktası bekletme**: Site Recovery varsayılan olarak kurtarma noktalarını 24 saat boyunca saklar. 1 ile 72 saat arasında bir değer yapılandırabilirsiniz.

- **Uygulamayla tutarlı anlık görüntü sıklığı**: Site Recovery varsayılan olarak 4 saatte bir uygulamayla tutarlı bir anlık görüntü alır. 1 ile 12 saat arasında bir değer yapılandırabilirsiniz. Uygulamayla tutarlı anlık görüntü, VM’nin içindeki uygulama verilerinin belirli bir noktadaki anlık görüntüsüdür. Birim Gölge Kopyası Hizmeti (VSS), anlık görüntü alınırken VM’deki uygulamanın tutarlı bir durumda olmasını sağlar.

- **Çoğaltma grubu**: Uygulamanız VM’ler arasında çoklu VM tutarlığına ihtiyaç duyuyorsa, bu VM’ler için bir çoğaltma grubu oluşturabilirsiniz. Seçilen VM’ler varsayılan olarak hiçbir çoğaltma grubunun parçası değildir.

  **Çoğaltma ilkesi**’nin yanındaki **Özelleştir** seçeneğine tıklayın ve sonra VM’leri çoğaltma grubunun bir parçası yapmak için çoklu VM tutarlılığı için **Evet**’e tıklayın. Yeni bir çoğaltma grubu oluşturabilir veya varolan bir çoğaltma grubunu kullanabilirsiniz. Çoğaltma grubunun parçası olacak VM’leri seçin ve **Tamam**’a tıklayın.

> [!IMPORTANT]
  Bir çoğaltma grubundaki tüm makineler, yük devredildiğinde paylaşılan kilitlenmeyle tutarlı ve uygulamayla tutarlı kurtarma noktalarına sahip olur. Çoklu VM tutarlılığını etkinleştirmek, iş yükü performansını etkileyebileceğinden yalnızca makineler aynı iş yükünü çalıştırıyorsa ve birden çok makinede tutarlığa ihtiyaç duyuyorsanız kullanılmalıdır.

> [!IMPORTANT]
  Çoklu VM tutarlılığını etkinleştirirseniz çoğaltma grubundaki makineler birbiriyle 20004 numaralı bağlantı noktası üzerinden iletişim kurar. VM’ler arasında 20004 numaralı bağlantı noktası üzerinden gerçekleştirilen iç iletişimi engelleyen bir güvenlik duvarı gereci olmadığından emin olun. Linux VM’lerinin çoğaltma grubunun bir parçası olmasını istiyorsanız, 20004 numaralı bağlantı noktası üzerinden giden trafiğin, belirli Linux sürümünün kılavuzuna göre el ile açıldığından emin olun.

### <a name="track-replication-status"></a>Çoğaltma durumunu izleme

1. En son durumu görüntülemek için **Ayarlar**’da **Yenile**’ye tıklayın.

2. **Ayarlar**>**İşler**>**Site Recovery işleri** bölümünde **Korumayı etkinleştir** işinin ilerleme durumunu izleyebilirsiniz.

3. **Ayarlar** > **Çoğaltılan Öğeler** bölümünde VM’lerin durumunu ve ilk çoğaltma ilerleme durumunu görüntüleyebilirsiniz. Ayarlarının detayına gitmek için VM’ye tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure VM’si için olağanüstü durum kurtarmayı yapılandırdınız. Sonraki adım yapılandırmanızı test etme.

> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma tatbikatı çalıştırma](azure-to-azure-tutorial-dr-drill.md)
