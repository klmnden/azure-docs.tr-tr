---
title: Azure'da VM başlatma veya yeniden boyutlandırmayla sorunlar | Microsoft Docs
description: Başlatma veya yeniden boyutlandırmayla varolan bir Windows sanal makinesini Azure Resource Manager dağıtım sorunlarını giderme
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: Deland-Han
manager: felixwu
editor: ''
tags: top-support-issue
ms.assetid: 0756b52d-4f5a-4503-ae45-c00a6a2edcdf
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.workload: required
ms.date: 06/15/2018
ms.author: delhan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f4e0c77c03856b4851ee5fe49bd6ae54d47f6c31
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35972088"
---
# <a name="troubleshoot-deployment-issues-with-restarting-or-resizing-an-existing-windows-vm-in-azure"></a>Başlatma veya yeniden boyutlandırmayla azure'da var olan bir Windows sanal makinesi dağıtım sorunlarını giderme
Durdurulmuş bir Azure sanal makine (VM) Başlat veya mevcut bir Azure VM'yi yeniden boyutlandırma çalıştığınızda, karşılaştığınız ortak bir ayırma hatası hatadır. Küme veya bölge kullanılabilir kaynak yok veya istenen VM boyutu desteklemez, bu hata oluşur.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="collect-activity-logs"></a>Toplama etkinlik günlükleri
Sorun gidermeye başlamak için sorunla ilişkili hatanın tanımlamak için etkinlik günlüklerini toplayın. Aşağıdaki bağlantılar, işlem hakkında ayrıntılı bilgi içerir:

[Dağıtım işlemlerini görüntüleme](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Azure kaynaklarını yönetmek için etkinlik günlüklerini görüntüleme](../../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>Sorun: bir VM'yi durdurduğunuzda başlatılırken hata
Bir VM'yi durdurduğunuzda, ancak bir ayırma hatası alma deneyin.

### <a name="cause"></a>Nedeni
VM'yi durdurduğunuzda isteğine bulut hizmetini barındıran konumundaki özgün küme denenmesi gerekir. Ancak, Küme isteği gerçekleştirmek kullanılabilir boş alan yok.

### <a name="resolution"></a>Çözüm
* Kullanılabilirlik kümesindeki tüm sanal makineleri durdurmak ve ardından her bir VM'yi yeniden başlatın.
  
  1. Tıklayın **kaynak grupları** > *kaynak grubunuzun* > **kaynakları** > *, kullanılabilirlik kümesi*  >  **Sanal makineler** > *sanal makinenizi* > **Durdur**.
  2. Tüm sanal makineleri durdurduktan sonra her biri durdurulmuş sanal makineler seçin ve Başlat'a tıklayın.
* Yeniden başlatma isteği daha sonra yeniden deneyin.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Sorun: varolan bir VM'yi yeniden boyutlandırma sırasında hata oluştu
Varolan bir VM'yi yeniden boyutlandırma, ancak bir ayırma hatası alma deneyin.

### <a name="cause"></a>Nedeni
Bulut hizmetini barındıran konumundaki özgün küme denenmesi VM yeniden boyutlandırmak için bir istek aldı. Ancak, küme, istenen VM boyutu desteklemez.

### <a name="resolution"></a>Çözüm
* Daha küçük bir VM boyutu isteği yeniden deneyin.
* İstenen VM boyutu değiştirilemiyorsa:
  
  1. Kullanılabilirlik kümesindeki tüm sanal makineler durdurun.
     
     * Tıklayın **kaynak grupları** > *kaynak grubunuzun* > **kaynakları** > *, kullanılabilirlik kümesi*  >  **Sanal makineler** > *sanal makinenizi* > **Durdur**.
  2. Tüm sanal makineleri durdurduktan sonra istenen VM daha büyük bir boyuta yeniden boyutlandırın.
  3. Yeniden boyutlandırılan VM'yi seçin ve tıklayın **Başlat**, ve her biri durdurulmuş sanal makineler başlatın.

## <a name="next-steps"></a>Sonraki adımlar
Azure'da yeni bir Windows sanal makine oluştururken bir sorunla karşılaşırsanız bkz [Azure'da yeni bir Windows sanal makine oluştururken dağıtım sorunlarını giderme](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

