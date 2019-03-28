---
title: Azure'da dağıtma Linux sanal makine sorunlarını giderme | Microsoft Docs
description: Azure Resource Manager dağıtım modelinde dağıtma Linux sanal makine sorunlarını giderin.
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/01/2018
ms.author: genli
ms.openlocfilehash: 1317a4731d3598c5fba317167ba4a45d95823ca2
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58539828"
---
# <a name="troubleshoot-deploying-linux-virtual-machine-issues-in-azure"></a>Azure'da dağıtma Linux sanal makine sorunlarını giderme

Azure'da sanal makine (VM) dağıtım sorunlarını giderme için inceleyin [üst sorunları](#top-issues) genel hatalar ve çözümleri için.

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.

## <a name="top-issues"></a>En sık karşılaşılan sorunlar
[!INCLUDE [virtual-machines-linux-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

## <a name="the-cluster-cannot-support-the-requested-vm-size"></a>İstenen VM boyutu küme desteği
\<properties supportTopicIds="123456789" resourceTags="windows" productPesIds="1234, 5678" />
- Daha küçük bir VM boyutu isteği yeniden deneyin.
- İstenen VM boyutu değiştirilemiyorsa:
    - Kullanılabilirlik kümesindeki tüm sanal makineler durdurun. Tıklayın **kaynak grupları** > kaynak grubunuzun > **kaynakları** > kullanılabilirlik kümeniz > **sanal makineler** > sanal makinenizi >  **Durdur**.
    - Tüm sanal makineleri durdurduktan sonra istenen boyutu VM oluşturun.
    - İlk olarak, yeni VM'yi başlatın ve ardından her biri durdurulmuş sanal makineler seçin ve Başlat'a tıklayın.


## <a name="the-cluster-does-not-have-free-resources"></a>Kümenin boş kaynak yok
\<properties supportTopicIds="123456789" resourceTags="windows" productPesIds="1234, 5678" />
- İstek daha sonra yeniden deneyin.
- Yeni VM'yi farklı bir kullanılabilirlik kümesinin bir parçası olarak
    - Bir VM'yi (aynı bölgede) farklı bir kullanılabilirlik oluşturun.
    - Yeni VM, aynı sanal ağa ekleyin.

## <a name="how-do-i-activate-my-monthly-credit-for-visual-studio-enterprise-bizspark"></a>Visual studio Enterprise (BizSpark) için aylık kredimle nasıl etkinleştirebilirim

Aylık kredinizi etkinleştirmek için bu bkz [makale](https://azure.microsoft.com/offers/ms-azr-0064p/).

## <a name="why-can-i-not-install-the-gpu-driver-for-an-ubuntu-nv-vm"></a>Neden GPU sürücüsünün Ubuntu NV sanal makinesi için yükleyebilirim değil?

GPU Linux desteği şu anda yalnızca Azure NC Ubuntu Server 16.04 LTS çalıştıran Vm'lerde olarak kullanılabilir. Daha fazla bilgi için [Linux çalıştıran N serisi VM'ler için GPU sürücülerini ayarlamak](../linux/n-series-driver-setup.md).

## <a name="my-drivers-are-missing-for-my-linux-n-series-vm"></a>My sürücüleri Linux N serisi sanal Makinem için eksik

Linux tabanlı VM'ler için sürücüleri yer [burada](../linux/n-series-driver-setup.md). 

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a>N serisi sanal Makinem içinde bir GPU örneği bulunamıyor

Windows Server 2016 veya Windows Server 2012 R2 çalıştıran Azure N serisi sanal makineler GPU yeteneklerinden yararlanmak için NVIDIA grafik sürücüleri dağıtımdan sonra her bir VM üzerinde yüklemeniz gerekir. Sürücü Kurulum bilgilerini kullanılabilir [Windows Vm'leri](../windows/n-series-driver-setup.md) ve [Linux Vm'leri](../linux/n-series-driver-setup.md).

## <a name="is-n-series-vms-available-in-my-region"></a>N serisi VM'ler, bulunduğum bölgede kullanılabilir mi?

Kullanılabilirlik denetleyebilirsiniz [bölge tablosuna göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services)ve fiyatlandırma [burada](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).

## <a name="i-am-not-able-to-see-vm-size-family-that-i-want-when-resizing-my-vm"></a>Sanal Makinem yeniden boyutlandırırken istiyorum VM boyutu ailesi görmeye değilim.

Bir VM çalışırken, bir fiziksel sunucuda yeniden dağıtılır. Fiziksel sunucuları Azure bölgelerinde genel fiziksel donanım kümelerinde gruplandırılır. Farklı donanım kümelerine taşınacak VM gerektiren bir VM'yi yeniden boyutlandırmadan hangi dağıtım modeli VM dağıtmak için kullanılan bağlı olarak farklılık gösterir.

- Klasik dağıtım modelinde, bulut hizmeti dağıtımı dağıtılan VM'lerin kaldırıldı ve sanal makineleri başka bir boyutu ailesinin bir boyutu değiştirmek için yeniden dağıtıldı.

- Resource Manager dağıtım modelinde dağıtılan VM'lerin, kullanılabilirlik kümesindeki kullanılabilirlik kümesindeki herhangi bir VM boyutunu değiştirmeden önce tüm Vm'leri durdurmanız gerekir.

## <a name="the-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a>Listelenen VM boyutu, kullanılabilirlik kümesi'nde dağıtırken desteklenmiyor.

Kullanılabilirlik kümesinin kümesinde desteklenen bir boyut seçin. Bir kullanılabilirlik ilk dağıtımınızı kullanılabilirlik kümesine olması gerekir ve sahip düşündüğünüz en büyük VM boyutu seçmenizi kümesi oluştururken önerilir.

## <a name="what-linux-distributionsversions-are-supported-on-azure"></a>Azure'da hangi Linux dağıtımları/sürümleri destekleniyor?

Linux listesini bulabilirsiniz [Azure-Endorsed dağıtımları](../linux/endorsed-distros.md).

## <a name="can-i-add-an-existing-classic-vm-to-an-availability-set"></a>Bir kullanılabilirlik kümesine mevcut bir Klasik VM'yi ekleyebilir miyim?

Evet. Var olan bir Klasik VM'yi bir yeni veya mevcut kullanılabilirlik kümesine ekleyebilirsiniz. Daha fazla bilgi için [mevcut bir sanal makine bir kullanılabilirlik kümesine ekleme](/previous-versions/azure/virtual-machines/windows/classic/configure-availability-classic#addmachine).


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/).

Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.
