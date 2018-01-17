---
title: "Planlama konuları Azure yığınının tümleşik sistemleri | Microsoft Docs"
description: "Şimdi planlamak ve çok düğümlü Azure yığını için hazırlanmak için yapabileceğinizi öğrenin."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 6ba6bed8321e1ffde8bc8959443682725da36827
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="planning-considerations-for-azure-stack-integrated-systems"></a>Planlama konuları Azure yığınının sistemleri tümleşik
Bir Azure tümleşik yığını sistemde düşünüyorsanız, bazı önemli planlama konuları dağıtım ve sistem merkeziniz nasıl uyduğunu etrafında anlamanız gerekir. Bu makalede, Azure yığını çok düğümlü sisteminiz için önemli altyapı kararları almanıza yardımcı olmak için bu noktalar üst düzey bir genel bakış sağlar. Bu noktalar anlaşılması veri merkeziniz için Azure yığın dağıtırken OEM donanım satıcınızla çalışırken yardımcı olur.  

> [!NOTE]
> Azure yığını çok düğümlü sistemleri yalnızca yetkili donanım satıcıları tarafından alınabilir. 

Azure bir dizi düzgün Azure yığın ortamınız ile tümleştirmek yapmanız gereken kararlar vardır yığını dağıtmak için. Bu bilgiler Planlama işlemi sırasında çözüm sağlayıcınızda ve hızlı ve sorunsuz Git işlem yardımcı olmak dağıtım başlamadan önce donanım satıcısının hazır olması gerekir.

Bilgi aralıkları ağı, güvenlik ve birçok farklı alanlara ve karar alıcılar bilgi gerektirebilir birçok önemli kararları ile kimlik bilgileri gereklidir. Bu nedenle, dağıtım başlamadan önce gerekli tüm bilgileri hazır olmasını sağlamak için birden çok ekibin kuruluşunuzdaki kişilerin çekme gerekebilir. Bu bilgi toplarken, donanım satıcınıza öneriler kararları için yararlı olabilir gibi konuşun yardımcı olabilir.

Araştırma ve gerekli bilgileri toplama sırasında ağ ortamınıza bazı dağıtım öncesi yapılandırma değişiklikleri yapmanız gerekebilir. Bu IP adresi alanlarını, yönlendiriciler, anahtarlar ve yeni Azure yığın çözüm anahtarları bağlantıyı hazırlamak için güvenlik duvarlarını yapılandırma Azure yığın çözüm ayırma içerebilir. Planlama ile Yardım kadar çizgili konu alanında Uzman bulunduğundan emin olun.

