---
title: "Azure Site Recovery ile kullanmak için kaynakları oluşturun | Microsoft Docs"
description: "Azure Azure Site Recovery hizmetini kullanarak şirket içi makineleri çoğaltma için hazırlanamadı öğrenin."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 12/31/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 71d740107eb2082e3f112941e1d4abd715d25807
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="prepare-azure-resources-for-replication-of-on-premises-machines"></a>Şirket içi makineleri çoğaltma için Azure kaynaklarını hazırlama

[Azure Site Recovery](site-recovery-overview.md) hizmeti tarafından iş uygulamalarınızı çalışır halde tutmaktan planlanan ve planlanmayan kesintiler sırasında kullanılabilir iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkı. Site Recovery yönetir ve şirket içi makineler ve Azure sanal makineleri (VM'ler), çoğaltma, yük devretme ve kurtarma gibi olağanüstü durum kurtarma düzenler.

Bu öğretici, şirket içi sanal makineleri (Hyper-V veya VMware) veya Windows/Linux fiziksel sunucuları Azure'a çoğaltmak istediğiniz zaman Azure bileşenleri hazırlamak nasıl gösterir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Hesabınızın çoğaltma izinleri olduğundan emin olun
> * Bir Azure Storage hesabı oluşturun.
> * Bir Azure ağı ayarlayın. Azure Vm'lerinin yük devretme sonrasında oluşturulduğunda Azure bu ağa alanına.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

http://portal.azure.com sayfasından Azure portalda oturum açın.

## <a name="verify-account-permissions"></a>Hesap izinlerini doğrulayın

Ücretsiz Azure hesabınız az önce oluşturduğunuz, aboneliğinizi yönetici demektir. Abonelik Yöneticisi değilse, gereksinim duyduğunuz izinleri atamak için yöneticinizle birlikte çalışarak. Yeni bir sanal makine için çoğaltmayı etkinleştirmek için şunlara sahip olmalısınız:

- Seçilen kaynak grubunda bir VM oluşturma izni
- Seçilen sanal ağ içinde bir VM oluşturma izni
- Seçilen depolama hesabına yazma izni

'Sanal makine Katılımcısı' yerleşik rolü bu izinlere sahiptir. Azure Site Recovery işlemlerini yönetme izni de gerekir. 'Site kurtarma katılımcı' rolü bir kurtarma Hizmetleri kasası Site Recovery işlemlerini yönetmek için gerekli tüm izinlere sahiptir.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Azure depolama alanında çoğaltılan makinelerin görüntüleri tutulur. Azure VM'ler, şirket içinden Azure'a yük depolama biriminden oluşturulur.

1. İçinde [Azure portal](https://portal.azure.com) menüsünde tıklatın **yeni** -> **depolama** -> **depolama hesabı**.
2. Depolama hesabınız için bir ad girin. Bu öğreticileri için ad kullanacağız **contosovmsacct1910171607**. Adı Azure içinde benzersiz olması ve numaraları ve yalnızca küçük harfler 3 ile 24 karakter arasında olması gerekir.
3. Kullanım **Resource Manager** dağıtım modeli.
4. Seçin **genel amaçlı** > **standart**.
5. Varsayılan seçmek **RA-GRS** depolama artıklığı.
6. Yeni depolama hesabını oluşturmak istediğiniz aboneliği seçin.
7. Yeni bir kaynak grubu belirtin. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bu öğreticileri için kullanırız adı **ContosoRG**.
8. Depolama hesabınız için coğrafi konumu seçin. Depolama hesabı kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir. Bu öğreticileri için kullandığımız **Batı Avrupa** bölge.

   ![oluşturma storageacct](media/tutorial-prepare-azure/create-storageacct.png)

9. Depolama hesabını oluşturmak için **Oluştur**’a tıklayın.

## <a name="create-a-vault"></a>Bir kasa oluşturun

1. Azure portal menüsünde tıklatın **yeni** > **izleme ve Yönetim** >
    **yedekleme ve Site kurtarma**.
2. **Ad** alanında, kasayı tanımlamak için bir kolay ad belirtin. Bu öğretici için kullandığımız **ContosoVMVault**.
3. Adlı varolan bir kaynak grubu seçin **contosoRG**.
4. Azure bölgesini belirtin **Batı Avrupa**, biz bu öğreticileri kümesinde kullandığınız.
5. Kasa panodan hızlı bir şekilde erişmek için tıklatın **panoya Sabitle** > **oluşturma**.

   ![Yeni kasa](./media/tutorial-prepare-azure/new-vault-settings.png)

   Yeni kasa görünür **Pano** > **tüm kaynakları**ve ana **kurtarma Hizmetleri kasaları** sayfası.

## <a name="set-up-an-azure-network"></a>Azure ağı ayarlama

Yük devretme sonrasında oluşturulan Azure Vm'lerinin depolama biriminden, bu ağa katılmış.

1. İçinde [Azure portal](https://portal.azure.com) menüsünde tıklatın **yeni** > **ağ** >
    **sanal ağ**
2. Bırakın **Resource Manager** dağıtım modeli olarak seçildi. Kaynağın, tercih edilen dağıtım modeline yöneticisidir.
   - Bir ağ adı belirtin. Ad Azure kaynak grubun içinde benzersiz olmalıdır. Adı kullanacağız **ContosoASRnet**
   - Varolan bir kaynak grubunu kullanın **contosoRG**.
   - Ağ 10.0.0.0/24 adres aralığını belirtin.
   - Bu öğretici için bir alt ağ gerekli değil.
   - Ağ oluşturulacağı aboneliği seçin.
   - Konumu seçin **Batı Avrupa**. Ağın, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.
3. **Oluştur**’a tıklayın.

   ![ağ oluştur](media/tutorial-prepare-azure/create-network.png)

   Sanal ağ oluşturmak için birkaç saniye sürer. Oluşturulduktan sonra Azure portal panosunda konusuna bakın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma Azure için şirket içi VMware altyapıyı hazırlama](tutorial-prepare-on-premises-vmware.md)
