---
title: "Azure Site Recovery ile kullanılacak kaynaklar oluşturma | Microsoft Docs"
description: "Azure Site Recovery kullanarak şirket içi makinelerin çoğaltması için Azure’ın nasıl hazırlanacağını öğrenin."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/16/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 2f6ff1d30eef1fe34e55457d9bdd4295804ec16a
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
---
# <a name="prepare-azure-resources-for-replication-of-on-premises-machines"></a>Şirket içi makinelerin çoğaltması için Azure kaynaklarını hazırlama

 [Azure Site Recovery](site-recovery-overview.md), planlı ve plansız kesintiler sırasında iş uygulamalarınızı çalışır durumda tutarak, iş sürekliliğinize ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur. Site Recovery, şirket içi makinelerin ve Azure sanal makinelerinin çoğaltma, yük devretme ve kurtarma gibi olağanüstü durum kurtarma işlemlerini yönetir ve düzenler.

Bu öğretici, şirket içi sanal makineleri (Hyper-V veya VMware) veya Windows/Linux fiziksel sunucuları Azure'a çoğaltmak istediğinizde Azure bileşenlerini nasıl hazırlayacağını gösterir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Hesabınızın çoğaltma izinlerine sahip olduğunu doğrulayın.
> * Bir Azure Storage hesabı oluşturun.
> * Bir Azure ağını ayarlama. Yük devretmeden sonra Azure sanal makineleri oluşturulduğunda sanal makineler bu Azure ağına katılır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](http://portal.azure.com)’da oturum açın.

## <a name="verify-account-permissions"></a>Hesap izinlerini doğrulama

Ücretsiz Azure hesabınızı oluşturduysanız aboneliğinizin yöneticisi siz olursunuz. Abonelik yöneticisi değilseniz, ihtiyaç duyduğunuz izinleri atamak için yöneticiyle birlikte çalışın. Yeni bir sanal makineye yönelik çoğaltmayı etkinleştirmek için şunları yapma iznine sahip olmalısınız:

- Seçilen kaynak grubunda sanal makine oluşturma.
- Seçilen sanal ağda sanal makine oluşturma.
- Seçilen depolama hesabına yazma.

Sanal Makine Katılımcısı yerleşik rolü bu izinlere sahiptir. Site Recovery işlemlerini yönetme izni de gerekir. Site Recovery Katkıda Bulunanı rolü, Kurtarma Hizmetleri kasasındaki Site Recovery işlemlerini yönetmek için gereken tüm izinlere sahiptir.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Çoğaltılan makinelerin görüntüleri Azure depolama alanında tutulur. Şirket içinden Azure’a yük devretme gerçekleştirdiğinizde depolama alanından Azure sanal makineleri oluşturulur.

1. [Azure portal](https://portal.azure.com)’da, **Yeni** > **Depolama** > **Depolama hesabı**’nı seçin.
2. **Depolama hesabı oluştur** bölümüne hesap için bir ad girin. Bu öğreticiler için **contosovmsacct1910171607** adını kullanın. Ad, Azure içinde benzersiz olmalı, 3 ila 24 karakter uzunluğunda olmalı ve yalnızca rakamlar ve küçük harfler içermelidir.
3. **Dağıtım modeli** bölümünde **Kaynak Yöneticisi**’ni seçin.
4. **Hesap türü** bölümünde **Genel amaçlı**’yı seçin. **Performans** bölümünde **Standart**’ı seçin. Blob depolamayı seçmeyin.
5. **Çoğaltma** bölümünde depolama yedeklemesi için **Okuma Erişimli Coğrafi Olarak Yedekli depolama**’yı seçin.
6. **Abonelik** bölümünde yeni depolama hesabını oluşturmak istediğiniz aboneliği seçin.
7. **Kaynak grubu** bölümünde yeni bir kaynak grubu adı girin. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bu öğreticiler için **ContosoRG** adını kullanın.
8. **Konum** bölümünde depolama hesabınız için coğrafi konumu seçin. Depolama hesabının, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir. Bu öğreticiler için **Batı Avrupa** bölgesini kullanın.

   ![Depolama hesabı oluşturma](media/tutorial-prepare-azure/create-storageacct.png)

9. Depolama hesabını oluşturmak için **Oluştur**’u seçin.

## <a name="create-a-vault"></a>Kasa oluşturma

1. Azure portalında **Kaynak oluştur** > **İzleme ve Yönetim** > **Backup and Site Recovery** seçeneklerini belirleyin.
2. **Ad** alanına kasayı tanımlamak için kolay bir ad girin. Bu öğretici için **ContosoVMVault** adını kullanın.
3. **Kaynak grubu** bölümünde **contosoRG** adlı mevcut kaynak grubunu seçin.
4. **Konum** bölümünde bu öğretici kümesinde kullanılan **Batı Avrupa** Azure bölgesini belirtin.
5. Panodan kasaya hızlı şekilde erişmek için **Panoya sabitle** > **Oluştur**’u seçin.

   ![Yeni kasa oluştur](./media/tutorial-prepare-azure/new-vault-settings.png)

   Yeni kasa, **Pano** > **Tüm kaynaklar** bölümünde ve ana **Kurtarma Hizmetleri kasaları** sayfasında görüntülenir.

## <a name="set-up-an-azure-network"></a>Azure ağı ayarlama

Yük devretmeden sonra depolamadan Azure sanal makineleri oluşturulduğunda sanal makineler bu ağa katılır.

1. [Azure portalında](https://portal.azure.com) **Kaynak oluştur** > **Ağ** > **Sanal ağ** seçeneklerini belirleyin.
2. Dağıtım modeli olarak **Resource Manager**’ı seçili bırakın. Tercih edilen dağıtım modeli Resource Manager'dır. Sonra aşağıdaki adımları uygulayın:

   a. **Ad** bölümünde bir ağ adı girin. Ad, Azure kaynak grubu içinde benzersiz olmalıdır. **ContosoASRnet** adını kullanın.

   b. **Kaynak grubu** bölümünde **contosoRG** adlı mevcut kaynak grubunu kullanın.

   c. **Adres aralığı** bölümünde adres aralığını **10.0.0.0/24** olarak girin.

   d. Bu öğretici için bir alt ağ gerekmez.

   e. **Abonelik** bölümünde ağın oluşturulacağı aboneliği seçin.

   f. **Konum** bölümünde **Batı Avrupa**’yı seçin. Ağın, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.

3. **Oluştur**’u seçin.

   ![Sanal ağ oluşturma](media/tutorial-prepare-azure/create-network.png)

   Sanal ağın oluşturulması birkaç saniye sürer. Oluşturulduktan sonra, Azure portalı panosunda görünür.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure’da olağanüstü durum kurtarma için şirket içi VMware altyapısını hazırlama](tutorial-prepare-on-premises-vmware.md)