## <a name="management-considerations"></a>Yönetim değerlendirmeleri
Azure yığın burada altyapısı hem de bir izin kilitlenmiştir korumalı bir sistem olduğundan ve ağ açısından. Ağ erişim denetimi listeleri (ACL'ler) yetkisiz tüm gelen trafiği ve altyapı bileşenler arasındaki tüm gereksiz iletişim engellemek için uygulanır. Bu sisteme erişmek yetkisiz kullanıcıların zorlaştırır.

Günlük yönetimi ve işlemleri için altyapısına Kısıtlanmamış yönetici erişimi yoktur. Azure yığın operatörleri sistem Yönetici portalı üzerinden veya Azure Resource Manager (aracılığıyla, PowerShell veya REST API) aracılığıyla yönetmeniz gerekir. Hyper-V Yöneticisi'ni veya yük devretme kümesi Yöneticisi gibi diğer yönetim araçları tarafından sisteme erişimi yoktur. Sistem korunmasına yardımcı olmak için üçüncü taraf yazılım (örneğin, aracıları) Azure yığın altyapısının bileşenleri içinde yüklenemez. Dış yönetim ve güvenlik yazılım ile birlikte çalışabilirlik PowerShell veya REST API oluşur.

Uyarı aracı adımlara çözülmüş olmayan sorunlarını gidermek için erişimi daha yüksek düzeyde gerekli olduğunda, destek ile çalışması gerekir. Destek daha gelişmiş işlemleri gerçekleştirmek için sistem geçici tam yönetici erişimi sağlamak için bir yöntem yoktur. 

## <a name="identity-considerations"></a>Kimlik konuları

### <a name="choose-identity-provider"></a>Kimlik sağlayıcısı seçin
Ya da Azure AD veya AD FS Azure yığın dağıtım için kullanmak istediğiniz hangi kimlik sağlayıcısı göz önüne almanız gerekir. Tam bir sistem yeniden dağıtım olmadan dağıtımdan sonra kimlik sağlayıcıları geçemezsiniz.

Katılmak bir Active Directory etki alanı, vb. Kimlik sağlayıcısı seçiminizi şifrelemeyle Kiracı sanal makineleri, kimlik sistemi ve kullandıkları, hesapları vardır. Bu ayrıdır.

Bir kimlik sağlayıcısı seçme hakkında daha fazla bilgi edinebilirsiniz [Azure yığın tümleşik sistemleri makale için dağıtım kararları](.\azure-stack-deployment-decisions.md).

### <a name="ad-fs-and-graph-integration"></a>AD FS ve grafik tümleştirme
Azure AD FS kimlik sağlayıcısı olarak kullanarak yığın dağıtmak isterseniz var olan bir AD FS örneği bir federasyon güveni üzerinden ile Azure yığında AD FS örneğini tümleştirmeniz gerekir. Bu kaynakları Azure yığınında kimlik doğrulaması yapmak için var olan bir Active Directory ormanında kimlikleri sağlar.

Ayrıca, mevcut Active Directory ile Azure yığın grafik hizmetinde tümleştirebilirsiniz. Bu rol tabanlı erişim denetimi (RBAC) Azure yığınında yönetmenize olanak sağlar. Bir kaynağa erişim yetkisi aktarıldığında LDAP protokolünü kullanarak var olan Active Directory ormanındaki kullanıcı hesabı grafik bileşeni arar.

Aşağıdaki diyagramda tümleşik AD FS ve grafik trafik akışı gösterilmektedir.
![AD FS ve grafik trafik akışını gösteren diyagram](media/azure-stack-deployment-planning/ADFSIntegration.PNG)

## <a name="licensing-model"></a>Lisans modeli

Kullanmak istediğiniz hangi lisans modeli karar vermeniz gerekir. Bağlı bir dağıtım için ödeme olarak-size-kullanım ve kapasite tabanlı lisans seçebilirsiniz. Ödeme olarak,-kullanımlı Azure bağlantısı Azure ticaret sonra faturalandırılır rapor kullanımını gerektirir. Yalnızca kapasite tabanlı lisans desteklenir dağıtırsanız internet bağlantısı kesilmiş. Lisans modelleri hakkında daha fazla bilgi için bkz: [Microsoft Azure paketleme ve fiyatlandırma yığını](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf).

## <a name="naming-decisions"></a>Adlandırma kararları

Azure yığın ad alanınız, özellikle bölge adını ve dış etki alanı adını planlama istediğiniz şekli hakkında düşünmek gerekir. Azure yığın dağıtımınız genel kullanıma yönelik uç noktalar için tam etki alanı adını (FQDN) bu iki ad birleşimidir &lt; *bölge*&gt;&lt;*external_FQDN*  &gt;, örneğin, *east.cloud.fabrikam.com*. Bu örnekte, Azure yığın portalları aşağıdaki URL'lere kullanılabilir olur:

- https://portal.east.cloud.fabrikam.com
- https://adminportal.east.cloud.fabrikam.com

Aşağıdaki tabloda, bu etki alanı adlandırma kararlar özetlenmektedir.

| Ad | Açıklama | 
| -------- | ------------- | 
|Bölge adı | İlk Azure yığın bölgenizi adı. Bu ad, Azure yığın yöneten ortak sanal IP adreslerinin (VIP) FQDN bir parçası olarak kullanılır. Genelde, bölge adı bir veri merkezi konum gibi bir fiziksel konum tanımlayıcısı olacaktır. | 
| Dış etki alanı adı | Dışa dönük VIP'ler ile uç noktaları için etki alanı adı sistemi (DNS) dilimi adı. FQDN ile bu ortak VIP'ler için kullanılır. | 
| Özel (iç) etki alanı adı | Altyapı yönetimi için Azure yığında oluşturulan etki alanı (ve iç DNS bölgesi) adı. 
| | |

## <a name="certificate-requirements"></a>Sertifika gereksinimleri

Dağıtım için genel kullanıma yönelik uç noktalar için Güvenli Yuva Katmanı (SSL) sertifikalar sağlaması gerekir. Yüksek bir düzeyde sertifikalar aşağıdaki gereksinimlere sahiptir:

- Tek joker sertifikası kullanabilir veya özel sertifika kümesi kullanın ve yalnızca depolama ve anahtar kasası gibi bitiş noktası için joker karakter kullan.
- Sertifika ortak güvenilen sertifika yetkilisi (CA) tarafından verilmiş veya müşteri tarafından yönetilen bir CA.

Hangi PKI hakkında daha fazla bilgi için sertifikaları Azure yığını ve bunları elde etmek için bkz: nasıl dağıtmak için gereken [Azure yığın ortak anahtar altyapısı sertifika gereksinimleri](azure-stack-pki-certs.md).  


> [!IMPORTANT]
> Sağlanan PKI sertifika bilgilerini genel bir yönerge olarak kullanılmalıdır. Azure yığını için herhangi bir PKI sertifika elde önce OEM donanım ortağınızla birlikte çalışır. Bunlar daha ayrıntılı sertifika yönerge ve gereksinimleri sağlar.



## <a name="time-synchronization"></a>Zaman eşitleme
Sunucuyla Azure yığın eşitlemek için kullanılan belirli bir zaman seçmelisiniz.  İç Hizmetleri birbirleri ile kimlik doğrulama için kullanılan Kerberos anahtarları oluşturmak için kullanılan zaman symbolization Azure yığını ve altyapı rollerinden önemlidir.

Altyapı bileşenlerinde çoğunu bir URL çözebilirsiniz saat eşitleme sunucusu için bir IP bazı yalnızca IP adresleri destekleyebilir ancak belirtmeniz gerekir. Kullanıcısıysanız olan bağlantısı kesilmiş dağıtım seçeneğini kullanarak, Şirket ağınızdaki olduğunuz zaman sunucusu emin ulaşılabilir Azure yığınında altyapı ağdan belirtmelisiniz.


## <a name="network-connectivity"></a>Ağ bağlantısı
Bu bölümde nasıl en iyi kararlar Azure yığın mevcut ağ ortamınıza tümleştirmek önemli yapmanıza yardımcı olacak Azure yığın ağ altyapı bilgileri sağlar. 

> [!NOTE]
> Dış DNS adlarını (örneğin, www.bing.com) Azure yığından çözümlemek için Azure yığın Azure yığın yetkili değil DNS isteklerini iletmek için kullanabileceğiniz DNS sunucuları sağlamak gerekir. Azure yığın DNS hakkında daha fazla bilgi için bkz: gereksinimleri, [Azure yığın datacenter tümleştirmesi - DNS](azure-stack-integrate-dns.md).

### <a name="physical-network-design"></a>Fiziksel ağ tasarımı
Azure yığın çözüm, işlem ve hizmetlerini desteklemek için esnek ve yüksek oranda kullanılabilir fiziksel bir altyapı gerektirir. Bizim önerilen tasarım diyagramı aşağıdadır:

![Önerilen Azure yığın ağ tasarımı](media/azure-stack-deployment-planning/recommended-design.png)


### <a name="logical-networks"></a>Mantıksal ağlar
Mantıksal ağlar temeldeki fiziksel ağ altyapısının bir soyutlamasını temsil eder. Bunlar, konaklar, sanal makineleri ve Hizmetleri için ağ atamalarını düzenlemek ve kolaylaştırmak için kullanılır. Mantıksal ağ oluşturmanın bir parçası ağ alanları sanal yerel ağlar (VLAN'lar), IP alt ağları ve her fiziksel konumdaki mantıksal ağ ile ilişkili IP alt ağı/VLAN çiftleri tanımlamak için oluşturulur.
Azure yığını için ağ altyapısı üzerinde anahtarları yapılandırılmış olan aşağıdaki mantıksal ağları kullanır:

### <a name="network-infrastructure"></a>Ağ altyapısı
Azure yığını için ağ altyapısı anahtarlar yapılandırılmış birden fazla mantıksal ağları oluşur. Aşağıdaki diyagramda, bu mantıksal ağları ve bunlar üst üstü ile (TOR), temel kart yönetim denetleyicisi (BMC) tümleştirme ve nasıl (müşteri ağ) anahtarları kenarlık gösterilmektedir.

![Mantıksal ağ bağlantılarının diyagramını ve anahtarı oluştur](media/azure-stack-deployment-planning/NetworkDiagram.png)

#### <a name="bmc-network"></a>BMC ağ
Bu ağ yönetim ağı için tüm temel kart yönetim denetleyicileri (olarak da bilinen Hizmet işlemciler, örneğin, iDRAC, Ilo, iBMC, vb.) bağlanma ayrılır. Varsa, (HLH) donanım yaşam döngüsü ana bu ağ üzerinde bulunan ve donanım Bakım ve/veya izleme için OEM belirli yazılım sağlayabilir. 

#### <a name="private-network"></a>Özel ağ
Bu /24 (254 ana bilgisayar IP'ın) ağ (Azure yığın bölgesinin kenarlık anahtar aygıtları genişletmez) Azure yığın bölgeye özeldir ve iki alt ağa ayrılmıştır:

- **Depolama ağı**. Dinamik geçiş (126 ana bilgisayar IP'ın) ağ alanları doğrudan ve sunucu ileti bloğu (SMB) depolama trafiği ve sanal makine kullanımını desteklemek için kullanılan bir /25. 
- **İç sanal IP ağ**. A/25-yalnızca iç atanmış VIP'ler yazılım yük dengeleyici için ağ.

#### <a name="azure-stack-infrastructure-network"></a>Azure yığın altyapı ağı
Bu/24 ağ iletişim kurmak ve aralarında veri değişimi iç Azure yığın bileşenleri için ayrılmış. Bu alt ağ olarak yönlendirilebilir IP adreslerinin gerektiriyor, ancak erişim denetim listeleri (ACL'ler) kullanarak çözüme özel tutulur, bir/27 boyutu eşdeğer çok küçük bir aralık dışında kenarlık anahtarları ötesinde yönlendirilecek beklenen değil bunlardan bazıları tarafından kullanılan ağ Dış Kaynaklar ve/veya internet erişimi gerektirdiğinde Hizmetleri. 

#### <a name="public-infrastructure-network"></a>Ortak altyapı ağı
Bu/27 ağdır daha önce bahsedilen Azure yığın altyapı alt ağdan çok küçük bir aralık, genel IP adresleri gerektirmez, ancak bir NAT veya saydam Proxy üzerinden Internet erişimi gerektirir. Bu ağ Acil Durum Kurtarma Konsolu sistem (ERCS) için ayrılacak, ERCS VM Azure kayıt sırasında Internet erişimi gerektirir ve sorun giderme amacıyla yönetim ağınıza yönlendirilebilir olmalıdır.

#### <a name="public-vip-network"></a>Ortak VIP ağ
Ortak VIP ağ Azure yığınında Ağ denetleyicisi atanır. Bir mantıksal ağ anahtarı üzerindeki değil. SLB adres havuzu kullanır ve atar/32 Kiracı İş yükleri için ağları. Geçiş yönlendirme tablosu üzerinde bu 32 IP'leri BGP aracılığıyla kullanılabilir bir yolu olarak bildirildiğini. Bu ağ, dış erişilebilir veya genel IP adresleri içerir. Kalan Kiracı VM'ler tarafından kullanılırken Azure yığın altyapısı bu ortak VIP ağdan en az 8 adres kullanır. Bu alt ağ boyutu en fazla /22 (1022 konakları) için en az /26 (64 konakları) aralığı, bir/24 için planlama öneririz ağ.

#### <a name="switch-infrastructure-network"></a>Anahtar altyapı ağı
Bu/26 ağdır (2 ana bilgisayar IP's) yönlendirilebilir noktadan noktaya IP/30 alt ve ayrılmış geri döngüler içeren alt ağ/32 alt ağlar için bant dışı yönetim ve BGP yönlendirici kimliği geçiş Bu IP adresi aralığı dışarıdan merkeziniz için Azure yığın çözümün yönlendirilebilir olmalıdır, özel veya genel IP'ler olabilir.

#### <a name="switch-management-network"></a>Anahtar Yönetim ağı
Bu /29 (6 ana bilgisayar IP) ayrılmış ağ anahtarlarını yönetim bağlantı noktalarını bağlanma. Dağıtım, yönetim ve sorun giderme için bant dışı erişim sağlar. Yukarıda belirtilen anahtar altyapı ağ üzerinden hesaplanır.

### <a name="transparent-proxy"></a>SAYDAM PROXY
Azure yığın çözüm normal web proxy'leri desteklemez. Veri merkezi bir proxy sunucu kullanmak için tüm trafiği gerektiriyorsa, ağınızdaki bölgeler arasındaki trafiğin ayrılması ilkesine göre işlemek için raf gelen tüm trafiği işlemeye saydam bir proxy yapılandırmanız gerekir. Saydam bir proxy (olarak da bilinen bir kesintiye uğratan, satır içi veya Zorlanmış proxy), herhangi bir özel istemci yapılandırma gerektirmeden ağ katmanında normal iletişimi kesintiye uğratır. İstemcileri proxy varlığını değil bilmeniz gerekir.

### <a name="publish-azure-stack-services"></a>Azure yığın Hizmetleri'ne yayımlama

Azure yığın Hizmetleri kullanıcıların dış Azure yığınından kullanılabilmesi gerekir. Azure yığın altyapı rolleri için çeşitli uç noktaları ayarlar. Bu uç noktalar VIP'ler ortak IP adresi havuzundan atanır. Dağıtım sırasında belirtilen dış DNS bölge içindeki her bir uç nokta için bir DNS girişi oluşturulur. Örneğin, kullanıcı portalı Portalı'nın DNS ana bilgisayar girişi atanır.  *&lt;bölge >.&lt; FQDN >*.

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
| Çok kiracılı Azure yığını, intranet dağıtımı | Konumdan konuma VPN | VNet Kiracı trafiğinden hedef için güvenlidir. | Bant genişliği, siteden siteye VPN tüneli ile sınırlıdır.<br><br>Sanal ağ ve hedef ağ üzerindeki bir VPN cihazı bir ağ geçidi gerektirir. | Burada bazı Kiracı trafiği Kurumsal dağıtımlar diğer kiracılardan güvenli hale getirilmelidir. |
| Kiracı Azure yığınının Internet dağıtım tek | Giden NAT | Daha hızlı aktarımları için daha iyi bant genişliği. | Şifrelenmemiş trafik; yalıtım veya TOR ötesinde şifreleme yok. | Barındırma senaryolarında nerede Kiracı kendi Azure yığın dağıtımına ve Azure yığın ortamına ayrılmış bir hattı alır. Örneğin, ExpressRoute ve çok protokollü etiket anahtarlama (MPLS).
| Çok kiracılı Azure yığınının Internet dağıtım | Konumdan konuma VPN | VNet Kiracı trafiğinden hedef için güvenlidir. | Bant genişliği, siteden siteye VPN tüneli ile sınırlıdır.<br><br>Sanal ağ ve hedef ağ üzerindeki bir VPN cihazı bir ağ geçidi gerektirir. | Barındırma sağlayıcı burada bir çok kiracılı bulut teklifi istediği senaryolarında, kiracılar birbirine ve trafik burada güvenmediğiniz şifrelenmelidir.
|  |  |  |  |  |

#### <a name="using-expressroute"></a>ExpressRoute kullanarak

Azure üzerinden Azure yığın bağlanabileceği [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) tek Kiracı intranet ve çok kiracılı senaryoları için. Sağlanan bir expressroute bağlantı hattı üzerinden gerekir [bağlantı sağlayıcı](https://docs.microsoft.com/azure/expressroute/expressroute-locations).

Aşağıdaki diyagramda bir tek Kiracı senaryo için ExpressRoute gösterir (burada "Müşteri'nin bağlantısı" ExpressRoute devresi).

![Gösteren tek Kiracı ExpressRoute senaryosu diyagramı](media/azure-stack-deployment-planning/ExpressRouteSingleTenant.PNG)

Aşağıdaki diyagramda bir çok kiracılı senaryo için ExpressRoute gösterir.

![Gösteren çok kiracılı ExpressRoute senaryosu diyagramı](media/azure-stack-deployment-planning/ExpressRouteMultiTenant.PNG)

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
