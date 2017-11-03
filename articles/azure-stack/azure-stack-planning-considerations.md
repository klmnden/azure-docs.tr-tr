---
title: "Planlama konuları Azure yığınının tümleşik sistemleri | Microsoft Docs"
description: "Şimdi planlamak ve çok düğümlü Azure yığını için hazırlanmak için yapabileceğinizi öğrenin."
services: azure-stack
documentationcenter: 
author: twooley
manager: byronr
editor: 
ms.assetid: 90f8fa1a-cace-4bfa-852b-5abe2b307615
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ms.author: twooley
ms.openlocfilehash: 8484f7947f23a00c05b34babf13cd75f9d227740
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2017
---
# <a name="planning-considerations-for-azure-stack-integrated-systems"></a>Planlama konuları Azure yığınının sistemleri tümleşik

*Uygulandığı öğe: Azure yığın tümleşik sistemleri*

Bir Azure tümleşik yığını sistemde düşünüyorsanız, bazı önemli planlama konuları dağıtım ve sistem merkeziniz nasıl uyduğunu etrafında anlamak istersiniz. Bu konuda Bu noktalar üst düzey bir genel bakış sağlar.

Tümleşik bir sistem satın almak karar verirseniz, daha ayrıntılı planlama işleminin büyük bölümü size kılavuzluk özgün donanım üreticisi (OEM) donanım satıcınıza yardımcı olur. Ayrıca gerçek dağıtım işlemini gerçekleştirecek.

## <a name="management-considerations"></a>Yönetim değerlendirmeleri

Azure yığın burada altyapısı hem de bir izin kilitlenmiştir korumalı bir sistem olduğundan ve ağ açısından. Ağ erişim denetimi listeleri (ACL'ler) yetkisiz tüm gelen trafiği ve altyapı bileşenler arasındaki tüm gereksiz iletişim engellemek için uygulanır. Bu sisteme erişmek yetkisiz kullanıcıların zorlaştırır.

Günlük yönetimi ve işlemleri için altyapısına Kısıtlanmamış yönetici erişimi yoktur. Azure yığın operatörleri sistem Yönetici portalı üzerinden veya Azure Resource Manager (aracılığıyla, PowerShell veya REST API) aracılığıyla yönetmeniz gerekir. Hyper-V Yöneticisi'ni veya yük devretme kümesi Yöneticisi gibi diğer yönetim araçları tarafından sisteme erişimi yoktur. Sistem korunmasına yardımcı olmak için üçüncü taraf yazılım (örneğin, aracıları) Azure yığın altyapısının bileşenleri içinde yüklenemez. Dış yönetim ve güvenlik yazılım ile birlikte çalışabilirlik PowerShell veya REST API oluşur.

Uyarı aracı adımlara çözülmüş olmayan sorunlarını gidermek için erişimi daha yüksek düzeyde gerekli olduğunda, destek ile çalışması gerekir. Destek daha gelişmiş işlemleri gerçekleştirmek için sistem geçici tam yönetici erişimi sağlamak için bir yöntem yoktur. 

## <a name="deploy-connected-or-disconnected"></a>Bağlı veya bağlantısı kesik dağıtma
 
Azure internet (ve Azure) bağlı veya bağlantısı kesilmiş yığını dağıtmayı seçebilirsiniz. Çoğu Azure Azure yığını ve Azure arasında karma senaryolar da dahil olmak üzere yığından elde etmek için dağıtmak için Azure bağlı istediğiniz. Aşağıdaki tabloda dağıtım modları arasındaki temel farklar özetlemek yardımcı olur.

| Alan | Bağlı mod | Bağlantı kesik modda |
| -------- | ------------- | ----------|
| Kimlik sağlayıcısı | Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) | Yalnızca AD FS |
| Market dağıtım | Market Azure yığınında için doğrudan Azure Marketi'nden öğeleri yükleme | Market Azure yığınında el ile Yönetimi gerektirir |
| Lisans modeli | Ödeme olarak-size-kullanım ve kapasite tabanlı | Kapasite tabanlı yalnızca|
| Düzeltme eki ve güncelleştirme  | Güncelleştirme paketleri doğrudan Azure yığın indirin | Çıkarılabilir medyayı ve ayrı bağlı bir aygıt gerektirir |
| | | |

