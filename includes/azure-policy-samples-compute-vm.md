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
ms.openlocfilehash: b6587a3928c2ddf0d00c90e07643525314690e42
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66155493"
---
### <a name="virtual-machines"></a>Virtual Machines

|  |  |
|---------|---------|
| [Bir Kaynak Grubundan özel VM kullanımına izin verin](../articles/governance/policy/samples/allow-custom-vm-image.md) |  Özel görüntülerin onaylı bir kaynak grubundan gelmesini gerektirir. Onaylı kaynak grubunun adını belirtirsiniz. |
| [Depolama Hesapları ve Sanal Makineler için izin verilen SKU'lar](../articles/governance/policy/samples/allowed-skus-storage.md) | Depolama hesapları ve sanal makinelerin onaylı SKU’lar kullanmasını gerektirir. Onaylı SKU’ları güvence altına almak için yerleşik ilkeleri kullanır. Onaylı bir sanal makine SKU’su dizisi ve onaylı bir depolama hesabı SKU’su dizisi belirtirsiniz. |
| [Onaylı VM görüntüleri](../articles/governance/policy/samples/allowed-custom-images.md) | Ortamınızda yalnızca onaylı özel görüntülerin dağıtılmasını gerektirir. Onaylanan bir görüntü kimliği dizisi belirtirsiniz. |
| [Uzantı yoksa denetle](../articles/governance/policy/samples/audit-extension-not-exist.md) | Sanal makine ile bir uzantı dağıtılmadıysa denetler. Dağıtılıp dağıtılmadığını görmek için uzantı yayımcısı ve türünü belirtirsiniz. |
| [İzin verilmeyen VM Uzantıları](../articles/governance/policy/samples/not-allowed-vm-extension.md) | Belirtilen uzantıların kullanılmasını engeller. Yasaklanan uzantı türlerini içeren bir dizi belirtirsiniz. |