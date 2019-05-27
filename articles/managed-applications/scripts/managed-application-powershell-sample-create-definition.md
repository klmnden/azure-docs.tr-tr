---
title: Azure PowerShell betik örneği - yönetilen uygulama tanımı oluşturma | Microsoft Docs
description: Azure PowerShell betik örneği - yönetilen uygulama tanımı oluşturma
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/27/2017
ms.author: tomfitz
ms.openlocfilehash: 35f1b805323da03f1622e5c355d6ffa6d960b275
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66171499"
---
# <a name="create-a-managed-application-definition-with-powershell"></a>PowerShell ile bir yönetilen uygulama tanımı oluşturma

Bu betik bir hizmet kataloğunda yönetilen bir uygulama tanımını yayımlar.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/managed-applications/create-definition/create-definition.ps1 "Create definition")]


## <a name="script-explanation"></a>Betik açıklaması

Bu betik, yönetilen uygulama tanımını oluşturmak için aşağıdaki komutu kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Yeni AzManagedApplicationDefinition](https://docs.microsoft.com/powershell/module/az.resources/new-azmanagedapplicationdefinition) | Yönetilen uygulama tanımı oluşturur. Gerekli dosyaları içeren paketi sağlar. |


## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamalara giriş için [Azure Yönetilen Uygulamalara genel bakış](../overview.md) konusunu inceleyin.
* PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/azure/get-started-azureps).
