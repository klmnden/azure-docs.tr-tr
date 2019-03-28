---
title: Azure'da dağıtma Windows sanal makine sorunlarını giderme | Microsoft Docs
description: Azure Resource Manager dağıtım modelinde dağıtma Windows sanal makine sorunlarını giderin.
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
ms.openlocfilehash: 5752731f08a7dc9ae8661e698aef9655837c6220
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58540712"
---
# <a name="troubleshoot-deploying-windows-virtual-machine-issues-in-azure"></a>Azure'da dağıtma Windows sanal makine sorunlarını giderme

Azure'da sanal makine (VM) dağıtım sorunlarını giderme için inceleyin [üst sorunları](#top-issues) genel hatalar ve çözümleri için.

Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.

## <a name="top-issues"></a>En sık karşılaşılan sorunlar
[!INCLUDE [virtual-machines-windows-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

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

## <a name="how-can-i-use-and-deploy-a-windows-client-image-into-azure"></a>Nasıl kullanın ve windows istemci görüntüsünü Azure'a dağıtabilirsiniz?

Uygun Visual Studio (eski adıyla MSDN) aboneliğiniz varsa Azure'da Windows 7, Windows 8 veya Windows 10 için geliştirme/test senaryoları kullanabilirsiniz. Bu [makale](../windows/client-images.md) azure'da çalışan bir Windows istemci için uygunluk gereksinimlerini özetler ve Azure Galerisi görüntülerini kullanır.

## <a name="how-can-i-deploy-a-virtual-machine-using-the-hybrid-use-benefit-hub"></a>Karma kullanım Avantajı'nı (HUB) kullanarak bir sanal makine nasıl dağıtabilir miyim?

Birkaç Azure hibrit kullanım Teklifi'nden yararlanarak Windows sanal makineleri dağıtmak için farklı yolu vardır.

Bir Kurumsal Anlaşma abonelik için:

• Azure hibrit kullanım teklifi ile önceden yapılandırılmış olan belirli Market görüntülerinden sanal makineler dağıtan.

Kurumsal Anlaşma için:

• Özel bir VM'yi karşıya yükleme ve bir Resource Manager şablonu veya Azure PowerShell kullanarak dağıtın.

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

 - [Azure hibrit kullanım teklifi genel bakış](https://azure.microsoft.com/pricing/hybrid-use-benefit/)

 - [İndirilebilir SSS](https://download.microsoft.com/download/4/2/1/4211AC94-D607-4A45-B472-4B30EDF437DE/Windows_Server_Azure_Hybrid_Use_FAQ_EN_US.pdf)

 - [Azure hibrit kullanım teklifi Windows Server ve Windows İstemcisi için](../windows/hybrid-use-benefit-licensing.md).

 - [Azure hibrit kullanım teklifi nasıl kullanabilirim](https://blogs.msdn.microsoft.com/azureedu/2016/04/13/how-can-i-use-the-hybrid-use-benefit-in-azure)

## <a name="how-do-i-activate-my-monthly-credit-for-visual-studio-enterprise-bizspark"></a>Visual studio Enterprise (BizSpark) için aylık kredimle nasıl etkinleştirebilirim

Aylık kredinizi etkinleştirmek için bu bkz [makale](https://azure.microsoft.com/offers/ms-azr-0064p/).

## <a name="how-to-add-enterprise-devtest-to-my-enterprise-agreement-ea-to-get-access-to-window-client-images"></a>Kurumsal geliştirme ve Test için kendi Kurumsal Sözleşme (penceresi istemci görüntülerine erişim elde etmek için EA) ekleme?

Yalnızca, Kurumsal Yönetici tarafından Kurumsal Geliştirme ve Test teklifi kapsamında abonelik oluşturma izni verilen Hesap Sahipleri bu işlemi yapabilir. Hesap sahibi Azure hesap Portalı aracılığıyla abonelik oluşturur ve sonra etkin Visual Studio aboneleri ortak Yöneticiler olarak eklemesi gerekir. Yönetme ve geliştirme ve test işlemlerinin gerektirdiği kaynakların böylece. Daha fazla bilgi için [Kurumsal geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0148p/).

## <a name="my-drivers-are-missing-for-my-windows-n-series-vm"></a>My sürücüleri Windows N serisi sanal Makinem için eksik

Windows tabanlı sanal makineler için sürücüleri yer [burada](../windows/n-series-driver-setup.md).

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a>N serisi sanal Makinem içinde bir GPU örneği bulunamıyor

Windows Server 2016 veya Windows Server 2012 R2 çalıştıran Azure N serisi sanal makineler GPU yeteneklerinden yararlanmak için NVIDIA grafik sürücüleri dağıtımdan sonra her bir VM üzerinde yüklemeniz gerekir. Sürücü Kurulum bilgilerini kullanılabilir [Windows Vm'leri](../windows/n-series-driver-setup.md) ve [Linux Vm'leri](../linux/n-series-driver-setup.md).

## <a name="is-n-series-vms-available-in-my-region"></a>N serisi VM'ler, bulunduğum bölgede kullanılabilir mi?

Kullanılabilirlik denetleyebilirsiniz [bölge tablosuna göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services)ve fiyatlandırma [burada](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).

## <a name="what-client-images-can-i-use-and-deploy-in-azure-and-how-to-i-get-them"></a>Hangi istemci görüntülerini kullanma ve miyim Azure'da dağıtmak ve nasıl için edinilir?

Azure'da geliştirme/test senaryoları için Windows 10, uygun bir Visual Studio (eski adıyla MSDN) aboneliğiniz sağlanan ya da Windows 7, Windows 8, kullanabilirsiniz. 

- Windows 10 görüntüleri kullanılabilir içinde Azure Galerisi'ndeki [uygun geliştirme ve test teklifleri](../windows/client-images.md#eligible-offers). 
- Visual Studio aboneleri teklif herhangi bir tür içinde aynı zamanda [yeterince hazırlayın ve oluşturun](../windows/prepare-for-upload-vhd-image.md) bir 64 bit Windows 7, Windows 8 veya Windows 10 görüntüsü ve ardından [karşıya Azure'a](../windows/upload-generalized-managed.md). Kullanımı, etkin Visual Studio aboneleri tarafından geliştirme ve test için sınırlı kalır.

Bu [makale](../windows/client-images.md) Azure'da ve Azure Galerisi'ndeki görüntüleri kullanımını çalışan Windows İstemcisi için uygunluk gereksinimlerini özetler.

## <a name="i-am-not-able-to-see-vm-size-family-that-i-want-when-resizing-my-vm"></a>Sanal Makinem yeniden boyutlandırırken istiyorum VM boyutu ailesi görmeye değilim.

Bir VM çalışırken, bir fiziksel sunucuda yeniden dağıtılır. Fiziksel sunucuları Azure bölgelerinde genel fiziksel donanım kümelerinde gruplandırılır. Farklı donanım kümelerine taşınacak VM gerektiren bir VM'yi yeniden boyutlandırmadan hangi dağıtım modeli VM dağıtmak için kullanılan bağlı olarak farklılık gösterir.

- Klasik dağıtım modelinde, bulut hizmeti dağıtımı dağıtılan VM'lerin kaldırıldı ve sanal makineleri başka bir boyutu ailesinin bir boyutu değiştirmek için yeniden dağıtıldı.

- Resource Manager dağıtım modelinde dağıtılan VM'lerin, kullanılabilirlik kümesindeki kullanılabilirlik kümesindeki herhangi bir VM boyutunu değiştirmeden önce tüm Vm'leri durdurmanız gerekir.

## <a name="the-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a>Listelenen VM boyutu, kullanılabilirlik kümesi'nde dağıtırken desteklenmiyor.

Kullanılabilirlik kümesinin kümesinde desteklenen bir boyut seçin. Bir kullanılabilirlik ilk dağıtımınızı kullanılabilirlik kümesine olması gerekir ve sahip düşündüğünüz en büyük VM boyutu seçmenizi kümesi oluştururken önerilir.

## <a name="can-i-add-an-existing-classic-vm-to-an-availability-set"></a>Bir kullanılabilirlik kümesine mevcut bir Klasik VM'yi ekleyebilir miyim?

Evet. Var olan bir Klasik VM'yi bir yeni veya mevcut kullanılabilirlik kümesine ekleyebilirsiniz. Daha fazla bilgi için [mevcut bir sanal makine bir kullanılabilirlik kümesine ekleme](/previous-versions/azure/virtual-machines/windows/classic/configure-availability-classic#addmachine).


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede herhangi bir noktada daha fazla yardıma ihtiyacınız olursa, üzerinde Azure uzmanlarıyla iletişime geçebilirsiniz [Azure MSDN ve Stack Overflow forumları](https://azure.microsoft.com/support/forums/).

Alternatif olarak, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.
