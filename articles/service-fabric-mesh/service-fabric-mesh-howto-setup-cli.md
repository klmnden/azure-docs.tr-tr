---
title: Azure Service Fabric Mesh CLI’yi ayarlama | Microsoft Docs
description: Azure Service Fabric Mesh CLI’yi ayarlamayı öğrenin.
services: service-fabric-mesh
keywords: ''
author: tylermsft
ms.author: twhitney
ms.date: 07/11/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: f56bcf0c844545e8883175da2bd3f22afdcd19ea
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39089536"
---
# <a name="set-up-the-service-fabric-mesh-cli"></a>Service Fabric Mesh CLI’yi ayarlama
Service Fabric Mesh’de kaynak dağıtmak ve yönetmek için Service Fabric Mesh CLI gerekir. 

Önizleme için, Azure Fabric Mesh CLI Azure CLI’nin uzantısı olarak yazılır. Azure Cloud Shell’de veya Azure CLI’nin yerel kurulumunda bunu yükleyebilirsiniz. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

CLI’yi yerel olarak yükleyip kullanmayı tercih ederseniz Azure CLI 2.0.35 veya üzerini yüklemeniz gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. CLI’nin en son sürümünü yüklemek veya en son sürümüne yükseltmek için bkz. [Azure CLI 2.0’ı yükleme][azure-cli-install].

Azure Service Fabric Mesh CLI modülünün önceki yüklemelerini kaldırın.

```azurecli-interactive
az extension remove --name mesh
```

Aşağıdaki komutu kullanarak Azure Service Fabric Mesh CLI uzantısı modülünü yükleyin. 

```azurecli-interactive
az extension add --source https://sfmeshcli.blob.core.windows.net/cli/mesh-0.8.1-py2.py3-none-any.whl
```

[Windows dağıtım ortamınızı](service-fabric-mesh-howto-setup-developer-environment-sdk.md) da ayarlayabilirsiniz.

[azure-cli-install]: /cli/azure/install-azure-cli