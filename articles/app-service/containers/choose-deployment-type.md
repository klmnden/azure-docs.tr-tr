---
title: Özel görüntü, çok kapsayıcılı veya yerleşik görüntü - Azure App Service'e dağıtma | Microsoft Docs
description: Linux'ta App Service için özel Docker kapsayıcı dağıtımı, çok kapsayıcılı ve yerleşik uygulama çerçevesi arasında karar verme
keywords: Azure app service, web uygulaması, linux, oss
services: app-service
documentationCenter: ''
author: msangapu
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
ms.openlocfilehash: c8a700bcd2780ef7b0c7ad1fbb513d4b4febffcb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60849991"
---
# <a name="custom-image-multi-container-or-built-in-platform-image"></a>Özel görüntü, çok kapsayıcılı veya yerleşik platform görüntüsü?

[Linux üzerinde App Service'te](app-service-linux-intro.md) Web'de yayımlanan uygulamanızı edinmenin üç farklı yollar sunar:

- **Özel görüntü dağıtımı**: "Uygulamanızı tüm dosyaları ve çalıştırılmaya hazır paket bağımlılıkları içeren bir Docker görüntüsü docker kapsayıcılarında çalıştırın".
- **Çok kapsayıcılı dağıtım**: "Uygulamanızı bir Docker Compose veya Kubernetes yapılandırma dosyası kullanarak birden çok kapsayıcıya docker kapsayıcılarında çalıştırın".
- **Yerleşik platform görüntüsü ile uygulama dağıtımı**: Ortak web uygulaması çalışma zamanları ve bağımlılıkları, düğüm ve PHP gibi yerleşik platform görüntülerimizi içerir. Herhangi birini kullanan [Azure App Service dağıtım yöntemleri](../deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) uygulamanız için web uygulamanızın depolama dağıtmak ve çalıştırmak için bir yerleşik platform görüntüsü'i kullanın.

## <a name="which-method-is-right-for-your-app"></a>Uygulamanız için hangi yöntemi hangisi? 

Dikkate alınması gereken temel unsurlar şunlardır:

- **Docker'ın geliştirme iş akışınızın kullanılabilirlik**: Özel görüntü geliştirme Docker geliştirme iş akışının temel bilgi gerektirir. Bir web uygulamasına özel bir görüntü dağıtımı, özel görüntü Docker Hub gibi bir depo konağına yayımlanmasını gerektirir. Docker ile ilgili bilgi sahibi olduğunuz ve Docker görevleri için yapı iş ekleyebilirsiniz veya uygulamanızla bir Docker görüntüsü yayımlıyorsanız, özel bir görüntü olasılıkla en iyi seçenektir.
- **Çok katmanlı mimarinin**: Web uygulama katmanı ve çok kapsayıcılı kullanarak özellikleri ayırmak için bir API katmanı gibi birden çok kapsayıcı dağıtın. 
- **Uygulama performansı**: Redis gibi bir önbellek katmanı kullanarak çok Kapsayıcılı uygulamanızı performansını artırın. Bunu yapmanın çok kapsayıcıyı seçin.
- **Benzersiz bir çalışma zamanı gereksinimleri**: Yerleşik platform görüntüleri çoğu web uygulamalarının ihtiyaçlarını karşılamak için tasarlanan, ancak bunların sağlamadığından sınırlıdır. Uygulamanızı benzersiz bağımlılıkları veya yerleşik görüntüleri yeteneğine nelerdir aşan başka bir çalışma zamanı gereksinimleri olabilir.
- **Derleme gereksinimleri**: İle [sürekli dağıtım](../deploy-continuous-deployment.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json), uygulamanız çalışmaya Azure'da doğrudan kaynak koddan elde edebilirsiniz. Dış bir derleme veya yayın işlem gereklidir. Ancak, özelleştirmeyi ölçme ve derleme araçları içinde kullanılabilirlik için bir sınır yoktur [Kudu](https://github.com/projectkudu/kudu/wiki) dağıtım altyapısı. Bağımlılıklar veya özel yapı mantığı için gereksinimleri arttıkça uygulamanızın Kudu'nın özellikleri aşıyorsa.
- **Disk okuma/yazma gereksinimleri**: Tüm web uygulamaları, web içeriği için bir depolama birimi ayrılır. Azure Depolama tarafından desteklenen, bu birimin takılı `/home` uygulamanın dosya sistemi içinde. Kapsayıcı dosya sistemi dosyaları, farklı bir uygulamanın tüm ölçek örneklerde içerik birimdeki dosyalar erişilebilir olur ve değişiklikler uygulama yeniden başlatmaları arasındaki açık kalır. Ancak, içerik biriminin disk gecikmesi yüksektir ve yerel kapsayıcı dosya sistemi ve erişim gecikme değerinden daha fazla değişken platformu yükseltmeleri, Planlanmamış kapalı kalma süresi ve ağ bağlantısı sorunları etkilenebilir. İçerik dosyaları için ağır salt okunur erişim gerektiren uygulamaları, görüntü dosya sistemi yerine içerik birim üzerindeki dosyaları yerleştirir özel görüntü dağıtımından faydalanabilir.
- **Kaynak kullanımı derleme**: Kaynağından bir uygulama dağıtıldığında, dağıtım betikleri Kudu kullandığı aynı App Service planı işlem ve depolama kaynakları çalışan uygulamayı çalıştırın. Daha fazla kaynak ya da istenen saatten büyük uygulama dağıtımları tüketebilir. Özellikle, bu tür bir etkinlik için optimize edilmemiş uygulama içerik birim yoğun disk etkinlik birçok dağıtım iş akışları oluşturun. Özel bir görüntü, uygulamanızın dosyaları ve bağımlılıkları tümünün tek bir pakette ak dosya aktarımlarının veya dağıtım eylemleri gerek kalmaz Azure'a sunar.
- **Hızlı yineleme için gereksinim**: Bir uygulama dockerizing ek derleme adımları gerektirir. Değişikliklerin etkili olması için yeni görüntünüzü bir depoyla her bir güncelleştirme göndermeniz gerekir. Bu güncelleştirmeler, ardından Azure ortamı alınır. Yerleşik kapsayıcılardan birini uygulamanızın gereksinimlerini karşılıyorsa, daha hızlı bir geliştirme iş akışı kaynağından dağıtma sunabilir.
