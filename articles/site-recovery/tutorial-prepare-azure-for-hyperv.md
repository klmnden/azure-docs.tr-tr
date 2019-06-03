---
title: Şirket içi makinelerin olağanüstü durum kurtarma işlemleri için Azure kaynaklarını hazırlama
description: Azure, Azure Site Recovery kullanarak şirket içi Hyper-V vm'leri için olağanüstü durum kurtarma hazırlamayı öğrenin
author: rayne-wiselman
ms.service: site-recovery
services: site-recovery
ms.topic: tutorial
ms.date: 05/30/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 07a5ee6ccdaecc78c9a8e61ae9e64a5264e3a875
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66418347"
---
# <a name="prepare-azure-resources-for-disaster-recovery-of-on-premises-machines"></a>Şirket içi makinelerin olağanüstü durum kurtarma işlemleri için Azure kaynaklarını hazırlama

 [Azure Site Recovery](site-recovery-overview.md) planlı ve plansız kesintiler sırasında çalışan iş uygulamaları tutarak iş sürekliliği ve olağanüstü durum kurtarma (BCDR) yardımcı olur. Site Recovery, şirket içi makinelerin ve Azure sanal makinelerinin çoğaltma, yük devretme ve kurtarma gibi olağanüstü durum kurtarma işlemlerini yönetir ve düzenler.

Bu öğretici, şirket içi Hyper-V Vm'leri için olağanüstü durum kurtarma ayarlama işlemi açıklanmaktadır bir serinin ilk bölümüdür.

> [!NOTE]
> Biz öğreticiler bir senaryo için en basit dağıtım yolu gösterecek şekilde tasarlayın. Bu öğreticiler, mümkün olduğunda varsayılan seçenekleri kullanın ve tüm olası ayarları ve yol gösterme. Daha fazla bilgi için karşılık gelen her senaryo için "Nasıl yapılır" bölümüne bakın.

Bu öğretici, şirket içi sanal makineleri (Hyper-V) Azure'a çoğaltmak istediğinizde Azure bileşenlerini nasıl hazırlayacağını gösterir. Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Azure hesabınızın çoğaltma izinlerine sahip olduğunu doğrulayın.
> * Çoğaltılan makinelerin görüntüleri depolayan bir Azure depolama hesabı oluşturun.
> * VM'ler ve diğer çoğaltma bileşenleri için meta verileri ve yapılandırma bilgilerini depolayan bir kurtarma Hizmetleri kasası oluşturun.
> * Bir Azure ağı ayarlayın. Yük devretme sonrasında Azure Vm'leri oluşturulduğunda bu ağa katılır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.

## <a name="sign-in"></a>Oturum aç

