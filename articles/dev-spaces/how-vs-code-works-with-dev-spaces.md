---
title: Visual Studio Code ile Azure geliştirme alanları nasıl çalışır?
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: zr-msft
ms.author: zarhoads
ms.date: 07/08/2019
ms.topic: conceptual
description: Visual Studio Code ile Azure geliştirme alanları nasıl çalışır?
keywords: Azure geliştirme alanları, geliştirme alanları, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, kapsayıcılar
ms.openlocfilehash: a7ec20908b75ae07532c16daab8950ace9cd67ae
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67712156"
---
# <a name="how-visual-studio-code-works-with-azure-dev-spaces"></a>Visual Studio Code ile Azure geliştirme alanları nasıl çalışır?

Visual Studio Code kullanabilirsiniz ve [Azure geliştirme alanları uzantısı][azds-extension] hazırlamak için çalıştırın ve hizmetlerinizi Azure geliştirme alanları ile hata ayıklama. Visual Studio Code ve Azure Dev alanları uzantısı ile şunları yapabilirsiniz:

* Çalıştırma ve hata ayıklama Hizmetleri aks'deki varlık oluşturma
* Java, Node.js ve .NET Core hizmetlerinizi bir geliştirme alanında çalıştırın
* Bir geliştirme alanında çalışan, Java, Node.js ve .NET Core Hizmetleri doğrudan hata ayıklama

## <a name="generate-assets"></a>Varlık oluşturma

Visual Studio Code ve Azure Dev alanları uzantısını projeniz için şu varlıkları oluşturun:

* Dockerfile'ları Java uygulamalarını Maven kullanarak Node.js uygulamaları ve .NET Core uygulamaları için
* Bir Dockerfile ile neredeyse her dil için Helm grafikleri
* Bir `azds.yaml` dosyasını [Azure geliştirme alanları yapılandırma dosyası][azds-yaml] projeniz için
* A `.vscode` projenize Java uygulamalarını Maven kullanarak Node.js uygulamaları ve .NET Core uygulamaları için Visual Studio Code başlatma yapılandırması ile klasörü

Dockerfile, Helm grafiği ve `azds.yaml` dosyalar çalıştırılırken oluşturulan aynı varlıkları `azds prep`. Bu dosyaları de Visual Studio Code'u dışında gibi çalışmasını AKS'de, projenizi çalıştırmak için kullanılabilir `azds up`. `.vscode` Klasör yalnızca Visual Studio kodu tarafından projenizi Visual Studio Code'dan AKS içinde çalıştırmak için kullanılır.

## <a name="run-your-service-in-aks"></a>AKS, hizmet çalıştırma

Projeniz için bir varlık oluşturduktan sonra Java, Node.js ve .NET Core hizmetlerinizi mevcut bir geliştirme alanında Visual Studio Code'dan çalıştırabilirsiniz. İçinde *hata ayıklama* sayfası Visual Studio Code, başlatma yapılandırmasından çağırabilirsiniz `.vscode` projenizi çalıştırmak için dizin.

AKS kümenizi oluşturmak ve Visual Studio Code dışında kümenizdeki Azure geliştirme alanları etkinleştirme gerekir. Örneğin, bu kurulum yapmak için Azure CLI veya Azure portalını kullanabilirsiniz. Mevcut dockerfile'ları Helm grafikleri, yeniden kullanabilir ve `azds.yaml` çalıştırılarak oluşturulan varlıklar gibi Visual Studio Code dışında oluşturulan dosyalar `azds prep`. Visual Studio Code dışında oluşturulan varlıkları tekrar, yine de ihtiyacınız bir `.vscode` dizin. Bu `.vscode` dizini Visual Studio code ve Azure Dev alanları uzantısı tarafından yeniden üretilebilir ve mevcut varlıklarınızı üzerine yazmaz.

.NET Core projeleri için aşağıdakiler gereklidir [ C# uzantısı][csharp-extension] installed to run your .NET service from Visual Studio Code. Also for Java projects using Maven, you must have the [Java Debugger for Azure Dev Spaces extension][java-extension] yanı yüklü [Maven'ın yüklü ve yapılandırılmış][maven] , Java çalıştırmak için Visual Studio code'dan hizmeti.

## <a name="debug-your-service-in-aks"></a>AKS, hizmetinde hata ayıklama

Projenizi başlattıktan sonra doğrudan Visual Studio Code'dan bir geliştirme alanında çalışan, Java, Node.js ve .NET Core Hizmetleri ayıklayabilirsiniz. Başlatma yapılandırmada `.vscode` directory hata ayıklama bir geliştirme alanında etkin olan bir hizmeti çalıştırmak için ek hata ayıklama bilgileri sağlar. Visual Studio Code ayrıca geliştirme alanlarınıza çalışan kapsayıcısında hata ayıklama işlemi kesme noktaları ayarlayın, değişkenleri incelemeyi ve diğer hata ayıklama işlemlerini gerçekleştirin izin veren ekler.


## <a name="use-visual-studio-code-with-azure-dev-spaces"></a>Azure geliştirme alanları ile Visual Studio Code'u kullanma

Visual Studio Code ve Azure Dev alanları uzantısını aşağıdaki hızlı başlangıçlar, Azure geliştirme alanları ile çalışma görebilirsiniz:

* [Java ile geliştirme][quickstart-java]
* [.NET ile geliştirme][quickstart-netcore]
* [Node.js ile geliştirme][quickstart-node]

[azds-extension]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
[azds-yaml]: how-dev-spaces-works.md#prepare-your-code
[csharp-extension]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp
[java-extension]: https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debugger-azds
[maven]: https://maven.apache.org
[quickstart-java]: quickstart-java.md
[quickstart-netcore]: quickstart-netcore.md
[quickstart-node]: quickstart-nodejs.md