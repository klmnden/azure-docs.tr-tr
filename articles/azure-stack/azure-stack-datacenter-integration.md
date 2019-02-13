---
title: Genel veri merkezi tümleştirme konuları için Azure Stack tümleşik sistemleri | Microsoft Docs
description: Artık planlamak ve çok düğümlü Azure Stack ile veri merkezi tümleştirmesi için hazırlanmak için neler yapabileceğinizi öğrenin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: wfayed
ms.lastreviewed: 09/12/2018
ms.openlocfilehash: 5ececb2d3c52a1da8c1a537e6223f17a9b83921f
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56207543"
---
# <a name="datacenter-integration-considerations-for-azure-stack-integrated-systems"></a>Azure Stack tümleşik sistemleri için veri merkezi tümleştirme konuları
Bir Azure Stack tümleşik sisteminde ilgileniyorsanız, dağıtım ve sistem Merkezinizde nasıl uyduğunu önemli planlama konuları anlamanız gerekir. Bu makalede, Azure Stack çok düğümlü sisteminiz için önemli altyapısı kararları vermenize yardımcı olmak için bu konuları üst düzey bir genel bakış sağlar. Bu noktalar anlaşılması, veri merkezinizi Azure Stack dağıtırken OEM donanım satıcınız ile çalışırken yardımcı olur.  

> [!NOTE]
> Azure Stack çok düğümlü sistemleri, yalnızca yetkili donanım satıcıları tarafından alınabilir. 

Azure Stack dağıtmak için hızlı ve sorunsuz bir şekilde Git işlemi yardımcı olmak dağıtım başlamadan önce çözümü sağlayıcınız için planlama bilgileri vermeniz gerekir. Bilgiler, ağ, güvenlik ve kimlik bilgileri ile birçok farklı alanlarını ve karar alıcılar gerektirebilecek birçok önemli kararlar aralıkları gereklidir. Bu nedenle, çekme dağıtım başlamadan önce gerekli tüm bilgileri hazır olmasını sağlamak için birden çok ekiplerinden kuruluşunuzdaki kişilerin gerekebilir. Bunlar öneri kararları için yararlı olabilir bu bilgiler toplanırken donanım satıcınızla iletişim kurmasına yardımcı olabilir.

Araştırma ve gerekli bilgileri toplama sırasında ağ ortamınıza bazı dağıtım öncesi yapılandırma değişikliklerini yapmak gerekebilir. Bu, yönlendiriciler, anahtarlar ve yeni Azure Stack çözüm anahtarları bağlantısını için hazırlamak için güvenlik duvarlarını yapılandırma Azure Stack çözüm için IP adresi alanları ayırma içerebilir. Planlama ile yardımcı kadar çizgili konu alanında Uzman yüklü olduğundan emin olun.

## <a name="capacity-planning-considerations"></a>Kapasite planlaması konuları
Azure Stack çözümünü edinme için değerlendirirken, donanım yapılandırma seçenekleri ve bunların Azure Stack çözüm genel kapasite üzerinde doğrudan etkisi olan yapılmalıdır. Bunlar, CPU, bellek yoğunluğu, depolama yapılandırması ve genel çözüm Ölçek (örneğin sunucuları sayısı) Klasik seçimleri içerir. Geleneksel bir sanallaştırma çözümü, kullanılabilir kapasitesini belirlemek için bu bileşenlerin basit aritmetik geçerli değildir. Azure Stack altyapısını veya yönetim bileşenleri çözüm içinde barındırmak için geliştirilmiştir ilk nedenidir. Çözümün kapasite bazıları ayrılır, dayanıklılık desteklemek üzere ikinci sebebi; Kiracı iş yüklerini bir kesintiyi en aza indiren bir yolla çözümün yazılım güncelleştiriliyor. 

