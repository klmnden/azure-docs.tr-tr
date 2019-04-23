---
title: Bir Azure Active Directory Uygulama Ara sunucusu dağıtımını planlama
description: Kuruluşunuzdaki uygulama proxy'si dağıtımını planlamak için bir uçtan uca kılavuz
services: active-directory
documentationcenter: azure
author: barbaraselden
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04-04-2019
ms.author: barbaraselden
ms.reviewer: ''
ms.openlocfilehash: 44393f80ab6ea01f0c2f52cb01dcd6241fab3d2d
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60000717"
---
# <a name="plan-an-azure-ad-application-proxy-deployment"></a>Bir Azure AD uygulama ara sunucusu dağıtımını planlama

Azure Active Directory (Azure AD) uygulama proxy'si, şirket içi uygulamalar için uzaktan erişim güvenli ve uygun maliyetli bir çözümdür. Bu, eski erişimi yönetmek "Bulut ilk" kuruluşlar için bir anlık geçiş yolu yerinde henüz modern protokolleri kullanma yeteneği olmayan uygulamalar sağlar. Ek tanıtıcı bilgiler için bkz [uygulama proxy'si nedir](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy) ve [nasıl uygulama Proxy çalışır](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy).

Bu makale, planlama, çalıştırmak ve Azure AD uygulama ara sunucusunu yönetmek için ihtiyacınız olan kaynakları içerir. 

## <a name="plan-your-implementation"></a>Uygulamanızı planlama

Aşağıdaki bölüm anahtarı için bir etkin dağıtım deneyimi ayarlayacak öğeleri planlama geniş kapsamlı bir görünümünü sağlar. 

### <a name="prerequisites"></a>Önkoşullar

Uygulamanız başlamadan önce aşağıdaki önkoşulları sağlamanız gerekir. Bu konuda bu Önkoşullar dahil, ortamınızı kurma hakkında daha fazla bilgi görebilirsiniz [öğretici](application-proxy-add-on-premises-application.md).

* **Bağlayıcılar**: Bağlayıcılar üzerine dağıtabileceğiniz basit aracıları şunlardır:
   * Fiziksel donanım şirket içi
   * Herhangi bir hiper yönetici çözümü içinde barındırılan bir sanal makine
   * Uygulama Ara Sunucusu hizmetine giden bağlantıyı etkinleştirmek için azure'da barındırılan bir sanal makine.

Bkz: [anlamak Azure AD uygulaması Proxy bağlayıcıları](application-proxy-connectors.md) daha ayrıntılı bir genel bakış.

   * Bağlayıcıyı barındıran gereken [etkin olması için TLS 1.2](application-proxy-add-on-premises-application.md) bağlayıcılarını yüklemeden önce.

   * Mümkünse, bağlayıcı dağıtma [aynı ağ](application-proxy-network-topology.md) ve arka uç web uygulama sunucuları olarak kesimi. Bir bulma uygulamalarının tamamladıktan sonra bağlayıcıyı konakları dağıtmak idealdir.

