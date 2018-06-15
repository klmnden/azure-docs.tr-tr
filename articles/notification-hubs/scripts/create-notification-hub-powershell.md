---
title: 'PowerShell Betiği: Azure notification hub oluştur | Microsoft Docs'
description: Azure bildirim hub'ı bu PowerShell Betiği oluşturur.
services: data-factory
author: dimazaid
manager: kpiteira
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 747d743a0573bd959b4d3c7100be8ae9451c5ed5
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33790982"
---
# <a name="use-powershell-to-create-an-azure-notification-hub"></a>Azure bildirim hub'ı oluşturmak için PowerShell kullanın

Bu örnek PowerShell komut dosyasını bir örnek Azure bildirim hub'ı oluşturur. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Önkoşullar
* **Azure aboneliği** - oluşturmak bir Azure aboneliğiniz yoksa bir [ücretsiz bir hesap](https://azure.microsoft.com/free/) başlamadan önce.

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/notification-hubs/create-notification-hub/create-notification-hub.ps1 "Create a notification hub")]


## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Örnek betiği çalıştırma sonra aşağıdaki komutu kaynak grubu ve onunla ilişkili tüm kaynakları kaldırmak için kullanabilirsiniz:

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [AzureRmNotificationHubsNamespace yeni](/powershell/module//azurerm.notificationhubs/new-azurermnotificationhubsnamespace) | Bildirim hub'ın bir ad oluşturur. |
| [AzureRmNotificationHub yeni](/powershell/module//azurerm.notificationhubs/new-azurermnotificationhubsnamespace) | Bildirim hub'ı oluşturur. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).
