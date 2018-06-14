---
title: Azure Windows VM dağıtımı sorunlarını giderme | Microsoft Docs
description: Azure'da yeni bir Windows sanal makine oluşturduğunuzda, Resource Manager dağıtım sorunlarını giderme
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: JiangChen79
manager: willchen
editor: ''
tags: top-support-issue, azure-resource-manager
ms.assetid: afc6c1a4-2769-41f6-bbf9-76f9f23bcdf4
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: cjiang
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fff29f6cfed4989386ca5bbd12184dce525add76
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
ms.locfileid: "27580380"
---
# <a name="troubleshoot-deployment-issues-when-creating-a-new-windows-vm-in-azure"></a>Azure'da yeni bir Windows VM oluştururken, dağıtım sorunlarını giderme
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a>Üst sorunları
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

Diğer VM Dağıtım sorunlar ve sorular için bkz: [azure'da dağıtma Windows sanal makine sorunlarını giderme](troubleshoot-deploy-vm.md).

## <a name="collect-activity-logs"></a>Toplama etkinliklerini günlüğe kaydeder
Sorun giderme başlatmak için sorunu ile ilişkili hata tanımlamak için etkinlik günlüklerini toplayın. Aşağıdaki bağlantıları izleyin işlemi ile ilgili ayrıntılı bilgiler içerir.

[Dağıtım işlemlerini görüntüleme](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Azure kaynaklarınızı yönetmek için etkinlik günlüklerini görüntüle](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**Y:** genelleştirilmiş, Windows işletim sistemi olan ve bunu karşıya ve/veya genelleştirilmiş ayarıyla yakalanan durumunda hataları olmayacak. Benzer şekilde, işletim sistemi ise Windows özelleştirilmiş ve bunu karşıya ve/veya özel ayarlarla yakalanan ardından hataları olmayacak.

**Karşıya yükleme hataları:**

**N<sup>1</sup>:** genelleştirilmiş Windows işletim sistemi ise ve olarak karşıya özelleştirilmiş, OOBE ekranında takılmış VM ile bir sağlama zaman aşımı hatası alırsınız.

**N<sup>2</sup>:** işletim sistemi Windows özelleştirilmiş ve genelleştirilmiş gibi karşıya varsa, yeni VM özgün bilgisayar adı, kullanıcı adı ve parola ile çalıştığı için OOBE ekranında takılmış VM ile sağlama bir hata alırsınız.

**Çözümleme**

Her iki bu hataları gidermek için [özgün VHD yüklemek için Add-AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx), mevcut şirket içi, işletim sistemi (genelleştirilmiş/özel) olarak aynı ayarı. Genelleştirilmiş gibi karşıya yüklemek için öncelikle, sysprep çalıştırılamıyor unutmayın.

**Hata yakalama:**

**N<sup>3</sup>:** genelleştirilmiş Windows işletim sistemi ise ve olarak yakalanan orijinal VM kullanılabilir olmadığından genelleştirilmiş olarak işaretlenen gibi özelleştirilmiş, sağlama zaman aşımı hatası alırsınız.

**N<sup>4</sup>:** Windows özelleştirilmiş ve genelleştirilmiş gibi yakalanan işletim sistemi ise, yeni VM özgün bilgisayar adı, kullanıcı adı ve parola ile çalıştığı için sağlama bir hata alırsınız. İşaretlendiğinden Ayrıca, orijinal VM kullanılamaz olarak özelleştirilmiş.

**Çözümleme**

Her iki bu hataları gidermek için geçerli görüntü Portalı'ndan silmek ve [geçerli VHD'leri yeniden yakalar](create-vm-specialized.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ayarıyla aynı olarak işletim sistemi (genelleştirilmiş/özel) için.

## <a name="issue-customgallerymarketplace-image-allocation-failure"></a>Sorun: Galeri/özel/Market görüntüsü; ayırma hatası
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
Durdurulmuş bir Windows VM başlattığınızda veya varolan bir Windows VM Azure ile yeniden boyutlandırın sorunlarıyla karşılaşırsanız bkz [başlatma veya mevcut bir Windows sanal makine azure'da yeniden boyutlandırma ile ilgili sorunları giderme Resource Manager dağıtım sorunlarını](restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

