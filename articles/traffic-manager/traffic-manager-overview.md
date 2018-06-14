---
title: Trafik Yöneticisi nedir | Microsoft Docs
description: Bu makalede, trafik Yöneticisi nedir ve uygulamanız için doğru trafik yönlendirme seçim olup anlamanıza yardımcı olur
services: traffic-manager
documentationcenter: ''
author: kumudd
manager: timlt
editor: ''
ms.assetid: 75d5ff9a-f4b9-4b05-af32-700e7bdfea5a
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/15/2017
ms.author: kumud
ms.openlocfilehash: 50d7f14d0d4234ee98d8a46e903b5f916cb02fab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23877058"
---
# <a name="overview-of-traffic-manager"></a>Traffic Manager'a Genel Bakış

Microsoft Azure trafik Yöneticisi, farklı veri merkezlerinde bulunan hizmet uç noktaları için kullanıcı trafiğinin dağıtımını denetlemenize olanak sağlar. Trafik Yöneticisi tarafından desteklenen hizmet uç noktaları ve bulut hizmetlerini Azure VM'ler, Web uygulamaları içerir. Traffic Manager’ı harici, Azure dışı uç noktalar için de kullanabilirsiniz.

Trafik yöneticisi etki alanı adı sistemi (DNS) istemci isteklerini trafik yönlendirme yöntemini ve uç noktaları durumunu göre en uygun uç noktasına yönlendirmek için kullanır. Trafik Yöneticisi sağlayan bir dizi [trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md) ve [uç nokta izleme seçenekleri](traffic-manager-monitoring.md) farklı uygulama gereksinimlerini ve otomatik yük devretme modelleri uyacak şekilde. Trafik Yöneticisi dahil tüm bir Azure bölgesi başarısızlığını hatalarına karşı esnektir.

## <a name="traffic-manager-benefits"></a>Trafik Yöneticisi avantajları

Trafik Yöneticisi yardımcı olabilir:

* **Kritik uygulamaların kullanılabilirliğini artırın**

    Trafik Yöneticisi uç noktalarınızı izleme ve bir uç nokta azaldığında otomatik yük devretme sağlayarak, uygulamalar için yüksek kullanılabilirlik sağlar.

* **Yüksek performanslı uygulamalar için yanıt hızını artırmak**

    Azure bulut Hizmetleri veya Web siteleri tüm dünyada bulunan veri merkezlerinde çalıştırmanıza olanak sağlar. Trafik Yöneticisi uç noktası trafiği ile en düşük ağ gecikmesini istemcisi yönlendirerek uygulama yanıt hızını artırır.

* **Kapalı kalma süresi olmadan hizmet bakımı gerçekleştirme**

    Uygulamalarınızın kapalı kalma süresi olmadan planlı bakım işlemleri gerçekleştirebilir. Bakım işlemi devam ederken trafik Yöneticisi trafiğini alternatif Uç noktalara yönlendirir.

* **Şirket içi ve bulut tabanlı uygulamalar birleştirin**

    Trafik Yöneticisi dış destekler, karma ile kullanılmak üzere etkinleştirmeden Azure olmayan uç Bulut ve "veri bloğu buluta," "geçirmek buluta," dahil olmak üzere dağıtımları ve "Yük devretme-bulut" senaryoları şirket.

* **Büyük ve karmaşık dağıtımlar için trafiği dağıtmak**

    Kullanarak [iç içe trafik Yöneticisi profillerine](traffic-manager-nested-profiles.md), trafik yönlendirme yöntemleri, daha büyük ve daha karmaşık dağıtımlar ihtiyaçlarını desteklemek için esnek ve Gelişmiş kurallar oluşturmak için birleştirilebilir.

## <a name="how-traffic-manager-works"></a>Traffic Manager nasıl çalışır?

Azure trafik Yöneticisi, uygulama uç noktalar arasında trafiğinin dağıtımını denetlemenize olanak sağlar. Bir uç nokta içinde veya Azure dışında barındırılan tüm Internet'e hizmetidir.

Trafik Yöneticisi iki temel fayda sağlar:

1. Bazı göre trafiğinin dağıtımını [trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md)
2. [Sürekli uç noktası durumunu izleme](traffic-manager-monitoring.md) ve uç noktaları başarısız olduğunda otomatik yük devretme