Daha sonra tam bir sistem yeniden dağıtım olmadan dağıtım modunu değiştiremezsiniz.

## <a name="identity-considerations"></a>Kimlik konuları

### <a name="choose-identity-provider"></a>Kimlik sağlayıcısı seçin

Ya da Azure AD veya AD FS Azure yığın dağıtım için kullanmak istediğiniz hangi kimlik sağlayıcısı göz önüne almanız gerekir. Tam bir sistem yeniden dağıtım olmadan dağıtımdan sonra kimlik sağlayıcıları geçemezsiniz.

Katılmak bir Active Directory etki alanı, vb. Kimlik sağlayıcısı seçiminizi şifrelemeyle Kiracı sanal makineleri, kimlik sistemi ve kullandıkları, hesapları vardır. Bu ayrıdır.

**Nedeniyle Azure AD kullanmayı deneyin**

- Mevcut yatırımların (Azure veya Office 365 gibi) Azure AD'nda zaten sahip.
- Azure ve Azure yığın Bulutlar arasında aynı kimlik kullanmak istediğiniz.
- Farklı kuruluşlarda aynı Azure yığın örneğinde barındırabildiği çoklu kiracı senaryoları desteklemek istediğiniz.
- Sağlama kullanıcılar, gruplar, vb. API'leri aracılığıyla Azure AD grafik aracılığıyla REST tabanlı dizin yönetimi kullanmak istediğiniz.

> [!NOTE]
> Aynı anda Azure AD örneğinde ve var olan bir AD FS örneği için bir Azure yığın dağıtımına bağlanamıyor. Azure AD ile dağıtma ve var olan bir AD FS örneğini kullanmak istiyorsanız, şirket içi devredebilir Azure AD ile AD FS örneği.

**AD FS kullanarak dikkate alınması gereken nedenler**

- Yok yok veya yalnızca kısmi internet bağlantısı.
- Yasal/Egemenlik gereksinimi yoktur.
- İşleç ve kullanıcı hesapları için kendi kimlik sistemi (örneğin, Kurumsal Active Directory) kullanmayı tercih. Bunu yapmak için Active Directory tarafından desteklenen mevcut AD FS örneği (Windows Server 2012, 2012 R2 veya 2016) ile devredebilir.
- Azure hiçbir Yatırımlar varsa ve Azure AD kullanmak istemiyorsanız.

> [!NOTE]
> Yalnızca Active Directory tarafından desteklenen başka bir AD FS örneği ile Azure yığın devredebilir. Diğer kimlik sağlayıcılardan desteklenmez. Azure yığın ile kullanmak istediğiniz diğer kimlik sağlayıcılardan varsa, Azure AD tabanlı dağıtım kullanmayı düşünün.

### <a name="ad-fs-and-graph-integration"></a>AD FS ve grafik tümleştirme

Azure AD FS kimlik sağlayıcısı olarak kullanarak yığın dağıtmak isterseniz var olan bir AD FS örneği bir federasyon güveni üzerinden ile Azure yığında AD FS örneğini tümleştirmeniz gerekir. Bu kaynakları Azure yığınında kimlik doğrulaması yapmak için var olan bir Active Directory ormanında kimlikleri sağlar.

Ayrıca, mevcut Active Directory ile Azure yığın grafik hizmetinde tümleştirebilirsiniz. Bu rol tabanlı erişim denetimi (RBAC) Azure yığınında yönetmenize olanak sağlar. Bir kaynağa erişim yetkisi aktarıldığında LDAP protokolünü kullanarak var olan Active Directory ormanındaki kullanıcı hesabı grafik bileşeni arar.

Aşağıdaki diyagramda tümleşik AD FS ve grafik trafik akışı gösterilmektedir.
![AD FS ve grafik trafik akışını gösteren diyagram](media/azure-stack-planning-considerations/ADFSIntegration.PNG)

## <a name="licensing-model"></a>Lisans modeli

