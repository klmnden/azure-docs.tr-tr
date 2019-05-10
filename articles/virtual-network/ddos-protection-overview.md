---
title: Azure DDoS koruması standart genel bakış | Microsoft Docs
description: Azure DDoS koruması hizmeti hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/13/2018
ms.author: kumud
ms.openlocfilehash: 41e9d88df49d153089e6dc7a12c5873ccc167279
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65209454"
---
# <a name="azure-ddos-protection-standard-overview"></a>Azure DDoS koruması standart genel bakış

Dağıtılmış hizmet engelleme (DDoS) saldırıları, uygulamalarını buluta taşıyan müşterilerin karşılaştığı en büyük kullanılabilirlik ve güvenlik sorunlarından biridir. DDoS saldırıları, bir uygulamanın kaynaklarını tüketerek uygulamanın geçerli kullanıcılar için kullanılamaz duruma gelmesini amaçlar. DDoS saldırıları internet üzerinden genel olarak erişilebilen herhangi bir uç noktasını hedefleyebilir.

Uygulama tasarımı, en iyi ile birlikte, azure DDoS koruması, DDoS saldırılarına karşı koruma sağlayın. Azure DDoS koruması, şu hizmet katmanlarında sağlar:

- **Temel**: Azure platformunun bir parçası olarak otomatik olarak etkinleştirilir. Her zaman açık trafik izleme ve gerçek zamanlı azaltma ortak ağ düzeyinde saldırı, Microsoft online services tarafından kullanılan aynı savunmaları sağlar. Tüm ölçek Azure'nın küresel ağı, dağıtmak ve bölgeler arasında saldırı trafiği azaltmak için kullanılabilir. IPv4 ve IPv6 Azure için koruması sağlanan [genel IP adresleri](virtual-network-public-ip-address.md).
- **Standart**: Azure sanal ağ kaynakları için özel olarak ayarlanmış bir temel hizmet katmanı üzerinden ek risk azaltma özellikleri sağlar. DDoS koruması standart etkinleştirmek, basit ve herhangi bir uygulama değişiklik gerektirmez. Koruma ilkeleri, adanmış trafiğini izleme ve makine öğrenimi algoritmaları ayarlanır. Azure Service Fabric örnekleri, Azure Load Balancer ve Azure Application Gateway gibi sanal ağlara dağıtılan kaynaklara ilişkili genel IP adresleri için ilkeler uygulanır, ancak bu koruma App Service ortamları için geçerli değildir. Gerçek zamanlı telemetri geçmişi yanı sıra, bir saldırı sırasında Azure izleme görünümleri aracılığıyla kullanılabilir. Zengin saldırı azaltma analytics, tanılama ayarları kullanılabilir. Uygulama katmanı koruması aracılığıyla eklenebilir [Azure Application Gateway Web uygulaması güvenlik duvarı](../application-gateway//application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya 3 taraf güvenlik duvarı Azure Market'ten yükleyerek. IPv4 ve IPv6 Azure için koruması sağlanan [genel IP adresleri](virtual-network-public-ip-address.md).

![Azure DDoS koruması temel vs. Standart](./media/ddos-protection-overview/ddoscomparison.png)

## <a name="types-of-ddos-attacks-that-ddos-protection-standard-mitigates"></a>DDoS koruması standart azaltır DDoS saldırılarının türleri

DDoS koruması standart aşağıdaki türdeki saldırıları azaltabilirsiniz:

- **Volumetric saldırıları**: Saldırı'nın görünüşte yasal trafiği önemli miktarda ağ katmanı doldurmak için hedeftir. Bu, UDP sel, yükseltme sel ve diğer sahte paket sel içerir. DDoS koruması standart azaltırız ve bunları Azure'nın küresel ağı ölçek ile otomatik olarak temizleme bu olası çok gigabayt saldırıları azaltır.
- **Protokol saldırıları**: Bu tür saldırıları, zayıflıklar Katman 3 olarak ve katman 4 protokol yığınının yararlanarak bir hedef erişilemez, işleyin. İçerdiği SYN baskın saldırılarına, yansıma saldırılarını ve diğer protokol saldırıları. DDoS koruması standart bu saldırıları, istemci ile etkileşim kurma ve kötü amaçlı trafiği engelleyen, kötü amaçlı ve yasal trafiği arasında ayrım azaltır. 
- **Kaynak (uygulama) katmanı saldırıları**: Bu tür saldırıları, konaklar arasında veri aktarımını kesintiye web uygulama paketleri hedefleyin. HTTP protokolü ihlallerine, SQL ekleme, siteler arası betik ve diğer 7. Katman saldırıları saldırıları içerir. Azure kullanan [Application Gateway web uygulaması güvenlik duvarı](../application-gateway/application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json), DDoS koruması bu saldırılara karşı koruma sağlamak için standart ile. Kullanılabilir ayrıca üçüncü taraf web uygulaması güvenlik duvarı teklifleri de [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps?page=1&search=web%20application%20firewall).

Standart DDoS koruması, sanal makineler, yük Dengeleyiciler ve uygulama ağ geçitleri ile ilişkili genel IP adresleri de dahil olmak üzere bir sanal ağ içindeki kaynaklarla korur. Application Gateway web uygulaması güvenlik duvarı ile birlikte, DDoS koruması standart tam katman 7 azaltma yeteneği Katman 3 sağlayabilir.

## <a name="ddos-protection-standard-features"></a>DDoS koruması standart özellikler

![DDoS işlevi](./media/ddos-protection-overview/ddosfeatures.png)

DDoS koruması standart özellikler şunlardır:

- **Yerel platform tümleştirme:** Yerel olarak Azure'a entegre. Azure portalı üzerinden yapılandırma içerir. Standart DDoS koruması, kaynaklarınızı ve kaynak yapılandırması farkındadır.
- **Kullanıma hazır koruma:** DDoS koruması standart etkin olarak Basitleştirilmiş yapılandırma, bir sanal ağ üzerindeki tüm kaynaklar hemen korur. Hiçbir müdahale veya kullanıcı tanımı gereklidir. Bunu algılandığında DDoS koruması standart anında ve otomatik olarak saldırı azaltır.
- **Her zaman açık trafik izleme:** Uygulama trafiği düzenlerinin, DDoS saldırılarının göstergelerini bakarak haftada 7 gün, günde 24 saat izlenir. Koruma ilkeleri aşıldığında azaltma gerçekleştirilir.
- **Uyarlamalı ayarlama:** Akıllı trafik profil oluşturma uygulamanızdaki trafiği zamanla öğrenir seçer ve hizmetiniz için en uygun profili güncelleştirir. Trafiği zamanla olan değişimini profilinin ayarlar.
- **Çok katmanlı koruma:** Tam yığın DDoS koruması, web uygulaması güvenlik duvarı ile kullanıldığında sağlar.
- **Kapsamlı azaltma Ölçek:** En büyük bilinen DDoS saldırılarına karşı korumaya yönelik genel kapasiteyle 60'tan fazla farklı saldırı türleri azaltılabilir.
- **Saldırı analizi:** Saldırı sırasında beş dakikalık artışlarla ayrıntılı raporlar ve saldırı sona erdikten sonra eksiksiz bir özet alın. Stream azaltma akış bir çevrimdışı güvenlik bilgileri ve Olay yönetimi (SIEM) sistemi neredeyse gerçek zamanlı bir saldırı sırasında izleme günlükleri.
- **Saldırı ölçümleri:** Her saldırılara karşı özetlenmiş ölçümler, Azure İzleyici erişilebilir.
- **Saldırı Uyarı:** Uyarılar, başlatma ve durdurma bir saldırı sırasında yapılandırılabilir ve saldırı'nın süresi boyunca yerleşik saldırı ölçümleri kullanarak. Uyarıları operasyonel yazılımınızı Microsoft Azure İzleyicisi günlükleri, Splunk, Azure depolama, e-posta ve Azure portalı gibi tümleştirin.
- **Maliyet garantisi:** Veri aktarımı ve uygulama ölçeklendirme hizmet iadeleri belgelenmiş bir DDoS saldırıları için.

## <a name="ddos-protection-standard-mitigation"></a>DDoS koruması standart azaltma

DDoS koruması standart gerçek trafik kullanımını izler ve sürekli DDoS ilkede tanımlanan eşiklere göre karşılaştırır. Trafik eşiği aşıldığında, DDoS saldırılarının otomatik olarak başlatılır. Risk azaltma trafiği eşiğin altında döndüğünde kaldırılır.

![Azaltma](./media/ddos-protection-overview/mitigation.png)

Azaltma, korumalı kaynağa gönderilen trafik DDoS koruması hizmeti tarafından yönlendirilir ve birkaç önleme denetimleri yapıldıktan, aşağıdaki denetimleri gibi:

- Paketleri internet belirtimlerine uygun ve hatalı biçimlendirilmiş değil emin olun.
- Trafik olası sahte bir paket olup olmadığını belirlemek için istemci ile etkileşim kurma (ör.: SYN Auth veya SYN tanımlama bilgisi veya onu kurtarmasını kaynağı için bir paket bırakarak).
- Hız sınırı paketleri, başka bir zorlama yöntemi varsa gerçekleştirilebilir.

DDoS koruması, saldırı trafiği engeller ve amaçlanan hedeflerine kalan trafik iletir. Saldırı algılama birkaç dakika içinde Azure İzleyici ölçümleri kullanarak size bildirilir. DDoS koruması standart telemetrisi üzerinde günlük yapılandırarak, gelecekteki analizler için kullanılabilir seçenekler günlükleri yazabilirsiniz. Standart DDoS koruması için Azure İzleyici ölçüm verileri 30 gün boyunca tutulur.

Microsoft ile kurdu [BreakingPoint Cloud](https://www.ixiacom.com/products/breakingpoint-cloud) burada oluşturun DDoS koruması etkinleştirilmiş genel IP adresleri simülasyonlar için trafiği bir arabirim oluşturmak için. Kesme noktası bulut benzetim yapmanıza olanak sağlar:

- Nasıl Microsoft Azure DDoS koruması standart Azure kaynaklarınızı DDoS saldırılarına karşı koruyan doğrula
- Olay yanıtlama işleminize DDoS saldırıya en iyi duruma getirme
- Belge DDoS uygunluk
- Ağ güvenlik gruplarını eğitin

## <a name="next-steps"></a>Sonraki adımlar

- [DDoS koruma standardını yapılandırma](manage-ddos-protection.md)
