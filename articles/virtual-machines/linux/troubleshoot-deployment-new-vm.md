---
title: Linux VM dağıtımı-RM sorunlarını giderme | Microsoft Docs
description: Azure'da yeni bir Linux sanal makine oluşturduğunuzda, Resource Manager dağıtım sorunlarını giderme
services: virtual-machines-linux, azure-resource-manager
documentationcenter: ''
author: JiangChen79
manager: felixwu
editor: ''
tags: top-support-issue, azure-resource-manager
ms.assetid: 906a9c89-6866-496b-b4a4-f07fb39f990c
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/09/2016
ms.author: cjiang
ms.openlocfilehash: aea5db05843b0175b8ef8b713944e12262e33010
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
ms.locfileid: "27579272"
---
# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Azure'da yeni bir Linux sanal makine oluşturma ile Resource Manager dağıtım sorunlarını giderme
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a>Üst sorunları
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

Diğer VM Dağıtım sorunlar ve sorular için bkz: [azure'da dağıtma Linux sanal makine sorunlarını giderme](troubleshoot-deploy-vm.md).
## <a name="collect-activity-logs"></a>Toplama etkinliklerini günlüğe kaydeder
Sorun giderme başlatmak için sorunu ile ilişkili hata tanımlamak için etkinlik günlüklerini toplayın. Aşağıdaki bağlantıları izleyin işlemi ile ilgili ayrıntılı bilgiler içerir.

[Dağıtım işlemlerini görüntüleme](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Azure kaynaklarınızı yönetmek için etkinlik günlüklerini görüntüle](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**Y:** genelleştirilmiş, Linux işletim sistemi olan ve bunu karşıya ve/veya genelleştirilmiş ayarıyla yakalanan durumunda hataları olmayacak. Benzer şekilde, işletim sistemi ise Linux özelleştirilmiş ve bunu karşıya ve/veya özel ayarlarla yakalanan ardından hataları olmayacak.

**Karşıya yükleme hataları:**

**N<sup>1</sup>:** genelleştirilmiş Linux işletim sistemi ise ve olarak karşıya VM sağlama aşamada takıldı için özelleştirilmiş, sağlama zaman aşımı hatası alırsınız.

**N<sup>2</sup>:** OS Linux özelleştirilmiş ve genelleştirilmiş gibi karşıya varsa, yeni VM özgün bilgisayar adı, kullanıcı adı ve parola ile çalıştığı için sağlama bir hata alırsınız.

**Çözüm:**

Her iki bu hataları gidermek için özgün VHD, mevcut şirket içi, işletim sistemi (genelleştirilmiş/özel) olarak aynı ayarı ile karşıya yükleyin. Genelleştirilmiş olarak karşıya yükleme, çalıştırmak - unutmayın ilk sağlamayı sonlandırın.

**Hata yakalama:**

**N<sup>3</sup>:** genelleştirilmiş Linux işletim sistemi ise ve olarak yakalanan orijinal VM kullanılabilir olmadığından genelleştirilmiş olarak işaretlenen gibi özelleştirilmiş, sağlama zaman aşımı hatası alırsınız.

**N<sup>4</sup>:** Linux özelleştirilmiş ve genelleştirilmiş gibi yakalanan işletim sistemi ise, yeni VM özgün bilgisayar adı, kullanıcı adı ve parola ile çalıştığı için sağlama bir hata alırsınız. İşaretlendiğinden Ayrıca, orijinal VM kullanılamaz olarak özelleştirilmiş.

**Çözüm:**

Her iki bu hataları gidermek için geçerli görüntü Portalı'ndan silmek ve [geçerli VHD'leri yeniden yakalar](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ayarıyla aynı olarak işletim sistemi (genelleştirilmiş/özel) için.

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Sorun: Özel / Galeri / Market görüntüsü; ayırma hatası
Bu hata istenen VM boyutu desteklemiyor veya isteği gerçekleştirmek için kullanılabilir boş alan yok bir kümeye yeni VM isteği sabitlenmiş durumlarda oluşur.

**1. neden:** küme istenen VM boyutu destekleyemez.

**Çözüm 1:**

* Daha küçük bir VM boyutu kullanarak isteği yeniden deneyin.
* İstenen VM boyutu değiştirilemiyorsa:
  * Kullanılabilirlik kümesindeki tüm sanal makineleri durdurun.
    Tıklatın **kaynak grupları** > *kaynak grubunuz* > **kaynakları** > *kullanılabilirlik kümesi* > **sanal makineleri** > *sanal makineniz* > **durdurmak**.
  * Tüm sanal makineleri durdurduktan sonra istenen boyutta yeni bir VM oluşturun.
  * İlk olarak, yeni VM Başlat durdurulmuş VM'lerin her birinde seçin ve tıklatın **Başlat**.

**Neden 2:** küme kaynakları serbest bırakmak yok.

**Çözüm 2:**

* İstek daha sonra yeniden deneyin.
* Yeni VM'yi farklı bir kullanılabilirlik kümesinin bir parçası olarak
  * Farklı bir kullanılabilirlik (aynı bölgede) kümesinde yeni bir VM oluşturun.
  * Yeni VM aynı sanal ağa ekleyin.

## <a name="next-steps"></a>Sonraki adımlar
Durdurulmuş bir Linux VM başlattığınızda veya varolan bir Linux VM azure'da yeniden boyutlandırma sorunlarıyla karşılaşırsanız bkz [başlatma veya varolan bir Linux sanal makinesini Azure yeniden boyutlandırma ile ilgili sorunları giderme Resource Manager dağıtım sorunlarını](restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