Kullanmak istediğiniz hangi lisans modeli karar vermeniz gerekir. Bağlı bir dağıtım için ödeme olarak-size-kullanım ve kapasite tabanlı lisans seçebilirsiniz. Ödeme olarak,-kullanımlı Azure bağlantısı Azure ticaret sonra faturalandırılır rapor kullanımını gerektirir. Yalnızca kapasite tabanlı lisans desteklenir dağıtırsanız internet bağlantısı kesilmiş. Lisans modelleri hakkında daha fazla bilgi için bkz: [Microsoft Azure paketleme ve fiyatlandırma yığını](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf).

## <a name="naming-decisions"></a>Adlandırma kararları

Azure yığın ad alanınız, özellikle bölge adını ve dış etki alanı adını planlama istediğiniz şekli hakkında düşünmek gerekir. Azure yığın dağıtımınız genel kullanıma yönelik uç noktalar için tam etki alanı adını (FQDN) bu iki ad birleşimidir &lt; *bölge*&gt;&lt;*external_FQDN*  &gt;, örneğin, *east.cloud.fabrikam.com*. Bu örnekte, Azure yığın portalları aşağıdaki URL'lere kullanılabilir olur:

- https://Portal.East.cloud.fabrikam.com
- https://adminportal.East.cloud.fabrikam.com

Aşağıdaki tabloda, bu etki alanı adlandırma kararlar özetlenmektedir.

| Ad | Açıklama | 
| -------- | ------------- | 
|Bölge adı | İlk Azure yığın bölgenizi adı. Bu ad, Azure yığın yöneten ortak sanal IP adreslerinin (VIP) FQDN bir parçası olarak kullanılır. Genelde, bölge adı bir veri merkezi konum gibi bir fiziksel konum tanımlayıcısı olacaktır. | 
| Dış etki alanı adı | Dışa dönük VIP'ler ile uç noktaları için etki alanı adı sistemi (DNS) dilimi adı. FQDN ile bu ortak VIP'ler için kullanılır. | 
| Özel (iç) etki alanı adı | Altyapı yönetimi için Azure yığında oluşturulan etki alanı (ve iç DNS bölgesi) adı. 
| | |

## <a name="certificate-requirements"></a>Sertifika gereksinimleri

Dağıtım için genel kullanıma yönelik uç noktalar için Güvenli Yuva Katmanı (SSL) sertifikalar sağlaması gerekir. Yüksek bir düzeyde sertifikalar aşağıdaki gereksinimlere sahiptir:

> [!IMPORTANT]
> Bu makalede sertifika bilgilerini yalnızca genel bir yönerge olarak sağlanır. Azure yığını için herhangi bir sertifika edinin önce OEM donanım ortağınızla birlikte çalışır. Bunlar daha ayrıntılı sertifika yönerge ve gereksinimleri sağlar.

- Tek joker sertifikası kullanabilir veya özel sertifika kümesi kullanın ve yalnızca depolama ve anahtar kasası gibi bitiş noktası için joker karakter kullan.
- Bir ortak güvenilen sertifika yetkilisi (CA) sertifikaları veren veya müşteri tarafından yönetilen bir CA.
 
Aşağıdaki tabloda Hizmetleri ve İlk Azure yığın dağıtımı için sertifika gerektiren genel kullanıma yönelik uç noktalarının sayısını gösterir. 

| İçin kullanılır | Uç Nokta 
| -------- | ------------- | 
| Azure Kaynak Yöneticisi (Yönetici) | adminmanagment. [Bölge]. [external_domain] |
| Azure Resource Manager (kullanıcı) | yönetimi. [Bölge]. [external_domain] |
| Portal (Yönetici) | adminportal. [Bölge]. [external_domain] |
| Portal (kullanıcı) | Portal. [Bölge]. [external_domain] |
| Anahtar kasası (kullanıcı) | &#42;. Kasa. [Bölge]. [external_domain] |
| Anahtar kasası (Yönetici) | &#42;. adminvault. [Bölge]. [external_domain] |
| Depolama | &#42;. BLOB. [Bölge]. [external_domain]<br>&#42;. Tablo. [Bölge]. [external_domain]<br>&#42;. sıra. [Bölge]. [external_domain]  |
| Grafik ** | Grafik. [Bölge]. [external_domain] |
| AD FS ** | ADFS. [Bölge]. [external_domain] |
| | |

