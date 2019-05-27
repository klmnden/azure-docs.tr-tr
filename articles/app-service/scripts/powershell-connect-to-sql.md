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
ms.openlocfilehash: b008ef47ba530affd65e4d0e5e293312cfe74b69
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66136518"
---
# <a name="connect-an-app-service-app-to-a-sql-database"></a>Bir App Service uygulaması bir SQL veritabanı'na bağlanma

Bu senaryoda, bir Azure SQL veritabanı ve App Service uygulaması oluşturmayı öğreneceksiniz. Daha sonra uygulama ayarlarını kullanarak uygulamaya SQL veritabanına bağlayacaksınız.

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview) bulunan yönergeleri kullanarak Azure PowerShell’i yükleyin ve ardından Azure ile bağlantı oluşturmak için `Connect-AzAccount` komutunu çalıştırın.

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/connect-to-sql/connect-to-sql.ps1?highlight=13 "Connect an app to a SQL database")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Betik örneği çalıştırıldıktan sonra aşağıdaki komutu kullanarak kaynak grubunu, App Service uygulamasını kaldırmak için kullanılabilir ve tüm ilgili kaynakları.

```powershell
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Yeni AzAppServicePlan](/powershell/module/az.websites/new-azappserviceplan) | App Service planı oluşturur. |
| [New-AzWebApp](/powershell/module/az.websites/new-azwebapp) | Bir App Service uygulaması oluşturur. |
| [New-AzSQLServer](/powershell/module/az.sql/new-azsqlserver) | SQL Veritabanı sunucusu oluşturur. |
| [Yeni AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) | SQL Veritabanı sunucusu için bir güvenlik duvarı kuralı oluşturur. |
| [Yeni AzSQLDatabase](/powershell/module/az.sql/new-azsqldatabase) | Bir veritabanı veya elastik bir veritabanı oluşturur. |
| [Set-AzWebApp](/powershell/module/az.websites/set-azwebapp) | Bir App Service uygulamasının yapılandırmasını değiştirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure App Service için ek Azure Powershell örnekleri bulunabilir [Azure PowerShell örnekleri](../samples-powershell.md).
