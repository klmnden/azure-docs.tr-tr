---
title: "Azure uygulama hizmeti Linux dağıtımda - özel görüntü veya yerleşik platform görüntüsü?  | Microsoft Belgeleri"
description: "Uygulama hizmeti Linux'ta için yerleşik uygulama çerçevesi özel Docker kapsayıcısı dağıtım arasında karar verme"
keywords: "Azure uygulama hizmeti, web uygulaması, linux, oss"
services: app-service
documentationCenter: 
authors: nickwalk
manager: cfowler
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: nickwalk
ms.openlocfilehash: 4a04bba2375b5a107bc3cb8cdc1a75d037c50af6
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="custom-image-or-built-in-platform-image"></a>Özel görüntü veya yerleşik platform görüntüsü?

[Uygulama hizmeti Linux'ta](app-service-linux-intro.md) uygulamanız Web'de yayımlanan alma için iki farklı yollarını sunar:

- **Özel görüntü dağıtımı**: "tüm dosyaları ve bağımlılıkları hazır Çalıştır paketi içeren bir Docker görüntüye uygulamanızı Dockerize".
- **Uygulama dağıtımı bir yerleşik platform görüntüsü ile**: ortak web uygulama çalışma zamanları ve bağımlılıklar düğümü ve PHP gibi bizim yerleşik platform görüntüleri içerir. Herhangi birini kullanmak [Azure uygulama hizmeti dağıtım yöntemleri](../app-service-deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) web uygulamanızın depolama birimine uygulamanızı dağıtmak ve çalıştırmak için bir yerleşik platform görüntüsü kullanın.

Hangi yöntemi uygulamanız için uygun hangisi? Dikkate alınması gereken birincil faktörleri şunlardır:

- **Docker kullanılabilirlik geliştirme iş akışınızda**: özel görüntü geliştirme Docker geliştirme iş akışının temel bilgi gerektirir. Bir web uygulaması için özel bir görüntü dağıtımı özel görüntünüzü Docker hub'a gibi deposu konağa yayımlanmasını gerektirir. Docker ile sahibiyseniz ve Docker görevleri yapı iş akışınız ekleyebilirsiniz ya da uygulamanızı Docker görüntü olarak zaten yayımlıyorsanız, özel bir görüntü neredeyse kesinlikle en iyi seçimdir.
- **Benzersiz çalışma zamanı gereksinimleri**: yerleşik platform görüntüleri çoğu web uygulamalarının ihtiyaçlarını karşılamak üzere tasarlanmıştır, ancak bunların özelleştirilebilirliğini sınırlıdır. Uygulamanızı benzersiz bağımlılıkları veya yerleşik görüntüleri yeteneğine nelerdir aşan diğer çalışma zamanı gereksinimleri olabilir.
- **Derleme gereksinimleri**: ile [sürekli dağıtım](../app-service-continuous-deployment.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json), uygulamanız hazır ve çalışır Azure üzerinde doğrudan kaynak kodundan alabilirsiniz. Dış bir yapı veya yayın işlem gereklidir. Ancak, derleme araçları içinde kullanılabilirliğini ve özelleştirilebilirliğini için bir sınır yoktur [Kudu](https://github.com/projectkudu/kudu/wiki) dağıtım altyapısı. Bağımlılıklar veya özel derleme mantığı için gereksinimleri arttıkça, uygulamanızın Kudu'nin özellikleri outgrow.
- **Disk okuma/yazma gereksinimleri**: tüm web uygulamaları, web içeriği için bir depolama birimi ayrılır. Azure Storage tarafından yedeklenen bu birimin takılı `/home` uygulamanın dosya sistemi içinde. Kapsayıcı filesystem dosyalarında aksine, bir uygulamanın tüm ölçek örneklerde içerik birimdeki dosyaları erişilebilir ve değişiklikler uygulama yeniden başlatmaları arasındaki korunur. Ancak, içerik biriminin disk gecikmesi yüksektir ve yerel kapsayıcı dosya sistemi ve erişim gecikmesi'den daha fazla değişken platformu yükseltmeleri, Planlanmamış kapalı kalma süresi ve ağ bağlantısı sorunları tarafından etkilenebilir. İçerik dosyaları ağır salt okunur erişim gerektiren uygulamalar görüntü dosya sistemi yerine içerik birim üzerindeki dosyaları yerleştirir özel görüntü dağıtımından yararlı.
- **Kaynak kullanımı yapı**: kaynağından bir uygulama dağıtıldığında, dağıtım betikleri Kudu kullandığı aynı App Service planı işlem ve depolama kaynaklarını çalışan uygulaması gibi çalıştırabilir. Daha fazla kaynak ya da istenen zamanından büyük uygulama dağıtımları tüketebilir. Özellikle, birçok dağıtım iş akışları yoğun disk etkinliği gibi etkinlik için optimize edilmemiş uygulama içerik birim oluşturun. Özel görüntü tüm uygulamanızın dosyaları ve bağımlılıkları için Azure ek dosya aktarımları veya dağıtım eylemleri için gerekli olan tek bir paket sunar.
- **Hızlı yineleme için gereken**: bir uygulama Dockerizing ek yapılandırma adımları gerektirir. Değişikliklerin etkili olabilmesi için her güncelleştirme deponuza yeni görüntünüzü göndermelisiniz. Bu güncelleştirmeler, ardından Azure ortamına alınır. Kaynak sunucudan dağıtma yerleşik kapsayıcıları birini uygulamanızın gereksinimlerini karşılıyorsa, daha hızlı geliştirme iş akışı sunabilir.