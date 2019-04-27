---
title: Azure Service Fabric kafes terminolojisi | Microsoft Docs
description: Azure Service Fabric Mesh için sık kullanılan terimler hakkında öğrenin.
services: service-fabric-mesh
keywords: ''
author: dkkapur
ms.author: dekapur
ms.date: 11/28/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 2d2661593ba3d9be2755d81803c8e248a2f7d0e1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60810594"
---
# <a name="service-fabric-mesh-terminology"></a>Service Fabric Mesh terminolojisi

Azure Service Fabric Mesh, geliştiricilerin sanal makineleri, depolama alanını veya ağ bileşenlerini yönetmeden mikro hizmet uygulamaları dağıtmasını sağlayan tam olarak yönetilen bir hizmettir. Bu makalede belgelerinde kullanılan terimler anlamak için Azure Service Fabric Mesh tarafından kullanılan terimler açıklanmaktadır.

## <a name="service-fabric"></a>Service Fabric

[Service Fabric](/azure/service-fabric/) , ölçeklenebilir ve güvenilir mikro Hizmetleri paketlemeyi, dağıtma ve kolay bir açık kaynak dağıtılmış sistemler platformudur. Service Fabric Service Fabric Mesh güç katan düzenleyicisidir. Service Fabric, nasıl yapı ve mikro hizmetler uygulamalarınızı çalıştırmak için seçenekler sağlar. Kendi hizmetlerinizi yazmanızı ve birden çok ortam seçenekleri uygulamayı çalıştırmak hedef konumu seçin, herhangi bir çerçeveyi kullanabilirsiniz.

## <a name="application-and-service-concepts"></a>Uygulama ve hizmeti kavramları

**Service Fabric Mesh uygulaması**: Service Fabric uygulamaları Mesh açıklanır [kaynak modeli](/azure/service-fabric-mesh/service-fabric-mesh-service-fabric-resources) (YAML ve JSON kaynak dosyaları) ve Service Fabric çalıştırıldığı herhangi bir ortama dağıtılabilir.

**Service Fabric yerel uygulaması**: Service Fabric yerel uygulamaları, tarafından açıklanmıştır [yerel uygulama modeli](/azure/service-fabric/service-fabric-application-model) (XML tabanlı bir uygulama ve hizmet bildirimleri).  Service Fabric yerel uygulamaları, Service Fabric Mesh içinde çalıştırılamaz.

**Uygulama**: Bir Service Fabric Mesh uygulaması dağıtım, sürüm ve Mesh uygulaması ömrünü birimidir. Her uygulama örneği yaşam döngüsü bağımsız olarak yönetilebilir.  Uygulamalar, bir veya daha fazla hizmet kod paketleri ve ayarları oluşur. Azure kaynak modeli (RM) şemayı kullanarak bir uygulama ile tanımlanır.  Hizmetleri özellikleri RM şablonunda Uygulama kaynağı olarak açıklanmıştır.  Ağ ve uygulama tarafından kullanılan birimleri uygulama tarafından başvurulur.  Bir uygulama oluştururken, uygulama, hizmetler, ağ ve birimlerin Service Fabric kaynak modelini kullanarak modellenir.

**Hizmet**: Bir uygulamada bir hizmeti temsil eden bir mikro hizmet ve eksiksiz ve tek başına bir işlevi gerçekleştirir. Her hizmet kodu paket ile ilişkili kapsayıcı görüntüsünü çalıştırmak için gereken her şeyi açıklayan bir veya daha fazla, kod paketleri oluşur.  Bir uygulama hizmeti çoğaltma sayısını giriş ve çıkış ölçeklendirilebilir.

**Kod paketi**: Kod paketleri aşağıdakiler dahil olmak üzere kod paket ile ilişkili kapsayıcı görüntüsünü çalıştırmak için gereken her şeyi açıklanmaktadır:

* Kapsayıcı adı, sürümü ve kayıt defteri
* Her kapsayıcı için gerekli CPU ve bellek kaynakları
* Ağ uç noktaları
* Ayrı birim kaynağına başvuran kapsayıcısında bağlamak için birimler.

## <a name="deployment-and-application-models"></a>Dağıtım ve uygulama modelleri 

Hizmetlerinizi dağıtmak için nasıl çalıştırılacağını tanımlamak gerekir. Service Fabric, üç farklı dağıtım modelleri destekler:

