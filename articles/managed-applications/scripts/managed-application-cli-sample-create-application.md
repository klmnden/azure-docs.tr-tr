---
title: "Azure CLI betik örnek - yönetilen bir uygulamayı dağıtma | Microsoft Docs"
description: "Azure CLI betik örnek - bir yönetilen uygulama tanımını dağıtma"
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/25/2017
ms.author: tomfitz
ms.openlocfilehash: 62d0247df3b3d9f242877e4ea27ccc871cf797c0
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2017
---
# <a name="deploy-a-managed-application-for-service-catalog-with-azure-cli"></a>Hizmet Kataloğu Azure CLI ile yönetilen bir uygulamayı dağıtmak

Bu komut dosyasını bir yönetilen uygulama tanımını hizmet Kataloğu'ndan dağıtır. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli[main](../../../cli_scripts/managed-applications/create-application/create-application.sh "Create application")]


## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, yönetilen uygulamayı dağıtmak için aşağıdaki komutu kullanır. Komut özgü belgelere Tablo bağlantıları her komut.

| Komut | Notlar |
|---|---|
| [az managedapp oluşturma](https://docs.microsoft.com/cli/azure/managedapp#az_managedapp_create) | Yönetilen bir uygulama oluşturun. Tanımı kimliği ve parametreleri için şablonu sağlayın. |


## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamaların giriş için bkz: [Azure yönetilen uygulama genel bakış](../overview.md).
* Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).
