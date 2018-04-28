---
title: Azure DDoS koruması standart genel bakış | Microsoft Docs
description: Azure DDoS koruması hizmeti hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/29/2018
ms.author: jdial
ms.openlocfilehash: 705f69f9143e3d2b27a3099f340218aaa12931f8
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="azure-ddos-protection-standard-overview"></a>DDoS koruması standart Azure genel bakış

Dağıtılmış Hizmet engelleme (DDoS) saldırıları uygulamalarını buluta taşıma müşterilerin karşılaştığı en büyük kullanılabilirlik ve güvenlik sorunları bazıları verilmiştir. DDoS saldırı uygulamayı yasal kullanıcılara kullanılamaz hale getirme uygulamanın kaynaklarını tüketebilir dener. DDoS saldırıları, Internet üzerinden genel olarak erişilebilen herhangi bir uç noktada hedeflenebilir.

Uygulama tasarımı en iyi uygulamaları ile birlikte azure DDoS koruması DDoS saldırılara karşı koruma sağlar. Aşağıdaki hizmet katmanları Azure DDos koruması sağlar:

- **Temel**: ek ücret ödemeden Azure platformu bir parçası olarak otomatik olarak etkinleştirilir. Her zaman açık trafiğini izleme ve ortak ağ düzeyinde saldırılar, gerçek zamanlı azaltma Microsoft online services tarafından kullanılan aynı savunma sağlar. Azure'nın genel ağ tüm ölçeğini dağıtmak ve bölgeler arasında saldırı trafiğini azaltmak için kullanılabilir. IPv4 ve IPv6 Azure için koruması sağlanan [genel IP adresleri](virtual-network-public-ip-address.md).
- **Standart**: özellikle Azure sanal ağ kaynakları için ayarlanmış temel hizmet katmanı üzerinden ek azaltma özellikleri sağlar. DDoS koruması standart etkinleştirmek, basit ve hiçbir uygulama değişiklikleri gerekir. Koruma ilkeleri ayrılmış trafiğini izleme ve machine learning algoritmaları aracılığıyla ayarlanmıştır. İlkeleri, ilişkili sanal ağlar, Azure yük dengeleyici, Azure uygulama ağ geçidi ve Azure Service Fabric örnekleri gibi dağıtılmış kaynaklar için genel IP adresleri için uygulanır. Gerçek zamanlı telemetri, bir saldırı sırasında ve geçmişi için Azure İzleyici görünümleri aracılığıyla kullanılabilir. Uygulama katmanı koruması aracılığıyla eklenebilir [Azure uygulama ağ geçidi Web uygulaması güvenlik duvarı](../application-gateway//application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Koruma için IPv4 Azure sağlanan [genel IP adresleri](virtual-network-public-ip-address.md).

![Azure DDoS koruması standart](./media/ddos-protection-overview/ddospic.png)

## <a name="types-of-ddos-attacks-that-ddos-protection-standard-mitigates"></a>DDoS koruması standart azaltır DDoS saldırı türleri

DDoS koruması standart aşağıdaki tür saldırılar en aza indirebileceğiniz:

- **Volumetric saldırıları**: görünen meşru trafiğin önemli miktarda ağ katmanla bölgesini doldurmak için saldırı 's hedeftir. UDP sel, yükseltme sel ve diğer sahte paket sel içerir. DDoS koruması standart ortaya çıkar ve bunları Azure'nın genel ağ ölçek ile otomatik olarak temizleme bu çok gigabayt saldırı azaltır.
- **Protokol saldırıları**: bu saldırıların bir hedef erişilemez, Katman 3 ve 4 katman protokol yığını bir zayıflık yararlanarak işlenemiyor. İçerdiği Eşitlemeye baskın saldırıları, yansıma saldırılarını ve diğer Protokolü saldırılardan. DDoS koruması standart bu saldırıların, istemci ile etkileşim ve kötü amaçlı trafiği engelleyen tarafından kötü amaçlı ve yasal trafiği arasında ayrım yapma azaltır. 
- **Kaynak (uygulama) katmanı saldırıları**: bu saldırıların ana bilgisayarlar arasında veri aktarımını kesintiye web uygulama paketleri hedef. HTTP protokolü ihlali, SQL ekleme, siteler arası komut dosyası ve diğer katman 7 saldırılara saldırılar içerir. Azure kullanmak [uygulama ağ geçidi web uygulaması güvenlik duvarı](../application-gateway/application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json), DDoS koruması bu saldırılara karşı koruma sağlamak için standart ile. Ayrıca üçüncü taraf web uygulaması güvenlik duvarı sunumu bulunmadığından bulunan [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps?page=1&search=web%20application%20firewall).

DDoS koruması standart sanal makineler, yük dengeleyici ve uygulama ağ geçitleri ile ilişkili ortak IP adreslerini içeren bir sanal ağ kaynaklarında korur. Uygulama ağ geçidi web uygulaması güvenlik duvarı ile birlikte, DDoS koruması standart tam katman 7 azaltma özelliği Katman 3 sağlayabilir.

## <a name="ddos-protection-standard-features"></a>DDoS koruması standart özellikler

![DDoS işlevi](./media/ddos-protection-overview/ddosfeatures.png)

DDoS koruması standart özellikleri içerir:

- **Yerel platform tümleştirme:** Azure yerel tümleştirilmiştir. Azure portalı üzerinden yapılandırma içerir. DDoS koruması standart kaynaklar ve kaynak yapılandırması bilir.
- **Anahtar teslim koruma:** Basitleştirilmiş yapılandırma DDoS koruması standart etkin hemen sonra bir sanal ağdaki tüm kaynaklar hemen korur. Hiçbir müdahale veya kullanıcı tanımı gereklidir. Bunu algılandığında DDoS koruması standart hemen ve otomatik olarak saldırı azaltır.
- **Her zaman açık trafiğini izleme:** DDoS saldırıları göstergeleri için arayan 24 saatlik haftanın 7 günü, izlenen uygulama trafik düzenleri. Koruma ilkeleri aşıldığında azaltma gerçekleştirilir.
- **Uyarlamalı ayarlama:** akıllı trafiği profil zaman içerisinde, uygulamanızın trafiği öğrenir ve seçer ve hizmetiniz için en uygun profili güncelleştirir. Trafik zaman içinde değiştikçe profilin ayarlar.
- **Katman 7 koruması için Katman 3:** bir web uygulaması güvenlik duvarı ile kullanıldığında tam yığını DDoS koruması sağlar.
- **Kapsamlı azaltma ölçeği:** üzerinden 60 farklı saldırı türleri azaltıldığından, en büyük bilinen DDoS saldırılarına karşı korumak için genel kapasiteye sahip.
- **Saldırı ölçümleri:** Summarized ölçümleri her saldırılara karşı Azure İzleyicisi aracılığıyla erişilebilir.
- **Saldırı Uyarı:** başlatma ve durdurma bir saldırının uyarıları yapılandırılabilir ve yerleşik saldırı ölçümleri saldırı 's süresinde kullanma. Microsoft Azure günlük analizi, Splunk, Azure Storage, e-posta ve Azure portalı gibi işletimsel, yazılımına uyarıları tümleştirin.
- **Maliyet garantisi:** veri aktarım ve uygulama genişleme hizmet iadeleri belgelenen DDoS saldırıları için.

## <a name="ddos-protection-standard-mitigation"></a>DDoS koruması standart azaltma

DDoS koruması standart gerçek trafiği kullanımı izleyicileri ve sürekli DDoS ilkede tanımlanan eşiklere karşılaştırır. Trafik eşiği aşıldığında DDoS azaltma otomatik olarak başlatılır. Trafik eşiğin altında geri döndüğünde, azaltma kaldırılır.

![Risk azaltma](./media/ddos-protection-overview/mitigation.png)

Azaltma sırasında korumalı kaynağa gönderilen trafiğin DDoS koruması hizmeti tarafından yönlendirilir ve bazı denetimleri, aşağıdaki denetimleri gibi yapılır:

- Paket Internet belirtimlerine uygun ve hatalı biçimlendirilmiş olmadığından emin olun.
- Trafiği olası sahte bir paket olup olmadığını belirlemek için istemci ile etkileşim (örneğin: Eşitlemeye Auth veya Eşitlemeye tanımlama bilgisi veya onu kurtarmasını kaynağı için bir paket bırakma).
- Hız sınırı paketleri, başka bir zorlama yöntemi varsa gerçekleştirilebilir.

DDoS koruması saldırı trafiği engeller ve kalan trafiğini hedeflenen hedefine iletir. Saldırı algılama birkaç dakika içinde Azure İzleyici ölçümleri kullanarak bildirilir. DDoS koruması standart telemetriyi günlüğü yapılandırarak, gelecekteki analiz için kullanılabilir seçenekleri için günlükleri yazabilirsiniz. DDoS koruması standart için Azure İzleyicisi'nde ölçüm verileri 30 gün boyunca tutulur.

Microsoft ile ortaklık [BreakingPoint bulut](https://www.ixiacom.com/products/breakingpoint-cloud) Burada, üretebilir trafiği ortak IP adreslerini benzetimleri DDoS koruması etkin karşı bir arabirim oluşturmak için. Kesme noktası bulut benzetimi sağlar:

- Nasıl Microsoft Azure DDoS koruması standart Azure kaynaklarınızı DDoS saldırılara karşı korur doğrula
- Olay yanıtlama işleminizi DDoS saldırıya en iyi duruma getirme
- Belge DDoS uyumluluk
- Ağ güvenlik gruplarını eğitme

## <a name="next-steps"></a>Sonraki adımlar

- [DDoS koruması standart yapılandırma](manage-ddos-protection.md)