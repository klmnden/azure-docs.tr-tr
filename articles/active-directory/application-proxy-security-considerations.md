---
title: Azure AD uygulama proxy'si için güvenlik konuları | Microsoft Docs
description: Azure AD uygulama proxy'si kullanarak güvenlik konuları kapsar
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2017
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 7f12d34218f880a6f8ad0c8f4de717868e0d89d1
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="security-considerations-for-accessing-apps-remotely-with-azure-ad-application-proxy"></a>Uygulamaları Azure AD uygulama proxy'si ile uzaktan erişim için güvenlik konuları

Bu makalede, kullanıcılar ve uygulamalarınızı Azure Active Directory Uygulama proxy'si kullandığınızda güvenli tutmak için iş bileşenleri açıklanmaktadır.

Aşağıdaki diyagramda gösterildiği nasıl Azure AD, şirket içi uygulamalara güvenli uzaktan erişim sağlar.

 ![Azure AD uygulama proxy'si aracılığıyla güvenli uzaktan erişim diyagramı](./media/application-proxy-security-considerations/secure-remote-access.png)

## <a name="security-benefits"></a>Güvenlik avantajları

Azure AD uygulama proxy'si aşağıdaki güvenlik avantajları sunar:

### <a name="authenticated-access"></a>Kimliği doğrulanmış erişim 

Azure Active Directory ön kimlik doğrulaması kullanmayı seçerseniz, yalnızca kimliği doğrulanmış bağlantılar ağınıza erişebilirsiniz.

Azure AD güvenlik belirteci hizmeti (STS) tüm kimlik doğrulaması için Azure AD uygulama proxy'si kullanır.  Ön kimlik doğrulaması, yalnızca kimliği doğrulanmış kimlikleri arka uç uygulaması erişebildiğinden gereği, anonim saldırıları, çok sayıda engeller.

Ön kimlik doğrulaması yöntemi olarak geçiş seçerseniz, bu avantajı elde etmezsiniz. 

### <a name="conditional-access"></a>Koşullu erişim

Ağınıza bağlantı kuran önce daha zengin İlkesi denetimleri uygulayın.

İle [koşullu erişim](active-directory-conditional-access-azure-portal-get-started.md), hangi trafiğin uç uygulamalarınızı erişmesine izin verilip kısıtlamalar tanımlayabilirsiniz. Oturum açma işlemleri kimlik doğrulama ve kullanıcı riski profili gücünü konuma göre kısıtlayan ilkeler oluşturabilirsiniz.

Koşullu erişim, çok faktörlü kimlik doğrulama ilkeleri, bir güvenlik katmanı, kullanıcı kimlik doğrulamalarına eklenmesinden yapılandırmak için de kullanabilirsiniz. 

### <a name="traffic-termination"></a>Trafik sonlandırma

Tüm trafiği bulutta sonlandırılır.

Azure AD uygulama proxy'si ters proxy olduğundan, tüm trafiği arka uç uygulamalara hizmeti sonlandırıldı. Oturum, yalnızca, arka uç sunucuları için doğrudan HTTP trafiği sunulmaz anlamına gelir arka uç sunucusu ile yeniden. Bu yapılandırma, hedeflenen saldırılara karşı daha iyi korunur anlamına gelir.

### <a name="all-access-is-outbound"></a>Tüm erişim giden 

Şirket ağına gelen bağlantıları açmanız gerekmez.

Uygulama proxy'si bağlayıcıları yalnızca gelen bağlantıları için güvenlik duvarı bağlantı noktalarını açmak için gerekli olduğu anlamına gelir Azure AD uygulama proxy'si Hizmeti giden bağlantıları kullanın. Geleneksel proxy'leri bir çevre ağına gerekli (olarak da bilinen *DMZ*, *sivil bölge*, veya *Perdeli alt ağ*) ve ağ ucunda kimlik doğrulamasız bağlantılar erişim izni. Bu senaryo trafiğini analiz ve ortamını korumak için web uygulaması güvenlik duvarı ürünleri yatırım gereklidir. Uygulama proxy'si ile tüm bağlantıları giden olduğu ve güvenli bir kanal üzerinden gerçekleşmesi için bir çevre ağına gerekmez.

Bağlayıcılar hakkında daha fazla bilgi için bkz: [anlamak Azure AD uygulama proxy'si Bağlayıcılar](application-proxy-understand-connectors.md).

### <a name="cloud-scale-analytics-and-machine-learning"></a>Bulut ölçekli analizi ve makine öğrenme 

Modern güvenlik koruması alın.

Azure Active Directory'nin parçası olduğu için uygulama proxy'si yararlanabilirsiniz [Azure AD Identity Protection](active-directory-identityprotection.md), Microsoft Security Response Center ve dijital Suçlar Birimi'nin verilerle. Birlikte biz proaktif olarak güvenliği aşılmış hesaplarını tanımlayan ve bu yüksek riskli oturum açma işlemlerine karşı koruma sağlar. Biz, hangi oturum açma oranıdır yüksek riskli olduğunu belirlemek için çok sayıda etkenleri dikkate alın. Bu etkenler bulaşmış bayrak cihazları, anonymizing ağlar ve alışılmadık veya olası konumları içerir.

Bu raporlar ve olayları birçoğu, aynı zamanda güvenlik bilgileri ve Olay yönetimi (SIEM) sistemleri ile tümleştirme için bir API aracılığıyla zaten kullanılabilir.

### <a name="remote-access-as-a-service"></a>Bir hizmet olarak uzaktan erişim

Koruma ve şirket içi sunucular düzeltme eki uygulama hakkında endişelenmeniz gerekmez.

Hala yüklenmemiş yazılım saldırıları, çok sayıda için hesapları. Her zaman en son güvenlik düzeltme eki ve yükseltmelerini almak için azure AD uygulama proxy'si Microsoft sahibi, bir Internet ölçeğinde hizmetidir.

Azure AD uygulama proxy'si tarafından yayımlanan uygulamaların güvenliğini artırmak için biz web Gezgin robots dizin oluşturma ve uygulamalarınızı arşivleme engelleyin. Bir web Gezgini robot yayımlanan bir uygulamanın robot kullanıcının ayarlarını almak için her çalıştığında uygulama proxy'si yanıtları içeren bir robots.txt dosyası ile `User-agent: * Disallow: /`.

### <a name="ddos-prevention"></a>DDOS önleme

Uygulama proxy'si aracılığıyla yayımlanan uygulamalar dağıtılmış reddi, hizmet (DDOS) saldırıları korunur.

Uygulama proxy'si hizmet uygulamaları ve ağ ulaşmaya çalışırken trafik miktarını izler. Uygulamalarınız için uzaktan erişim isteyen cihaz sayısını ani, Microsoft, ağ erişimi kısıtlar. 

Microsoft trafik modelleri ve aboneliğinizi bir bütün olarak tek tek uygulamalar için izler. Bir uygulama normal istekleri daha yüksek alırsa, bu uygulamaya erişmek için istekleri kısa bir süre için reddedilir. Tam aboneliğiniz normal istekleri daha yüksek almaya devam ederseniz, tüm uygulamalarınızın erişim istekleri reddedilir. Şirket içi kullanıcılarınız uygulamalarını erişme tutmak için bu koruyucu ölçü uygulama sunucularınızı uzaktan erişim istekleri tarafından aşırı tutar. 

## <a name="under-the-hood"></a>Başlık altında

Azure AD uygulama proxy'si iki bölümden oluşur:

* Bulut tabanlı hizmet: Bu hizmet, Azure üzerinde çalışır ve dış istemci/kullanıcı bağlantıları burada yapılan olduğu.
* [Şirket içi Bağlayıcısı'nı](application-proxy-understand-connectors.md): bir şirket içi bileşeni bağlayıcı Azure AD uygulama proxy'si hizmeti ve tanıtıcıları bağlantılardan dahili uygulamalara yapılan istekleri dinler. 

Bağlayıcı ve uygulama proxy'si hizmeti arasında bir akış kurulan zaman:

* Bağlayıcı ilk ayarlanır.
* Bağlayıcı uygulama proxy'si hizmetten yapılandırma bilgileri çeker.
* Bir kullanıcı bir yayımlanan uygulamaya erişiyor.

>[!NOTE]
>SSL üzerinden tüm iletişimler ortaya ve bunlar her zaman kökenli uygulama proxy'si hizmeti bağlayıcıya adresindeki. Yalnızca giden hizmetidir.

Bağlayıcı, neredeyse tüm çağrıları için uygulama proxy'si hizmeti kimlik doğrulaması için bir istemci sertifikası kullanır. Bu işlem tek özel durum ilk kurulumu, istemci sertifikası burada kurulur adımdır.

### <a name="installing-the-connector"></a>Bağlayıcısını yükleme

Bağlayıcı ilk olarak ayarlandığında, akış aşağıdakiler gerçekleşir:

1. Bağlayıcı kaydı hizmet Bağlayıcısı yüklemesi bir parçası olarak yapılır. Kullanıcılar, Azure AD yönetim kimlik bilgilerini girmeniz istenir. Bu kimlik doğrulamasını edinilen belirteci sonra Azure AD uygulama proxy'si hizmeti sunulur.
2. Uygulama proxy'si Hizmeti belirteç değerlendirir. Bu kullanıcının şirket Yöneticisi Kiracı içinde olup olmadığını denetler. Kullanıcı bir yönetici değilse, işlem sonlandırıldı.
3. Bağlayıcı, bir istemci sertifikası isteği oluşturur ve, uygulama proxy'si hizmet belirteci ile birlikte geçirir. Hizmet sırayla belirteci doğrular ve istemci sertifika isteğini imzalar.
4. Bağlayıcı uygulama proxy'si hizmeti ile gelecekteki iletişimi için istemci sertifikası kullanır.
5. Bağlayıcı sistem yapılandırma verilerinin ilk bir çekme, istemci sertifikasını kullanarak hizmetinden gerçekleştirir ve bunu şimdi istekleri almaya hazır.

### <a name="updating-the-configuration-settings"></a>Yapılandırma ayarları güncelleştiriliyor

Uygulama proxy'si hizmeti yapılandırma ayarlarını güncelleştirdiğinde, akış aşağıdakiler gerçekleşir:

1. Uygulama proxy'si hizmeti içinde yapılandırma uç noktası istemci sertifikasını kullanarak bağlayıcı bağlanır.
2. İstemci sertifikası doğrulandıktan sonra uygulama proxy'si hizmeti yapılandırma verileri (örneğin, bağlayıcı parçası olması gereken bağlayıcı grup) bağlayıcıya döndürür.
3. Geçerli sertifika 180 günden daha eski ise, bağlayıcı etkili bir şekilde 180 günde bir istemci sertifikası güncelleştirmeleri yeni bir sertifika isteği oluşturur.

### <a name="accessing-published-applications"></a>Yayımlanan uygulamalara erişme

Kullanıcılar yayımlanmış bir uygulama eriştiğinde, aşağıdaki olaylar uygulama proxy'si hizmeti ve uygulama ara sunucusu Bağlayıcısı arasında gerçekleşir:

1. [Hizmet uygulaması için kullanıcının kimliğini doğrular](#the-service-checks-the-configuration-settings-for-the-app)
2. [Hizmet isteği bağlayıcı sırasına yerleştirir.](#The-service-places-a-request-in-the-connector-queue)
3. [Bir bağlayıcı sırasından isteğini işler](#the-connector-receives-the-request-from-the-queue)
4. [Bağlayıcı için bir yanıt bekler](#the-connector-waits-for-a-response)
5. [Hizmet kullanıcı veri akışları](#the-service-streams-data-to-the-user)

Bu adımların her biri bir yerde neler hakkında daha fazla bilgi için okuma tutun.


#### <a name="1-the-service-authenticates-the-user-for-the-app"></a>1. Hizmet uygulaması için kullanıcının kimliğini doğrular

Geçiş, ön kimlik doğrulaması yöntemi olarak kullanmak üzere uygulamayı yapılandırdıysanız, bu bölümdeki adımları atlanır.

Azure AD ile preauthenticate uygulamanın yapılandırdıysanız, kullanıcıların kimliğini doğrulamak için Azure AD STS yönlendirilir ve aşağıdaki adımları gerçekleşir:

1. Koşullu erişim ilkesi gereksinimlere belirli uygulama için uygulama proxy'si denetler. Bu adım, kullanıcı uygulamaya atandı sağlar. İki aşamalı doğrulama gerekliyse, kimlik doğrulama sıralamasını ikinci bir kimlik doğrulama yöntemi için kullanıcıya sorar.

2. Tüm denetimler geçtikten sonra Azure AD STS uygulama için imzalı bir belirteç verir ve kullanıcı geri uygulama proxy'si hizmeti yönlendirir.

3. Uygulama proxy'si belirteç uygulamasını düzeltmek için verilmiş olduğunu doğrular. Diğer denetimleri ayrıca, belirteç Azure AD tarafından imzalanmış ve hala geçerli pencereye olduğundan emin olduktan gibi gerçekleştirir.

4. Uygulama Proxy kümeleri, kimlik doğrulamasının uygulamaya belirtmek için bir şifreli kimlik doğrulama tanımlama bilgisi oluştu. Tanımlama bilgisi belirteci Azure AD'den ve üzerinde tabanlı kimlik doğrulaması kullanıcı adı gibi diğer veri tabanlı bir sona erme zaman damgası içerir. Tanımlama bilgisinin yalnızca uygulama proxy'si hizmeti bilinen özel bir anahtarla şifrelenir.

5. Uygulama proxy'si kullanıcı ilk olarak istenen URL'ye yeniden yönlendirir.

Herhangi bir kısmını ön kimlik doğrulama adımları başarısız olursa, kullanıcının isteği reddedilir ve kullanıcı sorunun kaynağını belirten bir ileti gösterilir.


#### <a name="2-the-service-places-a-request-in-the-connector-queue"></a>2. Hizmet isteği bağlayıcı sırasına yerleştirir.

Bağlayıcılar için uygulama proxy'si Hizmeti giden bir bağlantıyı açık tutun. Bir istek geldiğinde, isteği açık bağlantılarından biri almak bağlayıcı hizmeti sıralar.

İstek üstbilgilerini, şifrelenmiş bir tanımlama, istek ve istek kimliğini yapan kullanıcı verileri gibi uygulama öğelerinden istek içerir Şifrelenmiş tanımlama bilgisi verileri istekle birlikte gönderilen rağmen kimlik doğrulama tanımlama değil.

#### <a name="3-the-connector-processes-the-request-from-the-queue"></a>3. Bağlayıcı sırasından isteğini işler. 

İsteğe bağlı olarak, uygulama proxy'si aşağıdaki eylemlerden birini gerçekleştirir:

* İstek basit bir işlemle ise (örneğin, bir RESTful ile olduğu gibi gövdesi içinde veri yok *almak* isteği), bağlayıcı hedef iç kaynak için bir bağlantı kurar ve bir yanıt için bekler.

* İstek gövdesinde ilişkili veri varsa (örneğin, bir RESTful *POST* işlemi), bağlayıcı uygulama proxy'si örneğine istemci sertifikasını kullanarak giden bir bağlantı sağlar. İstek verilerini ve iç kaynak bir bağlantı açmak için bu bağlantıyı kolaylaştırır. Bir bağlayıcı isteği aldıktan sonra uygulama proxy'si hizmeti içerik kullanıcıdan kabul başlar ve bağlayıcı için verileri iletir. Bağlayıcı sırayla, iç kaynak verileri iletir.

#### <a name="4-the-connector-waits-for-a-response"></a>4. Bağlayıcı için bir yanıt bekler.

İstek ve arka uç için tüm içerik aktarımını tamamlandıktan sonra bağlayıcı için bir yanıt bekler.

Bir yanıt aldıktan sonra bağlayıcı Üstbilgi ayrıntılarını döndürür ve dönüş verileri akış başlamak için uygulama proxy'si hizmetine giden bir bağlantı kurar.

#### <a name="5-the-service-streams-data-to-the-user"></a>5. Hizmet kullanıcı veri akışlarını. 

Bazı uygulama işlenmesini burada ortaya çıkabilir. Uygulamanızda üstbilgileri çevirmek için uygulama proxy'si veya URL'leri yapılandırdıysanız, bu işlem bu adım sırasında gerektiği şekilde gerçekleşir.


## <a name="next-steps"></a>Sonraki adımlar

[Azure AD uygulama proxy'si kullanırken ağ topolojisi hakkında önemli noktalar](application-proxy-network-topology-considerations.md)

[Azure AD uygulama proxy'si bağlayıcılar anlama](application-proxy-understand-connectors.md)
