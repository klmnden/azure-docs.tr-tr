---
title: Azure güvenlik ve uyumluluk planı - Iaas Web uygulaması için Avustralya korumalı
description: Azure güvenlik ve uyumluluk planı - Iaas Web uygulaması için Avustralya korumalı
services: security
author: meladie
ms.assetid: f53a25c4-1c75-42d6-a0e7-a91661673891
ms.service: security
ms.topic: article
ms.date: 08/23/2018
ms.author: meladie
ms.openlocfilehash: 3c82a88ea15b52672f9bed428e2e7af40a65309c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60610183"
---
# <a name="azure-security-and-compliance-blueprint---iaas-web-application-for-australia-protected"></a>Azure güvenlik ve uyumluluk planı - Iaas Web uygulaması için korumalı Avustralya

## <a name="overview"></a>Genel Bakış
Azure güvenlik ve uyumluluk planı koleksiyonu, depolanması ve alınması hedefleri ile uyumlu olan kamu AU korumalı veriler için uygun bir hizmet (Iaas) ortamı olarak bir altyapı dağıtımı için yönergeler sağlar Avustralya devlet bilgi güvenliği el ile (Avustralya sinyal Directorate (ASD) tarafından üretilen ISM). Bu şema genel bir başvuru mimarisi gösteren ve güvenli, uyumlu, çok katmanlı bir ortamda hassas devlet kurumları veri doğru işlenmesi yardımcı gösterir.

Bu başvuru mimarisi, Uygulama Kılavuzu ve tehdit modeli müşterilerin ASD uyumlu bir şekilde Azure'a iş yüklerini dağıtmaya yardımcı müşterilerin kendilerine ait planlanması ve sistem akreditasyon süreçleri gerçekleştirmek bir temel sağlar. Müşteriler bir Azure VPN ağ geçidi veya ExpressRoute Federasyon Hizmetleri kullanın ve şirket içi kaynaklarınızı Azure kaynakları ile tümleştirme için uygulamayı seçebilir. Müşteriler, şirket içi kaynakları kullanarak güvenlik etkilerini dikkate almanız gerekir. Ek yapılandırma gereksinimlerini karşılamak için gerekli aynıdır her bir müşterinin uygulama ayrıntılarına göre değişiklik gösterebilir.

ASD uyumluluk elde kayıtlı bir bilgi güvenlik denetçisi sistem denetimleri gerektirir. Müşteriler, uygun güvenlik yürütmek için sorumludur ve her bir müşterinin uygulama ayrıntılarına bağlı uyumluluk değerlendirme gereksinimleri değişebilir, bu mimariyi kullanarak oluşturulan bir çözümün temel.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri
Bu çözüm, bir başvuru mimarisi için SQL Server arka ucuna sahip bir Iaas web uygulaması dağıtır. Bir web katmanı, veri katmanı, Active Directory altyapı, uygulama ağ geçidi ve yük dengeleyici mimarisi içerir. Web ve veri katmanları için dağıtılan sanal makinelerin bir kullanılabilirlik kümesine yapılandırılır ve SQL Server örneklerinin bir Always On kullanılabilirlik grubu yüksek kullanılabilirlik için yapılandırılır. Etki alanına katılmış sanal makineleri ve Active Directory grup ilkeleri işletim sistemi düzeyinde güvenlik ve uyumluluk yapılandırmaları uygulamak için kullanılır. Yönetim Burcu ana bilgisayarı, yöneticilerin dağıtılan kaynaklara güvenli bir bağlantı sağlar.

Mimari, kuruluşun özel yerel ağ veya internet'ten şirket kullanıcıları tarafından güvenli bir şekilde erişilmek üzere web tabanlı iş yüklerini izin verme bir şirket içi ağı Azure'a genişleten güvenli bir hibrit ortam sunabilir. Şirket içi çözümler için müşteri sorumlu ve güvenlik, işlemlerini ve uyumluluk tüm boyutlarından sorumlu.

Bu çözümde yer alan Azure kaynaklarını bir şirket içi ağ veya datacentre ortak yerleşim tesisinizden (örneğin Kanbera, CDC) Azure VPN ağ geçidini kullanarak IPSec VPN veya ExpressRoute aracılığıyla bağlanabilirsiniz... Bir VPN kullanmaya karar iletilen verileri ve göz önünde ağ yolunda sınıflandırmada yapılmalıdır. Büyük veri gereksinimleri olan görev açısından kritik iş yükleri, büyük ölçekli çalıştıran müşteriler, Azure hizmetlerine özel ağ bağlantısı için ExpressRoute kullanarak hibrit ağ mimarisi düşünmelisiniz. Azure'a bağlantı mekanizmaları hakkında daha ayrıntılı bilgi için yönerge ve öneriler bölümüne bakın.