### <a name="resource-model"></a>Kaynak modeli
Service Fabric kaynakları, Service Fabric için ayrı ayrı dağıtılabilen herhangi bir şey olduğunu; uygulamalar, hizmetler, ağlar ve birimler dahil olmak üzere. Kaynakları bir küme uç noktasına dağıtılabilir bir JSON dosyası kullanılarak tanımlanır.  Service Fabric Mesh için Azure kaynak Model şeması kullanılır. Bir YAML dosyası şeması, daha kolay tanım dosyalarını yazmak için de kullanılabilir. Kaynakları, Service Fabric çalıştıran herhangi bir dağıtılabilir. Kaynak modeli, Service Fabric uygulamalarınızı açıklamak için basit bir yoldur. Kendi ana basit dağıtım ve kapsayıcılı hizmetlerin yönetimini sağlamaktır. Daha fazla bilgi edinmek için [Service Fabric kaynak modeli giriş](/azure/service-fabric-mesh/service-fabric-mesh-service-fabric-resources).

### <a name="native-model"></a>Yerel modeli
Yerel uygulama modeli için Service Fabric uygulamalarınızı tam alt düzey erişim sağlar. Uygulamalar ve hizmetler, XML bildirim dosyalarında kayıtlı türleri olarak tanımlanır.

Yerel modeli ve küme yönetimi API'leri C# ve Java Service Fabric çalışma zamanı API erişimi sağlayan Reliable Services framework destekler. Yerel modeli rastgele kapsayıcıları ve yürütülebilir dosyalar da destekler.

Yerel modeli, Service Fabric Mesh ortamda desteklenmiyor.  Daha fazla bilgi için [programlama modeline genel bakış](/azure/service-fabric/service-fabric-choose-framework).

### <a name="docker-compose"></a>Docker Compose 
[Docker Compose](https://docs.docker.com/compose/) Docker projesindeki bir parçasıdır. Service Fabric, Docker Compose modelini kullanarak uygulama dağıtmak için sınırlı destek sağlar.

## <a name="environments"></a>Ortamlar

Service Fabric birkaç farklı hizmet ve ürünleri temel alan bir açık kaynak platformu teknolojisidir. Microsoft aşağıdaki seçenekleri sağlar:

 - **Service Fabric kafes**: Microsoft Azure'da Service Fabric uygulamaları çalıştırmak için tam olarak yönetilen bir hizmet.
 - **Azure Service Fabric'e**: Azure Service Fabric kümesi teklifi barındırılan. Bu, Service Fabric ile birlikte Service Fabric Küme yükseltme ve yapılandırma yönetimi, Azure altyapı arasında tümleştirme sağlar.
 - **Service Fabric tek başına**: Yükleme ve yapılandırma araçları kümesi [herhangi bir Service Fabric kümelerini dağıtmayı](/azure/service-fabric/service-fabric-deploy-anywhere) (şirket içinde veya diğer bulut sağlayıcılarına). Azure tarafından yönetilen değil.
 - **Service Fabric geliştirme kümesi**: Bir yerel geliştirme deneyimi, Windows, Linux veya Mac için Service Fabric uygulamaları geliştirilmesini sağlar.

## <a name="environment-framework-and-deployment-model-support-matrix"></a>Ortam, framework ve dağıtım modeline destek matrisi
Farklı ortamlar, farklı çerçeveler ve dağıtım modelleri için destek düzeyine sahip. Aşağıdaki tabloda, desteklenen çerçevesi ve dağıtım modeli birleşimlerini açıklar.

| Uygulama türü | Tarafından açıklanan | Azure Service Fabric Mesh | Azure Service Fabric kümeleri (herhangi bir işletim sistemi)| Yerel küme | Tek başına küme |
|---|---|---|---|---|---|
| Service Fabric kafes uygulamaları | Kaynak Modeli (YAML & JSON) | Desteklenen |Desteklenmiyor | Windows - desteklenir, Linux ve Mac desteklenmeyen | Desteklenen Windows değil |
|Service Fabric yerel uygulamaları | Yerel uygulama modelini (XML) | Desteklenmiyor| Desteklenen|Desteklenen|Windows-desteklenir|

Aşağıdaki tabloda, farklı uygulama modelleri ve bunlar için Service Fabric karşı var olan araçlar açıklanmaktadır.

| Uygulama türü | Tarafından açıklanan | Visual Studio | Eclipse | SFCTL | AZ CLI | PowerShell|
|---|---|---|---|---|---|---|
| Service Fabric kafes uygulamaları | Kaynak Modeli (YAML & JSON) | VS 2017 |Desteklenmiyor |Desteklenmiyor | Destekleniyor - yalnızca ağ ortamı | Desteklenmiyor|
|Service Fabric yerel uygulamaları | Yerel uygulama modelini (XML) | VS 2017 ve VS 2015| Desteklenen|Desteklenen|Desteklenen|Desteklenen|

## <a name="next-steps"></a>Sonraki adımlar

Service Fabric Mesh hakkında daha fazla bilgi edinmek için [Service Fabric Mesh’e genel bakış](service-fabric-mesh-overview.md) makalesini okuyun.

[Sık sorulan sorulara](service-fabric-mesh-faq.md) yanıtlar bulun.
