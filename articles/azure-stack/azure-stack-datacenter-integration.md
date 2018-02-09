---
title: "Genel veri merkezinde tümleştirme konuları Azure yığınının tümleşik sistemleri | Microsoft Docs"
description: "Şimdi planlamak ve çok düğümlü Azure yığını ile veri merkezi tümleştirme için hazırlanmak için yapabileceğinizi öğrenin."
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
ms.date: 02/06/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: f93fc95d6bed517cae3adb706f690941f97c366e
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="datacenter-integration-considerations-for-azure-stack-integrated-systems"></a>Azure tümleşik yığını sistemler için veri merkezi tümleştirme konuları
Bir Azure tümleşik yığını sistemde düşünüyorsanız, bazı önemli planlama konuları dağıtım ve sistem merkeziniz nasıl uyduğunu etrafında anlamanız gerekir. Bu makalede, Azure yığını çok düğümlü sisteminiz için önemli altyapı kararları almanıza yardımcı olmak için bu noktalar üst düzey bir genel bakış sağlar. Bu noktalar anlaşılması veri merkeziniz için Azure yığın dağıtırken OEM donanım satıcınızla çalışırken yardımcı olur.  

> [!NOTE]
> Azure yığını çok düğümlü sistemleri yalnızca yetkili donanım satıcıları tarafından alınabilir. 

Azure yığın dağıtmak için hızlı ve sorunsuz Git işlem yardımcı olmak dağıtım başlamadan önce çözüm sağlayıcınızda planlama bilgilerini sağlamanız gerekir. Bilgi aralıkları ağı, güvenlik ve birçok farklı alanlara ve karar alıcılar bilgi gerektirebilir birçok önemli kararları ile kimlik bilgileri gereklidir. Bu nedenle, dağıtım başlamadan önce gerekli tüm bilgileri hazır olmasını sağlamak için birden çok ekibin kuruluşunuzdaki kişilerin çekme gerekebilir. Bu bilgi toplarken, donanım satıcınıza öneriler kararları için yararlı olabilir gibi konuşun yardımcı olabilir.

Araştırma ve gerekli bilgileri toplama sırasında ağ ortamınıza bazı dağıtım öncesi yapılandırma değişiklikleri yapmanız gerekebilir. Bu IP adresi alanlarını, yönlendiriciler, anahtarlar ve yeni Azure yığın çözüm anahtarları bağlantıyı hazırlamak için güvenlik duvarlarını yapılandırma Azure yığın çözüm ayırma içerebilir. Planlama ile Yardım kadar çizgili konu alanında Uzman bulunduğundan emin olun.

