---
title: "Azure Site Recovery ile kullanılacak kaynaklar oluşturma | Microsoft Docs"
description: "Azure Site Recovery hizmetini kullanarak şirket içi makinelerin çoğaltması için Azure’un nasıl hazırlanacağını öğrenin."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/16/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: d1aadd6b44d64f0bdb35ea02d628bedfc366ad3c
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="prepare-azure-resources-for-replication-of-on-premises-machines"></a>Şirket içi makinelerin çoğaltması için Azure kaynaklarını hazırlama

[Azure Site Recovery](site-recovery-overview.md) hizmeti, planlı ve plansız kesintiler sırasında iş uygulamalarınızı çalışır durumda tutarak, iş sürekliliğinize ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur. Site Recovery, şirket içi makinelerin ve Azure sanal makinelerinin çoğaltma, yük devretme ve kurtarma gibi olağanüstü durum kurtarma işlemlerini yönetir ve düzenler.

Bu öğretici, şirket içi sanal makineleri (Hyper-V veya VMware) veya Windows/Linux fiziksel sunucuları Azure'a çoğaltmak istediğinizde Azure bileşenlerini nasıl hazırlayacağını gösterir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Hesabınızın çoğaltma izinlerine sahip olduğunu doğrulama
> * Bir Azure Storage hesabı oluşturun.
> * Bir Azure ağını ayarlama. Yük devretmeden sonra Azure sanal makineleri oluşturulduğunda sanal makineler bu Azure ağına katılır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

http://portal.azure.com sayfasından Azure portalda oturum açın.

## <a name="verify-account-permissions"></a>Hesap izinlerini doğrulama

Ücretsiz Azure hesabınızı oluşturduysanız aboneliğinizin yöneticisi siz olursunuz. Abonelik yöneticisi değilseniz, ihtiyaç duyduğunuz izinleri atamak için yöneticiyle birlikte çalışın. Yeni bir sanal makineye yönelik çoğaltmayı etkinleştirmek için şunlara sahip olmalısınız:

- Seçilen kaynak grubunda sanal makine oluşturma izni
- Seçilen sanal ağda sanal makine oluşturma izni
- Seçilen depolama hesabına yazma izni

'Sanal Makine Katılımcısı' yerleşik rolü bu izinlere sahiptir. Azure Site Recovery işlemlerini yönetme izni de gerekir. 'Site Recovery Katkıda Bulunanı' rolü, Kurtarma Hizmetleri kasasındaki Site Recovery işlemlerini yönetmek için gereken tüm izinlere sahiptir.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Çoğaltılan makinelerin görüntüleri Azure depolama alanında tutulur. Şirket içinden Azure’a yük devretme gerçekleştirdiğinizde depolama alanından Azure sanal makineleri oluşturulur.

1. [Azure portal](https://portal.azure.com) menüsünde **Yeni** -> **Depolama** -> **Depolama hesabı**’na tıklayın.
2. **Depolama hesabı oluştur** bölümüne hesap için bir ad girin. Bu öğreticiler için **contosovmsacct1910171607** adını kullanacağız. Ad, Azure içinde benzersiz olmalı, 3 ila 24 karakter uzunluğunda olmalı ve yalnızca rakamlar ve küçük harfler içermelidir.
3. **Resource Manager** dağıtım modelini kullanın.
4. **Genel amaçlı** > **Standart** seçeneklerini belirleyin. Blob depolamayı seçmeyin.
5. Depolama yedekliliği için varsayılan **RA-GRS** seçeneğini belirleyin.
6. Yeni depolama hesabını oluşturmak istediğiniz aboneliği seçin.
7. Yeni bir kaynak grubu belirtin. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bu öğreticiler için **ContosoRG** adını kullanırız.
8. Depolama hesabınız için coğrafi konumu seçin. Depolama hesabının, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir. Bu öğreticiler için **Batı Avrupa** bölgesini kullanırız.

   ![create-storageacct](media/tutorial-prepare-azure/create-storageacct.png)

9. Depolama hesabını oluşturmak için **Oluştur**’a tıklayın.

## <a name="create-a-vault"></a>Kasa oluşturma

1. Azure portalında **Kaynak oluştur** > **İzleme ve Yönetim** > **Backup and Site Recovery** seçeneklerine tıklayın.
2. **Ad** alanında, kasayı tanımlamak için bir kolay ad belirtin. Bu öğretici için **ContosoVMVault** adını kullanırız.
3. **contosoRG** adlı mevcut kaynak grubunu seçin.
4. Bu öğretici kümesinde kullandığımız **Batı Avrupa** Azure bölgesini belirtin.
5. Panodan kasaya hızlıca erişmek için **Panoya sabitle** > **Oluştur** seçeneğine tıklayın.

   ![Yeni kasa](./media/tutorial-prepare-azure/new-vault-settings.png)

   Yeni kasa, **Pano** > **Tüm kaynaklar** kısmında ve ana **Kurtarma Hizmetleri kasaları** sayfasında görüntülenir.

## <a name="set-up-an-azure-network"></a>Azure ağı ayarlama

Yük devretmeden sonra depolamadan Azure sanal makineleri oluşturulduğunda sanal makineler bu ağa katılır.

1. [Azure portalında](https://portal.azure.com) **Kaynak oluştur** > **Ağ** > **Sanal ağ** seçeneklerine tıklayın.
2. Dağıtım modeli olarak **Resource Manager**’ı seçili bırakın. Tercih edilen dağıtım modeli Resource Manager'dır.
   - Bir ağ adı belirtin. Ad, Azure kaynak grubu içinde benzersiz olmalıdır. **ContosoASRnet** adını kullanacağız
   - **contosoRG** mevcut kaynak grubunu kullanın.
   - Ağ adres aralığını 10.0.0.0/24 olarak belirtin.
   - Bu öğretici için bir alt ağ gerekmez.
   - Ağın oluşturulacağı aboneliği seçin.
   - **Batı Avrupa** konumunu seçin. Ağın, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.
3. **Oluştur**’a tıklayın.

   ![create-network](media/tutorial-prepare-azure/create-network.png)

   Sanal ağın oluşturulması birkaç saniye sürer. Oluşturulduktan sonra, Azure portalı panosunda görünür.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure’da olağanüstü durum kurtarma için şirket içi VMware altyapısını hazırlama](tutorial-prepare-on-premises-vmware.md)
