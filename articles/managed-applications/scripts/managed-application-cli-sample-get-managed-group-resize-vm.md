---
title: "Azure CLI örnek komut dosyası - yönetilen kaynak grubunu Al ve sanal makineleri yeniden boyutlandırmak | Microsoft Docs"
description: "Azure CLI komut dosyası örneği - yönetilen kaynak grubunu Al ve sanal makineleri yeniden boyutlandırma"
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
ms.openlocfilehash: c78d2646471e40d60972cf91cb5bbd351f71a66c
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2017
---
# <a name="get-resources-in-a-managed-resource-group-and-resize-vms-with-azure-cli"></a>Yönetilen kaynak grubunda kaynaklar alın ve Azure CLI ile sanal makineleri yeniden boyutlandırma

Bu komut dosyası yönetilen kaynak grubundan kaynakları alır ve bu kaynak grubundaki sanal makineleri yeniden boyutlandırır.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli[main](../../../cli_scripts/managed-applications/get-application/get-application.sh "Get application")]


## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, yönetilen uygulamayı dağıtmak için aşağıdaki komutları kullanır. Komut özgü belgelere Tablo bağlantıları her komut.

| Komut | Notlar |
|---|---|
| [az managedapp listesi](https://docs.microsoft.com/cli/azure/managedapp#az_managedapp_list) | Yönetilen uygulamaların listesi. Sonuçları odaklanmak için sorgu değerleri sağlayın. |
| [az kaynak listesi](https://docs.microsoft.com/cli/azure/resource#az_resource_list) | Kaynakları listeler. Bir kaynak grubu ve sorgu değerleri sonucu odaklanmaya sağlayın. |
| [az vm yeniden boyutlandırma](https://docs.microsoft.com/cli/azure/vm#az_vm_resize) | Bir sanal makinenin boyutu güncelleştirin. |


## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamaların giriş için bkz: [Azure yönetilen uygulama genel bakış](../overview.md).
* Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).