Azure Active Directory ile Federasyon, kullanıcıların şirket içi kimlik bilgilerini kullanarak kimlik doğrulaması yapmak ve bir şirket içi Active Directory Federasyon Hizmetleri altyapısını kullanarak buluttaki tüm kaynaklara erişim sağlamak için kullanılmalıdır. Basitleştirilmiş, güvenli Kimlik Federasyonu ve web çoklu oturum açma özellikleri bu hibrit ortamı için Active Directory Federasyon Hizmetleri sağlar. Azure Active Directory Kurulum daha fazla ayrıntı için yönerge ve öneriler bölümüne bakın.

Çözüm, müşterilerin bekleyen verilerin gizliliğini depolama hizmeti Şifrelemesi'ni kullanmak için yapılandırabilirsiniz Azure depolama hesapları kullanır. Azure dayanıklılık için Müşteri'nin seçili bölgede verilerin üç kopyasını depolar. Azure bölgeleri dayanıklı bölge çiftlerinde dağıtılır ve veriler üç kopya ile ikinci bölgeye çoğaltılır, coğrafi olarak yedekli depolama sağlar. Bu, müşterinin birincil veri konumda veri kaybıyla sonuçlanan olumsuz bir olay engeller.

Gelişmiş güvenlik için bu çözümdeki tüm kaynaklar bir kaynak grubuyla Azure Resource Manager aracılığıyla yönetilir. Azure Active Directory rol tabanlı erişim denetimi erişimi denetlemek için kullanılan dağıtılan kaynakları ve Azure Key vault'taki anahtarları. Sistem durumu, Azure Güvenlik Merkezi ve Azure İzleyici izlenir. Müşteriler, günlükleri tutmak ve sistem durumu bir kolayca gezinebilir, tek bir Panoda görüntülemek için her iki izleme hizmetleri yapılandırın. Azure Application Gateway önleme modunda bir güvenlik duvarı olarak yapılandırılır ve TLS 1.2 ve üstünü trafiğiyle gerektirir.