[Azure Stack kapasite Planlayıcısı elektronik](https://aka.ms/azstackcapacityplanner) yaptığınız yardımcı haberdar iki yolla kapasite planlaması ile ilgili kararlar: ya da donanım teklifi seçerek ve kaynakların veya bir birleşimini uyacak şekilde çalışmadan tanımlayarak Azure Stack iş yükü kullanılabilir donanım destekleyebiliyorsa SKU'ları görüntülemek için çalışmaya yöneliktir. Son olarak, Azure Stack planlama ve yapılandırma kararları vermekte yardımcı olacak bir kılavuz ilgili olarak elektronik yöneliktir. 

Elektronik yerine kendi araştırma ve analiz için hizmet vermek için tasarlanmamıştır.  Microsoft, hiçbir taahhütte veya garantide, açık veya zımni, elektronik tabloda sağlanan bilgilere göre yapar.



## <a name="management-considerations"></a>Yönetim değerlendirmeleri
Azure Stack, izole edilmiş bir sistem burada altyapı hem de bir izin kilitli ve ağ açısından. Ağ erişim denetim listeleri (ACL'ler), yetkisiz gelen tüm trafiği ve altyapı bileşenleri arasındaki tüm gereksiz iletişimler engellemek için uygulanır. Bu, yetkisiz kullanıcıların sisteme erişmek zorlaştırır.

Günlük yönetimi ve işlemleri için altyapı için Kısıtlanmamış yönetici erişimi yoktur. Azure Stack operatörleri sistem Yönetici portalı üzerinden veya Azure Resource Manager (aracılığıyla, PowerShell veya REST API) aracılığıyla yönetmeniz gerekir. Hyper-V Yöneticisi'ni veya yük devretme kümesi Yöneticisi gibi diğer yönetim araçlarını sisteme erişimi yoktur. Üçüncü taraf yazılım (örn. aracılar), sistemin korunmasına yardımcı olmak için Azure Stack altyapısının bileşenleri içinde yüklenemez. Dış yönetim ve güvenlik yazılımı ile birlikte çalışabilirlik, PowerShell veya REST API gerçekleşir.

Uyarı dolayımlama adımlarla çözülebilecek olmayan sorunlarını gidermek için daha yüksek bir erişim düzeyi gerekli olmadığında, Microsoft Support çalışmalıdır. Desteği, daha gelişmiş bir işlemi gerçekleştirmeye sistemin geçici tam yönetici erişimi sağlamak için bir yöntem yoktur. 

## <a name="identity-considerations"></a>Kimlik konuları

### <a name="choose-identity-provider"></a>Kimlik sağlayıcısı seçin
Azure Stack dağıtımı, ya da Azure AD veya AD FS için kullanmak istediğiniz hangi kimlik sağlayıcısına göz önünde bulundurmanız gerekir. Kimlik sağlayıcıları, tam bir sistem yeniden dağıtım olmadan dağıtımdan sonra geçiş yapamazsınız. Azure AD hesabı sahibi olmadığınız ve size, bulut hizmet sağlayıcısı tarafından sağlanan bir hesap kullanıyorsanız ve sağlayıcı geçmek ve farklı bir Azure AD kullanmak karar verirseniz hesap, bu noktada f çözümü yeniden dağıtmak için çözüm sağlayıcınıza başvurun gerekecektir veya, ücret ödemeden.

Katılabilmesi için bir Active Directory etki alanı, vb. olup kimlik sağlayıcısı seçtiğiniz Kiracı sanal makineleri, kimlik sistemi ve kullandıkları, hesapları ilgisi yoktur. Ayrı budur.

Bir kimlik sağlayıcısı seçme hakkında daha fazla bilgi [Azure Stack tümleşik sistemleri bağlantı modelleri makale](./azure-stack-connection-models.md).

### <a name="ad-fs-and-graph-integration"></a>AD FS ve graf tümleştirme
Kimlik sağlayıcısı olarak AD FS kullanarak Azure Stack dağıtmayı tercih ederseniz, Azure Stack üzerinde AD FS örneği ile bir federasyon güveni mevcut bir AD FS örneği ile tümleştirmeniz gerekir. Bu, Azure Stack kaynakları ile kimlik doğrulaması yapmak için var olan bir Active Directory ormanında kimlikleri sağlar.

Ayrıca, Azure Stack Graph hizmeti mevcut Active Directory ile tümleştirebilirsiniz. Bu rol tabanlı erişim denetimi (RBAC) Azure Stack'te yönetmenize olanak sağlar. Bir kaynağa erişim yetkisi aktarıldığında LDAP protokolünü kullanarak mevcut Active Directory ormanındaki kullanıcı hesabını grafik bileşeni arar.

Aşağıdaki diyagramda, tümleşik AD FS ve graf trafik akışını gösterir.
![AD FS ve graf trafik akışını gösteren diyagram](media/azure-stack-datacenter-integration/ADFSIntegration.PNG)

## <a name="licensing-model"></a>Lisanslama modeli
Hangi lisans modeli kullanmak istediğiniz karar vermeniz gerekir. Kullanılabilir seçenekler, Azure Stack internet'e bağlı olup olmadığını dağıtma bağlıdır:
- İçin bir [bağlı dağıtım](azure-stack-connected-deployment.md),-,-kullandıkça veya kapasite tabanlı lisanslama seçebilirsiniz. ,-Kullandıkça, ardından Azure ticaret faturalandırılır rapor kullanımına ve bir Azure bağlantısı gerektirir. 
- Yalnızca kapasite tabanlı lisanslama desteklenen varsa, [dağıtma bağlantısı kesildi](azure-stack-disconnected-deployment.md) internet'ten. 

Lisans modelleri hakkında daha fazla bilgi için bkz. [paketleme ve fiyatlandırma Microsoft Azure Stack](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf).


## <a name="naming-decisions"></a>Adlandırma kararları

Azure Stack ad, bölge adını ve dış etki alanı adı özellikle planlamak istediğiniz hakkında düşünmek gerekir. Azure Stack dağıtımınıza genel kullanıma yönelik uç noktalar için dış tam etki alanı adını (FQDN) bu iki ad birleşimidir: &lt; *bölge*&gt;.&lt; *fqdn*&gt;. Örneğin, *east.cloud.fabrikam.com*. Bu örnekte, Azure Stack portalı aşağıdaki URL'lere kullanılabilir olur:

- https://portal.east.cloud.fabrikam.com
- https://adminportal.east.cloud.fabrikam.com

> [!IMPORTANT]
> Azure Stack dağıtımınız için seçtiğiniz bölge adı benzersiz olmalıdır ve portal adresi görünür. 

Aşağıdaki tabloda, bu etki alanı adlandırma kararlar özetler.

| Ad | Açıklama | 
| -------- | ------------- | 
|Bölge adı | İlk Azure Stack bölgenizdeki adı. Bu ad, FQDN bir parçası olarak Azure Stack yöneten genel sanal IP adreslerinin (VIP) kullanılır. Genelde, bölge adını, bir veri merkezi konumu gibi bir fiziksel konum tanımlayıcısı olacaktır.<br><br>Bölge adı yalnızca harf ve sayı 0-9 arasında oluşması gerekir. Gibi hiçbir özel karakterler "-" veya "#", vb. izin verilir.| 
| Dış etki alanı adı | Etki alanı adı sistemi (DNS) bölge adı ile VIP'ler dönük uç noktalar için. Bu genel VIP için FQDN kullanılır. | 
| Özel (iç) etki alanı adı | Altyapı yönetimi için Azure Stack üzerinde oluşturulan etki alanı (ve iç DNS bölgesi) adı. 
| | |

## <a name="certificate-requirements"></a>Sertifika gereksinimleri

Dağıtım için genel kullanıma yönelik uç noktalar için Güvenli Yuva Katmanı (SSL) sertifikalar sağlaması gerekir. Yüksek düzeyde, sertifikaların aşağıdaki gereksinimlere sahiptir:

- Tek bir joker sertifikası kullanabilirsiniz veya bir özel sertifikaları kullanacak ve yalnızca uç noktaları için depolama ve Key Vault gibi joker karakterler kullanın.
- Sertifika, ortak bir güvenilen sertifika yetkilisi (CA) tarafından verilebilir veya müşteri tarafından yönetilen bir CA.

Hangi PKI hakkında daha fazla bilgi için sertifikaları Azure Stack ve onları edinmek bkz, nasıl dağıtmak için gerekli olan [Azure Stack ortak anahtar altyapısı sertifika gereksinimleri](azure-stack-pki-certs.md).  


> [!IMPORTANT]
> Sağlanan PKI sertifika bilgileri, genel bir yönerge olarak kullanılmalıdır. Azure Stack için PKI sertifikalarını edinmeden önce OEM donanım ortağınızla birlikte çalışır. Bunlar daha ayrıntılı sertifika yönerge ve gereksinimlerin sağlar.


## <a name="time-synchronization"></a>Zaman eşitleme
Azure Stack eşitlenecek sunucuyla birlikte kullanılan belirli bir zaman seçmelisiniz.  Birbiriyle iç Hizmetleri kimlik doğrulaması için kullanılan Kerberos bileti oluşturmak için kullanılan zaman eşitleme için Azure Stack ve kendi altyapısı rollerinin önemlidir.

Altyapı bileşenleri çoğunu bir URL çözümlemek için zaman eşitleme sunucusu için bir IP bazı yalnızca IP adresleri destekleyebilir ancak belirtmeniz gerekir. Bağlantısı kesilmiş dağıtım seçeneğini kullanıyorsanız, bir saat sunucusu olduğunuz Kurumsal ağınızdaki emin Azure Stack'te altyapı ağdan erişilebildiğini belirtmeniz gerekir.

## <a name="connect-azure-stack-to-azure"></a>Azure Stack Azure'a bağlanma

Hibrit bulut senaryoları için Azure Stack Azure'a bağlanmak istediğiniz planlamanız gerekir. Azure Stack sanal ağlarda azure'daki sanal ağlara bağlanmak için desteklenen iki yöntem vardır: 
- **Siteden siteye**. Bir sanal özel ağ (VPN) bağlantısı IPSec üzerinden (IKE v1 ve IKE v2). Bu tür bir bağlantı bir VPN cihazı ya da Yönlendirme ve Uzaktan Erişim hizmeti (RRAS) gerektirir. Azure VPN ağ geçitleri hakkında daha fazla bilgi için bkz. [VPN Gateway hakkında](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Bu Tünel üzerinden iletişim şifrelenir ve güvenlidir. Ancak, bant genişliği (100-200 MB/sn) tüneli en fazla üretilen iş ile sınırlıdır.
- **Giden NAT**. Varsayılan olarak, Azure Stack tüm sanal makinelerin dış ağlara giden NAT aracılığıyla bağlantı sahibi olacağını Azure Stack'te oluşturulan her bir sanal ağ, kendisine atanmış bir genel IP adresi alır. Sanal makinenin doğrudan bir genel IP adresi atanmış bir genel IP adresine sahip yük dengeleyici arkasında olan ister, bu sanal ağın VIP kullanarak giden NAT aracılığıyla giden erişime sahip olacaktır. Bu sanal makine tarafından başlatılan ve dış ağlar için (internet veya intranet) giden iletişim için çalışır. Dışında sanal makine ile iletişim kurmak için kullanılamaz.

### <a name="hybrid-connectivity-options"></a>Karma bağlantı seçenekleri

Karma bağlantı için sunmak istediğiniz dağıtım türüne ve nereye dağıtılacağını göz önünde bulundurmanız önemlidir. Kiracı başına ağ trafiğini yalıtmak ihtiyacınız olup olmadığı ve bir intranet veya internet dağıtım sahip olacaksınız göz önünde bulundurmanız gerekir.

- **Tek kiracılı Azure Stack**. Bir kiracı yoksa gibi görünen, en az bir ağ açısından bakıldığında, bir Azure Stack dağıtımı. Olabilir çok abonelikleri Kiracı, ancak herhangi bir intranet hizmeti gibi tüm trafik aynı ağlar üzerinden geçen. Bir abonelik gelen ağ trafiğini başka bir abonelik olarak aynı ağ bağlantısı üzerinden geçer ve şifrelenmiş bir tünel yalıtılmış olması gerekmez.

- **Çok kiracılı Azure Stack**. Bir Azure Stack dağıtımı burada Azure Stack'e dış ağlara bağlanan her Kiracı aboneliğin trafik diğer kiracılardan gelen ağ trafiğini yalıtılmış olması gerekir.
 
- **İntranet dağıtım**. Kurumsal bir intranet üzerinde genellikle özel IP adres alanı ve bir veya daha fazla güvenlik duvarlarının arkasında yer alan bir Azure Stack dağıtımı. Genel internet üzerinden doğrudan yönlendirilemiyor gibi genel IP adreslerini gerçekten ortak değildir.

- **Internet dağıtım**. Ortak ağa bağlı bir Azure Stack dağıtımı, genel VIP aralığının Internet ve kullandığı Internet yönlendirilebilir genel IP adresleri. Dağıtımı yine de bir güvenlik duvarının arkasında kaplayabilir, ancak genel VIP aralığının genel İnternet'e ve Azure doğrudan erişilebilir değil.
 
Aşağıdaki tabloda, karma bağlantı senaryoları uzmanları, simgeler ve kullanım örnekleri özetlenmektedir.

| Senaryo | Bağlantı yöntemi | Uzmanları | Simgeler | İçin iyi |
| -- | -- | --| -- | --|
| Tek bir kiracı Azure Stack, intranet dağıtımı | Giden NAT | Daha hızlı aktarımları için daha iyi bant genişliği. Uygulaması kolaydır; ağ geçitleri gerekir. | Trafik; şifreli yalıtım veya yığın dışında şifrelemesi yok. | Tüm kiracılar eşit güvenilir olduğu kurumsal dağıtımlar.<br><br>Azure'da bir Azure ExpressRoute devresi kurumlar. |
| Çok kiracılı Azure Stack, intranet dağıtımı | Konumdan konuma VPN | Hedef VNet kiracısı giden trafik güvenlidir. | Bant genişliği, siteden siteye VPN tüneli ile sınırlıdır.<br><br>Bir ağ geçidi sanal ağ ve hedef ağ üzerindeki bir VPN cihazı gerektirir. | Burada bazı Kiracı trafiğine Kurumsal dağıtımlar diğer kiracılardan güvenli hale getirilmelidir. |
| Tek bir kiracı Azure Stack, internet dağıtım | Giden NAT | Daha hızlı aktarımları için daha iyi bant genişliği. | Trafik; şifreli yalıtım veya yığın dışında şifrelemesi yok. | Barındırma senaryolarında nerede Kiracı kendi Azure Stack dağıtımı ve Azure Stack ortamına ayrılmış bir bağlantı hattı alır. Örneğin, ExpressRoute ve çok protokollü etiket anahtarlama (MPLS).
| Çok kiracılı Azure Stack, internet dağıtım | Konumdan konuma VPN | Hedef VNet kiracısı giden trafik güvenlidir. | Bant genişliği, siteden siteye VPN tüneli ile sınırlıdır.<br><br>Bir ağ geçidi sanal ağ ve hedef ağ üzerindeki bir VPN cihazı gerektirir. | Burada çok kiracılı bir bulut sunmak için sağlayıcı istediği senaryoları barındırma, kiracılar birbirleriyle ve trafik burada güvenmediğiniz şifrelenmelidir.
|  |  |  |  |  |

### <a name="using-expressroute"></a>ExpressRoute kullanarak

Azure Stack Azure üzerinden bağlanabilirsiniz [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) hem tek kiracılı intranet hem de çok kiracılı senaryolarda. Sağlanan bir ExpressRoute bağlantı hattı üzerinden ihtiyacınız olacak [bağlantı sağlayıcı](https://docs.microsoft.com/azure/expressroute/expressroute-locations).

Aşağıdaki diyagramda, tek kiracılı senaryo için ExpressRoute gösterilmektedir (burada "Müşterinin bağlantısı" ExpressRoute bağlantı hattı).

![Tek kiracılı ExpressRoute senaryoyu gösteren diyagram](media/azure-stack-datacenter-integration/ExpressRouteSingleTenant.PNG)

Aşağıdaki diyagramda, ExpressRoute için çok kiracılı senaryo gösterilmektedir.

![Çok kiracılı ExpressRoute senaryoyu gösteren diyagram](media/azure-stack-datacenter-integration/ExpressRouteMultiTenant.PNG)

## <a name="external-monitoring"></a>Dış izleme
Azure Stack dağıtımı ve cihazlardan tüm uyarıları tek bir görünümünü elde etmek ve uyarılar bilet oluşturma için mevcut BT Hizmet Yönetimi iş akışlarını tümleştirileceği [Azure Stack'i izleme çözümleridışverimerkeziiletümleştirin](azure-stack-integrate-monitor.md).

Azure Stack çözümüyle dahil, donanım yaşam döngüsü konak donanımlara yönelik OEM satıcısı tarafından sağlanan Yönetim Araçları'nı çalıştıran bir Azure Stack dışında bilgisayardır. Bu araçlar veya doğrudan, veri merkezinizdeki mevcut izleme çözümleriyle tümleştirme diğer çözümleri kullanabilirsiniz.

Şu anda kullanılabilir seçeneklerin listesini aşağıdaki tabloda özetlenmiştir.

| Alan | Dış izleme çözümü |
| -- | -- |
| Azure Stack yazılımı | [Azure Stack Operations Manager için Yönetim Paketi](https://azure.microsoft.com/blog/management-pack-for-microsoft-azure-stack-now-available/)<br>[Nagios eklentisi](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details)<br>REST tabanlı API çağrıları | 
| Fiziksel sunucuları (Bmc'ler IPMI aracılığıyla) | OEM donanım - Operations Manager satıcı Yönetim Paketi<br>OEM donanım satıcısı tarafından sağlanan çözümü<br>Donanım satıcısı Nagios eklentileri | OEM iş ortağı destekli izleme çözümü (dahil) | 
| Ağ cihazlarını (SNMP) | Operations Manager ağ cihazı bulma<br>OEM donanım satıcısı tarafından sağlanan çözümü<br>Nagios anahtarı eklentisi |
| Kiracı abonelik durumu izleme | [Windows Azure için System Center Yönetim Paketi](https://www.microsoft.com/download/details.aspx?id=50013) | 
|  |  | 

Aşağıdaki gereksinimleri göz önünde bulundurun:
- Kullandığınız çözüm aracısız olması gerekir. Azure Stack bileşenleri içindeki üçüncü taraf aracılar yükleyemezsiniz. 
- System Center Operations Manager'ı kullanmak istiyorsanız, Operations Manager 2012 R2 veya Operations Manager 2016 gereklidir.

## <a name="backup-and-disaster-recovery"></a>Yedekleme ve olağanüstü durum kurtarma

Yedekleme ve olağanüstü durum kurtarma için planlama, Iaas sanal makinelerini ve PaaS hizmetlerini barındıran hem de temel alınan Azure Stack altyapı ve Kiracı uygulamalarınız ve verileriniz için planlama içerir. Bunları ayrı ayrı planlamanız gerekir.

### <a name="protect-infrastructure-components"></a>Altyapı bileşenlerini koruyun

Yapabilecekleriniz [Azure Stack yedekleme](azure-stack-backup-back-up-azure-stack.md) altyapı bileşenleri için bir SMB paylaşımı belirttiğiniz:

- Mevcut bir Windows tabanlı dosya sunucusu veya bir üçüncü taraf cihaz harici bir SMB dosya paylaşımına gerekir.
- Ağ anahtarları ve donanım yaşam döngüsü konak yedekleme için aynı Bu paylaşımı kullanmanız gerekir. OEM donanım satıcınız, bu Azure Stack için dış olarak yedekleme ve geri yükleme bu bileşenlerin rehberlik yardımcı olur. OEM satıcının öneriler temel alınarak backup iş akışları çalıştırmaktan sorumludur.

Geri dönülemez veri kaybı meydana gelirse, dağıtım girişleri ve tanımlayıcıları, hizmet hesapları, CA kök sertifikasını, Federasyon kaynaklarında (bağlantısı kesilmiş dağıtımlar), planlar, teklifler gibi veri yeniden çekirdek oluşturma dağıtımı için altyapı yedeklemeyi kullanabilirsiniz, Abonelikler, kotalar, RBAC İlkesi ve rol atamalarını ve Key Vault gizli dizileri.
 
### <a name="protect-tenant-applications-on-iaas-virtual-machines"></a>Iaas sanal makinelerinde Kiracı uygulamaları koruyun

Azure Stack yedekleme değil Kiracı uygulamaları ve verileri. Yedekleme ve olağanüstü durum kurtarma koruması için bir hedef Azure Stack için dış planlamanız gerekir. Kiracı koruma Kiracı temelli bir etkinliktir. Iaas sanal makineleri için kiracılar, dosya klasörleri, uygulama verilerini ve sistem durumunu korumak için konuk içi teknolojileri kullanabilirsiniz. Ancak, bir kuruluş veya hizmet sağlayıcısı olarak, aynı veri merkezinde veya bulutta harici olarak bir yedekleme ve kurtarma çözümü sunmak isteyebilirsiniz.

Linux veya Windows Iaas sanal makinelerini yedeklemek için yedekleme ürünleri konuk işletim sistemine erişimi olan dosya, klasör, işletim sistemi durumunu ve uygulama verilerini korumak için kullanmanız gerekir. Desteklenen üçüncü taraf ürünleri veya Azure Backup, System Center Data Center Protection Manager kullanabilirsiniz.

İkincil bir konuma veri çoğaltma ve olağanüstü bir durum oluşursa uygulama yük devretmelerini düzenlemek için Azure Site Recovery veya desteklenen üçüncü taraf ürünler kullanabilirsiniz. Ayrıca, Microsoft SQL Server gibi yerel çoğaltma destekleyen uygulamalar verilerini uygulamanın çalıştığı başka bir konuma çoğaltabilirsiniz.

## <a name="learn-more"></a>Daha fazla bilgi edinin

- Kullanım örnekleri, satın alma, iş ortakları ve OEM donanım satıcıları hakkında daha fazla bilgi için bkz. [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) ürün sayfası.
- Tümleşik sistemler, Azure Stack için yol haritası ve coğrafi kullanılabilirlik hakkında bilgi teknik incelemesine bakın: [Azure Stack: Bir Azure uzantısı](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 

## <a name="next-steps"></a>Sonraki adımlar
[Azure Stack dağıtım bağlantı modelleri](azure-stack-connection-models.md)
