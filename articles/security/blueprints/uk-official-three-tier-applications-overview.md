---
title: Azure güvenlik ve uyumluluk şeması - İngiltere resmi üç katmanlı Web uygulamaları otomatikleştirme
description: Azure güvenlik ve uyumluluk şeması - İngiltere resmi üç katmanlı Web uygulamaları otomatikleştirme
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 9c32e836-0564-4906-9e15-f070d2707e63
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/08/2018
ms.author: jomolesk
ms.openlocfilehash: bb0a667c28e4ed0be3e67a7d89f10903be2c9d2a
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="azure-security-and-compliance-blueprint---uk-offical-three-tier-web-applications-automation"></a>Azure güvenlik ve uyumluluk şeması - İngiltere OFFICAL üç katmanlı Web uygulamaları otomatikleştirme

## <a name="overview"></a>Genel Bakış

 Bu makalede İngiltere'deki resmi olarak sınıflandırılan birçok iş yükü işlemek için uygun bir Microsoft Azure üç katmanlı web tabanlı mimari sunmak için yönergeler ve otomasyon komut dosyaları sağlar.

 Yaklaşan bir altyapı kodu olarak kullanarak, kümesini [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) şablonları UK Ulusal siber Güvenlik Merkezi (NCSC için) 14 hizalar bir ortamda dağıtmak [bulut güvenlik ilkeleri](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) Internet güvenliği (CI) için merkezi [kritik güvenlik denetimleri](https://www.cisecurity.org/critical-controls.cfm).

 NCSC önerilir hizmeti güvenlik özelliklerini değerlendirmek ve Sorumluluk müşteri ve tedarikçi arasında bölme anlamanıza yardımcı olması için kendi bulut güvenlik ilkeleri müşteriler tarafından kullanılabilir. Sorumlulukları bölünmüş anlamanıza yardımcı olması için bu ilkelerin her biri karşı bilgiler sunulmuştur.

 Bu mimari ve karşılık gelen Azure Resource Manager şablonları Microsoft teknik incelemesi tarafından desteklenen [İngiltere 14 bulut güvenlik denetimleri kullanarak Microsoft Azure bulut](https://gallery.technet.microsoft.com/14-Cloud-Security-Controls-670292c1). Bu raporda nasıl Azure Hizmetleri catalogues İngiltere NCSC 14 bulut güvenlik böylece Hızlı İzleme yeteneklerini Microsoft Azure'da bulut tabanlı hizmetlere genel ve Birleşik Krallık kullanarak kendi uyumluluk yükümlülüklerin karşılamak kuruluşlar Etkinleştirme ilkeleri ile hizalayın bulut.

 Bu şablon, iş yükü için altyapıyı dağıtır. Uygulama kodu ve iş katmanı ve veri katmanı yazılımı destekleyici yüklenmiş ve yapılandırılmış olmasını gerekir. Ayrıntılı dağıtım yönergeleri kullanılabilir [burada](https://aka.ms/ukwebappblueprintrepo).

 Hızla ve kolayca - kaydolabilirsiniz sonra bir Azure aboneliğiniz yoksa [Azure ile çalışmaya başlama](https://azure.microsoft.com/get-started/).

## <a name="architecture-diagram-and-components"></a>Mimarisi diyagramı ve bileşenleri

 Birleşik Krallık resmi iş yüklerini destekleyen bir Azure bulut ortamında bulunan bir üç katmanlı web uygulama mimarisi Azure şablonları sunar. Mimari güvenli bir şekilde şirket kullanıcıları tarafından ya da Internet üzerinden erişilebilmesi için Azure olanağı tanıyan web tabanlı iş yükleri bir şirket içi ağına genişletir ve Güvenli Karma bir ortamınız sunar.

![Alternatif metin](images/diagram.png?raw=true "Azure UK OFFICAL üç katmanı mimarisi")

 Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Dağıtım mimarisi ayrıntılarını içinde bulunur [dağıtım mimarisi](#deployment-architecture) bölümü.

(1) /16 sanal ağ - işletimsel VNet
- (3) /24 alt - 3 katmanlı (Web, Biz, veriler)
- (1) /27 subnet - EKLER
- (1) /27 subnet - ağ geçidi alt ağı
- (1) /29 subnet - uygulama ağ geçidi alt ağı
- (Azure tarafından sağlanan) DNS kullandığı varsayılan
- Yönetim Vnet'e etkin eşleme
- Trafik akışını yönetmek için ağ güvenlik grubu (NSG)

(1) /24 sanal ağ - yönetim sanal ağ
- (1) /27 alt ağ
- (2) EKLER DNS ve (1) Azure DNS girişlerini kullanır
- İşletimsel Vnet'e etkin eşleme
- Trafik akışını yönetmek için ağ güvenlik grubu (NSG)

(1) uygulama ağ geçidi
- WAF - etkin
- WAF modu - önleme
- Kural kümesi: OWASP 3.0
- Bağlantı noktası 80 üzerinde HTTP dinleyicisi
- Bağlantı/trafik NSG düzenlemesi
- Genel IP adresi uç noktası (Azure) tanımlı

(1) VPN - rota tabanlı 2-siteye IPSec VPN tüneli
- Genel IP adresi uç noktası (Azure) tanımlı
- Bağlantı/trafik NSG düzenlemesi
- (1) yerel ağ geçidi (şirket içi uç noktası)
- (1) azure ağ geçidi (Azure uç noktası)

(9) Azure Iaas kötü amaçlı yazılımdan koruma DSC ayarlarla dağıtılan sanal makineler - tüm sanal makineler

- (2) active Directory etki alanı Hizmetleri etki alanı denetleyicileri (Windows Server 2012 R2)
  - (2) DNS sunucusu rolleri - VM başına 1
  - (2) NIC'ler işletimsel VNet - VM başına 1 bağlı
  - Her ikisi de etki alanına katılmış şablonda tanımlanan etki alanına
    - Dağıtımının bir parçası olarak oluşturulan etki alanı


- (1) Jumpbox (savunma ana bilgisayarı) yönetim VM
  - Genel IP adresi ile yönetim VNet üzerinde 1 NIC
    - NSG (çıkış) trafiği belirli kaynakları sınırlamak için kullanılır
  - Etki alanına katılmış değil


- (2) web katmanı VM'ler
  - (2) IIS sunucu rolleri - VM başına 1
  - (2) NIC'ler işletimsel VNet - VM başına 1 bağlı
  - Etki alanına katılmış değil


- (2) biz katmanı VM'ler
  - (2) NIC'ler işletimsel VNet - VM başına 1 bağlı
  - Etki alanına katılmış değil


- (2) veri katmanı VM'ler
  - (2) NIC'ler işletimsel VNet - VM başına 1 bağlı
  - Etki alanına katılmış değil

Kullanılabilirlik Kümeleri
- (1) active Directory etki alanı denetleyicisi VM set - 2 VM
- (1) web katmanı VM set - 2 VM
- (1) biz katmanı VM set - 2 VM
- (1) veri katmanı VM set - 2 VM

Load Balancer
- (1) web katmanı yük dengeleyici
- (1) biz katmanı yük dengeleyici
- (1) yük dengeleyici veri katmanı

Depolama
- (14) toplam depolama hesapları
  - Active Directory etki alanı denetleyicisi kullanılabilirlik kümesi
    - (2) birincil yerel olarak yedekli depolama (LRS) hesapları - her VM için 1  
    - (1) tanılama EKLER kullanılabilirlik kümesi için yerel olarak yedekli depolama (LRS) hesabı
  - Yönetim Jumpbox VM
    - (1) birincil yerel olarak yedekli depolama (LRS) hesabı için Jumpbox VM
    - (1) tanılama Jumpbox VM için yerel olarak yedekli depolama (LRS) hesabı
  - Web Katmanı VM'ler
    - (2) birincil yerel olarak yedekli depolama (LRS) hesapları - her VM için 1  
    - (1) tanılama Web Katmanı kullanılabilirlik kümesi için yerel olarak yedekli depolama (LRS) hesabı
  - Biz katmanı VM'ler
    - (2) birincil yerel olarak yedekli depolama (LRS) hesapları - her VM için 1  
    - (1) tanılama Biz katmanı kullanılabilirlik kümesi için yerel olarak yedekli depolama (LRS) hesabı
  - Veri katmanı VM'ler
    - (2) birincil yerel olarak yedekli depolama (LRS) hesapları - her VM için 1  
    - (1) tanılama veri katmanı kullanılabilirlik kümesi için yerel olarak yedekli depolama (LRS) hesabı

### <a name="deployment-architecture"></a>Dağıtım mimarisi:

**Şirket içi ağ**: bir kurum uygulanan özel bir yerel ağ.

**Üretim VNet**: üretim [VNet](https://docs.microsoft.com/azure/Virtual-Network/virtual-networks-overview) (sanal ağ) barındıran uygulama ve Azure üzerinde çalışan diğer işlemsel kaynakları. Her bir Vnet'teki yalıtma ve ağ trafiğini yönetmek için kullanılan çeşitli alt içerebilir.

**Web Katmanı**: gelen HTTP isteklerini işler. Yanıtları bu katmanı döndürülür.

**İş katmanı**: iş süreçlerini ve sistem için işlevsel diğer mantığını uygular.

**Veritabanı katmanı**: kalıcı veri depolama sağlar kullanarak [SQL Server Always On kullanılabilirlik grupları](https://msdn.microsoft.com/library/hh510230.aspx) yüksek kullanılabilirlik için. Müşteriler kullanabilir [Azure SQL veritabanı](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview) bir PaaS alternatif olarak.

**Ağ geçidi**: [VPN ağ geçidi](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) şirket içi ağ yönlendiricileri ve üretim VNet arasında bağlantı sağlar.

**Internet ağ geçidi ve genel IP adresi**: internet ağ geçidi uygulama hizmetlerini kullanıcılara Internet üzerinden kullanıma sunar. Bu hizmetlere erişme trafiği kullanarak güvenli bir [uygulama ağ geçidi](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) katman 7 Yönlendirme ve Yük Dengeleme yeteneklerini web uygulaması Güvenlik Duvarı (WAF) koruması ile teklifi.

**Yönetim VNet**: Bu [VNet](https://docs.microsoft.com/azure/Virtual-Network/virtual-networks-overviewcontains) yönetim ve izleme kapasiteleri VNet üretimde çalışan iş yükleri için uygulama kaynaklarını içerir.

**Jumpbox**: olarak da adlandırılan bir [savunma konak](https://en.wikipedia.org/wiki/Bastion_host), Yöneticiler için sanal makineleri üretim VNet bağlanmak için kullandığınız ağ üzerinde güvenli bir VM olduğu. Sıçrama kutusunun sadece güvenli bir listede yer alan genel IP adreslerinden gelen uzak trafiğe izin veren bir NSG’si vardır. Uzak Masaüstü (RDP) trafiğine izin vermek için kaynak trafiği NSG'de tanımlanması gerekiyor. Üretim kaynaklarının yönetimini güvenli Jumpbox VM kullanarak RDP aracılığıyla ' dir.

**Kullanıcı tanımlı yollar**: [kullanıcı tanımlı yollar](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) Azure Vnet içinde IP trafik akışını tanımlamak için kullanılır.

**Sanal ağlar eşlenen ağ**: üretim ve Yönetim sanal ağlara bağlı kullanarak [VNet eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).
Bu sanal ağlar hala ayrı kaynakları olarak yönetilebilir, ancak bu sanal makineler için tüm bağlantı amaçları için bir tane olarak görünür. Bu ağlar birbirleriyle doğrudan özel IP adresleri kullanarak iletişim kurar. VNet eşlemesi aynı Azure bölgesinde olan sanal ağlar tabidir.

**Ağ güvenlik grupları**: [Nsg'ler](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) erişim denetimi izin veren veya reddeden sanal ağ içinde trafik listeleri içerir. Nsg'ler alt ağ veya tek tek VM düzeyinde trafiğinin güvenliğini sağlamak için kullanılabilir.

**Active Directory etki alanı Hizmetleri (AD DS)**: Bu ayrılmış bir mimarisi [Active Directory etki alanı Hizmetleri](https://technet.microsoft.com/library/hh831484.aspx) dağıtım.

**Günlüğe kaydetme ve Denetim**: [Azure etkinlik günlüğü](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) aboneliğinizde işlemi oluştuğunda, işlemin durumunu ve değerlerini işlemi kimin başlattığını gibi kaynaklar üzerinde gerçekleştirilen işlemler yakalar yardımcı olabilecek diğer özellikleri işlemi araştırın. Azure etkinlik günlüğü bir abonelik tüm eylemleri yakalayan bir Azure platform hizmetidir. Günlükleri, arşivlenen veya gerekirse verilebilir.

**Ağ izleme ve uyarı verme**: [Azure Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) platform hizmet sağlayan ağ paketi yakalama, akış günlüğü, topoloji araçları ve Tanılama, sanal ağlar içinde ağ traffics değil.

## <a name="guidance-and-recommendations"></a>Yönerge ve öneriler

### <a name="business-continuity"></a>İş Sürekliliği

**Yüksek kullanılabilirlik**: sunucu iş yükleri gruplandırılır bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) azure'da sanal makinelerin yüksek kullanılabilirliğini sağlamaya yardımcı olmak için. Bu yapılandırma planlı veya plansız bir bakım olayı sırasında en az bir sanal makinenin kullanılabilir durumda olmasını ve %99,95 karşıladığından emin olun yardımcı olan Azure SLA.

### <a name="logging-and-audit"></a>Günlüğe kaydetme ve Denetim

**İzleme**: [Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started) etkinlik günlüğü, ölçümleri ve tanılama günlükleri, Azure kaynaklarınızın izleme için tek bir kaynak sağlar platform hizmetidir. Azure İzleyici görselleştirme, sorgu, yol, arşiv ve ölçümleri ve Azure kaynaklarında'ten gelen günlükleri hareket için yapılandırılabilir. Kaynak tabanlı erişim denetimi kullanıcılar günlükleri değiştirme yeteneğini olmadığından emin olun yardımcı olmak için denetim izi güvenliğini sağlamak için kullanılır önerilir.

**Etkinlik günlükleri**: yapılandırmak [Azure etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) aboneliğinizde kaynaklara gerçekleştirilen işlemler hakkında bilgi sağlamak için.

**Tanılama günlüklerini**: [tanılama günlüklerini](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) olan bir kaynak tarafından gösterilen tüm günlükleri. Bu günlükler Windows olayı sistem günlükleri, blob, tablo ve kuyruk günlükleri dahil olabilir.

**Güvenlik Duvarı günlüklerini**: uygulama ağ geçidi tam tanılama ve erişim günlükleri sağlar. Güvenlik duvarı günlükleri, WAF’nin etkin olduğu application gateway kaynakları için kullanılabilir.

**Arşivleme oturum**: arşivleme ve tanımlanmış saklama dönemi veri depolama, merkezileştirilmiş bir Azure depolama alanına yazmak üzere yapılandırılabilir günlük hesabı. Azure günlük analizi kullanarak günlükleri işlenebilir veya üçüncü taraf SIEM sistemler tarafından.

### <a name="identity"></a>Kimlik

**Active Directory etki alanı Hizmetleri**: Bu mimarinin Azure Active Directory etki alanı Hizmetleri dağıtım sunar. Azure’da Active Directory uygulamaya yönelik belirli öneriler için aşağıdaki makalelere bakın:

[Azure Active Directory etki alanı Hizmetleri (AD DS) genişletme](https://docs.microsoft.com/azure/guidance/guidance-identity-adds-extend-domain).

[Windows Server Active Directory, Azure sanal makinelerde dağıtmak için yönergeler](https://msdn.microsoft.com/library/azure/jj156090.aspx).

**Active Directory Tümleştirme**: ayrılmış bir AD DS mimarisi için alternatif olarak, müşteriler kullanmak isteyebilirsiniz [Azure Active Directory](https://docs.microsoft.com/azure/guidance/guidance-ra-identity#using-azure-active-directory) tümleştirme veya [Azure Active Directory'de bir şirket içi birleşik Orman](https://docs.microsoft.com/azure/guidance/guidance-ra-identity#using-active-directory-in-azure-joined-to-an-on-premises-forest).

### <a name="security"></a>Güvenlik

**Yönetim güvenliği**: Yöneticiler için Yönetim sanal ağ ve güvenilir bir kaynaktan RDP kullanarak Jumpbox bağlanmak bu şeması sağlar. Yönetim sanal ağ için ağ trafiğini Nsg'ler kullanılarak denetlenir. Trafik Jumpbox içeren alt ağ erişebilmeniz için güvenilir bir IP aralığından 3389 numaralı bağlantı noktasına erişim sınırlıdır.

Müşteriler de dikkate kullanarak bir [Gelişmiş Güvenlik yönetim modeli](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/securing-privileged-access) ortamı yönetim VNet ve Jumpbox bağlanırken güvenli hale getirmek için. Gelişmiş güvenlik için müşteriler kullanmanız önerilir bir [ayrıcalıklı erişim iş istasyonu](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/privileged-access-workstations#what-is-a-privileged-access-workstation-paw) ve RDGateway yapılandırma. Ağ sanal Gereçleri ve ortak/özel kullanımını DMZ'ler daha fazla güvenlik geliştirmeleri sunar.

**Ağ güvenliğini sağlama**: [ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (Nsg'ler), ikinci düzey yanlış yapılandırılmış veya devre dışı bırakılmış bir ağ geçidi atlayarak gelen trafiği karşı koruma sağlamak üzere her alt ağ için önerilir. Örnek - [bir NSG dağıtmak için Resource Manager şablonu](https://github.com/mspnp/template-building-blocks/tree/v1.0.0/templates/buildingBlocks/networkSecurityGroups).

**Ortak uç noktalarını güvenli hale getirme**: internet ağ geçidi uygulama hizmetlerini kullanıcılara Internet üzerinden kullanıma sunar. Bu hizmetlere erişme trafiği kullanarak güvenli bir [uygulama ağ geçidi](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction), bir Web uygulaması güvenlik duvarı ve HTTPS protokolü yönetimini sağlar.

**IP aralıklarını**: mimarisinde IP aralıkları: önerilen aralıkları. Müşterilerin kendi ortamı göz önünde bulundurun ve uygun aralıkların kullanmak için önerilir.

**Karma bağlantı**: bulut tabanlı iş yüklerini Azure VPN ağ geçidini kullanarak IPSec VPN üzerinden şirket içi veri merkezine bağlı. Müşteriler, uygun bir VPN ağ geçidi Azure'a bağlanmak için kullandığınız emin olmalısınız. Örnek - [VPN ağ geçidi Resource Manager şablonu](https://github.com/mspnp/template-building-blocks/tree/v1.0.0/templates/buildingBlocks/vpn-gateway-vpn-connection). Büyük ölçekli çalıştıran müşteriler, büyük veri gereksinimleri olan görev kritik iş yükleri bir karma ağ mimarisi kullanmayı düşünün isteyebilirsiniz [ExpressRoute](https://docs.microsoft.com/azure/guidance/guidance-hybrid-network-expressroute) Microsoft'a özel ağ bağlantısı için bulut hizmetlerini.

**Sorunları ayrılması**: sanal ağlar yönetim işlemleri ve iş işlemleri için bu başvuru mimarisinin ayırır. Ayrı sanal ağlar ve alt ağları izin trafiği yönetim, aşağıdaki ağ kesimleri arasında Nsg'leri kullanarak trafiği giriş ve çıkış kısıtlamaları da dahil olmak üzere [Microsoft bulut Hizmetleri ve ağ güvenliği](https://docs.microsoft.com/azure/best-practices-network-security) en iyi uygulamalar.

**Kaynak Yönetimi**: sanal makineleri, sanal ağlar ve yük Dengeleyiciler gibi Azure kaynakları birlikte halinde gruplayarak yönetilen [Azure kaynak gruplarını](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groupsresource). Yalnızca yetkili kullanıcıların erişimini kısıtlamak için kaynak tabanlı erişim denetimi rolleri sonra her kaynak grubuna atanabilir.

**Erişim denetimi kısıtlamaları**: kullanım [rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) (RBAC) kullanarak uygulamanızı kaynakları yönetmek için [özel roller](https://docs.microsoft.com/azure/role-based-access-control/custom-roles) RBAC işlemlerini kısıtlamak için kullanılabilir, DevOps her katmanında gerçekleştirebilirsiniz. İzin verme kullanırsanız [en az ayrıcalık prensibi](https://msdn.microsoft.com/library/hdb58b2f(v=vs.110).aspx#Anchor_1). Tüm yapılandırma değişikliklerinin planlı olduğundan emin olmak için yönetim işlemlerinin tümünü günlüğe kaydedin ve normal denetimler gerçekleştirin.

**Internet erişimi**: Bu başvuru mimarisinin utilises [Azure uygulama ağ geçidi](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) ağ geçidi ve yük dengeleyici Internet'e olarak. Bazı müşteriler, ayrıca güvenlik alternatif olarak ağ ek katmanlar için üçüncü taraf ağ sanal Gereçleri kullanma düşünebilirsiniz [Azure uygulama ağ geçidi](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction).

**Azure Güvenlik Merkezi**: [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) Abonelikteki kaynakların güvenlik durumu merkezi bir görünümünü sağlar ve tehlike giren kaynaklarını önlemeye yardımcı öneriler sağlar. Ayrıca, daha ayrıntılı ilkelerini etkinleştirmek için de kullanılabilir. Örneğin, kuruluşun kendi duruş riske uyarlamak olanak tanır belirli kaynak gruplarına ilkeleri uygulanabilir. Müşterilerin Azure aboneliklerinin Azure Güvenlik Merkezi'nde etkinleştirmeniz önerilir.

## <a name="ncsc-cloud-security-principles-compliance-documentation"></a>NCSC bulut güvenlik ilkelerine uyum belgeleri

Dama yapma ticari hizmet (ticari ve satın alma etkinlik hükümeti tarafından geliştirmek için çalışır kurumu), teklifleri resmi düzeyinde kapsayan Microsoft kapsam Kurumsal bulut hizmetlerine G bulut v6 sınıflandırma yenilenir. Azure ve bulut G ayrıntılarını bulunabilir [Azure UK G-bulut güvenlik değerlendirme özeti](https://www.microsoft.com/en-us/trustcenter/compliance/uk-g-cloud).

Bu şeması NCSC belgelenen 14 bulut güvenlik ilkeleri hizalar [bulut güvenlik ilkeleri](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) İngiltere-resmi sınıflandırılmış iş yüklerini destekleyen bir ortam sağlamak için.

[Müşteri sorumluluk matrisi](https://aka.ms/blueprintuk-gcrm) (Excel çalışma kitabı) listeler tüm 14 bulut güvenlik ilkeleri ve ilke uygulaması sorumluluğu olup matrisi, her ilke (veya ilke Altbölüm), gösterir Microsoft, müşteri veya ikisi arasında paylaşılan.

[İlkesi uygulaması matris](https://aka.ms/ukwebappblueprintpim) (Excel çalışma kitabı) listeleri tüm 14 bulut güvenlik ilkeleri ve matris gösterir, her ilke (veya ilke Altbölüm) için tasarlanmış bir müşteri sorumluluğu müşteri Sorumlulukları şeması Otomasyon ilke ve uygulama ilkesi requirement(s) ile nasıl hizalandığını 2) bir açıklama 1) uyguluyorsa matris. Bu içerik da kullanılabilir [burada](https://github.com/Azure/uk-official-three-tier-webapp/blob/master/principles-overview.md).

Ayrıca, bulut denetim matris bulut sağlayıcıları hesaplanmasında müşterileri desteklemek için ve bulut hizmetlerine taşımadan önce yanıtlanması soru belirlemek için bulut güvenlik Alliance (CSA) yayımladı. Yanıt olarak, Microsoft Azure CSA anlaşma değerlendirme Initiative soru yanıtlanan ([CSA CAIQ](https://www.microsoft.com/en-us/TrustCenter/Compliance/CSA)), Microsoft önerilen ilkeler nasıl ele açıklar.

## <a name="deploy-the-solution"></a>Çözümü dağıtma

Dağıtım kullanıcılar bu şeması Otomasyon dağıtmak için kullanabilir iki yöntem vardır. İlk yöntem başvuru mimarisi dağıtmak için Azure portal ikinci yöntem utilises bir PowerShell betiğini kullanır. Ayrıntılı dağıtım yönergeleri kullanılabilir [burada](https://aka.ms/ukwebappblueprintrepo).

## <a name="disclaimer"></a>Bildirim

 - Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR AÇIK, ZIMNİ VEYA NİZAMİ BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-değil." URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bu belgede belirtilen bilgiler ve görüntüler bildirim yapılmadan değiştirilebilir. Bu belgeyi okuma müşterilerin kullanım riski size aittir.
 - Bu belge müşterilerle herhangi bir Microsoft ürünü veya çözümleri üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler kopyalayabilir ve bu belgeyi iç başvuru amacıyla kullanın.
 - Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.
 - Bu mimari müşterilerin belirli gereksinimlerine ayarlamak bir temel olarak hizmet için tasarlanmıştır ve olarak kullanılmamalıdır-bir üretim ortamında.
 - Bu belge, bir başvuru olarak geliştirilen ve tüm anlamına gelir, bir müşteri belirli uyumluluk gereksinimleri ve düzenlemelere karşılayabilecek tanımlamak için kullanılmamalıdır. Müşteriler kendi organizasyon onaylı müşteri uygulamaları üzerinde yasal Destek'ten arama.