[Azure Portal](https://portal.azure.com) oturum açın.

## <a name="verify-account-permissions"></a>Hesap izinlerini doğrulama

Ücretsiz Azure hesabı oluşturduysanız, bu abonelik için yönetici demektir. Yönetici değilseniz, ihtiyaç duyduğunuz izinleri atamak için yöneticiyle birlikte çalışın. Yeni bir sanal makineye yönelik çoğaltmayı etkinleştirmek için şunları yapma iznine sahip olmalısınız:

- Seçilen kaynak grubunda sanal makine oluşturma.
- Seçilen sanal ağda sanal makine oluşturma.
- Seçilen depolama hesabına yazma.

Bu görevleri tamamlamak için hesabınıza sanal makine Katılımcısı yerleşik rolü atanması gerekir. Site Recovery işlemlerini bir kasada yönetmek için hesabınıza Site Recovery katkıda bulunan yerleşik rolünün atanması gerekir.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Çoğaltılan makinelerin görüntüleri Azure depolama alanında tutulur. Şirket içinden Azure’a yük devretme gerçekleştirdiğinizde depolama alanından Azure sanal makineleri oluşturulur. Depolama hesabının, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.

1. İçinde [Azure portalında](https://portal.azure.com) menüsünde **kaynak Oluştur** > **depolama** > **depolama hesabı - blob, dosya, tablo, kuyruk** .
2. **Depolama hesabı oluştur** bölümüne hesap için bir ad girin.  Seçtiğiniz adın Azure içinde benzersiz olmalıdır, 3 ila 24 karakter uzunluğunda ve yalnızca kullanılması küçük harfler ve sayılar. Bu öğretici için kullanmak **contosovmsacct1910171607**.
3. **Dağıtım modeli** bölümünde **Kaynak Yöneticisi**’ni seçin.
4. İçinde **hesap türü**seçin **depolama (genel amaçlı v1)** . Blob depolamayı seçmeyin.
5. **Çoğaltma** bölümünde depolama yedeklemesi için **Okuma Erişimli Coğrafi Olarak Yedekli depolama**’yı seçin. Devre dışı olarak ayarlanması gereken güvenli aktarım bırakın.
6. **Performans** bölümünde **Standart**’ı seçin. Ardından **erişim katmanı**, varsayılan seçeneği işaretleyin **etkin**.
7. İçinde **abonelik**, yeni depolama hesabını oluşturmak istediğiniz aboneliği seçin.
8. **Kaynak grubu** bölümünde yeni bir kaynak grubu adı girin. Bir Azure kaynak grubu, Azure kaynaklarını dağıtıldığı ve yönetildiği mantıksal bir kapsayıcıdır. Bu öğretici için kullanmak **ContosoRG**.
9. İçinde **konumu**, depolama hesabınız için coğrafi konumu seçin. Bu öğretici için kullanmak **Batı Avrupa**.
10. Depolama hesabını oluşturmak için **Oluştur**’u seçin.

   ![Depolama hesabı oluşturma](media/tutorial-prepare-azure/create-storageacct.png)

## <a name="create-a-recovery-services-vault"></a>Kurtarma hizmetleri kasası oluşturma

1. Azure portalında **+ kaynak Oluştur**ve ardından Azure Marketi için kurtarma Hizmetleri arayın.
2. Seçin **Backup ve Site Recovery (OMS)** . Sonra sihirbazın **Backup ve Site Recovery** sayfasında **Oluştur**.
1. İçinde **kurtarma Hizmetleri kasası > adı**, kasayı tanımlamak için bir kolay ad girin. Bu öğretici için **ContosoVMVault** adını kullanın.
2. İçinde **kaynak grubu**, mevcut bir kaynak grubunu seçin veya yeni bir tane oluşturun. Bu öğretici için kullanmak **contosoRG**.
3. İçinde **konumu**, burada kasa olmalıdır bölgeyi seçin. Bu öğretici için kullanmak **Batı Avrupa**.
4. Panodan kasaya hızlı şekilde erişmek için **Panoya sabitle** > **Oluştur**’u seçin.

![Yeni kasa oluştur](./media/tutorial-prepare-azure/new-vault-settings.png)

Yeni kasa, **Pano** > **Tüm kaynaklar** bölümünde ve ana **Kurtarma Hizmetleri kasaları** sayfasında görüntülenir.

## <a name="set-up-an-azure-network"></a>Azure ağı ayarlama

Yük devretmeden sonra depolamadan Azure sanal makineleri oluşturulduğunda sanal makineler bu ağa katılır.

1. [Azure portalında](https://portal.azure.com) **Kaynak oluştur** > **Ağ** > **Sanal ağ** seçeneklerini belirleyin. Resource Manager dağıtım modeli olarak seçili bırakın.
2. **Ad** bölümünde bir ağ adı girin. Ad, Azure kaynak grubu içinde benzersiz olmalıdır. Bu öğretici için kullanmak **ContosoASRnet**.
3. Ağın oluşturulacağı kaynak grubunu belirtin. Bu öğreticide, mevcut kaynak grubunu kullanın **contosoRG**.
4. İçinde **adres aralığı**, girin **10.0.0.0/24** ağ aralığı. Bu ağ için alt ağ yok.
5. **Abonelik** bölümünde ağın oluşturulacağı aboneliği seçin.
6. İçinde **konumu**, seçin **Batı Avrupa**. Ağın, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.
7. Hiçbir Ağ Hizmeti uç noktası ile temel DDoS Koruması varsayılan seçenekleri bırakın.
8. **Oluştur**’u seçin.

![Sanal ağ oluşturma](media/tutorial-prepare-azure/create-network.png)

Sanal ağın oluşturulması birkaç saniye sürer. Oluşturulduktan sonra Azure portalı panosunda görürsünüz.

## <a name="useful-links"></a>Yararlı bağlantılar

Hakkında bilgi edinin:
- [Azure ağı](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)
- [Yönetilen diskler](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview)



## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Şirket içi Hyper-V altyapınızı azure'a olağanüstü durum kurtarmaya hazırlama](hyper-v-prepare-on-premises-tutorial.md)
