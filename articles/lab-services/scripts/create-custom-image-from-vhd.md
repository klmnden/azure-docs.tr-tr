---
title: 'PowerShell Betiği: Azure Lab Services bir VHD dosyasından bir özel görüntü oluşturma | Microsoft Docs'
description: Bu PowerShell Betiği, Azure Lab Services VHD dosyasından özel bir görüntü oluşturur.
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/11/2018
ms.author: spelluru
ms.openlocfilehash: 2d0cc4012adf2c17b2f7a2e769f2d666b158a8c8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66160556"
---
# <a name="use-powershell-to-create-a-custom-image-from-a-vhd-file-in-azure-lab-services"></a>Azure Lab Services VHD dosyasından özel bir görüntü oluşturmak için PowerShell kullanma

Bu örnek PowerShell Betiği, Azure Lab Services bir VHD dosyasından özel bir görüntü oluşturur.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

## <a name="prerequisites"></a>Önkoşullar
* **Bir laboratuvar**. Komut dosyası var olan bir laboratuvar olmasını gerektirir. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/devtest-lab/create-custom-image-from-vhd/create-custom-image-from-vhd.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [Get-AzResource](/powershell/module/az.resources/get-azresource) | Kaynakları alır. |
| [Get-AzStorageAccountKey](/powershell/module/az.storage/get-azstorageaccountkey) | Azure Depolama hesabının erişim anahtarlarını alır. |
| [Yeni AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) | Bir Azure dağıtımındaki bir kaynak grubuna ekler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Lab Services PowerShell Betiği örnekleri, içinde bulunabilir [Azure Lab Services PowerShell örnekleri](../samples-powershell.md).
