---
title: 'PowerShell: BYOK TDE - etkinleştirme Azure SQL veritabanı yönetilen örneği | Microsoft Docs'
description: Bir Azure SQL yönetilen bekleyen şifreleme için BYOK saydam veri şifrelemesi (TDE) kullanmaya başlamak için örnek yapılandırma konusunda bilgi PowerShell kullanarak.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: d428c74a486c4bcb6ea1568fbb8abd813960c708
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52974986"
---
# <a name="manage-transparent-data-encryption-in-a-managed-instance-using-your-own-key-from-azure-key-vault"></a>Saydam veri şifrelemesi kullanarak kendi anahtarınızı Azure anahtar Kasası'ndaki bir yönetilen örneğinde yönetme

Bu PowerShell Betiği örneği, Azure SQL yönetilen Azure Key vault'tan bir anahtar kullanarak örneği için kendi anahtarını Getir senaryosunda saydam veri şifrelemesi (TDE) yapılandırır. TDE Getir bilgisayarınızı kendi anahtarını (BYOK) destekli hakkında daha fazla bilgi için bkz: [TDE kendi anahtarını Getir için Azure SQL](../transparent-data-encryption-byok-azure-sql.md).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz, bu öğretici Azure PowerShell modülü 5.7.0 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz Bu öğretici Azurerm.SQL'e PowerShell paketi önizleme sürümünü de gerektirir. *4.11.6-preview*. Yüklemek için aşağıdaki komutu çalıştırın: `Install-Module -Name AzureRM.Sql -RequiredVersion 4.11.6-preview -AllowPrerelease`

## <a name="sample-scripts"></a>Örnek komut dosyaları

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/transparent-data-encryption/setup-tde-byok-sqlmi.ps1 "Set up BYOK TDE for SQL Managed Instance")]

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek SQL Veritabanı PowerShell betiği örnekleri [Azure SQL Veritabanı PowerShell betikleri](../sql-database-powershell-samples.md) içinde bulunabilir.
