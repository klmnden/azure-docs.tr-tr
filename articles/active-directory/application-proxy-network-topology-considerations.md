---
title: "Azure Active Directory Uygulama proxy'si kullanırken ağ topolojisi hakkında önemli noktalar | Microsoft Docs"
description: "Azure AD uygulama proxy'si kullanırken ağ topolojisi konuları kapsar."
services: active-directory
documentationcenter: 
author: kgremban
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 62b28ba446b4ffb13071b81805767bb34288fe2e
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="network-topology-considerations-when-using-azure-active-directory-application-proxy"></a>Azure Active Directory Uygulama proxy'si kullanırken ağ topolojisi hakkında önemli noktalar

Bu makalede, Azure Active Directory (Azure AD) uygulama proxy'si yayımlama ve uygulamalarınızı uzaktan erişim için kullanırken ağ topolojisi konuları açıklanmaktadır.

## <a name="traffic-flow"></a>Trafik akışı

Azure AD uygulama proxy'si aracılığıyla uygulama yayımlandığında, kullanıcıların trafiğinden uygulamalara üç bağlantıları üzerinden akar:

1. Azure üzerinde Azure AD uygulama proxy'si hizmet ortak uç kullanıcı bağlanır
2. Uygulama proxy'si hizmeti uygulaması Ara sunucusu Bağlayıcısı'nı bağlanır
3. Uygulama Ara sunucusu Bağlayıcısı hedef uygulaması'na bağlanır

![Hedef uygulama için kullanıcıdan trafik akışını gösteren diyagram](./media/application-proxy-network-topologies/application-proxy-three-hops.png)

## <a name="tenant-location-and-application-proxy-service"></a>Kiracı konumu ve uygulama proxy'si hizmeti

Azure AD kiracısı için kaydolduğunuzda, kiracınızın bölge belirttiğiniz ülkeye göre belirlenir. Uygulama proxy'si etkinleştirdiğinizde, kiracınız için uygulama proxy'si hizmet örnekleri seçilen veya Azure AD kiracınıza aynı bölgede ya da kendisine en yakın bölgeyi oluşturuldu.

Örneğin, Azure AD kiracınıza ait bölge Avrupa Birliği (AB) ise, tüm uygulama proxy'si bağlayıcılar AB Azure veri merkezlerinde de hizmet örneği kullanın. Kullanıcılara erişim uygulamaları yayımlandığında, trafiği, bu konumda uygulama proxy'si hizmet örnekleri geçer.

## <a name="considerations-for-reducing-latency"></a>Gecikme dikkat edilmesi gereken noktalar

Tüm proxy çözümleri gecikme ağ bağlantınızı tanıtır. Hangi proxy ya da VPN çözümü uzaktan erişim çözümünüzü seçtiğiniz olsun, bu her zaman bağlantı Kurumsal ağınızdaki etkinleştirme sunucular kümesi içerir.

Kuruluşlar, genellikle kendi çevre ağında sunucu uç noktalarını içerir. Azure AD uygulama proxy'si ile bağlayıcıları Kurumsal ağınızda bulunması sırasında trafiği ancak, bulut proxy hizmeti aracılığıyla akar. Çevre ağı gereklidir.

Sonraki bölümlerde daha gecikme süresini azaltmanıza yardımcı olmak için ek öneriler içerir. 

### <a name="connector-placement"></a>Bağlayıcı yerleştirme

Uygulama proxy'si, Kiracı konumunuza göre örnekleri konumunu seçer. Ancak, bağlayıcı yükleneceği karar vermek, ağ trafiğinizi gecikme özelliklerini tanımlamak için güç vermiş olursunuz.

Uygulama proxy'si hizmetini kurmadan, aşağıdaki soruları sormaya:

* Uygulama nerede?
* Uygulama erişen kullanıcıların çoğunun bulunduğu?
* Uygulama proxy'si örneği nerede?
* Azure veri merkezleri, Azure ExpressRoute veya benzer bir VPN gibi ayarlamak için adanmış ağ bağlantısı zaten var mı?

