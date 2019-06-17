---
title: Sık sorulan sorular - Azure ayrılmış HSM | Microsoft Docs
description: Sık sorulan sorular Azure ayrılmış HSM üzerinde farklı konuları kapsayan
services: dedicated-hsm
author: johncdawson
manager: barbkess
tags: azure-resource-manager
ms.custom: mvc
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 5/8/2019
ms.author: barclayn
ms.openlocfilehash: b73b6bdc0158591565281ca2e86a9a474c4196d9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65467732"
---
# <a name="frequently-asked-questions-faq"></a>Sık sorulan sorular (SSS)

Microsoft Azure ayrılmış HSM hakkında sık sorulan sorulara yanıtlar bulun.

## <a name="the-basics"></a>Temel bilgileri

### <a name="q-what-is-a-hardware-security-module-hsm"></a>S: Bir donanım güvenlik modülü (HSM) nedir?

Bir donanım güvenlik modülü (HSM) korunmasına ve şifreleme anahtarlarını yönetmek için kullanılan fiziksel bir bilgi işlem cihazıdır. Hsm'lerde depolanan anahtarları şifreleme işlemleri için kullanılabilir. Anahtar malzemesi kurcalamaya karşı korumalı, değiştirmeye donanım modülleri güvenli bir şekilde kalır. HSM yalnızca izin kimliği doğrulanmış ve yetkili anahtar kullanma için uygulamaları. Anahtar malzemeleri asla HSM koruma sınırlarından ayrılmaz.

### <a name="q-what-is-the-azure-dedicated-hsm-offering"></a>S: Azure ayrılmış HSM neler sunar?

