---
title: Azure PowerShell betik örneği - bir uygulamayı bir SQL veritabanı'na bağlanma | Microsoft Docs
description: Azure PowerShell betik örneği - bir App Service uygulaması bir SQL veritabanı'na bağlanma
services: app-service\web
documentationcenter: ''
author: syntaxc4
manager: erikre
editor: ''
tags: azure-service-management
ms.assetid: 055440a9-fff1-49b2-b964-9c95b364e533
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: fc0046f16222fe20a7b11901690acccaae382a6c
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2018
ms.locfileid: "53650129"
---
# <a name="connect-an-app-service-app-to-a-sql-database"></a>Bir App Service uygulaması bir SQL veritabanı'na bağlanma

Bu senaryoda, bir Azure SQL veritabanı ve App Service uygulaması oluşturmayı öğreneceksiniz. Daha sonra uygulama ayarlarını kullanarak uygulamaya SQL veritabanına bağlayacaksınız.

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview) bulunan yönergeleri kullanarak Azure PowerShell’i yükleyin ve ardından Azure ile bağlantı oluşturmak için `Connect-AzureRmAccount` komutunu çalıştırın.

## <a name="sample-script"></a>Örnek betik

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/connect-to-sql/connect-to-sql.ps1?highlight=13 "Connect an app to a SQL database")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Betik örneği çalıştırıldıktan sonra aşağıdaki komutu kullanarak kaynak grubunu, App Service uygulamasını kaldırmak için kullanılabilir ve tüm ilgili kaynakları.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | App Service planı oluşturur. |
| [New-AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Bir App Service uygulaması oluşturur. |
| [New-AzureRMSQLServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | SQL Veritabanı sunucusu oluşturur. |
| [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) | SQL Veritabanı sunucusu için bir güvenlik duvarı kuralı oluşturur. |
| [New-AzureRMSQLDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) | Bir veritabanı veya elastik bir veritabanı oluşturur. |
| [Set-AzureRmWebApp](/powershell/module/azurerm.websites/set-azurermwebapp) | Bir App Service uygulamasının yapılandırmasını değiştirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure App Service için ek Azure Powershell örnekleri bulunabilir [Azure PowerShell örnekleri](../samples-powershell.md).
