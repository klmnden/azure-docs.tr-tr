---
title: Veri bloğu düğümleri bir HPC Pack kümeye ekleme | Microsoft Docs
description: İsteğe bağlı Azure HPC Pack kümede çalışan rolü örnekleri bir bulut hizmetinde çalışan eklenerek öğrenin
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-service-management,hpc-pack
ms.assetid: 24b79a8a-24ad-4002-ae76-75abc9b28c83
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 10/14/2016
ms.author: danlep
ms.openlocfilehash: eee9183321f21676271c8a9c7e023c80c4daf554
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30915117"
---
# <a name="add-on-demand-burst-nodes-to-an-hpc-pack-cluster-in-azure"></a>İsteğe bağlı "aşırı" düğümleri Azure HPC Pack kümede ekleyin
Ayarladığınız varsa bir [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) küme Azure üzerinde bir şekilde kolayca küme kapasite yukarı veya aşağı, önceden yapılandırılmış işlem düğümü VM'ler kümesi korumadan ölçek isteyebilirsiniz. Bu makalede, isteğe bağlı "aşırı" düğümleri (çalışan rolü örnekleri bir bulut hizmetinde çalışan) eklemek baş düğümüne Azure işlem kaynakları olarak gösterilmiştir. 

> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

![Düğümleri veri bloğu][burst]

Bu makaledeki adımları Azure düğümleri bir bulut tabanlı HPC paketi üstbilgi düğüm VM'ine test veya kavram kanıtı dağıtımı için hızlı bir şekilde eklemek yardımcı olur. Üst düzey adımları "İçin Azure veri bloğu için" adımları aynıdır bulut işlem kapasitesini bir şirket içi HPC Pack kümeye eklemek için. Bir öğretici için bkz: [Microsoft HPC paketi ile karma işlem kümesi ayarlama](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Ayrıntılı yönergeler ve üretim dağıtımlarında değerlendirmeleri için bkz: [Microsoft HPC Pack ile azure'a veri bloğu](https://technet.microsoft.com/library/gg481749.aspx).

## <a name="prerequisites"></a>Önkoşullar
* **Bir Azure VM dağıtılan HPC paketi üstbilgi düğümü** -tek başına bir baş düğüm VM veya daha büyük bir kümenin parçası olan bir kullanabilirsiniz. Tek başına bir baş düğüm oluşturmak için bkz: [bir HPC Pack baş düğümünde Azure VM'deki dağıtmak](../../virtual-machines-windows-hpcpack-cluster-headnode.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Otomatik HPC paketi küme dağıtım seçenekleri için bkz: [küme oluşturmak ve bir Windows HPC yönetmek için seçenekleri Microsoft HPC Pack ile azure'da](../../virtual-machines-windows-hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
  
  > [!TIP]
  > Kullanırsanız [HPC Pack Iaas dağıtım betiği](hpcpack-cluster-powershell-script.md) küme oluşturmak için Azure veri bloğu düğümler otomatik dağıtımınızda dahil edebilirsiniz. Bu makaledeki örneklerde bakın.
  > 
  > 
* **Azure aboneliği** - Azure düğümleri eklemek için baş düğüm VM dağıtmak için kullanılan aynı abonelik veya farklı bir abonelik (ya da abonelik) seçebilirsiniz.
* **Çekirdek kota** -özellikle çok çekirdekli boyutlarıyla birkaç Azure düğümleri dağıtmak isterseniz, çekirdek, Kotayı artırmak gerekebilir. Bir Kotayı artırmak için [bir çevrimiçi müşteri destek isteği açma](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) herhangi bir ücret alınmaz.

## <a name="step-1-create-a-cloud-service-and-a-storage-account-for-the-azure-nodes"></a>1. adım: bir bulut hizmeti ve Azure düğümleri için bir depolama hesabı oluşturma
Azure Portalı'nı veya eşdeğer Araçlar Azure düğümleriniz dağıtmak için gereken aşağıdaki kaynaklarını yapılandırmak için kullanın:

* Yeni bir Azure bulut hizmeti (Klasik)
* Yeni bir Azure depolama hesabı (Klasik)

> [!NOTE]
> Aboneliğinizde var olan bir bulut hizmeti yeniden yok. 
> 
> 

**Dikkat edilmesi gerekenler**

* Oluşturmayı planladığınız her Azure düğüm şablonu için ayrı bulut hizmeti yapılandırın. Ancak, birden çok düğüm şablonları için aynı depolama hesabı kullanabilirsiniz.
* Bulut Hizmeti'nin ve dağıtım için bir depolama hesabı aynı Azure bölgesinde bulun öneririz.

## <a name="step-2-configure-an-azure-management-certificate"></a>2. adım: Azure yönetim sertifikası yapılandırma
İşlem kaynakları Azure düğümleri eklemek için karşılık gelen bir sertifika dağıtımı için kullanılan Azure aboneliğine baş düğüm ve karşıya bir yönetim sertifikası gerekir.

Bu senaryo için seçtiğiniz **varsayılan HPC Azure yönetim sertifikası** HPC paketi yükler ve baş düğümünde otomatik olarak yapılandırır. Bu sertifika, amaçları ve kavram kanıtı dağıtımları test etmek için kullanışlıdır. Bu sertifikanın kullanılması için aboneliğe dosyası C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer VM baş düğümünden karşıya yükleyin. Sertifikayı karşıya yüklemek için [Azure portal](https://portal.azure.com):

1. Tıklatın **abonelikleri** > *your_subscription_name*.

2. Tıklatın **yönetim sertifikaları** > **karşıya**.

Yönetim sertifikası yapılandırmak için ek seçenekler için bkz: [Azure veri bloğu dağıtımları için Azure yönetim sertifikası yapılandırmak için senaryolar](http://technet.microsoft.com/library/gg481759.aspx).

## <a name="step-3-deploy-azure-nodes-to-the-cluster"></a>3. adım: Azure düğümleri kümeye dağıtma
Ekleyin ve bu senaryoda Azure düğümleri başlangıç adımları genellikle bir şirket içi baş düğüm adımlarla aynıdır. Aşağıdaki bölümlerde daha fazla bilgi için bkz: [Microsoft HPC paketi ile Azure düğümlerine dağıtma adımları](https://technet.microsoft.com/library/gg481758.aspx):

* Bir Azure düğüm şablonu oluşturma
* Windows HPC kümesine Azure düğümleri eklemek
* (Sağlayamaz) Azure düğümleri Başlat

Ekleyip düğümleri Başlat sonra küme işlerini çalıştırmak hazır.

Azure düğümleri dağıtımı sırasında sorunlarla karşılaşırsanız, bkz: [sorun giderme dağıtımları, Azure düğümleri Microsoft HPC paketi ile](http://technet.microsoft.com/library/jj159097.aspx).

## <a name="next-steps"></a>Sonraki adımlar
* Bir işlem yoğunluklu örnek boyutu için veri bloğu düğümleri kullanmak için konuları Bkz [yüksek performanslı işlem VM boyutları](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Otomatik olarak Büyüt veya bilgi işlem küme iş yükü göre kaynakları Azure Küçült, görmek istiyorsanız, [otomatik olarak Büyüt ve Azure işlem kaynakları bir HPC Pack kümesindeki Küçült](hpcpack-cluster-node-autogrowshrink.md).

<!--Image references-->
[burst]: ./media/hpcpack-cluster-node-burst/burst.png