Azure ayrılmış HSM doğrudan bir müşterinin sanal ağa bağlı bir Azure veri merkezlerinde bulunan Hsm'lerde sağlayan bir bulut tabanlı bir hizmettir. Bu HSM'ler, adanmış ağ Gereçleri (Gemalto'nın SafeNet ağ HSM 7 modeli A790) sahiptir. Doğrudan bir müşterilerin özel IP adresi alanına dağıtılan ve Microsoft HSM'ler şifreleme işlevselliği için herhangi bir erişimi yok. Yalnızca müşteri bu cihazlar üzerinde tam yönetimsel ve şifreleme denetime sahiptir. Cihaz yönetimi için müşterilerin sorumluluğundadır ve doğrudan cihazlarından tam etkinlik günlüklerini alabilirsiniz. Ayrılmış Hsm'lerin 140-2 Düzey 3, HIPAA, PCI-DSS ve eIDAS ve diğer birçok gibi FIPS uyumluluk/yasal gereksinimleri karşılamak müşterilerin yardımcı olur.

### <a name="q-what-hardware-is-used-for-dedicated-hsm"></a>S: Ayrılmış HSM donanımı kullanılır?

Microsoft Azure ayrılmış HSM hizmeti yerine getirmeleri için Gemalto ile iş ortaklığı kurdu. Kullanılan belirli cihaz [SafeNet Luna ağ HSM 7 modeli A790](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/). Bu cihaz yalnızca FIPS 140-2 Düzey 3'ü doğrulanmış bellenim sağlar, ancak ayrıca düşük gecikme süreli, yüksek performanslı ve yüksek kapasiteli 10 bölümleri aracılığıyla sunar. 

### <a name="q-what-is-an-hsm-used-for"></a>S: HSM ne için kullanılır?

HSM’ler SSL (güvenli yuva katmanı) gibi şifreleme işlevleri, verileri şifreleme, PKI (ortak anahtar altyapısı), DRM (dijital hak yönetimi) ve belgeleri imzalama amacıyla kullanılır.

### <a name="q-how-does-dedicated-hsm-work"></a>S: Ayrılmış HSM nasıl çalışır?

Müşteriler, PowerShell veya komut satırı arabirimi kullanarak belirli bölgelerde HSM'ler sağlayabilirsiniz. Müşteri, HSM bağlı ve bir kez sağladınız hangi sanal ağ Hsm'leri atanmış IP adresleri müşterinin özel IP adres alanı içinde belirtilen alt ağda kullanılabilir olacağını belirtir. Ardından müşteriler HSM'ler Gereci Yönetim için SSH kullanarak bağlanabilirsiniz ve yönetim, HSM istemci bağlantıları kurmak HSM'ler başlatmak, bölümleri oluşturmak, tanımlamak ve gibi bölüm Müdürü, şifreleme Sorumlu Başkan ve şifreleme kullanıcı rolleri atayın. Daha sonra müşteri uygulamalarından şifreleme işlemleri gerçekleştirmesi için HSM istemci araçları/SDK/yazılım sağlanan Gemalto kullanır.

### <a name="q-what-software-is-provided-with-the-dedicated-hsm-service"></a>S: Hangi yazılım ile ayrılmış HSM hizmetini sağlanır?

Gemalto kez Microsoft tarafından sağlanan HSM cihaz için tüm yazılım sağlar. Yazılım kullanılabilir [Gemalto müşteri desteği portalı](https://supportportal.gemalto.com/csm/). Ayrılmış HSM hizmetini kullanan müşteriler Gemalto desteklemek ve erişim ve ilgili yazılım indirilmesini sağlayan bir müşteri kimliği için kayıtlı olması gereklidir. FIPS 140-2 Düzey 3'ü doğrulanmış üretici yazılımı sürümü 7.0.3 uyumlu 7.2 sürüm desteklenen istemci yazılımıdır. 

### <a name="q-does-azure-dedicated-hsm-offer-password-based-and-ped-based-authentication"></a>S: Azure ayrılmış HSM PED tabanlı ve parola tabanlı kimlik doğrulaması sunar?

Şu anda Azure ayrılmış HSM HSM'ler yalnızca parola tabanlı kimlik doğrulaması sağlar.

### <a name="q-will-azure-dedicated-hsm-host-my-hsms-for-me"></a>S: Azure ayrılmış HSM my HSM'ler benim için barındıracak?

Microsoft, yalnızca ayrılmış HSM hizmeti aracılığıyla Gemalto SafeNet Luna ağ HSM sunar ve herhangi bir müşteri tarafından sağlanan cihaza barındıramaz.

### <a name="q-does-azure-dedicated-hsm-support-payment-pinetf-features"></a>S: Azure ayrılmış HSM destek ödeme (PIN/ETF) özellikleri mu?

Azure ayrılmış HSM hizmetini SafeNet Luna ağ HSM 7 (model A790) cihazlar kullanır. Bu cihazlar, ödeme HSM belirli işlevleri (örneğin, PIN veya ETF) veya sertifikaları desteklemez. Lütfen ödeme HSM'ler gelecekte desteklemek üzere Azure ayrılmış HSM hizmetini isterseniz, geribildirim için Microsoft hesap temsilcinize geçirin.

### <a name="q-which-azure-regions-is-dedicated-hsm-available-in"></a>S: Hangi Azure bölgeleri ayrılmış HSM kullanılabilir?

Geç Mart 2019'den itibaren ayrılmış HSM aşağıda listelenen 14 bölgede kullanılabilir. Daha fazla bölge planlanmaktadır ve Microsoft hesap temsilcinize aracılığıyla ele alınması.

* Doğu ABD
* Doğu ABD 2
* Batı ABD
* Orta Güney ABD
* Güneydoğu Asya
* Doğu Asya
* Kuzey Avrupa
* Batı Avrupa
* Birleşik Krallık Güney
* Birleşik Krallık Batı
* Orta Kanada
* Doğu Kanada
* Avustralya Doğu
* Avustralya Güneydoğu

## <a name="interoperability"></a>Birlikte çalışabilirlik

### <a name="q-how-does-my-application-connect-to-a-dedicated-hsm"></a>S: Uygulamam için adanmış bir HSM nasıl bağlanıyor?

HSM istemci araçları/SDK/yazılım sağlanan Gemalto uygulamalarınızdan şifreleme işlemleri gerçekleştirmek için kullanın. Yazılım kullanılabilir [Gemalto müşteri desteği portalı](https://supportportal.gemalto.com/csm/). Ayrılmış HSM hizmetini kullanan müşteriler Gemalto desteklemek ve erişim ve ilgili yazılım indirilmesini sağlayan bir müşteri kimliği için kayıtlı olması gereklidir.

### <a name="q-can-an-application-connect-to-dedicated-hsm-from-a-different-vnet-in-or-across-regions"></a>S: Bir uygulama için ayrılmış HSM içinde veya bölgeler farklı bir sanal AĞA bağlanabilir miyim?

Evet, kullanmanız gerekecektir [VNET eşlemesi](../virtual-network/virtual-network-peering-overview.md) sanal ağlar arasında bağlantı kurmak için bir bölge içinde. Bölgeler arası bağlantı için kullanmalısınız [VPN ağ geçidi](../vpn-gateway/vpn-gateway-about-vpngateways.md).

### <a name="q-can-i-synchronize-dedicated-hsm-with-on-premises-hsms"></a>S: Ayrılmış HSM ile şirket içi HSM'ler eşitleme yapabilirsiniz?

Evet, şirket içi HSM'ler ayrılmış HSM ile eşitleyebilirsiniz. [Noktadan noktaya VPN veya siteden siteye noktası](../vpn-gateway/vpn-gateway-about-vpngateways.md) bağlantısı, şirket içi ağınız ile bağlantı kurmak için kullanılabilir.

### <a name="q-can-i-encrypt-data-used-by-other-azure-services-using-keys-stored-in-dedicated-hsm"></a>S: Ayrılmış HSM içinde depolanan anahtarları kullanarak diğer Azure Hizmetleri tarafından kullanılan verileri şifreleyebilir miyim?

Hayır. Azure ayrılmış HSM'ler yalnızca, sanal ağ içinde erişilebilir.

### <a name="q-can-i-import-keys-from-an-existing-on-premises-hsm-to-dedicated-hsm"></a>S: Ayrılmış HSM için anahtarları mevcut bir şirket içi HSM'NİZDEN alabilirim?

Evet, şirket içi Gemalto SafeNet HSM'ler varsa. Birden fazla yöntem vardır. Gemalto HSM belgelerine başvurun.

### <a name="q-what-operating-systems-are-supported-by-dedicated-hsm-client-software"></a>S: Ayrılmış HSM istemci yazılımı tarafından desteklenen işletim sistemleri?

* Windows, Linux, Solaris, AIX, HP-UX, FreeBSD
* Sanal: VMware, hyperv, Xen, KVM

### <a name="q-how-do-i-configure-my-client-application-to-create-a-high-availability-configuration-with-multiple-partitions-from-multiple-hsms"></a>S: Birden çok bölümdeki verileri birden çok Hsm'niz ile yüksek kullanılabilirlik yapılandırması oluşturmak için istemci uygulamamın nasıl yapılandırabilirim?

Yüksek kullanılabilirlik sağlamak için her HSM bölümleri kullanmak için HSM istemci uygulaması yapılandırması'kurmak gerekir. Gemalto HSM istemci yazılımı belgelerine bakın.

### <a name="q-what-authentication-mechanisms-are-supported-by-dedicated-hsm"></a>S: Ayrılmış HSM tarafından hangi kimlik doğrulaması mekanizmaları desteklenir?

Azure ayrılmış HSM SafeNet ağ HSM 7 cihazları (A790 model) kullanır ve parola tabanlı kimlik doğrulamasını destekler.

### <a name="q-what-sdks-apis-client-software-is-available-to-use-with-dedicated-hsm"></a>S: Ayrılmış HSM ile kullanmak hangi API, SDK'ları istemci yazılımları mevcuttur?

PKCS #11, Java (JCA/JCE), Microsoft CAPI ve CNG, OpenSSL

### <a name="q-can-i-importmigrate-keys-from-luna-56-hsms-to-azure-dedicated-hsms"></a>S: İçeri aktarma/5/6 Luna hsm'lerinizdeki anahtarları Azure ayrılmış HSM'ler için geçişini sağlayabilir miyim?

Evet. Lütfen Gemalto Geçiş Kılavuzu'na bakın. 

## <a name="using-your-hsm"></a>HSM kullanma

### <a name="q-how-do-i-decide-whether-to-use-azure-key-vault-or-azure-dedicated-hsm"></a>S: Azure anahtar kasası veya Azure ayrılmış HSM kullanmak için nasıl karar verebilirim?

Azure ayrılmış HSM HSM'ler kullanan Azure şirket içi uygulamaları geçirme kuruluşlar için uygun bir seçenektir. Ayrılmış Hsm'lerin çok az değişiklikle bir uygulamayı geçirmek için bir seçenek sunar. Bir Azure VM veya Web uygulamasıyla çalışan uygulama kodu şifreleme işlemleri gerçekleştirilir, ayrılmış HSM kullanabilirler. Genel olarak, yazılım HSM'ler anahtarsız SSL, ADCS (Active Directory Sertifika Hizmetleri), uygulama ağ geçidi veya traffic manager gibi ayrılması, HSM anahtar deposu kullanabilirsiniz gibi destekleyen Iaas (hizmet olarak altyapı) modelleri çalışan verdiği veya benzer PKI araçları, Araçlar/uygulamaları belge imzalama, kod imzalama veya bir EKM (Genişletilebilir anahtar yönetimi) sağlayıcısını kullanarak bir hsm'de ana anahtar ile TDE (saydam veritabanı şifrelemesi) ile yapılandırılmış SQL Server (Iaas) için kullanılır. Azure Key Vault "geliştirilen içinde-buluta" uygulamalar için veya şifreleme PaaS (hizmet olarak platform) tarafından Müşteri verilerinin nerede işlenen rest senaryoları veya Office 365 müşteri anahtarı, Azure Information Protection gibi SaaS (hizmet olarak yazılım) senaryoları için uygundur , Azure Disk şifrelemesi, Azure Data Lake Store şifrelemesi müşteri tarafından yönetilen anahtar, Azure depolama şifreleme ile müşteri ile yönetilen anahtar ve müşteri ile Azure SQL yönetilen anahtar.

### <a name="q-what-usage-scenarios-best-suit-azure-dedicated-hsm"></a>S: Hangi kullanım senaryolarını en iyi Azure ayrılmış HSM uygun?

Azure ayrılmış HSM geçiş senaryoları için çok uygundur. HSM'ler zaten kullanmakta olduğunuz şirket içi uygulamaları azure'a taşıyorsanız anlamına gelir. Bu uygulama için çok az değişiklikle Azure'a geçirmek için düşük uyuşmazlıkları seçeneği sağlar. Azure VM veya Web uygulaması çalışan uygulama kodu şifreleme işlemleri gerçekleştirilir, ayrılmış HSM kullanılabilir. Genel olarak, anahtar deposu ayırmanız HSM, gibi kullanabileceğiniz gibi HSM'ler destekleyen Iaas (hizmet olarak altyapı) modellerinde çalışan yazılımı verdiği:

* Uygulama ağ geçidi veya traffic Manager'a anahtarsız SSL
* ADCS (Active Directory Sertifika Hizmetleri)
* Benzer PKI araçları
* Belge imzalama için kullanılan araçları/uygulamalar
* Kod imzalama
* Bir EKM (Genişletilebilir anahtar yönetimi) sağlayıcısını kullanarak bir hsm'de ana anahtar ile TDE (saydam veritabanı şifrelemesi) ile yapılandırılmış SQL Server (Iaas)

### <a name="q-can-dedicated-hsm-be-used-with-office-365-customer-key-azure-information-protection-azure-data-lake-store-disk-encryption-azure-storage-encryption-azure-sql-tde"></a>S: Ayrılmış HSM; Office 365 Müşteri Anahtarı, Azure Information Protection, Azure Data Lake Store, Disk Şifreleme, Azure Depolama şifrelemesi, Azure SQL TDE ile birlikte kullanılabilir mi?

Hayır. Ayrılmış HSM erişilemiyor vermez böylece doğrudan bir müşterinin özel IP adres alanına diğer Microsoft ya da Azure Hizmetleri tarafından sağlanır.

## <a name="administration-access-and-control"></a>Yönetim ve erişim denetimi

### <a name="q-does-the-customer-get-full-exclusive-control-over-the-hsms-with-dedicated-hsms"></a>S: Müşteri ile ayrılmış HSM'ler HSM'ler üzerinde tam özel denetim elde mu?

Evet. Her bir HSM Gereci tam olarak tek bir müşteriye ayrılmış ve başka hiç kimse kez sağlanan yönetimsel denetim sahibi ve Yönetici Parola değiştirildi vardır.

### <a name="q-what-level-of-access-does-microsoft-have-to-my-hsm"></a>S: Microsoft benim HSM'ye erişim düzeyini var mı?

Microsoft, HSM herhangi bir yönetici veya şifreleme denetime sahip değil. Microsoft izleme düzeyi sıcaklık ve bileşen sistem durumu gibi temel telemetri almak için seri bağlantı noktası bağlantısı aracılığıyla erişimi yok. Bu sistem durumu sorunlarının proaktif bildirim sağlamak Microsoft sağlar. Gerekirse, müşteri bu hesabı devre dışı bırakabilirsiniz.

### <a name="q-what-is-the-tenantadmin-account-microsoft-uses-i-am-used-to-the-admin-user-being-admin-on-safenet-hsms"></a>S: "Tenantadmin" Microsoft hesabı nedir kullanır, SafeNet HSM'ler "Yönetici" Yönetici kullanıcı için kullanılan miyim?

HSM cihazını, yönetici, her zamanki varsayılan parola ile bir varsayılan kullanıcı ile birlikte gelir. Microsoft, müşteriler tarafından sağlanacak bekleyen bir havuzdaki tüm cihaz çalışırken varsayılan parolaları kullanımda olan istemedi. Bu bizim katı güvenlik gereksinimleri karşılaması değil. Bu nedenle, atılan güçlü bir parola zaman sağlama sırasında ayarladık. Ayrıca, zaman sağlama sırasında yeni bir kullanıcı yönetici rolünde "tenantadmin" adlı oluştururuz. Bu kullanıcının varsayılan parolası vardır ve müşterilerin bu ilk eylem olarak ilk yeni sağlanan cihazda oturum açarken değiştirin. Bu işlem yüksek derece güvenlik sağlar ve müşterilerimiz için tek yönetim denetimi halidir tutar. "Tenantadmin" kullanıcı bu hesabı kullanmak bir müşteri tercih yönetici kullanıcının parolasını sıfırlamak için kullanılabilir unutulmamalıdır. 

### <a name="q-can-microsoft-or-anyone-at-microsoft-access-keys-in-my-dedicated-hsm"></a>S: Microsoft veya Microsoft'tan herhangi my ayrılmış HSM Microsoft erişim anahtarlarını kullanabilir?

Hayır. Microsoft, müşteri ayrılmış HSM ayrılan depolanan anahtarları için herhangi bir erişimi yok.

### <a name="q-can-i-upgrade-softwarefirmware-on-hsms-allocated-to-me"></a>S: Yazılım/bana ayrılmış HSM'ler bellenimini yükseltebilirim?

En iyi destek almak için Microsoft yazılım / HSM bellenimini yükseltme önerir. Ancak, müşteri belirli özellikleri farklı üretici yazılımı sürümlerini gerekli olduğunda yazılım/bellenimi yükseltmek dahil olmak üzere tam yönetimsel denetime sahip. Değişiklik yapmadan önce bu gibi etkilerini anlaşılması gerekir, örneğin, FIPS etkin durumu doğrulandı. 

### <a name="q-how-do-i-manage-dedicated-hsm"></a>S: Ayrılmış HSM nasıl yönetebilirim?

Bunları erişerek, ayrılmış HSM'ler yönetebilirsiniz SSH kullanarak.

### <a name="q-how-do-i-manage-partitions-on-the-dedicated-hsm"></a>S: Ayrılmış HSM bölümler nasıl yönetebilirim?

Gemalto HSM istemci yazılımı, bölümler ve HSM'ler yönetmek için kullanılır.

### <a name="q-how-do-i-monitor-my-hsm"></a>S: My HSM nasıl izleyebilirim?

Bir müşteri, HSM etkinlik günlükleri syslog ve SNMP aracılığıyla tam erişime sahip olur. Bir müşteri HSM'ler olayları ve günlükleri almak için bir syslog sunucunuza veya SNMP sunucusu ayarlamak gerekir.

### <a name="q-can-i-get-full-access-log-of-all-hsm-operations-from-dedicated-hsm"></a>S: Tüm HSM işlemlerinin tam erişim günlüğüne ayrılmış HSM'den alabilir miyim?

Evet. Bir syslog sunucusuna HSM Gereci günlüklerini gönderebilirsiniz

## <a name="high-availability"></a>Yüksek kullanılabilirlik

### <a name="q-is-it-possible-to-configure-high-availability-in-the-same-region-or-across-multiple-regions"></a>S: Aynı bölgedeki veya birden çok bölgede yüksek kullanılabilirlik yapılandırmak mümkündür?

Evet. Yüksek kullanılabilirlik yapılandırma ve Kurulum Gemalto tarafından sağlanan HSM istemci yazılımının gerçekleştirilir. Aynı sanal AĞDA veya diğer sanal ağlar aynı bölgedeki veya farklı bölgelerdeki veya şirket içi HSM'ler HSM'ler kullanarak siteden siteye bir sanal ağa bağlı veya aynı yüksek kullanılabilirlik yapılandırması için noktadan noktaya VPN eklenebilir. Bu yalnızca anahtar malzemesi ve roller gibi özel olmayan yapılandırma öğelerini eşitler unutulmamalıdır.

### <a name="q-can-i-add-hsms-from-my-on-premises-network-to-a-high-availability-group-with-azure-dedicated-hsm"></a>S: HSM'ler my şirket içi ağdan Azure ayrılmış HSM ile yüksek kullanılabilirlik grubuna ekleyebilirim?

Evet. Bunlar, SafeNet Luna ağ HSM 7 için yüksek kullanılabilirlik gereksinimleri karşılaması gerekir.

### <a name="q-can-i-add-luna-56-hsms-from-on-premises-networks-to-a-high-availability-group-with-azure-dedicated-hsm"></a>S: 5/6 Luna HSM'ler şirket içi ağlardan Azure ayrılmış HSM ile yüksek kullanılabilirlik grubuna ekleyebilirim?

Hayır.

### <a name="q-how-many-hsms-can-i-add-to-the-same-high-availability-configuration-from-one-single-application"></a>S: Kaç tane HSM'ler tek bir uygulamadan diğerine aynı yüksek kullanılabilirlik yapılandırması için ekleyebilirim?

16 bir yüksek kullanılabilirlik grubunda üye var. altında gitti, tam kısıtlama mükemmel sonuçları ile test etme

## <a name="support"></a>Destek

### <a name="q-what-is-the-sla-for-dedicated-hsm-service"></a>S: Ayrılmış HSM hizmeti için SLA'sı nedir?

Ayrılmış HSM hizmet için sağlanan belirli bir çalışma süresi garantisi yoktur. Microsoft ağ aygıtına düzeyi erişim sağlayacak ve bu nedenle standart Azure ağ SLA'lar uygulayın.

### <a name="q-how-are-the-hsms-used-in-azure-dedicated-hsm-protected"></a>S: HSM'ler, Azure ayrılmış HSM korumalı içinde nasıl kullanılır?

Azure veri merkezleri, fiziksel ve yordam kapsamlı güvenlik denetimleri vardır. Buna ek olarak, ayrılmış HSM'ler bir veri merkezinin daha kısıtlı erişim alanında barındırılır. Bu alanlar, ek fiziksel erişim denetimi ve ek güvenlik için video kamera gözetim vardır.

### <a name="q-what-happens-if-there-is-a-security-breach-or-hardware-tampering-event"></a>S: Bir güvenlik ihlali veya olay oynama donanım varsa ne olur?

Ayrılmış HSM hizmetini SafeNet ağ HSM 7 cihazları kullanır. Bu gereçler, fiziksel ve mantıksal değiştirme algılama destekler. HSM'ler otomatik olarak zeroized bir kurcalamaya olay olursa.

### <a name="q-how-do-i-ensure-that-keys-in-my-dedicated-hsms-are-not-lost-due-to-error-or-a-malicious-insider-attack"></a>S: My ayrılmış HSM'ler anahtarlarında hata veya kötü amaçlı saldırı nedeniyle kaybolmamasını nasıl emin olabilirim?

HSM'ler normal düzenli yedeklemesi için olağanüstü durum kurtarma gerçekleştirmek için bir şirket içi HSM yedekleme cihazı kullanmak için önerilir. Eşler arası veya siteden siteye VPN bağlantısı için bir HSM yedekleme cihazı bağlı bir şirket içi iş istasyonu kullanmanız gerekecektir.

### <a name="q-how-do-i-get-support-for-dedicated-hsm"></a>S: Ayrılmış HSM için destek ne elde ederim?

Destek, hem Microsoft hem de Gemalto tarafından sağlanır.  Donanım veya ağ erişimi ile ilgili bir sorun varsa, Microsoft ve HSM yapılandırma, yazılım ile ilgili bir sorun olması ve uygulama geliştirme Lütfen raise Gemalto ile bir destek isteği ile bir destek isteği Yükselt. Microsoft ile bir destek isteği oluşturun, belirsiz bir sorun varsa ve ardından Gemalto olarak bağlı gereklidir. 

### <a name="q-how-do-i-get-the-client-software-documentation-and-access-to-integration-guidance-for-the-safenet-luna-7-hsm"></a>S: Yazılım, belgelere ve tümleştirme yönergeleri için SafeNet Luna 7 HSM erişim istemci nasıl alabilirim?

Gemalto müşteri desteği Portalı'nda kaydı sağlayan sağlanan hizmet için kaydolduktan sonra Gemalto Müşteri Kimliği olur. Bu, tüm yazılım ve belgeleri yanı sıra etkinleştirme destek istekleri Gemalto ile doğrudan erişmesine olanak sağlar.

### <a name="q-if-there-is-a-security-vulnerability-found-and-a-patch-is-released-by-gemalto-who-is-responsible-for-upgradingpatching-osfirmware"></a>S: Varsa yükseltme/düzeltme eki uygulama işletim sistemi/üretici yazılımı için sorumlu Gemalto tarafından bulunan bir güvenlik açığı ve bir düzeltme eki yayımlanır?

Microsoft, müşterilere ayrılmış HSM'ler için bağlantı özelliği yok. Müşteriler, yükseltme ve bunların HSM'ler düzeltme eki gerekir.

### <a name="q-what-if-i-need-to-reboot-my-hsm"></a>S: Peki my HSM yeniden başlatma gerekecek?

Bir komut satırı yeniden başlatma seçeneğini HSM var, ancak biz yeniden başlatma işlemi yanıt vermemeye sorun aralıklı olarak yaşayan ve güvenli için önerilir, bu nedenle yeniden cihaza fiziksel olarak yeniden sağlamak için Microsoft ile bir destek isteği oluşturun. 

## <a name="cryptography-and-standards"></a>Şifreleme ve standartları

### <a name="q-is-it-safe-to-store-encryption-keys-for-my-most-important-data-in-dedicated-hsm"></a>S: Ayrılmış HSM içinde en önemli verilerimi için şifreleme anahtarları depolamak güvenli mi?

Evet, özel HSM kullanın FIPS 140-2 Düzey 3 hsm'leri SafeNet ağ HSM 7 cihazları sağlar. 

### <a name="q-what-cryptographic-keys-and-algorithms-are-supported-by-dedicated-hsm"></a>S: Ayrılmış HSM tarafından hangi şifreleme anahtarlarını ve algoritmalar destekleniyor?

Ayrılmış HSM hizmetini hazırlar SafeNet ağ HSM 7 cihazları. Çok çeşitli şifreleme anahtar türleri ve algoritmaları dahil olmak üzere destekledikleri: Suite B için tam destek

* Asimetrik:
  * RSA
  * DSA
  * Diffie-Hellman
  * Eliptik Eğri
  * Şifreleme (ECDSA, ECDH, Ed25519, ECIES) ile kullanıcı tanımlı adlandırılmış ve Brainpool Eğriler, KCDSA
* Simetrik:
  * AES-GCM
  * Üçlü DES
  * DES
  * ARIA, ÇEKİRDEK
  * RC2
  * RC4
  * RC5
  * CAST
  * Özet/HMAC karma veya ileti: SHA-1, SHA-2, SM3
  * Anahtar türetme: SP800 108 sayacı modu
  * Anahtar kaydırma: SP800 38F
  * Rastgele sayı oluşturma: FIPS 140-2 ile BSI DRG.4 uymak DRBG (SP 800-90 CTRL modu) Onaylandı

### <a name="q-is-dedicated-hsm-fips-140-2-level-3-validated"></a>S: Ayrılmış HSM FIPS 140-2 Düzey 3 doğrulandı mı?

Evet. Ayrılmış HSM hizmetini kullanım FIPS 140-2 Düzey 3 hsm'leri SafeNet ağ HSM 7 cihazları sağlar.

### <a name="q-what-do-i-need-to-do-to-make-sure-i-operate-dedicated-hsm-in-fips-140-2-level-3-validated-mode"></a>S: Ne yapmak ihtiyacım miyim çalışan ayrılmış HSM FIPS emin olmak için modu 140-2 Düzey 3 doğrulandı?

Ayrılmış HSM hizmetini SafeNet Luna ağ HSM 7 cihazları sağlar. Bu gereçler kullanma FIPS 140-2 Düzey 3 hsm'leri. Ayrıca dağıtılan varsayılan yapılandırma, işletim sistemi ve üretici yazılımı FIPS doğrulanmasına sahiptir. FIPS 140-2 Düzey 3 uyumluluk için herhangi bir eylemde bulunmanız gerekmez.

### <a name="q-how-does-a-customer-ensure-that-when-an-hsm-is-deprovisioned-all-the-key-material-is-wiped-out"></a>S: Bir müşteri bir HSM sağlaması kaldırıldığında tüm anahtar malzemesi silindikten olduğunu nasıl emin?

Sağlama kaldırmayı istemeden önce bir müşteri Gemalto sağlanan HSM istemci araçlarını kullanarak HSM zeroized gerekir.

## <a name="performance-and-scale"></a>Performans ve ölçeklendirme

### <a name="q-how-many-cryptographic-operations-are-supported-per-second-with-dedicated-hsm"></a>S: Ayrılmış HSM ile saniye başına kaç şifreleme işlemleri desteklenir?

Ayrılmış HSM hükümlerine SafeNet ağ HSM 7 cihazları (model A790). Bazı işlemler için en yüksek performansı bir özeti aşağıda verilmiştir: 

* RSA-2048: saniye başına 10.000 işlem
* ECC P256: saniyede 20.000 işlem
* AES-GCM: saniyede 17,000

### <a name="q-how-many-partitions-can-be-created-in-dedicated-hsm"></a>S: Ayrılmış HSM içinde kaç bölüm oluşturulabilir?

A790 kullanılan SafeNet Luna HSM 7 modeli, hizmetin maliyeti 10 bölümler için bir lisans içerir. Cihaz 100 bölüm sınırı vardır ve bu sınıra kadar bölümleri ekleme Lisans ek ücrete neden ve cihaz üzerinde yeni bir lisans dosyası'nün yüklenmesi gerekir.

### <a name="q-how-many-keys-can-be-supported-in-dedicated-hsm"></a>S: Ayrılmış HSM içinde kaç anahtarları desteklenir?

Maksimum anahtar sayısı, kullanılabilir belleğin bir işlevdir. SafeNet Luna 7 modeli A790 32 MB bellek var. Aşağıdaki numaralarını da asimetrik anahtarları kullanıyorsanız anahtar çiftleri için geçerlidir.

* RSA 2048 - 19,000
* ECC P256 - 91,000

Kapasite, şablon anahtar oluşturma ve bölüm sayısı belirli bir anahtar özniteliklere bağlı olarak değişir.
