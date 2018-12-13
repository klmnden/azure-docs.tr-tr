---
title: Özel görüntü, çok kapsayıcılı veya yerleşik görüntü - Azure App Service'e dağıtma | Microsoft Docs
description: Linux'ta App Service için özel Docker kapsayıcı dağıtımı, çok kapsayıcılı ve yerleşik uygulama çerçevesi arasında karar verme
keywords: Azure app service, web uygulaması, linux, oss
services: app-service
documentationCenter: ''
authors: msangapu
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2018
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: d53fc8b3971a1003b4f5d9b9e52f86ee73829cc2
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53315833"
---
# <a name="custom-image-multi-container-or-built-in-platform-image"></a>Özel görüntü, çok kapsayıcılı veya yerleşik platform görüntüsü?

[Linux üzerinde App Service'te](app-service-linux-intro.md) Web'de yayımlanan uygulamanızı edinmenin üç farklı yollar sunar:

- **Özel görüntü dağıtımı**: "Uygulamanızı tüm dosyaları ve çalıştırılmaya hazır paket bağımlılıkları içeren bir Docker görüntüsü docker kapsayıcılarında çalıştırın".
- **Çok kapsayıcılı dağıtım**: "Uygulamanızı bir Docker Compose veya Kubernetes yapılandırma dosyası kullanarak birden çok kapsayıcıya docker kapsayıcılarında çalıştırın". Daha fazla bilgi için [çok kapsayıcılı uygulama](#multi-container-apps-supportability).
- **Yerleşik platform görüntüsü ile uygulama dağıtımı**: Ortak web uygulaması çalışma zamanları ve bağımlılıkları, düğüm ve PHP gibi yerleşik platform görüntülerimizi içerir. Herhangi birini kullanan [Azure App Service dağıtım yöntemleri](../app-service-deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) uygulamanız için web uygulamanızın depolama dağıtmak ve çalıştırmak için bir yerleşik platform görüntüsü'i kullanın.

## <a name="which-method-is-right-for-your-app"></a>Uygulamanız için hangi yöntemi hangisi? 

Dikkate alınması gereken temel unsurlar şunlardır:

- **Docker'ın geliştirme iş akışınızın kullanılabilirlik**: Özel görüntü geliştirme Docker geliştirme iş akışının temel bilgi gerektirir. Bir web uygulamasına özel bir görüntü dağıtımı, özel görüntü Docker Hub gibi bir depo konağına yayımlanmasını gerektirir. Docker ile ilgili bilgi sahibi olduğunuz ve Docker görevleri için yapı iş ekleyebilirsiniz veya uygulamanızla bir Docker görüntüsü yayımlıyorsanız, özel bir görüntü olasılıkla en iyi seçenektir.
- **Çok katmanlı mimarinin**: Web uygulama katmanı ve çok kapsayıcılı kullanarak özellikleri ayırmak için bir API katmanı gibi birden çok kapsayıcı dağıtın. 
- **Uygulama performansı**: Redis gibi bir önbellek katmanı kullanarak çok Kapsayıcılı uygulamanızı performansını artırın. Bunu yapmanın çok kapsayıcıyı seçin.
- **Benzersiz bir çalışma zamanı gereksinimleri**: Yerleşik platform görüntüleri çoğu web uygulamalarının ihtiyaçlarını karşılamak için tasarlanan, ancak bunların sağlamadığından sınırlıdır. Uygulamanızı benzersiz bağımlılıkları veya yerleşik görüntüleri yeteneğine nelerdir aşan başka bir çalışma zamanı gereksinimleri olabilir.
- **Derleme gereksinimleri**: İle [sürekli dağıtım](../app-service-continuous-deployment.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json), uygulamanız çalışmaya Azure'da doğrudan kaynak koddan elde edebilirsiniz. Dış bir derleme veya yayın işlem gereklidir. Ancak, özelleştirmeyi ölçme ve derleme araçları içinde kullanılabilirlik için bir sınır yoktur [Kudu](https://github.com/projectkudu/kudu/wiki) dağıtım altyapısı. Bağımlılıklar veya özel yapı mantığı için gereksinimleri arttıkça uygulamanızın Kudu'nın özellikleri aşıyorsa.
- **Disk okuma/yazma gereksinimleri**: Tüm web uygulamaları, web içeriği için bir depolama birimi ayrılır. Azure Depolama tarafından desteklenen, bu birimin takılı `/home` uygulamanın dosya sistemi içinde. Kapsayıcı dosya sistemi dosyaları, farklı bir uygulamanın tüm ölçek örneklerde içerik birimdeki dosyalar erişilebilir olur ve değişiklikler uygulama yeniden başlatmaları arasındaki açık kalır. Ancak, içerik biriminin disk gecikmesi yüksektir ve yerel kapsayıcı dosya sistemi ve erişim gecikme değerinden daha fazla değişken platformu yükseltmeleri, Planlanmamış kapalı kalma süresi ve ağ bağlantısı sorunları etkilenebilir. İçerik dosyaları için ağır salt okunur erişim gerektiren uygulamaları, görüntü dosya sistemi yerine içerik birim üzerindeki dosyaları yerleştirir özel görüntü dağıtımından faydalanabilir.
- **Kaynak kullanımı derleme**: Kaynağından bir uygulama dağıtıldığında, dağıtım betikleri Kudu kullandığı aynı App Service planı işlem ve depolama kaynakları çalışan uygulamayı çalıştırın. Daha fazla kaynak ya da istenen saatten büyük uygulama dağıtımları tüketebilir. Özellikle, bu tür bir etkinlik için optimize edilmemiş uygulama içerik birim yoğun disk etkinlik birçok dağıtım iş akışları oluşturun. Özel bir görüntü, uygulamanızın dosyaları ve bağımlılıkları tümünün tek bir pakette ak dosya aktarımlarının veya dağıtım eylemleri gerek kalmaz Azure'a sunar.
- **Hızlı yineleme için gereksinim**: Bir uygulama dockerizing ek derleme adımları gerektirir. Değişikliklerin etkili olması için yeni görüntünüzü bir depoyla her bir güncelleştirme göndermeniz gerekir. Bu güncelleştirmeler, ardından Azure ortamı alınır. Yerleşik kapsayıcılardan birini uygulamanızın gereksinimlerini karşılıyorsa, daha hızlı bir geliştirme iş akışı kaynağından dağıtma sunabilir.

## <a name="multi-container-apps-supportability"></a>Çok kapsayıcılı uygulamalar desteklenebilirliği

### <a name="supported-docker-compose-configuration-options"></a>Docker Compose desteklenen yapılandırma seçenekleri
- command
- entrypoint
- environment
- image
- ports
- restart
- services
- volumes

### <a name="unsupported-docker-compose-configuration-options"></a>Desteklenmeyen Docker Compose yapılandırma seçenekleri
- build (izin verilmez)
- depends_on (yoksayılır)
- networks (yoksayılır)
- secrets (yoksayılır)
- 80 ve 8080 (yoksayıldı) dışındaki bağlantı noktaları

> [!NOTE]
> Açıkça çağrılmayan diğer tüm seçenekler de Genel Önizleme'de yoksayılır.

### <a name="supported-kubernetes-configuration-options"></a>Desteklenen Kubernetes yapılandırma seçenekleri
- args
- command
- containers
- image
- ad
- ports
- spec

> [!NOTE]
>Açıkça çağrılmayan diğer tüm Kubernetes seçenekleri de Genel Önizleme'de desteklenmez.
>
