---
title: 'PowerShell Betiği: Azure Laboratuvar Hizmetleri VHD dosyasında özel bir görüntü oluşturun | Microsoft Docs'
description: Bu PowerShell Betiği Azure Laboratuvar Hizmetleri VHD dosyasında özel bir görüntü oluşturur.
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
ms.openlocfilehash: 19b7c3c6018ec56b056761c336bc56c8b63b47a2
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34636411"
---
# <a name="use-powershell-to-create-a-custom-image-from-a-vhd-file-in-azure-lab-services"></a>Azure Laboratuvar Hizmetleri VHD dosyasında özel bir görüntü oluşturmak için PowerShell kullanma

Bu örnek PowerShell komut dosyası Azure Laboratuvar Hizmetleri VHD dosyasında özel bir görüntü oluşturur

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Önkoşullar
* **Bir laboratuvar**. Komut dosyası var olan bir laboratuvar olmasını gerektirir. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/devtest-lab/create-custom-image-from-vhd/create-custom-image-from-vhd.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Kaynakları alır. |
| [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) | Azure Depolama hesabının erişim anahtarlarını alır. |
| [Yeni-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) | Bir Azure dağıtımı bir kaynak grubuna ekler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Laboratuvar Hizmetleri PowerShell komut dosyası örnekleri bulunabilir [Azure Laboratuvar Hizmetleri PowerShell örnekleri](../samples-powershell.md).