* **Ağ erişim ayarları**: Azure AD uygulama ara sunucusu bağlayıcıları [HTTPS (TCP bağlantı noktası 443) ve HTTP (TCP bağlantı noktası 80) aracılığıyla azure'a bağlanma girişiminde](application-proxy-add-on-premises-application.md). 

   * Sondaki Bağlayıcısı TLS trafik desteklenmiyor ve bağlayıcılar ile ilgili Azure uygulama proxy'si uç noktalarını güvenli bir kanal kurmasını engeller.

   * Tüm bağlayıcılar ve Azure arasında giden TLS iletişimde satır içi İnceleme biçimlerinin kaçının. Bir bağlayıcı ve arka uç uygulamaları arasında iç İnceleme mümkündür, ancak kullanıcı deneyimini düşebilir ve bu nedenle önerilmez.

   * Proxy bağlayıcıları kendilerini Yük Dengeleme de desteklenen ya da gerekli değildir.

### <a name="important-considerations-before-configuring-azure-ad-application-proxy"></a>Azure AD uygulama ara sunucusunu yapılandırmadan önce önemli noktalar

Yapılandırma ve Azure AD uygulama proxy'si uygulamak için aşağıdaki temel gereksinimlerinin karşılanması gerekir.

*  **Azure ekleme**: Uygulama proxy'si dağıtmadan önce kullanıcı kimliklerini bir şirket içi dizininizden eşitlenmiş veya gerekir doğrudan, Azure AD kiracıları içinde oluşturulur. Azure AD uygulama proxy'si için bunları erişim verme yayımlanan uygulamaları önce önceden kimlik doğrulamasını ve çoklu oturum açma (SSO) gerçekleştirmek için gerekli kullanıcı kimlik bilgilerini sağlamak için kimlik eşitleme sağlar.

* **Ortak sertifika**: Özel etki alanı adları kullanıyorsanız, Microsoft olmayan güvenilir bir sertifika yetkilisi tarafından verilen bir ortak sertifika tedarik gerekir. Kuruluş gereksinimlerinize bağlı olarak bir sertifika alma biraz zaman alabilir ve işlem mümkün olduğunca erken başlangıç önerilir. Azure uygulama proxy'si standardını destekleyen [joker](application-proxy-wildcard.md), veya SAN tabanlı sertifikalar.

* **Etki alanı gereksinimleri**: Kerberos Kısıtlı temsilci (KCD) kullanarak yayımladığınız uygulamalarda çoklu oturum açma bağlayıcı konak yayımlama olan uygulamalarla aynı AD etki alanına etki alanına katılmış olmasını gerektirir. Konuyla ilgili ayrıntılı bilgi için bkz. [KCD için çoklu oturum açma](application-proxy-configure-single-sign-on-with-kcd.md) uygulama ara sunucusu ile. Bağlayıcı hizmeti, yerel sistem bağlamında çalışır ve özel bir kimlik kullanacak şekilde yapılandırılmamalıdır.

* **URL'ler için DNS kayıtları**

   * Uygulama proxy'sinde özel etki alanları kullanmadan önce önceden tanımlı uygulama ara sunucusu adresi için özel tanımlanmış dış URL çözümlemek istemcilerin genel DNS'de bir CNAME kaydı oluşturmalısınız. Özel bir etki alanı kullanan bir uygulama için bir CNAME kaydı oluşturmak başarısız olan uzak kullanıcıların uygulamaya bağlanmasını engeller. Bu nedenle CNAME kayıtları sağlayıcısı için sağlayıcı DNS'den değişebilir eklemek için gereken adımları öğrenin nasıl [Azure portalını kullanarak DNS kayıtlarını ve kayıt kümelerini yönetmeyi](https://docs.microsoft.com/azure/dns/dns-operations-recordsets-portal).

   * Benzer şekilde, bağlayıcı konakları yayımlanan uygulamaları, dahili URL'yi çözümleyebilmesi olması gerekir.

* **Yönetici haklarına ve rolleri**

   * **Bağlayıcı yükleme** üzerinde yüklendiği Windows server için yerel yönetici hakları gerektirir. Ayrıca, kimlik doğrulaması ve bağlayıcı örneği Azure AD kiracınıza kaydetmek için bir uygulama yöneticisi rolü en az gerektirir. 

   * **Uygulama yayımlama ve Yönetim** gerektiren *Uygulama Yöneticisi* rol. Uygulama yöneticileri dizinde kayıtları, SSO ayarlarını, kullanıcı ve Grup atamalarını ve lisanslama, uygulama proxy'si ayarları ve onay dahil olmak üzere tüm uygulamaları yönetebilirsiniz. Koşullu erişimi yönetme hakkı vermez. *Bulut uygulaması Yöneticisi* uygulama proxy'si ayarları yönetimi izin vermediğinden dışında tüm becerileri, uygulama yöneticisi rolüne sahip.

* **Lisanslama**: Uygulama proxy'si aracılığıyla Azure AD temel aboneliği kullanılabilir. Başvurmak [Azure Active Directory fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/active-directory/) lisans seçenekleri ve özelliklerin tam listesi için. 

* Uygulama Yöneticisi hakları almak için bir rol ayrıcalık gerekebilir [Privileged Identity Manager](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) (PIM), böylece hesabınıza uygun olduğundan emin olun. 

### <a name="application-discovery"></a>Uygulama bulma

Aşağıdaki bilgileri toplayarak uygulama proxy'si yayımlanan tüm kapsamındaki uygulamaları envanterini derleyin:

| Bilgi türü| Bilgi toplamak için |
|---|---|
| Hizmet Türü| Örneğin: SharePoint, SAP, CRM, özel bir Web uygulaması, API |
| Uygulama platformu | Örneğin: Windows IIS, Apache on Linux, Tomcat, NGINX |
| Etki alanı üyeliği| Web sunucusunun tam etki alanı adı (FQDN) |
| Uygulama konumu | Web sunucusu veya grup altyapınızda bulunduğu |
| İç erişim | Uygulama dahili olarak erişirken kullanılan tam URL. <br> Bir grup, hangi tür Yük Dengeleme kullanılıyor? <br> Olup uygulamanın kendisinden başka kaynaklardan içerik çizer.<br> Uygulama WebSockets üzerinden çalışıp çalışmadığını belirleyin. |
| Dış erişim | Uygulama zaten dışarıdan Internet'e satıcı çözümü. <br> Dış erişim için kullanmak istediğiniz URL. SharePoint, alternatif erişim eşlemeleri başına yapılandırıldığından emin olun [bu kılavuz](https://docs.microsoft.com/SharePoint/administration/configure-alternate-access-mappings). Aksi takdirde, dış URL'leri tanımlamanız gerekir. |
| Ortak sertifika | Özel bir etki alanı kullanıyorsanız, karşılık gelen bir konu adına sahip bir sertifika edinin. bir sertifika varsa seri numarası ve nereden alınacağı konumu dikkat edin. |
| Kimlik doğrulaması türü| Temel, Windows tümleşik kimlik doğrulaması, form tabanlı, üst bilgi tabanlı ve talep gibi uygulama desteği tarafından desteklenen kimlik doğrulama türü. <br>Uygulama belirli bir etki alanı hesabı altında çalışacak şekilde yapılandırıldıysa, tam etki alanı adı (FQDN) hizmet hesabı unutmayın.<br> SAML tabanlı, tanımlayıcı ve yanıt URL'lerini. <br> Üst bilgi tabanlı kimlik doğrulamasını işlemek için belirli bir gereksinim ve satıcı çözümü yazın. |
| Bağlayıcı grubu adı | Bu arka uç uygulaması için conduit ve SSO sağlamak üzere atanmış bir bağlayıcı grubu mantıksal adı. |
| Kullanıcılar/Gruplar erişim | Kullanıcılar veya kullanıcı grupları, uygulamaya dış erişim izni verilir. |
| Ek gereksinimler | Herhangi bir ek uzaktan erişim veya uygulama yayımlama içine faktörlenebilen güvenlik gereksinimlerini unutmayın. |

Bu indirebileceğiniz [uygulama envanteri elektronik](https://aka.ms/appdiscovery) uygulamalarınızı envantere almak üzere.

### <a name="define-organizational-requirements"></a>Kuruluş gereksinimlerini tanımlayın

Kuruluşunuzun iş gereksinimlerini tanımlama alanları şunlardır: Her alanı gereksinim örnekleri içerir.

 **Erişim**

* Herhangi bir etki alanına katılmış veya Azure AD kullanarak cihazları katıldığında etki alanı ve Azure AD kullanıcılarının sorunsuz çoklu oturum açma (SSO) güvenli bir şekilde ile yayımlanan uygulamalara erişebilirsiniz.

* MFA kaydedilen ve bir kimlik doğrulama yöntemi olarak Microsoft Authenticator uygulamasını cep telefonundan kayıtlı koşuluyla onaylı kişisel cihazlara sahip kullanıcılar yayımlanmış uygulamalara güvenli şekilde erişebilir.

**İdare** 

* Yöneticiler, tanımlamak ve yaşam döngüsünü uygulama proxy'si aracılığıyla yayımlanan uygulamalar için kullanıcı atamalarını izleme.

**Güvenlik**

* Yalnızca kullanıcılar grup üyeliği aracılığıyla uygulamalara atanan veya bu uygulamaları tek tek erişebilirsiniz.

**Performans**

* İç ağdan uygulama erişimi karşılaştırıldığında uygulama performans düşüşü olmadan yoktur.

**Kullanıcı deneyimi**

* Kullanıcıların, herhangi bir aygıt platformuna tanıdık şirket URL'leri kullanarak uygulamalarına erişmek nasıl haberdar.

**Denetim**
* Yöneticiler kullanıcı erişim etkinliğini denetleme olanağına sahip olursunuz.


### <a name="best-practices-for-a-pilot"></a>Bir pilot için en iyi uygulamalar

Zaman ve çaba tam olarak tek bir uygulama için çoklu oturum açma (SSO) ile uzaktan erişim sokmak için gereken miktarını belirler. Bunu, ilk bulma, yayımlama ve genel test olarak değerlendirir bir pilot çalıştırarak yapın. Tümleşik Windows kimlik doğrulaması (IWA) için önceden yapılandırılmış olan IIS tabanlı basit bir web uygulaması kullanarak, bu kurulum başarıyla pilot uzaktan erişim ve SSO için en az çaba gerektirdiği bir temel oluşturmak yardımcı olacaktır.

Aşağıdaki tasarım öğeleri pilot uygulamanızı bir üretim kiracıdaki doğrudan başarısını yükseltmeniz gerekir.  

**Bağlayıcı Yönetim**:  

* Bağlayıcılar uygulamalarınız şirket içi kanalı sağlayan bir temel rol oynar. Kullanarak **varsayılan** bağlayıcı grubu ilk pilot yayımlanan uygulamaları üretime commissioning önce test etmek için yeterli. Başarılı bir şekilde test edilen uygulamalar, ardından üretim bağlayıcı grupları için taşınabilir.

**Uygulama Yönetimi**:

* Çalışanlarınızın dış URL, bildiğiniz ve rolünüzle unutmayın olasılıktır. Önceden tanımlanmış bizim msappproxy.net veya onmicrosoft.com sonekleri kullanarak uygulamanızı yayımlama kaçının. Bunun yerine, bir mantıksal ana bilgisayar adı gibi önek tanıdık en üst düzey doğrulanmış bir etki alanı, sağlamak *intranet. < customers_domain > .com*.

* Pilot uygulama simgesi görünürlüğünü başlatma simgesi hâli Azure MyApps portalında gizleyerek pilot bir gruba kısıtlayın. Üretim için hazır olduğunda uygulamayı, ilgili hedef kitleye, aynı ön üretim kiracıda ya da uygulamayı üretim kiracınızın yayımlama için kapsamını belirleyebilirsiniz.

**Çoklu oturum açma ayarları**: Bazı SSO ayarları ayarlayın, böylece bağımlılıkları sağlayarak gecikmeler önceden gönderilen değişiklik denetimi önlemek için zaman alabilir, belirli bağımlılıkları vardır. Bu, etki alanına katılma Kerberos Kısıtlı temsilci (KCD) kullanarak ve zaman alan diğer etkinliklerini dikkate alma SSO gerçekleştirmek için bağlayıcı konakları içerir. Örneğin, bir PING Access örneği oluşturan üst bilgi tabanlı SSO gerek olmadığını ayarlanıyor.

**Bağlayıcı konak ve hedef uygulama arasında SSL**: Güvenlik kapsamını, olduğundan TLS bağlayıcı konak ve hedef uygulama arasındaki her zaman kullanılmalıdır. Kullanıcı kimlik bilgilerini ardından etkili bir şekilde düz metin olarak iletilirken saldırganların özellikle web uygulaması form tabanlı kimlik doğrulaması için (FBA) yapılandırılmışsa.

**Artımlı olarak uygulayın ve her bir adımın test**. Aşağıdaki yönergeler uygulanarak tüm kullanıcı ve iş gereksinimlerinin karşılandığından emin olmak için bir uygulama yayımlandıktan sonra temel işlevsel testleri yürütün:

1. Test ve web uygulaması'na genel erişimi devre dışı ön kimlik doğrulama ile doğrulayın.
2. Başarılı olursa ön kimlik doğrulama ve ata kullanıcılar ve Gruplar'ı etkinleştirin. Test ve erişimi doğrulayabilirsiniz.
3. Ardından uygulamanız için SSO'yu yöntemi ekleyin ve erişimi doğrulamak için tekrar test edin.
4. Koşullu erişim ve MFA ilkelerini gerektiği gibi uygulanır. Test ve erişimi doğrulayabilirsiniz.

**Sorun giderme araçları**: Sorunlarını giderirken, her zaman bağlayıcı konaktaki tarayıcısından yayımlanmış uygulamaya erişim doğrulayarak başlayın ve uygulamanın beklendiği gibi çalıştığını doğrulayın. Daha basit kolay bir şekilde kök nedeni belirlemek için Kurulum, bu nedenle göz önünde bulundurun gibi yalnızca tek bir bağlayıcıyı ve hiçbir SSO kullanarak en az bir yapılandırma ile ilgili sorunlar oluşturmaya çalışırken. Bazı durumlarda, hata ayıklama araçları Telerik'ın Fiddler gibi bir web uygulama proxy'si yoluyla erişilen erişim veya içerik sorunları vazgeçilmez kanıtlayabilirsiniz. Fiddler'ı izleme yardımcı olmak ve trafik iOS ve Android gibi mobil platformlar için hata ayıklama için proxy olarak da yapabilir ve neredeyse herhangi bir şey, yapılandırılabilir bir ara sunucu üzerinden yönlendirmek için. Bkz: [sorun giderme kılavuzu](application-proxy-troubleshoot.md) daha fazla bilgi için.

## <a name="implement-your-solution"></a>Çözümünüzü uygulama

### <a name="deploy-application-proxy"></a>Uygulama proxy'si dağıtma

Bu konuda, uygulama ara sunucusunu dağıtmak için adımları ele alınmaktadır [uzaktan erişim için şirket içi uygulama eklemek için öğretici](application-proxy-add-on-premises-application.md). Yükleme başarılı değilse seçin **uygulama proxy'si sorunlarını giderme** portalı veya sorun giderme kılavuzu[uygulama proxy'si aracı Bağlayıcısı yüklemeyle ilgili sorunları için](application-proxy-connector-installation-problem.md).

### <a name="publish-applications-via-application-proxy"></a>Uygulama proxy'si aracılığıyla uygulama yayımlama

Uygulamaları yayımlama, tüm önkoşullara uyduğunuzdan ve uygulama proxy'si sayfanın birkaç kayıtlı gösteren bağlayıcılar ve etkin olduğunu varsayar.

Kullanarak da uygulama yayımlayabilirsiniz [PowerShell](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview).

Bir uygulama yayımlama sırasında izlemek için en iyi yöntemlerden bazıları aşağıda verilmiştir:

* **Bağlayıcı grupları kullanmak**: Her bir ilgili uygulama yayımlamak için belirlenmiş olan bir bağlayıcı grubu atayın.

* **Arka uç uygulama zaman aşımını ayarlayın**: Bu ayar, uygulamanın 75 istemci işlemi işlemeye saniyeden fazla nerede gerektirebilir senaryolarda yararlıdır. Örneğin ne zaman bir istemci bir sorgu için bir web uygulaması gönderen bir veritabanı için bir ön uç işlevi görür. Ön uç, arka uç veritabanı sunucusuna bu sorgu gönderir ve bir yanıt bekler, ancak zaman bir yanıt alır, konuşma istemci tarafında zaman aşımına uğrar. 180 saniye uzun işlemlerin tamamlanması uzun sağlar için zaman aşımını ayarlama.

* **Uygun bir tanımlama bilgisi türleri kullanın**

   * **Yalnızca HTTP tanımlama bilgisi**: Uygulama proxy'si HTTPOnly bayrağı set-cookie HTTP yanıt üstbilgileri dahil sağlayarak ek güvenlik sağlar. Bu ayar, siteler arası betik (XSS) gibi alan yararlanma saldırılarını hafifletmeye yardımcı olur. Bu oturum tanımlama bilgisinin erişmesi istemci/kullanıcı aracıları için Hayır bırakın. Örneğin, uygulama ara sunucusu üzerinden bağlanan bir Uzak Masaüstü Ağ geçidi için RDP/MTSC istemci yayımladı.

   * **Tanımlama bilgisi güvenli**: Bir tanımlama bilgisi ile güvenli öznitelik ayarlandığında, istek, bir TLS güvenli kanal üzerinden iletilen kullanıcı aracısı (istemci-tarafı uygulaması) yalnızca tanımlama bilgisi HTTP isteklerini içerir. Bu, bir tanımlama bilgisi etkinleştirilmelidir bu nedenle şifresiz kanal güvenliğinin bozulması riskini azaltmaya yardımcı olur.

   * **Kalıcı bir tanımlama bilgisi**: Bunu ya da süresi dolana veya silinene kadar geçerli kalan tarafından tarayıcı kapanışlar arasında kalıcı hale getirmek uygulama ara sunucusu oturum tanımlama bilgisini verir. Office gibi zengin bir uygulama içinde bir yayımlanan web uygulamasına bir belge olmadan kimlik doğrulaması için yeniden uyarılmasını kullanıcı eriştiği senaryoları için kullanılır. Etkinleştirme dikkatli ancak kalıcı tanımlama bilgileri sonuçta bir hizmet, yetkisiz erişim risk karşılayan diğer denetimlerle birlikte kullanılmaması durumunda bırakabilirsiniz. Bu ayar, yalnızca tanımlama bilgilerini işlemler arasında paylaşamaz eski uygulamalar için kullanılmalıdır. Bu ayar kullanmak yerine işlemleri arasında paylaşım tanımlama bilgilerini işlemek için uygulamanızı güncelleştirmeniz daha iyidir.

* **Üst bilgilerinde URL'leri**: Bu senaryolar için kuruluşunuzun genel ad alanı (a.k.a bölünmüş DNS) eşleştirilecek iç DNS burada yapılandırılamaz etkinleştirin. Uygulamanızın gerektirdiği istemci istekte özgün ana bilgisayar üstbilgisi sürece Evet olarak ayarlanırsa bu değeri bırakın. Alternatif gerçek trafiği ve dış URL ana bilgisayar üst bilgisi olarak FQDN yönlendirme için iç URL FQDN'yi kullanır bağlayıcı sağlamaktır. Çoğu durumda bu alternatif işlev uygulamaya normal olarak, uzaktan erişildiğinde sağlar, ancak kullanıcılarınız içinde ve dışında URL eşleşen sahip avantajlarını kaybedersiniz.

* **Uygulama gövdesindeki URL'leri**: Yanıtları istemciye geri çevrilmesi için bu uygulama bağlantılar istediğinizde bir uygulama için uygulama gövdesi bağlantı çeviri açın. Etkinleştirilirse, bu işlev, uygulama proxy'si istemcilere döndürülen HTML ve CSS yanıtları bulur, tüm iç bağlantıları çevirme, en iyi bir performans girişiminde sağlar. Sabit kodlanmış mutlak veya içerik NetBIOS shortname bağlantılar içeren uygulamaları veya diğer bağlantılar içerikle yayımlama şirket içi uygulamalarda olduğunda yararlıdır.

Böylece uygulama başına düzeyinde kullanıcı deneyimi üzerinde denetime sahip uygulamaları Burada yayımlanan bir yayımlanan uygulama bağlantılar diğer senaryolar için bağlantı çeviri veya her uygulama etkinleştirin.

Örneğin, uygulama tüm birbirine bağlamasını Proxy üzerinden yayımlanan üç uygulama olduğunu varsayalım: Avantajları, giderleri ve seyahat yanı sıra dördüncü bir uygulama, uygulama proxy'si aracılığıyla yayımlandığından değilse geri bildirim.

![Resim 1](media/App-proxy-deployment-plan/link-translation.png)

Bağlantı çeviri avantajları uygulama için etkinleştirdiğinizde, uygulamalardan kurumsal ağ dışından erişen kullanıcılar bunlara erişebilmesi için giderleri ve seyahat bağlantılar bu uygulamalarda, dış URL'leri yönlendirilir. Bu iki uygulama için bağlantı çeviri üzere etkinleştirmemiş çünkü avantajları için geri gider ve seyahat bağlantılardan çalışmaz. Olmadığından hiçbir dış URL avantajları uygulama kullanan kullanıcıların Kurumsal Ağ dışından geri bildirim uygulamasından erişmek mümkün olmayacaktır. Bu nedenle, geri bildirim bağlantısını yeniden yönlendirilen değil. Ayrıntılı bilgilere bakın [bağlamak çeviriyi ve diğer yeniden yönlendirme seçenekleri](application-proxy-configure-hard-coded-link-translation.md).

### <a name="access-your-application"></a>Uygulama erişimi

Erişimi yönetmek için çeşitli seçenekler mevcut uygulama proxy'si için yayımlanmış kaynakları, bu nedenle seçin belirli senaryo ve ölçeklenebilirlik gereksinimlerinize en uygun. Genel yaklaşımları içerecek: Azure AD Connect ile eşitlenen şirket içi gruplarını kullanarak Azure AD'deki dinamik gruplar oluşturmayı temel alarak kullanıcı öznitelikleri, bir kaynak sahibi veya tüm bunların bir birleşimi tarafından yönetilen bir Self Servis gruplarını kullanma. Avantajları her için bağlı kaynaklar bakın.

Kullanıcılar uygulamaya erişim atama en basit yolu, ne gidip **kullanıcılar ve gruplar** sol bölmedeki seçeneklerden kullanıcılarınızın yayımlanan uygulamanıza ve grupların veya kişilerin doğrudan atama.

![Resim 24](media/App-proxy-deployment-plan/add-user.png)

Ayrıca Self Servis erişimi kullanıcılara uygulamanıza bunlar şu anda üyesi olmayan bir grup atama ve Self Servis seçeneklerini yapılandırma izin verebilirsiniz.

![25 resmi](media/App-proxy-deployment-plan/allow-access.png)

Etkinleştirilirse, kullanıcılar ardından MyApps portalında ve istek Access'e oturum olabilecek ve onaylanmış ve zaten izin verilen Self Servis Grup ya da onay gerektiren için belirlenmiş onaylayanın eklediği otomatik olarak ya da.

Konuk kullanıcılar da olabilir [uygulama proxy'si aracılığıyla Azure AD B2B aracılığıyla yayımlanan dahili uygulamalara erişmek için davet](https://docs.microsoft.com/azure/active-directory/b2b/add-users-information-worker).

Anonim olarak normalde erişilebilen şirket içi uygulamaları, hiçbir kimlik doğrulaması gerektiren, uygulamanın bulunan seçeneği devre dışı bırakmak tercih edebilirsiniz için **özellikleri**.

![26 resmi](media/App-proxy-deployment-plan/assignment-required.png)


Bu seçenek Hayır olarak ayarlanırsa bırakarak kullanıcıların izinleri olmayan Azure AD uygulama ara sunucusu aracılığıyla şirket içi uygulamaya erişmek için bu nedenle dikkatli olanak tanır.

Uygulamanızı yayımladıktan sonra dış URL'sini bir tarayıcıda yazarak veya kendi simgesinin erişilebilmelidir [ https://myapps.microsoft.com ](https://myapps.microsoft.com/).

### <a name="enable-pre-authentication"></a>Ön kimlik doğrulamasını etkinleştirme

Uygulamanızı uygulama proxy'si aracılığıyla erişilebilir olduğunu doğrulayın. 

1. Gidin **Azure Active Directory** > **kurumsal uygulamalar** > **tüm uygulamaları** ve yönetmek istediğiniz uygulamayı seçin.

2. Seçin **uygulama proxy'si**.

3. İçinde **ön kimlik doğrulama** alan, seçmek için açılan listeyi kullanın **Azure Active Directory**seçip **Kaydet**.

Etkin ön kimlik doğrulaması ile Azure AD için kimlik doğrulaması sınaması ve kimlik doğrulaması gerektiriyorsa, ardından arka uç uygulaması Ayrıca, meydan. Başlangıçta HTTP için yapılandırılmış herhangi bir uygulama artık HTTPS ile güvenli şekilde ön kimlik doğrulaması, Azure AD'ye geçiş değiştirme Ayrıca dış URL HTTPS ile yapılandırır.

### <a name="enable-single-sign-on"></a>Çoklu oturum açmayı etkinleştirin

Kullanıcıların yalnızca Azure AD erişirken kez oturum açmasına gerek olmadığından SSO en iyi kullanıcı deneyimini ve güvenlik sağlar. Bir kullanıcı önceden doğrulandıktan sonra uygulama Proxy Bağlayıcısı şirket içi uygulamaya kullanıcı adına kimlik doğrulaması yaparak SSO gerçekleştirilir. Arka uç uygulaması, kullanıcının kendisi gibi oturum açma işler. 

Seçme **geçiş** seçenek hiç olmadığı kadar Azure AD'ye kimlik doğrulaması yapmak zorunda kalmadan yayımlanan uygulamanıza erişmeleri kullanıcılara izin verir.

SSO gerçekleştirmek yalnızca Azure AD kullanıcıların erişim sırasında SSO için çalışması için önceden kimlik doğrulaması için uygulamanızı yapılandırılması için bir kaynağa erişim isteğinde bulunan kullanıcı mevcutsa, aksi takdirde SSO seçenekleri devre dışı bırakılacak mümkün olur.

Okuma [Azure AD uygulamaları için çoklu oturum açma](what-is-single-sign-on.md) uygulamalarınızı yapılandırırken en uygun SSO yöntemi seçmenize yardımcı olmak için.

###  <a name="working-with-other-types-of-applications"></a>Diğer uygulama türleri ile çalışma

Azure AD uygulama proxy'si de bizim Azure AD kimlik doğrulama kitaplığı kullanmak için geliştirilen uygulamaları destekler ([ADAL](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries)) veya Microsoft kimlik doğrulama kitaplığı ([MSAL](https://azure.microsoft.com/blog/start-writing-applications-today-with-the-new-microsoft-authentication-sdks/)). Bu, yerel istemci uygulamaları tarafından verilen belirteçler kullanıcılar adına ön kimlik doğrulama gerçekleştirmek için istemci istek üst bilgilerini alınan Azure AD kullanan destekler.

Okuma [yerel ve mobil istemci uygulamalarını yayımlama](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-native-client) ve [talep tabanlı uygulamaları](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-claims-aware-apps) kullanılabilir uygulama Proxy yapılandırmaları hakkında bilgi edinmek için.

### <a name="use-conditional-access-to-strengthen-security"></a>Güvenliği güçlendirmek için koşullu erişim

Uygulama güvenliğini koruyun ve yanıt veren güvenlik özellikleri karmaşık tehditleri şirket içi ve buluttaki Gelişmiş bir kümesi gerektirir. Saldırganlar genellikle zayıf, varsayılan veya çalınan kullanıcı kimlik bilgileri sayesinde Kurumsal ağa erişim elde edin.  Microsoft kimlik temelli güvenlik yönetimi ve ayrıcalıklı ve ayrıcalıksız kimlik koruma çalınan kimlik bilgilerinin kullanımını azaltır.

Aşağıdaki özellikleri, Azure AD uygulama ara sunucusunu desteklemek için kullanılabilir:

* Kullanıcı ve konum tabanlı koşullu erişim: Coğrafi konum veya IP adresiyle göre kullanıcı erişimini kısıtlayarak korunan hassas verileri tutmak [konum tabanlı koşullu erişim ilkeleri](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-locations).

* Cihaz tabanlı koşullu erişim: Şirket verilerini yalnızca kayıtlı, onaylanan ve uyumlu cihazların erişebildiğinden emin olmak [cihaz tabanlı koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-policy-connected-applications).

* Uygulama tabanlı koşullu erişim: İş, bir kullanıcı kurumsal ağ üzerinde bulunmadığında durdurmak zorunda değildir. [Kurumsal Bulut ve şirket içi uygulamalara güvenli](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam) ve koşullu erişim ile denetimi korumak.

* Risk tabanlı koşullu erişim: Kötü amaçlı bir bilgisayar korsanları ile verilerinizi koruma bir [risk tabanlı koşullu erişim ilkesi](https://www.microsoft.com/cloud-platform/conditional-access) uygulanabilecek tüm uygulamalar ve tüm kullanıcılara olup şirket içi veya bulutta.

* Azure AD uygulama panelinde: Dağıtılan uygulama proxy'si hizmeti ile güvenli bir şekilde yayımlanan uygulamalar, kullanıcılarınızın bulmak ve tüm uygulamalara erişmek için basit bir hub sunar. Yeni uygulamalar ve gruplara erişim isteyebilir veya aracılığıyla, diğerleri adına bu kaynaklara erişimi yönetme olanağı gibi Self Servis özellikler ile verimliliği artırma [erişim paneli](https://aka.ms/AccessPanelDPDownload).

## <a name="manage-your-implementation"></a>Uygulamanızı yönetme

### <a name="required-roles"></a>Gerekli rolleri

Microsoft Azure AD ile görevleri gerçekleştirmek için mümkün olan en küçük Ayrıcalık verme ilkesi sorunlarınızda. [Mevcut olan farklı Azure rolleri gözden](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) ve her kişi, ihtiyaçlarınızı karşılamak için doğru olanı seçin. Bazı roller geçici olarak uygulanmış ve dağıtım tamamlandıktan sonra kaldırılmış gerekebilir.

| İş rolü| İş görevleri| Azure AD rolleri |
|---|---|---|
| Yardım Masası Yöneticisi | Uygun için genellikle sınırlı son kullanıcı bildirilen sorunları ve kullanıcıların parolalarını değiştirme, yenileme belirteçleri geçersiz kılmalarını ve hizmetin sistem durumunu izleme gibi sınırlı görevleri gerçekleştirme. | Yardım Masası Yöneticisi |
| Kimlik Yönetimi| Uygulama proxy'si hata ayıklamak için okuma Azure AD oturum açma raporlarını ve denetim günlüklerini ilgili sorunlar.| Güvenlik okuyucusu |
| Uygulama sahibi| Oluşturun ve kurumsal uygulamaları, uygulama kayıtları ve uygulama proxy'si ayarları tüm özelliklerini yönetebilir.| Uygulama Yöneticisi |
| Altyapı yöneticisi | Sertifika aktarma sahibi | Uygulama Yöneticisi |

Bilgi veya kaynakları güvenli hale getirmek için erişimi olan kişi sayısını en aza indirir, yetkisiz erişim veya yetkili bir kullanıcı yanlışlıkla hassas kaynak etkileyen alma kötü amaçlı bir aktör olasılığını azaltmada yardımcı olur. 
 
Ancak, kullanıcıların günlük ayrıcalıklı işlemleri, bu nedenle yalnızca temel zamanında (JIT) zorlamayı yine [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) ilkeleri üzerine sağlamak için ayrıcalıklı Azure kaynaklarına erişim ve Azure AD, bizim verimli yönetim erişimi yönetme ve denetleme yaklaşım önerilir.

### <a name="reporting-and-monitoring"></a>Raporlama ve izleme

Azure AD sağlama kullanım ve denetim günlüklerini ve raporları ile operasyonel durum, kuruluşunuzun kullanıcı ek Öngörüler sağlar. 

#### <a name="application-audit-logs"></a>Uygulama denetim günlükleri

Bu günlükler cihaz ve uygulama erişen kullanıcı hakkında bilgi yanı sıra, uygulama proxy'si ile yapılandırılan uygulamalar için oturum açma bilgileri ayrıntılı olarak açıklanmaktadır. Bunlar, Azure portalında ve denetim API'si yer alır.

#### <a name="windows-event-logs-and-performance-counters"></a>Windows olay günlükleri ve performans sayaçları

Bağlayıcılar, hem yönetim hem de oturum sahip günlükleri. Yönetici günlükler anahtar olayları ve bunların hatalarını içerir. Oturum günlükleri tüm işlemleri ve bunların işleme ayrıntılarını içerir. Günlükleri ve sayaçları Windows olay günlüklerindeki bulunur ve bu izleyin [Azure İzleyici'de olay günlüğü veri kaynaklarını yapılandırmak için öğretici](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-windows-events).

### <a name="troubleshooting-guide-and-steps"></a>Sorun giderme kılavuzu ve adımları

Sık karşılaşılan sorunları ve bunların kılavuzunu ile nasıl çözüleceğine hakkında daha fazla bilgi [sorun giderme](application-proxy-troubleshoot.md) hata iletileri. 

Bu makaleler, yaygın senaryoları kapsar, ancak destek kuruluşunuz için kendi sorun giderme kılavuzları de oluşturabilirsiniz. 

* [Uygulama sayfası görüntülenirken sorun oluşuyor](application-proxy-page-appearance-broken-problem.md)
* [Uygulamanın yüklenmesi çok uzun sürüyor](application-proxy-page-load-speed-problem.md)
* [Uygulama sayfasındaki bağlantılar çalışmıyor](application-proxy-page-links-broken-problem.md)
* [Uygulamam için açılacak bağlantı noktaları](application-proxy-connectivity-ports-how-to.md)
* [Uygulamam için bağlayıcı grubunda çalışan bağlayıcı yok](application-proxy-connectivity-no-working-connector.md)
* [Azure portalda yapılandırma](application-proxy-config-how-to.md)
* [Uygulamamda çoklu oturum açmayı yapılandırma](application-proxy-config-sso-how-to.md)
* [Yönetici portalında uygulama oluştururken sorun oluşuyor](application-proxy-config-problem.md)
* [Kerberos Kısıtlanmış Temsilini Yapılandırma](application-proxy-back-end-kerberos-constrained-delegation-how-to.md)
* [PingAccess ile yapılandırma](application-proxy-back-end-ping-access-how-to.md)
* [Bu Kurumsal uygulama hatası erişemiyor](application-proxy-sign-in-bad-gateway-timeout-error.md)
* [Uygulama Ara Sunucusu Aracı Bağlayıcısı’nı yüklerken sorun oluşuyor](application-proxy-connector-installation-problem.md)
* [Oturum açma sorunu](application-sign-in-problem-on-premises-application-proxy.md)
