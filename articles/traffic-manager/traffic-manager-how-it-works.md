---
title: Azure Traffic Manager nasıl çalışır? | Microsoft Docs
description: Bu makalede, yüksek performans ve kullanılabilirlik, web uygulamaları için trafik Traffic Manager tarafından nasıl yönlendirdiği anlamanıza yardımcı olur
services: traffic-manager
documentationcenter: ''
author: KumudD
manager: twooley
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/05/2019
ms.author: kumud
ms.openlocfilehash: 52469cb2735b2270815191ec0815daee350882a4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60330321"
---
# <a name="how-traffic-manager-works"></a>Traffic Manager nasıl çalışır?

Azure Traffic Manager, uygulama uç noktalar genelinde trafiğinin dağıtımını denetlemenizi sağlar. Uç nokta, Azure içinde veya dışında barındırılan İnternet'e yönelik bir hizmettir.

Traffic Manager, iki temel fayda sağlar:

- Dağıtım birkaç birine göre trafik [trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md)
- [Uç nokta durumunu sürekli izleme](traffic-manager-monitoring.md) ve uç noktaları başarısız olduğunda otomatik yük devretme

Bir istemci bir hizmete bağlanma girişiminde bulunduğunda, bu hizmetin DNS adını bir IP adresi için ilk çözmeniz gerekir. Ardından, istemci hizmete erişmek için bu IP adresine bağlanır.

**Anlamak için en önemli nokta Traffic Manager DNS düzeyinde çalışmasıdır.**  Traffic Manager trafik yönlendirme yönteminin kurallara göre belirli hizmet uç noktaları istemcilere yönlendirmek için DNS kullanır. İstemcileri seçili uç noktaya bağlanmak **doğrudan**. Traffic Manager, bir proxy ya da bir ağ geçidi değil. Traffic Manager, istemci ile hizmet arasında geçen trafiği görmez.

## <a name="traffic-manager-example"></a>Traffic Manager örneği

Contoso Corp yeni bir iş ortağı portalı geliştirilmiştir. Bu portalı URL'si https://partners.contoso.com/login.aspx. Uygulama Azure üç bölgede barındırılır. Kullanılabilirliği geliştirmek ve genel performansı en üst düzeye çıkarmak için Traffic Manager kullanılabilir en yakın uç noktasına istemci trafiğini dağıtmak için kullanırlar.

Bu yapılandırma elde etmek için aşağıdaki adımları tamamlayın:

1. Üç hizmet örneği dağıtın. Bu dağıtımları DNS adları 'contoso-us.cloudapp .net', 'contoso-eu.cloudapp .net' ve 'contoso-asia.cloudapp .net' dir.
1. 'Contoso.trafficmanager.net' adlı Traffic Manager profili oluşturun ve üç uç noktalar genelinde 'Performans' trafik yönlendirme yöntemini kullanacak şekilde yapılandırın.
1. Kullanıcıların gösterim etki alanı adı, '' bir DNS CNAME kaydı kullanılarak ', contoso.trafficmanager.NET'e yönlendirmek için partners.contoso.com','nı yapılandırın.

![Traffic Manager DNS yapılandırması][1]

> [!NOTE]
> Gösterim etki alanı ile Azure Traffic Manager kullanırken, Traffic Manager etki alanı adınızı gösterim etki alanı adınızı işaret edecek bir CNAME kullanmanız gerekir. DNS standartları 'apex' (veya kök) bir etki alanına bir CNAME oluşturmanızı izin vermez. Bu nedenle, 'contoso.com' ('naked' etki alanı olarak da adlandırılır) için bir CNAME oluşturamazsınız. Yalnızca 'contoso.com' gibi 'www.contoso.com' altındaki bir etki alanı için CNAME oluşturabilirsiniz. Bu sınırlamaya geçici bir çözüm için şirket DNS etki alanınızı barındırmaya öneririz [Azure DNS](../dns/dns-overview.md) ve kullanarak [diğer ad kayıtlarını](../dns/tutorial-alias-tm.md) traffic manager profilinize yönlendirin. Alternatif olarak, "contoso.com" için "www.contoso.com" gibi bir diğer ad doğrudan istekleri için basit bir HTTP yeniden yönlendirme kullanabilirsiniz.

### <a name="how-clients-connect-using-traffic-manager"></a>Traffic Manager'ı kullanarak istemcilerin nasıl bağlanacağını

Önceki örnekten devam ederek bir istemci istediğinde sayfa https://partners.contoso.com/login.aspx, istemci DNS adı ve bağlantı çözmek için aşağıdaki adımları gerçekleştirir:

