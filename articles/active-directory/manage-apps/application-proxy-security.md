---
title: Azure AD uygulama proxy'si için güvenlik konuları | Microsoft Docs
description: Azure AD uygulama proxy'si kullanımıyla ilgili güvenlik konuları kapsar
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
ms.date: 09/08/2017
ms.author: celested
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7bb9fc806779565581fa7667749402f5608edd80
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60292767"
---
# <a name="security-considerations-for-accessing-apps-remotely-with-azure-ad-application-proxy"></a>Uygulamaları Azure AD uygulama proxy'si ile uzaktan erişim için güvenlik konuları

Bu makalede, kullanıcılar ve uygulamalar, Azure Active Directory Uygulama proxy'si kullandığınızda güvenli tutmak için çalışan bileşenleri açıklanmaktadır.

Aşağıdaki diyagramda gösterildiği Azure AD, şirket içi uygulamalara güvenli uzaktan erişim sağlar.

 ![Azure AD uygulama proxy'si aracılığıyla güvenli uzaktan erişim diyagramı](./media/application-proxy-security/secure-remote-access.png)

## <a name="security-benefits"></a>Güvenlik açısından faydalı

Azure AD uygulama ara sunucusu, aşağıdaki güvenlik avantajları sunar:

### <a name="authenticated-access"></a>Kimliği doğrulanmış erişim 

Ardından Azure Active Directory ön kimlik doğrulaması kullanmayı seçerseniz, yalnızca kimliği doğrulanmış bağlantılar, ağ erişebilirsiniz.

Azure AD güvenlik belirteci hizmeti (STS) tüm kimlik doğrulaması için Azure AD uygulama ara sunucusu kullanır.  Arka uç uygulaması yalnızca kimliği doğrulanmış kimliğe erişebildiğinden ön kimlik doğrulaması yapısı gereği, çok sayıda anonim saldırıları engeller.

Geçişli ön kimlik doğrulaması yönteminiz olarak seçerseniz, bu avantaj elde etmezsiniz. 

### <a name="conditional-access"></a>Koşullu erişim

Ağ bağlantıları kurulan önce daha zengin ilke denetimleri uygulayın.

İle [koşullu erişim](../conditional-access/overview.md), trafiğin hangi arka uç uygulamalarınızı erişmesine izin verilip kısıtlamalar tanımlayabilirsiniz. Oturum açma kimlik doğrulaması ve kullanıcı riski profili gücünü konuma göre kısıtlayan ilkeler oluşturabilirsiniz.

Koşullu erişim, bir güvenlik katmanı, kullanıcı kimlik doğrulamalarına eklenmesinden çok faktörlü kimlik doğrulaması ilkeleri yapılandırmak için de kullanabilirsiniz. Ayrıca, uygulamalarınızı ayrıca Microsoft Cloud App Security'ye üzerinden gerçek zamanlı izleme ve denetim sağlamak için Azure AD koşullu erişim aracılığıyla yönlendirilebilir [erişim](https://docs.microsoft.com/cloud-app-security/access-policy-aad) ve [oturumu](https://docs.microsoft.com/cloud-app-security/session-policy-aad) ilkeleri

### <a name="traffic-termination"></a>Trafik sonlandırma

Tüm trafiği, bulutta sonlandırılır.

Azure AD uygulama proxy'si bir ters proxy olduğundan, tüm trafiği arka uç uygulamaları için hizmeti sonlandırıldı. Oturum, arka uç sunucuları doğrudan HTTP trafiğine gösterilmez anlamına gelir arka uç sunucusuyla yeniden. Bu yapılandırma daha iyi hedeflenmiş saldırılara karşı korumalı anlamına gelir.

### <a name="all-access-is-outbound"></a>Tüm erişim giden 

Kurumsal ağa gelen bağlantı açmanız gerekmez.

Uygulama Proxy bağlayıcıları yalnızca gelen bağlantılar için güvenlik duvarı bağlantı noktalarını açmanız gerek yoktur anlamına gelir. Azure AD uygulama ara Sunucusu hizmetine giden bağlantıları kullanın. Geleneksel proxy'leri bir çevre ağına gerekli (olarak da bilinen *DMZ*, *sivil bölge*, veya *denetimli alt ağ*) ve kimlik doğrulamasız bağlantılar erişim izni Ağ ucunda. Bu senaryo, web uygulaması güvenlik duvarı ürünleri trafiği analiz edin ve ortam korumak için yaptığı Yatırımlar gereklidir. Tüm bağlantıları giden ve güvenli bir kanal üzerinden gerçekleşmesi için uygulama ara sunucusu ile bir çevre ağına gerekmez.

Bağlayıcılar hakkında daha fazla bilgi için bkz. [anlamak Azure AD uygulama ara sunucusu bağlayıcıları](application-proxy-connectors.md).

### <a name="cloud-scale-analytics-and-machine-learning"></a>Bulut ölçekli analiz ve makine öğrenimi 

Son teknoloji ürünü güvenlik koruması alın.

Azure Active Directory'nin parçası olduğu için uygulama proxy'si yararlanabilir [Azure AD kimlik koruması](../active-directory-identityprotection.md), dijital Suçlar birimi ve Microsoft Security Response Center verilerle. Birlikte biz belirleyebildiğini ele geçirilen hesaplar ve riskli oturum açma işlemleri karşı koruma sağlar. Biz, hangi oturum açma denemesi yüksek riskli olduğunu belirlemek için çok sayıda etkene dikkate alın. Bu etkenler, bayrak virüs bulaşmış cihazlar, anonymizing ağlara ve alışılmadık veya olasılığı düşük konumu içerir.

Bu raporlar ve olaylar çoğunu zaten güvenlik bilgileri ve Olay yönetimi (SIEM) sistemlerine ile tümleştirmeye yönelik bir API aracılığıyla kullanılabilir.

### <a name="remote-access-as-a-service"></a>Hizmet olarak uzaktan erişim

Koruma ve şirket içi sunucular düzeltme eki uygulama hakkında endişelenmeniz gerekmez.

Çok sayıda saldırıları için hala yüklenmemiş yazılım hesaplar. Her zaman en son güvenlik düzeltme eklerinin ve yükseltmelerini almak için azure AD uygulama ara sunucusu, Microsoft sahibi, bir Internet ölçeğinde hizmetidir.

Azure AD uygulama proxy'si tarafından yayımlanan uygulamaların güvenliğini artırmak için biz web Gezgin robotları dizin oluşturma ve uygulamalarınızı arşivleme engelleyin. Bir web Gezgini robot yayınlanmış bir uygulama için robot kullanıcının ayarlarını almak için her çalıştığında uygulama ara sunucusu yanıt içeren bir robots.txt dosyasıyla `User-agent: * Disallow: /`.

### <a name="ddos-prevention"></a>DDOS önleme

Uygulama Proxy üzerinden yayımlanan uygulamalarla dağıtılmış engelleme, hizmet (DDOS) saldırılarına karşı korunur.

Uygulama proxy'si hizmeti, uygulamalar ve ağ ulaşmaya çalışırken trafik miktarını izler. Uygulamalarınıza uzaktan erişim isteğinde bulunan cihaz sayısını ani, Microsoft, ağ erişimi kısıtlar. 

Microsoft, tek tek uygulamalar için ve bir bütün olarak aboneliğiniz için trafik düzenlerini izler. Ardından bir uygulama normal istekleri daha yüksek alırsa, bu uygulamaya erişmek için istekleri kısa bir süre için reddedildi. Ardından, tüm abonelik üzerinden normal istekleri daha yüksek alırsanız, uygulamalarınıza erişmek için istekleri reddedilir. Böylece şirket içi kullanıcılarınız uygulamalarını erişme tutmak önleyici bu ölçüyü uygulama sunucularınızı uzaktan erişim istekleri tarafından aşırı tutar. 

## <a name="under-the-hood"></a>Başlık altında

Azure AD uygulama proxy'si iki bölümden oluşur:

* Bulut tabanlı hizmeti: Bu hizmet, Azure'da çalışan ve dış istemci/kullanıcı bağlantıları burada yapılan olduğu.
* [Şirket içi Bağlayıcısı'nı](application-proxy-connectors.md): Bir şirket içi Bileşen iç uygulamaları için Azure AD uygulama proxy'si hizmeti ve tanıtıcıları bağlantılardan gelen istekler için bağlayıcıyı dinler. 

Bir akış Bağlayıcısı ve uygulama proxy'si hizmeti arasında kurulan olduğunda:

* Bağlayıcıyı ilk kez ayarlanır.
* Bağlayıcı, uygulama proxy'si hizmeti yapılandırma bilgileri çeker.
* Bir kullanıcı, yayımlanan bir uygulamanın erişir.

>[!NOTE]
>Tüm iletişim SSL üzerinden yapılır ve uygulama proxy'si Hizmeti Bağlayıcısı, bunlar her zaman kaynaklanan. Yalnızca giden bir hizmettir.

Bağlayıcı, neredeyse tüm çağrıları için uygulama proxy'si hizmeti kimlik doğrulaması için bir istemci sertifikası kullanır. Bu işlem tek özel durumu ilk kurulumu, istemci sertifikasını burada kurulur adımdır.

### <a name="installing-the-connector"></a>Bağlayıcısını yükleme

Bağlayıcı ilk ayarlandığında, akış aşağıdakiler gerçekleşir:

1. Bağlayıcı kaydı için hizmet Bağlayıcısı'nı yüklemesinin bir parçası olarak'olmuyor. Kullanıcılar, Azure AD yönetici kimlik bilgilerini girmeniz istenir. Bu kimlik doğrulamasını edinilen belirteci daha sonra Azure AD uygulama ara Sunucusu hizmetine sunulur.
2. Uygulama proxy'si hizmeti, belirteci değerlendirir. Bu kullanıcı kiracıda bir şirket yöneticisi olup olmadığını denetler. Kullanıcı bir yönetici değilse, işlem sonlandırıldı.
3. Bağlayıcı, istemci sertifika isteği oluşturur ve, uygulama proxy'si hizmeti, belirteci ile birlikte geçirir. Hizmet sırayla belirteci doğrular ve istemci sertifika isteğini imzalar.
4. Bağlayıcı, uygulama proxy'si hizmeti ile gelecekteki iletişimi için istemci sertifikası kullanır.
5. Bağlayıcı sistem yapılandırma verilerinin ilk bir çekme istemci sertifikasını kullanarak hizmetinden gerçekleştirir ve istekleri almak artık hazırdır.

### <a name="updating-the-configuration-settings"></a>Yapılandırma ayarları güncelleştiriliyor

Uygulama proxy'si hizmeti yapılandırma ayarlarını güncelleştirdiğinde, akış aşağıdakiler gerçekleşir:

1. İstemci sertifikasını kullanarak uygulama proxy'si hizmeti dahilinde yapılandırma uç bağlayıcı bağlanır.
2. İstemci sertifikası doğrulandıktan sonra uygulama Proxy hizmetini yapılandırma verileri (örneğin, bağlayıcı parçası olması gereken bağlayıcı grubu) bağlayıcıya döndürür.
3. Geçerli sertifika 180 günden daha eski ise, bağlayıcı etkili bir şekilde 180 günde bir istemci sertifikasını güncelleştirmeleri yeni bir sertifika isteği oluşturur.

### <a name="accessing-published-applications"></a>Yayımlanan uygulamalara erişme

Kullanıcılar, yayımlanmış bir uygulama eriştiğinde, aşağıdaki olaylar uygulama proxy'si hizmeti ve uygulama Proxy Bağlayıcısı arasında gerçekleşir:

1. Hizmet uygulaması için kullanıcı kimliğini doğrular
2. Hizmet isteği bağlayıcı sırada yerleştirir.
3. İstek kuyruğu'ndan bir bağlayıcı işler
4. Bağlayıcı için bir yanıt bekler
5. Hizmet veri kullanıcı akışları

Bu adımların her biri bir yerde neler hakkında daha fazla bilgi edinmek için okumaya devam edin.


#### <a name="1-the-service-authenticates-the-user-for-the-app"></a>1. Hizmet uygulaması için kullanıcı kimliğini doğrular

Geçiş, ön kimlik doğrulama yöntemi kullanmak üzere uygulamayı yapılandırdıysanız, bu bölümdeki adımları atlanır.

Uygulamayı Azure AD'ye preauthenticate yapılandırdıysanız, kullanıcıların kimliğini doğrulamak için Azure AD STS yönlendirilirsiniz ve yerde aşağıdaki adımları uygulayın:

1. Koşullu erişim ilkesi gereksinimlere belirli uygulama için uygulama proxy'si denetler. Bu adım, uygulamaya atanmış olan kullanıcı sağlar. İki aşamalı doğrulama gerekliyse, ikinci bir kimlik doğrulama yöntemi için kullanıcı kimlik doğrulaması dizisi ister.

2. Tüm denetimleri geçtikten sonra STS Azure AD uygulama için imzalı bir belirteç verir ve kullanıcıyı uygulama ara Sunucusu hizmetine yönlendirir.

3. Uygulama proxy'si, belirtecin doğru uygulama için verildiğini doğrular. Bunu diğer Ayrıca, belirteç Azure AD tarafından imzalanmış ve hala geçerli pencere içinde olduğundan emin olduktan gibi denetler.

4. Uygulama proxy'si ayarlar söz konusu kimlik doğrulamasını uygulama belirtmek için bir şifrelenmiş kimlik doğrulama tanımlama bilgisi oluştu. Tanımlama bilgisi belirteci Azure AD'den ve kimlik doğrulamasını temel alan kullanıcı adı gibi diğer verileri temel alan bir sona erme zaman damgası içerir. Tanımlama bilgisinin yalnızca uygulama proxy'si hizmeti bilinen özel bir anahtarla şifrelenir.

5. Uygulama Ara sunucusu, kullanıcı başta istenen URL'ye yeniden yönlendirir.

Herhangi bir bölümünü ön kimlik doğrulama adımları başarısız olursa, kullanıcının isteği reddedildi ve kullanıcıya sorunun kaynağını belirten bir ileti gösterilir.


#### <a name="2-the-service-places-a-request-in-the-connector-queue"></a>2. Hizmet isteği bağlayıcı sırada yerleştirir.

Bağlayıcılar uygulama ara Sunucusu hizmetine bir giden bağlantı açık tutun. Bir istek geldiğinde, hizmet isteği açık bağlantılarından biri öğrenilip Bağlayıcısı sıralar.

İstek uygulama istek üstbilgileri, istek ve istek kimliğini yapan kullanıcının şifrelenmiş tanımlama bilgisi verileri gibi öğeleri içerir. İstekle birlikte gönderilen şifrelenmiş bir tanımlama bilgisi verileri de kimlik doğrulama tanımlama değil.

#### <a name="3-the-connector-processes-the-request-from-the-queue"></a>3. Bağlayıcı, sıradan bir isteği işler. 

İsteği temel alarak, uygulama ara sunucusu aşağıdaki eylemlerden birini gerçekleştirir:

* İstek basit bir işlemle ise (örneğin, bir RESTful ile olduğu gibi gövdesi içinde veri yok *alma* isteği), bağlayıcıyı hedef iç kaynak için bir bağlantı kurar ve bir yanıt bekler.

* İstek gövdesinde ilişkili veri varsa (örneğin, bir RESTful *POST* işlemi), bağlayıcı uygulama ara sunucusu örneğine istemci sertifikasını kullanarak bir giden bağlantı sağlar. İstek verilerini ve iç kaynak için bir bağlantı açmak için bu bağlantıyı kolaylaştırır. Bağlayıcısından isteği aldıktan sonra uygulama proxy'si hizmeti içeriği kullanıcıdan kabul etmesini başlar ve veri bağlayıcıya iletir. Bağlayıcı, iç kaynak verileri sırayla iletir.

#### <a name="4-the-connector-waits-for-a-response"></a>4. Bağlayıcı için bir yanıt bekler.

Arka uç için tüm içerik aktarımını ve istek tamamlandıktan sonra bağlayıcı için bir yanıt bekler.

Bir yanıtını aldıktan sonra bağlayıcıyı üst bilgisi ayrıntıları dönmek ve dönüş verileri akışı başlatmak için uygulama ara Sunucusu hizmetine giden bir bağlantı kurar.

#### <a name="5-the-service-streams-data-to-the-user"></a>5. Hizmet, kullanıcı veri akışları. 

Bazı uygulama işlenmesini burada ortaya çıkabilir. Uygulamanızda üstbilgileri çevirmek için uygulama proxy'si veya URL'leri yapılandırdıysanız, bu işlem sırasında bu adımı gerektiğinde'olmuyor


## <a name="next-steps"></a>Sonraki adımlar

[Azure AD uygulama ara sunucusu kullanırken ağ topolojisi hakkında önemli noktalar](application-proxy-network-topology.md)

[Azure AD uygulama ara sunucusu bağlayıcıları anlama](application-proxy-connectors.md)
