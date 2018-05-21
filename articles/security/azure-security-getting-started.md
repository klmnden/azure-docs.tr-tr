---
title: Microsoft Azure security ile çalışmaya başlama | Microsoft Docs
description: Bu makalede, Microsoft Azure güvenlik özellikleri ve genel konular varlıklarına bir bulut sağlayıcısı geçirdiğiniz kuruluşlar için genel bakış sağlar.
services: security
documentationcenter: na
author: barclayn
manager: mbaldwin
editor: TomSh
ms.assetid: 8d8a0088-c85a-48e7-bd04-2bc7b78b0691
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/26/2018
ms.author: barclayn
ms.openlocfilehash: a908c242b5d41d5cd61d8775bdbe53f3cdddd3ec
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="getting-started-with-microsoft-azure-security"></a>Microsoft Azure’daki güvenlik özellikleriyle çalışmaya başlama

Derleme veya BT varlıklar için bir bulut sağlayıcısı geçirmek, uygulamalar ve hizmetler ve bulut tabanlı varlıklarınızı güvenliği yönetmek için sağladıkları denetimleri ile verileri korumak için bir kuruluşun yeteneklerini güvenmek.

Azure altyapısı tesisten uygulamalara kadar milyonlarca müşteriye aynı anda hizmet verecek şekilde tasarlanmıştır ve işletmelerin güvenlik ihtiyaçlarını karşılayabilecek güvenilir bir temel sunar. Buna ek olarak, Azure’da çok çeşitli ve yapılandırılabilir güvenlik seçenekleri ile bunlar üzerinde denetim imkanı sunulmaktadır. Böylece, dağıtımlarınıza özel gereksinimleri karşılamak için güvenlik özelliklerini uyarlayabilirsiniz.

Azure güvenliğiyle ilgili bu genel bakış makalesinde şu konuları inceleyeceğiz:

* Azure Hizmetleri ve özellikler, hizmetler ve Azure içindeki verileri güvenli hale getirmek için kullanabilirsiniz.
* Nasıl Microsoft verileriniz ve uygulamalarınız korunmasına yardımcı olmak için Azure altyapı güvenliğini sağlar.

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

BT altyapısı, veri ve uygulama erişimini denetlemek, kritik öneme sahiptir. Microsoft Azure, çok sayıda standartları ve API'ler için Azure Active Directory (Azure AD), Azure Storage ve destek gibi hizmetleri tarafından bu özellikleri sunar.