![Traffic Manager'ı kullanarak bağlantı kurma][2]

1. İstemci adı 'partners.contoso.com' çözmek için yapılandırılan özyinelemeli DNS hizmeti için bir DNS sorgusu gönderir. 'Yerel DNS' hizmet olarak da adlandırılan bir özyinelemeli DNS hizmeti, DNS etki alanlarınızı doğrudan barındırmıyor. Bunun yerine, istemciyi çeşitli yetkili DNS hizmetleri bir DNS ad çözümlemesi için gereken Internet üzerinden iletişim kurmasını işini off-loads.
2. Özyinelemeli DNS hizmeti DNS adını çözümlemek için 'contoso.com' etki alanı için ad sunucularını bulur. Ardından 'partners.contoso.com' DNS kaydı istemek için bu ad sunucularıyla iletişim kurar. Contoso.com DNS sunucuları için contoso.trafficmanager.net CNAME kaydı döndürür.
3. Ardından, özyinelemeli DNS hizmeti, Azure Traffic Manager hizmeti tarafından sağlanan 'trafficmanager.net' etki alanı için ad sunucularını bulur. Ardından bu DNS sunucuları için 'contoso.trafficmanager.net' DNS kaydı için bir istek gönderir.
4. Traffic Manager ad sunucularıyla isteği alır. Bunlar, temel bir uç nokta seçin:

    - Yapılandırılan her bir uç nokta durumunu (devre dışı uç noktalar alınmadı)
    - Traffic Manager durumu tarafından belirlenen her uç noktasının, geçerli durumunu denetler. Daha fazla bilgi için [Traffic Manager uç nokta izleme](traffic-manager-monitoring.md).
    - Seçtiğiniz trafik yönlendirme yöntemi. Daha fazla bilgi için [Traffic Manager yönlendirme yöntemleri](traffic-manager-routing-methods.md).

5. Seçtiğiniz uç nokta başka bir DNS CNAME kaydı döndürülür. Bu durumda, contoso us.cloudapp.net döndürülen bize varsayalım.
6. Ardından, özyinelemeli DNS hizmeti 'cloudapp.net' etki alanı için ad sunucularını bulur. 'Contoso-us.cloudapp .net' istemek için bu ad sunucularını indirecekse DNS kaydı. ABD tabanlı hizmet uç noktasının IP adresini içeren bir DNS 'A' kaydı döndürülür.
7. Özyinelemeli DNS hizmeti, sonuçları birleştirir ve istemciye tek bir DNS yanıtı döndürür.
8. İstemci, DNS sonuçlarını alır ve belirli bir IP adresine bağlar. İstemci uygulama Hizmeti uç noktası için Traffic Manager aracılığıyla değil, doğrudan bağlanır. Bir HTTPS uç noktası olduğundan, istemci gerekli SSL/TLS el sıkışma gerçekleştirir ve sonra bir HTTP GET isteği yapar ' / login.aspx' sayfa.

Özyinelemeli DNS hizmeti aldığı DNS yanıtlarını önbelleğe kaydeder. DNS çözümleyicisi istemci cihazında da sonucu önbelleğe alır. Önbelleğe alma, önbellekten verileri kullanarak yerine diğer ad sunucularını sorgulama daha hızlı bir şekilde yanıtlanması gereken sonraki DNS sorguları sağlar. Önbellek süresi 'zaman yaşam' (TTL) özelliği her bir DNS kaydı tarafından belirlenir. Kısa değerler daha hızlı önbellek süre sonu ve bu nedenle daha fazla gidiş dönüş için Traffic Manager ad sunucularıyla neden. Daha uzun değerleri, başarısız bir uç nokta uzağa trafiği daha uzun sürebilir, anlamına gelir. Traffic Manager 0 saniye kadar düşük ile 2.147.483.647 saniye olarak yüksek olması için Traffic Manager DNS yanıtlarını kullanılan TTL değerini yapılandırmanıza olanak tanır (ile uyumlu en büyük aralık [RFC 1035](https://www.ietf.org/rfc/rfc1035.txt)), değeri sayesinde, en iyi Uygulamanızın ihtiyaçlarını dengeler.


## <a name="next-steps"></a>Sonraki adımlar

Traffic Manager hakkında daha fazla bilgi edinin [uç nokta izleme ve otomatik yük devretme](traffic-manager-monitoring.md).

Traffic Manager hakkında daha fazla bilgi edinin [trafik yönlendirme yöntemlerini](traffic-manager-routing-methods.md).

<!--Image references-->
[1]: ./media/traffic-manager-how-traffic-manager-works/dns-configuration.png
[2]: ./media/traffic-manager-how-traffic-manager-works/flow.png

