---
title: "Azure DDoS koruması standart genel bakış | Microsoft Docs"
description: "Azure DDoS koruması hizmeti hakkında bilgi edinin."
services: virtual-network
documentationcenter: na
author: kumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/22/2017
ms.author: kumud
ms.openlocfilehash: 76da0d4e805c732d40a7bd02e5c70973c792e26c
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="azure-ddos-protection-standard-overview"></a>DDoS koruması standart Azure genel bakış

>[!IMPORTANT]
>Azure DDoS koruması standart şu anda önizlemede değil. Sınırlı sayıda DDoS koruması standart Azure kaynaklarını desteğinin ve bölgeler select sayısı. Yapmanız [hizmet için kayıt](http://aka.ms/ddosprotection) DDoS koruması aboneliğiniz için etkin standart almak için sınırlı Önizleme sırasında. Etkinleştirme işlemi boyunca size yol göstermesi için kayıt sırasında Azure DDoS ekibi tarafından kurulur. DDoS koruması standart Doğu ABD, Batı ABD ve Batı Orta ABD bölgelerde kullanılabilir. Önizleme sırasında hizmeti kullandığınız için sizden ücret istenmese.

Dağıtılmış Hizmet engelleme (DDoS) saldırıları en büyük kullanılabilirlik biridir ve güvenlik uygulamalarını buluta taşıma yan yana müşteriler ilgilidir. DDoS saldırı uygulamayı yasal kullanıcılara kullanılamaz hale getirme uygulamanın kaynaklarını tüketebilir dener. DDoS saldırıları, Internet üzerinden genel olarak erişilebilen herhangi bir uç noktada hedeflenebilir.

Uygulama tasarımı en iyi yöntemlerle birlikte birleştirilmiş Azure DDoS koruması bu saldırılarına karşı savunma sağlar. Bu iki hizmet katmanları sağlanır: 

- **Azure DDoS koruması temel** -ek ücret ödemeden Azure platformu bir parçası olarak zaten otomatik olarak etkinleştirilir. Ortak ağ düzeyinde saldırılar, her zaman açık trafiği izleme ve gerçek zamanlı azaltma Microsoft online services tarafından kullanılan aynı savunma sağlar.  Azure'nın genel ağ tüm ölçeğini dağıtmak ve bölgeler arasında saldırı trafiğini azaltmak için kullanılabilir. 
- **Azure DDoS koruması standart** -özellikle sanal ağ kaynakları için ayarlanmış ek azaltma özellikleri sağlar. Etkinleştirmek basit bir işlemdir ve hiçbir uygulama değişiklikleri gerektirir. Koruma ilkeleri ayrılmış trafiğini izleme ve machine learning algoritmaları ve yük dengeleyici, uygulama ağ geçidi ve Service Fabric örnekleri gibi sanal ağ kaynaklarına ilişkili genel IP'ler uygulanan aracılığıyla ayarlanmıştır.  Gerçek zamanlı telemetri, bir saldırı sırasında ve geçmişi için Azure İzleyici görünümleri aracılığıyla kullanılabilir. Uygulama katmanı korumaları aracılığıyla eklenebilir [uygulama ağ geçidi Web uygulaması güvenlik duvarı](https://azure.microsoft.com/services/application-gateway/). 

![Azure DDoS koruması standart](./media/ddos-protection-overview/ddos-protection-overview-fig2.png)

DDoS koruması standart geliştirme, test ve üretim ortamlarında deneyin öneririz. Deneyimlerinizi hakkında geri bildirim sağlamak için aşağıdaki kaynakları kullanın:
- [Microsoft Azure Forumunda Azure DDoS koruması](https://feedback.azure.com/forums/905032-azure-ddos-protection). 
- [MSDN Forumu Azure DDoS koruması](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azureddosprotection)
- [Yığın taşması Azure DDos koruması](https://stackoverflow.com/tags/azure-ddos/info)

Destek sorunları için yapabileceğiniz [Azure destek bileti açmadan](../azure-supportability/how-to-create-azure-support-request.md). DDoS koruması standart önizlemede olmakla birlikte, destek en iyi çaba ilkesine göre sağlanır.

## <a name="types-of-ddos-attacks-that-ddos-protection-standard-mitigates"></a>DDoS koruması standart azaltır DDoS saldırı türleri

DDoS koruması standart bu tür saldırılar en aza indirebileceğiniz:

- **Volumetric saldırıları** -görünen meşru trafiğin önemli miktarda ağ katmanla bölgesini doldurmak için saldırı 's hedeftir. UDP sel, yükseltme sel ve diğer sahte paket sel içerir. DDoS koruması standart azaltır bu çok gigabayt saldırı ortaya çıkar ve bunları temizleme Azure'nın genel ağ ölçeği otomatik olarak kullanır. 
- **Protokol saldırıları** -bu saldırıların hedef erişilemez bir zayıflık Katman 3 ve katman 4 protokol yığınında yararlanarak işlenemiyor. İçerdiği Eşitlemeye baskın saldırıları, yansıma saldırılarını ve diğer Protokolü saldırılardan. DDoS koruması standart istemci ile etkileşim ve kötü amaçlı trafiği engelleyen tarafından kötü amaçlı ve yasal trafiği arasında ayrım yapma bu saldırıların azaltır. 
- **Uygulama katmanı saldırıları** -ana bilgisayarlar arasında veri aktarımını kesintiye web uygulama paketlerini bu saldırıların hedef. HTTP protokolü ihlali, SQL ekleme, siteler arası komut dosyası ve diğer katman 7 saldırılardan içerir. Bu saldırılara karşı koruma sağlamak için uygulama ağ geçidi WAF DDoS koruması standart ile kullanma. 

DDoS koruması standart VM'ler, iç yük dengeleyicileri ve uygulama ağ geçitleri ile ilişkili ortak IP dahil sanal ağ kaynaklarında korur. Uygulama ağ geçidi WAF SKU ile birlikte, DDoS koruması standart L7 azaltma özelliği için tam L3 sağlayabilir.

## <a name="ddos-protection-standard-features"></a>DDoS koruması standart özellikler

![DDoS işlevi](./media/ddos-protection-overview/ddos-overview-fig1.png)

DDoS koruması standart özellikleri içerir: 

- **Yerel platform tümleştirme:** DDoS koruması standart yerel olarak Azure tümleştirilmiştir ve Azure portal ve PowerShell üzerinden yapılandırmayı içerir. DDoS koruması standart kaynaklar ve kaynak yapılandırması bilir.
- **Her zaman açık trafiğini izleme:** izlenen 24 x DDoS saldırıları göstergeleri için arayan 7, uygulama trafik düzenlerini şunlardır. Koruma ilkeleri aşıldığında azaltma gerçekleştirilir.
- **Anahtar teslim koruma:** Basitleştirilmiş yapılandırma DDoS koruması standart etkin hemen sonra bir sanal ağdaki tüm kaynaklar hemen korur. Hiçbir müdahale veya kullanıcı tanımı gereklidir - algıladı bir kez DDoS koruması standart hemen ve otomatik olarak saldırı azaltır.
- **Uyarlamalı ayarlama:** akıllı trafiği profil zaman içerisinde, uygulamanızın trafiği öğrenir ve seçer ve hizmetiniz için en uygun profili güncelleştirir. Trafik zaman içinde değiştikçe profilin ayarlar.
- **Bir uygulama ağ geçidi ile L7 koruma L3:** uygulama ağ geçidi WAF özellikleri sağlayan tam yığını DDoS koruması.
- **Kapsamlı azaltma ölçeği:** üzerinden 60 farklı saldırı türleri azaltıldığından en büyük bilinen DDoS saldırılarına karşı korumak için genel kapasiteye sahip. 
- **Saldırı ölçümleri:** Summarized ölçümleri her saldırılara karşı Azure İzleyicisi aracılığıyla erişilebilir.
- **Saldırı Uyarı:** başlatma ve durdurma saldırının uyarıları yapılandırılabilir ve saldırı kullanıcının süre kullanarak yerleşik saldırı ölçümleri. OMS, Splunk, Azure Storage, e-posta ve Azure portalı gibi işletimsel, yazılımına uyarıları tümleştirin.
- **Maliyet garantisi:** veri aktarım ve uygulama genişleme hizmet iadeleri belgelenen DDoS saldırıları için.

## <a name="ddos-protection-standard-mitigation"></a>DDoS koruması standart azaltma

Microsoft'un DDoS koruması hizmeti gerçek trafiği kullanımı izleyicileri ve sürekli DDoS ilkede tanımlanan eşiklere karşılaştırır. Bu trafik eşiği aşıldığında DDoS azaltma otomatik olarak başlatılır. Trafik eşiğin altında geri döndüğünde, azaltma kaldırılır.

Azaltma sırasında DDoS koruması hizmeti tarafından korunan bir kaynağa doğru trafiği yönlendirilir ve birkaç denetimleri yapılır. Bu denetimler, genellikle aşağıdaki işlevi gerçekleştirir:

- Paket Internet belirtimlerine uygun ve hatalı biçimlendirilmiş olmadığından emin olun.
- Bu olası sahte bir paket olup olmadığını belirlemek için istemci ile etkileşim (örneğin: Eşitlemeye Auth veya Eşitlemeye tanımlama bilgisi veya onu kurtarmasını kaynağı için bir paket bırakma).
- Hız sınırı paketler başka bir zorlama yöntemi varsa gerçekleştirilebilir.

DDoS koruması blokları trafiği ve İleri hedeflenen hedef trafik kalan saldırı. Saldırı algılama birkaç dakika içinde Azure İzleyici ölçümleri kullanarak bildirilir. DDoS koruması standart telemetriyi günlüğü yapılandırarak, gelecekteki analiz için kullanılabilir seçenekleri için günlükleri yazabilirsiniz. DDoS koruması standart için Azure İzleyicisi'nde ölçüm verileri şu anda 30 gün boyunca tutulur.

Müşterilerin kendi DDoS saldırıları benzetimini yapmak için önermiyoruz. Müşterilerin bir DDoS istemek için bir destek kanalıyla bunun yerine, kullanabilirsiniz saldırı Azure ağ tarafından yürütülen benzetimi. Mühendisin DDoS saldırı (bağlantı noktaları, protokoller, hedef IP) ayrıntılarını düzenlemeniz ve test zamanlamak için bir zaman düzenlemek için sizinle iletişim kuracaktır.

## <a name="next-steps"></a>Sonraki adımlar

- DDoS koruması standart kullanarak yönetme hakkında daha fazla bilgi [Azure PowerShell](ddos-protection-manage-ps.md) veya [Azure portal](ddos-protection-manage-portal.md).
