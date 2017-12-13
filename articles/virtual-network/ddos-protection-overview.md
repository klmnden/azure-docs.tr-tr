---
title: "Azure DDoS koruması standart genel bakış | Microsoft Docs"
description: "Azure DDoS koruması hizmeti hakkında bilgi edinin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2017
ms.author: jdial
ms.openlocfilehash: 6b15be022ba3b8373cfb852be8fc6915824801dc
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ddos-protection-standard-overview"></a>DDoS koruması standart Azure genel bakış

Dağıtılmış Hizmet engelleme (DDoS) saldırıları uygulamalarını buluta taşıma müşterilerin karşılaştığı en büyük kullanılabilirlik ve güvenlik sorunları biri olan. DDoS saldırı uygulamayı yasal kullanıcılara kullanılamaz hale getirme uygulamanın kaynaklarını tüketebilir dener. DDoS saldırıları, Internet üzerinden genel olarak erişilebilen herhangi bir uç noktada hedeflenebilir.

Azure DDoS koruması, uygulama tasarım en iyi uygulamaları ile birlikte DDoS saldırılara karşı koruma sağlar. Aşağıdaki hizmet katmanları Azure DDos koruması sağlar: 

- **Azure DDoS koruması temel**: ek ücret ödemeden Azure platformu bir parçası olarak otomatik olarak etkinleştirilir. Ortak ağ düzeyinde saldırılar her zaman açık trafiği izleme ve gerçek zamanlı azaltma Microsoft online services tarafından kullanılan aynı savunma sağlar. Azure'nın genel ağ tüm ölçeğini dağıtmak ve bölgeler arasında saldırı trafiğini azaltmak için kullanılabilir. IPv4 ve IPv6 Azure için koruması sağlanan [genel IP adresleri](virtual-network-public-ip-address.md).
- **Azure DDoS koruması standart** özellikle Azure sanal ağ kaynakları için ayarlanmış ek azaltma özellikleri sağlar. Etkinleştirmek basit bir işlemdir ve hiçbir uygulama değişiklikleri gerekir. Ayrılmış trafiğini izleme ve machine learning algoritmaları ve ilişkili sanal ağlar, Azure yük dengeleyici, Azure uygulama ağ geçidi ve Azure gibi dağıtılmış kaynaklar için genel IP adresleri uygulanan aracılığıyla koruma ilkeleri ayarlanmış Service Fabric örnekleri. Gerçek zamanlı telemetri, bir saldırı sırasında ve geçmişi için Azure İzleyici görünümleri aracılığıyla kullanılabilir. Uygulama katmanı korumaları aracılığıyla eklenebilir [uygulama ağ geçidi Web uygulaması güvenlik duvarı](https://azure.microsoft.com/services/application-gateway). Koruma için IPv4 Azure sağlanan [genel IP adresleri](virtual-network-public-ip-address.md). 

![Azure DDoS koruması standart](./media/ddos-protection-overview/ddos-protection-overview-fig2.png)

> [!IMPORTANT]
> Azure DDoS koruması standart şu anda önizlemede değil. Koruma, sanal makineler, yük dengeleyici ve uygulama ağ geçitleri gibi ilişkili Azure genel IP adresine sahip herhangi bir Azure kaynak için sağlanır. Yapmanız [kaydetmek](http://aka.ms/ddosprotection) hizmeti aboneliğiniz için DDoS koruması standart etkinleştirebilmeniz için. Kayıt sonra Azure DDoS ekibi, kişiler ve etkinleştirme işleminde size rehberlik eder. DDoS koruması standart kullanılabilir Doğu ABD, Doğu ABD 2, Batı ABD, Batı Orta ABD, Kuzey Avrupa, Batı Avrupa, Japonya Batı, Japonya Doğu, Doğu Asya ve Güneydoğu Asya bölgeleri. Önizleme sırasında hizmeti kullandığınız için sizden ücret istenmese.

DDoS koruması standart geliştirme, test ve üretim ortamlarında deneyin öneririz. Deneyimlerinizi hakkında geri bildirim sağlamak için aşağıdaki kaynakları kullanın:
- [Microsoft Azure Forumunda Azure DDoS koruması](https://feedback.azure.com/forums/905032-azure-ddos-protection). 
- [MSDN Forumu Azure DDoS koruması](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azureddosprotection)
- [Yığın taşması Azure DDos koruması](https://stackoverflow.com/tags/azure-ddos/info)

Destek sorunları için yapabileceğiniz [Azure destek bileti açmadan](../azure-supportability/how-to-create-azure-support-request.md). DDoS koruması standart önizlemede olmakla birlikte, destek en iyi çaba ilkesine göre sağlanır.

## <a name="types-of-ddos-attacks-that-ddos-protection-standard-mitigates"></a>DDoS koruması standart azaltır DDoS saldırı türleri

DDoS koruması standart bu tür saldırılar en aza indirebileceğiniz:

- **Volumetric saldırıları**: görünen meşru trafiğin önemli miktarda ağ katmanla bölgesini doldurmak için saldırı 's hedeftir. UDP sel, yükseltme sel ve diğer sahte paket sel içerir. DDoS koruması standart bu çok gigabayt saldırı ortaya çıkar ve bunları temizleme Azure'nın genel ağ ölçek, otomatik olarak yararlanarak azaltır. 
- **Protokol saldırıları**: bu saldırıların hedef erişilemez bir zayıflık Katman 3 ve 4 katman protokol yığını yararlanarak işlenemiyor. İçerdiği Eşitlemeye baskın saldırıları, yansıma saldırılarını ve diğer Protokolü saldırılardan. DDoS koruması standart bu saldırıların, istemci ile etkileşim ve kötü amaçlı trafiği engelleyen tarafından kötü amaçlı ve yasal trafiği arasında ayrım yapma azaltır. 
- **Uygulama katmanı saldırıları**: bu saldırıların ana bilgisayarlar arasında veri aktarımını kesintiye web uygulama paketleri hedef. HTTP protokolü ihlali, SQL ekleme, siteler arası komut dosyası ve diğer katman 7 saldırılara içerir. Azure kullanmak [uygulama ağ geçidi web uygulaması güvenlik duvarı](../application-gateway/application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json), DDoS koruması bu saldırılara karşı koruma sağlamak için standart ile. 

DDoS koruması standart sanal makineler, yük dengeleyici ve uygulama ağ geçitleri ile ilişkili ortak IP adreslerini içeren bir sanal ağ kaynaklarında korur. Uygulama ağ geçidi web uygulaması güvenlik duvarı ile birlikte, DDoS koruması standart tam katman 7 azaltma özelliği Katman 3 sağlayabilir.

## <a name="ddos-protection-standard-features"></a>DDoS koruması standart özellikler

![DDoS işlevi](./media/ddos-protection-overview/ddos-overview-fig1.png)

DDoS koruması standart özellikleri içerir: 

- **Yerel platform tümleştirme:** yerel Azure'da tümleşik ve Azure portal ve PowerShell üzerinden yapılandırmayı içerir. DDoS koruması standart kaynaklar ve kaynak yapılandırması bilir.
- **Her zaman açık trafiğini izleme:** DDoS saldırıları göstergeleri için arayan 24 saatlik haftanın 7 günü, izlenen uygulama trafik düzenleri. Koruma ilkeleri aşıldığında azaltma gerçekleştirilir.
- **Anahtar teslim koruma:** Basitleştirilmiş yapılandırma DDoS koruması standart etkin hemen sonra bir sanal ağdaki tüm kaynaklar hemen korur. Hiçbir müdahale veya kullanıcı tanımı gereklidir. Bunu algılandığında DDoS koruması standart hemen ve otomatik olarak saldırı azaltır.
- **Uyarlamalı ayarlama:** akıllı trafiği profil zaman içerisinde, uygulamanızın trafiği öğrenir ve seçer ve hizmetiniz için en uygun profili güncelleştirir. Trafik zaman içinde değiştikçe profilin ayarlar.
- **Katman 7 koruması için Katman 3:** tam yığını DDoS koruması, bir uygulama ağ geçidi ile kullanıldığında sağlar.
- **Kapsamlı azaltma ölçeği:** üzerinden 60 farklı saldırı türleri azaltıldığından, en büyük bilinen DDoS saldırılarına karşı korumak için genel kapasiteye sahip. 
- **Saldırı ölçümleri:** Summarized ölçümleri her saldırılara karşı Azure İzleyicisi aracılığıyla erişilebilir.
- **Saldırı Uyarı:** başlatma ve durdurma bir saldırının uyarıları yapılandırılabilir ve yerleşik saldırı ölçümleri saldırı 's süresinde kullanma. Microsoft Operations Management Suite, Splunk, Azure Storage, e-posta ve Azure portalı gibi işletimsel, yazılımına uyarıları tümleştirin.
- **Maliyet garantisi:** veri aktarım ve uygulama genişleme hizmet iadeleri belgelenen DDoS saldırıları için.

## <a name="ddos-protection-standard-mitigation"></a>DDoS koruması standart azaltma

Microsoft'un DDoS koruması hizmeti gerçek trafiği kullanımı izleyicileri ve sürekli DDoS ilkede tanımlanan eşiklere karşılaştırır. Bu trafik eşiği aşıldığında DDoS azaltma otomatik olarak başlatılır. Trafik eşiğin altında geri döndüğünde, azaltma kaldırılır.

Azaltma sırasında korumalı kaynağa gönderilen trafiğin DDoS koruması hizmeti tarafından yönlendirilir ve birkaç denetimleri yapılır. Bu denetimler, genellikle aşağıdaki işlevleri gerçekleştirir:

- Paket Internet belirtimlerine uygun ve hatalı biçimlendirilmiş olmadığından emin olun.
- Trafiği olası sahte bir paket olup olmadığını belirlemek için istemci ile etkileşim (örneğin: Eşitlemeye Auth veya Eşitlemeye tanımlama bilgisi veya onu kurtarmasını kaynağı için bir paket bırakma).
- Hız sınırı paketleri, başka bir zorlama yöntemi varsa gerçekleştirilebilir.

DDoS koruması blokları saldırılara karşı trafiği ve hedeflenen hedefine trafik kalan iletir. Saldırı algılama birkaç dakika içinde Azure İzleyici ölçümleri kullanarak bildirilir. DDoS koruması standart telemetriyi günlüğü yapılandırarak, gelecekteki analiz için kullanılabilir seçenekleri için günlükleri yazabilirsiniz. DDoS koruması standart için Azure İzleyicisi'nde ölçüm verileri şu anda 30 gün boyunca tutulur.

Müşterilerin kendi DDoS saldırıları benzetimini yapmak için önermiyoruz. Müşterilerin bir DDoS istemek için bir destek kanalıyla bunun yerine, kullanabilirsiniz saldırı Azure ağ tarafından yürütülen benzetimi. Mühendisin DDoS saldırı (bağlantı noktaları, protokoller, hedef IP) ayrıntılarını düzenlemeniz ve test zamanlamak için bir zaman düzenlemek için sizinle iletişim kuracaktır.

## <a name="next-steps"></a>Sonraki adımlar

- DDoS koruması standart kullanarak yönetme hakkında daha fazla bilgi [Azure PowerShell](ddos-protection-manage-ps.md) veya [Azure portal](ddos-protection-manage-portal.md).
