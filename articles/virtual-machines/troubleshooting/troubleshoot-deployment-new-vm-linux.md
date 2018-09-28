---
title: Linux VM dağıtımı sorunlarını giderme | Microsoft Docs
description: Azure'da yeni bir Linux sanal makine oluşturduğunuzda, Resource Manager dağıtım sorunlarını giderme
services: virtual-machines-linux, azure-resource-manager
documentationcenter: ''
author: JiangChen79
manager: jeconnoc
editor: ''
tags: top-support-issue, azure-resource-manager
ms.assetid: 906a9c89-6866-496b-b4a4-f07fb39f990c
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/09/2016
ms.author: cjiang
ms.openlocfilehash: 08009ca7f9faaa75e593670c22cf864c12236e8b
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47414708"
---
# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Azure'da yeni bir Linux sanal makine oluşturma Resource Manager dağıtım sorunlarını giderme
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a>En sık karşılaşılan sorunlar
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

Diğer VM Dağıtım sorunlar ve sorular için bkz. [azure'da dağıtma Linux sanal makine sorunlarını giderme](troubleshoot-deploy-vm-linux.md).

## <a name="collect-activity-logs"></a>Toplama etkinlik günlükleri
Sorun gidermeye başlamak için sorunla ilişkili hatanın tanımlamak için etkinlik günlüklerini toplayın. Aşağıdaki bağlantılar, izlenecek işlem hakkında ayrıntılı bilgi içerir.

[Dağıtım işlemlerini görüntüleme](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Azure kaynaklarını yönetmek için etkinlik günlüklerini görüntüleme](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**Y:** genelleştirilmiş, Linux işletim sistemi olan ve karşıya yüklendi ve genelleştirilmiş ayarıyla yakalanan durumunda hataları olmaz. Benzer şekilde, işletim sistemi Linux özelleştirilmiş ve karşıya yüklendi ve özel ayarlarla yakalanan sonra olmayacaktır hataları.

**Karşıya yükleme hataları:**

**N<sup>1</sup>:** işletim Sisteminin Linux genelleştirilmiş olarak karşıya ise VM sağlama aşamasında takılı olduğundan özelleştirilmiş, sağlama bir zaman aşımı hatası alırsınız.

**N<sup>2</sup>:** işletim Sisteminin Linux özelleştirilmiş ve genelleştirilmiş olarak yüklenmiş ise, yeni VM özgün bilgisayar adı, kullanıcı adı ve parola ile çalıştığı için sağlama hatası alırsınız.

**Çözüm:**

Hem bu hataları gidermek için özgün VHD, mevcut şirket içi, işletim sistemi (genelleştirilmiş/özel) olarak aynı ayarı ile karşıya yükleyin. Çalıştırılacak - genelleştirilmiş olarak yüklemek için unutmayın ilk sağlamasını kaldırma.

**Hataları yakalamaya:**

**N<sup>3</sup>:** genelleştirilmiş Linux işletim sistemi ve olarak yakalanır özgün VM genelleştirilmiş olarak işaretlenir kullanılabilir olmadığı için özelleştirilmiş, sağlama bir zaman aşımı hatası alırsınız.

**N<sup>4</sup>:** Linux özelleştirilmiş ve genelleştirilmiş olarak yakalanan işletim sistemi ise, yeni VM özgün bilgisayar adı, kullanıcı adı ve parola ile çalıştığı için sağlama hatası alırsınız. İşaretli olduğu için ayrıca orijinal VM kullanılamaz olarak özelleştirilmiş.

**Çözüm:**

Hem bu hataları gidermek için geçerli görüntünün portaldan silin ve [geçerli Vhd'lerden elde](../linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) işletim sistemi (genelleştirilmiş/özel) olarak aynı ayarı ile.

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Sorun: Özel / Galeri / Market görüntüsü; ayırma hatası
Yeni VM istek, istenen VM boyutu destekleyemiyorsa veya isteği gerçekleştirmek için kullanılabilir boş alan yok bir kümeye sabitlenmiş durumlarda bu hata oluşur.

**1. neden:** küme istenen VM boyutu destekleyemez.

**1. çözüm:**

* Daha küçük bir VM boyutu isteği yeniden deneyin.
* İstenen VM boyutu değiştirilemiyorsa:
  * Kullanılabilirlik kümesindeki tüm sanal makineler durdurun.
    Tıklayın **kaynak grupları** > *kaynak grubunuzun* > **kaynakları** > *, kullanılabilirlik kümesi*  >  **Sanal makineler** > *sanal makinenizi* > **Durdur**.
  * Tüm sanal makineleri durdurduktan sonra istenen boyutu yeni bir VM oluşturun.
  * İlk olarak, yeni VM'yi başlatın ve ardından her biri durdurulmuş sanal makineler seçin ve tıklayın **Başlat**.

**2. neden:** küme ücretsiz kaynak yok.

**2. çözüm:**

* İsteği daha sonra yeniden deneyin.
* Yeni VM'yi farklı bir kullanılabilirlik kümesinin bir parçası olarak
  * (Aynı bölgede) farklı bir kullanılabilirlik yeni bir VM oluşturun.
  * Yeni VM, aynı sanal ağa ekleyin.

## <a name="next-steps"></a>Sonraki adımlar
Linux VM'yi durdurduğunuzda veya azure'da var olan bir Linux sanal makinesi'yeniden boyutlandırma sırasında sorunlarla karşılaşırsanız bkz [başlatma veya yeniden boyutlandırmayla var olan bir Linux sanal makine azure'da sorun giderme Resource Manager dağıtım sorunlarını](../linux/restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

