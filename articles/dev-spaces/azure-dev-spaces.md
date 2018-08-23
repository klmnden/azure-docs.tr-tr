---
title: Azure Dev Spaces | Microsoft Docs
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 06/01/2018
ms.topic: tutorial
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Hizmeti, kapsayıcılar
manager: douge
ms.openlocfilehash: 14b51cc2ad2e8e0f294e5e73e542001e30d21c9d
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2018
ms.locfileid: "41918845"
---
# <a name="azure-dev-spaces"></a>Azure Dev Spaces
Azure Dev Spaces, ekiplere yönelik hızlı ve yinelemeli bir Kubernetes geliştirme deneyimi sunar. Minimum geliştirme makinesi kurulumu ile, doğrudan Azure Kubernetes Service (AKS) içinde yinelemeli olarak kapsayıcıları çalıştırabilir ve kapsayıcıların hatasını ayıklayabilirsiniz. Windows, Mac veya Linux’ta, Visual Studio, Visual Studio Code veya komut satırı gibi bilindik araçları kullanarak geliştirme gerçekleştirin.

[!INCLUDE[](includes/dev-spaces-preview.md)]

## <a name="how-azure-dev-spaces-simplifies-kubernetes-development"></a>Azure Dev Spaces’in Kubernetes geliştirmeyi kolaylaştırması 

Azure Dev Spaces, geliştirme ekiplerinin aşağıdaki yöntemlerle Kubernetes’te daha üretken olmasına yardımcı olur:
- Her ekip üyesi için yerel geliştirme makinesi kurulumunu en aza indirme ve Azure’da yönetilen bir Kubernetes kümesi olan AKS’de doğrudan çalışma.
- Visual Studio 2017 veya Visual Studio Code kullanarak doğrudan Kubernetes’te hızlı şekilde kodu yineleyebilir ve kodun hatalarını ayıklayabilirsiniz.
- Geliştirmeden üretime kadar kullanmanız için kod varlıkları olarak Docker ve Kubernetes yapılandırması oluşturun. 
- Yönetilen bir Kubernetes kümesini ekibinizle paylaşın ve işbirliğine dayalı olarak birlikte çalışın. Kodunuzu yalıtılmış olarak geliştirin ve bağımlılıkları çoğaltmadan ya da taklit etmeden diğer bileşenlerle uçtan uca testler yapın.

[!INCLUDE[](includes/get-started.md)]

![](media/azure-dev-spaces/vscode-overview.png)

## <a name="see-also"></a>Ayrıca Bkz.

[Azure Kubernetes Service](/azure/aks)