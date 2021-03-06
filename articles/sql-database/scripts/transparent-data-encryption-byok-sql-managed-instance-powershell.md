---
title: "PowerShell: -BYOK TDE'yi etkinleştirmek Azure SQL veritabanı yönetilen örneği | Microsoft Docs"
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
ms.date: 04/19/2019
ms.openlocfilehash: c2c4bd7bffd923430d0817cb6ea975f4c1596623
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66729157"
---
# <a name="manage-transparent-data-encryption-in-a-managed-instance-using-your-own-key-from-azure-key-vault-preview"></a>Kendi anahtarınızı Azure anahtar kasasından (Önizleme) kullanarak yönetilen örneğe saydam veri şifrelemeyi yönetme

Bu PowerShell Betiği örneği, Azure SQL yönetilen Azure Key vault'tan bir anahtar kullanarak örneği için kendi anahtarını getir (Önizleme) senaryosunda saydam veri şifrelemesi (TDE) yapılandırır. TDE Getir bilgisayarınızı kendi anahtarını (BYOK) destekli hakkında daha fazla bilgi için bkz: [TDE kendi anahtarını Getir için Azure SQL](../transparent-data-encryption-byok-azure-sql.md).

## <a name="prerequisites"></a>Önkoşullar

- Mevcut bir yönetilen örneği. Bkz: [PowerShell kullanarak Azure SQL veritabanı yönetilen örneği](sql-database-create-configure-managed-instance-powershell.md).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Her iki PowerShell kullanarak yerel olarak veya Azure Cloud Shell kullanarak AZ PowerShell 1.1.1-preview veya sonraki bir önizleme sürümünü gerektirir. Yükseltme gerekiyorsa, bkz [Azure PowerShell modülü yükleme](/powershell/azure/install-az-ps), veya modülü yüklemek için örnek kod aşağıda.

`Install-Module -Name Az.Sql -RequiredVersion 1.1.1-preview -AllowPrerelease -Force`

PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="sample-scripts"></a>Örnek komut dosyaları

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/transparent-data-encryption/setup-tde-byok-sqlmi.ps1 "Set up BYOK TDE for SQL Managed Instance")]

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek SQL Veritabanı PowerShell betiği örnekleri [Azure SQL Veritabanı PowerShell betikleri](../sql-database-powershell-samples.md) içinde bulunabilir.
