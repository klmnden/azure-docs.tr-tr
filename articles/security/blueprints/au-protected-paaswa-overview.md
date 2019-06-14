---
title: Azure güvenlik ve uyumluluk planı - PaaS Web uygulaması için Avustralya korumalı
description: Azure güvenlik ve uyumluluk planı - PaaS Web uygulaması için Avustralya korumalı
services: security
author: meladie
ms.assetid: 708aa129-b226-4e02-85c6-1f86e54564e4
ms.service: security
ms.topic: article
ms.date: 08/23/2018
ms.author: meladie
ms.openlocfilehash: c17f16ce796c9f296facd69c18de4effc7ff5258
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60610154"
---
# <a name="azure-security-and-compliance-blueprint---paas-web-application-for-australia-protected"></a>Azure güvenlik ve uyumluluk planı - PaaS Web uygulaması için Avustralya korumalı

## <a name="overview"></a>Genel Bakış

Azure güvenlik ve uyumluluk planı koleksiyonu, depolanması ve alınması hedefleri ile uyumlu olan kamu AU korumalı veriler için uygun bir hizmet (PaaS) ortamı olarak bir platform dağıtımı için yönergeler sağlar Avustralya devlet bilgi güvenliği el ile (Avustralya sinyal Directorate (ASD) tarafından üretilen ISM). Bu şema genel bir başvuru mimarisi gösteren ve güvenli, uyumlu, çok katmanlı bir ortamda hassas devlet kurumları veri doğru işlenmesi yardımcı gösterir.

Bu başvuru mimarisi, Uygulama Kılavuzu ve tehdit modeli müşterilerin ASD uyumlu bir şekilde Azure'a iş yüklerini dağıtmaya yardımcı müşterilerin kendilerine ait planlanması ve sistem akreditasyon süreçleri gerçekleştirmek bir temel sağlar. Müşteriler bir Azure VPN ağ geçidi veya ExpressRoute Federasyon Hizmetleri kullanın ve şirket içi kaynaklarınızı Azure kaynakları ile tümleştirme için uygulamayı seçebilir. Müşteriler, şirket içi kaynakları kullanarak güvenlik etkilerini dikkate almanız gerekir. Ek yapılandırma gereksinimlerini karşılamak için gerekli aynıdır her bir müşterinin uygulama ayrıntılarına göre değişiklik gösterebilir.

ASD uyumluluk elde kayıtlı bir bilgi güvenlik denetçisi sistem denetimleri gerektirir. Müşteriler, uygun güvenlik yürütmek için sorumludur ve her bir müşterinin uygulama ayrıntılarına bağlı uyumluluk değerlendirme gereksinimleri değişebilir, bu mimariyi kullanarak oluşturulan bir çözümün temel.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri
Bu çözüm, bir Azure SQL veritabanı arka ucu ile bir PaaS web uygulaması için bir başvuru mimarisi sağlar. Web uygulaması, yalıtılmış bir ortamda Azure uygulama hizmeti olan Azure bir veri özel, adanmış bir ortamda, barındırılır. Ortam yük, Azure tarafından yönetilen sanal makinelerdeki web uygulaması için trafiği dengeler. Tüm web uygulama bağlantıları, TLS 1.2 en az bir sürümü gerektirir. Bu mimari, ağ güvenlik grupları, bir Application Gateway, Azure DNS ve yük dengeleyici de içerir.

Mimari, kuruluşun özel yerel ağ veya internet'ten şirket kullanıcıları tarafından güvenli bir şekilde erişilmek üzere web tabanlı iş yüklerini izin verme bir şirket içi ağı Azure'a genişleten güvenli bir hibrit ortam sunabilir. Şirket içi çözümler için müşteri sorumlu ve güvenlik, işlemlerini ve uyumluluk tüm boyutlarından sorumlu.

