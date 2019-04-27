---
title: Güvenliğe genel bakış - Azure App Service | Microsoft Docs
description: App Service güvenli yardımları hakkında uygulamanızı ve nasıl daha fazla uygulamanızı tehditlerden kilitleyebilir öğrenin.
keywords: Azure app service, web uygulaması, mobil uygulama, API uygulaması, işlev uygulaması, güvenlik, güvenli, güvenli, uyumluluk, uyumlu, sertifika, sertifika, https, ftps, tls, güven, şifreleme, şifreleme, şifrelenmiş, IP kısıtlaması, kimlik doğrulaması, yetkilendirme, authn, autho MSI, yönetilen hizmet kimliği, yönetilen kimlik, gizli anahtarları, gizli, düzeltme eki uygulama, düzeltme eki, düzeltme ekleri, sürüm, yalıtım, ağ yalıtımı, ddos, mıtm
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
ms.date: 08/24/2018
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 1e4feaed9f4e8f6dd3275da25e33e57197731572
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60838969"
---
# <a name="security-in-azure-app-service"></a>Azure App Service'te güvenlik

Bu makalede gösterilmektedir [Azure App Service](overview.md) yardımcı olur, web uygulaması, mobil uygulama arka ucu, API uygulaması, güvenli ve [işlev uygulaması](/azure/azure-functions/). Ayrıca, daha fazla yerleşik App Service özelliklerini uygulamanızla güvenliğini sağlamak gösterir.

Azure sanal makineleri, depolama, ağ bağlantıları, web çerçeveleri, yönetim ve tümleştirme özellikleri dahil olmak üzere App Service platformu bileşenleri etkin olarak güvenli ve sağlamlaştırılmış. App Service Canlı uyumluluk denetimleri emin olmak için sürekli olarak geçer:

