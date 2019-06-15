---
title: "PowerShell Betiği: Bir Azure bildirim hub'ı oluşturma | Microsoft Docs"
description: Bu PowerShell Betiği bir Azure bildirim hub'ı oluşturur.
services: notification-hubs
author: dimazaid
manager: kpiteira
editor: spelluru
ms.service: notification-hubs
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 93c5118962fed1bfda4d32afba942a6600664e5d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66172150"
---
# <a name="use-powershell-to-create-an-azure-notification-hub"></a>Bir Azure bildirim hub'ı oluşturmak için PowerShell kullanma

Bu örnek PowerShell Betiği, bir örnek Azure bildirim hub'ı oluşturur. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Önkoşullar
* **Azure aboneliği** - Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/notification-hubs/create-notification-hub/create-notification-hub.ps1 "Create a notification hub")]


## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Örnek betiği çalıştırdıktan sonra aşağıdaki komutu kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için kullanabilirsiniz:

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzNotificationHubsNamespace](/powershell/module/az.notificationhubs/new-aznotificationhubsnamespace) | Bildirim hub'ı için bir ad alanı oluşturur. |
| [New-AzNotificationHub](/powershell/module/az.notificationhubs/new-aznotificationhub) | Bildirim hub'ı oluşturur. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).