Bir istemci bir hizmete bağlanma girişiminde bulunduğunda, hizmetin DNS adını bir IP adresi ilk çözmeniz gerekir. İstemci ardından hizmete erişmek için bu IP adresine bağlanır.

**Anlamak için en önemli nokta trafik Yöneticisi DNS düzeyinde çalışmasıdır.**  Trafik Yöneticisi DNS istemcileri belirli hizmet uç noktalarına trafik yönlendirme yöntemini kurallara göre yönlendirmek için kullanır. İstemcileri seçili uç noktasına bağlanmak **doğrudan**. Trafik Yöneticisi, bir proxy veya ağ geçidi değil. Trafik Yöneticisi, istemci ile hizmet arasında geçen trafiğine görmez.

### <a name="traffic-manager-example"></a>Trafik Yöneticisi örneği

Contoso Corp yeni bir iş ortağı portalı geliştirmiştir. Bu portalı https://partners.contoso.com/login.aspx URL'sidir. Uygulama Azure üç bölgelerde barındırılır. Kullanılabilirlik geliştirmek ve genel performansını en üst düzeye çıkarmak için trafik Yöneticisi en yakın kullanılabilir uç nokta için istemci trafiğini dağıtmak için kullanırlar.

Bu yapılandırma elde etmek için bunlar aşağıdaki adımları tamamlayın:

1. Kendi hizmet üç örneği dağıtın. Bu dağıtımlar DNS adları 'contoso-us.cloudapp .net', 'contoso-eu.cloudapp .net' ve 'contoso-asia.cloudapp .net' dir.
2. 'Contoso.trafficmanager.net' adlı bir trafik Yöneticisi profili oluşturun ve üç uç noktalar arasında 'Performans' trafik yönlendirme yöntemini kullanmak üzere yapılandırın.
* Kullanıcıların gösterim etki alanı adı, '' bir DNS CNAME kaydı kullanılarak olan contoso.trafficmanager.NET'e', yönlendirmek için partners.contoso.com', yapılandırın.

![Trafik Yöneticisi DNS yapılandırması][1]

> [!NOTE]
> Bir gösterim etki alanını Azure Traffic Manager ile kullanırken, trafik yöneticisi etki alanı adınızı gösterim etki alanı adınızı işaret etmek için bir CNAME kullanmanız gerekir. DNS standartlarında bir CNAME bir etki alanı 'tepesindeki' (veya kök) oluşturmanıza izin vermez. Bu nedenle, '('naked' etki alanı olarak da adlandırılır) contoso.com' için bir CNAME oluşturamazsınız. Yalnızca altında "contoso.com", "www.contoso.com" gibi bir etki alanı için bir CNAME oluşturabilirsiniz. Bu sınırlamaya geçici bir çözüm için "www.contoso.com" gibi bir diğer ad için "contoso.com" için doğrudan isteklere basit bir HTTP yeniden yönlendirme kullanmanızı öneririz.

### <a name="how-clients-connect-using-traffic-manager"></a>Trafik Yöneticisi'ni kullanarak istemcilerin nasıl bağlanacağını

Bir istemci sayfa https://partners.contoso.com/login.aspx istediğinde önceki örnekten devam etmeden, istemci DNS adını çözümlemek ve bağlantı kurmak için aşağıdaki adımları gerçekleştirir:

![Trafik Yöneticisi'ni kullanarak bağlantı kurma][2]

1. İstemci adı 'partners.contoso.com' çözümlemek için yapılandırılan yinelemeli DNS hizmeti için bir DNS sorgusu gönderir. 'Yerel DNS' hizmet olarak da adlandırılan bir yinelemeli DNS hizmeti DNS etki alanı doğrudan barındırmıyor. Bunun yerine, istemciyi çeşitli yetkili DNS hizmetleri, DNS adını çözümlemek için gerekli Internet üzerinden iletişim kurmasını iş off-loads.
2. DNS adı çözümlemek için özyinelemeli DNS hizmeti "contoso.com" etki alanı için ad sunucularını bulur. Ardından 'partners.contoso.com' DNS kaydı istemek için bu ad sunucularıyla iletişim kurar. Contoso.com DNS sunucuları için contoso.trafficmanager.net işaret eden bir CNAME kaydı döndürür.
3. Ardından, özyinelemeli DNS hizmeti Azure trafik Yöneticisi hizmeti tarafından sağlanan 'trafficmanager.net' etki alanı için ad sunucularını bulur. Ardından bu DNS sunucularına 'contoso.trafficmanager.net' DNS kaydı için bir istek gönderir.
4. Trafik Yöneticisi ad sunucuları isteği alır. Bunlar dayalı bir uç nokta seçin:

    - Her bitiş noktasıyla yapılandırılmış durumu (devre dışı uç noktaları alınmadı)
    - Trafik Yöneticisi sistem tarafından belirlenen her bitiş geçerli durumunu denetler. Daha fazla bilgi için bkz: [trafik Yöneticisi uç noktası izleme](traffic-manager-monitoring.md).
    - Seçilen trafik yönlendirme yöntemi. Daha fazla bilgi için bkz: [Traffic Manager yönlendirme yöntemleri](traffic-manager-routing-methods.md).

5. Seçilen uç nokta başka bir DNS CNAME kaydı döndürülür. Bu durumda, contoso us.cloudapp.net döndürülen bize varsayalım.
6. Ardından, özyinelemeli DNS hizmeti 'cloudapp.net' etki alanı için ad sunucularını bulur. 'Contoso-us.cloudapp .net' istemek için bu ad sunucularıyla iletişim kurar DNS kaydı. ABD tabanlı hizmet uç IP adresini içeren bir 'A' DNS kaydı döndürülür.
7. Yinelemeli DNS hizmeti sonuçları birleştirir ve istemci için tek bir DNS yanıtı döndürür.
8. İstemci DNS sonuçlarını alır ve belirli IP adresine bağlanır. İstemci uygulama Hizmeti uç noktası için trafik Yöneticisi ile doğrudan bağlanır. Bir HTTPS uç noktası olduğundan, istemci gerekli SSL/TLS anlaşması gerçekleştirir ve sonra bir HTTP GET isteği yapar ' / login.aspx' sayfa.

Yinelemeli DNS hizmeti aldığı DNS yanıtlarını önbelleğe kaydeder. İstemci cihazda DNS Çözümleyicisi ayrıca sonucu önbelleğe alır. Önbelleğe alma, veri önbellekten yerine tarafından diğer ad sunucularını sorgulama daha hızlı bir şekilde yanıtlanması gereken sonraki DNS sorgularını sağlar. Önbellek süresini 'time-to-live' (TTL) özelliği her bir DNS kaydı tarafından belirlenir. Kısa değerler trafik Yöneticisi ad sunucuları için daha hızlı önbellek süre sonu ve bu nedenle daha fazla gidiş dönüş sonuçlanır. Başarısız bir uç nokta çıktığınızda doğrudan trafiği daha uzun sürebilir, uzun değerleri anlamına gelir. Traffic Manager trafik Yöneticisi DNS yanıtları 0 saniye olarak en düşük ve yüksek 2.147.483.647 saniye olarak kullanılacak TTL yapılandırmanıza olanak verir (ile uyumlu en büyük aralığı [RFC 1035](https://www.ietf.org/rfc/rfc1035.txt)), böylece, uygulamanızın gereksinimlerine en iyi dengeleyen bir değer seçin.

## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma bilgileri için bkz: [trafik Yöneticisi fiyatlandırma](https://azure.microsoft.com/pricing/details/traffic-manager/).

## <a name="faq"></a>SSS

Trafik Yöneticisi ile ilgili sık sorulan sorular için bkz: [trafik Yöneticisi ile ilgili SSS](traffic-manager-FAQs.md)

## <a name="next-steps"></a>Sonraki adımlar

Trafik Yöneticisi hakkında daha fazla bilgi [uç nokta izleme ve otomatik yük devretme](traffic-manager-monitoring.md).

Trafik Yöneticisi hakkında daha fazla bilgi [trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md).

Azure'un diğer önemli [ağ özelliklerinden](../networking/networking-overview.md) bazıları hakkında bilgi edinin.

<!--Image references-->
[1]: ./media/traffic-manager-how-traffic-manager-works/dns-configuration.png
[2]: ./media/traffic-manager-how-traffic-manager-works/flow.png

