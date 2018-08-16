---
title: include dosyası
description: include dosyası
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: a8f5adc8e6745fdbfd31d1a9cb9407672d61da07
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2018
ms.locfileid: "40129361"
---
### <a name="configure-your-aks-cluster-to-use-azure-dev-spaces"></a>AKS kümenizi Azure Dev Spaces kullanacak şekilde yapılandırma

Bir komut penceresi açın ve AKS kümenizi içeren kaynak grubuyla AKS kümesi adınızı kullanarak aşağıdaki Azure CLI komutunu girin. Komut, kümenizi Azure Dev Spaces desteğiyle yapılandırır.

   ```cmd
   az aks use-dev-spaces -g MyResourceGroup -n MyAKS
   ```

