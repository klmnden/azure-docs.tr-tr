---
title: Azure Service Fabric Mesh CLI’yi ayarlama | Microsoft Docs
description: Azure Service Fabric Mesh CLI’yi ayarlamayı öğrenin.
services: service-fabric-mesh
keywords: ''
author: tylermsft
ms.author: twhitney
ms.date: 07/26/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 7e8a12a215c94102f6b08262f129faebf9cfcde9
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49115633"
---
# <a name="set-up-the-service-fabric-mesh-cli"></a>Service Fabric Mesh CLI’yi ayarlama
Service Fabric Mesh’de kaynak dağıtmak ve yönetmek için Service Fabric Mesh Komut Satırı Arabirimi (CLI) gerekir. 

Önizleme için, Azure Fabric Mesh CLI Azure CLI’nin uzantısı olarak yazılır. Azure Cloud Shell’de veya Azure CLI’nin yerel kurulumunda bunu yükleyebilirsiniz. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

## <a name="install-the-service-fabric-mesh-cli-locally"></a>Service Fabric Mesh CLI’sini yerel olarak yükleme
CLI’yi yerel olarak yükleyip kullanmayı tercih ederseniz Azure CLI 2.0.43 veya üzerini yüklemeniz gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. CLI’nin en son sürümünü yüklemek veya en son sürümüne yükseltmek için bkz. [Azure CLI yükleme][azure-cli-install].

Aşağıdaki komutu kullanarak Azure Service Fabric Mesh CLI uzantısı modülünü yükleyin. 

```azurecli-interactive
az extension add --name mesh
```

Var olan bir Azure Service Fabric Mesh CLI modülünü güncelleştirmek için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az extension update --name mesh
```

[Windows dağıtım ortamınızı](service-fabric-mesh-howto-setup-developer-environment-sdk.md) da ayarlayabilirsiniz.

[azure-cli-install]: /cli/azure/install-azure-cli
