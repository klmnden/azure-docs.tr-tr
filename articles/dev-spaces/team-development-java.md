---
title: Azure geliştirme Java ve VS Code kullanarak alanları ile takım geliştirme
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: stepro
ms.author: stephpr
ms.date: 08/01/2018
ms.topic: tutorial
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, kapsayıcılar, Helm, hizmet kafes, ağ hizmeti Yönlendirme, kubectl, k8s '
manager: mmontwil
ms.openlocfilehash: 458cf762b2d24d325a21f2f8032895a0a87f957c
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65765182"
---
[!INCLUDE [](../../includes/devspaces-team-development-1.md)]

### <a name="make-a-code-change"></a>Bir kod değişikliği yaptığınızda
Git için VS Code penceresinin `mywebapi` ve düzenlemek için bir kod `String index()` yönteminde `src/main/java/com/ms/sample/mywebapi/Application.java`, örneğin:

```java
@RequestMapping(value = "/", produces = "text/plain")
public String index() {
    return "Hello from mywebapi says something new";
}
```

[!INCLUDE [](../../includes/devspaces-team-development-2.md)]
