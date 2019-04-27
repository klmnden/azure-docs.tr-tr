---
title: Azure Active Directory Uygulama proxy'si kullanılırken ağ topolojisi hakkında önemli noktalar | Microsoft Docs
description: Azure AD uygulama ara sunucusu kullanırken, ağ topolojisi konuları kapsar.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/28/2017
ms.author: celested
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4d80f58215b1a8f1b93db158cd2f47186ba6354a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60443458"
---
# <a name="network-topology-considerations-when-using-azure-active-directory-application-proxy"></a>Azure Active Directory Uygulama proxy'si kullanılırken ağ topolojisi hakkında önemli noktalar

Bu makalede, Azure Active Directory (Azure AD) uygulama proxy'si yayımlama ve uygulamalarınızı uzaktan erişim için kullanılırken ağ topolojisi hakkında önemli noktalar açıklanmaktadır.

## <a name="traffic-flow"></a>Trafik akışı

Azure AD uygulama proxy'si aracılığıyla uygulama yayınlandıktan sonra uygulamaları kullanıcılardan gelen trafiği üç bağlantıları üzerinden akar:

1. Azure AD uygulama proxy'si hizmeti genel uç noktaya azure'da kullanıcı bağlanır.
2. Uygulama proxy'si hizmeti için uygulama Proxy Bağlayıcısı bağlanır.
3. Hedef uygulama için uygulama Proxy Bağlayıcısı bağlanır

![Trafik akışı, hedef uygulamaya kullanıcıdan gösteren diyagram](./media/application-proxy-network-topology/application-proxy-three-hops.png)

## <a name="tenant-location-and-application-proxy-service"></a>Kiracı konumu ve uygulama proxy'si hizmeti

Azure AD kiracısı için kaydolduğunuzda, kiracınızın bölge belirttiğiniz ülkeye göre belirlenir. Uygulama Ara sunucusunu etkinleştirme, kiracınız için uygulama proxy'si hizmeti örnekleri seçilen veya Azure AD kiracınızla aynı bölgede ya da kendisine en yakın bölgede oluşturulur.

Örneğin, Birleşik Krallık, Azure AD kiracınızın ülke veya bölge ise, tüm uygulama ara sunucusu bağlayıcıları AB veri merkezlerinde hizmet örneklerini kullanın. Kullanıcılar erişiminizi yayımlanan uygulamaları, uygulama proxy'si hizmeti örnekleri bu konumda trafiklerini geçer.

## <a name="considerations-for-reducing-latency"></a>Gecikme süresini azaltmak için dikkat edilmesi gerekenler

Tüm proxy çözümleri ağ bağlantınızı gecikmelere neden. Hangi Ara sunucu veya VPN çözümü uzaktan erişim çözümünüzü seçtiğiniz ne olursa olsun, her zaman kurumsal ağınızdaki bağlantısını etkinleştirme sunucular kümesi içerir.

Kuruluşlar, genellikle kendi çevre ağında sunucu uç noktaları içerir. Azure AD uygulama proxy'si ile bağlayıcıları şirket ağınızda bulunması sırasında trafik ancak, bulut proxy hizmeti aracılığıyla akar. Çevre ağ gereklidir.

Sonraki bölümlerde daha gecikme süresini azaltmanıza yardımcı olmak üzere ek öneriler içerir. 

### <a name="connector-placement"></a>Bağlayıcı yerleştirme

Uygulama proxy'si, Kiracı konumlarına göre sizin için örnekleri konumunu seçer. Ancak, bağlayıcı'yı yüklemeye karar ağ trafiğinizin gecikme özelliklerini tanımlamak için faaliyetlerine sahip olursunuz.

Uygulama proxy'si hizmeti ayarlanıyor, için aşağıdaki soruları sormaya:

* Uygulamayı nerede bulunur?
* Uygulama erişim çoğu kullanıcılar nerede bulunuyor?
* Uygulama Ara sunucusu örneği nerede bulunur?
* Zaten bir Azure veri merkezleri, Azure ExpressRoute veya benzer bir VPN gibi ayarlama Adanmış ağ bağlantısı gerekiyor?

