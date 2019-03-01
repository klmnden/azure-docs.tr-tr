---
title: Azure geliştirme alanları paylaşma
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
author: zr-msft
ms.author: zarhoads
ms.date: 05/11/2018
ms.topic: conceptual
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, kapsayıcılar, Helm, hizmet kafes, ağ hizmeti Yönlendirme, kubectl, k8s '
ms.openlocfilehash: 76332c30143601d05fdf951e68f9e9aa8256a924
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57191382"
---
# <a name="share-azure-dev-spaces"></a>Azure geliştirme alanları paylaşın

Azure geliştirme alanları ile takımınızdaki diğer kişilerle, geliştirme alanı paylaşabilir. Her geliştirici kendi alanlarına diğerleri bozucu ödemeden olmadan çalışabilir. Ayrıca, birlikte bir alanda çalışma mocks oluşturun veya bağımlılıkları benzetimini yapmak zorunda kalmadan kod baştan sona test etmek etkinleştirebilirsiniz. Bkz: [takım geliştirme hakkında bilgi edinin](../team-development-nodejs.md) Kılavuzu daha fazla bilgi için.

## <a name="set-up-a-dev-space-for-multiple-developers"></a>Birden çok geliştirici için geliştirme boşlukla ayarlayın

1. Azure'da bir geliştirme alanı oluşturun. Seçin [.NET Core ve VS Code](../get-started-netcore.md), [.NET Core ve Visual Studio](../get-started-netcore-visualstudio.md), veya [Node.js ve VS Code](../get-started-nodejs.md). Seçilen Azure aboneliği sahibi veya katkıda bulunan erişiminiz olması gerekir.
1. Azure geliştirme alanı'nın yapılandırma **kaynak grubu** için [katkıda bulunan erişimi](/azure/active-directory/role-based-access-control-configure) her ekip üyesi için. Geliştirme boşluk ait kaynak grubu, bu komutu çalıştırarak denetleyebilirsiniz: `azds list-up`
1. Ekip üyelerine isteyin **geliştirme alanı seçin** içinde geliştirmek üzere.
     * **Komut satırı veya VS Code**: Mevcut Azure geliştirme alanları görmek için erişiminiz: `azds space list`. Geliştirme alanı seçmek için: `azds space select`.
     * **Visual Studio IDE**: Bir projeyi Visual Studio'da açık seçin **Azure geliştirme alanları** başlatma ayarları açılır listeden. Açılır iletişim kutusunda, var olan bir küme seçin.

    ![Visual Studio başlatma ayarları açılır](../media/get-started-netcore-visualstudio/LaunchSettings.png)

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [takım geliştirme hakkında bilgi edinin](../team-development-nodejs.md) daha fazla bilgi için.
