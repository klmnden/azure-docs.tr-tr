---
title: "Azure'da dağıtma Windows sanal makine sorunlarını giderme | Microsoft Docs"
description: "Azurethe Resource Manager dağıtım modelinde dağıtma Windows sanal makine sorunlarını giderin."
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 6a1697061d20d26b4263c02487180fee81e87947
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="troubleshoot-deploying-windows-virtual-machine-issues-in-azure"></a>Azure'da dağıtma Windows sanal makine sorunlarını giderme

Azure'da sanal makine (VM) dağıtım sorunlarını gidermek için inceleyin [üst sorunları](#top-issues) yaygın hatalar ve çözümleri için.

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.

## <a name="top-issues"></a>Üst sorunları
[!INCLUDE [virtual-machines-windows-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

## <a name="the-cluster-cannot-support-the-requested-vm-size"></a>Küme istenen VM boyutu desteklemiyor
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- Daha küçük bir VM boyutu kullanarak isteği yeniden deneyin.
- İstenen VM boyutu değiştirilemiyorsa:
    - Kullanılabilirlik kümesindeki tüm sanal makineleri durdurun. Tıklatın **kaynak grupları** > kaynak grubunuz > **kaynakları** > kullanılabilirlik kümesi > **sanal makineleri** > sanal makineniz > **durdurmak**.
    - Tüm sanal makineleri durdurduktan sonra istenen boyutta VM oluşturun.
    - İlk olarak, yeni VM başlatmak ve durdurulmuş VM'lerin her birinde seçin ve Başlat'ı tıklatın.


## <a name="the-cluster-does-not-have-free-resources"></a>Küme kaynakları serbest bırakmak yok
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- İstek daha sonra yeniden deneyin.
- Yeni VM'yi farklı bir kullanılabilirlik kümesinin bir parçası olarak
    - Farklı bir kullanılabilirlik (aynı bölgede) kümesindeki bir VM oluşturun.
    - Yeni VM aynı sanal ağa ekleyin.

## <a name="how-can-i-use-and-deploy-a-windows-client-image-into-azure"></a>Nasıl kullanın ve windows istemci görüntüsünü Azure'a dağıtabilirsiniz?

Uygun bir Visual Studio (önceki adıyla MSDN) aboneliğiniz varsa, Windows 7, Windows 8 veya Windows 10 Azure üzerinde geliştirme ve test senaryoları için kullanabilirsiniz. Bu [makale](client-images.md) azure'da çalışan Windows İstemcisi için uygunluk gereksinimleri özetler ve Azure Galerisi görüntülerini kullanır.

## <a name="how-can-i-deploy-a-virtual-machine-using-the-hybrid-use-benefit-hub"></a>Karma kullanımı Avantajı (HUB) kullanarak bir sanal makine nasıl dağıtabilir miyim?

Çeşitli Windows Azure karma kullanımı avantajı ile sanal makineleri dağıtmak için farklı şekillerde vardır.

Bir Kurumsal Anlaşma abonelik için:

• Azure karma kullanımı avantajı ile önceden yapılandırılmış belirli Market görüntülerden sanal makineleri dağıtma.

Kurumsal Anlaşma için:

• Özel bir VM karşıya yükleyin ve bir Resource Manager şablonu veya Azure PowerShell kullanarak dağıtın.

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

 - [Azure karma kullanımı avantajı genel bakış](https://azure.microsoft.com/pricing/hybrid-use-benefit/)

 - [İndirilebilir SSS](http://download.microsoft.com/download/4/2/1/4211AC94-D607-4A45-B472-4B30EDF437DE/Windows_Server_Azure_Hybrid_Use_FAQ_EN_US.pdf)

 - [Windows Server ve Windows İstemcisi için Azure karma kullanımı avantajı](hybrid-use-benefit-licensing.md).

 - [Azure'da karma kullanma avantajını nasıl kullanabilirim](https://blogs.msdn.microsoft.com/azureedu/2016/04/13/how-can-i-use-the-hybrid-use-benefit-in-azure)

## <a name="how-do-i-activate-my-monthly-credit-for-visual-studio-enterprise-bizspark"></a>Visual studio Enterprise (BizSpark) için aylık my kredi nasıl etkinleştirilsin mi

Aylık kredinizin etkinleştirmek için bu bkz [makale](https://azure.microsoft.com/offers/ms-azr-0064p/).

## <a name="how-to-add-enterprise-devtest-to-my-enterprise-agreement-ea-to-get-access-to-window-client-images"></a>Enterprise geliştirme ve Test için my Kurumsal Anlaşma (penceresi istemci görüntülerine erişmek için EA) ekleme?

Bir kuruluş yöneticisi tarafından Bunu yapmak için izin verilen hesap sahiplerine Enterprise geliştirme ve Test teklifini dayalı abonelikleri oluşturma yeteneği sınırlıdır. Hesap sahibinin abonelikleri Azure hesap Portalı aracılığıyla oluşturur ve ardından etkin Visual Studio abonelerinden ortak yönetici olarak eklemeniz gerekir. Yönetme ve geliştirme ve test için gereken kaynakları kullanmak böylece. Daha fazla bilgi için bkz: [Enterprise geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0148p/).

## <a name="my-drivers-are-missing-for-my-windows-n-series-vm"></a>My sürücüleri için Windows N-serisi VM'ime eksik

Windows tabanlı VM'ler için sürücülerin bulunduğu [burada](n-series-driver-setup.md).

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a>GPU örneği N-serisi VM'im içinde bulunamıyor

Windows Server 2016 veya Windows Server 2012 R2 çalıştıran Azure N-serisi VM'ler GPU yeteneklerinden yararlanabilmek için NVIDIA grafik sürücüleri dağıtımdan sonra her VM yüklemeniz gerekir. Sürücü Kurulum bilgileri yüklenebilir [Windows Vm'lerini](n-series-driver-setup.md) ve [Linux VM'ler](../linux/n-series-driver-setup.md).

## <a name="are-client-images-supported-for-n-series"></a>İstemci görüntüleri N-seri için destekleniyor mu?

Şu anda Azure yalnızca Windows Server ve Linux işletim sistemlerini çalıştıran sanal makinelerin N-serisi destekler.

## <a name="is-n-series-vms-available-in-my-region"></a>N-serisi VM'ler bulunduğum bölgede var mı?

Kullanılabilirlik denetleyebilirsiniz [bölge tablosu tarafından kullanılabilen ürünler](https://azure.microsoft.com/regions/services)ve fiyatlandırma [burada](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).

## <a name="what-client-images-can-i-use-and-deploy-in-azure-and-how-to-i-get-them"></a>Hangi istemci görüntüleri ı kullanabilir ve Azure'da dağıtmak ve için güncelleştirmeleri nasıl edinebilirim?

Windows 10 azure'da geliştirme ve test senaryoları için uygun bir Visual Studio (önceki adıyla MSDN) aboneliğiniz olması koşuluyla veya Windows 7, Windows 8 kullanabilirsiniz. 

- Windows 10 görüntüleri kullanılabilir içinde Azure galerisinden [uygun geliştirme ve test sunar](client-images.md#eligible-offers). 
- Visual Studio abonelerinden teklif herhangi bir türde içinde için de [yeterli hazırlayın ve oluşturun](prepare-for-upload-vhd-image.md) bir 64-bit Windows 7, Windows 8 veya Windows 10 görüntüsü ve ardından [karşıya yüklemek için Azure](upload-generalized-managed.md). Kullanım geliştirme ve test için etkin Visual Studio aboneler tarafından sınırlı kalır.

Bu [makale](client-images.md) Azure ve Azure galeri görüntüleri kullanımını çalışan Windows İstemcisi için uygunluk gereksinimleri özetlenmektedir.

## <a name="i-am-not-able-to-see-vm-size-family-that-i-want-when-resizing-my-vm"></a>VM'im yeniden boyutlandırılırken istediğiniz VM boyutu ailesi görmeye değilim.

Bir VM çalışırken, bir fiziksel sunucuya dağıtılır. Fiziksel sunuculardan Azure bölgelerindeki ortak fiziksel donanım kümelerde gruplandırılır. Farklı donanım kümelerine taşınacak VM gerektiren bir VM'yi yeniden boyutlandırılırken hangi dağıtım modeli VM dağıtmak için kullanılan bağlı olarak farklılık gösterir.

- Klasik dağıtım modeli, bulut hizmeti dağıtımı dağıtılan VM'ler kaldırıldı ve sanal makineleri başka bir boyutu ailesindeki bir boyutu değiştirmek için imzalanması gerekir.

- Resource Manager dağıtım modelinde dağıtılan VM'ler, tüm sanal makineleri kullanılabilirlik kullanılabilirlik kümesindeki herhangi bir VM boyutunu değiştirmeden önce kümesini durdurmanız gerekir.

## <a name="the-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a>Listelenen VM boyutu kullanılabilirlik kümesinde dağıtırken desteklenmiyor.

Kullanılabilirlik kümesinin kümede desteklenen bir boyutu seçin. Bir kullanılabilirlik ilk dağıtımınızı kullanılabilirlik kümesi olması gerekir ve sahip düşündüğünüz en büyük VM boyutu seçmek için kümesi oluştururken önerilir.

## <a name="can-i-add-an-existing-classic-vm-to-an-availability-set"></a>Mevcut bir Klasik VM'yi bir kullanılabilirlik kümesi ekleyebilir miyim?

Evet. Mevcut bir Klasik VM'yi bir yeni veya var olan kullanılabilirlik kümesine ekleyebilirsiniz. Daha fazla bilgi için bkz: [var olan bir sanal makine bir kullanılabilirlik kümesine ekleme](classic/configure-availability.md#addmachine).


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumları](https://azure.microsoft.com/support/forums/).

Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.
