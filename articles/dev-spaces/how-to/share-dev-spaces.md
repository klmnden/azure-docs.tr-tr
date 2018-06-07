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
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Hizmeti, kapsayıcılar
manager: douge
ms.openlocfilehash: c469a71ec1357dd6f260ab613fed72110d1ed880
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34824362"
---
# <a name="share-azure-dev-spaces"></a>Paylaşım Azure Dev alanları

Azure Dev alanları ile geliştirme alanınızda başkalarıyla ekibinizin paylaşabilirsiniz. Her geliştirici, kendi alanda başkalarının parçalamak, korkusu olmadan çalışabilir. Ayrıca, bir alanda birlikte çalışan, kod uçtan uca mocks oluşturmak veya bağımlılıkları benzetimini yapmak zorunda kalmadan test etmek etkinleştirebilirsiniz. Bkz: [takım geliştirme hakkında daha fazla bilgi](../get-started-nodejs.md#learn-about-team-development) daha fazla bilgi için Kılavuzu.

Birden çok geliştiriciler için bir geliştirme alanını ayarlamak için:
1. Bir geliştirme alanı Azure'da oluşturun. Seçin [.NET Core ve VS Code](../get-started-netcore.md), [.NET Core ve Visual Studio](../get-started-netcore-visualstudio.md), veya [Node.js ve VS Code](../get-started-nodejs.md). Seçili Azure aboneliğinin sahibi veya katkıda erişiminizin olması gerekir.
1. Azure Dev alanı ait yapılandırma **kaynak grubu** için [katkıda bulunan erişim](/azure/active-directory/role-based-access-control-configure) her ekip üyesi için. Şu komutu çalıştırarak geliştirme boşluk ait kaynak grubu denetleyebilirsiniz: `azds resource list`
1. Takım üyeleri için sorun **geliştirme alanı seçin** içinde geliştirmek için.
     * **Komut satırı veya VS Code**: var olan Azure Dev Spacess görmek için erişiminiz: `azds resource list`. Bir geliştirici alanı seçmek için: `azds resource select`.
     * **Visual Studio IDE**: Visual Studio'da seçin bir projeyi açın **Azure Dev alanları** öğesinden başlatma ayarları açılır. Açılan iletişim kutusunda, var olan bir kümeyi seçin.

![Visual Studio başlatma ayarları açılır](../media/get-started-netcore-visualstudio/LaunchSettings.png)