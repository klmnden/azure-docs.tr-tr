---
title: İçinde Azure Traffic Manager gerçek kullanıcı ölçümleri
description: Gerçek kullanıcı ölçümleri, Traffic Manager giriş
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/16/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: fd37ef739522955ae8227db39a41aecf199d65c3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60329590"
---
# <a name="traffic-manager-real-user-measurements-overview"></a>Traffic Manager gerçek kullanıcı ölçümleri'ne genel bakış

Performans yönlendirme yöntemini kullanmak için bir Traffic Manager profili ayarladığınızda, hizmetin DNS sorgu isteği nereden geldiğini ve bu istek sahipleri en düşük gecikme süresi sağlayan bir Azure bölgesine doğrudan yönlendirme kararlarını verir arar. Bu, farklı son kullanıcı ağlar için Traffic Manager tutar ağ gecikme süresi bilgilerinin yararlanarak gerçekleştirilir.

Sahip Traffic Manager göz önünde bulundurun, bilgilerini de yönlendirme kararları verirken ve ağ gecikme ölçümlerini Azure bölgeleri için son kullanıcılarınızın kullanın, istemci uygulamalarından ölçmek gerçek kullanıcı ölçümleri sağlar. Gerçek kullanıcı ölçümleri kullanılacak seçerek, son kullanıcılarınızın bulunduğu bu ağlardan gelen istekler için yönlendirme doğruluğunu artırabilirsiniz. 

## <a name="how-real-user-measurements-work"></a>Gerçek kullanıcı ölçümleri nasıl çalışır?

Gerçek kullanıcı ölçümleri, bunlar kullanılır son kullanıcı ağlardan görülen Azure bölgeleri için istemci uygulamaları ölçü gecikmeniz sağlayarak çalışır. Kullanıcılar tarafından farklı konumlarda (örneğin, Kuzey Amerika bölgeleri için) üzerinden erişilen bir web sayfası varsa, örneğin, gerçek kullanıcı ölçümleri performans yönlendirme yöntemini, en iyi Azure bölgesi almak için kullanabileceğiniz sunucunuzun Uygulama barındırılır.

Web sayfaları'nda (benzersiz bir anahtar da ile) bir Azure sağlanan JavaScript ekleyerek başlar. Her bir kullanıcı, bir Web sitesini ziyaret eden, yaptıktan sonra JavaScript, Traffic Manager'ın Azure bölgelerini ölçme bilgilerini almak için sorgular. Hizmet, ardından bu bölgeler art arda yükleyerek bu Azure bölgelerinde barındırılan ve gecikme süresini saat arasındaki belirtmeye tek pikselli bir görüntü istek gönderildiği ölçü ve ne zaman ilk baytı alındığı zamanı komut dosyasına bir uç nokta kümesine yapılandırmayı döndürür. . Bu ölçümleri, Traffic Manager hizmeti için geri raporlanır.

Zaman içinde birçok kez böyle ve birçok ağlar, ağ gecikme süresi özellikleri hakkında daha doğru bilgi başlama Traffic Manager önde gelen son kullanıcılarınızın bulunur. Bu bilgiler, Traffic Manager tarafından verilen yönlendirme kararlarını dahil edilecek alma başlatır. Sonuç olarak, bunu artırılmış doğruluk gönderilen gerçek kullanıcı ölçümleri temel alınarak bu kararları doğurur.

Gerçek kullanıcı ölçümleri kullandığınızda, Traffic Manager'a gönderilen ölçülerin sayısına göre faturalandırılır. Fiyatlandırma hakkında ayrıntılı bilgi için ziyaret [Traffic Manager fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/traffic-manager/).

## <a name="next-steps"></a>Sonraki adımlar
- Nasıl kullanacağınızı öğrenin [web sayfaları ile gerçek kullanıcı ölçümleri](traffic-manager-create-rum-web-pages.md)
- Bilgi [Traffic Manager nasıl çalışır?](traffic-manager-overview.md)
- Daha fazla bilgi edinin [Mobile Center](https://docs.microsoft.com/mobile-center/)
- Daha fazla bilgi edinin [trafik yönlendirme yöntemlerini](traffic-manager-routing-methods.md) Traffic Manager tarafından desteklenen
- Bilgi edinmek için nasıl [Traffic Manager profili oluşturma](traffic-manager-create-profile.md)

