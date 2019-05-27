---
title: Azure PowerShell Betik Örneği - Bir web uygulamasını yedekleme | Microsoft Docs
description: Azure PowerShell Betik Örneği - Bir web uygulamasını yedekleme
services: app-service\web
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: fc755f82-ca3e-4532-b251-690b699324d6
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 10/30/2017
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: 43a99835776563f469efa3e0b14992203b34915e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66136634"
---
# <a name="back-up-a-web-app-using-powershell"></a>PowerShell kullanarak bir web uygulamasını yedekleme

Bu örnek betik, App Service’te ilgili kaynaklarıyla birlikte bir web uygulaması oluşturur ve sonra bu web uygulaması için tek seferlik bir yedekleme oluşturur. 

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview) bulunan yönergeleri kullanarak Azure PowerShell’i yükleyin ve ardından Azure ile bağlantı oluşturmak için `Connect-AzAccount` komutunu çalıştırın. 

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/backup-onetime/backup-onetime.ps1?highlight=1-5 "Back up a web app")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Betik örneği çalıştırıldıktan sonra, kaynak grubunu, web uygulamasını ve ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```powershell
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Yeni AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount) | Bir Depolama hesabı oluşturur. |
| [New-AzStorageContainer](/powershell/module/az.storage/new-AzStoragecontainer) | Bir Azure depolama kapsayıcısı oluşturur. |
| [Yeni AzStorageContainerSASToken](/powershell/module/az.storage/new-AzStoragecontainersastoken) | Azure depolama kapsayıcısı için bir SAS belirteci oluşturur.  |
| [Yeni AzAppServicePlan](/powershell/module/az.websites/new-azappserviceplan) | App Service planı oluşturur. |
| [New-AzWebApp](/powershell/module/az.websites/new-azwebapp) | Bir web uygulaması oluşturur. |
| [Yeni AzWebAppBackup](/powershell/module/az.websites/new-azwebappbackup) | Bir web uygulaması için yedek oluşturur. |
| [Get-AzWebAppBackupList](/powershell/module/az.websites/get-azwebappbackuplist) | Bir web uygulamasının yedekleme listesini alır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure App Service Web Apps için ek Azure PowerShell örneklerini [Azure PowerShell örnekleri](../samples-powershell.md) bölümünde bulabilirsiniz.