## <a name="management-considerations"></a>Yönetim değerlendirmeleri
Azure yığın burada altyapısı hem de bir izin kilitlenmiştir korumalı bir sistem olduğundan ve ağ açısından. Ağ erişim denetimi listeleri (ACL'ler) yetkisiz tüm gelen trafiği ve altyapı bileşenler arasındaki tüm gereksiz iletişim engellemek için uygulanır. Bu sisteme erişmek yetkisiz kullanıcıların zorlaştırır.

Günlük yönetimi ve işlemleri için altyapısına Kısıtlanmamış yönetici erişimi yoktur. Azure yığın operatörleri sistem Yönetici portalı üzerinden veya Azure Resource Manager (aracılığıyla, PowerShell veya REST API) aracılığıyla yönetmeniz gerekir. Hyper-V Yöneticisi'ni veya yük devretme kümesi Yöneticisi gibi diğer yönetim araçları tarafından sisteme erişimi yoktur. Sistem korunmasına yardımcı olmak için üçüncü taraf yazılım (örneğin, aracıları) Azure yığın altyapısının bileşenleri içinde yüklenemez. Dış yönetim ve güvenlik yazılım ile birlikte çalışabilirlik PowerShell veya REST API oluşur.

Uyarı aracı adımlara çözülmüş olmayan sorunlarını gidermek için erişimi daha yüksek düzeyde gerekli olduğunda, Microsoft Support çalışmanız gerekir. Destek daha gelişmiş işlemleri gerçekleştirmek için sistem geçici tam yönetici erişimi sağlamak için bir yöntem yoktur. 

## <a name="identity-considerations"></a>Kimlik konuları

### <a name="choose-identity-provider"></a>Kimlik sağlayıcısı seçin
Ya da Azure AD veya AD FS Azure yığın dağıtım için kullanmak istediğiniz hangi kimlik sağlayıcısı göz önüne almanız gerekir. Tam bir sistem yeniden dağıtım olmadan dağıtımdan sonra kimlik sağlayıcıları geçemezsiniz.

Katılmak bir Active Directory etki alanı, vb. Kimlik sağlayıcısı seçiminizi şifrelemeyle Kiracı sanal makineleri, kimlik sistemi ve kullandıkları, hesapları vardır. Bu ayrıdır.

Bir kimlik sağlayıcısı seçme hakkında daha fazla bilgi edinebilirsiniz [Azure yığın tümleşik sistemleri bağlantı modelleri makale](.\azure-stack-connection-models.md).

### <a name="ad-fs-and-graph-integration"></a>AD FS ve grafik tümleştirme
Azure AD FS kimlik sağlayıcısı olarak kullanarak yığın dağıtmak isterseniz var olan bir AD FS örneği bir federasyon güveni üzerinden ile Azure yığında AD FS örneğini tümleştirmeniz gerekir. Bu kaynakları Azure yığınında kimlik doğrulaması yapmak için var olan bir Active Directory ormanında kimlikleri sağlar.

Ayrıca, mevcut Active Directory ile Azure yığın grafik hizmetinde tümleştirebilirsiniz. Bu rol tabanlı erişim denetimi (RBAC) Azure yığınında yönetmenize olanak sağlar. Bir kaynağa erişim yetkisi aktarıldığında LDAP protokolünü kullanarak var olan Active Directory ormanındaki kullanıcı hesabı grafik bileşeni arar.

Aşağıdaki diyagramda tümleşik AD FS ve grafik trafik akışı gösterilmektedir.
![AD FS ve grafik trafik akışını gösteren diyagram](media/azure-stack-datacenter-integration/ADFSIntegration.PNG)

## <a name="licensing-model"></a>Lisans modeli
Kullanmak istediğiniz hangi lisans modeli karar vermeniz gerekir. Kullanılabilir seçenekler, Azure yığın Internet'e bağlı olup olmadığına dağıtmak bağlıdır:
- İçin bir [bağlı dağıtım](azure-stack-connected-deployment.md), ödeme olarak-size-kullanım ve kapasite tabanlı lisans seçebilirsiniz. Ödeme olarak,-kullanımlı Azure bağlantısı Azure ticaret sonra faturalandırılır rapor kullanımını gerektirir. 
- Kapasite tabanlı yalnızca lisans desteklenir varsa, [dağıtmak bağlantısı kesilmiş](azure-stack-disconnected-deployment.md) internet'ten. 

Lisans modelleri hakkında daha fazla bilgi için bkz: [Microsoft Azure paketleme ve fiyatlandırma yığını](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf).


## <a name="naming-decisions"></a>Adlandırma kararları

Azure yığın ad alanınız, özellikle bölge adını ve dış etki alanı adını planlama istediğiniz şekli hakkında düşünmek gerekir. Dış tam etki alanı adını (FQDN) Azure yığın dağıtımınız genel kullanıma yönelik uç noktalar için bu iki ad birleşimidir: &lt; *bölge*&gt;.&lt; *fqdn*&gt;. Örneğin, *east.cloud.fabrikam.com*. Bu örnekte, Azure yığın portalları aşağıdaki URL'lere kullanılabilir olur:

- https://portal.east.cloud.fabrikam.com
- https://adminportal.east.cloud.fabrikam.com

> [!IMPORTANT]
> Azure yığın dağıtımınız için seçtiğiniz bölge adı benzersiz olmalıdır ve portal adresler görüntülenir. 

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

## <a name="connect-azure-stack-to-azure"></a>Azure yığın Azure'a bağlanma

Karma bulut senaryosu için nasıl Azure yığın Azure'a bağlanmak istediğiniz planlamanız gerekir. Azure'da sanal ağlar Azure yığınında sanal ağlara bağlanmak için desteklenen iki yöntem vardır: 
- **Siteden siteye**. Bir sanal özel ağ (VPN) bağlantısı üzerinden IPSec (IKE v1 ve IKE v2). Bu tür bir bağlantı, bir VPN cihazı veya Yönlendirme ve Uzaktan Erişim hizmeti (RRAS) gerektirir. Azure VPN ağ geçitleri hakkında daha fazla bilgi için bkz: [VPN Gateway hakkında](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Bu Tünel üzerinden iletişim şifrelenir ve güvenlidir. Ancak, bant genişliği (100-200 MB/sn) tüneli en yüksek verimlilik ile sınırlıdır.
- **Giden NAT**. Varsayılan olarak, Azure yığın tüm sanal makinelerin giden NAT aracılığıyla dış ağlara bağlantı gerekir Azure yığınında oluşturulan her sanal ağ kendisine atanmış bir ortak IP adresi alır. Sanal makine doğrudan bir ortak IP adresi atanır, bir ortak IP adresine sahip yük dengeleyici arkasında olup, sanal ağ VIP kullanarak giden NAT aracılığıyla giden erişime sahip olacaktır. Bu, sanal makine tarafından başlatılan ve dış ağları için (internet veya intranet) hedefleyen iletişimi için çalışır. Dışında sanal makine ile iletişim kurmak için kullanılamaz.

### <a name="hybrid-connectivity-options"></a>Karma bağlantı seçenekleri

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

### <a name="using-expressroute"></a>ExpressRoute kullanarak

Azure üzerinden Azure yığın bağlanabileceği [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) tek Kiracı intranet ve çok kiracılı senaryoları için. Sağlanan bir expressroute bağlantı hattı üzerinden gerekir [bağlantı sağlayıcı](https://docs.microsoft.com/azure/expressroute/expressroute-locations).

Aşağıdaki diyagramda bir tek Kiracı senaryo için ExpressRoute gösterir (burada "Müşteri'nin bağlantısı" ExpressRoute devresi).

![Gösteren tek Kiracı ExpressRoute senaryosu diyagramı](media/azure-stack-datacenter-integration/ExpressRouteSingleTenant.PNG)

Aşağıdaki diyagramda bir çok kiracılı senaryo için ExpressRoute gösterir.

![Gösteren çok kiracılı ExpressRoute senaryosu diyagramı](media/azure-stack-datacenter-integration/ExpressRouteMultiTenant.PNG)

## <a name="external-monitoring"></a>Dış izleme
Azure yığın dağıtımına ve cihazlardan tüm uyarıları, tek bir görünümünü almak için ve raporlama için var olan BT Hizmet Yönetimi iş akışlarını içine uyarıları tümleştirmek için yapabileceğiniz [Azure yığın çözümleriniİzlemedışverimerkeziiletümleştirme](azure-stack-integrate-monitor.md).

Azure yığın çözümüne eklenmiş, donanım yaşam döngüsü konak donanım için OEM satıcı tarafından sağlanan Yönetim Araçları'nı çalıştıran Azure yığın dışında bilgisayardır. Bu araçlar ya da var olan bir izleme çözümü, veri merkezinizdeki doğrudan tümleştirileceğini diğer çözümleri kullanabilirsiniz.

Aşağıdaki tablo şu anda kullanılabilir seçeneklerinin listesini özetler.

| Alan | Dış izleme çözümü |
| -- | -- |
| Azure yığın yazılım | [Operations Manager için Azure yığın yönetim paketi](https://azure.microsoft.com/blog/management-pack-for-microsoft-azure-stack-now-available/)<br>[Nagios eklentisi](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details)<br>REST tabanlı API çağrıları | 
| Fiziksel sunucuları (Bmc'ler IPMI aracılığıyla) | OEM donanım - Operations Manager satıcı Yönetim Paketi<br>OEM donanım satıcısı tarafından sağlanan çözümü<br>Donanım satıcısı Nagios eklentileri | OEM ortağı destekli izleme çözümü (dahil) | 
| Ağ aygıtlarını (SNMP) | Operations Manager ağ aygıtı bulma<br>OEM donanım satıcısı tarafından sağlanan çözümü<br>Nagios anahtar eklentisi |
| Kiracı abonelik durumunu izleme | [Windows Azure için System Center Yönetim Paketi](https://www.microsoft.com/download/details.aspx?id=50013) | 
|  |  | 

Aşağıdaki gereksinimleri dikkate alın:
- Kullandığınız çözüm aracısız olması gerekir. Azure yığın bileşenleri içindeki üçüncü taraf aracılar yükleyemezsiniz. 
- System Center Operations Manager kullanmak istiyorsanız, Operations Manager 2012 R2 veya Operations Manager 2016 gereklidir.

## <a name="backup-and-disaster-recovery"></a>Yedekleme ve olağanüstü durum kurtarma

Yedekleme ve olağanüstü durum kurtarma için planlama Iaas sanal makineleri ve PaaS hizmetlerini barındıran her iki temel Azure yığın altyapı ve Kiracı uygulamalar ve veriler için planlama içerir. Bunlar için ayrı ayrı planlamanız gerekir.

### <a name="protect-infrastructure-components"></a>Altyapı bileşenlerine koruma

Yapabilecekleriniz [Azure yığın yedekleme](azure-stack-backup-back-up-azure-stack.md) altyapı bileşenlerini bir SMB paylaşımı, belirttiğiniz:

- Bir dış SMB dosya paylaşımında var olan bir Windows tabanlı bir dosya sunucusu veya bir üçüncü taraf cihaz gerekir.
- Ağ anahtarları ve donanım yaşam döngüsü konak yedekleme için aynı bu paylaşım kullanmanız gerekir. OEM donanım satıcınıza Azure yığınına dış bunlar gibi yedekleme ve geri yükleme bu bileşenlerin kılavuzluk yardımcı olur. OEM satıcının öneriye dayalı yedekleme iş akışları çalıştırmaktan sorumludur.

Geri dönülemez veri kaybı meydana gelirse, dağıtım girişleri ve tanımlayıcıları, hizmet hesapları, CA kök sertifikasını, Federasyon kaynaklarında (bağlantısı kesilmiş dağıtımlar), planları, teklifleri gibi veri yeniden çekirdek oluşturma dağıtımı için altyapı yedekleme kullanabilirsiniz, Abonelikler, kotalar, RBAC İlkesi ve rol atamalarını ve anahtar kasası gizli.
 
### <a name="protect-tenant-applications-on-iaas-virtual-machines"></a>Iaas sanal makinelerdeki Kiracı uygulamaları koruma

Azure yığın geri değil Kiracı uygulamaları ve verileri. Yedekleme ve olağanüstü durum kurtarma koruması için bir hedef Azure yığınına dış planlamanız gerekir. Kiracı koruma Kiracı güdümlü bir etkinliktir. Iaas sanal makineler için kiracılar, dosya klasörler, uygulama verilerini ve sistem durumunu korumak için konuk teknolojileri kullanabilirsiniz. Ancak, bir kuruluş ya da hizmet sağlayıcısı olarak, aynı veri merkezinde veya harici bir bulutta bir yedekleme ve kurtarma çözümü sunmak isteyebilirsiniz.

Linux veya Windows Iaas sanal makineleri yedeklemek için yedekleme ürünleri konuk işletim sistemine erişimi olan dosya, klasör, işletim sistemi durumunu ve uygulama verilerini korumak için kullanmanız gerekir. Üçüncü taraf ürünleri desteklenen veya Azure Backup, System Center Data Center Protection Manager, kullanabilirsiniz.

İkincil bir konuma veri çoğaltabilir ve olağanüstü bir durum oluşursa, uygulama yük devretme düzenlemek için Azure Site Recovery veya desteklenen üçüncü taraf ürünleri kullanabilirsiniz. (Tümleşik sistemlerin ilk sürümünü Azure Site Recovery geri dönme desteği olmaz. Ancak, yeniden çalışma işlemini elle aracılığıyla elde edebilirsiniz.) Ayrıca, yerel çoğaltma (örneğin, Microsoft SQL Server) destekleyen uygulamalar, uygulamanın çalıştığı başka bir konuma veri çoğaltabilirsiniz.

> [!IMPORTANT]
> Bir Iaas sanal makine Konuk düzeyinde çalışır koruma teknolojileri tümleşik sistemlerin ilk sürümünü destekliyoruz. Temel alınan altyapı sunucularındaki aracılar yükleyemezsiniz.

## <a name="learn-more"></a>Daha fazla bilgi edinin

- Kullanım örnekleri, satın alma, iş ortakları ve OEM donanım satıcıları hakkında daha fazla bilgi için bkz: [Azure yığın](https://azure.microsoft.com/overview/azure-stack/) ürün sayfası.
- Tümleşik sistemleri Azure yığını için yol haritası ve coğrafi kullanılabilirlik hakkında bilgi için teknik incelemesine bakın: [Azure yığın: Azure uzantısı](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 

## <a name="next-steps"></a>Sonraki adımlar
[Azure yığın dağıtım bağlantı modeli](azure-stack-connection-models.md)