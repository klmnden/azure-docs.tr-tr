---
title: include dosyası
description: include dosyası
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/18/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: eadff6f61bd9e5979ae835230de53562ff8542d1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47004008"
---
### <a name="virtual-machines"></a>Virtual Machines

|  |  |
|---------|---------|
| [Bir Kaynak Grubundan özel VM kullanımına izin verin](../articles/governance/policy/samples/allow-custom-vm-image.md) |  Özel görüntülerin onaylı bir kaynak grubundan gelmesini gerektirir. Onaylı kaynak grubunun adını belirtirsiniz. |
| [Depolama Hesapları ve Sanal Makineler için izin verilen SKU'lar](../articles/governance/policy/samples/allowed-skus-storage.md) | Depolama hesapları ve sanal makinelerin onaylı SKU’lar kullanmasını gerektirir. Onaylı SKU’ları güvence altına almak için yerleşik ilkeleri kullanır. Onaylı bir sanal makine SKU’su dizisi ve onaylı bir depolama hesabı SKU’su dizisi belirtirsiniz. |
| [Onaylı VM görüntüleri](../articles/governance/policy/samples/allowed-custom-images.md) | Ortamınızda yalnızca onaylı özel görüntülerin dağıtılmasını gerektirir. Onaylanan bir görüntü kimliği dizisi belirtirsiniz. |
| [Uzantı yoksa denetle](../articles/governance/policy/samples/audit-ext-not-exist.md) | Sanal makine ile bir uzantı dağıtılmadıysa denetler. Dağıtılıp dağıtılmadığını görmek için uzantı yayımcısı ve türünü belirtirsiniz. |
| [İzin verilmeyen VM Uzantıları](../articles/governance/policy/samples/not-allowed-vm-ext.md) | Belirtilen uzantıların kullanılmasını engeller. Yasaklanan uzantı türlerini içeren bir dizi belirtirsiniz. |