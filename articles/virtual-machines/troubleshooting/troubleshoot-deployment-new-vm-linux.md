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
ms.openlocfilehash: 9fea914fdf9b025fd5d38219a6bfc81b4a9cc584
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62125625"
---
# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Azure'da yeni bir Linux sanal makine oluşturma Resource Manager dağıtım sorunlarını giderme
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a>En sık karşılaşılan sorunlar
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

Diğer VM dağıtım sorunları ve soruları için bkz. [Azure'da Linux sanal makine dağıtma sorunlarını giderme](troubleshoot-deploy-vm-linux.md).

## <a name="collect-activity-logs"></a>Toplama etkinlik günlükleri
Sorun gidermeye başlamak için sorunla ilişkili hatanın tanımlamak için etkinlik günlüklerini toplayın. Aşağıdaki bağlantılar, izlenecek işlem hakkında ayrıntılı bilgi içerir.

[Dağıtım işlemlerini görüntüleme](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Azure kaynaklarını yönetmek için etkinlik günlüklerini görüntüleme](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**Y:** Genelleştirilmiş bir Linux işletim sistemi olan ve karşıya yüklenen ve genelleştirilmiş ayarıyla yakalanan, ardından olmayacaktır hataları. Benzer şekilde, işletim sistemi Linux özelleştirilmiş ve karşıya yüklendi ve özel ayarlarla yakalanan sonra olmayacaktır hataları.

**Karşıya yükleme hataları:**

**N<sup>1</sup>:** İşletim sistemi Linux genelleştirilmiş olarak karşıya ise VM sağlama aşamasında takılı olduğundan özelleştirilmiş, sağlama bir zaman aşımı hatası alırsınız.

**N<sup>2</sup>:** İşletim sistemi Linux özelleştirilmiş ve genelleştirilmiş olarak yüklenmiş ise, yeni VM özgün bilgisayar adı, kullanıcı adı ve parola ile çalıştığı için sağlama hatası alırsınız.

**Çözüm:**

Hem bu hataları gidermek için mevcut şirket içi, işletim sistemi (genelleştirilmiş/özel) olarak aynı ayarı ile özgün VHD karşıya yükleyin. Çalıştırılacak - genelleştirilmiş olarak yüklemek için unutmayın ilk sağlamasını kaldırma.

**Hataları yakalamaya:**

**N<sup>3</sup>:** İşletim sistemi Linux genelleştirilmiş olarak yakalanır ise özgün VM genelleştirilmiş olarak işaretlenir kullanılabilir olmadığı için özelleştirilmiş, sağlama bir zaman aşımı hatası alırsınız.

**N<sup>4</sup>:** Linux özelleştirilmiş ve genelleştirilmiş olarak yakalanan işletim sistemi ise, yeni VM özgün bilgisayar adı, kullanıcı adı ve parola ile çalıştığı için sağlama hatası alırsınız. İşaretli olduğu için ayrıca orijinal VM kullanılamaz olarak özelleştirilmiş.

**Çözüm:**

Hem bu hataları gidermek için geçerli görüntünün portaldan silin ve [geçerli Vhd'lerden elde](../linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) işletim sistemi (genelleştirilmiş/özel) olarak aynı ayarı ile.

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Sorun: Özel / Galeri / Market görüntüsü; ayırma hatası
Yeni VM istek, istenen VM boyutu destekleyemiyorsa veya isteği gerçekleştirmek için kullanılabilir boş alan yok bir kümeye sabitlenmiş durumlarda bu hata oluşur.

**1. neden:** Küme, istenen VM boyutu destekleyemez.

**1. çözüm:**

* Daha küçük bir VM boyutu isteği yeniden deneyin.
* İstenen VM boyutu değiştirilemiyorsa:
  * Kullanılabilirlik kümesindeki tüm sanal makineler durdurun.
    Tıklayın **kaynak grupları** > *kaynak grubunuzun* > **kaynakları** >  *, kullanılabilirlik kümesi*  >  **Sanal makineler** > *sanal makinenizi* > **Durdur**.
  * Tüm sanal makineleri durdurduktan sonra istenen boyutu yeni bir VM oluşturun.
  * İlk olarak, yeni VM'yi başlatın ve ardından her biri durdurulmuş sanal makineler seçin ve tıklayın **Başlat**.

**2. neden:** Kümenin boş kaynak yok.

**2. çözüm:**

* İsteği daha sonra yeniden deneyin.
* Yeni VM'yi farklı bir kullanılabilirlik kümesinin bir parçası olarak
  * (Aynı bölgede) farklı bir kullanılabilirlik yeni bir VM oluşturun.
  * Yeni VM, aynı sanal ağa ekleyin.

## <a name="next-steps"></a>Sonraki adımlar
Linux VM'yi durdurduğunuzda veya azure'da var olan bir Linux sanal makinesi'yeniden boyutlandırma sırasında sorunlarla karşılaşırsanız bkz [başlatma veya yeniden boyutlandırmayla var olan bir Linux sanal makine azure'da sorun giderme Resource Manager dağıtım sorunlarını](../linux/restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

