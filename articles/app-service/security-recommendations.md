---
title: Azure App Service için güvenlik önerileri
description: Azure App Service için güvenlik önerileri. Bu önerileri uygulayan yardım edecek ve hizmetlerimizi paylaşılan sorumluluk modeli içinde açıklandığı gibi güvenlik yükümlülüklerinizi yerine web uygulaması çözümleriniz için genel güvenliği artırır.
services: app-service
author: barclayn
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 06/17/2019
ms.author: barclayn
ms.custom: security-recommendations
ms.openlocfilehash: 53cd2b1dde1158a1c46f798e57911dad110dc87e
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67718257"
---
# <a name="security-recommendations-for-app-service"></a>App Service için güvenlik önerileri

Bu makale, Azure App Service için güvenlik önerileri içerir. Bu önerileri uygulayan yardım edecek ve hizmetlerimizi paylaşılan sorumluluk modeli içinde açıklandığı gibi güvenlik yükümlülüklerinizi yerine Web uygulaması çözümleriniz için genel güvenliği artırır. Hangi Microsoft hakkında daha fazla bilgi hizmet sağlayıcısı sorumlulukları karşılamak için için okuma [Azure altyapı güvenlik](../security/azure-security-infrastructure.md).

## <a name="general"></a>Genel

| Öneri | Açıklamalar |
|-|-|----|
| Güncel olarak takip edin | Desteklenen platformlar, programlama dilleri, protokoller ve çerçeveleri en son sürümlerini kullanın. |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Öneri | Açıklamalar |
|-|----|
| Adsız erişim devre dışı bırak | Anonim istekler desteklemek gerekli olmadıkça adsız erişim devre dışı bırakın. Azure App Service kimlik doğrulama seçenekleri hakkında daha fazla bilgi için bkz. [kimlik doğrulama ve yetkilendirme Azure App Service'te](overview-authentication-authorization.md).|
| Kimlik doğrulaması gerektir | Mümkün olduğunda, App Service kimlik doğrulama modülü kod yazmak yerine kimlik doğrulaması ve yetkilendirme işlemek için kullanın. Bkz: [kimlik doğrulama ve yetkilendirme Azure App Service'te](overview-authentication-authorization.md). |
| Kimliği doğrulanmış erişimi olan arka uç kaynakları koruma | Kullanıcının kimliğini kullanın veya bir arka uç kaynağa kimlik doğrulaması için bir uygulama kimliği kullanın. Bir uygulama kimliği kullanmayı seçtiğinizde bir [yönetilen kimliği](overview-managed-identity.md).
| İstemci sertifikası kimlik doğrulaması gerektir | İstemci sertifikası kimlik doğrulamasını sağlayan sertifikaları kullanarak kimlik doğrulaması istemcilerden bağlantılar yalnızca vererek güvenliği artırır. |

## <a name="data-protection"></a>Veri koruma

| Öneri | Açıklamalar |
|-|-|
| HTTP'yi HTTPS'ye yönlendirme | Varsayılan olarak, istemciler, hem HTTP veya HTTPS kullanarak web Apps'e bağlanabilir. HTTP, HTTPS hem de güvenli bir bağlantı sağlamak için SSL/TLS protokolü kullandığından, HTTPs için yeniden yönlendirme şifrelenir ve kimliği doğrulanmış öneririz. |
| Azure kaynaklarına iletişimi şifrelemek | Uygulamanızı bağlandığında, Azure kaynakları için gibi [SQL veritabanı](https://azure.microsoft.com/services/sql-database/) veya [Azure depolama](/azure/storage/), bağlantı Azure'da kalır. Azure paylaşılan ağ bağlantısı geçer olduğundan, her zaman tüm iletişimi şifrelemek. |
| Olası en son TLS sürümünü zorunlu tut | 2018'den itibaren TLS 1.2 yeni Azure App Service uygulamalarını kullanın. Daha yeni sürümleri eski protokol sürümleri üzerinde güvenlik geliştirmeleri içerir. |
| FTPS kullanın | App Service, dosyalarınızı dağıtmak için FTP ve FTPS hem destekler. FTPS FTP yerine mümkün olduğunca kullanın. Biri veya ikisi bu protokolleri, kullanımda olmadığında, aşağıdakileri yapmalısınız [bunları devre dışı](deploy-ftp.md#enforce-ftps). |
| Uygulama verilerinin güvenliğini sağlama | Veritabanı kimlik bilgileri, API belirteçleri veya özel anahtarlar gibi uygulama gizli kod veya yapılandırma dosyalarınızda depolamayın. Olarak erişmek için yaygın olarak kabul edilen yaklaşımdır [ortam değişkenlerini](https://wikipedia.org/wiki/Environment_variable) standart deseni kullanarak istediğiniz dilde. Azure App Service'te, ortam değişkenleri ile tanımlayabilirsiniz [uygulama ayarları](web-sites-configure.md) ve [bağlantı dizeleri](web-sites-configure.md). Uygulama ayarlarının ve bağlantı dizelerinin Azure'da şifrelenmiş olarak depolanır. Uygulama ayarları, uygulama başlatıldığında yalnızca uygulamanızın işlem belleğe eklenmiş önce şifresi çözülür. Şifreleme anahtarlarını düzenli olarak döndürülür. Alternatif olarak, Azure App Service uygulamanızı facebook veya [Azure anahtar kasası](/azure/key-vault/) Gelişmiş gizli dizileri yönetimi. Tarafından [yönetilen bir kimlikle Key Vault'a erişme](../key-vault/tutorial-web-application-keyvault.md), App Service uygulamanızı ihtiyacınız gizli dizileri güvenli bir şekilde erişebilir. |

## <a name="networking"></a>Ağ

| Öneri | Açıklamalar |
|-|-|
| Statik IP kısıtlamaları kullan | Windows üzerinde Azure App Service, uygulamanıza erişmek için izin verilen IP adreslerinin bir listesini tanımlamanıza olanak sağlar. Bir IP adresi aralığı bir alt ağ maskesi tarafından tanımlanan veya izin verilenler tek tek IP adresleri içerebilir. Daha fazla bilgi için [Azure uygulama hizmeti statik IP kısıtlamaları](app-service-ip-restrictions.md).  |
| Yalıtılmış fiyatlandırma katmanını kullanın | Yalıtılmış fiyatlandırma katmanı hariç tüm katmanlar uygulamalarınızı Azure App Service paylaşılan ağ altyapısında çalıştırın. Yalıtılmış katmanı tam ağ yalıtımı, uygulamalarınız içinde ayrılmış bir çalıştırarak size [App Service ortamı](environment/intro.md). App Service ortamı içinde kendi örneğini çalıştıran [Azure sanal ağı](/azure/virtual-network/).|
| Erişim şirket içi kaynaklara güvenli bağlantılar kullanın | Kullanabileceğiniz [karma bağlantılar](app-service-hybrid-connections.md), [sanal ağ tümleştirmesi](web-sites-integrate-with-vnet.md), veya [App Service ortamının](environment/intro.md) şirket kaynaklarına bağlanmak için. |
| Gelen ağ trafiğini maruz kalma riskinizi sınırı | Ağ güvenlik grupları ağ erişimi kısıtlayabilir ve ortaya çıkarılan uç noktalar sayısını kontrol sağlar. Daha fazla bilgi için [nasıl için gelen trafiği denetlemek için bir App Service ortamı](environment/app-service-app-service-environment-control-inbound-traffic.md). |

## <a name="monitoring"></a>İzleme

| Öneri | Açıklamalar |
|-|-|
|Azure Güvenlik Merkezi'ni kullanarak standart katman | [Azure Güvenlik Merkezi](../security-center/security-center-app-services.md) yerel olarak Azure App Service ile tümleşiktir. Bu değerlendirme çalıştırabilir ve güvenlik önerileri sağlamak. |

## <a name="next-steps"></a>Sonraki adımlar

Ek güvenlik gereksinimleri olup olmadığını görmek için uygulama sağlayıcınıza başvurun. Güvenli uygulamalar geliştirme hakkında daha fazla bilgi için bkz. [güvenli geliştirme belgeleri](../security/abstract-develop-secure-apps.md).
