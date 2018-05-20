---
title: Azure Dev alanları paylaşma | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: article
description: Kapsayıcılar ve Azure üzerinde mikro ile hızlı Kubernetes geliştirme
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcıları
manager: douge
ms.openlocfilehash: 140a487a0a605e0f03122868af0d2d12bcdba35b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="share-azure-dev-spaces"></a>Paylaşım Azure Dev alanları

Azure Dev alanları ile geliştirme alanınızda başkalarıyla ekibinizin paylaşabilirsiniz. Her geliştirici, kendi alanda başkalarının parçalamak, korkusu olmadan çalışabilir. Ayrıca, bir alanda birlikte çalışan, kod uçtan uca mocks oluşturmak veya bağımlılıkları benzetimini yapmak zorunda kalmadan test etmek etkinleştirebilirsiniz. Bkz: [takım geliştirme hakkında daha fazla bilgi](../get-started-nodejs.md#learn-about-team-development) daha fazla bilgi için Kılavuzu.

Birden çok geliştiriciler için bir ortamı ayarlamak için:
1. Bir geliştirme alanı Azure'da oluşturun. Seçin [.NET Core ve VS Code](../get-started-netcore.md), [.NET Core ve Visual Studio](../get-started-netcore-visualstudio.md), veya [Node.js ve VS Code](../get-started-nodejs.md). Seçili Azure aboneliğinin sahibi veya katkıda erişiminizin olması gerekir.
1. Azure Dev alanı ait yapılandırma **kaynak grubu** için [katkıda bulunan erişim](/azure/active-directory/role-based-access-control-configure) her ekip üyesi için. Bir ortam kaynak grubu, şu komutu çalıştırarak denetleyebilirsiniz: `azds resource list`
1. Takım üyeleri için sorun **ortamı seçin** içinde geliştirmek için.
     * **Komut satırı veya VS Code**: var olan Azure Dev Spacess görmek için erişiminiz: `azds resource list`. Bir ortam seçmek için: `azds resource select`.
     * **Visual Studio IDE**: Visual Studio'da seçin bir projeyi açın **Azure Dev alanları** öğesinden başlatma ayarları açılır. Açılan iletişim kutusunda, varolan bir geliştirme ortamı seçin.

![Visual Studio başlatma ayarları açılır](../media/get-started-netcore-visualstudio/LaunchSettings.png)