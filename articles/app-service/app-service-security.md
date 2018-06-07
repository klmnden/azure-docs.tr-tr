---
title: Azure uygulama hizmeti ve Azure işlevleri güvenliği | Microsoft Docs
description: App Service'in nasıl güvenli yardımcı olduğunu hakkında uygulamanızı ve nasıl daha fazla uygulamanızı tehditlere karşı aşağı kilitleyebilir öğrenin.
keywords: Azure uygulama hizmeti, web uygulaması, mobil uygulama, API uygulaması, işlev uygulaması, güvenlik, güvenli, güvenli, uyumluluk, uyumlu, sertifika, sertifika, https, ftps, tls, güven, şifreleme, şifreleme, şifrelenmiş, IP kısıtlaması, kimlik doğrulama, yetkilendirme, kimlik doğrulama, autho MSI, yönetilen hizmet kimliği, gizli, gizli, düzeltme eki uygulama, düzeltme eki, düzeltme ekleri, sürüm, yalıtım, ağ yalıtımı, ddos, mıtm
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2018
ms.author: cephalin
ms.openlocfilehash: 2ca1c1518589e60a03570e1c2063381f749ed9aa
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655369"
---
# <a name="security-in-azure-app-service-and-azure-functions"></a>Azure uygulama hizmeti ve Azure işlevleri güvenliği

Bu makale size nasıl gösterir [Azure App Service](app-service-web-overview.md) yardımcı güvenli web uygulaması, mobil uygulama arka ucu, API uygulaması ve [Azure işlevleri](/azure/azure-functions/). Ayrıca, uygulamanızı yerleşik uygulama hizmeti özellikleri ile nasıl daha fazla alabilecek gösterir.

Azure VM'ler, depolama, ağ bağlantıları, web çerçeveleri, yönetim ve tümleştirme özellikleri dahil olmak üzere, App Service platformu bileşenlerini etkin olarak güvenli ve sıkı. Uygulama hizmeti düşük uyumluluk denetimleri emin olmak için sürekli olarak geçer:

- Uygulama kaynaklarınız [güvenli](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) diğer müşterilerin Azure kaynaklarından.
- [VM örnekleri ve çalışma zamanı yazılım düzenli olarak güncelleştirilen](app-service-patch-os-runtime.md) yeni keşfedilen güvenlik açıklarını için. 
- Gizli anahtarları (gibi bağlantı dizeleri), uygulama ve diğer Azure kaynaklarına arasında iletişimi (gibi [SQL veritabanı](/services/sql-database/)) Azure içinde kalır ve tüm ağ sınırlar arası değil. Gizli depolandığında her zaman şifrelenir.
- Uygulama hizmeti bağlantısı üzerinden tüm iletişim özellikleri, gibi [karma bağlantı](app-service-hybrid-connections.md), şifrelenir. 
- Azure PowerShell, Azure CLI, Azure SDK'ları, REST API'leri gibi uzak yönetim araçlarıyla bağlantıları tüm şifrelenir.
- Yönetim korur altyapı ve platform kötü amaçlı yazılımlardan, 24 saatlik tehdit dağıtılmış hizmet engelleme (DDoS) man-in--middle (MITM) ve diğer tehditlere.

