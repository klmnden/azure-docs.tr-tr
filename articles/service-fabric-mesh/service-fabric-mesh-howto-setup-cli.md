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
ms.openlocfilehash: c0e2aefe1222263b169e21490da079b165a57321
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2018
ms.locfileid: "42108487"
---
# <a name="set-up-the-service-fabric-mesh-cli"></a>Service Fabric Mesh CLI’yi ayarlama
Service Fabric Mesh’de kaynak dağıtmak ve yönetmek için Service Fabric Mesh CLI gerekir. 

Önizleme için, Azure Fabric Mesh CLI Azure CLI’nin uzantısı olarak yazılır. Azure Cloud Shell’de veya Azure CLI’nin yerel kurulumunda bunu yükleyebilirsiniz. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

CLI’yi yerel olarak yükleyip kullanmayı tercih ederseniz Azure CLI 2.0.43 veya üzerini yüklemeniz gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. CLI’nin en son sürümünü yüklemek veya en son sürümüne yükseltmek için bkz. [Azure CLI 2.0’ı yükleme][azure-cli-install].

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