Bağlayıcı, hem Azure hem de uygulamalarınızı (Adım 2 ve 3. trafik akışı diyagramı) ile bağlayıcı etkiler yerleşimini bu iki bağlantı gecikme süresini şekilde iletişim kurması gerekir. Bağlayıcı yerleşimini değerlendirirken, aşağıdaki noktaları göz önünde bulundurun:

* Kerberos Kısıtlı temsilci (KCD) için çoklu oturum açmayı kullanmak istiyorsanız, bağlayıcı görebilmesi için bir veri merkezi gerekir. Ayrıca, bağlayıcı sunucunun etki alanına katılmış olması gerekir.  
* Şüpheye düştüğünüzde uygulamaya yakın bağlayıcısı yükleyin.

### <a name="general-approach-to-minimize-latency"></a>Gecikme süresini en aza indirmek için genel yaklaşım

Her ağ bağlantısı iyileştirerek uçtan uca trafik gecikme süresini en aza indirebilirsiniz. Her bağlantı iyileştirilebilir:

* Atlama iki ucunu arasındaki uzaklığı azaltır.
* Geçiş için doğru ağ seçme. Örneğin, genel Internet yerine özel bir ağa geçiş nedeniyle ayrılmış bağlantılar daha hızlı olabilir.

Kurumsal ağınız ile Azure arasında adanmış bir VPN veya ExpressRoute bağlantısı varsa, kullanmak isteyebilirsiniz.

## <a name="focus-your-optimization-strategy"></a>En iyi duruma getirme stratejinizi odaklanın

Kullanıcılarınızın uygulama proxy'si hizmeti arasında bağlantı kontrol etmek için yapabileceğiniz çok az yoktur. Kullanıcılar, ev ağı, bir kafeterya veya farklı bir ülkede uygulamalarınızı erişebilir. Bunun yerine, uygulama proxy'si hizmeti uygulaması Ara sunucusu bağlayıcıları bağlantılara iyileştirebilirsiniz. Ortamınızı şu desenlerden eklemeyi göz önünde bulundurun.

### <a name="pattern-1-put-the-connector-close-to-the-application"></a>1. Desen: Uygulamayı yakın bağlayıcısını yerleştirme

Müşteri ağ yakın hedef uygulama Bağlayıcısı'nı koyun. Bu yapılandırma bağlayıcı ve uygulamayı Kapat olduğundan topografi diyagram 3 adımda en aza indirir. 

Bağlayıcınızı görebilmesi için etki alanı denetleyicisi gerekiyorsa, bu düzen avantajlıdır. Çoğu senaryo için iyi çalıştığı için müşterilerimizin çoğu bu düzeni kullanın. Bu düzen Ayrıca hizmet Bağlayıcısı'nı arasındaki trafiğin iyileştirilmesi 2 deseni ile birleştirilebilir.

### <a name="pattern-2-take-advantage-of-expressroute-with-microsoft-peering"></a>2. Desen: Microsoft eşlemesi ile ExpressRoute avantajlarından yararlanın

Microsoft eşlemesi ile ayarlanan ExpressRoute varsa, uygulama ara sunucusu Bağlayıcısı'nı arasındaki trafik için daha hızlı ExpressRoute bağlantısı kullanabilirsiniz. Bağlayıcı, uygulamanın yakın ağınıza açık durumdadır.

### <a name="pattern-3-take-advantage-of-expressroute-with-private-peering"></a>3. Desen: ExpressRoute özel eşlemesi avantajlarından yararlanın

Adanmış bir VPN veya ExpressRoute Azure ve şirket ağınız arasında özel eşlemesi ile ayarlanan varsa, başka bir seçeneğiniz vardır. Bu yapılandırmada, Azure sanal ağında genellikle bir şirket ağına uzantı olarak kabul edilir. Bu nedenle Azure veri merkezinde bağlayıcıyı yükleyin ve yine de bağlayıcı uygulama bağlantının düşük gecikme süresi gereksinimleri karşılaması.