Bu çözümde yer alan Azure kaynaklarını bir şirket içi ağ veya datacentre ortak yerleşim tesisinizden (CDC Kanbera, örneğin) bir VPN ağ geçidini kullanarak IPSec VPN ve ExpressRoute aracılığıyla bağlanabilirsiniz. Bir VPN kullanmaya karar iletilen verileri ve göz önünde ağ yolunda sınıflandırmada yapılmalıdır. Büyük veri gereksinimleri olan görev açısından kritik iş yükleri, büyük ölçekli çalıştıran müşteriler, Azure hizmetlerine özel ağ bağlantısı için ExpressRoute kullanarak hibrit ağ mimarisi düşünmelisiniz. Başvurmak [yönerge ve öneriler](#guidance-and-recommendations) bölüm azure'a bağlantı mekanizmaları hakkında daha ayrıntılı bilgi için.

Azure Active Directory ile Federasyon, kullanıcıların şirket içi kimlik bilgilerini kullanarak kimlik doğrulaması yapmak ve bir şirket içi Active Directory Federasyon Hizmetleri altyapısını kullanarak buluttaki tüm kaynaklara erişim sağlamak için kullanılmalıdır. Basitleştirilmiş, güvenli Kimlik Federasyonu ve web çoklu oturum açma özellikleri bu hibrit ortamı için Active Directory Federasyon Hizmetleri sağlar. Başvurmak [yönerge ve öneriler](#guidance-and-recommendations) Azure Active Directory Kurulum daha fazla ayrıntı için bölüm.

Çözüm, müşterilerin bekleyen verilerin gizliliğini depolama hizmeti Şifrelemesi'ni kullanmak için yapılandırabilirsiniz Azure depolama hesapları kullanır. Azure dayanıklılık için Müşteri'nin seçili bölgede verilerin üç kopyasını depolar. Azure bölgeleri dayanıklı bölge çiftlerinde dağıtılır ve veriler üç kopya ile ikinci bölgeye çoğaltılır, coğrafi olarak yedekli depolama sağlar. Bu, müşterinin birincil veri konumda veri kaybıyla sonuçlanan olumsuz bir olay engeller.

Gelişmiş güvenlik için bu çözümdeki tüm Azure kaynaklarını bir kaynak grubuyla Azure Resource Manager aracılığıyla yönetilir. Azure Active Directory rol tabanlı erişim denetimi erişimi denetlemek için kullanılan dağıtılan kaynakları ve Azure Key vault'taki anahtarları. Sistem durumu, Azure Güvenlik Merkezi ve Azure İzleyici izlenir. Müşteriler, günlükleri tutmak ve sistem durumu bir kolayca gezinebilir, tek bir Panoda görüntülemek için her iki izleme hizmetleri yapılandırın. Azure Application Gateway önleme modunda bir güvenlik duvarı olarak yapılandırılır ve TLS 1.2 sürümü olmayan trafiği izin vermiyor veya üzeri. Çözüm, Azure uygulama hizmeti ortamı v2, çok müşterili bir ortamda web katmanı yalıtmak için kullanır.

![PaaS Web uygulaması AU korumalı başvuru mimarisi için](images/au-protected-paaswa-architecture.png?raw=true "PaaS Web uygulaması için başvuru AU korumalı mimarisi diyagramı")

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Diğer ayrıntılar için bkz [dağıtım mimarisi](#deployment-architecture) bölümü.

- Application Gateway
    - Web uygulaması güvenlik duvarı
        - Güvenlik Duvarı modu: önleme
        - Kural kümesi: OWASP
        - Dinleyici bağlantı noktası: 443
- Application Insights
- Azure Active Directory
- Azure uygulama hizmeti ortamı v2
- Azure Otomasyonu
- Azure DNS
- Azure Key Vault
- Azure Load Balancer
- Azure İzleyici
- Azure Resource Manager
- Azure Güvenlik Merkezi
- Azure SQL Veritabanı
- Azure Storage
- Azure İzleyici günlükleri
- Azure Sanal Ağ
    - (1) /16 ağ
    - (4) /24 ağlar
    - Ağ güvenlik grupları
- Ağ güvenlik grupları
- Kurtarma Hizmetleri kasası
- Azure Web Uygulaması

Bu şema kullanılmak üzere korumalı sınıflandırma Avustralya siber Güvenlik Merkezi (ACSC) tarafından onaylanmış değil Azure hizmetleri içerir. Microsoft, müşterilerin gözden geçirin, yayımlanan güvenlik ve Denetim raporları, bu Azure Hizmetleri ile ilgili ve Azure hizmet kendi iç akreditasyonu ve kullanım için uygun olup olmadığını belirlemek için kendi risk yönetim çerçevesi kullanın önerir Korumalı sınıflandırması.

## <a name="deployment-architecture"></a>Dağıtım mimarisi
Aşağıdaki bölümde dağıtım ve uygulama öğeleri ayrıntılı olarak açıklanmaktadır.

**Azure Resource Manager**: [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) müşterilerin kaynaklarla çözümünde bir grup olarak çalışmanıza olanak tanır. Müşteriler dağıtma, güncelleştirme veya tek ve eşgüdümlü bir işlemle çözüm için tüm kaynakları silin. Müşteriler, dağıtım için bir şablon kullanabilirsiniz ve bu şablon test, hazırlama ve üretim gibi farklı ortamlarda çalışabilir. Resource Manager Güvenlik, denetleme ve etiketleme müşteriler kaynaklarını dağıttıktan sonra yönetmenize yardımcı olacak özellikler sunar.

**Kale ana bilgisayarı**: Kale ana bilgisayarı tek kullanıcılara bu ortama dağıtılan kaynaklara erişmek giriş noktasıdır. Kale ana bilgisayarı, genel IP adreslerinden gelen uzak trafiğine yalnızca güvenli bir listede vererek dağıtılan kaynaklara güvenli bir bağlantı sağlar. Uzak Masaüstü (RDP) trafiğine izin vermek için trafik kaynağını ağ güvenlik grubu listesinde tanımlanması gerekir.

Bu çözüm aşağıdaki yapılandırmaları olan bir etki alanına katılmış Burcu ana bilgisayarı olarak bir sanal makine oluşturur:
-   [Kötü amaçlı yazılımdan koruma uzantısı](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [Azure tanılama uzantısı](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) Azure Key Vault kullanma
-   Bir [otomatik kapatma ilke](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) kullanımda olmadığında sanal makine kaynakların tüketimini azaltmak için

**App Service ortamı v2**: [Azure App Service ortamı](https://docs.microsoft.com/azure/app-service/environment/intro) , App Service uygulamalarını yüksek ölçekte güvenli bir şekilde çalıştırmak için tam yalıtılmış ve ayrılmış bir ortam sağlayan bir App Service özelliğidir.

App Service ortamları yalnızca tek bir müşterinin uygulamalarını çalıştırmak için yalıtılmış ve her zaman bir sanal ağa dağıtılır. Her iki uygulama gelen ve giden ağ trafiği üzerinde ayrıntılı denetim imajlarını ve uygulamaları, şirket içi kurumsal kaynaklara sanal ağlar üzerinden yüksek hızda güvenli bağlantılar kurabilirsiniz.

Bu mimari için App Service ortamları kullanımını aşağıdaki denetimleri/yapılandırmalar için izin ver:

- App Service ortamları yalıtılmış hizmet planı kullanmak için yapılandırılmalıdır
    - Farklı sınıflandırmaları farklı uygulamalar için App Service ortamları yapılandırma
- Güvenli bir Azure sanal ağ içinde ana bilgisayar ve ağ güvenlik kuralları
- App Service ortamları HTTPS iletişimi için otomatik olarak imzalanan bir iç yük dengeleyici sertifikayla yapılandırılmış. En iyi uygulama, Microsoft, Gelişmiş güvenlikten yararlanmaya başlamak için güvenilir bir sertifika yetkilisi kullanımını önerir.
- [İç Yük Dengeleme modu](https://docs.microsoft.com/azure/app-service-web/app-service-environment-with-internal-load-balancer) (mod 3)
- Devre dışı [TLS sürüm 1.0 ve v1.1](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Değişiklik [TLS şifreleme](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Denetim [gelen trafiği N/W bağlantı noktaları](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic)
- [Web uygulaması güvenlik duvarı – veri kısıtlama](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-web-application-firewall)
- İzin [Azure SQL veritabanı trafiği](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-network-architecture-overview)

**Azure Web uygulaması**: [Azure App Service](https://docs.microsoft.com/azure/app-service/) müşterilerin oluşturmanıza ve altyapı yönetimine gerek kalmadan kendi seçtiğiniz programlama dilinde web uygulamaları barındırmanıza olanak sağlar. Otomatik ölçeklendirme ve yüksek kullanılabilirlik sunar, hem Windows hem de Linux’ı destekler ve GitHub, Azure DevOps Services veya herhangi bir Git deposundan otomatik dağıtımlar sağlar.

### <a name="virtual-network"></a>Sanal Ağ
10\.200.0.0/16 bir adres alanı ile özel bir sanal ağ mimarisini tanımlar.

**Ağ güvenlik grupları**: [Ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) izin veren veya bir sanal ağ içinde trafiği reddeden erişim denetim listeleri içerir. Ağ güvenlik grupları, trafiğin bir alt ağ veya tek tek sanal makine düzeyinde güvenliğini sağlamak için kullanılabilir. Aşağıdaki ağ güvenlik grupları mevcut:
- Application Gateway için 1 ağ güvenlik grubu
- App Service ortamı için 1 ağ güvenlik grubu
- Azure SQL veritabanı için 1 ağ güvenlik grubu
- Kale ana bilgisayarı için 1 ağ güvenlik grubu

Sahip ağ güvenlik gruplarının her biri belirli bağlantı noktaları ve protokolleri çözüm güvenli bir şekilde ve doğru bir şekilde çalışabilmek açın. Ayrıca, aşağıdaki yapılandırmalar için her bir ağ güvenlik grubu etkinleştirilir:

  - [Tanılama günlüklerini ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) etkinleştirilir ve bir depolama hesabında depolanmış
  - Azure İzleyici günlüklerine bağlı olduğu [ağ güvenlik grubunun tanılama](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Alt ağlar**: Her alt ağa karşılık gelen ağ güvenlik grubu ile ilişkilidir.

**Azure DNS**: DNS veya etki alanı adı sistemi çevirmek için sorumludur (veya çözümleme) IP adresini bir Web sitesi veya hizmet adı. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) Azure altyapısı kullanılarak ad çözümlemesi sağlayan bir barındırma hizmeti DNS etki alanları için. Kullanıcılar, etki alanlarını azure'da barındırarak aynı kimlik bilgileri, API'ler, Araçlar ve diğer Azure hizmetlerinde faturalama DNS kayıtlarını yönetebilirsiniz. Azure DNS özel DNS etki alanları da destekler.

**Azure yük dengeleyici**: [Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) müşterilerin uygulamalarını ölçeklendirme ve yüksek kullanılabilirlik hizmetleri oluşturma olanak tanır. Yük Dengeleyici, gelen yanı sıra giden senaryoları destekler ve düşük gecikme süreli, yüksek aktarım hızı sağlar ve akışlar tüm TCP ve UDP uygulamaları için en fazla bir milyonlarca ölçeklendirir.

### <a name="data-in-transit"></a>Aktarım durumundaki veriler
Azure, Azure veri merkezlerinden tüm iletişimi varsayılan olarak şifreler. 

Ağları ait müşteriden Aktarımdaki korunan veriler için Mimari Azure ExpressRoute ve Internet IPSec ile yapılandırılmış bir VPN ağ geçidi ile kullanır.

Ayrıca, Azure Yönetim Portalı aracılığıyla azure'a tüm işlemleri TLS 1.2 kullanarak HTTPS gerçekleştirilir.

### <a name="data-at-rest"></a>Bekleyen veriler
Mimarisi, bekleyen veri şifrelemesi, Denetim veritabanı ve diğer ölçüler verilerinizi korumanızı sağlar.

**Azure depolama**: Şifrelenmiş veri rest gereksinimleri karşılamak için tüm [Azure depolama](https://azure.microsoft.com/services/storage/) kullanan [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption). Bu, kuruluş güvenlik taahhütlerine ve Avustralya hükümeti ISM tarafından tanımlanan uyumluluk gereksinimlerini desteklemek üzere verileri koruyarak yardımcı olur.

**Azure Disk şifrelemesi**: [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) veri diskleri için birim şifrelemesi sağlamak için Windows BitLocker özelliğidir yararlanır. Çözüm denetlemenize ve disk şifreleme anahtarlarını yönetmek için Azure anahtar kasası ile tümleştirilir.

**Azure SQL veritabanı**: Azure SQL veritabanı örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:
-   [Active Directory kimlik doğrulaması ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-AAD-authentication) veritabanı kullanıcıları ve diğer Microsoft Hizmetleri tek bir merkezi konumda kimlik yönetimini sağlar.
-   [SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) veritabanı olaylarını izler ve bir denetim günlüğüne bir Azure depolama hesabında yazar.
-   Azure SQL veritabanını kullanacak şekilde yapılandırıldığını [saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), gerçek zamanlı şifreleme ve şifre çözme veritabanının, ilişkili yedeklemeler gerçekleştirir ve bilgileri korumak için işlem günlüğü dosyaları bekletin. Veriler güvencesi yetkisiz erişim ayarlanmamış saydam veri şifrelemesi sağlar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) doğru izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) şüpheli veritabanı etkinlikleri, potansiyel açıklar, SQL ekleme saldırıları ve anormal veritabanı erişim için güvenlik uyarıları sağlayarak oluşunca algılama ve olası tehditlere yanıt sağlar. desenler. SQL tehdit algılama uyarıları ile tümleştirilir [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/), şüpheli etkinlik ayrıntılarını içerir ve önerilen eylem araştırmak ve tehdidi azaltmak için.
-   [Always Encrypted sütunları](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) hassas verileri hiçbir zaman içinde bir veritabanı sistemi düz metin olarak göründüğünden emin olun. Veri şifrelemesi etkinleştirildikten sonra yalnızca istemci uygulamaları veya uygulama sunucuları anahtarlarına erişimi ile düz metin verilere erişebilir.
- [SQL veritabanı dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) tarafından ayrıcalıksız kullanıcılar veya uygulamalar için veri maskeleme, hassas verilerin görünürlüğünü ayrıcalık sahibi sınırlar. Dinamik veri maskeleme, otomatik olarak olası hassas verileri bulun ve uygulanması için uygun maskeleri önerin. Bu, hassas verilerin yetkisiz erişim veritabanı yok, erişim azaltmaya yardımcı olur. Müşterilerin, dinamik veri maskeleme ayarları kendi veritabanı şeması uyması için ayarlamanız gerekir.

### <a name="identity-management"></a>Kimlik yönetimi
Müşteriler, şirket içi Active Directory Federasyon ile federasyona eklemek için hizmetleri kullanan [Azure Active Directory](https://azure.microsoft.com/services/active-directory/), Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmeti olan. [Azure Active Directory Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) şirket içi dizinleri Azure Active Directory ile tümleşir. Bu çözümdeki tüm kullanıcılar Azure Active Directory hesaplarını Azure SQL veritabanına erişen kullanıcılar dahil olmak üzere, gerektirir. Federasyon oturum açma ile kullanıcılar Azure Active Directory ile oturum açın ve şirket içi kimlik bilgilerini kullanarak Azure kaynaklarında kimlik doğrulamasını.

Ayrıca, aşağıdaki Azure Active Directory özellikleri, Azure ortamına veri erişimi yönetmenize yardımcı olmak:
- Azure Active Directory kullanan uygulamaya kimlik doğrulaması gerçekleştirilir. Daha fazla bilgi için [uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Ayrıca, Azure SQL veritabanı uygulamaya kimlik doğrulaması için Azure Active Directory veritabanı sütun şifreleme kullanır. Daha fazla bilgi için bkz. nasıl [Azure SQL veritabanındaki hassas verileri korumaya](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
- [Azure rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) yöneticilerin kullanıcıların işlerini yapması için gereken sadece erişim miktarını vermek için ayrıntılı erişim izinleri tanımlamanıza olanak tanır. Azure kaynakları için her sınırsız kullanıcı izin vermek yerine, yöneticiler verilerine erişmek için yalnızca belirli eylemleri izin verebilir. Abonelik yöneticisine abonelik erişimi sınırlıdır.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) belirli bilgilere erişebilen kullanıcı sayısını en aza indirmek müşterilerin sağlar. Yöneticiler, Azure Active Directory Privileged Identity Management, bulmak, kısıtlamak ve ayrıcalıklı kimlikleri ve bu kimliklerin kaynaklara erişimini izlemek için kullanabilirsiniz. Bu işlev, gerektiğinde talep üzerine tam zamanında yönetimsel erişim uygulamak için de kullanılabilir.
- [Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) kuruluşların kimliklerini etkileyen olası güvenlik açıklarını algılar, bir kuruluşun kimlikleri ilgili algılanan kuşkulu eylemleri için otomatik yanıtlar yapılandırır ve araştırır bunları gidermek için uygun eylemde için şüpheli olayları.

**Azure çok faktörlü kimlik doğrulaması**: Kimlikleri korumak için çok faktörlü kimlik doğrulaması uygulanmalıdır. [Azure multi-Factor Authentication](https://azure.microsoft.com/services/multi-factor-authentication/) kullanıcıları korumak için kimlik doğrulaması için ikinci bir yöntem sağlar bir kullanımı kolay, ölçeklenebilir ve güvenilir bir çözüm. Azure multi-Factor Authentication, bulutun gücünü kullanır ve şirket içi Active Directory ve özel uygulamalarla tümleştirilir. Bu koruma, yüksek hacimli, görev açısından kritik senaryolar için genişletilir.

### <a name="security"></a>Güvenlik
**Gizli dizileri Yönetim**: Çözüm [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) anahtar ve gizli dizi yönetimi. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Aşağıdaki Azure Key Vault özellikleri müşteri verilerini korumaya yardımcı olur:
- Gelişmiş erişim ilkeleri gereksinim olarak yapılandırılır.
- Key Vault erişim ilkeleri anahtarlara ve gizli anahtarları en düşük gerekli izinlerle tanımlanır.
- Tüm anahtarları ve gizli anahtarları Key vault'ta sona erme tarihi vardır.
- Tüm anahtarları Key vault'ta özel bir donanım güvenlik modülleri tarafından korunur. Anahtar türü, bir donanım güvenlik modülü korumalı anahtarlar 2048 bit RSA anahtarı değil.
- Tüm kullanıcılar ve kimlikler rol tabanlı erişim denetimi kullanarak minimum gerekli izinleri verilir.
- Key Vault için tanılama günlükleri ile 365 gün en az bir saklama süresi etkinleştirilir.
- Anahtarlar için izin verilen şifreleme işlemleri gerekli olanlarla sınırlıdır.

**Azure Güvenlik Merkezi**: İle [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro), müşterilerin merkezi olarak uygulama ve iş yüklerinizde güvenlik ilkelerini yönetme, sınırlama, tehditlere maruz kalma riskinizi ve algılayabilir ve saldırılara karşılık vermek. Ayrıca, Azure Güvenlik Merkezi, yapılandırma ve güvenlik duruşunu ve verilerin korunmasına yardımcı olmak için hizmet öneriler sağlamak üzere Azure hizmetlerinin mevcut yapılandırmaları erişir.

Azure Güvenlik Merkezi, ortamlarını hedefleyen potansiyel saldırılar müşterileri uyarmak için çeşitli algılama özellikleri kullanır. Bu uyarılar uyarıyı neyin tetiklediği, hedeflenen kaynaklar ve saldırının kaynağı hakkındaki değerli bilgileri içerir. Azure Güvenlik Merkezi'nde bulunan bir dizi [güvenlik uyarıları önceden tanımlanmış](https://docs.microsoft.com/azure/security-center/security-center-alerts-type)bir tehdit tetiklenen veya şüpheli etkinlik gerçekleştiğinde. [Özel uyarı kuralları](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) Azure Güvenlik Merkezi'nde müşterilerin kendi ortamından toplanmış verileri temel alan yeni güvenlik uyarılarını tanımlamanıza izin verin.

Azure Güvenlik Merkezi, müşterilerin bulmasını ve olası güvenlik sorunlarını gidermek için daha basit hale öncelikli güvenlik uyarıları ve olayları sağlar. A [tehdit zekası raporu](https://docs.microsoft.com/azure/security-center/security-center-threat-report) her olay yanıt ekiplerinin tehditleri araştırmanıza ve düzeltme yardımcı olmak için tehdit algılanan için oluşturulur.

**Uygulama ağ geçidi**: Mimari, bir web uygulaması güvenlik duvarı ve etkin OWASP kural kümesi ile bir uygulama ağ geçidi kullanarak güvenlik açıklarını riskini azaltır. Ek özellikler şunlardır:

- [SSL uç bitiş](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Etkinleştirme [SSL yük boşaltma](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Devre dışı [TLS sürüm 1.0 ve v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Web uygulaması güvenlik duvarı](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (önleme modu)
- [Önleme modu](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) OWASP 3.0 ruleset ile
- Etkinleştirme [Tanılama Günlüğü](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)
- [Özel durum araştırmaları](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center) ve [Azure Danışmanı](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) ek koruma ve bildirimleri sağlar. Azure Güvenlik Merkezi, ayrıca bir saygınlığı sistemi sağlar.

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

**Azure Otomasyonu**: [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) runbook'ları yöneten depolar ve çalıştırır. Bu çözümde, runbook'ları, Azure SQL veritabanı'ndan günlüklerini toplama yardımcı olur. Otomasyon [değişiklik izleme](https://docs.microsoft.com/azure/automation/automation-change-tracking) ortamındaki değişiklikler kolayca belirlemek müşterilerin çözüm sağlar.

**Azure İzleyici**: [Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) kullanıcıların performans izleme, güvenliği koruma ve denetim, uyarılar oluşturabilir ve Azure kaynaklarını izleme API çağrıları dahil olmak üzere, verileri arşivlemek kuruluşların etkinleştirerek eğilimleri belirlemenize yardımcı olur.

Azure Ağ İzleyicisi: [Azure Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) izleyin, tanılayın, ölçümleri görüntüleyin ve etkinleştirme veya günlükleri için bir Azure sanal ağ içindeki kaynaklarla devre dışı araçları sağlar.  Uluslar varlıklar, Nsg ve sanal makineler için Ağ İzleyicisi akış günlükleri uygulamalıdır. Bu günlükler, yalnızca güvenlik günlükleri depolanır ve depolama hesabına erişim için rol tabanlı erişim denetimleri ile güvenli hale getirilmelidir ayrılmış bir depolama hesabında depolanmalıdır.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisine yönelik veri akış diyagramı kullanılabilir [indirme](https://aka.ms/au-protected-paaswa-tm) veya altında bulunabilir. Bu model, değişiklikler yaparken sistemi altyapısında potansiyel risk puanları anlamasına yardımcı olabilir.

![PaaS Web uygulaması AU-PROTECTED tehdit modeli için](images/au-protected-paaswa-threat-model.png?raw=true "PaaS Web uygulaması için AU korumalı tehdit modeli diyagramı")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri
Bu uyumluluk belgeleri, platformları ve Hizmetleri Microsoft tarafından sağlanan temel Microsoft tarafından oluşturulur. Müşteri dağıtımları çok çeşitli nedeniyle bu belgeler, yalnızca Azure ortamında barındırılan bir çözüm için genelleştirilmiş bir yaklaşım sağlar. Müşteriler belirleyin ve diğer ürünleri ve Hizmetleri, kendi işletim ortamları ve iş sonuçlarını temel. Şirket kaynaklarını kullanmayı seçen müşteriler, güvenlik ve şirket içi kaynaklarla işlemlerinde incelemeniz gerekir. Belgelenmiş çözümü kendi özel şirket içinde ve güvenlik gereksinimlerini karşılamak için müşteriler tarafından özelleştirilebilir.

[Azure güvenlik ve uyumluluk planı - AU-PROTECTED müşteri sorumluluk matris](https://aka.ms/au-protected-crm) AU kuralı tarafından gerekli tüm güvenlik denetimleri listeler Bu matris her denetimi uyarlamasını Microsoft, müşteri sorumluluğu olup ayrıntıları veya ikisi arasında paylaşılan.

[Azure güvenlik ve uyumluluk planı - AU-PROTECTED, PaaS, Web, uygulama, uygulama, matris](https://aka.ms/au-protected-paaswa-cim) üzerinde denetimleri AU korumalı PaaS web uygulaması mimarisi tarafından açıklanmıştır bilgi sağlar dahil olmak üzere Uygulama her kapsanan denetim gereksinimlerini nasıl karşıladığı, ayrıntılı açıklamaları.

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
-   [Parola geri yazmayı devre dışı bırak](https://docs.microsoft.com/azure/active-directory/authentication/quickstart-sspr)
-   [Cihaz geri yazmayı devre dışı bırak](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-feature-device-writeback)
-   Varsayılan ayarlarını bırakın [yanlışlıkla silmeleri engelleme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-feature-prevent-accidental-deletes) ve [otomatik yükseltme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-feature-automatic-upgrade)


## <a name="disclaimer"></a>Bildirim

 - Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR EXPRESS, ZIMNİ VEYA YASAL BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-olduğundan." Bilgi ve URL ve diğer Internet Web sitesi referansları da dahil olmak üzere bu belgede, bildirilmeksizin değiştirilebilir. Müşteriler bu belgeyi okuma KULLANIMLARDAN doğacak riskler size aittir.
 - Bu belge, müşterilerle herhangi bir Microsoft ürünü veya çözümler üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler kopyalayabilir ve dahili başvuru amacıyla bu belgeyi kullanın.
 - Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.
 - Bu mimari, müşterilerin kendi belirli gereksinimlerine ayarlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-üretim ortamıdır.
 - Bu belge, bir başvuru olarak geliştirilir ve tüm anlamına gelir, bir müşteri özel uyumluluk gereksinimlerini ve düzenlemeleri karşılayabilecek tanımlamak için kullanılmamalıdır. Müşterilerin onaylı müşteri uygulamaları kuruluşları yasal Destek'ten arama.
