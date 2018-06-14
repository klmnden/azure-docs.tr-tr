---
title: Azure'da dağıtma Linux sanal makine sorunlarını giderme | Microsoft Docs
description: Azurethe Resource Manager dağıtım modelinde dağıtma Linux sanal makine sorunlarını giderin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: 18a8c8c543e640af6d66d6075ca3a6c6799d4d64
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
ms.locfileid: "34072564"
---
# <a name="troubleshoot-deploying-linux-virtual-machine-issues-in-azure"></a>Azure'da dağıtma Linux sanal makine sorunlarını giderme

Azure'da sanal makine (VM) dağıtım sorunlarını gidermek için inceleyin [üst sorunları](#top-issues) yaygın hatalar ve çözümleri için.

Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumları](https://azure.microsoft.com/support/forums/). Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.

## <a name="top-issues"></a>Üst sorunları
[!INCLUDE [virtual-machines-linux-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

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

## <a name="how-do-i-activate-my-monthly-credit-for-visual-studio-enterprise-bizspark"></a>Visual studio Enterprise (BizSpark) için aylık my kredi nasıl etkinleştirilsin mi

Aylık kredinizin etkinleştirmek için bu bkz [makale](https://azure.microsoft.com/offers/ms-azr-0064p/).

## <a name="why-can-i-not-install-the-gpu-driver-for-an-ubuntu-nv-vm"></a>Neden GPU sürücüsünün bir Ubuntu NV VM için yükleyebilir miyim değil mi?

Linux GPU desteği şu anda yalnızca Azure NC Ubuntu Server 16.04 LTS çalıştıran Vm'lerinde olarak kullanılabilir. Daha fazla bilgi için bkz: [GPU sürücüleri Linux çalıştıran N-serisi VM'ler için ayarlanmış](n-series-driver-setup.md).

## <a name="my-drivers-are-missing-for-my-linux-n-series-vm"></a>My sürücüleri için Linux N-serisi VM'ime eksik

Linux tabanlı VM'ler için sürücülerin bulunduğu [burada](n-series-driver-setup.md). 

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a>GPU örneği N-serisi VM'im içinde bulunamıyor

Windows Server 2016 veya Windows Server 2012 R2 çalıştıran Azure N-serisi VM'ler GPU yeteneklerinden yararlanabilmek için NVIDIA grafik sürücüleri dağıtımdan sonra her VM yüklemeniz gerekir. Sürücü Kurulum bilgileri yüklenebilir [Windows Vm'lerini](../windows/n-series-driver-setup.md) ve [Linux VM'ler](n-series-driver-setup.md).

## <a name="is-n-series-vms-available-in-my-region"></a>N-serisi VM'ler bulunduğum bölgede var mı?

Kullanılabilirlik denetleyebilirsiniz [bölge tablosu tarafından kullanılabilen ürünler](https://azure.microsoft.com/regions/services)ve fiyatlandırma [burada](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).

## <a name="i-am-not-able-to-see-vm-size-family-that-i-want-when-resizing-my-vm"></a>VM'im yeniden boyutlandırılırken istediğiniz VM boyutu ailesi görmeye değilim.

Bir VM çalışırken, bir fiziksel sunucuya dağıtılır. Fiziksel sunuculardan Azure bölgelerindeki ortak fiziksel donanım kümelerde gruplandırılır. Farklı donanım kümelerine taşınacak VM gerektiren bir VM'yi yeniden boyutlandırılırken hangi dağıtım modeli VM dağıtmak için kullanılan bağlı olarak farklılık gösterir.

- Klasik dağıtım modeli, bulut hizmeti dağıtımı dağıtılan VM'ler kaldırıldı ve sanal makineleri başka bir boyutu ailesindeki bir boyutu değiştirmek için imzalanması gerekir.

- Resource Manager dağıtım modelinde dağıtılan VM'ler, tüm sanal makineleri kullanılabilirlik kullanılabilirlik kümesindeki herhangi bir VM boyutunu değiştirmeden önce kümesini durdurmanız gerekir.

## <a name="the-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a>Listelenen VM boyutu kullanılabilirlik kümesinde dağıtırken desteklenmiyor.

Kullanılabilirlik kümesinin kümede desteklenen bir boyutu seçin. Bir kullanılabilirlik ilk dağıtımınızı kullanılabilirlik kümesi olması gerekir ve sahip düşündüğünüz en büyük VM boyutu seçmek için kümesi oluştururken önerilir.

## <a name="what-linux-distributionsversions-are-supported-on-azure"></a>Hangi Linux dağıtımları/sürümleri, Azure üzerinde desteklenir?

Linux listesini bulabilirsiniz [Azure-Endorsed dağıtımları](endorsed-distros.md).

## <a name="can-i-add-an-existing-classic-vm-to-an-availability-set"></a>Mevcut bir Klasik VM'yi bir kullanılabilirlik kümesi ekleyebilir miyim?

Evet. Mevcut bir Klasik VM'yi bir yeni veya var olan kullanılabilirlik kümesine ekleyebilirsiniz. Daha fazla bilgi için bkz: [var olan bir sanal makine bir kullanılabilirlik kümesine ekleme](../windows/classic/configure-availability-classic.md#addmachine).


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede herhangi bir noktada daha fazla yardıma gereksinim duyarsanız, üzerinde Azure uzmanlar başvurabilirsiniz [MSDN Azure ve yığın taşması forumları](https://azure.microsoft.com/support/forums/).

Alternatif olarak, Azure destek olay dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.
