---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: c20f86fe7fdcfc7ecc940923a8c98fa1fbf4cf65
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188147"
---
## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group) ile bir kaynak grubu oluşturun. Azure kaynak grubu; işlev uygulamaları, veritabanları ve depolama hesapları gibi Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek `myResourceGroup` adlı bir kaynak grubu oluşturur.  
Cloud Shell kullanmıyorsanız önce `az login` kullanarak oturum açın.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```
Genellikle kaynak grubunuzu ve kaynakları kendinize yakın bir bölgede oluşturursunuz. App Service planları için desteklenen tüm konumları görüntülemek için [az appservice list-locations](/cli/azure/appservice#az-appservice-list-locations) komutunu çalıştırın.