Bağlayıcı, hem Azure hem de uygulamalarınızı (Adım 2 ve 3'te trafik akışı diyagramı) ile bağlayıcı etkiler yerleşimini bu iki bağlantı gecikmesi şekilde iletişim kurması gerekir. Bağlayıcı yerleşimini değerlendirirken, aşağıdaki noktaları göz önünde bulundurun:

* Çoklu oturum açma için kısıtlı Kerberos temsilci (KCD) kullanmak istiyorsanız, bağlayıcı bir görüş veri merkezine gerekir. Ayrıca, bağlayıcı sunucusu etki alanına katılmış olması gerekir.  
* Şüpheli olduğunda, bağlayıcıyı yakın uygulamayı yükleyin.

### <a name="general-approach-to-minimize-latency"></a>Gecikme süresi en aza indirmek için genel yaklaşım

Her ağ bağlantısı iyileştirerek uçtan uca trafik gecikme süresini en aza indirebilirsiniz. Her bağlantı tarafından iyileştirilebilir:

* İki atlama ucunun arasındaki uzaklığı azaltır.
* Geçiş yapmak için doğru ağ seçme. Örneğin, genel Internet yerine özel bir ağa geçiş adanmış bağlantılar nedeniyle daha hızlı olabilir.

Azure ile şirket ağınız arasında ayrılmış bir VPN ya da ExpressRoute bağlantı varsa, kullanmak isteyebilirsiniz.

## <a name="focus-your-optimization-strategy"></a>En iyi duruma getirme stratejinizi odaklanın

Kullanıcılarınızın ve uygulama proxy'si hizmeti arasındaki bağlantıyı denetlemek için yapabileceğiniz az yoktur. Kullanıcılar, uygulamalarınızı ev ağı, bir kafeterya veya farklı bir ülkede erişebilir. Bunun yerine, uygulamalar için uygulama proxy'si bağlayıcılar için uygulama proxy'si hizmeti bağlantılarından iyileştirebilirsiniz. Ortamınızdaki aşağıdaki desenleri eklemeyi göz önünde bulundurun.

### <a name="pattern-1-put-the-connector-close-to-the-application"></a>Deseni 1: bağlayıcı uygulama yakın yerleştirme

Hedef uygulama yakın bağlayıcı müşteri ağ yerleştirin. Bağlayıcı ve uygulama Kapat olduğundan 3. adım topografi diyagramda bu yapılandırma en aza indirir. 

Bağlayıcınızı bir görüş etki alanı denetleyicisine gerekiyorsa, bu deseni avantajlıdır. Çoğu senaryo için iyi çalışır olduğundan müşterilerimizin çoğu bu deseni kullanır. Bu deseni da Desen hizmet Bağlayıcısı'nı arasındaki trafiği en iyi duruma getirmek için 2 ile birleştirilebilir.

### <a name="pattern-2-take-advantage-of-expressroute-with-public-peering"></a>Desen 2: Genel eşliği ile ExpressRoute yararlanın

ExpressRoute genel eşliği ile ayarlanan varsa, uygulama proxy'si ve bağlayıcı arasındaki trafik için daha hızlı ExpressRoute bağlantı kullanabilirsiniz. Hala ağınızdaki uygulama yakın Bağlayıcıdır.

### <a name="pattern-3-take-advantage-of-expressroute-with-private-peering"></a>Desen 3: özel eşleme ile ExpressRoute yararlanın

Ayrılmış bir VPN veya şirket ağınız ve Azure arasında özel eşleme ile ExpressRoute ayarlanan varsa, başka bir seçeneğiniz vardır. Bu yapılandırmada, Azure sanal ağında genellikle şirket ağının bir uzantısı olarak kabul edilir. Bu nedenle Azure veri merkezinde Bağlayıcısı'nı yükleyin ve hala bağlayıcı uygulama bağlantısının düşük gecikme gereksinimleri karşılamak.

Ayrılmış bir bağlantı üzerinden trafik akışı için gecikme tehlikeye değil. Bağlayıcı Azure AD Kiracı konumunuz yakın bir Azure veri merkezinde yüklü olduğundan, ayrıca geliştirilmiş uygulama proxy'si Hizmeti Bağlayıcısı gecikme alırsınız.

![Azure veri merkezi içinde yüklü bağlayıcı gösteren diyagram](./media/application-proxy-network-topologies/application-proxy-expressroute-private.png)

### <a name="other-approaches"></a>Diğer yaklaşımlar

Bu makaleyi odağını bağlayıcı yerleştirme olsa da, daha iyi gecikme özelliklerini almak için uygulama yerleşimini de değiştirebilirsiniz.

Giderek, kuruluşların kendi ağları barındırılan ortamlara taşıyor. Bu, bunları uygulamalarını şirket ağlarına parçası olduğunu da barındırılan bir ortamda ve etki alanı içinde devam sağlar. Bu durumda, önceki bölümlerde ele desenleri, yeni uygulama konumuna uygulanabilir. Bu seçenek düşünüyorsanız, bkz: [Azure AD etki alanı Hizmetleri](../active-directory-domain-services/active-directory-ds-overview.md).

Ayrıca, kullanma, bağlayıcılar düzenleme göz önünde bulundurun [bağlayıcı grupları](active-directory-application-proxy-connectors-azure-portal.md) farklı konumlarda ve ağlar hedef uygulamalar için. 

## <a name="common-use-cases"></a>Genel kullanım örnekleri

Bu bölümde, birkaç yaygın senaryolar üzerinden yol. Varsayımında Azure AD kiracısı (ve bu nedenle proxy hizmeti uç noktası) Amerika Birleşik Devletleri (ABD) bulunur. Durumları dünyanın diğer bölgeler için de geçerlidir. Bu kullanımda ele alınan durumları.

Bu senaryolar için her bağlantının bir "durak" çağırın ve daha kolay tartışma için sayı:

- **1 atlama**: kullanıcı için uygulama proxy'si hizmeti
- **2 atlama**: uygulama ara sunucusu Bağlayıcısı için uygulama proxy'si hizmeti
- **3 atlama**: Hedef uygulama için uygulama ara sunucusu Bağlayıcısı 

### <a name="use-case-1"></a>Kullanım örneği 1

**Senaryo:** ağındaki bir kuruluşun ABD aynı bölgede kullanıcılarla uygulama olur. ExpressRoute ya da VPN Azure veri merkezi ve şirket ağı arasında mevcut.

**Öneri:** izleyin düzeni 1, önceki bölümde açıklanan. Gerekirse, geliştirilmiş gecikme için ExpressRoute kullanmayı düşünün.

Basit bir desen budur. Uygulama yakın bağlayıcı koyarak atlama 3 iyileştirin. Bağlayıcı genellikle görüş uygulama ve KCD işlemlerini gerçekleştirmek için veri merkezi ile yüklenen ayrıca doğal bir seçim olmasıdır.

![Kullanıcılar, proxy, bağlayıcı ve uygulama tüm ABD'de olduğunu gösteren diyagram](./media/application-proxy-network-topologies/application-proxy-pattern1.png)

### <a name="use-case-2"></a>Kullanım örneği 2

**Senaryo:** ABD kuruluş ağındaki genel dağılmış kullanıcılarla uygulamadır. ExpressRoute ya da VPN Azure veri merkezi ve şirket ağı arasında mevcut.

**Öneri:** izleyin düzeni 1, önceki bölümde açıklanan. 

Yeniden ortak Düzen atlama 3, en iyi duruma getirme Bağlayıcısı'nı uygulama yakın nereye yöneliktir. Atlama 3 tüm aynı bölge içinde ise genellikle pahalı değil. Ancak, kullanıcıların dünya çapında ABD uygulama proxy'si örneğinde erişmesi gereken çünkü atlama 1 kullanıcının bulunduğu bağlı olarak, daha pahalı olabilir. Bu, herhangi bir proxy çözümü genel dağılmış kullanıcılar ilgili benzer özelliklere sahip dikkate değerdir.

![Kullanıcılar genel yayılır, ancak proxy, bağlayıcı ve uygulama ABD'de gösteren diyagram](./media/application-proxy-network-topologies/application-proxy-pattern2.png)

### <a name="use-case-3"></a>Kullanım örneği 3

**Senaryo:** ABD kuruluş ağındaki uygulama olur. Ortak eşleme ile ExpressRoute, Azure ve şirket ağı arasında mevcut.

**Öneri:** desenleri 1 ve 2 ' nin önceki bölümde açıklanan izleyin.

İlk olarak, olabildiğince uygulama mümkün olduğunca yakın Bağlayıcısı'nı koyun. Ardından, sistem atlama 2 ExpressRoute otomatik olarak kullanır. 

ExpressRoute bağlantı ortak eşleme kullanılıyorsa, proxy ve bağlayıcı arasındaki trafiğin o bağlantı üzerinden akar. Atlama 2 gecikme optimize etti.

![ExpressRoute proxy ve bağlayıcı arasında gösteren diyagram](./media/application-proxy-network-topologies/application-proxy-pattern3.png)

### <a name="use-case-4"></a>Kullanım örneği 4

**Senaryo:** ABD kuruluş ağındaki uygulama olur. Özel eşleme ile ExpressRoute, Azure ve şirket ağı arasında mevcut.

**Öneri:** izleyin düzeni 3, önceki bölümde açıklanan.

ExpressRoute özel eşleme aracılığıyla şirket ağına bağlı Azure veri merkezinde Bağlayıcısı'nı koyun. 

Bağlayıcı Azure veri merkezinde yerleştirilebilir. Bağlayıcı hala uygulama ve özel ağ üzerinden veri merkezi için bir satır görüş olduğundan, atlama 3 en iyi duruma getirilmiş kalır. Ayrıca, atlama 2 daha iyi duruma getirilmiştir.

![Bağlayıcı Azure veri merkezinde ve ExpressRoute Bağlayıcısı ve uygulama arasında gösteren diyagram](./media/application-proxy-network-topologies/application-proxy-pattern4.png)

### <a name="use-case-5"></a>Kullanım örneği 5

**Senaryo:** uygulama proxy'si örneği ve çoğu kullanıcılarla ABD AB kuruluş ağındaki uygulama olur.

**Öneri:** uygulama yakın Bağlayıcısı'nı koyun. Uygulama proxy'si örneği aynı bölgede olması olur erişen ABD kullanıcı olduğundan atlama 1 çok pahalı değil. Atlama 3 getirilmiştir. Atlama 2 en iyi duruma getirmek için ExpressRoute kullanmayı düşünün. 

![Bağlayıcı ve uygulama AB ABD kullanıcıları ve proxy gösteren diyagram](./media/application-proxy-network-topologies/application-proxy-pattern5b.png)

Ayrıca, bu durumda bir değişken kullanarak göz önünde bulundurun. Olasılığı olan sonra kuruluşunuzdaki kullanıcıların çoğunun ABD'de ise ağınız ABD için genişletir. ABD'de Bağlayıcısı'nı koyun ve uygulamaya özel iç kurumsal ağ satır AB kullanın. Bu şekilde atlama 2 ve 3 en iyi duruma getirilir.

![AB uygulamada ABD kullanıcıları, proxy ve bağlayıcı gösteren diyagram](./media/application-proxy-network-topologies/application-proxy-pattern5c.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Uygulama Ara sunucusunu etkinleştirme](active-directory-application-proxy-enable.md)
- [Çoklu oturum açmayı etkinleştirme](active-directory-application-proxy-sso-using-kcd.md)
- [Koşullu erişimi etkinleştirme](application-proxy-enable-remote-access-sharepoint.md)
- [Uygulama Ara sunucusu ile ilgili sorunları giderme](active-directory-application-proxy-troubleshoot.md)