[Azure AD](../active-directory/active-directory-whatis.md) bir kimlik deposu ve kimlik doğrulama, yetkilendirme ve erişim denetimi kuruluşun kullanıcıları, grupları sağlayan ve nesneleri altyapısı. Azure AD, geliştiricilerin uygulamalarında kimlik yönetimini tümleştirmeleri için verimli bir yöntem de sunar. Endüstri standardı protokoller gibi [SAML 2.0](https://en.wikipedia.org/wiki/SAML_2.0), [WS-Federasyon](https://msdn.microsoft.com/library/bb498017.aspx), ve [Openıd Connect](http://openid.net/connect/) .NET, Java, Node.js ve PHP gibi platformlarda oturum açma olası olun.

REST tabanlı Graph API, geliştiricilere her platformda dizinden okumaya ve dizine yazma olanağı sunar. Desteği aracılığıyla [OAuth 2.0](http://oauth.net/2/), geliştiricilerin mobil oluşturabilir ve Microsoft ve üçüncü taraf tümleştirme web uygulamaları API'leri web ve kendi güvenli web API oluşturma. .Net, Windows Mağazası, iOS ve Android’e yönelik mevcut açık kaynak istemci kitaplıklarına ek olarak yeni kitaplıklar da geliştirilmektedir.

### <a name="how-azure-enables-identity-and-access-management"></a>Azure’da kimlik ve erişim yönetimini etkinleştirme

Azure AD kuruluşunuz için tek başına bulut dizini olarak veya mevcut şirket içi Active Directory ortamınızla tümleşik bir çözüm olarak kullanılabilir. Tümleştirme özelliklerinden bazıları dizin eşitleme ve çoklu oturum açma (SSO) hizmetleridir. Bunlar, mevcut şirket içi kimliklerinizi ulaşabileceği buluta genişletmek ve yönetici ve kullanıcı deneyimini geliştirmek.

Diğer kimlik ve erişim yönetimi özelliklerinden bazıları şunlardır:

* Azure AD, barındırıldıkları yerden bağımsız olarak SaaS uygulamaları için [SSO](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) özelliğini etkinleştirir. Bazı uygulamalar Azure AD federasyonu kullanırken diğerleri parola SSO hizmetinden yararlanır. Federasyon uygulamaları kullanıcı hazırlama ve parola kasası desteği de sunabilir.
* [Azure Storage](https://azure.microsoft.com/services/storage/) verilerine erişim, kimlik doğrulaması ile denetlenir. Her Depolama hesabı bir birincil anahtara sahip ([depolama hesabı anahtarı](https://msdn.microsoft.com/library/azure/ee460785.aspx), veya SAK) ve ikincil bir gizli anahtar (paylaşılan erişim imzası veya SAS).
* Azure AD kimlik Federasyon üzerinden bir hizmet olarak kullanarak sağlar [Active Directory Federasyon Hizmetleri](../active-directory/fundamentals-identity.md), eşitleme ve çoğaltma ile şirket içi dizinleri.
* [Azure çok faktörlü kimlik doğrulaması](../active-directory/authentication/multi-factor-authentication.md) oturum açma işlemleri bir mobil uygulama, telefon araması veya kısa mesaj kullanarak doğrulamasını gerektiren çok faktörlü kimlik doğrulama hizmetidir. Bu Azure AD ile Azure multi-Factor Authentication sunucusu ile ve özel uygulamalar ve dizinler SDK'sını kullanarak güvenli şirket içi kaynaklara yardımcı olmak için kullanılabilir.
* [Azure AD etki alanı Hizmetleri](https://azure.microsoft.com/services/active-directory-ds/) , etki alanı denetleyicilerini dağıtmaya olmadan Azure sanal makineleri bir etki alanına katın olanak tanır. Bu sanal makinelere Kurumsal Active Directory kimlik bilgilerinizle oturum açın ve sanal makinelerin etki alanına katılmış tüm Azure sanal makinelerinde güvenlik temelleri zorlamak için Grup İlkesi kullanarak yönetebilirsiniz.
* [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) yüz milyonlarca kimlikleri için ölçeklenebilir bir yüksek oranda kullanılabilir kimlik genel yönetim hizmeti tüketiciye yönelik uygulamalar için sağlar. Bu hizmet mobil platformlar ve web platformlarıyla tümleştirilebilir. Tüketicileriniz özelleştirilebilir deneyimler aracılığıyla tüm uygulamalarınıza, var olan sosyal hesaplarını kullanarak veya yeni kimlik bilgileri oluşturma oturum açabilir.

## <a name="data-access-control-and-encryption"></a>Veri erişim denetimi ve şifreleme

Microsoft, tüm Azure işlemlerinde Görev Ayrımı ve [En Az Ayrıcalık](https://en.wikipedia.org/wiki/Principle_of_least_privilege) ilkelerini uygular. Azure destek personelinin verilere erişebilmesi için özel olarak izin vermeniz gerekir ve tek seferlik izin verilen bu erişim günlüğe kaydedilir, denetlenir ve müdahale tamamlandıktan sonra iptal edilir.

Azure aktarımda veya durağan verilerin korunması için birden çok yetenekleri de sağlar. Bu veri, dosyaları, uygulamaları, hizmetleri, iletişim ve sürücüler için şifreleme içerir. Azure'da yerleştirme önce bilgilerini şifrelemek ve ayrıca anahtarları, şirket içi veri merkezleri içinde depolar.

![Azure’da Microsoft Kötü Amaçlı Yazılımdan Koruma](./media/azure-security-getting-started/sec-azgsfig1.PNG)

### <a name="azure-encryption-technologies"></a>Azure şifreleme teknolojileri

[Azure AD Raporlama](../active-directory/active-directory-reporting-audit-events.md) özelliğini kullanarak abonelik ortamınıza gerçekleştirilen yönetici erişimleri hakkında bilgi toplayabilirsiniz. Yapılandırabileceğiniz [BitLocker Sürücü Şifrelemesi](https://technet.microsoft.com/library/cc732774.aspx) Azure gizli bilgileri içeren VHD'lerde.

Verilerinizin güvenliğini sağlamanıza yardımcı olacak diğer Azure özellikleri:

* Uygulama geliştiriciler kullanabilir, Azure'da Windows kullanarak dağıtın uygulamalara şifreleme [CryptoAPI](https://msdn.microsoft.com/library/ms867086.aspx) ve .NET Framework.
* İstemci tarafı şifreleme anahtarları Azure Blob Depolama için tamamen denetler. Depolama hizmeti anahtarları asla görmez ve verilerin şifresini çözemez.
* [Azure Rights Management (Azure RMS)](https://technet.microsoft.com/library/jj585026.aspx) (ile [RMS SDK](https://msdn.microsoft.com/library/dn758244.aspx)) dosya ve veri düzeyi şifreleme ve veri sızıntısı önleme ilke tabanlı erişim yönetimi üzerinden sağlar.
* Azure destekler [tablo ve sütun düzeyi şifreleme (TDE/Temizle)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database.aspx) SQL Server sanal makineleri ve onu destekleyen üçüncü taraf Anahtar Yönetimi sunucuları veri merkezlerinde şirket.
* Depolama Hesabı Anahtarları, Paylaşılan Erişim İmzaları, yönetim sertifikaları ve diğer anahtarlar her bir Azure kiracısına özeldir.
* Azure [StorSimple](http://www.microsoft.com/server-cloud/products/storsimple/overview.aspx) karma depolama Azure Storage'a yüklemeden önce bir 128-bit ortak/özel anahtar çifti aracılığıyla verileri şifreler.
* Azure destekler ve SSL/TLS, IPSec ve AES, veri türleri, kapsayıcıları ve taşımaları bağlı olarak dahil olmak üzere çok sayıda şifreleme mekanizmaları kullanır.

## <a name="virtualization"></a>Sanallaştırma

Azure platformu, sanallaştırılmış bir ortam kullanır. Kullanıcı örnekleri çalıştırmak bir fiziksel ana bilgisayar sunucusu erişimi olmayan bir tek başına sanal makineler olarak ve fiziksel kullanarak bu yalıtım zorlanan [işlemci (halka-0/halkası-3) ayrıcalık düzeylerini](https://en.wikipedia.org/wiki/Protection_ring).

Halka 0 en yüksek, halka 3 ise en düşük ayrıcalıklara sahiptir. Daha düşük ayrıcalıklı halkası 1'de konuk işletim sistemini çalıştıran ve en az ayrıcalıklı Halka 3'te uygulamaları çalıştırın. Bu fiziksel kaynak sanallaştırma yöntemi sayesinde konuk işletim sistemi ve hiper yönetici arasında net bir ayrım sağlanarak bu iki bileşen arasında ek bir güvenlik ayrımı yapılmış olur.

Azure hiper yönetici mikro çekirdek gibi davranır ve tüm donanım erişim istekleri Konuk sanal makineleri ana bilgisayar işleme için VMBus adında bir paylaşılan bellek arabirimi kullanarak geçirir. Bu, kullanıcıların sistemde ham okuma/yazma/yürütme erişimine sahip olmasını önler ve sistem kaynaklarının paylaşımıyla ilgili riskleri azaltır.

![Azure’da Microsoft Kötü Amaçlı Yazılımdan Koruma](./media/azure-security-getting-started/sec-azgsfig2.PNG)

### <a name="how-azure-implements-virtualization"></a>Azure’da sanallaştırma uygulamaları

Azure hiper yöneticide uygulanabilir ve bir yapı denetleyicisi aracısı tarafından yapılandırılan bir hiper yönetici Güvenlik Duvarı (paket filtresi) kullanır. Bu bileşen, kiracıların yetkisiz erişimden korunmasına yardımcı olur. Varsayılan olarak, tüm trafiği, bir sanal makine oluşturulduğunda ve ardından doku Denetleyicisi aracı eklemek için paket filtresi yapılandırır engellenir *kuralları ve özel durumları* yetkili trafiğine izin vermek için.

Burada programlanan iki kural kategorisi vardır:

* **Makine yapılandırma veya altyapı kuralları**: varsayılan olarak, tüm iletişimin engellenir. Bir sanal makine, DHCP ve DNS trafiği gönderip almasına izin veren özel durumlar vardır. Sanal makineler ayrıca "Genel" internet'e trafiği göndermek ve küme ve işletim sistemi Etkinleştirme sunucusu içindeki diğer sanal makineleri trafiği göndermek. Sanal makineler listesi izin verilen giden hedef Azure yönlendirici alt içermiyor, bu Azure yönetim uç ve diğer Microsoft özellikleri.
* **Rol yapılandırma dosyası**: Bu gelen erişim denetim kiracının hizmet modelini temel alan listeleri (ACL'ler) tanımlar. Bir kiracı Web ön uç bağlantı noktası 80 üzerinde belirli bir sanal makine varsa bir uç nokta içinde yapılandırıyorsanız Örneğin, ardından Azure tüm IP'ler için TCP bağlantı noktası 80 açar [Azure Klasik dağıtım modeli](../azure-resource-manager/resource-manager-deployment-model.md). Ardından sanal makinede çalışan bir arka uç veya çalışan rolü varsa, yalnızca aynı Kiracı içinde sanal makine için çalışan rolü açar.

## <a name="isolation"></a>Yalıtım

Başka bir önemli bulut güvenlik gereksinimi paylaşılan çok kiracılı mimarisinde dağıtımlar arasında bilgi yetkisiz ve istemeyerek aktarımını önlemek üzere ayrılmasıdır.

Azure uygulayan [ağ erişim denetimi](https://azure.microsoft.com/blog/network-isolation-options-for-machines-in-windows-azure-virtual-networks/) ve VLAN yalıtımı, ACL'leri, ile arasında ayrım yapma yük dengeleyicileri ve IP filtreleri. Dış trafiğini sınırlayan gelen bağlantı noktalarını ve protokolleri, sanal makinelerde tanımlarsınız. Azure ağ sahte trafiğini önlemek ve güvenilir platform bileşenleri için gelen ve giden trafiği kısıtlamak için filtreyi uygular. Sınır koruma cihazlarına uygulanan trafik akış ilkeleri, trafiği varsayılan olarak reddeder.

![Azure’da Microsoft Kötü Amaçlı Yazılımdan Koruma](./media/azure-security-getting-started/sec-azgsfig3.PNG)

İç ağ trafiğini dış trafikten ayırmak için Ağ Adresi Çevirisi (NAT) kullanılır. İç trafik dışarıdan yönlendirilemez. Dışarıdan yönlendirilebilen [sanal IP adresleri](http://blogs.msdn.com/b/cloud_solution_architect/archive/2014/11/08/vips-dips-and-pips-in-microsoft-azure.aspx), yalnızca Azure içinde yönlendirilebilen [iç Dinamik IP](http://blogs.msdn.com/b/cloud_solution_architect/archive/2014/11/08/vips-dips-and-pips-in-microsoft-azure.aspx) adreslerine çevrilir.

Azure sanal makineler için dış trafiğin ACL yönlendiricileri, yük Dengeleyiciler, güvenlik duvarı ve Katman 3 anahtarları. Yalnızca bilinen belirli protokollere izin verilir. ACL'ler yönetimi için kullanılan diğer VLAN'lar için konuk sanal makinelerden kaynaklanan trafiğini sınırlandırmak için yer alır. Ayrıca, işletim sistemi daha fazla ana bilgisayar IP filtreleri aracılığıyla filtre trafiğinin hem veri katmanları bağlantı ve ağ trafiğinde sınırlar.

### <a name="how-azure-implements-isolation"></a>Azure’da yalıtım uygulamaları

Kiracı İş yükleri için altyapı kaynaklarını ayırma için Azure yapı denetleyicisi sorumludur ve sanal makineler ana bilgisayardan tek yönlü iletişim yönetir. Azure hiper yönetici sanal makineler arasındaki bellek ve işlem ayrımı zorunlu kılan ve güvenli bir şekilde konuk işletim sistemi kiracılar için ağ trafiğini yönlendirir. Azure ayrıca kiracılar, depolama ve sanal ağlar için yalıtım uygular.

* Her Azure AD kiracısı güvenlik sınırları kullanarak mantıksal olarak yalıtılmış.
* Azure depolama hesapları her abonelik için benzersiz ve bir depolama hesabı anahtarı kullanarak kimlik doğrulamalı erişimi gerekir.
* Sanal ağlar birleşimi benzersiz özel IP adresleri, güvenlik duvarları ve IP ACL'ler mantıksal olarak yalıtılmış. Yük dengeleyiciler trafiği uç nokta tanımlarını temel alarak uygun kiracılara yönlendirir.

## <a name="virtual-networks-and-firewalls"></a>Sanal ağlar ve güvenlik duvarları

[Dağıtılmış ve sanal ağlar](http://download.microsoft.com/download/4/3/9/43902EC9-410E-4875-8800-0788BE146A3D/Windows%20Azure%20Network%20Security%20Whitepaper%20-%20FINAL.docx) Azure Yardımı'nda özel ağ trafiğinizi diğer Azure sanal ağ trafiğinden mantıksal olarak yalıtılmış olduğundan emin olun.

![Azure’da Microsoft Kötü Amaçlı Yazılımdan Koruma](./media/azure-security-getting-started/sec-azgsfig4.PNG)

Aboneliğiniz birden çok yalıtılmış özel ağlar (ve içermelidir güvenlik duvarı, Yük Dengeleme ve ağ adresi çevirisi).

Azure, trafiği mantıksal olarak ayırmak için her bir Azure kümesinde üç ana ağ ayırma düzeyi sunar. [Sanal yerel ağlar](https://azure.microsoft.com/services/virtual-network/) (VLAN) müşteri trafiği Azure ağı geri kalanından ayırmak için kullanılır. Küme dışından Azure ağına erişim, yük dengeleyicilerle kısıtlanır.

Sanal makinelere gelen ve giden ağ trafiği hiper yönetici sanal anahtar aracılığıyla geçmesi gerekir. İşletim sistemi kök IP Filtresi bileşeni kök sanal makineden Konuk sanal makineler ve Konuk sanal makineleri birbirinden yalıtır. Bir kiracının düğümlerini ve diğer kiracılardan ayırmanın (Müşteri'nin hizmet yapılandırmasını temel alarak) ortak Internet arasındaki iletişimi kısıtlamak için trafik filtreleme gerçekleştirir.

IP Filtresi Konuk sanal makinelerden önlemeye yardımcı olur:

* Sahte trafiği oluşturuluyor.
* Ele almak için değil, onlara trafiğini alma.
* Korumalı altyapı uç noktaları için trafiği yönlendirerek.
* Gönderme veya uygunsuz alma yayın trafiği.

Sanal makinelerinizi üzerine yerleştirebilirsiniz [Azure sanal ağlar](https://azure.microsoft.com/documentation/services/virtual-network/). Bu sanal ağlar, şirket içi ortamlarda yapılandırdığınız ağlara benzer ve genelde bir sanal anahtar ile ilişkilendirilir. Sanal makineler aynı sanal ağa bağlı ek yapılandırmaya gerek olmadan birbiriyle iletişim kurabilir. Sanal ağ içindeki farklı alt ağlar da yapılandırabilirsiniz.

Güvenli iletişim sanal ağınızda yardımcı olması için aşağıdaki Azure sanal ağ teknolojileri kullanabilirsiniz:

* [**Ağ güvenlik grupları (Nsg'ler)**](../virtual-network/security-overview.md). Bir NSG'yi bir veya daha fazla sanal makine örnekleri denetim trafiği, sanal ağınızda bulunan kullanabilirsiniz. NSG’de trafik yönüne, protokole, kaynak adresle bağlantı noktasına ve hedef adresle bağlantı noktasına göre trafiğe izin veren ya da reddeden erişim denetim kuralları yer alır.
* [**Kullanıcı tanımlı yönlendirme**](../virtual-network/virtual-networks-udr-overview.md). Paketlerin sanal gereç yoluyla bir sanal ağ güvenlik Gereci gitmek için belirli bir alt ağa akan paketlerde için sonraki atlama belirtin oluşturma kullanıcı tanımlı yollar tarafından yönlendirilmesini kontrol edebilirsiniz.
* [**IP iletimi**](../virtual-network/virtual-networks-udr-overview.md). Bir sanal ağ güvenlik cihazının kendine gönderilmemiş olan trafiği alabilmesi gerekir. Bir sanal makine başka hedeflere yönelik trafiği alabilmesine izin vermek için sanal makine için IP iletimini etkinleştirin.
* [**Zorlamalı tünel oluşturma**](../vpn-gateway/vpn-gateway-about-forced-tunneling.md). Yeniden yönlendirme veya "bir sanal ağdaki sanal makineleriniz tarafından oluşturulan tüm Internet'e bağlı trafik zorla" zorlamalı tünel sağlar, denetleme ve denetim için siteden siteye VPN tüneli aracılığıyla şirket içi konumunuz yedekleyin.
* [**Uç nokta ACL'lerini**](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Hangi makinelerin gelen bağlantıları Internet üzerinden sanal ağınızdaki bir sanal makine uç nokta ACL'lerini tanımlayarak izin verilen kontrol edebilirsiniz.
* [**İş ortağı ağ güvenliği çözümleri**](https://azure.microsoft.com/marketplace/). Azure Marketi'nde erişmek için iş ortağı ağ güvenlik çözümlerini mevcuttur.

### <a name="how-azure-implements-virtual-networks-and-firewalls"></a>Azure sanal ağlar ve güvenlik duvarları nasıl uygular

Azure güvenlik duvarları paket filtreleme varsayılan olarak tüm konak ile Konuk sanal makineleri uygular. Azure Marketi'nden Windows işletim sistemi görüntüleri Windows Güvenlik Duvarı varsayılan olarak etkin de vardır. Azure genel kullanıma yönelik ağları denetim iletişimleri çevre yük Dengeleyiciler IP müşteri yöneticiler tarafından yönetilen ACL'ler temel.

Azure’un normal çalışma süreçleri veya olağanüstü durum sırasında bir müşterinin verilerini taşıması halinde ilgili işlemler özel ve şifreli iletişim kanallarından gerçekleştirilir. Sanal ağlar ve güvenlik duvarları kullanmak için Azure tarafından işe diğer özellikleri şunlardır:

* **Yerel ana bilgisayar güvenlik duvarı**: Azure Service Fabric ve Azure Storage yok hiper yönetici sahip yerel bir işletim sisteminde çalışır. Bu nedenle windows güvenlik duvarı kuralları önceki iki kümeleriyle yapılandırılır. Performansı iyileştirmek için depolama alanı yerel sistemde çalışır.
* **Ana bilgisayar güvenlik duvarı**: ana bilgisayar güvenlik duvarı, hiper yönetici çalışan ana bilgisayar işletim sistemi korumaktır. Kurallar, yalnızca Service Fabric denetleyicisi izin vermek ve konak işletim sistemi belirli bir bağlantı noktası için iletişim kutularını atlamak için programlanmış. Diğer özel durumlar ise DHCP ve DNS yanıtlarına izin vermek üzere belirlenmiştir. Azure konak işletim sistemi için güvenlik duvarı kuralları şablonu sahip bir makine yapılandırma dosyasını kullanır. Konak dış saldırıdan yalnızca bilinen, kimlik doğrulamalı kaynaklardan gelen iletişimi izin verecek biçimde yapılandırılmış bir Windows Güvenlik Duvarı tarafından korunur.
* **Konuk Güvenlik Duvarı'nı**: (örneğin, konuk işletim sistemini Windows Güvenlik Duvarı parçası) farklı yazılım programlanmış ancak sanal makine anahtarı paket filtre kurallarında çoğaltır. İletişim IP Filtresi ana yapılandırmalar tarafından izin verilen olsa bile Konuk sanal makine Güvenlik Duvarı'nı ya da konuk sanal makineden iletişimleri sınırlamak için yapılandırılabilir. Örneğin, iki birbirine bağlamak için yapılandırılmış, sanal ağlar arasındaki iletişimi kısıtlamak için konuk sanal makine Güvenlik Duvarı'nı kullanmayı seçebilirsiniz.
* **Depolama Güvenlik Duvarı'nı (FW)**: depolama ön uç güvenlik duvarını yalnızca 80/443 numaralı bağlantı noktalarını ve diğer gerekli yardımcı programı bağlantı noktaları trafik filtreleri. Depolama arka uç güvenlik duvarını depolama ön uç sunuculardan yalnızca gelen iletişimi kısıtlar.
* **Sanal ağ geçidi**: [Azure sanal ağ geçidi](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) Azure sanal ağındaki iş yüklerinizi, şirket içi sitelere bağlanan şirketler arası ağ geçidi olarak görev yapar. Şirket içi sitelere bağlanmak için gereken [IPSec siteden siteye VPN tünelleri](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md), aracılığıyla veya [ExpressRoute](../expressroute/expressroute-introduction.md) devreler. IPSec/IKE VPN tünelleri, ağ geçitleri IKE el sıkışmaları gerçekleştirmek ve sanal ağlar ve şirket içi siteler arasında IPSec S2S VPN tünelleri oluşturabilir. Sanal ağ geçitleri de sonlandırmak [noktadan siteye VPN'lerde](../vpn-gateway/vpn-gateway-point-to-site-create.md).

## <a name="secure-remote-access"></a>Güvenli uzaktan erişim

Bulutta depolanan verilerin taşıma sırasında açıklardan yararlanılmasını önlemek ve gizlilikle bütünlüğün korunmasını sağlamak için yeterli korumaya sahip olması gerekir. Buna bir kuruluşun ilke tabanlı, denetlenebilir kimlik ve erişim yönetim sistemleri ile bağlantılı ağ denetimleri dahildir.

Yerleşik şifreleme teknolojisi, dağıtımlar içindeki ve arasındaki, Azure bölgeleri arasındaki ve Azure ile şirket içi veri merkezleri arasındaki iletişimleri şifrelemenizi sağlar. İle sanal makineleri için yönetici erişimine [uzak masaüstü oturumları](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json), [uzaktan Windows PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/09/07/weekend-scripter-remoting-the-cloud-with-windows-azure-and-powershell.aspx), ve Azure Portalı'nı her zaman şifrelenir.

Güvenli, şirket içi veri merkezini buluta genişletmek için Azure hem de sağlar [siteden siteye VPN](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md) ve [noktadan siteye VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md), ayrılmış bağlantılarıyla artı [ExpressRoute](../expressroute/expressroute-introduction.md) (Azure VPN üzerinden sanal ağlara bağlantılar şifrelenir).

### <a name="how-azure-implements-secure-remote-access"></a>Azure’da güvenli uzaktan erişim uygulamaları

Her zaman Azure portala kimlik doğrulaması ve SSL/TLS gerektirir. Yönetim sertifikalarını güvenli yönetimi etkinleştirecek şekilde yapılandırabilirsiniz. Endüstri standardı güvenlik protokolleri gibi [SSTP](https://technet.microsoft.com/magazine/2007.06.cableguy.aspx) ve [IPSec](https://en.wikipedia.org/wiki/IPsec) tam olarak desteklenir.

[Azure ExpressRoute](../expressroute/expressroute-introduction.md), Azure veri merkezleri ile şirketinizde veya bir birlikte bulundurma ortamında bulunan altyapı arasında özel bağlantılar oluşturmanızı sağlar. ExpressRoute bağlantıları ortak İnternet üzerinden geçmemektedir. Bunlar, daha fazla güvenilirlik, yüksek hız, düşük gecikme ve normal Internet tabanlı bağlantılar daha yüksek güvenlik sağlar. Bazı durumlarda, arasında veri aktarma şirket içi konumlara ve ExpressRoute bağlantıları kullanarak Azure önemli maliyet avantajları da sağlar.

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

Kimliği doğrulanmış olayların günlüğe kaydedilmesini bir denetim izi'ni üreten güvenlikle ilişkili Azure sağlar ve izinsiz için dayanıklı olacak şekilde tasarlanmıştır. Bu, güvenlik olay günlüklerini Azure altyapı sanal makineler ve Azure AD gibi sistem bilgilerini içerir. Güvenlik olay izleme DHCP veya DNS sunucusu IP adresi değişiklikleri gibi Olay toplama içerir; erişim bağlantı noktaları, protokolleri veya tasarım gereği engellenmiş durumda olan IP adresleri çalıştı; Güvenlik İlkesi veya Güvenlik Duvarı ayarlarında değişiklikler; hesabı veya grup oluşturma; ve beklenmeyen veya işlemler sürücü yükleme.

![Azure’da Microsoft Kötü Amaçlı Yazılımdan Koruma](./media/azure-security-getting-started/sec-azgsfig5.PNG)

Ayrıcalıklı kullanıcı erişimini ve etkinliklerini, yetkilendirilmiş ve yetkilendirilmemiş erişim girişimlerini, sistem özel durumlarını ve bilgi güvenliği olaylarını kaydeden denetim günlükleri belirli bir süre boyunca saklanır. Günlük toplama ve bekletme ayarlarını kendi gereksinimlerinize göre yapılandırdığınızdan, günlüklerinizin bekletilme süresi size bağlıdır.

### <a name="how-azure-implements-logging-and-monitoring"></a>Azure’da günlüğe kaydetme ve izleme uygulamaları

Azure, yönetilen yerel ve sanal işlem, depolama veya yapı düğümlerine Yönetim Aracıları (MA) ve Azure Güvenlik İzleme (ASM) aracıları dağıtır. Her Yönetim Aracısı, bir hizmet ekibi depolama hesabını Azure sertifika depolama alanından aldığı sertifikayla kimlik doğrulamasından geçirecek ve önceden yapılandırılmış tanılama ve olay verilerini depolama hesabına iletecek şekilde yapılandırılmıştır. Bu aracılar müşterilerin sanal makinelerine dağıtılmaz.

Azure yöneticileri, kimlik doğrulamalı ve denetimli erişim için günlüklere bir web portalı üzerinden erişir. Yöneticiler günlükleri ayrıştırabilir, filtreleyebilir, bağıntı kurabilir ve çözümleyebilir. Günlükler için Azure hizmet ekibi depolama hesapları günlüklerde değişiklik yapılmasını önlemeye yardımcı olmak için doğrudan yönetici erişimine karşı korunur.

Microsoft günlükleri Syslog protokolünü kullanarak ağ aygıtlarının ve Microsoft Denetim toplama Hizmetleri (ACS) kullanarak ana bilgisayar sunucularına toplar. Bu günlükler, şüpheli olayları için uyarılar oluşturulduğunda günlük veritabanına yerleştirilir. Yöneticiler bu günlüklere erişebilir ve onları çözümleyebilir.

[Azure Tanılama](https://msdn.microsoft.com/library/azure/gg433048.aspx), Azure'da çalışan bir uygulamadan tanılama verileri toplamanızı sağlayan Azure özelliğidir. Hata ayıklama ve sorun giderme performansını ölçmek, kaynak kullanımı, trafik analizi, kapasite planlaması izleme ve denetim için tanılama veri budur. Toplanan tanılama verileri Azure depolama hesabına aktarılarak saklanabilir. Aktarımları da zamanlanabilir veya isteğe bağlı.

## <a name="threat-mitigation"></a>Tehdit azaltma

Azure’da yalıtım, şifreleme ve filtrelemeye ek olarak altyapıyı ve hizmetleri korumaya yönelik bir dizi tehdit azaltma sistemi ve işlemi kullanılmaktadır. Bunlara DDoS, ayrıcalık artırma ve [OWASP Top-10](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project) gibi gelişmiş tehditleri tespit etmek ve azaltmak için kullanılan iç denetim ve teknolojiler dahildir.

Microsoft’un bulut altyapısının güvenliğini sağlamak için uyguladığı güvenlik denetimleri ve risk yönetim süreçleri, güvenlik olayı riskini azaltmaktadır. Bir olay oluşursa, güvenlik olay Yönetimi (SIM) ekibin Microsoft çevrimiçi güvenlik hizmetleri ve uyumluluk (OSSC) ekibin içinde herhangi bir zamanda yanıtlamak hazırdır.

### <a name="how-azure-implements-threat-mitigation"></a>Azure’da tehdit azaltma uygulamaları

Azure güvenlik denetimleri tehdit hafifletme ve müşterilerin kendi ortamlarında olası tehditlerin azaltılmasına yardımcı olmak için yerinde sahiptir. Aşağıdaki listede, Azure tarafından sunulan tehdit azaltma özellikleri özetlenmektedir:

* [Azure kötü amaçlı yazılımdan koruma](azure-security-antimalware.md) tüm altyapı sunucuları üzerinde varsayılan olarak etkindir. İsteğe bağlı olarak, kendi sanal makinelerde etkinleştirebilirsiniz.
* Microsoft, tehditleri algılamak ve açıklardan yararlanılmasını önlemek için sunucuları, ağları ve uygulamaları sürekli izlemektedir. Otomatik uyarılar, yöneticileri olağan dışı davranışlar konusunda uyararak hem iç hem de dış tehditlere karşı düzeltme eylemi gerçekleştirmelerini sağlar.
* Web uygulaması güvenlik duvarı gibi aboneliklerinizi içinde üçüncü taraf güvenlik çözümlerini dağıtmak [Barracuda](https://techlib.barracuda.com/ng54/deployonazure).
* Microsoft'un yaklaşım sızma testi içerir "[kırmızı gruplama](http://download.microsoft.com/download/C/1/9/C1990DBA-502F-4C2A-848D-392B93D9B9C3/Microsoft_Enterprise_Cloud_Red_Teaming.pdf)," Microsoft Güvenlik uzmanları (müşteri olmayan) dinamik üretim sistemleri savunma gerçek, Gelişmiş, kalıcı tehditlere karşı test etmek için Azure saldırmak içerir.
* Tümleşik dağıtım sistemleri, Azure platformunda güvenlik düzeltme eklerinin dağıtımını ve yüklenmesini yönetir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/)

[Azure Güvenlik Ekibi Blogu](http://blogs.msdn.com/b/azuresecurity/)

[Microsoft Güvenlik Yanıt Merkezi](https://technet.microsoft.com/library/dn440717.aspx)

[Active Directory Blogu](http://blogs.technet.com/b/ad/)
