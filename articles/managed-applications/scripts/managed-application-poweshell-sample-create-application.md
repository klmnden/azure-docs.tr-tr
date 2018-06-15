---
title: Azure PowerShell Betiği örnek - yönetilen bir uygulamayı dağıtma | Microsoft Docs
description: Azure PowerShell Betiği örnek - bir yönetilen uygulama tanımını dağıtma
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
ms.openlocfilehash: 2429d561beffed5bc171b9dbc2c2c9c88eba3313
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2017
ms.locfileid: "23941253"
---
# <a name="deploy-a-managed-application-for-a-service-catalog-with-powershell"></a>PowerShell ile bir hizmet kataloğu için yönetilen bir uygulamayı dağıtma

Bu komut dosyasını bir yönetilen uygulama tanımını hizmet Kataloğu'ndan dağıtır.


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/managed-applications/create-application/create-application.ps1 "Create application")]


## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, yönetilen uygulamayı dağıtmak için aşağıdaki komutu kullanır. Komut özgü belgelere Tablo bağlantıları her komut.

| Komut | Notlar |
|---|---|
| [AzureRmManagedApplication yeni](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermmanagedapplication) | Yönetilen bir uygulama oluşturun. Tanımı kimliği ve parametreleri için şablonu sağlayın. |


## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamaların giriş için bkz: [Azure yönetilen uygulama genel bakış](../overview.md).
* PowerShell hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](https://docs.microsoft.com/powershell/azure/get-started-azureps).
