---
title: Azure PowerShell örnek komut dosyası - yönetilen kaynak grubunu Al ve sanal makineleri yeniden boyutlandırmak | Microsoft Docs
description: Azure PowerShell komut dosyası örneği - yönetilen kaynak grubunu Al ve sanal makineleri yeniden boyutlandırma
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: poweshell
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/27/2017
ms.author: tomfitz
ms.openlocfilehash: f549f26cb3f9fdb2d805d2efb2c0e1706abe3edb
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2017
ms.locfileid: "23940945"
---
# <a name="get-resources-in-a-managed-resource-group-and-resize-vms-with-powershell"></a>Yönetilen kaynak grubunda kaynaklar almak ve VM'ler PowerShell ile yeniden boyutlandırın

Bu komut dosyası yönetilen kaynak grubundan kaynakları alır ve bu kaynak grubundaki sanal makineleri yeniden boyutlandırır.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/managed-applications/get-application/get-application.ps1 "Get application")]


## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, yönetilen uygulamayı dağıtmak için aşağıdaki komutları kullanır. Komut özgü belgelere Tablo bağlantıları her komut.

| Komut | Notlar |
|---|---|
| [Get-AzureRmManagedApplication](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermmanagedapplication) | Yönetilen uygulamaların listesi. Sonuçları odaklanmak için kaynak grubu adı belirtin. |
| [Get-AzureRmResource](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermresource) | Kaynakları listeler. Bir kaynak grubu ve kaynak türü sonucu odaklanmaya sağlayın. |
| [Update-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvm) | Bir sanal makinenin boyutu güncelleştirin. |


## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamaların giriş için bkz: [Azure yönetilen uygulama genel bakış](../overview.md).
* PowerShell hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](https://docs.microsoft.com/powershell/azure/get-started-azureps).
