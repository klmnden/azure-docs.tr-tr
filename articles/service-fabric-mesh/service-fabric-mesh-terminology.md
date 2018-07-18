---
title: Azure Service Fabric kafes terminolojisi | Microsoft Docs
description: Azure Service Fabric Mesh için sık kullanılan terimler hakkında öğrenin.
services: service-fabric-mesh
keywords: ''
author: rwike77
ms.author: ryanwi
ms.date: 07/12/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 672e27bf53679c52dab8d42a52378aa90eba33cb
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39114319"
---
# <a name="service-fabric-mesh-terminology"></a>Service Fabric Mesh terminolojisi

Azure Service Fabric Mesh geliştiricilerin mikro hizmet uygulamaları sanal makineler, depolama, yönetme veya ağ dağıtmanıza olanak sağlayan tam olarak yönetilen bir hizmettir. Bu makalede belgelerinde kullanılan terimler anlamak için Azure Service Fabric Mesh tarafından kullanılan terimler açıklanmaktadır.

## <a name="service-fabric"></a>Service Fabric

Service Fabric, ölçeklenebilir ve güvenilir mikro Hizmetleri paketlemeyi, dağıtma ve kolay bir açık kaynak dağıtılmış sistemler platformudur. Service Fabric Service Fabric Mesh güç katan düzenleyicisidir. Service Fabric, nasıl yapı ve mikro hizmetler uygulamalarınızı çalıştırmak için seçenekler sağlar. Kendi hizmetlerinizi yazmanızı ve birden çok ortam seçenekleri uygulamayı çalıştırmak hedef konumu seçin, herhangi bir çerçeveyi kullanabilirsiniz.

## <a name="deployment-and-application-models"></a>Dağıtım ve uygulama modelleri 

Hizmetlerinizi dağıtmak için nasıl çalıştırılacağını tanımlamak gerekir. Service Fabric, üç farklı dağıtım modelleri destekler:

### <a name="resource-model"></a>Kaynak modeli
Uygulamalar, hizmetler, ağlar ve birimler dahil olmak üzere Service Fabric için ayrı ayrı dağıtılabilen herhangi bir şey kaynaklardır. Bir YAML dosyası ya da Azure kaynak modeli şemayı kullanarak bir JSON dosyası kullanarak kaynakları tanımlanır.

Kaynak modeli, Service Fabric uygulamalarınızı açıklamak için basit bir yoldur. Kendi ana basit dağıtım ve kapsayıcılı hizmetlerin yönetimini sağlamaktır.

Kaynakları, Service Fabric çalıştıran herhangi bir dağıtılabilir.

### <a name="native-model"></a>Yerel modeli
Yerel uygulama modeli için Service Fabric uygulamalarınızı tam alt düzey erişim sağlar. Uygulamalar ve hizmetler, XML bildirim dosyalarında kayıtlı türleri olarak tanımlanır.

Yerel modeli ve küme yönetimi API'leri C# ve Java Service Fabric çalışma zamanı API erişimi sağlayan Reliable Services framework destekler. Yerel modeli rastgele kapsayıcıları ve yürütülebilir dosyalar da destekler.

Yerel modeli, Service Fabric Mesh ortamda desteklenmiyor.

### <a name="docker-compose"></a>Docker Compose 
[Docker Compose](https://docs.docker.com/compose/) Docker projesindeki bir parçasıdır. Service Fabric, Docker Compose modelini kullanarak uygulama dağıtmak için sınırlı destek sağlar.

## <a name="environments"></a>Ortamlar

Service Fabric birkaç farklı hizmet ve ürünleri temel alan bir açık kaynak platformu teknolojisidir. Microsoft aşağıdaki seçenekleri sağlar:

 - **Service Fabric Mesh**: Microsoft Azure'da Service Fabric uygulamaları çalıştırmak için tam olarak yönetilen bir hizmet.
 - **Azure Service Fabric**: Azure'da barındırılan Service Fabric kümesi teklifi. Bu, Service Fabric ile birlikte Service Fabric Küme yükseltme ve yapılandırma yönetimi, Azure altyapı arasında tümleştirme sağlar.
 - **Service Fabric tek başına**: yükleme ve yapılandırma araçları kümesi [herhangi bir Service Fabric kümelerini dağıtmayı](/azure/service-fabric/service-fabric-deploy-anywhere) (şirket içinde veya diğer bulut sağlayıcılarına). Azure tarafından yönetilen değil.
 - **Service Fabric geliştirme kümesi**: Service Fabric uygulamaları geliştirme için Windows, Linux veya Mac üzerinde bir yerel geliştirme deneyimi sağlar.

## <a name="environment-framework-and-deployment-model-support-matrix"></a>Ortam, framework ve dağıtım modeline destek matrisi
Farklı ortamlar, farklı çerçeveler ve dağıtım modelleri için destek düzeyine sahip. Aşağıdaki tabloda, desteklenen çerçevesi ve dağıtım modeli birleşimlerini açıklar.

|Frameworks\Deployment modeli |Kaynak modeli |Yerel modeli | Oluştur|
|---|---|---|---|
|Reliable Actors ve güvenilir hizmetler |Desteklenmiyor |Desteklenen |Desteklenmiyor |
|Herhangi bir framework veya dil |Kapsayıcılarda desteklenir |Kapsayıcılar ve işlemler olarak desteklenen |Kapsayıcılarda desteklenir |

Desteklenen ortam ve dağıtım modeli birleşimleri aşağıdaki tabloda açıklanmaktadır.

|Environment\Deployment modeli |Kaynak modeli |Yerel modeli |Oluştur |
|---|---|---|---|
|Azure Service Fabric Mesh |Desteklenen |Desteklenmiyor|Desteklenmiyor |
|Diğer tüm ortamlardaki |Desteklenen (bazı kaynaklara sahip bir ortamda çalışmak için Önkoşullar) |Desteklenen |Sınırlı destek |

## <a name="next-steps"></a>Sonraki adımlar

Service Fabric Mesh hakkında daha fazla bilgi için genel bakış okuyun:
- [Service Fabric Mesh genel bakış](service-fabric-mesh-overview.md)
