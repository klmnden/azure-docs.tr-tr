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
ms.openlocfilehash: 7f4dced6f82622ba735b1b059f30d88830347fba
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34625857"
---
## <a name="configure-your-aks-cluster-to-use-azure-dev-spaces"></a>AKS kümenizi Azure Dev alanlarını kullanacak şekilde yapılandırın

Bir komut penceresi açın ve AKS kümenizi ve AKS küme adınızı içeren kaynak grubunu kullanarak aşağıdaki Azure CLI komutu girin. Komutu, küme Azure Dev alanları için destek yapılandırır.

   ```cmd
   az aks use-dev-spaces -g MyResourceGroup -n MyAKS
   ```

