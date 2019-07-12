---
title: Azure geliştirme alanları giriş
titleSuffix: Azure Dev Spaces
author: zr-msft
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.author: zarhoads
ms.date: 05/07/2019
ms.topic: overview
description: Azure geliştirme alanları giriş
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, kapsayıcılar, kubectl, k8s
manager: gwallace
ms.openlocfilehash: 33ac5a7aa6d823105b87325ba52aa77cd9b9b3a3
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67706295"
---
# <a name="azure-dev-spaces"></a>Azure Dev Spaces

Azure geliştirme alanları bir hızlı, yinelemeli Kubernetes geliştirme için Azure Kubernetes Service (AKS) kümeleri teams'de deneyimidir. Paylaşılan bir AKS kümesinde ekibinizle işbirliği yapabilir. Azure geliştirme alanları uygulamanızın tüm bileşenleri olmadan çoğaltmayı veya sahte işlem bağımlılıklarını AKS test etmenizi sağlar. Yinelemeli olarak çalıştırın ve AKS ile en az bir geliştirme makinesi Kurulumu doğrudan kapsayıcılarında hata ayıklama.

![](media/azure-dev-spaces/collaborate-graphic.gif)


## <a name="how-azure-dev-spaces-simplifies-kubernetes-development"></a>Azure Dev Spaces’in Kubernetes geliştirmeyi kolaylaştırması

Azure geliştirme alanları, tüm mikro hizmetler mimarisi veya AKS'de çalışan uygulama ile doğrudan çalışmak takımlar sağlayarak geliştirme ve mikro hizmet uygulamalarını hızlı bir yinelemesini odaklanmak için takımlar yardımcı olur. Azure geliştirme alanları, ayrıca rest AKS kümesi ya da diğer geliştiriciler etkilemeden yalıtım, mikro hizmetler mimarisinde bölümlerini birbirinden bağımsız olarak güncelleştirmek için bir yol sağlar. Azure geliştirme alanları, geliştirme ve test alt düzey geliştirme ve test ortamlarında yöneliktir ve üretim AKS kümeleri üzerinde çalıştırmak için tasarlanmamıştır.

Takımlar iş ile tüm uygulama ve doğrudan AKS, Azure geliştirme alanları işbirliği yapın:

* Yerel makine Kurulum en aza indirir
* Yeni Takım geliştiriciler için hazırlık süresi azalır
* Daha hızlı yineleme aracılığıyla bir takımın hızını artırır.
* Takım üyeleri kümeyi kullanabilir olduğundan yedekli geliştirme ve tümleştirme ortamlarında sayısını azaltır.
* Çoğaltma veya bağımlılıkları sahte ihtiyacını ortadan kaldırır
* Geliştirme ekipleri ve bunun yanı sıra, DevOps takımları gibi çalıştıkları takımlar arasında işbirliğini artırır.

Azure geliştirme alanları projeleriniz için Docker ve Kubernetes varlıklar oluşturmak için araçlar sağlar. Bu araç, hem geliştirme alanı hem de diğer AKS kümeleri yeni ve mevcut uygulamaları kolayca eklemenize olanak sağlar.

Azure geliştirme alanları birlikte nasıl çalıştığı hakkında daha fazla bilgi için bkz. [nasıl Azure geliştirme alanları çalışır ve olan yapılandırılmış][how-dev-spaces-works].

## <a name="supported-regions-and-configurations"></a>Desteklenen bölgeler ve yapılandırmaları

Azure Dev Spaces yalnızca **Doğu ABD**, **Doğu ABD 2**, **Orta ABD**, **Batı ABD 2**, **Kuzey Avrupa**, **Batı Avrupa**, **UK Güney**, **Güneydoğu Asya** ve **Avustralya Doğu**, **Kanada Orta** ve **Kanada Doğu** bölgelerindeki AKS kümelerinde desteklenir. Azure Dev Spaces, AKS üzerinde uygulamalarınızı derleyip çalıştırmak için [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) veya [Visual Studio Code](https://code.visualstudio.com/download) ile birlikte Linux, MacOS veya Windows 8 ya da sonraki sürümlerde yüklü [Azure Dev Spaces uzantısının](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) kullanılmasını gerektirir. Kullanarak da destekler [Visual Studio](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) yüklü Windows 8 veya üzeri. Azure geliştirme iş yükünü Visual Studio 2019 için ihtiyacınız olacak. Visual Studio 2017 için Web geliştirme iş yükü gerekir ve [Kubernetes için Visual Studio Araçları](https://aka.ms/get-vsk8stools).

## <a name="next-steps"></a>Sonraki adımlar

Takım geliştirme Hızlı Başlangıç ile Azure geliştirme alanları ile ekipler için hızlı, yinelemeli geliştirme hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Takım geliştirme hızlı başlangıç](quickstart-team-development.md)


[how-dev-spaces-works]: how-dev-spaces-works.md
[team-development-quickstart]: quickstart-team-development.md