Altyapı ve platform güvenliği hakkında daha fazla bilgi için bkz: [Azure Güven Merkezi](https://azure.microsoft.com/overview/trusted-cloud/).

Aşağıdaki bölümlerde daha fazla uygulama hizmeti uygulamanızı tehditlere karşı korumak nasıl gösterir.

## <a name="https-and-certificates"></a>HTTPS ve sertifikaları

Uygulama hizmeti uygulamalarınızı ile güvenliğini sağlamanıza olanak tanır [HTTPS](https://wikipedia.org/wiki/HTTPS). Uygulamanızı oluşturulduğunda, varsayılan etki alanı adını (\<app_name >. azurewebsites.net) zaten HTTPS kullanılarak erişilebilir. Varsa, [uygulamanız için özel bir etki alanı yapılandırmak](app-service-web-tutorial-custom-domain.md), ayrıca gerekir [özel bir sertifikayla güvenli](app-service-web-tutorial-custom-ssl.md) böylece istemci tarayıcıları özel etki alanınız için güvenli HTTPS bağlantıları yapabilirsiniz. Bunu yapmanın iki yolu vardır:

- **Uygulama hizmeti sertifika** -doğrudan Azure'da bir sertifika oluşturun. ' De sertifika güvende [Azure anahtar kasası](/azure/key-vault/)ve uygulama hizmeti uygulamanızı aktarılabilir. Daha fazla bilgi için bkz: [satın alın ve Azure uygulama hizmetiniz için bir SSL sertifikası yapılandırma](web-sites-purchase-ssl-web-site.md).
- **Üçüncü taraf sertifika** - bir güvenilir sertifika yetkilisinden satın alınmış özel bir SSL sertifikasını yükleyin ve uygulama hizmeti uygulamanızı bağlayın. Uygulama hizmeti tek etki alanlı sertifikaları ve joker karakterli sertifikalar destekler. Ayrıca, test amacıyla otomatik olarak imzalanan sertifikalar da destekler. Daha fazla bilgi için bkz: [Azure Web uygulamaları için var olan özel bir SSL sertifikası bağlama](app-service-web-tutorial-custom-ssl.md).

## <a name="insecure-protocols-http-tls-10-ftp"></a>Güvenli olmayan protokoller (HTTP, TLS 1.0, FTP)

Uygulamanızı tüm şifrelenmemiş (HTTP) bağlantıları karşı güvenliğini sağlamak için uygulama hizmeti HTTPS zorlamak için tek tıklamayla yapılandırma sağlar. Güvenli olmayan istekleri bile uygulama kodunuz ulaşmadan hemen etkinleştirilir. Daha fazla bilgi için bkz: [zorunlu HTTPS](app-service-web-tutorial-custom-ssl.md#enforce-https).

[TLS](https://wikipedia.org/wiki/Transport_Layer_Security) 1.0 artık değerlendirilir güvenli endüstri standartları tarafından gibi [PCI DSS](https://wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard). App Service, eski protokolleri tarafından devre dışı bırakmanıza olanak sağlar [TLS 1.1/1.2 zorlamayı](app-service-web-tutorial-custom-ssl.md#enforce-tls-1112).

Uygulama hizmeti, dosyalarınızı dağıtmak için FTP ve FTPS destekler. Ancak, FTPS FTP yerine mümkünse kullanılmalıdır. Birine veya ikisine de bu protokollerin kullanımda olmadığında, aşağıdakileri yapmalısınız [bunları devre dışı](app-service-deploy-ftp.md#enforce-ftps).

## <a name="static-ip-restrictions"></a>Statik IP kısıtlamaları

Varsayılan olarak, uygulama hizmeti uygulamanızı Internet tüm IP adreslerinden gelen istekleri kabul eder, ancak küçük bir alt IP adreslerinin bu erişimi sınırlandırabilirsiniz. Windows uygulama hizmeti uygulamanızı erişmesine izin verilen IP adreslerinin listesi tanımlamanıza olanak sağlar. İzin verilenler tek tek IP adresleri içerebilir veya bir IP adresi aralığı bir alt ağ maskesi kullanılarak tanımlanmış. Daha fazla bilgi için bkz: [Azure uygulama hizmeti statik IP kısıtlamaları](app-service-ip-restrictions.md).

Windows uygulama hizmeti için de IP adreslerini dinamik olarak yapılandırarak kısıtlayabilirsiniz _web.config_. Daha fazla bilgi için bkz: [dinamik IP Güvenlik <dynamicIpSecurity> ](https://docs.microsoft.com/iis/configuration/system.webServer/security/dynamicIpSecurity/).

## <a name="client-authentication-and-authorization"></a>İstemci kimlik doğrulaması ve yetkilendirme

Azure uygulama hizmeti anahtar teslim kimlik doğrulama ve yetkilendirme fazla kullanıcı ya da istemci uygulamaları sağlar. Etkinleştirildiğinde, kullanıcıları ve çok az kayıpla veya hiç uygulama kodu ile istemci uygulamaları'nda oturum açabilir. Kendi kimlik doğrulama ve yetkilendirme çözümü veya bunun yerine, işlemek uygulama hizmeti izin. Kodunuzu düşmeden önce yetkisiz isteklerini reddeder ve bunları devre dışı uygulama kodunuz teslim etmeden önce kimlik doğrulaması ve yetkilendirme modülü web isteklerini işler.

Uygulama hizmeti kimlik doğrulama ve yetkilendirme Azure Active Directory, Microsoft hesapları, Facebook, Google ve Twitter gibi birden çok kimlik doğrulama sağlayıcılarını destekler. Daha fazla bilgi için bkz: [kimlik doğrulama ve yetkilendirme Azure App Service'te](app-service-authentication-overview.md).

## <a name="service-to-service-authentication"></a>Hizmetten hizmete kimlik doğrulaması

Bir arka uç hizmeti karşı kimlik doğrulamasını yaparken, uygulama hizmeti gereksiniminizi bağlı olarak iki farklı mekanizma sağlar:

- **Hizmet kimlik** -uygulama kimliğini kullanarak uzak kaynak oturum açın. Uygulama hizmeti kolayca oluşturmanıza olanak tanır bir [yönetilen hizmet kimliği](app-service-managed-service-identity.md), diğer hizmetlerle gibi kimliğini doğrulamak için kullanabileceğiniz [Azure SQL veritabanı](/azure/sql-database/) veya [Azure anahtar kasası](/azure/key-vault/). Bu yaklaşımın bir uçtan uca öğretici için bkz: [uygulama kullanarak hizmeti arasında güvenli Azure SQL veritabanı bağlantı yönetilen hizmet kimliği](app-service-web-tutorial-connect-msi.md).
- **Üzerinde-adına-of (OBO)** -atanmış erişim uzak kaynaklara kullanıcı adına olun. Azure Active Directory ile kimlik doğrulama sağlayıcısı olarak, uygulama hizmeti uygulamanızı temsilci açma uzak hizmet gerçekleştirebilirler [Azure Active Directory grafik API'si](../active-directory/develop/active-directory-graph-api.md) veya uzak bir API uygulaması App Service'te. Bu yaklaşımın bir uçtan uca öğretici için bkz: [kimlik doğrulama ve kullanıcıların uçtan uca Azure App Service'te yetkilendirmek](app-service-web-tutorial-auth-aad.md).

## <a name="connectivity-to-remote-resources"></a>Bağlantı uzak kaynaklar

Uzak kaynaklara erişmek için uygulamanız gerekebilir üç tür vardır: 

- [Azure kaynakları](#azure-resources)
- [Bir Azure sanal ağ içindeki kaynakların](#resources-inside-an-azure-virtual-network)
- [Şirket içi kaynakları](#on-premises-resources)

Bu durumların tümünde App Service, güvenli bağlantı kurmak bir yol sağlar, ancak en iyi güvenlik uygulamaları hala gözlemlemek. Örneğin, arka uç kaynak şifrelenmemiş bağlantılara izin veren olsa bile her zaman şifreli bağlantıları kullanın. Ayrıca, arka uç Azure hizmeti en düşük IP adresleri kümesini izin verdiğinden emin olun. Uygulamanızı giden IP adreslerini bulabilirsiniz [gelen ve giden IP adreslerini Azure App Service'te](app-service-ip-addresses.md).

### <a name="azure-resources"></a>Azure kaynakları

Uygulamanızı bağlandığında Azure kaynaklarına gibi [SQL veritabanı](/services/sql-database/) ve [Azure Storage](/azure/storage/), bağlantı Azure içinde kalır ve tüm ağ sınırlar arası değil. Ancak Azure paylaşılan ağ bağlantısı geçer, böylece her zaman bağlantınızı şifrelendiğinden emin olun. 

Uygulamanızı barındırılıyorsa bir [uygulama hizmeti ortamı](environment/intro.md), aşağıdakileri yapmalısınız [desteklenen Sanal Ağ Hizmeti uç noktalarını kullanarak Azure hizmetlerine bağlanmak](../virtual-network/virtual-network-service-endpoints-overview.md).

### <a name="resources-inside-an-azure-virtual-network"></a>Bir Azure sanal ağ içindeki kaynakların

Uygulamanızı kaynaklara erişebilir bir [Azure Virtual Network](/azure/virtual-network/) aracılığıyla [sanal ağ tümleştirmesinin](web-sites-integrate-with-vnet.md). Tümleştirme noktadan siteye VPN kullanarak sanal ağ ile kurulur. Uygulama, ardından özel IP adresleri kullanarak sanal ağ kaynaklarına erişim sağlayabilir. Noktadan siteye bağlantı ancak, yine Azure paylaşılan ağlarda çapraz geçiş yapıyorsa. 

Kaynak bağlantınızı azure'da tamamen paylaşılan ağlardan yalıtmak için uygulamanızı oluşturun bir [uygulama hizmeti ortamı](environment/intro.md). Uygulama hizmeti ortamı her zaman ayrılmış bir sanal ağ için Dağıtılmış olduğundan, bağlantı uygulama ve sanal ağ içindeki kaynaklar arasında tam olarak ayrı tutulur. Başka bir uygulama hizmeti ortamında ağ güvenlik konuları için bkz: [ağ yalıtımı](#network-isolation).

### <a name="on-premises-resources"></a>Şirket içi kaynakları

Üç yolla veritabanları gibi şirket kaynaklarına güvenli şekilde erişebilir: 

- [Karma bağlantılar](app-service-hybrid-connections.md) -uzak kaynağınız TCP tüneli üzerinden Noktadan noktaya bağlantı kurar. TCP tünel, TLS 1.2 paylaşılan erişim imzası (SAS) anahtarları kullanılarak oluşturulur.
- [Sanal ağ tümleştirmesinin](web-sites-integrate-with-vnet.md) - açıklandığı gibi siteden siteye VPN ile [bir Azure sanal ağ içindeki kaynakların](#resources-inside-an-azure-virtual-network), ancak sanal ağ şirket ağınıza bağlı bir [ siteden siteye VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md). Bu ağ topolojisinde uygulamanızı sanal ağınızdaki diğer kaynaklara gibi şirket kaynaklarına bağlanabilir.
- [Uygulama hizmeti ortamı](environment/intro.md) - açıklandığı gibi siteden siteye VPN ile [bir Azure sanal ağ içindeki kaynakların](#resources-inside-an-azure-virtual-network), ancak sanal ağ şirket ağınıza bağlı bir [siteden siteye VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md). Bu ağ topolojisinde uygulamanızı sanal ağınızdaki diğer kaynaklara gibi şirket kaynaklarına bağlanabilir.

## <a name="application-secrets"></a>Uygulama parolaları

Veritabanı kimlik bilgileri, API belirteçleri ve özel anahtarlar gibi uygulama gizli kod veya yapılandırma dosyalarınızda depolamayın. Bunları olarak erişmek için yaygın olarak kabul edilen yaklaşımdır [ortam değişkenleri](https://wikipedia.org/wiki/Environment_variable) seçim dilinizde standart desenini kullanarak. Ortam değişkenleri tanımlamak için yol aracılığıyladır App Service'te [uygulama ayarları](web-sites-configure.md#app-settings) (ve .NET uygulamaları için özellikle [bağlantı dizeleri](web-sites-configure.md#connection-strings)). Uygulama ayarlarının ve bağlantı dizelerinin Azure'da şifrelenmiş olarak depolanır ve bunlar uygulama başlatıldığında yalnızca uygulamanızın işlem belleğe eklenmiş önce şifresi. Şifreleme anahtarlarını düzenli olarak döndürülür.

Alternatif olarak, uygulama hizmeti uygulamanızı ile tümleştirebilirsiniz [Azure anahtar kasası](/azure/key-vault/) Gelişmiş gizli yönetimi. Tarafından [bir yönetilen hizmet kimliği içeren anahtar kasası erişim](../key-vault/tutorial-web-application-keyvault.md), uygulama hizmeti uygulamanızı ihtiyacınız gizli anahtarları güvenli şekilde erişebilir.

## <a name="network-isolation"></a>Ağ yalıtımı

Dışında **Isolated** fiyatlandırma katmanı, tüm katmanlar uygulamalarınızı App Service paylaşılan ağ altyapısında çalıştırın. Örneğin, genel IP adresleri ve ön uç yük Dengeleyiciler diğer kiracılar ile paylaşılır. **Isolated** katmanı size tam ağ yalıtımı uygulamalarınızı ayrılmış bir içinde çalıştırarak [uygulama hizmeti ortamı](environment/intro.md). Uygulama hizmeti ortamı kendi örneğinde çalışan [Azure Virtual Network](/azure/virtual-network/). Bu, olanak sağlar: 

- Ağ erişimi kısıtlamak [ağ güvenlik grubu](../virtual-network/virtual-networks-nsg.md). 
- Uygulamalarınızı bir adanmış ortak uç noktası aracılığıyla adanmış ön uçlar ile hizmet.
- Azure sanal ağınız içinde yalnızca erişime izin veren bir iç yük dengeleyici (ILB) kullanarak iç uygulama hizmet. ILB internet'ten uygulamalarınızın toplam yalıtımı sağlar, özel alt ağdan bir IP adresi vardır.
- [Bir web uygulaması Güvenlik Duvarı (WAF) arkasındaki bir ILB kullanmak](environment/integrate-with-application-gateway.md). WAF DDoS koruması, URI filtreleme ve SQL ekleme önleme gibi genel kullanıma yönelik uygulamalarınızı Kurumsal düzeyde koruma sunar.

Daha fazla bilgi için bkz: [Azure App Service ortamları giriş](environment/intro.md).