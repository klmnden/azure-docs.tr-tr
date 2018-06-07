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
ms.openlocfilehash: 44ce986fcfdf079d3077c007a378f3467073d00d
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34823189"
---
### <a name="configure-your-aks-cluster-to-use-azure-dev-spaces"></a>AKS kümenizi Azure Dev alanlarını kullanacak şekilde yapılandırın

Bir komut penceresi açın ve AKS kümenizi ve AKS küme adınızı içeren kaynak grubunu kullanarak aşağıdaki Azure CLI komutu girin. Komutu, küme Azure Dev alanları için destek yapılandırır.

   ```cmd
   az aks use-dev-spaces -g MyResourceGroup -n MyAKS
   ```

