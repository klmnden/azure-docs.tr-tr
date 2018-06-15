---
title: include dosyası
description: include dosyası
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/17/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: eedce1ef3a4e7d56cfaf3142a72e370c96f9f575
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34664651"
---
### <a name="virtual-machines"></a>Virtual Machines

|  |  |
|---------|---------|
| [Bir Kaynak Grubundan özel VM kullanımına izin verin](../articles/azure-policy/scripts/allow-custom-vm-image.md) |  Özel görüntülerin onaylı bir kaynak grubundan gelmesini gerektirir. Onaylı kaynak grubunun adını belirtirsiniz. |
| [Depolama Hesapları ve Sanal Makineler için izin verilen SKU'lar](../articles/azure-policy/scripts/allowed-skus-storage.md) | Depolama hesapları ve sanal makinelerin onaylı SKU’lar kullanmasını gerektirir. Onaylı SKU’ları güvence altına almak için yerleşik ilkeleri kullanır. Onaylı bir sanal makine SKU’su dizisi ve onaylı bir depolama hesabı SKU’su dizisi belirtirsiniz. |
| [Onaylı VM görüntüleri](../articles/azure-policy/scripts/allowed-custom-images.md) | Ortamınızda yalnızca onaylı özel görüntülerin dağıtılmasını gerektirir. Onaylanan bir görüntü kimliği dizisi belirtirsiniz. |
| [Uzantı yoksa denetle](../articles/azure-policy/scripts/audit-ext-not-exist.md) | Sanal makine ile bir uzantı dağıtılmadıysa denetler. Dağıtılıp dağıtılmadığını görmek için uzantı yayımcısı ve türünü belirtirsiniz. |
| [İzin verilmeyen VM Uzantıları](../articles/azure-policy/scripts/not-allowed-vm-ext.md) | Belirtilen uzantıların kullanılmasını engeller. Yasaklanan uzantı türlerini içeren bir dizi belirtirsiniz. |