** Grafik ve AD FS bitiş noktaları için sertifikaları yalnızca AD FS dağıtımları için gereklidir.

Tek joker sertifikası kullanmak istiyorsanız, ilk Azure yığın dağıtım için altı ilgili alternatif adlarına (SAN'lar) toplam gerekir. Bu SANs şunlardır: 

- &#42;. [Bölge]. [external_domain]
- &#42;. Kasa. [Bölge]. [external_domain]
- &#42;. adminvault. [Bölge]. [external_domain]
- &#42;. BLOB. [Bölge]. [external_domain]
- &#42;. Tablo. [Bölge]. [external_domain] 
- &#42;. sıra. [Bölge]. [external_domain]

## <a name="time-synchronization"></a>Zaman eşitleme

Azure yığın saat sunucusu IP adresi çözümlenebilir bir dış saat sunucusu ile eşitlemeniz gerekir. Kurumsal ağ üzerindeki bir saat sunucusu bağlantısı kesilmiş bir dağıtım için gereklidir.

## <a name="network-connectivity"></a>Ağ bağlantısı

### <a name="dns-integration"></a>DNS tümleştirmesi

Dış DNS adlarını (örneğin, www.bing.com) Azure yığından çözümlemek için Azure yığın Azure yığın yetkili değil DNS isteklerini iletmek için kullanabileceğiniz DNS sunucuları sağlamak gerekir. Dağıtım giriş olarak hataya dayanıklılık için DNS ileticileri kullanmak üzere en az iki sunucu sağlamanız gerekir.

Dış Azure yığınından (örneğin, Kurumsal ormanı) Azure yığın uç noktalarının DNS adlarını çözmek için Azure yığını için kullanmak istediğiniz üst bölgeyi barındıran DNS sunucuları ile dış DNS bölgesini barındıran DNS sunucularını tümleştirmeniz gerekir. Bu, Azure yığını ve veri merkezinde var olan DNS bölgeleri arasında DNS ad çözümlemesi gerektirir. Bunu başarmak için koşullu iletme veya bölge temsilcisi gibi yöntemleri kullanacaksınız. Üst bölgeyi barındıran Azure yığın dış DNS ad alanınız için DNS sunucuları üzerinde doğrudan denetim varsa, iletme koşullu öneririz. (Dış Azure yığın DNS bölgenizi şirket etki alanı adınızı (örneğin, azurestack.contoso.com ve contoso.com) bir alt etki alanı olarak görünüyorsa, bölge temsilcisi yerine kullanmanız gerekir.

### <a name="network-infrastructure"></a>Ağ altyapısı

Azure yığını için ağ altyapısı anahtarlar yapılandırılmış birden fazla mantıksal ağları oluşur. Aşağıdaki diyagramda, bu mantıksal ağları ve bunlar üst üstü ile (TOR), temel kart yönetim denetleyicisi (BMC) tümleştirme ve nasıl (müşteri ağ) anahtarları kenarlık gösterilmektedir.

![Mantıksal ağ bağlantılarının diyagramını ve anahtarı oluştur](media/azure-stack-planning-considerations/NetworkDiagram.png)

Aşağıdaki tabloda Mantıksal ağlar ve planlamanız gerekir ilişkili IPv4 alt ağ aralıklarını gösterir.

| Mantıksal ağ | Açıklama | Boyut | 
| -------- | ------------- | ------------ | 
| Genel VIP | Azure yığın hizmetleriyle Kiracı sanal makineler tarafından kullanılan rest, küçük bir kümesini ortak IP adresleridir. Azure yığın altyapısı bu ağ 32 adreslerini kullanır. Bu, App Service ve SQL kaynak sağlayıcıları kullanmayı planlıyorsanız, 7 daha fazla kullanır. | / 26 (62 konakları) - /22 (1022 ana bilgisayarları)<br><br>Önerilen /24 (254 ana) = | 
| Anahtar altyapısı | Noktadan noktaya IP adreslerini yönlendirme amacıyla, ayrılmış yönetim arabirimleri ve anahtara atanmalıdır geri döngü adresleri geçin. | /26 | 
| Altyapı | Azure yığın iç bileşenleri için iletişim kurmak için kullanılır. | /24 |
| Özel | Depolama ağı ve özel VIP'ler için kullanılır. | /24 | 
| BMC | Fiziksel ana bilgisayarlarda bmc'li iletişim kurmak için kullanılır. | /27 | 
| | | |

### <a name="uplink-to-border-switches"></a>Kenarlık anahtarları yukarı

Kenarlık anahtarları, veri merkezinizdeki için yapılandırılmış bir yukarı gerekir. Aşağıdaki yöntemlerden birini kullanarak bir Azure tümleşik yığını sistem parçası olan üst üstü (TOR) anahtarları Bu katman 3 trafiği yönlendirebilir:

- Sınır Ağ Geçidi Protokolü (BGP) 
- statik yönlendirme

Yazılım Yük Dengelemesi çoğaltıcı (SLB MUX) dış ağlara tarafından yayımlanan yolların otomatik güncelleştirme sağladığından BGP öneririz. Dağıtımdan sonra ortak IP adresi aralıkları eklerseniz, bu önemlidir. Gelen eşliği BGP yaparsanız TOR toplama anahtarına anahtarlarını TOR anahtarlarını bağlı, dağıtım sonrası el ile müdahale olmadan toplama geçiş için otomatik olarak bildirildiğini eklemek ortak IP adresi aralıkları. Statik yönlendirme kullanırsanız, bir ortak IP Adres Bloğu Ekle her zaman yeni aralıkları için rotaları el ile güncelleştirmeniz gerekir.

### <a name="proxy-server"></a>Proxy sunucusu

Azure yığını yalnızca saydam proxy sunucuları destekler. Saydam proxy, herhangi bir özel istemci yapılandırma gerektirmeden ağ katmanında istekleri kesintiye uğratır.

### <a name="publish-azure-stack-services"></a>Azure yığın Hizmetleri'ne yayımlama

Azure yığın Hizmetleri kullanıcıların dış Azure yığınından kullanılabilmesi gerekir. Azure yığın altyapı rolleri için çeşitli uç noktaları ayarlar. Bu uç noktalar VIP'ler ortak IP adresi havuzundan atanır. Dağıtım sırasında belirtilen dış DNS bölge içindeki her bir uç nokta için bir DNS girişi oluşturulur. Örneğin, kullanıcı portalı DNS ana bilgisayar girişi atanır "portal. <*bölge*>. <*external_FQDN*>." 

#### <a name="ports-and-urls"></a>Bağlantı noktaları ve URL'leri

Azure yığın Hizmetleri yapma (portalları gibi Azure Resource Manager, DNS, vb.) kullanılabilir dış ağlara, gelen trafiğe Bu uç noktalar için belirli URL'leri, bağlantı noktalarını ve protokolleri izin vermeniz gerekir.
 
Bir dağıtımda saydam proxy yukarı burada geleneksel proxy sunucusu belirli bağlantı noktalarını ve URL'ler giden iletişim için izin vermelidir. Bu bağlantı noktaları ve URL'leri kimlik, Market dağıtım, düzeltme ve güncelleştirme, kayıt ve kullanım verilerini içerir.

Daha fazla bilgi için bkz.
- [Gelen bağlantı noktalarını ve protokolleri](https://docs.microsoft.com/azure/azure-stack/azure-stack-integrate-endpoints#ports-and-protocols-inbound)
- [Giden bağlantı noktaları ve URL'leri](https://docs.microsoft.com/azure/azure-stack/azure-stack-integrate-endpoints#ports-and-urls-outbound)

### <a name="connect-azure-stack-to-azure"></a>Azure yığın Azure'a bağlanma

Karma bulut senaryosu için nasıl Azure yığın Azure'a bağlanmak istediğiniz planlamanız gerekir. Azure'da sanal ağlar Azure yığınında sanal ağlara bağlanmak için desteklenen iki yöntem vardır: 
- **Siteden siteye**. Bir sanal özel ağ (VPN) bağlantısı üzerinden IPSec (IKE v1 ve IKE v2). Bu tür bir bağlantı, bir VPN cihazı veya Yönlendirme ve Uzaktan Erişim hizmeti (RRAS) gerektirir. Azure VPN ağ geçitleri hakkında daha fazla bilgi için bkz: [VPN Gateway hakkında](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Bu Tünel üzerinden iletişim şifrelenir ve güvenlidir. Ancak, bant genişliği (100-200 MB/sn) tüneli en yüksek verimlilik ile sınırlıdır.
- **Giden NAT**. Varsayılan olarak, Azure yığın tüm sanal makinelerin giden NAT aracılığıyla dış ağlara bağlantı gerekir Azure yığınında oluşturulan her sanal ağ kendisine atanmış bir ortak IP adresi alır. Sanal makine doğrudan bir ortak IP adresi atanır, bir ortak IP adresine sahip yük dengeleyici arkasında olup, sanal ağ VIP kullanarak giden NAT aracılığıyla giden erişime sahip olacaktır. Bu, sanal makine tarafından başlatılan ve dış ağları için (internet veya intranet) hedefleyen iletişimi için çalışır. Dışında sanal makine ile iletişim kurmak için kullanılamaz.

#### <a name="hybrid-connectivity-options"></a>Karma bağlantı seçenekleri

Karma bağlantı için sunmak istediğiniz dağıtım türlerini ve onu dağıtılacağı dikkate almak önemlidir. Bir intranet veya internet dağıtım elde edersiniz ve Kiracı başına ağ trafiğini yalıtmak ihtiyacınız olup olmadığı göz önüne almanız gerekir.

- **Tek Kiracı Azure yığın**. Bir kiracı ise gibi görünen, en az bir ağ iletişimi açısından bakıldığında, bir Azure yığın dağıtımı. Olabilir birçok abonelikleri Kiracı, ancak tüm intranet hizmeti gibi tüm trafiğin aynı ağlar üzerinden geçen. Bir abonelik gelen ağ trafiğinin başka bir abonelik olarak aynı ağ bağlantısı üzerinden geçen ve şifreli bir tünel üzerinden yalıtılmış olması gerekmez.

- **Çok kiracılı Azure yığın**. Bir Azure yığın dağıtımına burada Azure yığınına dış ağlara bağlanan her Kiracı aboneliğinin trafiği diğer kiracılar ağ trafiğinden yalıtılmış olması gerekir.
 
- **İntranet dağıtımı**. Kurumsal intranet üzerinde genellikle özel IP adres alanı ve bir veya daha fazla güvenlik duvarı arkasında bulunur Azure yığın dağıtımı. Ortak Internet üzerinden doğrudan yönlendirilemiyor gibi ortak IP adresleri gerçekten ortak değildir.

- **Internet dağıtım**. Ortak ağa bağlı bir Azure yığın dağıtımına genel VIP aralığının Internet ve kullandığı Internet yönlendirilebilir ortak IP adresleri. Dağıtımı hala bir güvenlik duvarının arkasında kalmaya devam, ancak genel VIP aralığı ortak Internet ve Azure doğrudan erişilebilir değil.
 
Aşağıdaki tabloda uzmanları, simgeler ve kullanım durumları ile karma bağlantı senaryolar özetlenmiştir.

| Senaryo | Bağlantı yöntemi | Uzmanları | Simgeler | İyi için |
| -- | -- | --| -- | --|
| Kiracı Azure yığını, intranet dağıtımı tek | Giden NAT | Daha hızlı aktarımları için daha iyi bant genişliği. Uygulaması kolaydır; ağ geçitleri gerekir. | Şifrelenmemiş trafik; yalıtım veya TOR ötesinde şifreleme yok. | Tüm kiracılar eşit derecede güvenilen nerede Kurumsal dağıtımlar.<br><br>Azure Azure expressroute bağlantı hattına sahip kuruluşlar. |
| Çok kiracılı Azure yığını, intranet dağıtımı | Siteden siteye VPN | VNet Kiracı trafiğinden hedef için güvenlidir. | Bant genişliği, siteden siteye VPN tüneli ile sınırlıdır.<br><br>Sanal ağ ve hedef ağ üzerindeki bir VPN cihazı bir ağ geçidi gerektirir. | Burada bazı Kiracı trafiği Kurumsal dağıtımlar diğer kiracılardan güvenli hale getirilmelidir. |
| Kiracı Azure yığınının Internet dağıtım tek | Giden NAT | Daha hızlı aktarımları için daha iyi bant genişliği. | Şifrelenmemiş trafik; yalıtım veya TOR ötesinde şifreleme yok. | Barındırma senaryolarında nerede Kiracı kendi Azure yığın dağıtımına ve Azure yığın ortamına ayrılmış bir hattı alır. Örneğin, ExpressRoute ve çok protokollü etiket anahtarlama (MPLS).
| Çok kiracılı Azure yığınının Internet dağıtım | Siteden siteye VPN | VNet Kiracı trafiğinden hedef için güvenlidir. | Bant genişliği, siteden siteye VPN tüneli ile sınırlıdır.<br><br>Sanal ağ ve hedef ağ üzerindeki bir VPN cihazı bir ağ geçidi gerektirir. | Barındırma sağlayıcı burada bir çok kiracılı bulut teklifi istediği senaryolarında, kiracılar birbirine ve trafik burada güvenmediğiniz şifrelenmelidir.
|  |  |  |  |  |

#### <a name="using-expressroute"></a>ExpressRoute kullanarak

Azure üzerinden Azure yığın bağlanabileceği [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) tek Kiracı intranet ve çok kiracılı senaryoları için. Sağlanan bir expressroute bağlantı hattı üzerinden gerekir [bağlantı sağlayıcı](https://docs.microsoft.com/azure/expressroute/expressroute-locations).

Aşağıdaki diyagramda bir tek Kiracı senaryo için ExpressRoute gösterir (burada "Müşteri'nin bağlantısı" ExpressRoute devresi).

![Gösteren tek Kiracı ExpressRoute senaryosu diyagramı](media/azure-stack-planning-considerations/ExpressRouteSingleTenant.PNG)

Aşağıdaki diyagramda bir çok kiracılı senaryo için ExpressRoute gösterir.

![Gösteren çok kiracılı ExpressRoute senaryosu diyagramı](media/azure-stack-planning-considerations/ExpressRouteMultiTenant.PNG)

## <a name="external-monitoring"></a>Dış izleme
Azure yığın dağıtımına ve cihazlardan tüm uyarıları, tek bir görünümünü almak için ve raporlama için var olan BT Hizmet Yönetimi iş akışlarını içine uyarıları tümleştirmek için Azure yığın çözümlerini izleme dış veri merkezi ile tümleştirebilirsiniz.

Azure yığın çözümüne eklenmiş, donanım yaşam döngüsü konak donanım için OEM satıcı tarafından sağlanan Yönetim Araçları'nı çalıştıran Azure yığın dışında bilgisayardır. Bu araçlar ya da var olan bir izleme çözümü, veri merkezinizdeki doğrudan tümleştirileceğini diğer çözümleri kullanabilirsiniz.

Aşağıdaki tablo şu anda kullanılabilir seçeneklerinin listesini özetler.

| Alan | Dış izleme çözümü |
| -- | -- |
| Azure yığın yazılım | - [Operations Manager için Azure yığın yönetim paketi](https://azure.microsoft.com/blog/management-pack-for-microsoft-azure-stack-now-available/)<br>- [Nagios eklentisi](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details)<br>-REST tabanlı API çağrıları | 
| Fiziksel sunucuları (Bmc'ler IPMI aracılığıyla) | -Operations Manager satıcı Yönetim Paketi<br>-OEM donanım satıcısı tarafından sağlanan çözümü<br>-Donanım satıcınızla Nagios eklentileri | OEM ortağı destekli izleme çözümü (dahil) | 
| Ağ aygıtlarını (SNMP) | -Operations Manager ağ aygıtı bulma<br>-OEM donanım satıcısı tarafından sağlanan çözümü<br>-Nagios anahtar eklentisi |
| Kiracı abonelik durumunu izleme | - [Windows Azure için System Center Yönetim Paketi](https://www.microsoft.com/download/details.aspx?id=50013) | 
|  |  | 

Aşağıdaki gereksinimleri dikkate alın:
- Kullandığınız çözüm aracısız olması gerekir. Azure yığın bileşenleri içindeki üçüncü taraf aracılar yükleyemezsiniz. 
- System Center Operations Manager kullanmak istiyorsanız, bu Operations Manager 2012 R2 veya Operations Manager 2016 gerektirir.

## <a name="backup-and-disaster-recovery"></a>Yedekleme ve olağanüstü durum kurtarma

Yedekleme ve olağanüstü durum kurtarma için planlama Iaas sanal makineleri ve PaaS hizmetlerini barındıran her iki temel Azure yığın altyapı ve Kiracı uygulamalar ve veriler için planlama içerir. Bunlar için ayrı ayrı planlamanız gerekir.

### <a name="protect-infrastructure-components"></a>Altyapı bileşenlerine koruma

Azure yığını, belirttiğiniz bir paylaşıma altyapı bileşenlerini yedekler.

- Bir dış SMB dosya paylaşımında var olan bir Windows tabanlı bir dosya sunucusu veya bir üçüncü taraf cihaz gerekir.
- Ağ anahtarları ve donanım yaşam döngüsü konak yedekleme için aynı bu paylaşım kullanmanız gerekir. OEM donanım satıcınıza Azure yığınına dış bunlar gibi yedekleme ve geri yükleme bu bileşenlerin kılavuzluk yardımcı olur. OEM satıcının öneriye dayalı yedekleme iş akışları çalıştırmaktan sorumludur.

Geri dönülemez veri kaybı meydana gelirse, dağıtım girişleri ve tanımlayıcıları, hizmet hesapları, CA kök sertifikasını, Federasyon kaynaklarında (bağlantısı kesilmiş dağıtımlar), planları, teklifleri gibi veri yeniden çekirdek oluşturma dağıtımı için altyapı yedekleme kullanabilirsiniz, Abonelikler, kotalar, RBAC İlkesi ve rol atamalarını ve anahtar kasası gizli.
 
### <a name="protect-tenant-applications-on-iaas-virtual-machines"></a>Iaas sanal makinelerdeki Kiracı uygulamaları koruma

Azure yığın geri değil Kiracı uygulamaları ve verileri. Yedekleme ve olağanüstü durum kurtarma koruması için bir hedef Azure yığınına dış planlamanız gerekir. Kiracı koruma Kiracı güdümlü bir etkinliktir. Iaas sanal makineler için kiracılar, dosya klasörler, uygulama verilerini ve sistem durumunu korumak için konuk teknolojileri kullanabilirsiniz. Ancak, bir kuruluş ya da hizmet sağlayıcısı olarak, aynı veri merkezinde veya harici bir bulutta bir yedekleme ve kurtarma çözümü sunmak isteyebilirsiniz.

Linux veya Windows Iaas sanal makineleri yedeklemek için yedekleme ürünleri konuk işletim sistemine erişimi olan dosya, klasör, işletim sistemi durumunu ve uygulama verilerini korumak için kullanmanız gerekir. Üçüncü taraf ürünleri desteklenen veya Azure Backup, System Center Data Center Protection Manager, kullanabilirsiniz.

İkincil bir konuma veri çoğaltabilir ve olağanüstü bir durum oluşursa, uygulama yük devretme düzenlemek için Azure Site Recovery veya desteklenen üçüncü taraf ürünleri kullanabilirsiniz. (Tümleşik sistemlerin ilk sürümünü Azure Site Recovery geri dönme desteği olmaz. Ancak, yeniden çalışma işlemini elle aracılığıyla elde edebilirsiniz.) Ayrıca, yerel çoğaltma (örneğin, Microsoft SQL Server) destekleyen uygulamalar, uygulamanın çalıştığı başka bir konuma veri çoğaltabilirsiniz.

> [!IMPORTANT]
> Bir Iaas sanal makine Konuk düzeyinde çalışır koruma teknolojileri tümleşik sistemlerin ilk sürümünü destekliyoruz. Temel alınan altyapı sunucularındaki aracılar yükleyemezsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Kullanım örnekleri, satın alma, iş ortakları ve OEM donanım satıcıları hakkında daha fazla bilgi için bkz: [Azure yığın](https://azure.microsoft.com/overview/azure-stack/) ürün sayfası.
- Tümleşik sistemleri Azure yığını için yol haritası ve coğrafi kullanılabilirlik hakkında bilgi için teknik incelemesine bakın: [Azure yığın: Azure uzantısı](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 
