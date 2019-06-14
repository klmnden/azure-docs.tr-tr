---
title: Microsoft azure'da soyut kapsayıcı güvenliği
description: Microsoft Azure teknik incelemesi kapsayıcı güvenliği için Özet.
author: TomShinder
ms.author: TomSh
ms.date: 09/05/2018
ms.topic: article
ms.service: security
ms.openlocfilehash: 0f892767bb7ab8202a27f298b1cb708d9802aaa4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60590091"
---
# <a name="container-security-in-microsoft-azure"></a>Microsoft azure'da kapsayıcı güvenliği
## <a name="abstract"></a>Özet

Kapsayıcı teknolojisini bulut bilgi işlem dünyada yapısal değişikliğe neden oluyor. Kapsayıcılar, böylece kaynakları daha verimli bir şekilde kullanarak bir işletim sistemi, tek bir örneği bir uygulamanın birden çok örneğini çalıştırmak mümkün kılar. Kapsayıcılar, kuruluşların tutarlılık ve esneklik sağlar. Uygulama masaüstünde geliştirilen, bir sanal makinede test ve üretim bulutta için dağıtılıp çünkü bunlar sürekli dağıtımı etkinleştirme. Kapsayıcıları, çevikliği, düzenli işlemler, ölçeklenebilirlik ve kaynak iyileştirmesi nedeniyle daha düşük maliyetleri sunar.

Birçok BT uzmanları, kapsayıcı teknolojisini görece olarak daha yeni olduğundan, bir üretim ortamında görünürlük ve kullanım yetersizliği ilgili güvenlik endişelerini sahip. Geliştirme takımları, genellikle en iyi güvenlik yöntemleri farkında değildir. Bu teknik incelemede, güvenlik operasyon ekibi ve kapsayıcı geliştirme ve Microsoft Azure platformunda dağıtımları güvenli hale getirmek için yaklaşım seçme içinde geliştiricilere yardımcı olabilir.

Bu yazıda, kapsayıcılar, kapsayıcı dağıtımı ve yönetimi ve yerel platform hizmetlerini açıklar. Ayrıca, kapsayıcı kullanımını Azure platformunda ortaya çıkan çalışma zamanı güvenlik sorunları açıklar. Şekiller ve örnekler bu kağıt üzerinde Docker kapsayıcı modeli ve Kubernetes kapsayıcı düzenleyicisi olarak odaklanır. Çoğu güvenlik önerileri, Microsoft iş ortaklarından Azure platformunda diğer kapsayıcı modelleri için de geçerlidir.

[Teknik incelemeyi indirin](https://azure.microsoft.com/mediahandler/files/resourcefiles/container-security-in-microsoft-azure/Open%20Container%20Security%20in%20Microsoft%20Azure.pdf)