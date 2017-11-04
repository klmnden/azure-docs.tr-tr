---
title: "Azure CLI betik örnek - bir yönetilen uygulama tanımı oluşturun. | Microsoft Docs"
description: "Azure CLI betik örnek - bir yönetilen uygulama tanımı oluştur"
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
ms.openlocfilehash: 6cfc51e5787ad6ab100638d0965b69232cda070a
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2017
---
# <a name="create-a-managed-application-definition-with-azure-cli"></a>Azure CLI ile bir yönetilen uygulama tanımı oluşturun

Bu komut dosyasını bir yönetilen uygulama tanımını bir hizmet Kataloğu yayımlar. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli[main](../../../cli_scripts/managed-applications/create-definition/create-definition.sh "Create definition")]


## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, yönetilen uygulama tanımı oluşturmak için aşağıdaki komutu kullanır. Komut özgü belgelere Tablo bağlantıları her komut.

| Komut | Notlar |
|---|---|
| [az managedapp tanımı oluşturun](https://docs.microsoft.com/cli/azure/managedapp/definition#az_managedapp_definition_create) | Bir yönetilen uygulama tanımı oluşturun. Gerekli dosyaları içeren paket sağlayın. |


## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamaların giriş için bkz: [Azure yönetilen uygulama genel bakış](../overview.md).
* Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).
