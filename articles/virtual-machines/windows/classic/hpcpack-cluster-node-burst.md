---
title: Bir HPC Pack kümesine kapasite aşımı düğümlerini ekleyin | Microsoft Docs
description: Bir HPC Pack kümesine isteğe bağlı Azure içinde çalışan bir bulut hizmetinde çalışan rolü örnekleri eklenerek öğrenin
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
ms.openlocfilehash: 7d42c026975a18c7574e4bc64ec28ab3ed0082bc
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51248461"
---
# <a name="add-on-demand-burst-nodes-to-an-hpc-pack-cluster-in-azure"></a>Azure'da bir HPC Pack kümesine isteğe bağlı "patlama" düğümleri Ekle
Ayarlarsanız bir [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) küme Azure'da hızla küme kapasitesi ölçeği artırın veya azaltın, önceden yapılandırılmış işlem düğümü sanal makinelerinde bir dizi korumadan için bir yol isteyebilirsiniz. Bu makalede azure'da bir baş düğüm için işlem kaynakları olarak isteğe bağlı "patlama" düğümleri (çalışan bir bulut hizmetinde çalışan rolü örnekleri) eklemek nasıl gösterir. 

> [!IMPORTANT] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

![Kapasite aşımı düğümlerini][burst]

Bu makaledeki adımlarda bir bulut tabanlı HPC paketi üstbilgi düğüm VM'ine bir test veya kavram kanıtı dağıtımı için hızlı bir şekilde Azure düğümleri eklemenize yardımcı olur. Üst düzey adımlar "Azure'a geçmenize" adımları aynıdır eklemek için bulut bilgi işlem, bir şirket içi HPC Pack kümesine kapasitesi. Bir öğretici için bkz. [Microsoft HPC Pack ile karma işlem kümesi ayarlama](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Ayrıntılı yönergeler ve üretim dağıtımlarında değerlendirmeleri için bkz. [Microsoft HPC Pack ile azure'a veri bloğu](https://technet.microsoft.com/library/gg481749.aspx).

## <a name="prerequisites"></a>Önkoşullar
* **Dağıtılan bir Azure VM'de HPC paketi üstbilgi düğümü** -tek başına bir üstbilgi düğüm VM'ine ya da daha büyük bir kümenin parçası olan birini kullanabilirsiniz. Tek başına bir baş düğüm oluşturmak için bkz [bir Azure VM'de HPC paketi baş düğümü dağıtma](../../virtual-machines-windows-hpcpack-cluster-headnode.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Otomatik HPC Pack küme dağıtım seçenekleri için bkz. [kümesi oluşturmak ve bir Windows HPC yönetmek için seçenekleri Microsoft HPC Pack ile azure'da](../../virtual-machines-windows-hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
  
  > [!TIP]
  > Kullanırsanız [HPC Pack Iaas dağıtım betiği](hpcpack-cluster-powershell-script.md) Azure'da küme oluşturma için Azure kapasite aşımı düğümlerini otomatik dağıtımınızda dahil edebilirsiniz. Bu makaledeki örneklere bakın.
  > 
  > 
* **Azure aboneliği** - Azure düğümleri eklemek için baş düğüm VM dağıtmak için kullanılan aynı abonelikte veya farklı bir abonelik (veya abonelikler) seçebilirsiniz.
* **Çekirdek kota** -özellikle birden fazla çekirdekli boyutları ile birçok Azure düğümleri dağıtmayı seçerseniz, çekirdek kotasını artırmanız gerekebilir. Bir Kotayı artırmak için [bir çevrimiçi müşteri destek isteği açın](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) ücret olmadan.

## <a name="step-1-create-a-cloud-service-and-a-storage-account-for-the-azure-nodes"></a>1. adım: bir bulut hizmeti ve Azure düğümleri için bir depolama hesabı oluşturma
Azure düğümlerinizi dağıtmak için gereken aşağıdaki kaynaklarını yapılandırmak için Azure portalı ya da eşdeğer araçlarını kullanın:

* Yeni bir Azure bulut hizmeti (Klasik)
* Yeni bir Azure depolama hesabı (Klasik)

> [!NOTE]
> Aboneliğinizdeki mevcut bir bulut hizmetinin yeniden kullanmayın. 
> 
> 

**Dikkat edilmesi gerekenler**

* Oluşturmayı planladığınız her bir Azure düğümüne şablonu için ayrı bir bulut hizmeti yapılandırın. Ancak, birden çok düğüm şablonları için aynı depolama hesabını kullanabilirsiniz.
* Bulut hizmeti ve dağıtım için depolama hesabı aynı Azure bölgesinde bulun öneririz.

## <a name="step-2-configure-an-azure-management-certificate"></a>2. adım: Azure yönetim sertifikası yapılandırma
İşlem kaynağı olarak Azure düğümleri eklemek için baş düğüm ve karşıya yükleme karşılık gelen sertifika dağıtım için kullanılan bir Azure aboneliğine yönetim sertifikası gerekir.

Bu senaryo için seçtiğiniz **varsayılan HPC Azure yönetim sertifikası** HPC Pack yükleyen ve baş düğüme otomatik olarak yapılandırır. Bu sertifika, amaçları ve kavram kanıtı dağıtımı test etmek için kullanışlıdır. Bu sertifikayı kullanmak için abonelik dosyası C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer VM baş düğümünden karşıya yükleyin. Sertifikayı karşıya yüklemek için [Azure portalında](https://portal.azure.com):

1. Tıklayın **abonelikleri** > *your_subscription_name*.

2. Tıklayın **yönetim sertifikaları** > **karşıya**.

Yönetim sertifikası yapılandırmak ek seçenekler için bkz. [Azure veri bloğu dağıtımları için Azure yönetim sertifikası yapılandırma senaryolarını](https://technet.microsoft.com/library/gg481759.aspx).

## <a name="step-3-deploy-azure-nodes-to-the-cluster"></a>3. adım: Azure düğümleri kümeye dağıtma
Ekleyin ve bu senaryoda Azure düğümleri başlangıç adımları genellikle şirket içi baş düğüm adımlarla aynıdır. Aşağıdaki bölümlerde daha fazla bilgi için bkz. [Microsoft HPC Pack ile Azure düğümlerine dağıtma adımları](https://technet.microsoft.com/library/gg481758.aspx):

* Bir Azure düğümüne şablonu oluşturma
* Windows HPC kümesine Azure düğümleri ekleme
* (Sağlayamaz) Azure düğümleri Başlat

Bunlar, ekleyin ve düğümler başlar sonra küme işlerini çalıştırmak için kullanımınıza hazır olursunuz.

Azure düğümleri dağıtırken sorunlarla karşılaşırsanız bkz [sorun giderme dağıtımları, Azure düğümleri Microsoft HPC Pack ile](https://technet.microsoft.com/library/jj159097.aspx).

## <a name="next-steps"></a>Sonraki adımlar
* Yoğun işlem gücü kullanımlı örnek boyutu için kapasite aşımı düğümlerini kullanılacak hususlarına bkz [yüksek performanslı işlem VM boyutları](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* İsterseniz otomatik olarak büyütün veya Azure bulut bilgi işlem küme iş yüküne göre kaynakları küçültme, bkz: [otomatik olarak büyütme ve küçültme bir HPC Pack kümesinde Azure işlem kaynaklarını](hpcpack-cluster-node-autogrowshrink.md).

<!--Image references-->
[burst]: ./media/hpcpack-cluster-node-burst/burst.png
