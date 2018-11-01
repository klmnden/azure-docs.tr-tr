---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: d44865dc3189a7f9dc05106baf9f4d120e5e8bf6
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133089"
---
## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#az_group_create) ile bir kaynak grubu oluşturun. Azure kaynak grubu; işlev uygulamaları, veritabanları ve depolama hesapları gibi Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek `myResourceGroup` adlı bir kaynak grubu oluşturur.  
Cloud Shell kullanmıyorsanız önce `az login` kullanarak oturum açın.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```
Genellikle kaynak grubunuzu ve kaynakları kendinize yakın bir bölgede oluşturursunuz. App Service planları için desteklenen tüm konumları görüntülemek için [az appservice list-locations](/cli/azure/appservice#az-appservice-list-locations) komutunu çalıştırın.