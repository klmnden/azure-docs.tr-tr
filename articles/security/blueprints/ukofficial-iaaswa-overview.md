---
title: Azure güvenlik ve uyumluluk planı - UK-OFFICIAL için üç katmanlı Iaas Web uygulaması
description: Azure güvenlik ve uyumluluk planı - UK-OFFICIAL için üç katmanlı Iaas Web uygulaması
services: security
author: jomolesk
ms.assetid: 9c32e836-0564-4906-9e15-f070d2707e63
ms.service: security
ms.topic: article
ms.date: 02/08/2018
ms.author: jomolesk
ms.openlocfilehash: 13ea2b68027c81bca7b43cef62cf7039aa0ea8dd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60609492"
---
# <a name="azure-security-and-compliance-blueprint---three-tier-iaas-web-application-for-uk-official"></a>Azure güvenlik ve uyumluluk planı - UK-OFFICIAL için üç katmanlı Iaas Web uygulaması

## <a name="overview"></a>Genel Bakış

 Bu makalede, Birleşik Krallık'ta resmi olarak sınıflandırılan çok sayıda iş yükü işlemek için uygun bir Microsoft Azure üç katmanlı web tabanlı mimari sunmak için rehberlik ve Otomasyon betikleri sağlar.

 Yaklaşan kod olarak altyapı, kümesini [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) şablonları dağıtma hizalar bir ortamda, Birleşik Krallık Ulusal siber Güvenlik Merkezi (NCSC için) 14 [bulut güvenliği prensipleri](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) Internet güvenliği (CIS) için merkezi [kritik güvenlik denetimleri](https://www.cisecurity.org/critical-controls.cfm).

 NCSC önerilir hizmeti güvenlik özelliklerini değerlendirmek ve Sorumluluk müşteri ve sağlayıcı arasında bölme anlamanıza yardımcı olmak için kendi bulut güvenliği prensipleri müşteriler tarafından kullanılabilir. Bilgi sorumlulukları bölme anlamanıza yardımcı olması için bu ilkelerin her biri karşı sağladık.

 Bu mimari, karşılık gelen Azure Resource Manager şablonları ve Microsoft teknik incelemeyi tarafından desteklenen [UK için 14 bulut güvenlik denetimleri kullanarak Microsoft Azure bulut](https://gallery.technet.microsoft.com/14-Cloud-Security-Controls-670292c1). Bu belgede Azure hizmetlerinin nasıl Kataloglarınızı UK NCSC 14 bulut güvenlik böylece Hızlı Gönderim yeteneklerini bulut tabanlı hizmetler Microsoft Azure genel ve Birleşik Krallık'ta kullanarak kendi uyumluluk sorumlulukları karşılamak kuruluşların Etkinleştirme ilkeleri ile Hizala bulut.

 Bu şablon, iş yükü için altyapıyı dağıtır. Uygulama kodu ve iş katmanı ve veri katmanı yazılım destekleyen yüklenmiş ve yapılandırılmış olması gerekir. Ayrıntılı dağıtım yönergeleri [burada](https://aka.ms/ukwebappblueprintrepo).

 Hızla ve kolayca - kaydolabilirsiniz sonra bir Azure aboneliğiniz yoksa [Azure ile çalışmaya başlama](https://azure.microsoft.com/get-started/).

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri

 Azure şablonları, bir üç katmanlı web uygulaması mimarisi UK resmi iş yüklerini destekleyen bir Azure bulut ortamında sunun. Mimari, kurumsal kullanıcılar tarafından veya internet'ten güvenli bir şekilde erişilmesini Azure olanağı tanıyan web tabanlı iş yükleri bir şirket içi ağa genişleten bir güvenli hibrit ortamı sunar.

![Üç katmanlı Iaas Web uygulaması başvuru mimarisi diyagramı UK resmi için](images/ukofficial-iaaswa-architecture.png?raw=true "UK resmi başvurusu mimari şeması için üç katmanlı Iaas Web uygulaması")

 Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Dağıtım mimarisi ayrıntılarını yerleştirilir [dağıtım mimarisi](#deployment-architecture) bölümü.

((1) /16 sanal ağ - işletimsel VNet
- (3) /24 alt - 3 katmanlı (Web, Biz, veriler)
- (1) en az/27 alt ağ - EKLER
- ((1) en az/27 alt ağ - ağ geçidi alt ağı
- ((1) /29 alt ağ - uygulama ağ geçidi alt ağı
- Kullanır (Azure tarafından sağlanan) DNS varsayılan
- Etkin Yönetim sanal ağa eşleme
- Trafik akışını yönetmek için ağ güvenlik grubu (NSG)

((1) /24 sanal ağ - sanal yönetim ağı
- ((1) en az/27 alt ağ
- (2) DNS EKLER ve (1) Azure DNS girişlerini kullanır
- İşletimsel Vnet'e etkin eşleme
- Trafik akışını yönetmek için ağ güvenlik grubu (NSG)

(1) uygulama ağ geçidi
- WAF etkinken-
- WAF modu - önleme
- Kural kümesi: OWASP 3.0
- Bağlantı noktası 80 üzerinde HTTP dinleyicisi
- Bağlantı/trafik NSG'de düzenlemesi
- Genel IP adresi uç noktası (Azure) tanımlanan

(1) VPN - rota tabanlı 2 siteden siteye IPSec VPN tüneli
- Genel IP adresi uç noktası (Azure) tanımlanan
- Bağlantı/trafik NSG'de düzenlemesi
- (1) yerel ağ geçidi (şirket içi uç noktası)
- (1) azure ağ geçidi (Azure uç noktası)

(9) Azure Iaas kötü amaçlı yazılımdan koruma DSC ayarlarla dağıtılan sanal makineler - tüm sanal makineler

- (2) active Directory etki alanı Hizmetleri etki alanı denetleyicileri (Windows Server 2012 R2)
  - (2) DNS sunucu rollerini - VM başına 1
  - (2) işletimsel VNet - VM başına 1 bağlı NIC'ler
  - Her ikisi de şablonda tanımlanan etki alanına katılır
    - Dağıtımın bir parçası olarak oluşturulan etki alanı


- (1) Sıçrama kutusu (Kale ana bilgisayarı) yönetim VM
  - Genel IP adresiyle yönetim VNet üzerinde 1 NIC
    - NSG trafiği (daraltma/genişletme) belirli kaynaklara sınırlamak için kullanılır
  - Etki alanına katılmış


- (2) web katmanı VM'ler
  - (2) IIS sunucu rollerini - VM başına 1
  - (2) işletimsel VNet - VM başına 1 bağlı NIC'ler
  - Etki alanına katılmış


- (2) biz katmanı Vm'leri
  - (2) işletimsel VNet - VM başına 1 bağlı NIC'ler
  - Etki alanına katılmış


- (2) veri katmanı Vm'leri
  - (2) işletimsel VNet - VM başına 1 bağlı NIC'ler
  - Etki alanına katılmış

Kullanılabilirlik Kümeleri
- (1) active Directory etki alanı denetleyicisi VM'SİNİN set - 2 VM
- (1) web katmanı VM set - 2 VM
- (1) biz katman sanal makine set - 2 VM
- (1) veri katmanı VM set - 2 VM

Load Balancer
- (1) web katmanı yük dengeleyiciye
- (1) biz katmanı yük dengeleyiciye
- (1) yük dengeleyici veri katmanı

Depolama
- (14) toplam depolama hesapları
  - Active Directory etki alanı denetleyicisi kullanılabilirlik kümesi
    - (2) her VM için 1 - birincil yerel olarak yedekli depolama (LRS) hesapları  
    - (1) yerel olarak yedekli depolama (LRS) hesabı EKLER kullanılabilirlik kümesi için tanılama
  - Yönetim Sıçrama kutusu VM
    - (1) birincil yerel olarak yedekli depolama (LRS) hesabı için Sıçrama kutusu VM
    - (1) yerel olarak yedekli depolama (LRS) hesabı Sıçrama kutusu sanal makine için tanılama
  - Web Katmanı VM'ler
    - (2) her VM için 1 - birincil yerel olarak yedekli depolama (LRS) hesapları  
    - (1) yerel olarak yedekli depolama (LRS) hesabı Web Katmanı kullanılabilirlik kümesi için tanılama
  - Biz katmanı Vm'leri
    - (2) her VM için 1 - birincil yerel olarak yedekli depolama (LRS) hesapları  
    - (1) yerel olarak yedekli depolama (LRS) hesabı Biz katmanı kullanılabilirlik kümesi için tanılama
  - Veri katmanı Vm'leri
    - (2) her VM için 1 - birincil yerel olarak yedekli depolama (LRS) hesapları  
    - (1) yerel olarak yedekli depolama (LRS) hesabı veri katmanı kullanılabilirlik kümesi için tanılama

### <a name="deployment-architecture"></a>Dağıtım mimarisi:

**Şirket içi ağ**: Kuruluşta uygulanan bir özel yerel ağ.

**Üretim VNet**: Üretim [VNet](https://docs.microsoft.com/azure/Virtual-Network/virtual-networks-overview) uygulamayı ve Azure'da çalışan diğer işlem kaynakları (sanal ağ için) barındırır. Her sanal ağ yalıtma ve ağ trafiğini yönetmek için kullanılan çeşitli alt ağlar içeriyor olabilir.

**Web Katmanı**: Gelen HTTP isteklerini işler. Bu katman ile yanıt döndürülür.

**İş katmanı**: Implements iş süreçleri ve diğer sistem işlevsel mantığı.

**Veritabanı katmanı**: Kalıcı veri depolama sağlar kullanarak [SQL Server Always On kullanılabilirlik grupları](https://msdn.microsoft.com/library/hh510230.aspx) yüksek kullanılabilirlik için. Müşterileri kullanabilir [Azure SQL veritabanı](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview) bir PaaS alternatif olarak.

**Ağ geçidi**: [VPN ağ geçidi](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) şirket içi ağ yönlendiricileri ve üretim sanal ağ arasında bağlantı sağlar.

**Internet Ağ ve genel IP adresi**: İnternet ağ geçidi, internet üzerinden kullanıcılara uygulama hizmetleri sunar. Bu hizmetlere erişen trafiği kullanarak güvenli bir [Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) 7. Katman Yönlendirme ve Yük Dengeleme ile web uygulaması Güvenlik Duvarı (WAF) koruma özellikleri sunar.

**Yönetim sanal ağ**: Bu [VNet](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) yönetim ve izleme işlevlerini VNet üretimde çalışan iş yükleri için uygulama kaynakları içerir.

**Sıçrama kutusu**: Olarak da adlandırılan bir [Burcu ana bilgisayarı](https://en.wikipedia.org/wiki/Bastion_host), ağ üzerinde yer alan yöneticilerin VNet üretimde Vm'lerine bağlanmak için kullandıkları güvenli bir VM olduğu. Sıçrama kutusunun sadece güvenli bir listede yer alan genel IP adreslerinden gelen uzak trafiğe izin veren bir NSG’si vardır. Uzak Masaüstü (RDP) trafiğine izin vermek için trafik kaynağını NSG'de tanımlanması gerekir. Üretim kaynak yönetimi, güvenli bir Sıçrama kutusu VM kullanarak RDP aracılığıyla değildir.

**Kullanıcı tanımlı yollar**: [Kullanıcı tanımlı yollar](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) Azure sanal ağları içindeki IP trafiğinin akışını tanımlamak için kullanılır.

**Eşlenmiş sanal ağlarda ağ**: Kullanarak üretim ve Yönetim sanal ağlara bağlı [VNet eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).
Bu sanal ağ ayrı kaynaklar olarak yönetilmeye devam eder, ancak bu sanal makineler için tüm bağlantılarda tek olarak görünür. Bu ağlar birbiriyle doğrudan özel IP adresleri kullanarak iletişim kurar. VNet eşlemesi aynı Azure bölgesinde olan Vnet'ler tabidir.

**Ağ güvenlik grupları**: [Nsg'ler](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) erişim denetimi izin veren veya reddeden trafiği bir sanal ağ içindeki listelerini içerir. Nsg bir alt ağ veya tek tek VM düzeyinde trafiği güvenli hale getirmek için kullanılabilir.

**Active Directory etki alanı Hizmetleri (AD DS)**: Bu ayrılmış bir mimarisi [Active Directory Domain Services](https://technet.microsoft.com/library/hh831484.aspx) dağıtım.

**Günlüğe kaydetme ve Denetim**: [Azure etkinlik günlüğü](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) işlemi gerçekleştiğinde işlem başlatan gibi aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen yakalamaları işlemleri, işlemin durumunu ve yardımcı olabilecek diğer özelliklerin değerlerine araştırma işlem. Azure etkinlik günlüğü bir Abonelikteki tüm eylemleri yakalayan bir Azure platform hizmetidir. Günlükleri, arşivlenen veya gerekirse verilebilir.

**İzleme ve uyarı ağ**: [Azure Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) ağ traffics içinde sanal ağlarınız için bir platform hizmeti sağlamaktadır ağ paket yakalama, akış günlüğe kaydetme, topoloji araçları ve tanılama.

## <a name="guidance-and-recommendations"></a>Yönerge ve öneriler

### <a name="business-continuity"></a>İş Sürekliliği

**Yüksek kullanılabilirlik**: Sunucu iş yükleri gruplanır bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) azure'daki sanal makinelerin yüksek kullanılabilirlik sağlamak için. Bu yapılandırma planlı veya Plansız bakım olayı sırasında en az bir sanal makine kullanılabilir ve % 99,95 oranında karşılamak emin olun yardımcı olur. Azure SLA'sı.

### <a name="logging-and-audit"></a>Günlüğe kaydetme ve Denetim

**İzleme**: [Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started) etkinlik günlüğü, ölçümleri ve tanılama günlüklerini tüm Azure kaynaklarınızın izlemek için tek bir kaynak sağlayan platform hizmetidir. Azure İzleyici görselleştirin, sorgu, yönlendirme, arşiv ve azure'daki kaynaklardan gelen günlükler ve ölçümler üzerinde işlem için yapılandırılabilir. Kaynak tabanlı erişim denetimi, denetim izi'ni kullanıcılar günlükleri değiştirme becerisi olmadığından emin olun yardımcı olmak için güvenli hale getirmek için kullandığınız önerilir.

**Etkinlik günlükleri**: Yapılandırma [Azure etkinlik günlüklerini](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) , aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlamak için.

**Tanılama günlükleri**: [Tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) olan bir kaynak tarafından bir günlüklerdir. Bu günlükler, Windows olayı sistem günlükleri, blob, tablo ve kuyruk günlükleri dahil olabilir.

**Güvenlik duvarı günlükleri**: Application Gateway tam tanılama ve erişim günlükleri sağlar. Güvenlik duvarı günlükleri, WAF’nin etkin olduğu application gateway kaynakları için kullanılabilir.

**Günlük arşivleme**: Günlük veri depolama, arşivleme ve tanımlanan saklama süresi için merkezi bir Azure depolama hesabına yazma için yapılandırılabilir. Azure İzleyici günlüklerine kullanarak günlükleri işlenebilir veya üçüncü taraf SIEM sistemleri tarafından.

### <a name="identity"></a>Kimlik

**Active Directory etki alanı Hizmetleri**: Bu mimari, Azure'da bir Active Directory Domain Services Dağıtımı sunar. Azure’da Active Directory uygulamaya yönelik belirli öneriler için aşağıdaki makalelere bakın:

[Active Directory etki alanı Hizmetleri (AD DS) Azure'a genişletme](https://docs.microsoft.com/azure/guidance/guidance-identity-adds-extend-domain).

[Azure sanal makineler üzerinde Windows Server Active Directory dağıtma ilkeleri](https://msdn.microsoft.com/library/azure/jj156090.aspx).

**Active Directory Tümleştirmesi**: Ayrılmış bir AD DS mimarisi için alternatif olarak, müşteriler kullanmak isteyebileceğiniz [Azure Active Directory](https://docs.microsoft.com/azure/guidance/guidance-ra-identity) tümleştirme veya [Azure Active Directory'ye katılmış bir şirket içi ormana](https://docs.microsoft.com/azure/guidance/guidance-ra-identity).

### <a name="security"></a>Güvenlik

**Yönetim güvenliği**: Yöneticiler için Yönetim sanal ağ ve Sıçrama kutusuna gelen RDP güvenilir bir kaynaktan kullanarak bağlanmak bu şema sağlar. Nsg'leri kullanarak ağ trafiğini sanal ağ yönetimi için denetlenir. Trafiği Sıçrama kutusu içeren alt ağın erişebilmeniz için güvenilir bir IP aralığından 3389 numaralı bağlantı noktasına erişim kısıtlanır.

Müşteriler de göz önünde bulundurmanız kullanarak bir [Gelişmiş Güvenlik yönetim modeli](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/securing-privileged-access) Yönetim sanal ağ ve Sıçrama kutusu bağlanırken ortam güvenliğini sağlamak için. Gelişmiş güvenlik için müşterilerin kullanmak önerilen bir [ayrıcalıklı erişim iş istasyonu](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/privileged-access-workstations#what-is-a-privileged-access-workstation-paw) ve RDGateway yapılandırma. Ağ sanal Gereçleri ve ortak/özel DMZ'ler daha fazla güvenlik geliştirmeleri sunar.

**Ağ güvenliğini sağlama**: [Ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (Nsg'ler), ikinci bir yanlış yapılandırılmış veya devre dışı ağ geçidi atlayan gelen trafiğe karşı bir koruma düzeyi sağlamak her alt ağ için önerilir. Örnek - [bir NSG dağıtmak için Resource Manager şablonu](https://github.com/mspnp/template-building-blocks/tree/v1.0.0/templates/buildingBlocks/networkSecurityGroups).

**Açık uç noktalarını güvenli hale getirme**: İnternet ağ geçidi, internet üzerinden kullanıcılara uygulama hizmetleri sunar. Bu hizmetlere erişen trafiği kullanarak güvenli bir [Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction), bir Web uygulaması güvenlik duvarı ve HTTPS protokolü yönetim sağlar.

**IP aralıklarını**: IP aralıklarını mimaride önerilen aralıktır. Müşteriler, kendi ortamlarından göz önünde bulundurun ve uygun aralıkların kullanmak için önerilir.

**Karma bağlantı**: Bulut tabanlı iş yükleri, Azure VPN ağ geçidini kullanarak IPSec VPN aracılığıyla şirket içi veri merkezine bağlıdır. Müşteriler, uygun bir VPN ağ geçidi Azure'a bağlanmak için kullandığınız emin olmalısınız. Örnek - [VPN ağ geçidi Resource Manager şablonu](https://github.com/mspnp/template-building-blocks/tree/v1.0.0/templates/buildingBlocks/vpn-gateway-vpn-connection). Büyük ölçekli çalıştıran müşteriler, görev açısından kritik iş yükleri büyük veri gereksinimleri olan bir hibrit ağ mimarisi kullanmayı göz önünde hazırlandığını [ExpressRoute](https://docs.microsoft.com/azure/guidance/guidance-hybrid-network-expressroute) Microsoft'a özel ağ bağlantısı için bulut Hizmetleri.

**Görev ayrımı nettir**: Bu başvuru mimarisi, sanal ağlar yönetim işlemleri ve işletme işlemleri için ayırır. Ayrı sanal ağlar ve alt ağlar arasında aşağıdaki ağ kesimlerini Nsg'leri kullanarak trafiği giriş ve çıkış kısıtlamaları da dahil olmak üzere, trafik yönetimi izin [Microsoft bulut Hizmetleri ve ağ güvenliği](https://docs.microsoft.com/azure/best-practices-network-security) en iyi uygulamalar.

**Kaynak Yönetimi**: Vm'leri, sanal ağlar ve yük Dengeleyiciler gibi Azure kaynaklarını birbirine halinde gruplayarak yönetilen [Azure kaynak grupları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). Kaynak tabanlı erişim denetimi rolleri, ardından yalnızca yetkili kullanıcıların erişimini kısıtlamak için her bir kaynak grubuna atanabilir.

**Erişim denetimi kısıtlamalarını**: Kullanım [rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) (RBAC) kullanarak uygulama kaynaklarınızı yönetmek için [özel roller](https://docs.microsoft.com/azure/role-based-access-control/custom-roles) RBAC, DevOps, her katmanda gerçekleştirebileceği işlemleri kısıtlamak için kullanılabilir. İzin verirken kullanın [en az ayrıcalık ilkesini](https://msdn.microsoft.com/library/hdb58b2f(v=vs.110).aspx#Anchor_1). Tüm yapılandırma değişikliklerinin planlı olduğundan emin olmak için yönetim işlemlerinin tümünü günlüğe kaydedin ve normal denetimler gerçekleştirin.

**Internet erişimi**: Bu başvuru mimarisi kullanan [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) internet'e yönelik ağ geçidi ve yük dengeleyici olarak. Alternatif olarak güvenlik ağ ek katmanı için üçüncü taraf ağ sanal Gereçleri kullanarak bazı müşteriler de düşünebilirsiniz [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction).

**Azure Güvenlik Merkezi**: [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) Abonelikteki kaynakların güvenlik durumu merkezi bir görünümünü sağlar ve tehlike giren kaynakları önlemeye yardımcı öneriler sağlar. Ayrıca, daha ayrıntılı ilkelerini etkinleştirmek için de kullanılabilir. Örneğin, kendi duruşunu risk için uygun hale getirmek Kurumsal sağlayan belirli kaynak gruplarına ilkeler uygulanabilir. Müşteriler Azure Güvenlik Merkezi Azure aboneliklerinde etkinleştirmenizi öneririz.

## <a name="ncsc-cloud-security-principles-compliance-documentation"></a>NCSC bulut güvenlik ilkelerinin uyumluluk belgeleri

Dama yapma ticari hizmeti (ticari ve tedarik etkinliği tarafından kamu artırmak için çalışan Aracısı) resmi düzeyinde tüm teklifleri kapsayan Microsoft kapsamındaki Kurumsal bulut hizmetlerine G-Cloud v6 sınıflandırmasını yenilendi. Azure G-Cloud ve ayrıntıları bulunabilir [Azure UK G-Cloud güvenlik değerlendirme özeti](https://www.microsoft.com/en-us/trustcenter/compliance/uk-g-cloud).

Bu şema içinde NCSC belgelenen 14 bulut güvenliği prensipleri hizalar [bulut güvenliği prensipleri](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) UK-OFFICIAL sınıflandırılan iş yüklerini destekleyen bir ortam sağlamak için.

[Müşteri sorumluluk matris](https://aka.ms/ukofficial-crm) (Excel çalışma kitabı) tüm 14 bulut güvenliği prensipleri listeler ve ilke uygulama sorumluluğu olup matrisi, her ilke (veya ilke Altbölüm), gösterir Microsoft, müşteri veya ikisi arasında paylaşılan.

[İlkesi uygulama matris](https://aka.ms/ukofficial-iaaswa-pim) tüm 14 bulut güvenlik ilkeleri ve matris (Excel çalışma kitabı) listesi gösterir, her ilke (veya ilke Altbölüm) için tasarlanmış bir müşteri sorumluluğu müşteri Sorumlulukları 1) ilke ve uygulama ile İlkesi gereksinimlerin nasıl hizalandığını 2) bir açıklama şema Otomasyon kullanılıyorsa, matris.

Ayrıca, bulut güvenliği İttifakı (CSA) bulut denetim matrisi bulut sağlayıcılarının veriyi değerlendirmede müşterileri desteklemek ve bulut Hizmetleri taşımadan önce yanıtlanması soru tanımlamak için yayımladı. Yanıt olarak, Microsoft Azure CSA fikir birliğine varılmış değerlendirme girişimi anketi yanıtlanmış ([CSA CAIQ](https://www.microsoft.com/en-us/TrustCenter/Compliance/CSA)), Microsoft önerilen ilkeler nasıl ele açıklar.

## <a name="deploy-the-solution"></a>Çözümü dağıtma

Bu şema Otomasyon dağıtmak için dağıtım kullanıcılar kullanabilir iki yöntem vardır. İlk yöntem ikinci yöntem başvuru mimarisini dağıtmak için Azure portalını kullanan bir PowerShell Betiği kullanır. Ayrıntılı dağıtım yönergeleri [burada](https://aka.ms/ukofficial-iaaswa-repo).

## <a name="disclaimer"></a>Bildirim

 - Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR EXPRESS, ZIMNİ VEYA YASAL BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-olduğundan." Bilgi ve URL ve diğer Internet Web sitesi referansları da dahil olmak üzere bu belgede, bildirilmeksizin değiştirilebilir. Müşteriler bu belgeyi okuma KULLANIMLARDAN doğacak riskler size aittir.
 - Bu belge, müşterilerle herhangi bir Microsoft ürünü veya çözümler üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler kopyalayabilir ve dahili başvuru amacıyla bu belgeyi kullanın.
 - Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.
 - Bu mimari, müşterilerin kendi belirli gereksinimlerine ayarlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-üretim ortamıdır.
 - Bu belge, bir başvuru olarak geliştirilir ve tüm anlamına gelir, bir müşteri özel uyumluluk gereksinimlerini ve düzenlemeleri karşılayabilecek tanımlamak için kullanılmamalıdır. Müşterilerin onaylı müşteri uygulamaları kuruluşları yasal Destek'ten arama.
