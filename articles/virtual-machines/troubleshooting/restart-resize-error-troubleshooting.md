---
title: Azure'da VM başlatma veya yeniden boyutlandırmayla sorunlar | Microsoft Docs
description: Başlatma veya yeniden boyutlandırmayla mevcut bir sanal makine Azure Resource Manager dağıtım sorunlarını giderme
services: virtual-machines
documentationcenter: ''
author: Deland-Han
manager: felixwu
editor: ''
tags: top-support-issue
ms.assetid: 0756b52d-4f5a-4503-ae45-c00a6a2edcdf
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f510a111a6c8846b300c09f368a3a2a05b2bb7ad
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60306988"
---
# <a name="troubleshoot-deployment-issues-with-restarting-or-resizing-an-existing-windows-vm-in-azure"></a>Başlatma veya yeniden boyutlandırmayla azure'da var olan bir Windows sanal makinesi dağıtım sorunlarını giderme
Durdurulmuş bir Azure sanal makine (VM) Başlat veya mevcut bir Azure VM'yi yeniden boyutlandırma çalıştığınızda, karşılaştığınız ortak bir ayırma hatası hatadır. Küme veya bölge kullanılabilir kaynak yok veya istenen VM boyutu desteklemez, bu hata oluşur.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="collect-activity-logs"></a>Toplama etkinlik günlükleri
Sorun gidermeye başlamak için sorunla ilişkili hatanın tanımlamak için etkinlik günlüklerini toplayın. Aşağıdaki bağlantılar, işlem hakkında ayrıntılı bilgi içerir:

[Dağıtım işlemlerini görüntüleme](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Azure kaynaklarını yönetmek için etkinlik günlüklerini görüntüleme](../../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>Sorun: Durdurulmuş bir sanal makine başlatma hatası
Bir VM'yi durdurduğunuzda, ancak bir ayırma hatası alma deneyin.

### <a name="cause"></a>Nedeni
VM'yi durdurduğunuzda isteğine bulut hizmetini barındıran konumundaki özgün küme denenmesi gerekir. Ancak, Küme isteği gerçekleştirmek kullanılabilir boş alan yok.

### <a name="resolution"></a>Çözüm
* Kullanılabilirlik kümesindeki tüm sanal makineleri durdurmak ve ardından her bir VM'yi yeniden başlatın.
  
  1. Tıklayın **kaynak grupları** > *kaynak grubunuzun* > **kaynakları** > *, kullanılabilirlik kümesi*  >  **Sanal makineler** > *sanal makinenizi* > **Durdur**.
  2. Tüm sanal makineleri durdurduktan sonra her biri durdurulmuş sanal makineler seçin ve Başlat'a tıklayın.
* Yeniden başlatma isteği daha sonra yeniden deneyin.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Sorun: Varolan bir VM'yi yeniden boyutlandırma hatası
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
Azure'da yeni bir Windows sanal makine oluştururken bir sorunla karşılaşırsanız bkz [Azure'da yeni bir Windows sanal makine oluştururken dağıtım sorunlarını giderme](../windows/troubleshoot-deployment-new-vm.md).