Trafik, ayrılmış bir bağlantı üzerinden akan çünkü gecikme gizliliğinin tehlikeye atılmasını. Bir Azure veri merkezinde, Azure AD Kiracı konumu yakın Bağlayıcısı yüklü olmadığından uygulama proxy'si Hizmeti Bağlayıcısı gecikme süresini kısaltmak de alırsınız.

![Azure veri merkezinde yüklü bağlayıcı gösteren diyagram](./media/application-proxy-network-topology/application-proxy-expressroute-private.png)

### <a name="other-approaches"></a>Diğer yaklaşımlar

Bu makalenin odak bağlayıcı yerleştirme olsa da, daha iyi gecikme özelliklerini elde etmek için uygulamaya yerleşimini de değiştirebilirsiniz.

Giderek, kuruluşların kendi ağları barındırılan ortamlara taşınıyor. Bu uygulamalarını aynı zamanda, Kurumsal ağın parçası olan barındırılan bir ortamda yerleştirin ve etki alanı içinde devam etmelerini sağlar. Bu durumda, önceki bölümlerde ele desenleri, yeni uygulama konumuna uygulanabilir. Bu seçeneği değerlendirmeyi planlamaktadır olup [Azure AD Domain Services](../../active-directory-domain-services/active-directory-ds-overview.md).

Ayrıca, bağlayıcılarınızı kullanarak düzenleme göz önünde bulundurun [bağlayıcı grupları](application-proxy-connector-groups.md) farklı konumlara ve ağları olan hedef uygulamalar için. 

## <a name="common-use-cases"></a>Genel kullanım örnekleri

Bu bölümde, bazı yaygın senaryolar üzerinden inceleyeceğiz. Varsayımında Azure AD kiracısı (ve bu nedenle proxy hizmet uç noktası) Amerika Birleşik Devletleri (ABD) bulunur. Çalışmalarını diğer bölgelerle dünya için de geçerlidir. Bu kullanımı ele konuları.

Bu senaryolar için her bağlantı bir "durak" arayın ve bunları daha kolay tartışmak için sayı:

- **1 atlama**: Kullanıcıya uygulama proxy'si hizmeti
- **2 atlama**: Uygulama Proxy Bağlayıcısı için uygulama proxy'si hizmeti
- **3 atlama**: Hedef uygulama için uygulama ara sunucusu Bağlayıcısı 

### <a name="use-case-1"></a>Kullanım örneği 1

**Senaryo:** Kullanıcıları aynı bölgede olan bir kuruluşun ağındaki ABD uygulamasıdır. Herhangi bir ExpressRoute veya VPN kurumsal ağ ve Azure veri merkezi arasında yok.

**Öneri:** Önceki bölümde açıklanan düzeni, 1 izleyin. İçin gecikme süresini kısaltmak, ExpressRoute kullanılarak gerekirse göz önünde bulundurun.

Bu basit bir desendir. Atlama 3, bağlayıcı uygulama yakın yerleştirerek iyileştirin. Bağlayıcı genellikle görebilmesi KCD işlemleri gerçekleştirmek için bir veri merkezine ve uygulama ile birlikte yüklenir ayrıca doğal bir seçim olmasıdır.

![Kullanıcılar, proxy, bağlayıcı ve uygulama tüm ABD atandığını gösteren diyagram](./media/application-proxy-network-topology/application-proxy-pattern1.png)

### <a name="use-case-2"></a>Kullanım örneği 2

**Senaryo:** Bir kuruluşun ağındaki ABD'deki, dünya çapında yayılmış kullanıcılarla uygulamasıdır. Herhangi bir ExpressRoute veya VPN kurumsal ağ ve Azure veri merkezi arasında yok.

**Öneri:** Önceki bölümde açıklanan düzeni, 1 izleyin. 

Yeniden yaygın atlama 3, en iyi duruma getirme Bağlayıcısı'nı uygulama yakın yerleştirdiğiniz modelidir. Atlama 3 tümü aynı bölge içinde ise genellikle pahalı değil. Ancak, dünya genelinde kullanıcılar uygulama ara Sunucusu örneğinde ABD erişmeniz gerekir çünkü atlama 1 kullanıcının olduğu bağlı olarak, daha pahalı olabilir. Bu, herhangi bir proxy çözümüne genel dağılmış kullanıcılar ile ilgili benzer özelliklere sahip olduğunu hatalarının ayıklanabileceğini belirtmekte yarar.