![Iaas Web uygulaması AU korumalı başvuru mimarisi için](images/au-protected-iaaswa-architecture.png?raw=true "Iaas Web uygulaması için başvuru AU korumalı mimarisi diyagramı") 

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Diğer ayrıntılar için bkz [dağıtım mimarisi](#deployment-architecture) bölümü.

- Kullanılabilirlik Kümeleri
    - (1) SQL küme düğümleri
    - (1) web/IIS
- Azure Active Directory
- Azure Application Gateway
    - (1) web uygulaması güvenlik duvarı
        - Güvenlik Duvarı modu: önleme
        - Kural kümesi: OWASP 3.0
        - Dinleyici bağlantı noktası: 443
- Azure bulut tanığı
- Azure Key Vault
- Azure Load Balancer
- Azure İzleyici
- Azure Resource Manager
- Azure Güvenlik Merkezi
- Azure İzleyici günlükleri
- Azure Storage
- Azure Sanal Makineler
    - (1) yönetim/savunma (Windows Server 2016 Datacenter)
    - (2) SQL Server kümesi düğümünün (Windows Server 2016 üzerinde SQL Server 2017)
    - (2) web/IIS (Windows Server 2016 Datacenter)
- Azure Sanal Ağ
    - (4) ağ güvenlik grupları
    - Azure Ağ İzleyicisi
- Kurtarma Hizmetleri kasası

Bu şema kullanılmak üzere korumalı sınıflandırma Avustralya siber Güvenlik Merkezi (ACSC) tarafından onaylanmış değil Azure hizmetleri içerir. Bu başvuru mimarisinde dahil tüm hizmetleri dağıtımını sınırlandırma işaretçileri (DLM) düzeyinde ACSC tarafından onaylanmış. Microsoft, müşterilerin gözden geçirin, yayımlanan güvenlik ve Denetim raporları, bu Azure Hizmetleri ile ilgili ve Azure hizmet kendi iç akreditasyonu ve kullanım için uygun olup olmadığını belirlemek için kendi risk yönetim çerçevesi kullanın önerir Korumalı sınıflandırması.

## <a name="deployment-architecture"></a>Dağıtım mimarisi
Aşağıdaki bölümde dağıtım ve uygulama öğeleri ayrıntılı olarak açıklanmaktadır.

**Kale ana bilgisayarı**: Kale ana bilgisayarı tek kullanıcılara bu ortama dağıtılan kaynaklara erişmek giriş noktasıdır. Kale ana bilgisayarı, genel IP adreslerinden gelen uzak trafiğine yalnızca güvenli bir listede vererek dağıtılan kaynaklara güvenli bir bağlantı sağlar. Uzak Masaüstü (RDP) trafiğine izin vermek için trafik kaynağını ağ güvenlik grubu listesinde tanımlanması gerekir.

Bu çözüm aşağıdaki yapılandırmaları olan bir etki alanına katılmış Burcu ana bilgisayarı olarak bir sanal makine oluşturur:
-   [Kötü amaçlı yazılımdan koruma uzantısı](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [Azure tanılama uzantısı](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) Azure Key Vault kullanma
-   Bir [otomatik kapatma ilke](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) kullanımda olmadığında sanal makine kaynakların tüketimini azaltmak için

### <a name="virtual-network"></a>Sanal ağ
10.200.0.0/16 bir adres alanı ile özel bir sanal ağ mimarisini tanımlar.

**Ağ güvenlik grupları**: Bu çözüm, kaynakları ayrı web alt ağı, veritabanı alt ağı, Active Directory alt ve bir sanal ağ içinde yönetim alt ağı ile bir mimari dağıtır. Alt ağlar için yalnızca bu gerekli system ve yönetim işlevselliği için alt ağlar arasındaki trafiği kısıtlamak için ayrı alt ağlara uygulanan ağ güvenlik grubu kuralları tarafından mantıksal olarak ayrılır.

Yapılandırma için bkz: [ağ güvenlik grupları](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json) ile bu çözümü dağıtıldı. Kuruluşlar, ağ güvenlik grupları kullanarak yukarıda dosyasını düzenleyerek yapılandırabilirsiniz [bu belgeleri](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) bir kılavuz olarak.

Alt ağlar, adanmış ağ güvenlik grubu vardır:
- 1 ağ güvenlik grubu için uygulama ağ geçidi (LBNSG)
- Kale ana bilgisayarı (MGTNSG) için 1 ağ güvenlik grubu
- SQL sunucuları ve bulut tanığı (SQLNSG) için 1 ağ güvenlik grubu
- web Katmanı (WEBNSG) için 1 ağ güvenlik grubu

### <a name="data-in-transit"></a>Aktarımdaki verileri
Azure, Azure veri merkezlerinden tüm iletişimi varsayılan olarak şifreler. 

Ağları ait müşteriden Aktarımdaki korunan veriler için IPSec ile yapılandırılmış bir VPN ağ geçidi ile Internet veya ExpressRoute mimarisi kullanır.

Ayrıca, Azure Yönetim Portalı aracılığıyla azure'a tüm işlemleri TLS 1.2 kullanarak HTTPS gerçekleştirilir.

### <a name="data-at-rest"></a>Bekleyen veriler
Mimarisi, bekleyen veri şifrelemesi, Denetim veritabanı ve diğer ölçüler verilerinizi korumanızı sağlar.

**Azure depolama**: Şifrelenmiş veri rest gereksinimleri karşılamak için tüm [Azure depolama](https://azure.microsoft.com/services/storage/) kullanan [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption). Bu, kuruluş güvenlik taahhütlerine ve Avustralya hükümeti ISM tarafından tanımlanan uyumluluk gereksinimlerini desteklemek üzere verileri koruyarak yardımcı olur.

**Azure Disk şifrelemesi**: [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) veri diskleri için birim şifrelemesi sağlamak için Windows BitLocker özelliğidir yararlanır. Çözüm denetlemenize ve disk şifreleme anahtarlarını yönetmek için Azure anahtar kasası ile tümleştirilir.

**SQL Server**: SQL Server örneğini aşağıdaki veritabanı güvenlik önlemlerini kullanır:
-   [SQL Server Denetim](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine?view=sql-server-2017) veritabanı olaylarını izler ve Denetim günlükleri için yazar.
-   [Saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) gerçek zamanlı şifreleme ve şifre çözme veritabanı, ilişkili yedeklemeler ve işlem günlük dosyaları bekleyen bilgileri korumak için gerçekleştirir. Veriler güvencesi yetkisiz erişim ayarlanmamış saydam veri şifrelemesi sağlar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) doğru izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [Şifrelenmiş sütunlar](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-wizard?view=sql-server-2017) hassas verileri hiçbir zaman içinde bir veritabanı sistemi düz metin olarak göründüğünden emin olun. Veri şifrelemesi etkinleştirildikten sonra yalnızca istemci uygulamaları veya uygulama sunucuları anahtarlarına erişimi ile düz metin verilere erişebilir.
- [Dinamik veri maskeleme](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking?view=sql-server-2017) tarafından ayrıcalıksız kullanıcılar veya uygulamalar için veri maskeleme, hassas verilerin görünürlüğünü ayrıcalık sahibi sınırlar. Dinamik veri maskeleme, otomatik olarak olası hassas verileri bulun ve uygulanması için uygun maskeleri önerin. Bu, hassas verilerin yetkisiz erişim veritabanı yok, erişim azaltmaya yardımcı olur. **Müşteriler, dinamik veri maskeleme, veritabanı şeması uyması için ayarları ayarlamak için sorumludur.**

### <a name="identity-management"></a>Kimlik yönetimi
Müşteriler, şirket içi Active Directory Federasyon ile federasyona eklemek için hizmetleri kullanan [Azure Active Directory](https://azure.microsoft.com/services/active-directory/), Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmeti olan. [Azure Active Directory Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) şirket içi dizinleri Azure Active Directory ile tümleşir. Bu çözümdeki tüm kullanıcılar Azure Active Directory hesaplarını gerektirir. Federasyon oturum açma ile kullanıcılar Azure Active Directory ile oturum açın ve şirket içi kimlik bilgilerini kullanarak Azure kaynaklarında kimlik doğrulamasını.

Ayrıca, aşağıdaki Azure Active Directory özellikleri, Azure ortamına veri erişimi yönetmenize yardımcı olmak:
- Azure Active Directory kullanan uygulamaya kimlik doğrulaması gerçekleştirilir. Daha fazla bilgi için [uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).
- [Azure rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) yöneticilerin kullanıcıların işlerini yapması için gereken sadece erişim miktarını vermek için ayrıntılı erişim izinleri tanımlamanıza olanak tanır. Azure kaynakları için her sınırsız kullanıcı izin vermek yerine, yöneticiler verilerine erişmek için yalnızca belirli eylemleri izin verebilir. Abonelik yöneticisine abonelik erişimi sınırlıdır.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) belirli bilgilere erişebilen kullanıcı sayısını en aza indirmek müşterilerin sağlar. Yöneticiler, Azure Active Directory Privileged Identity Management, bulmak, kısıtlamak ve ayrıcalıklı kimlikleri ve bu kimliklerin kaynaklara erişimini izlemek için kullanabilirsiniz. Bu işlev, gerektiğinde talep üzerine tam zamanında yönetimsel erişim uygulamak için de kullanılabilir.
- [Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) kuruluş etkileyen olası güvenlik açıklarını algılar&#39;s kimlikleri, bir kuruluş için ilgili algılanan kuşkulu eylemleri için otomatik yanıtlar yapılandırır&#39;s kimlikleri ve bunları gidermek için uygun eylemde için şüpheli olayları araştırır.

**Azure çok faktörlü kimlik doğrulaması**: Kimlikleri korumak için çok faktörlü kimlik doğrulaması uygulanmalıdır. [Azure multi-Factor Authentication](https://azure.microsoft.com/services/multi-factor-authentication/) kullanıcıları korumak için kimlik doğrulaması için ikinci bir yöntem sağlar bir kullanımı kolay, ölçeklenebilir ve güvenilir bir çözüm. Azure multi-Factor Authentication, bulutun gücünü kullanır ve şirket içi Active Directory ve özel uygulamalarla tümleştirilir. Bu koruma, yüksek hacimli, görev açısından kritik senaryolar için genişletilir.

### <a name="security"></a>Güvenlik
**Gizli dizileri Yönetim**: Çözüm [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) anahtar ve gizli dizi yönetimi. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Aşağıdaki Azure Key Vault özellikleri müşterilerin korumak ve böyle verilere Yardımı:

- Gelişmiş erişim ilkeleri gereksinim olarak yapılandırılır.
- Key Vault erişim ilkeleri anahtarlara ve gizli anahtarları en düşük gerekli izinlerle tanımlanır.
- Tüm anahtarları ve gizli anahtarları Key vault'ta sona erme tarihi vardır.
- Tüm anahtarları Key vault'ta özel bir donanım güvenlik modülleri tarafından korunur. Anahtar türü, bir donanım güvenlik modülü korumalı anahtarlar 2048 bit RSA anahtarı değil.
- Tüm kullanıcılar ve kimlikler rol tabanlı erişim denetimi kullanarak minimum gerekli izinleri verilir.
- Key Vault için tanılama günlükleri ile 365 gün en az bir saklama süresi etkinleştirilir.
- Anahtarlar için izin verilen şifreleme işlemleri gerekli olanlarla sınırlıdır.
- Çözüm, Iaas sanal makine disk şifreleme anahtarlarını ve gizli anahtarları yönetmek için Azure anahtar kasası ile tümleştirilmiştir.

**Düzeltme Eki Yönetimi**: Bu başvuru mimarisinin bir parçası olarak dağıtılmış Windows sanal makineleri, Windows Update hizmetinden otomatik güncelleştirmeleri almak için varsayılan olarak yapılandırılır. Bu çözüm ayrıca içerir [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) üzerinden güncelleştirilmiş dağıtımları oluşturulabilir düzeltme eki gerektiğinde sanal makinelere için hizmet.

**Kötü amaçlı yazılımdan koruma**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) belirlenmesi ve virüslerin, casus yazılımların ve diğer kötü amaçlı yazılım kaldırılmasına yardımcı olan gerçek zamanlı koruma özelliği için sanal makineler sağlar, kötü amaçlı veya istenmeyen yazılım bilinen yapılandırılabilir uyarı ile çalışır yükleme veya korumalı sanal makineler üzerinde çalıştırın.

**Azure Güvenlik Merkezi**: İle [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro), müşterilerin merkezi olarak uygulama ve iş yüklerinizde güvenlik ilkelerini yönetme, sınırlama, tehditlere maruz kalma riskinizi ve algılayabilir ve saldırılara karşılık vermek. Ayrıca, Azure Güvenlik Merkezi, yapılandırma ve güvenlik duruşunu ve verilerin korunmasına yardımcı olmak için hizmet öneriler sağlamak üzere Azure hizmetlerinin mevcut yapılandırmaları erişir.

Azure Güvenlik Merkezi, ortamlarını hedefleyen potansiyel saldırılar müşterileri uyarmak için çeşitli algılama özellikleri kullanır. Bu uyarılar uyarıyı neyin tetiklediği, hedeflenen kaynaklar ve saldırının kaynağı hakkındaki değerli bilgileri içerir. Azure Güvenlik Merkezi'nde bulunan bir dizi [güvenlik uyarıları önceden tanımlanmış](https://docs.microsoft.com/azure/security-center/security-center-alerts-type)bir tehdit tetiklenen veya şüpheli etkinlik gerçekleştiğinde. [Özel uyarı kuralları](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) Azure Güvenlik Merkezi'nde müşterilerin kendi ortamından toplanmış verileri temel alan yeni güvenlik uyarılarını tanımlamanıza izin verin.

Azure Güvenlik Merkezi, müşterilerin bulmasını ve olası güvenlik sorunlarını gidermek için daha basit hale öncelikli güvenlik uyarıları ve olayları sağlar. A [tehdit zekası raporu](https://docs.microsoft.com/azure/security-center/security-center-threat-report) her olay yanıt ekiplerinin tehditleri araştırmanıza ve düzeltme yardımcı olmak için tehdit algılanan için oluşturulur.

Ayrıca, bu başvuru mimarisi kullanan [güvenlik açığı değerlendirmesi](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations) Azure Güvenlik Merkezi'nde. Yapılandırıldıktan sonra (örn. Qualys) iş ortağı Aracısı, güvenlik açığı verilerini iş ortağının yönetim platformuna bildirir. Buna karşılık, iş ortağının yönetim platformuna Güvenlik Açığı ve durum verileri Azure Güvenlik Merkezi, izleme güvenlik açığından etkilenen sanal makinelerin hızlı bir şekilde tanımlamak müşterilerin olanak sağlar.

**Azure uygulama ağ geçidi**: Mimari, Azure Application Gateway ile yapılandırılmış bir web uygulaması güvenlik duvarı ve etkin OWASP ruleset kullanarak güvenlik açıklarını riskini azaltır. Ek özellikler şunlardır:

- [SSL uç bitiş](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Etkinleştirme [SSL yük boşaltma](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Devre dışı [TLS sürüm 1.0 ve v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Web uygulaması güvenlik duvarı](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (önleme modu)
- [Önleme modu](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) OWASP 3.0 ruleset ile
- Etkinleştirme [Tanılama Günlüğü](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)
- [Özel durum araştırmaları](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center) ve [Azure Danışmanı](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) ek koruma ve bildirimleri sağlar. Azure Güvenlik Merkezi, ayrıca bir saygınlığı sistemi sağlar.

### <a name="business-continuity"></a>İş sürekliliği
**Yüksek kullanılabilirlik**: Çözümü dağıtan tüm sanal makinelerin bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Kullanılabilirlik kümeleri, sanal makinelerin kullanılabilirliğini artırmak için birden fazla yalıtılmış donanım kümesi arasında dağıtılmasını sağlar. En az bir sanal makine % 99,95 oranında toplantı planlı veya Plansız bakım olayı sırasında kullanılabilir Azure SLA'sı.

**Kurtarma Hizmetleri kasası**: [Kurtarma Hizmetleri kasası](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) yedekleme verileri barındırır ve tüm yapılandırmaları bu mimaride Azure sanal makineleri korur. Bir kurtarma Hizmetleri kasası ile müşterilerin dosya ve klasörleri bir Iaas sanal makineden sanal makinenin tamamını geri yüklemeden daha hızlı geri yükleme süreleri etkinleştirme geri yükleyebilirsiniz.

**Bulut tanığı**: [Bulut tanığı](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering#BKMK_CloudWitness) yararlanan Azure eklenen ve yönetim noktası olarak Windows Server 2016 yük devretme kümesi çekirdek tanığı türüdür. Diğer tüm çekirdek tanıkları gibi bulut tanığı, bir oy alır ve çekirdek hesaplamalarına katılabilir, ancak standart genel kullanıma açık Azure Blob Depolama kullanır. Bu, genel bulutta barındırılan sanal makineleri bakım ek yükü ortadan kaldırır.

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve Denetim
Azure Hizmetleri, sistem ve kullanıcı etkinliğini yanı sıra, sistem durumu kapsamlı bir şekilde oturum:
- **Etkinlik günlükleri**: [Etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bir Abonelikteki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlükleri bir işlemin Başlatıcı belirlemek yardımcı olabilir, oluşumunu ve durum zaman.
- **Tanılama günlükleri**: [Tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından oluşturulan tüm günlükleri içerir. Bu günlükler, Windows olayı sistem günlükleri, Azure depolama günlükleri, anahtar kasası denetim günlüklerini ve Application Gateway erişim ve güvenlik duvarı günlükleri içerir. Tüm tanılama günlükleri için merkezi ve şifrelenmiş Azure depolama hesabına arşivleme yazın. Bekletme kuruluşa özgü saklama gereksinimlerini karşılamak için kullanıcı-730 gün için yapılandırılabilir,.

**Azure İzleyici günlüklerine**: Bu günlükler, birleştirilmiş [Azure İzleyici günlükleri](https://azure.microsoft.com/services/log-analytics/) işleme, depolama ve Panosu raporlama. Toplandığında, veriler her bir veri türü için ayrı tablolar halinde düzenlenir ve böylece özgün kaynağına bakılmaksızın tüm verilerin birlikte analiz edilmesi sağlanır. Ayrıca, Azure Güvenlik Merkezi güvenlik olay verilerine erişmek ve diğer hizmetlerden gelen verilerle birleştirmek için Kusto sorguları kullanmak müşterilerin sağlayan Azure İzleyici günlükleri ile tümleştirilir.

Aşağıdaki Azure [izleme çözümleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) Bu mimarinin bir parçası olarak dahil edilir:
-   [Active Directory değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözümü risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir ve öneriler için dağıtılan sunucu altyapısı belirli öncelikli bir listesini sağlar.
- [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL sistem durumu denetimi çözümü, risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir ve müşterilerin Önceliklendirilmiş öneriler için dağıtılan sunucu altyapısı belirli listesini sağlar.
- [Aracı sistem durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): Aracı durumu çözümü, kaç aracının dağıtılır ve kullanıcıların coğrafi dağılımı yanı sıra yanıt vermeyen aracı sayısı ve işletimsel veriler gönderen aracıların sayısını raporlar.
-   [Etkinlik günlüğü analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Etkinlik günlüğü analizi çözümü, bir müşteri için tüm Azure abonelikleri arasında Azure etkinlik günlüklerini analiziyle yardımcı olur.

**Azure Otomasyonu**: [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) runbook'ları yöneten depolar ve çalıştırır. Bu çözümde, runbook'ları, Azure SQL Server günlük toplama yardımcı olur. Otomasyon [değişiklik izleme](https://docs.microsoft.com/azure/automation/automation-change-tracking) ortamındaki değişiklikler kolayca belirlemek müşterilerin çözüm sağlar.

**Azure İzleyici**: [Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) kullanıcıların performans izleme, güvenliği koruma ve denetim, uyarılar oluşturabilir ve Azure kaynaklarını izleme API çağrıları dahil olmak üzere, verileri arşivlemek kuruluşların etkinleştirerek eğilimleri belirlemenize yardımcı olur.

[Azure Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview): Azure Ağ İzleyicisi, Azure sanal ağındaki kaynaklarda izleme, tanılama, ölçümleri görüntüleme ve günlükleri etkinleştirme veya devre dışı bırakma işlemleri için araçlar sağlar.  Uluslar varlıklar, Nsg ve sanal makineler için Ağ İzleyicisi akış günlükleri uygulamalıdır. Bu günlükler, yalnızca güvenlik günlükleri depolanır ve depolama hesabına erişim için rol tabanlı erişim denetimleri ile güvenli hale getirilmelidir ayrılmış bir depolama hesabında depolanmalıdır.

## <a name="threat-model"></a>Tehdit modeli
Bu başvuru mimarisine yönelik veri akış diyagramı kullanılabilir [indirme](https://aka.ms/au-protected-iaaswa-tm) veya altında bulunabilir. Bu model, değişiklikler yaparken sistemi altyapısında potansiyel risk puanları anlamasına yardımcı olabilir.

![Iaas Web uygulaması için AU korumalı tehdit modeli](images/au-protected-iaaswa-threat-model.png?raw=true "Iaas Web uygulaması için AU korumalı tehdit modeli diyagramı")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri
Bu uyumluluk belgeleri, platformları ve Hizmetleri Microsoft tarafından sağlanan temel Microsoft tarafından oluşturulur. Müşteri dağıtımları çok çeşitli nedeniyle bu belgeler, yalnızca Azure ortamında barındırılan bir çözüm için genelleştirilmiş bir yaklaşım sağlar. Müşteriler belirleyin ve diğer ürünleri ve Hizmetleri, kendi işletim ortamları ve iş sonuçlarını temel. Şirket kaynaklarını kullanmayı seçen müşteriler, güvenlik ve şirket içi kaynaklarla işlemlerinde incelemeniz gerekir. Belgelenmiş çözümü kendi özel şirket içinde ve güvenlik gereksinimlerini karşılamak için müşteriler tarafından özelleştirilebilir.

[Azure güvenlik ve uyumluluk planı: AU-PROTECTED müşteri sorumluluk matris](https://aka.ms/au-protected-crm) AU korumalı tarafından gerekli tüm güvenlik denetimleri listeler. Bu matris her denetimi uyarlamasını Microsoft, müşteri sorumluluğu olup ayrıntıları veya ikisi arasında paylaşılan.

[Azure güvenlik ve uyumluluk planı – AU-PROTECTED, Iaas, Web, uygulama, uygulama, matris](https://aka.ms/au-protected-iaaswa-cim) üzerinde denetimleri AU korumalı Iaas web uygulaması mimarisi tarafından açıklanmıştır bilgi sağlar dahil olmak üzere Uygulama her kapsanan denetim gereksinimlerini nasıl karşıladığı, ayrıntılı açıklamaları.

## <a name="guidance-and-recommendations"></a>Yönerge ve öneriler
### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute
Sınıflandırılmış bilgileri için güvenli bir IPSec VPN tüneli kullanarak güvenli bir şekilde bu Iaas web uygulaması başvuru mimarisi bir parçası olarak dağıtılan kaynakların bir bağlantı kurmak için yapılandırılması gerekir. Müşteriler, bir IPSec VPN uygun şekilde ayarlayarak, Aktarımdaki veriler için koruma katmanı ekleyebilirsiniz.

Azure ile güvenli bir IPSec VPN tüneli uygulayarak, şirket içi ağ ile bir Azure sanal ağı arasında sanal bir özel bağlantı oluşturulabilir. Bu bağlantı, Internet üzerinden gerçekleşir ve şifreli bir bağlantı müşterinin ağınız ve Azure arasında güvenli bir şekilde "tüneli" bilgilerin müşterilere sağlar. Siteden siteye VPN ölçeklerde tarafından dağıtılan yıllardır güvenli, olgun bir teknolojidir. 

Microsoft, siteden siteye VPN ile Internet trafiği VPN tüneli içinde geçiş için bir özel bağlantı seçeneği sunar. Azure ExpressRoute, Azure ve şirket içi konum veya Exchange barındırma sağlayıcısı arasında ayrılmış bir bağlantıdır ve özel ağ olarak kabul edilir. ExpressRoute bağlantıları Internet üzerinden geçmemektedir gibi bu bağlantılar, daha fazla güvenilirlik, daha yüksek hızlar ve daha düşük gecikme süreleri tipik Internet üzerinden sunar. Ayrıca, bu müşterinin telekomünikasyon sağlayıcıları, doğrudan bir bağlantı olduğu için veriler Internet üzerinden yolculuk ediyor mu değil ve ona bu nedenle olarak gösterilmez.

Bir şirket içi ağı Azure'a genişleten güvenli bir hibrit ağı uygulamak için en iyi uygulamalardan bazılarıdır [kullanılabilir](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid). 

İnternet'te veya Azure ExpressRoute kullanarak olmadığını sınıflandırılmış verileri korumaya yardımcı olmak için müşteriler kendi IPSec VPN aşağıdaki ayarları uygulayarak yapılandırmanız gerekir:

• Müşteri VPN Başlatıcı 14400 saniyeden büyük bir SA yaşam süresi'için yapılandırılmış olması gerekir.
• Müşteri VPN Başlatıcı Ikev1 agresif modu devre dışı bırakmanız gerekir.
• Müşteri VPN Başlatıcı kusursuz iletme gizliliği yapılandırmanız gerekir.
• VPN Başlatıcı müşteri HMAC SHA256 veya büyük yapılandırmanız gerekir.

Yapılandırma seçenekleri için VPN cihazları ve IPSec / IKE parametreleri [kullanılabilir](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices) gözden geçirme.

### <a name="azure-active-directory-setup"></a>Azure Active Directory Kurulumu
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) dağıtımını yöneten ve ortam ile etkileşim personel erişimi sağlama için gereklidir. Mevcut bir Windows Server Active Directory, Azure Active Directory ile tümleştirilebilir [dört tıklama](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express).

Ayrıca, [Azure Active Directory Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) ile şirket içi Federasyon yapılandırmak müşterileriniz [Active Directory Federasyon Hizmetleri]( https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-azure-adfs) ve Azure Active Directory. Federasyon oturum açma ile müşteriler, şirket ağındaki kullanıcıların parolalarını yeniden girmek zorunda kalmadan parolalarını şirket içi ile Azure Active Directory tabanlı hizmetlere oturum açmalarını etkinleştirebilirsiniz. Active Directory Federasyon Hizmetleri ile Federasyon seçeneğini kullanarak yeni bir Active Directory Federasyon Hizmetleri yüklemesini dağıtabilir veya bir Windows Server 2012 R2 grubunda var olan bir yüklemesini belirtebilirsiniz.

Azure Active Directory eşitleme durumundan sınıflandırılmış verileri önlemek için müşterilerin Azure Active Directory Connect içerisinde bulunan aşağıdaki ayarlara uygulayarak Azure Active Directory'ye çoğaltılır öznitelikleri kısıtlayabilirsiniz:
- [Filtreleme etkinleştir](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-configure-filtering)
- [Parola Karması eşitleme devre dışı bırak](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization)
- [Parola geri yazmayı devre dışı bırak](https://docs.microsoft.com/azure/active-directory/authentication/quickstart-sspr)
- [Cihaz geri yazmayı devre dışı bırak](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-feature-device-writeback)
- Varsayılan ayarlarını bırakın [yanlışlıkla silmeleri engelleme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-feature-prevent-accidental-deletes) ve [otomatik yükseltme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-feature-automatic-upgrade)

## <a name="disclaimer"></a>Bildirim
- Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR EXPRESS, ZIMNİ VEYA YASAL BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-olduğundan." Bilgi ve URL ve diğer Internet Web sitesi referansları da dahil olmak üzere bu belgede, bildirilmeksizin değiştirilebilir. Müşteriler bu belgeyi okuma KULLANIMLARDAN doğacak riskler size aittir.
- Bu belge, müşterilerle herhangi bir Microsoft ürünü veya çözümler üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.
- Müşteriler kopyalayabilir ve dahili başvuru amacıyla bu belgeyi kullanın.
- Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.
- Bu mimari, müşterilerin kendi belirli gereksinimlerine ayarlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-üretim ortamıdır.
- Bu belge, bir başvuru olarak geliştirilir ve tüm anlamına gelir, bir müşteri özel uyumluluk gereksinimlerini ve düzenlemeleri karşılayabilecek tanımlamak için kullanılmamalıdır. Müşterilerin onaylı müşteri uygulamaları kuruluşları yasal Destek'ten arama.
