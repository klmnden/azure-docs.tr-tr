---
title: Azure Site Recovery ile şirket içi makineler için Azure'da olağanüstü durum kurtarma hazırlığı yapma | Microsoft Docs
description: Azure Site Recovery ile şirket içi makinelerin olağanüstü durum kurtarma işlemleri için Azure’ın nasıl hazırlanacağını öğrenin.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 04/15/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: d2de871176917dcc24d910b3742bdb2700c4f25d
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64691764"
---
# <a name="prepare-azure-resources-for-disaster-recovery-of-on-premises-machines"></a>Şirket içi makinelerin olağanüstü durum kurtarma işlemleri için Azure kaynaklarını hazırlama

Bu makalede şirket içi VMware Vm'leri, Hyper-V Vm'leri veya Windows/Linux fiziksel sunucularını azure'a olağanüstü durum kurtarma ayarlayabilirsiniz böylece, Azure kaynakları ve bileşenleri hazırlama kullanarak [Azure Site Recovery](site-recovery-overview.md) hizmeti.

Bu makale, şirket içi sanal makineler için olağanüstü durum kurtarmanın nasıl ayarlanacağını gösteren serideki ilk öğreticidir. 


Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure hesabı, çoğaltma izinlerine sahip olduğunu doğrulayın.
> * Kurtarma Hizmetleri kasası oluşturun. Kasada VM'lerin meta veri ve yapılandırma bilgileri ile diğer çoğaltma bileşenleri tutulur.
> * Azure sanal ağı (VNet) ayarlama ayarlayın. Yük devretme sonrasında Azure Vm'leri oluşturulduğunda bu ağa katılır.

> [!NOTE]
> Öğreticiler bir senaryo için en basit dağıtım yolu gösterir. Mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı yönergeler için Site Recovery İçindekiler bölümünde nasıl yapılır makalesine gözden geçirin.

## <a name="before-you-start"></a>Başlamadan önce

- Mimarisini İnceleme [VMware](vmware-azure-architecture.md), [Hyper-V](hyper-v-azure-architecture.md), ve [fiziksel sunucu](physical-azure-architecture.md) olağanüstü durum kurtarma.
- Sık sorulan sorular için okuma [VMware](vmware-azure-common-questions.md) ve [Hyper-V](hyper-v-azure-common-questions.md)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun. İçin oturum açın [Azure portalında](https://portal.azure.com).


## <a name="verify-account-permissions"></a>Hesap izinlerini doğrulama

Yalnızca ücretsiz Azure hesabınızı oluşturduysanız aboneliğinizin Yöneticisi olduğunuz ve ihtiyaç duyduğunuz izinleri vardır. Abonelik yöneticisi değilseniz, ihtiyaç duyduğunuz izinleri atamak için yöneticiyle birlikte çalışın. Yeni bir sanal makineye yönelik çoğaltmayı etkinleştirmek için şunları yapma iznine sahip olmalısınız:

- Seçilen kaynak grubunda sanal makine oluşturma.
- Seçilen sanal ağda sanal makine oluşturma.
- Bir Azure depolama hesabına yazma.
- Yazma için bir Azure yönetilen disk.

Bu görevleri tamamlamak için hesabınıza Sanal Makine Katkıda Bulunan yerleşik rolünün atanması gerekir. Ayrıca Site Recovery işlemlerini bir kasada yönetmek için hesabınıza Site Recovery Katkıda Bulunan yerleşik rolünün atanması gerekir.


## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

1. Azure portalında **+ kaynak Oluştur**ve markette Ara **kurtarma**.
2. Tıklayın **yedekleme ve Site Recovery (OMS)**, Backup ve Site Recovery sayfasında tıklayın **Oluştur**. 
1. İçinde **kurtarma Hizmetleri kasası** > **adı**, kasayı tanımlamak için bir kolay ad girin. Bu öğretici dizisi için **ContosoVMVault**’u kullanacağız.
2. İçinde **kaynak grubu**, mevcut bir kaynak grubunu seçin veya yeni bir tane oluşturun. Bu öğretici için kullandığımız **contosoRG**.
3. İçinde **konumu**, hangi kasa olmalıdır bölgeyi seçin. **Batı Avrupa** kullanacağız.
4. Panodan kasaya hızlı şekilde erişmek için **Panoya sabitle** > **Oluştur**’u seçin.

   ![Yeni kasa oluştur](./media/tutorial-prepare-azure/new-vault-settings.png)

   Yeni kasa, **Pano** > **Tüm kaynaklar** bölümünde ve ana **Kurtarma Hizmetleri kasaları** sayfasında görüntülenir.

## <a name="set-up-an-azure-network"></a>Azure ağı ayarlama

Şirket içinde makineleri Azure'a çoğaltılan yönetilen diskler. Yük devretme gerçekleştiğinde Azure Vm'leri yönetilen bu disklerden oluşturulan ve bu yordamda, belirttiğiniz Azure ağ alanına katıldı.

1. [Azure portalında](https://portal.azure.com) **Kaynak oluştur** > **Ağ** > **Sanal ağ** seçeneklerini belirleyin.
2. Tutun **Resource Manager** dağıtım modeli olarak seçilmiş.
3. **Ad** bölümünde bir ağ adı girin. Ad, Azure kaynak grubu içinde benzersiz olmalıdır. Bu öğreticide **ContosoASRnet** kullanıyoruz.
4. İçinde ağın oluşturulacağı kaynak grubunu belirtin. Biz mevcut **contosoRG** kaynak grubunu kullanıyoruz.
5. İçinde **adres aralığı**, ağ aralığı girin. Kullandığımız **10.1.0.0/24**ve bir alt ağı kullanmıyor.
6. **Abonelik** bölümünde ağın oluşturulacağı aboneliği seçin.
7. İçinde **konumu**, Kurtarma Hizmetleri kasası oluşturulduğu grubundakiyle aynı bölgeyi seçin. Müşterilerimize öğreticide sahip **Batı Avrupa**. Ağ, kasa ile aynı bölgede olması gerekir.
8. Ağda hizmet uç noktası olmadan temel DDoS korumasının varsayılan seçeneklerini bırakıyoruz.
9. **Oluştur**’a tıklayın.

   ![Sanal ağ oluşturma](media/tutorial-prepare-azure/create-network.png)

Sanal ağın oluşturulması birkaç saniye sürer. Oluşturulduktan sonra, Azure portalı panosunda görünür.




## <a name="next-steps"></a>Sonraki adımlar

- VMware olağanüstü durum kurtarma için [şirket içi VMware altyapısını hazırlama](tutorial-prepare-on-premises-vmware.md).
- Hyper-V olağanüstü durum kurtarma için [şirket içi Hyper-V sunucuları hazırlama](hyper-v-prepare-on-premises-tutorial.md).
- Fiziksel sunucu olağanüstü durum kurtarma için [yapılandırma sunucusu ve kaynak ortamını ayarlama](physical-azure-disaster-recovery.md)
- Azure ağları [hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview) yönetilen diskler.
