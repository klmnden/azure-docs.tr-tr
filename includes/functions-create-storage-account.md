---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 889b9c0cf944085f5f42ece892d5cac747a27240
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66132088"
---
## <a name="create-an-azure-storage-account"></a>Azure Depolama hesabı oluşturma

İşlevler, işlevlerinizin durumunu ve diğer bilgilerini korumak için genel amaçlı bir Azure Depolama hesabı kullanır. Oluşturduğunuz kaynak grubunda [az storage account create](/cli/azure/storage/account) komutunu kullanarak genel amaçlı depolama hesabı oluşturun.

Aşağıdaki komutta `<storage_name>` yer tutucusunu gördüğünüz yere genel olarak benzersiz bir depolama hesabı adı yazın. Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir.

```azurecli-interactive
az storage account create --name <storage_name> --location westeurope --resource-group myResourceGroup --sku Standard_LRS
```

Depolama hesabı oluşturulduktan sonra Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{
  "creationTime": "2017-04-15T17:14:39.320307+00:00",
  "id": "/subscriptions/bbbef702-e769-477b-9f16-bc4d3aa97387/resourceGroups/myresourcegroup/...",
  "kind": "Storage",
  "location": "westeurope",
  "name": "myfunctionappstorage",
  "primaryEndpoints": {
    "blob": "https://myfunctionappstorage.blob.core.windows.net/",
    "file": "https://myfunctionappstorage.file.core.windows.net/",
    "queue": "https://myfunctionappstorage.queue.core.windows.net/",
    "table": "https://myfunctionappstorage.table.core.windows.net/"
  },
     ....
    // Remaining output has been truncated for readability.
}
```
