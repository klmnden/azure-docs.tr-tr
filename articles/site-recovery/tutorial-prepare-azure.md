---
title: Azure Site Recovery ile şirket içi makineler için Azure'da olağanüstü durum kurtarma hazırlığı yapma | Microsoft Docs
description: Azure Site Recovery ile şirket içi makinelerin olağanüstü durum kurtarma işlemleri için Azure’ın nasıl hazırlanacağını öğrenin.
services: site-recovery
author: mayurigupta13
ms.service: site-recovery
ms.topic: tutorial
ms.date: 03/03/2019
ms.author: mayg
ms.custom: MVC
ms.openlocfilehash: dd84becdf7043f3ae1c8070bdc1918d377bc3e3b
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57337881"
---
# <a name="prepare-azure-resources-for-disaster-recovery-of-on-premises-machines"></a>Şirket içi makinelerin olağanüstü durum kurtarma işlemleri için Azure kaynaklarını hazırlama

 [Azure Site Recovery](site-recovery-overview.md), planlı ve plansız kesintiler sırasında iş uygulamalarınızı çalışır durumda tutarak, iş sürekliliğinize ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur. Site Recovery, şirket içi makinelerin ve Azure sanal makinelerinin çoğaltma, yük devretme ve kurtarma gibi olağanüstü durum kurtarma işlemlerini yönetir ve düzenler.

Bu makale, şirket içi sanal makineler için olağanüstü durum kurtarmanın nasıl ayarlanacağını gösteren serideki ilk öğreticidir. Hem şirket içi VMware VM'lerini, Hyper-V VM'lerini hem de fiziksel sunucuları koruyanlar için uygundur.

> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu size göstermek için tasarlanmıştır. Mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı yönergeler için ilgili senaryonun **Nasıl Yapılır** bölümüne başvurun.

Bu makale, şirket içi sanal makineleri (Hyper-V veya VMware) veya Windows/Linux fiziksel sunucularını Azure'a çoğaltmak istediğinizde Azure bileşenlerini nasıl hazırlayacağınızı gösterir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure hesabınızın çoğaltma izinlerine sahip olduğunu doğrulayın.
> * Kurtarma Hizmetleri kasası oluşturun. Kasada VM'lerin meta veri ve yapılandırma bilgileri ile diğer çoğaltma bileşenleri tutulur.
> * Bir Azure ağı ayarlayın. Yük devretmeden sonra Azure sanal makineleri oluşturulduğunda sanal makineler bu Azure ağına katılır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](http://portal.azure.com) oturum açın.

## <a name="verify-account-permissions"></a>Hesap izinlerini doğrulama

Ücretsiz Azure hesabınızı oluşturduysanız aboneliğinizin yöneticisi siz olursunuz. Abonelik yöneticisi değilseniz, ihtiyaç duyduğunuz izinleri atamak için yöneticiyle birlikte çalışın. Yeni bir sanal makineye yönelik çoğaltmayı etkinleştirmek için şunları yapma iznine sahip olmalısınız:

- Seçilen kaynak grubunda sanal makine oluşturma.
- Seçilen sanal ağda sanal makine oluşturma.
- Depolama hesabına yazma.
- Yönetilen diske yazma.

Bu görevleri tamamlamak için hesabınıza Sanal Makine Katkıda Bulunan yerleşik rolünün atanması gerekir. Ayrıca Site Recovery işlemlerini bir kasada yönetmek için hesabınıza Site Recovery Katkıda Bulunan yerleşik rolünün atanması gerekir.


## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

1. Azure portalında **+ kaynak Oluştur**ve markette Ara **kurtarma Hizmetleri**.
2. Tıklayın **yedekleme ve Site Recovery (OMS)**, Backup ve Site Recovery sayfasında tıklayın **Oluştur**. 
1. İçinde **kurtarma Hizmetleri kasası** > **adı**, kasayı tanımlamak için bir kolay ad girin. Bu öğretici dizisi için **ContosoVMVault**’u kullanacağız.
2. İçinde **kaynak grubu**, mevcut bir kaynak grubunu seçin veya yeni bir tane oluşturun. Bu öğretici için kullandığımız **contosoRG**.
3. İçinde **konumu**, hangi kasa olmalıdır bölgeyi seçin. **Batı Avrupa** kullanacağız.
4. Panodan kasaya hızlı şekilde erişmek için **Panoya sabitle** > **Oluştur**’u seçin.

   ![Yeni kasa oluştur](./media/tutorial-prepare-azure/new-vault-settings.png)

   Yeni kasa, **Pano** > **Tüm kaynaklar** bölümünde ve ana **Kurtarma Hizmetleri kasaları** sayfasında görüntülenir.

## <a name="set-up-an-azure-network"></a>Azure ağı ayarlama

Yük devretme sonrasında oluşturulan Azure Vm'lerinin yönetilen diskleri, bu ağa katılır.

1. [Azure portalında](https://portal.azure.com) **Kaynak oluştur** > **Ağ** > **Sanal ağ** seçeneklerini belirleyin.
2. Dağıtım modeli olarak **Resource Manager**’ı seçili bırakıyoruz.
3. **Ad** bölümünde bir ağ adı girin. Ad, Azure kaynak grubu içinde benzersiz olmalıdır. Bu öğreticide **ContosoASRnet** kullanıyoruz.
4. İçinde ağın oluşturulacağı kaynak grubunu belirtin. Biz mevcut **contosoRG** kaynak grubunu kullanıyoruz.
5. **Adres aralığı** bölümünde ağ için **10.0.0.0/24** aralığını girin. Biz bu ağda alt ağ kullanmıyoruz.
6. **Abonelik** bölümünde ağın oluşturulacağı aboneliği seçin.
7. **Konum** bölümünde **Batı Avrupa**’yı seçin. Ağın, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.
8. Ağda hizmet uç noktası olmadan temel DDoS korumasının varsayılan seçeneklerini bırakıyoruz.
9. **Oluştur**’a tıklayın.

   ![Sanal ağ oluşturma](media/tutorial-prepare-azure/create-network.png)

   Sanal ağın oluşturulması birkaç saniye sürer. Oluşturulduktan sonra, Azure portalı panosunda görünür.

## <a name="useful-links"></a>Yararlı bağlantılar

- Azure ağları [hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview) yönetilen diskler.



## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure’da olağanüstü durum kurtarma için şirket içi VMware altyapısını hazırlama](tutorial-prepare-on-premises-vmware.md)
