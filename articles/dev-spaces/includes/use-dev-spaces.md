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
ms.openlocfilehash: 2563f7c36283521541562bcd88f973d86a6f672a
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
## <a name="configure-your-aks-cluster-to-use-azure-dev-spaces"></a>AKS kümenizi Azure Dev alanlarını kullanacak şekilde yapılandırın

Bir komut penceresi açın ve AKS kümenizi ve AKS küme adınızı içeren kaynak grubunu kullanarak aşağıdaki Azure CLI komutları girin:

   ```cmd
   az extension add --name dev-spaces-preview 
   az aks use-dev-spaces -g MyResourceGroup -n MyAKS
   ```
İlk komut, Azure Dev alanları için destek eklemek için Azure CLI için uzantı yükler. ve Azure Dev alanları desteğiyle kümenizi ikinci yapılandırır.
