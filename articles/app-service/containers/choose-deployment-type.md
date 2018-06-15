---
title: Azure uygulama hizmeti Linux dağıtımda - özel görüntü, çok kapsayıcı ya da yerleşik platform görüntüsü?  | Microsoft Docs
description: Uygulama hizmeti Linux'ta için özel Docker kapsayıcısı dağıtımı, birden çok kapsayıcı ve yerleşik uygulama çerçevesi arasında karar verme
keywords: Azure uygulama hizmeti, web uygulaması, linux, oss
services: app-service
documentationCenter: ''
authors: msangapu
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2018
ms.author: msangapu
ms.openlocfilehash: 012f78fc07f237e8ed532246c81a3c86bb6ab4ac
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33764351"
---
# <a name="custom-image-multi-container-or-built-in-platform-image"></a>Özel görüntü, çok kapsayıcı veya yerleşik platform görüntüsü?

[Uygulama hizmeti Linux'ta](app-service-linux-intro.md) uygulamanız Web'de yayımlanan alma için üç farklı yollarını sunar:

- **Özel görüntü dağıtımı**: "tüm dosyaları ve bağımlılıkları hazır Çalıştır paketi içeren bir Docker görüntüye uygulamanızı Dockerize".
- **Birden çok kapsayıcı dağıtım**: "uygulamanızı Docker Compose veya bir Kubernetes yapılandırma dosyası kullanarak birden çok kapsayıcı arasında Dockerize". Daha fazla bilgi için bkz: [çok kapsayıcı uygulama](#multi-container-apps-supportability).
- **Uygulama dağıtımı bir yerleşik platform görüntüsü ile**: ortak web uygulama çalışma zamanları ve bağımlılıklar düğümü ve PHP gibi bizim yerleşik platform görüntüleri içerir. Herhangi birini kullanmak [Azure uygulama hizmeti dağıtım yöntemleri](../app-service-deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) web uygulamanızın depolama birimine uygulamanızı dağıtmak ve çalıştırmak için bir yerleşik platform görüntüsü kullanın.

##<a name="which-method-is-right-for-your-app"></a>Hangi yöntemi uygulamanız için uygun hangisi? 

Dikkate alınması gereken birincil faktörleri şunlardır:

- **Docker kullanılabilirlik geliştirme iş akışınızda**: özel görüntü geliştirme Docker geliştirme iş akışının temel bilgi gerektirir. Bir web uygulaması için özel bir görüntü dağıtımı özel görüntünüzü Docker hub'a gibi deposu konağa yayımlanmasını gerektirir. Docker ile sahibiyseniz ve Docker görevleri yapı iş akışınız ekleyebilirsiniz ya da uygulamanızı Docker görüntü olarak zaten yayımlıyorsanız, özel bir görüntü neredeyse kesinlikle en iyi seçimdir.
- **Çok katmanlı mimarisi**: web uygulama katmanı ve yetenekleri çok kapsayıcısını kullanarak ayırmak için bir API katmanı gibi birden çok kapsayıcı dağıtma. 
- **Uygulama performansı**: gibi Redis önbelleği Katmanı'nı kullanarak çok kapsayıcı uygulamanızı performansı arttırır. Bunu başarmak için çok kapsayıcısı seçin.
- **Benzersiz çalışma zamanı gereksinimleri**: yerleşik platform görüntüleri çoğu web uygulamalarının ihtiyaçlarını karşılamak üzere tasarlanmıştır, ancak bunların özelleştirilebilirliğini sınırlıdır. Uygulamanızı benzersiz bağımlılıkları veya yerleşik görüntüleri yeteneğine nelerdir aşan diğer çalışma zamanı gereksinimleri olabilir.
- **Derleme gereksinimleri**: ile [sürekli dağıtım](../app-service-continuous-deployment.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json), uygulamanız hazır ve çalışır Azure üzerinde doğrudan kaynak kodundan alabilirsiniz. Dış bir yapı veya yayın işlem gereklidir. Ancak, derleme araçları içinde kullanılabilirliğini ve özelleştirilebilirliğini için bir sınır yoktur [Kudu](https://github.com/projectkudu/kudu/wiki) dağıtım altyapısı. Bağımlılıklar veya özel derleme mantığı için gereksinimleri arttıkça, uygulamanızın Kudu'nin özellikleri outgrow.
- **Disk okuma/yazma gereksinimleri**: tüm web uygulamaları, web içeriği için bir depolama birimi ayrılır. Azure Storage tarafından yedeklenen bu birimin takılı `/home` uygulamanın dosya sistemi içinde. Kapsayıcı filesystem dosyalarında aksine, bir uygulamanın tüm ölçek örneklerde içerik birimdeki dosyaları erişilebilir ve değişiklikler uygulama yeniden başlatmaları arasındaki korunur. Ancak, içerik biriminin disk gecikmesi yüksektir ve yerel kapsayıcı dosya sistemi ve erişim gecikmesi'den daha fazla değişken platformu yükseltmeleri, Planlanmamış kapalı kalma süresi ve ağ bağlantısı sorunları tarafından etkilenebilir. İçerik dosyaları ağır salt okunur erişim gerektiren uygulamalar görüntü dosya sistemi yerine içerik birim üzerindeki dosyaları yerleştirir özel görüntü dağıtımından yararlı.
- **Kaynak kullanımı yapı**: kaynağından bir uygulama dağıtıldığında, dağıtım betikleri Kudu kullandığı aynı App Service planı işlem ve depolama kaynaklarını çalışan uygulaması gibi çalıştırabilir. Daha fazla kaynak ya da istenen zamanından büyük uygulama dağıtımları tüketebilir. Özellikle, birçok dağıtım iş akışları yoğun disk etkinliği gibi etkinlik için optimize edilmemiş uygulama içerik birim oluşturun. Özel görüntü tüm uygulamanızın dosyaları ve bağımlılıkları için Azure ek dosya aktarımları veya dağıtım eylemleri için gerekli olan tek bir paket sunar.
- **Hızlı yineleme için gereken**: bir uygulama Dockerizing ek yapılandırma adımları gerektirir. Değişikliklerin etkili olabilmesi için her güncelleştirme deponuza yeni görüntünüzü göndermelisiniz. Bu güncelleştirmeler, ardından Azure ortamına alınır. Kaynak sunucudan dağıtma yerleşik kapsayıcıları birini uygulamanızın gereksinimlerini karşılıyorsa, daha hızlı geliştirme iş akışı sunabilir.

## <a name="multi-container-apps-supportability"></a>Birden çok kapsayıcı uygulamaları desteklenebilirlik

### <a name="supported-docker-compose-configuration-options"></a>Desteklenen Docker Compose yapılandırma seçenekleri
- komutu
- EntryPoint
- environment
- görüntü
- Bağlantı noktaları
- Yeniden başlatma
- Hizmetleri
- Birimleri

### <a name="unsupported-docker-compose-configuration-options"></a>Desteklenmeyen Docker Compose yapılandırma seçenekleri
- derleme (izin yok)
- depends_on (göz ardı)
- ağları (göz ardı)
- Gizli (göz ardı)
- 80 ve 8080 (göz ardı) dışındaki bağlantı noktaları

> [!NOTE]
> Açıkça çağrılan diğer seçenekleri de genel önizlemede göz ardı edilir.

### <a name="supported-kubernetes-configuration-options"></a>Desteklenen Kubernetes yapılandırma seçenekleri
- bağımsız değişken
- komutu
- containers
- görüntü
- ad
- Bağlantı noktaları
- belirtimi

> [!NOTE]
>Açıkça çağrılan diğer Kubernetes seçenekleri genel önizlemede desteklenmez.
>