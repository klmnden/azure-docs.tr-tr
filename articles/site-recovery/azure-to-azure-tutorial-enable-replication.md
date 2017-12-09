---
title: "Olağanüstü durum kurtarma, ikincil bir Azure bölgesine Azure Site Recovery (Önizleme) ile Azure VM'ler için ayarlayın."
description: "Azure Site Recovery hizmetini kullanarak farklı Azure bölgesine, Azure VM'ler için olağanüstü durum kurtarma ayarlanacağını öğrenin."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/01/2017
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 2608e0e0c87df1e7c6d034cf0977ed0e16b128cf
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="set-up-disaster-recovery-for-azure-vms-to-a-secondary-azure-region-preview"></a>Olağanüstü durum kurtarma, ikincil bir Azure bölgesine (Önizleme) Azure VM'ler için ayarlama

[Azure Site Recovery](site-recovery-overview.md) hizmeti yönetme ve çoğaltma, yük devretme ve yeniden çalışma şirket içi makineler ve Azure sanal makineleri (VM'ler) yönetme olağanüstü durum kurtarma stratejiniz katkıda bulunur.

Bu öğretici, ikincil bir Azure bölgesine olağanüstü durum kurtarma Azure Vm'leri için nasıl ayarlanacağını gösterir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Kurtarma Hizmetleri kasası oluşturma
> * Hedef kaynak ayarlarını doğrulayın
> * VM'ler için giden erişimini Ayarla
> * Bir sanal makine için çoğaltmayı etkinleştirme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

- Anladığınızdan emin olun [senaryo mimarisi ve bileşenleri](concepts-azure-to-azure-architecture.md).
- Gözden geçirme [destek gereksinimleri](site-recovery-support-matrix-azure-to-azure.md) tüm bileşenler için.

## <a name="create-a-vault"></a>Bir kasa oluşturun

Kaynak bölgesi dışında herhangi bir bölgede kasası oluşturun.

1. [Azure Portal](https://portal.azure.com) > **Kurtarma Hizmetleri**’nde oturum açın.
2. **Yeni** > **İzleme ve Yönetim** > **Backup ve Site Recovery** seçeneğine tıklayın.
3. **Ad** alanında, kasayı tanımlamak için bir kolay ad belirtin. Birden fazla aboneliğiniz varsa, uygun olanı seçin.
4. Bir kaynak grubu oluşturun veya varolan bir tanesini seçin. Bir Azure bölgesi belirtin. Desteklenen bölgeleri kontrol etmek için [Azure Site Recovery Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/) bölümündeki coğrafi kullanılabilirlik kısmına bakın.
5. Kasa panodan hızlı bir şekilde erişmek için tıklatın **panoya Sabitle** ve ardından **oluşturma**.

   ![Yeni kasa](./media/azure-to-azure-tutorial-enable-replication/new-vault-settings.png)

   Yeni kasa eklenen **Pano** altında **tüm kaynakları**ve ana **kurtarma Hizmetleri kasaları** sayfası.

## <a name="verify-target-resources"></a>Hedef kaynakları doğrulayın

1. Azure aboneliğiniz olağanüstü durum kurtarma için kullanılan hedef bölgede VM'ler oluşturmak izin verdiğinden emin olun. Gerekli Kotayı etkinleştirmek için desteğe başvurun.

2. Aboneliğinizi VM'ler kaynağınız VM'ler eşleşen boyutlarıyla desteklemek için yeterli kaynak bulunduğundan emin olun. Site Recovery aynı boyutta veya en yakın olası boyutu hedef VM için seçer.

## <a name="configure-outbound-network-connectivity"></a>Giden ağ bağlantısını yapılandır

Site Recovery'nın beklendiği şekilde çalışması çoğaltmak istediğiniz VM'lerin giden ağ bağlantısı'nda bazı değişiklikler yapmanız gerekir.

- Site kurtarma denetim ağ bağlantısı için bir kimlik doğrulama proxy'si kullanımını desteklemez.
- Bir kimlik doğrulama proxy'si varsa, çoğaltma etkinleştirilemez.

### <a name="outbound-connectivity-for-urls"></a>URL'ler için giden bağlantı

Giden bağlantıyı denetlemek için bir URL tabanlı bir güvenlik duvarı proxy kullanıyorsanız, Site Recovery tarafından kullanılan aşağıdaki URL'lere erişim verin.

| **URL** | **Ayrıntılar** |
| ------- | ----------- |
| *.blob.core.windows.net | Sanal makineden kaynak bölge önbellek depolama hesabına yazılması veri sağlar. |
| Login.microsoftonline.com | Yetkilendirme ve kimlik doğrulaması için Site Recovery hizmeti URL'lerine sağlar. |
| *.hypervrecoverymanager.windowsazure.com | Site Recovery hizmeti ile iletişim kurmak VM sağlar. |
| *. servicebus.windows.net | Site Recovery izleme yazmak için VM ve tanılama verilerini sağlar. |

### <a name="outbound-connectivity-for-ip-address-ranges"></a>IP adres aralıkları için giden bağlantı

Giden bağlantıyı denetlemek için herhangi bir IP tabanlı bir güvenlik duvarı, proxy veya NSG kuralları kullanırken, aşağıdaki IP adres aralıklarını güvenilir listesinde olması gerekir. Aralıkları listesi, aşağıdaki bağlantılardan birini indirin:

  - [Microsoft Azure veri merkezi IP aralıkları](http://www.microsoft.com/en-us/download/details.aspx?id=41653)
  - [Windows Azure veri merkezi IP aralıkları Almanya](http://www.microsoft.com/en-us/download/details.aspx?id=54770)
  - [Windows Çin'de Azure veri merkezi IP aralıkları](http://www.microsoft.com/en-us/download/details.aspx?id=42064)
  - [Office 365 URL’leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity)
  - [Site Recovery Hizmeti uç noktası IP adresleri](https://aka.ms/site-recovery-public-ips)

Ağınızda ağ erişim denetimleri yapılandırmak için bu listeyi kullanın. Bu kullanabilirsiniz [betik](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702) gerekli NSG kuralları oluşturmak için.

## <a name="verify-azure-vm-certificates"></a>Azure VM sertifikaların doğrulama

Tüm son kök sertifikaları Windows veya Linux çoğaltmak istediğiniz Vm'leri üzerinde mevcut olduğundan emin olun. En son kök sertifikaları değilseniz, VM olamaz kayıtlı Site kurtarma, son güvenlik kısıtlamaları.

- Tüm güvenilen kök sertifikalar makinede; böylece Windows VM'ler için VM, en son Windows güncelleştirmeleri yükleyin. Bağlantısı kesilmiş bir ortamda standart Windows Update ve kuruluşunuz için sertifika güncelleştirme işlemlerini izleyin.

- Linux VM'ler için VM son güvenilen kök sertifikaları ve sertifika iptal listesini almak için Linux dağıtımcı tarafından sağlanan yönergeleri izleyin.

## <a name="set-permissions-on-the-account"></a>Hesabın izinleri ayarlama

Azure Site Recovery, Site Recovery yönetim işlemlerini denetlemek için üç yerleşik roller sağlar.

- **Site kurtarma katkıda bulunan** -bu rol bir kurtarma Hizmetleri kasası Azure Site Recovery işlemlerini yönetmek için gerekli tüm izinlere sahiptir. Bu rolüne sahip bir kullanıcı ancak oluşturmak veya bir kurtarma Hizmetleri kasası silemez veya diğer kullanıcıların erişim hakları atayın. Bu rol etkinleştirin ve olağanüstü durum kurtarma uygulamaları ya da tüm kuruluşlar için yönetmek olağanüstü durum kurtarma Yöneticiler için idealdir.

- **Site kurtarma işleci** -yürütme izni ve yük devretme ve yeniden çalışma operations manager bu rolü vardır. Bu rolüne sahip bir kullanıcı etkinleştirmek veya çoğaltma devre dışı bırakmak, oluşturmak veya kasalarını silinemez, yeni altyapı kaydetmek veya diğer kullanıcıların erişim hakları atayın. Bu rolü sanal makine ya da uygulama sahipleri ve BT yöneticileri tarafından istendiğinde uygulamaları devredebildiğini bir olağanüstü durum kurtarma operatörü için uygundur. Olağanüstü Durum Çözümlemesi, DR işleci POST koruyun ve geri dönme sanal makineler.

- **Site kurtarma okuyucu** -bu rol tüm Site Recovery yönetim işlemlerini görüntülemek için gerekli izinlere sahip. Bu rol kimin koruma geçerli durumunu izleyebilir ve Destek biletlerini Yükselt BT izleme yöneticinin için uygundur.

Daha fazla bilgi almak [Azure RBAC yerleşik rolleri](../active-directory/role-based-access-built-in-roles.md)

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

### <a name="select-the-source"></a>Bir kaynak seçin

1. Kurtarma Hizmetleri kasalarının kasa adını tıklatın > **+ Çoğalt**.
2. İçinde **kaynak**seçin **Azure - Önizleme**.
3. İçinde **kaynak konumu**, çalışmakta her yere Vm'lerinizi Azure bölgesinde bir kaynak seçin.
4. Seçin **Azure sanal makine dağıtım modeli** VM'ler için: **Resource Manager** veya **Klasik**.
5. Seçin **kaynak kaynak grubu** Resource Manager VM'ler için veya **bulut hizmeti** Klasik VM'ler için.
6. Ayarları kaydetmek için **Tamam**’a tıklayın.

### <a name="select-the-vms"></a>Sanal makineleri seçin

Site Recovery abonelik ve kaynak grubu/bulut hizmeti ile ilişkili sanal makinelerin listesini alır.

1. İçinde **sanal makineleri**, çoğaltmak istediğiniz sanal makineleri seçin.
2. **Tamam** düğmesine tıklayın.

### <a name="configure-replication-settings"></a>Çoğaltma ayarlarını yapılandırma

Site Recovery, varsayılan ayarları ve hedef bölgesi için çoğaltma ilkesi oluşturur. Gereksinimlerinize göre ayarlarını değiştirebilirsiniz.

1. Tıklatın **ayarları** hedef ayarlarını görüntülemek için.
2. Varsayılan hedefi ayarlarını geçersiz kılmak için tıklatın **Özelleştir**. 

![Ayarları yapılandırma](./media/azure-to-azure-tutorial-enable-replication/settings.png)


- **Hedef konum**: olağanüstü durum kurtarma için kullanılan hedef bölgesi. Hedef konumu Site Recovery kasası konumu eşleştiğini öneririz.

- **Hedef kaynak grubu**: yük devretme sonrasında Azure Vm'lerinin tutan hedef bölgede kaynak grubu. Varsayılan olarak, Site Recovery "asr" soneki ile hedef bölgede yeni bir kaynak grubu oluşturur.

- **Hedef sanal ağ**: sanal makineleri yük devretme sonrasında bulunduğu hedef bölgede ağ.
  Varsayılan olarak, Site Recovery, bir "asr" soneki ile hedef bölgede yeni bir sanal ağ (ve alt ağlar) oluşturur.

- **Önbellek depolama hesapları**: kaynak bölgede Site Recovery kullanan bir depolama hesabı. Kaynak VM'ler değişiklikler çoğaltma hedef konumu için önce bu hesaba gönderilir.

- **Hedef depolama hesapları**: varsayılan olarak, Site Recovery VM depolama hesabı kaynak yansıtmak üzere hedef bölgede yeni bir depolama hesabı oluşturur.

- **Hedef kullanılabilirlik kümeleri**: varsayılan olarak, Site Recovery "asr" soneki ile hedef bölgede yeni bir kullanılabilirlik oluşturur. Vm'leri kaynak bölge kümesinde parçası ise yalnızca kullanılabilirlik kümeleri ekleyebilirsiniz.

- **Çoğaltma İlkesi adı**: İlke adı.

- **Kurtarma noktası bekletme**: varsayılan olarak, Site Recovery kurtarma noktalarına 24 saat tutar. 1 ila 72 saat arasında bir değer yapılandırabilirsiniz.

- **Uygulamayla tutarlı anlık görüntü sıklığı**: varsayılan olarak, Site Recovery 4 saatte bir uygulamayla tutarlı anlık görüntüsünü alır. 1 ve 12 saat arasında herhangi bir değer yapılandırabilirsiniz. Uygulamayla tutarlı anlık görüntü VM içinde uygulama verilerinin zaman içinde nokta anlık görüntüsüdür. Birim Gölge Kopyası Hizmeti (VSS) sağlar, uygulama VM üzerinde anlık görüntü alınırken tutarlı bir durumda olan.

### <a name="track-replication-status"></a>Çoğaltma durumunu izleme

1. İçinde **ayarları**, tıklatın **yenileme** en son durumunu almak için.

2. İlerleme durumunu izleyebilirsiniz **korumayı etkinleştir** iş **ayarları** > **işleri** > **Site Recovery işleri**.

3. İçinde **ayarları** > **çoğaltılan öğeler**, VM'ler ve ilk çoğaltma ilerleme durumunu görüntüleyebilirsiniz. VM ayarlarına detaya gitmek için tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir Azure VM için olağanüstü durum kurtarma yapılandırılmış. Yapılandırmanızı test sonraki adımdır.

> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma tatbikatı çalıştırma](azure-to-azure-tutorial-dr-drill.md)
