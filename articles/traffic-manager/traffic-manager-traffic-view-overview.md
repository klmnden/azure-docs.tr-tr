---
title: "Trafiği Azure trafik Yöneticisi görünümü | Microsoft Docs"
description: "Trafik Yöneticisi trafiği görünüm giriş"
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/19/2017
ms.author: kumud
ms.custom: 
ms.openlocfilehash: 0a07ff578c6afeedc6f3806d52bfe5aef6945c04
ms.sourcegitcommit: bd0d3ae20773fc87b19dd7f9542f3960211495f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2017
---
# <a name="traffic-manager-traffic-view"></a>Trafik Yöneticisi trafiği görünümü

>[!NOTE]
>Trafik Yöneticisi'nde trafiği görünümü özelliği genel önizlemede ve genel kullanılabilirlik özellikleri sürüm gibi aynı düzeyde kullanılabilirlik ve güvenilirlik olmayabilir. Özellik desteklenmiyor, yetenekleri kısıtlı ve tüm Azure konumlarda kullanılamayabilir. Kullanılabilirlik ve bu özellik durumunu en güncel bildirimleri için denetleme [Azure trafik Yöneticisi'ni güncelleştirir](https://azure.microsoft.com/updates/?product=traffic-manager) sayfası.

Trafik Yöneticisi profili oluşturulduğunda ayarlanmış yönlendirme yöntemini esas alarak sağlıklı uç noktalar için son kullanıcılarınızın yönlendirilmiş böylece DNS düzeyi yönlendirme sağlar. Bu trafik Yöneticisi, kullanıcı tabanları (düzeyinde bir DNs Çözümleyicisi ayrıntı düzeyi) ve bunların trafiğini desen bir görünüm sağlar. Trafik görünüm etkinleştirdiğinizde, bu bilgileri ile eyleme dönüştürülebilir Öngörüler sağlamak için işlenir. 

Trafik görünümünü kullanarak şunları yapabilirsiniz:
- (bir yerel DNS Çözümleyicisi düzeyi ayrıntı düzeyi kadar), kullanıcı temellerine bulunduğu anlayın.
- (Azure Traffic Manager tarafından işlenen DNS sorgularını olarak gözlenen) trafiği hacmi görüntülemek bu bölgelerden kaynaklanan.
-  Bu kullanıcılar tarafından yaşadı temsilcisi gecikme süresi nedir içine Öngörüler alın.
- Azure bölgeleri uç noktaları sahip olduğu için bu kullanıcı temellerine her derin Dalış belirli bir trafik düzenleri içine. 

Örneğin, hangi bölgelerin çok sayıda trafiği sahip ancak daha yüksek gecikme yaşar anlamak için trafiği görünümü kullanabilirsiniz. Ardından bu kullanıcıların daha düşük gecikme süresi deneyimi böylece, yeni Azure bölgeleri ayak izini genişletmeye planlamak için bu bilgileri kullanabilirsiniz.

## <a name="how-traffic-view-works"></a>Trafik görünümü nasıl çalışır?

Trafik Yöneticisi bu özelliği etkinleştirilmiş bir profili karşı son yedi gün içinde alınan gelen sorguları bakmak sağlayarak trafiği görünüm çalışır. Bu bilgilerden, ardından kullanıcıları konumunu bir gösterimi kullanılan DNS Çözümleyicisi kaynak IP ayıklar. Bunlar ardından birlikte trafik Yöneticisi tarafından tutulan IP adreslerinin coğrafi bilgileri kullanarak kullanıcı temel bölgeler oluşturmak için bir DNS Çözümleyicisi düzeyi ayrıntı düzeyi adresindeki gruplandırılır. Trafik Yöneticisi ardından sorgu yönlendirildi ve bu bölgelerdeki kullanıcılar için bir trafik akış eşleme oluşturur Azure bölgeleri bakar.
Sonraki adımda, trafik Yöneticisi Azure bölgesi eşleme bu bölgelerdeki kullanıcılar tarafından yaşadı ortalama gecikme süresi anlamak farklı son kullanıcı ağlar için tutar ağ Intelligence gecikme tablolar ile kullanıcı temel bölgesine karşılık gelen zaman Azure bölgeleri bağlanılıyor. Bu hesaplamalar, sırasında tabloda verilmiştir bir gerçekleştiriliyordu size görüntülenmeden önce yerel DNS Çözümleyicisi IP. Bu bilgileri bir analytics iş akışı tercih ettiğiniz işleyebilmesi için bu bilgileri bir CSV dosyası olarak karşıdan yükleyebilirsiniz.
Trafik görünümü kullandığınızda, sunulan öngörüleri oluşturmak için kullanılan veri noktalarının sayısına göre faturalandırılır. Şu anda kullanılan tek veri noktası Traffic Manager profilinize karşı alınan sorguları türüdür. Fiyatlandırma hakkında daha fazla ayrıntı için ziyaret [trafik Yöneticisi fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/traffic-manager/).


## <a name="next-steps"></a>Sonraki adımlar

- Bilgi [trafik Yöneticisi nasıl çalışır?](traffic-manager-overview.md)
- Daha fazla bilgi edinmek [trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md) Traffic Manager tarafından desteklenen
- Bilgi edinmek için nasıl [bir Traffic Manager profili oluşturma](traffic-manager-create-profile.md)

