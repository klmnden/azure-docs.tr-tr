---
title: Azure geliştirme alanları paylaşma | Microsoft Docs
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
ms.openlocfilehash: 9345015a8732da02ea68711cd807556a543d8d01
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37930153"
---
# <a name="share-azure-dev-spaces"></a>Azure geliştirme alanları paylaşın

Azure geliştirme alanları ile takımınızdaki diğer kişilerle, geliştirme alanı paylaşabilir. Her geliştirici kendi alanlarına diğerleri bozucu ödemeden olmadan çalışabilir. Ayrıca, birlikte bir alanda çalışma mocks oluşturun veya bağımlılıkları benzetimini yapmak zorunda kalmadan kod baştan sona test etmek etkinleştirebilirsiniz. Bkz: [takım geliştirme hakkında bilgi edinin](../team-development-nodejs.md) Kılavuzu daha fazla bilgi için.

## <a name="set-up-a-dev-space-for-multiple-developers"></a>Birden çok geliştirici için geliştirme boşlukla ayarlayın

1. Azure'da bir geliştirme alanı oluşturun. Seçin [.NET Core ve VS Code](../get-started-netcore.md), [.NET Core ve Visual Studio](../get-started-netcore-visualstudio.md), veya [Node.js ve VS Code](../get-started-nodejs.md). Seçilen Azure aboneliği sahibi veya katkıda bulunan erişiminiz olması gerekir.
1. Azure geliştirme alanı'nın yapılandırma **kaynak grubu** için [katkıda bulunan erişimi](/azure/active-directory/role-based-access-control-configure) her ekip üyesi için. Geliştirme boşluk ait kaynak grubu, bu komutu çalıştırarak denetleyebilirsiniz: `azds list`
1. Ekip üyelerine isteyin **geliştirme alanı seçin** içinde geliştirmek üzere.
     * **Komut satırı veya VS Code**: var olan Azure geliştirme alanları görmek için erişiminiz: `azds space list`. Geliştirme alanı seçmek için: `azds space select`.
     * **Visual Studio IDE**: Visual Studio'da seçin bir projeyi açmayı **Azure geliştirme alanları** başlatma ayarları açılır listeden. Açılır iletişim kutusunda, var olan bir küme seçin.

    ![Visual Studio başlatma ayarları açılır](../media/get-started-netcore-visualstudio/LaunchSettings.png)

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [takım geliştirme hakkında bilgi edinin](../team-development-nodejs.md) daha fazla bilgi için.