![Kullanıcılar genel olarak yayılır, ancak proxy, bağlayıcı ve uygulama ABD'de gösteren diyagram](./media/application-proxy-network-topology/application-proxy-pattern2.png)

### <a name="use-case-3"></a>Kullanım örneği 3

**Senaryo:** ABD'deki bir kuruluşun ağındaki uygulamasıdır. Microsoft eşlemesi ile ExpressRoute, Azure ve şirket ağı arasında yok.

**Öneri:** 1 ve 2 ' nin önceki bölümde açıklanan desenlerini izleyin.

İlk olarak, bağlayıcı olabildiğince uygulamaya mümkün olduğunca yakın yerleştirin. Ardından, sistem otomatik olarak 2 atlama için Expressroute'u kullanır. 

Microsoft eşleme ExpressRoute bağlantı kullanıyorsanız, proxy ve bağlayıcı arasındaki trafik, bağlantı üzerinden akar. Atlama 2, gecikme süresi en iyi hale getirdi.

![ExpressRoute proxy ve bağlayıcı arasında gösteren diyagram](./media/application-proxy-network-topology/application-proxy-pattern3.png)

### <a name="use-case-4"></a>Kullanım örneği 4

**Senaryo:** ABD'deki bir kuruluşun ağındaki uygulamasıdır. ExpressRoute özel eşlemesi, Azure ve şirket ağı arasında yok.

**Öneri:** Önceki bölümde açıklanan düzeni 3 ' ü izleyin.

Azure veri merkezine ExpressRoute özel eşlemesi üzerinden şirket ağına bağlı Bağlayıcısı'nı koyun. 

Bağlayıcısı Azure veri merkezinde yerleştirilebilir. Bağlayıcı görebilmesi için uygulama ve özel ağ üzerinden veri merkezi hala olduğundan, atlama 3 en iyi duruma getirilmiş kalır. Ayrıca, atlama 2 daha da İyileştirildi.

![Bağlayıcı Azure veri merkezi ve ExpressRoute bağlayıcı ve uygulama arasında gösteren diyagram](./media/application-proxy-network-topology/application-proxy-pattern4.png)

### <a name="use-case-5"></a>Kullanım örneği 5

**Senaryo:** Bir kuruluşun ağındaki uygulama ara sunucusu örneği ve kullanıcıların çoğu ABD'deki AB uygulamasıdır.

**Öneri:** Uygulamayı yakın Bağlayıcısı'nı koyun. Uygulama Ara sunucusu örneği aynı bölgede olması erişen ABD kullanıcı için atlama 1 çok pahalı değil. Atlama 3 optimize edilmiştir. Atlama 2 iyileştirmek için ExpressRoute kullanmayı düşünün. 

![ABD'de Bağlayıcısı ve AB'deki uygulama ile kullanıcı ve proxy gösteren diyagram](./media/application-proxy-network-topology/application-proxy-pattern5b.png)

Ayrıca, bu durumda bir değişken kullanarak göz önünde bulundurun. Sonra büyük olasılıkla, kuruluştaki kullanıcıların çoğu ABD'de varsa, ağınızı ABD için genişletir. ABD'de bağlayıcı yerleştirin ve uygulamaya ayrılmış iç kurumsal ağa satırı AB'de kullanın. 2. ve 3 Bu şekilde atlama en iyi duruma getirilir.

![ABD, AB uygulamada kullanıcılar ve proxy Bağlayıcısı'nı gösteren diyagram](./media/application-proxy-network-topology/application-proxy-pattern5c.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Uygulama Ara sunucusunu etkinleştirme](application-proxy-add-on-premises-application.md)
- [Çoklu oturum açmayı etkinleştirme](application-proxy-configure-single-sign-on-with-kcd.md)
- [Koşullu erişimi etkinleştirme](application-proxy-integrate-with-sharepoint-server.md)
- [Uygulama Ara sunucusu ile ilgili sorunları giderme](application-proxy-troubleshoot.md)