- Uygulama kaynaklarınız [güvenli](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) diğer müşterilerin Azure kaynaklarından.
- [Sanal makine örnekleri ve çalışma zamanı yazılım düzenli olarak güncelleştirilen](overview-patch-os-runtime.md) adresi yeni bulunan güvenlik açıklarına. 
- Gizli anahtarları (örneğin, bağlantı dizeleri), uygulama ve diğer Azure kaynakları arasında iletişimi (gibi [SQL veritabanı](https://azure.microsoft.com/services/sql-database/)) Azure içinde kalır ve ağ sınırları çapraz değil. Gizli dizileri, depolandığında her zaman şifrelenir.
- App Service bağlantısı üzerinden tüm iletişimi özellikleri, aşağıdaki gibi [karma bağlantı](app-service-hybrid-connections.md), şifrelenir. 
- Azure PowerShell, Azure CLI, Azure SDK, REST API'leri gibi uzak yönetim araçları bağlantıları tüm şifrelenir.
- 24 saatlik tehdit Yönetimi korur altyapı ve platform kötü amaçlı yazılımlardan, dağıtılmış hizmet engelleme (DDoS) adam-de-adam (MITM) ve diğer tehditlerden.

Azure'da altyapı ve platform güvenliği hakkında daha fazla bilgi için bkz. [Azure Güven Merkezi](https://azure.microsoft.com/overview/trusted-cloud/).

Aşağıdaki bölümlerde, App Service uygulamanızı tehditlere karşı daha iyi korumak nasıl gösterir.

## <a name="https-and-certificates"></a>HTTPS'yi ve sertifikaları

App Service ile uygulamalarınızı güvenli hale getirmenize olanak tanır [HTTPS](https://wikipedia.org/wiki/HTTPS). Uygulamanızı oluşturulduğunda, varsayılan etki alanı adını (\<app_name >. azurewebsites.net) zaten HTTPS kullanılarak erişilebilir. Varsa, [uygulamanız için özel bir etki alanı yapılandırma](app-service-web-tutorial-custom-domain.md), da [özel bir sertifika ile güvenli](app-service-web-tutorial-custom-ssl.md) yapabilmesi istemci tarayıcıları özel etki alanınıza güvenli HTTPS bağlantıları. Bunu yapmanın iki yolu vardır:

- **App Service sertifikası** -doğrudan Azure'da bir sertifika oluşturun. ' De sertifika güvende [Azure anahtar kasası](/azure/key-vault/)ve App Service uygulamanıza içeri aktarılabilir. Daha fazla bilgi için [satın alma ve Azure App Service için SSL sertifikası yapılandırma](web-sites-purchase-ssl-web-site.md).
- **Üçüncü taraf sertifika** - bir güvenilir sertifika yetkilisinden satın alınmış özel bir SSL sertifikasını karşıya yükleyin ve App Service uygulamanızı bağlayın. App Service, hem tek etki alanı sertifikaları hem de joker karakterli sertifikalar destekler. Ayrıca, test amacıyla otomatik olarak imzalanan sertifikaları da destekler. Daha fazla bilgi için [mevcut bir özel SSL sertifikasını Azure App Service'e bağlama](app-service-web-tutorial-custom-ssl.md).

## <a name="insecure-protocols-http-tls-10-ftp"></a>Güvenli olmayan protokoller (HTTP, TLS 1.0, FTP)

Uygulamanızı tüm şifrelenmemiş (HTTP) bağlantıları karşı güvenli hale getirmek için App Service HTTPS zorlama için tek tıklamayla yapılandırmasını sağlar. Uygulama kodunuza bile ulaşmadan önce güvenli olmayan istekleri yerine açık olabilir. Daha fazla bilgi için [HTTPS zorlama](app-service-web-tutorial-custom-ssl.md#enforce-https).

[TLS](https://wikipedia.org/wiki/Transport_Layer_Security) 1.0 artık kabul güvenli sektör standartlarıyla gibi [PCI DSS](https://wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard). App Service, eski protokollere devre dışı bırakmanıza olanak tanır [TLS 1.1/1.2 zorlama](app-service-web-tutorial-custom-ssl.md#enforce-tls-versions).

App Service, dosyalarınızı dağıtmak için FTP ve FTPS hem destekler. Ancak, FTPS FTP yerine bu tamamen mümkünse kullanılmalıdır. Biri veya ikisi bu protokolleri, kullanımda olmadığında, aşağıdakileri yapmalısınız [bunları devre dışı](deploy-ftp.md#enforce-ftps).

## <a name="static-ip-restrictions"></a>Statik IP kısıtlamaları

Varsayılan olarak, App Service uygulamanızı tüm IP adresleri, internet'ten gelen istekleri kabul eder, ancak bu IP adreslerinden oluşan küçük bir alt kümesine erişimi sınırlayabilirsiniz. Windows üzerinde App Service, uygulamanıza erişmek için izin verilen IP adreslerinin bir listesini tanımlamanıza olanak sağlar. Bir IP adresi aralığı bir alt ağ maskesi tarafından tanımlanan veya izin verilenler tek tek IP adresleri içerebilir. Daha fazla bilgi için [Azure uygulama hizmeti statik IP kısıtlamaları](app-service-ip-restrictions.md).

Windows üzerinde App Service için da IP adresleri dinamik olarak yapılandırarak kısıtlayabilirsiniz _web.config_. Daha fazla bilgi için [dinamik IP Güvenlik \<dynamicIpSecurity >](https://docs.microsoft.com/iis/configuration/system.webServer/security/dynamicIpSecurity/).

## <a name="client-authentication-and-authorization"></a>İstemci kimlik doğrulaması ve yetkilendirme

Azure App Service, kullanıma hazır kimlik doğrulaması ve yetkilendirme kullanıcı veya istemci uygulamalar sağlar. Etkinleştirildiğinde, kullanıcılar ve istemci uygulamalarını çok az kayıpla veya hiç uygulama kodu ile oturum açabilirsiniz. Kendi kimlik doğrulama ve yetkilendirme çözümü veya App Service, bunun yerine, işlemek izin verin. Kimlik doğrulama ve yetkilendirme modülü bunları devre dışı uygulama kodunuzu teslim etmeden önce web isteklerini işleyen ve kodunuzu ulaşmadan önce yetkisiz istekleri reddeder.

App Service kimlik doğrulaması ve yetkilendirme, Azure Active Directory, Microsoft hesapları, Facebook, Google ve Twitter gibi birden çok kimlik doğrulama sağlayıcılarını destekler. Daha fazla bilgi için [kimlik doğrulama ve yetkilendirme Azure App Service'te](overview-authentication-authorization.md).

## <a name="service-to-service-authentication"></a>Hizmetten hizmete kimlik doğrulaması

App Service, bir arka uç hizmetine karşı kimlik doğrulamasını yaparken, gereksinimlerinize bağlı olarak iki farklı mekanizma sağlar:

- **Hizmet kimlik** -uzak kaynağa uygulamanın kendi kimliğini kullanarak oturum açın. App Service kolayca oluşturmanıza olanak tanıyan bir [yönetilen kimliği](overview-managed-identity.md), aşağıdakiler gibi diğer hizmetleri ile kimlik doğrulaması yapmak için kullanabileceğiniz [Azure SQL veritabanı](/azure/sql-database/) veya [Azure anahtar kasası](/azure/key-vault/). Bu yaklaşımın bir uçtan uca öğretici için bkz [bir yönetilen kimlik kullanarak App Service'ten Azure SQL veritabanı'nı güvenli bağlantı](app-service-web-tutorial-connect-msi.md).
- **On-behalf-of (OBO)** -Temsilcili erişim kullanıcının adına uzak kaynaklara olun. Azure Active Directory ile kimlik doğrulama sağlayıcısı olarak, App Service uygulamanızı bir uzak hizmete temsilci oturum açma gibi gerçekleştirebilirsiniz [Azure Active Directory Graph API'si](../active-directory/develop/active-directory-graph-api.md) veya App Service'te bir Uzak API uygulaması. Bu yaklaşımın bir uçtan uca öğretici için bkz [kimlik doğrulama ve kullanıcıları uçtan uca Azure App Service'te yetkilendirme](app-service-web-tutorial-auth-aad.md).

## <a name="connectivity-to-remote-resources"></a>Bağlantı uzak kaynaklar

Uzak kaynaklara erişmek için uygulamanızı gerekebilir üç tür vardır: 

- [Azure kaynakları](#azure-resources)
- [Bir Azure sanal ağ içindeki kaynakları](#resources-inside-an-azure-virtual-network)
- [Şirket içi kaynaklara](#on-premises-resources)

Her durumda, App Service, güvenli bağlantılar kurmak bir yol sağlar, ancak yine de en iyi güvenlik uygulamaları gözlemleyin. Örneğin, arka uç kaynağa şifrelenmemiş bağlantılarına izin veren olsa bile her zaman şifreli bağlantıları kullanın. Ayrıca, arka uçta Azure hizmeti en düşük IP adresleri kümesini verdiğinden emin olun. Giden IP adresleri için uygulamanızı bulabileceğinizi [gelen ve giden IP adresleri, Azure App Service'te](overview-inbound-outbound-ips.md).

### <a name="azure-resources"></a>Azure kaynakları

Uygulamanızı bağlandığında, Azure kaynakları için gibi [SQL veritabanı](https://azure.microsoft.com/services/sql-database/) ve [Azure depolama](/azure/storage/), bağlantı, Azure içinde kalır ve ağ sınırları çapraz değil. Ancak, azure'da paylaşılan ağ bağlantısı geçer, böylece her zaman bağlantınız şifrelenir emin olun. 

Uygulamanızın içinde barındırılıyorsa bir [App Service ortamı](environment/intro.md), aşağıdakileri yapmalısınız [desteklenen sanal ağ hizmet uç noktaları kullanarak Azure hizmetlerine bağlanın](../virtual-network/virtual-network-service-endpoints-overview.md).

### <a name="resources-inside-an-azure-virtual-network"></a>Bir Azure sanal ağ içindeki kaynakları

Uygulamanızı kaynaklara erişebilir bir [Azure sanal ağı](/azure/virtual-network/) aracılığıyla [sanal ağ tümleştirmesi](web-sites-integrate-with-vnet.md). Noktadan siteye VPN kullanarak sanal ağ ile tümleştirme kurulur. Uygulama, daha sonra özel IP adreslerini kullanarak sanal ağ içindeki kaynaklara erişebilirsiniz. Noktadan siteye bağlantı ancak yine de azure'da paylaşılan ağları erişir. 

Kaynak bağlantısı tamamen azure'da paylaşılan ağları yalıtmak için uygulamanızı oluşturmak bir [App Service ortamı](environment/intro.md). App Service ortamı her zaman ayrılmış bir sanal ağın dağıtılmış olduğundan, uygulama ve sanal ağ içindeki kaynaklar arasında bağlantı tamamen yalıtılır. App Service ortamı ağ güvenliği diğer yönleri için bkz: [ağ yalıtımı](#network-isolation).

### <a name="on-premises-resources"></a>Şirket içi kaynaklara

Üç yolla veritabanları gibi şirket içi kaynaklara güvenli bir şekilde erişebilir: 

- [Karma bağlantılar](app-service-hybrid-connections.md) -uzak kaynağınız TCP tüneli üzerinden Noktadan noktaya bağlantı kurar. TCP tüneli, TLS 1.2 paylaşılan erişim imzası (SAS) anahtarı kullanılarak oluşturulur.
- [Sanal ağ tümleştirmesi](web-sites-integrate-with-vnet.md) - açıklandığı gibi siteden siteye VPN ile [bir Azure sanal ağ içindeki kaynakları](#resources-inside-an-azure-virtual-network), ancak sanal ağ ile şirket içi ağınıza bağlı bir [ siteden siteye VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md). Bu ağ topolojisinde, uygulamanız, sanal ağdaki diğer kaynaklar gibi şirket içi kaynaklara bağlanabilir.
- [App Service ortamı](environment/intro.md) - açıklandığı gibi siteden siteye VPN ile [bir Azure sanal ağ içindeki kaynakları](#resources-inside-an-azure-virtual-network), ancak sanal ağ ile şirket içi ağınıza bağlı bir [siteden siteye VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md). Bu ağ topolojisinde, uygulamanız, sanal ağdaki diğer kaynaklar gibi şirket içi kaynaklara bağlanabilir.

## <a name="application-secrets"></a>Uygulama gizli dizilerini

Veritabanı kimlik bilgileri, API belirteçleri ve özel anahtarlar gibi uygulama gizli kod veya yapılandırma dosyalarınızda depolamayın. Olarak erişmek için yaygın olarak kabul edilen yaklaşımdır [ortam değişkenlerini](https://wikipedia.org/wiki/Environment_variable) standart deseni kullanarak istediğiniz dilde. App Service ortam değişkenlerini aracılığıyla yoludur [uygulama ayarları](web-sites-configure.md#app-settings) (ve .NET uygulamaları için özellikle [bağlantı dizeleri](web-sites-configure.md#connection-strings)). Uygulama ayarlarının ve bağlantı dizelerinin Azure'da şifrelenmiş olarak depolanır ve bunların uygulama başlatıldığında yalnızca uygulamanızın işlem belleğe eklenmiş önce şifresi. Şifreleme anahtarlarını düzenli olarak döndürülür.

Alternatif olarak, App Service uygulamanızı facebook veya [Azure anahtar kasası](/azure/key-vault/) Gelişmiş gizli dizileri yönetimi. Tarafından [yönetilen bir kimlikle Key Vault'a erişme](../key-vault/tutorial-web-application-keyvault.md), App Service uygulamanızı ihtiyacınız gizli dizileri güvenli bir şekilde erişebilir.

## <a name="network-isolation"></a>Ağ yalıtımı

Dışında **yalıtılmış** fiyatlandırma katmanı, tüm katmanlar, uygulamalarınızı App Service paylaşılan ağ altyapısında çalıştırın. Örneğin, genel IP adresleri ve ön uç yük Dengeleyiciler diğer kiracılar ile paylaşılır. **Yalıtılmış** katmanı size tam ağ yalıtımı, uygulamalarınız içinde ayrılmış bir çalıştırarak [App Service ortamı](environment/intro.md). App Service ortamı içinde kendi örneğini çalıştıran [Azure sanal ağı](/azure/virtual-network/). Olanak tanır: 

- Ağ erişimi kısıtlamak [ağ güvenlik grupları](../virtual-network/virtual-networks-dmz-nsg.md). 
- Uygulamalarınızı bir adanmış genel uç noktası ile özel ön uç işlevi görür.
- Azure sanal ağınız içindeki yalnızca erişime izin veren bir iç yük dengeleyici (ILB) kullanarak iç uygulama işlevi görür. ILB internet'ten uygulamalarınızın toplam yalıtımı sağlar, özel alt ağdan bir IP adresi vardır.
- [Bir web uygulaması Güvenlik Duvarı (WAF) arkasındaki bir ILB kullanın](environment/integrate-with-application-gateway.md). WAF, DDoS koruması, URI filtreleme ve SQL ekleme önleme gibi genel kullanıma yönelik uygulamalarınız için kurumsal düzeyde koruma sunar.

Daha fazla bilgi için [Azure App Service ortamlarına giriş](environment/intro.md). 
