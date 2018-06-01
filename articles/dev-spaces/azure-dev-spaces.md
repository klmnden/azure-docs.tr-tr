---
title: Azure Dev Spaces | Microsoft Docs
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: tutorial
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Hizmeti, kapsayıcılar
manager: douge
ms.openlocfilehash: 344947b7906d15e819e372e0affe4af3c34ba69b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34198767"
---
# <a name="azure-dev-spaces"></a>Azure Dev Spaces
Azure Dev Spaces, Kubernetes üzerinde hızla geliştirmenize yardımcı olur. Azure Dev Spaces’i kullanarak ayrıca Azure Kubernetes kapsayıcılarınızda hata ayıklama gibi tam geliştirme özellikleri eklersiniz ve bulutta VS Code, Visual Studio gibi bilindik araçlar ya da komut satırı kullanarak kapsayıcıları yinelemeli olarak geliştirebilirsiniz. Azure Dev Spaces, kendi alanlarında tek kod dallarının yalıtımının, geliştirme yaşam döngüsünde kritik bir öneme sahip olduğu takım geliştirme ile özellikle ilgilidir.

## <a name="how-azure-dev-spaces-simplifies-kubernetes-development"></a>Azure Dev Spaces’in Kubernetes geliştirmeyi kolaylaştırması 

Bu yaklaşımın birkaç avantajı vardır:

* Bulut kaynaklarına tam erişimle, üretimi temsil eden altyapısız bir geliştirme ortamı elde etmek.
* Node.js ve .NET Core kapsayıcılarının hatalarını, VS Code veya Visual Studio ile doğrudan Kubernetes içinde ayıklamak. Diğer tüm diller komut satırı arabirimi ile geliştirilebilir.
* Maliyetleri azaltmak ve yeni takım üyeleri için yerel makine kurulumunu en aza indirmek üzere geliştirme takımınızda bir Kubernetes örneğini paylaşın.
* Kodunuzu yalıtılmış olarak geliştirin ve bağımlılıkları çoğaltmadan ya da taklit etmeden diğer bileşenlerle uçtan uca testler yapın.

[!INCLUDE[](includes/get-started.md)]

![](media/azure-dev-spaces/vscode-overview.png)