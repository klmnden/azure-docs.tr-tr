---
title: Azure güvenlik hizmetleri ve teknolojileri | Microsoft Docs
description: Bu makale Azure güvenlik hizmetleri ve teknolojileri seçkin bir listesini sağlar.
services: security
documentationcenter: na
author: barclayn
manager: barbkess
editor: TomSh
ms.assetid: a5a7f60a-97e2-49b4-a8c5-7c010ff27ef8
ms.service: security
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/29/2019
ms.author: barclayn
ms.openlocfilehash: 13183282e5e607f0052194a474203f97e0160adb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60610906"
---
# <a name="security-services-and-technologies-available-on-azure"></a>Güvenlik Hizmetleri ve teknolojileri Azure üzerinde kullanılabilir

Mevcut ve gelecekteki Azure müşterilerine sunduğumuz tartışmalara biz genellikle "tüm Azure sunduğu teknolojileri ve Hizmetleri güvenlikle ilgili listesi gerekiyor?" sorulur

Bulut hizmeti sağlayıcısı seçenekleri değerlendirirken, bu bilgileri sağlamak yararlıdır. Bu nedenle başlamanıza yardımcı olmak için bu listenin sağladık.

Zamanla, bu liste değiştirin ve Azure gibi büyütün. Bu sayfada, güvenlikle ilgili hizmetleri ve teknolojileri hakkında güncel kalmak için düzenli olarak kontrol ettiğinizden emin olun.

## <a name="general-azure-security"></a>Genel Azure güvenliği
|Hizmet|Açıklama|
|--------|--------|
|[Azure&nbsp;güvenlik&nbsp;Merkezi](../security-center/security-center-intro.md)| Hibrit bulut iş yüklerinde güvenlik yönetimi ve Gelişmiş tehdit koruması sağlayan bir bulut iş yükü koruması çözümü.|
|[Azure Anahtar Kasası.](../key-vault/key-vault-overview.md)| Parolalar, bağlantı dizelerini ve diğer bilgileri çalışan uygulamalarınızı çalışır durumda bulundurmanıza gerek için güvenli bir gizli dizi deposu. |
|[Azure İzleyici günlükleri](../log-analytics/log-analytics-overview.md)|Telemetri ve diğer verileri toplar ve uygulamalar ve kaynaklar için operasyonel içgörüler sunmak için bir sorgu dili ve analiz altyapısı sağlayan izleme hizmeti. Güvenlik Merkezi gibi tek başına veya diğer hizmetler ile kullanılabilir. |
|[Azure geliştirme ve Test Laboratuvarları](../devtest-lab/devtest-lab-overview.md)|Geliştiricilere ve test edicilere hızlı bir şekilde yardımcı olan bir hizmet oluşturma ortamları Azure'da Harcamaları azaltın ve maliyetleri denetleyin.  |

<!---|[Azure&nbsp;Disk&nbsp;Encryption](azure-security-disk-encryption-overview.md)| THIS WILL GO TO THE NEW OVERVIEW TOPIC MEGHAN STEWART IS WRITING|--->

## <a name="storage-security"></a>Depolama alanı güvenliği
|Hizmet|Açıklama|
|------|--------|
| [Azure&nbsp;depolama&nbsp;hizmet&nbsp;şifreleme](../storage/common/storage-service-encryption.md)|Azure depolama, verilerinizi otomatik olarak şifreler ve güvenlik özelliği.   |
|[StorSimple karma depolama şifreli](../storsimple/storsimple-ova-overview.md)| Şirket içi cihazlar ve Azure bulut depolama arasındaki Depolama görevlerini yöneten tümleşik bir çözüm.|
|[Azure istemci tarafı şifreleme](../storage/common/storage-client-side-encryption.md)| Azure Storage'a yüklemeden önce istemci uygulamalarının içinde verileri şifreler ve istemci tarafı şifreleme çözümü; Ayrıca indirirken verilerin şifresini çözer. |
| [Azure depolama paylaşılan erişim imzaları](../storage/common/storage-dotnet-shared-access-signature-part-1.md)|Paylaşılan erişim imzası, depolama hesabınızdaki kaynaklara temsilci erişimi sağlar.  |
|[Azure depolama hesabı anahtarları](../storage/common/storage-create-storage-account.md)| Depolama hesabına erişim sağlandığında kimlik doğrulaması için kullanılan Azure depolama için bir erişim denetimi yöntemi. |
|[SMB 3.0 şifrelemesi ile Azure dosya paylaşımları](../storage/files/storage-files-introduction.md)|Otomatik sağlayan bir ağ güvenlik teknolojisi ağ şifrelemesi için sunucu ileti bloğu (SMB) dosya paylaşım protokolüdür. |
|[Azure depolama analizi](https://docs.microsoft.com/rest/api/storageservices/Storage-Analytics)| Depolama hesabınızdaki veriler, bir günlük kaydı ve ölçümler oluşturma teknolojisi. |

<!------>

## <a name="database-security"></a>Veritabanı güvenliği
|Hizmet|Açıklama|
|------|--------|
| [Azure&nbsp;SQL&nbsp;güvenlik duvarı](../sql-database/sql-database-firewall-configure.md)|Veritabanına çalışan ağ tabanlı saldırılara karşı koruyan bir ağ erişim denetimi özelliği. |
|[Azure&nbsp;SQL&nbsp;hücre&nbsp;şifreleme düzeyi](https://blogs.msdn.microsoft.com/sqlsecurity/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database/)| Ayrıntılı bir düzeyde şifreleme sağlar bir veritabanı güvenlik teknolojisidir.  |
| [Azure&nbsp;SQL&nbsp;bağlantı şifrelemesi](../sql-database/sql-database-control-access.md)|SQL Veritabanı güvenliği sağlamak için erişimi IP adresine göre bağlantıyı sınırlayan güvenlik duvarı kuralları, kullanıcıların kimliğini kanıtlamasını gerektiren kimlik doğrulama sistemleri ve kullanıcıları belirli eylemler ve verilerle sınırlayan yetkilendirme sistemleriyle denetler. |
| [Azure SQL her zaman şifreleme](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017)|Kredi kartı numaraları veya Ulusal Kimlik numaraları (örneğin, ABD sosyal güvenlik numaraları) gibi Azure SQL veritabanı veya SQL Server veritabanlarında depolanan hassas verileri korur.  |
| [Azure&nbsp;SQL&nbsp;saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql?view=azuresqldb-current)| Tüm veritabanı depolama şifreler veritabanı güvenlik özelliği. |
| [Azure SQL veritabanı denetimi](../sql-database/sql-database-auditing.md)|Bir veritabanı özelliği, veritabanı olaylarını izler ve Azure depolama hesabınızdaki bir denetim günlüğüne yazar denetim.  |


## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi
|Hizmet|Açıklama|
|------|--------|
| [Azure&nbsp;rol&nbsp;tabanlı&nbsp;erişim denetimi](../active-directory/role-based-access-control-configure.md)|Bir erişim denetimi özelliği, kullanıcıların yalnızca Yöneticiler kuruluştaki rollerine dayalı olarak erişimi için gerekli kaynakları erişmesine izin vermek için tasarlanmıştır.  |
| [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md)|Bir çok kiracılı, bulut tabanlı dizin ve Azure içinde birden çok kimlik yönetimi hizmetlerini destekleyen bir bulut tabanlı kimlik doğrulama deposu.  |
| [Azure Active Directory B2C](../active-directory-b2c/active-directory-b2c-overview.md)|Nasıl denetlemenizi sağlayan bir kimlik yönetimi hizmetidir müşteriler kaydolma, oturum açma ve Azure tabanlı uygulamaları kullanırken profillerini yönetin.   |
| [Azure Active Directory etki alanı Hizmetleri](../active-directory-domain-services/active-directory-ds-overview.md)| Active Directory Domain Services yönetilen ve bulut tabanlı sürümü. |
| [Azure çok faktörlü kimlik doğrulaması](../active-directory/authentication/multi-factor-authentication.md)| Kimlik doğrulaması ve güvenli bilgilerine erişime izin vermeden önce doğrulama birkaç farklı biçimleri kullanan güvenlik sağlama. |

## <a name="backup-and-disaster-recovery"></a>Yedekleme ve olağanüstü durum kurtarma
|Hizmet|Açıklama|
|------|--------|
| [Azure&nbsp;yedekleme](../backup/backup-introduction-to-azure-backup.md)| Yedekleme ve geri yükleme Azure bulutunda veri için kullanılan bir Azure tabanlı hizmeti. |
| [Azure&nbsp;Site&nbsp;kurtarma](../site-recovery/site-recovery-overview.md)|Birincil bir siteden bir hatadan sonra hizmetleri düzenli kurtarmayı etkinleştirmek için ikincil bir konuma fiziksel ve sanal makineler (VM) üzerinde çalışan iş yüklerini çoğaltır çevrimiçi bir hizmettir. |

## <a name="networking"></a>Ağ
|Hizmet|Açıklama|
|------|--------|
| [Ağ&nbsp;güvenlik&nbsp;grupları](../virtual-network/virtual-networks-nsg.md)| İzin verme veya reddetme kararları yapmak için 5-tanımlama grubu kullanarak ağ tabanlı erişim denetimi özelliği.  |
| [Azure VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md)| Bir VPN uç noktası, izin vermek için Azure sanal ağlarına erişimi içi ve dışı karışık olarak kullanılan bir ağ aygıtı.  |
| [Azure uygulama ağ geçidi](../application-gateway/application-gateway-introduction.md)|Gelişmiş web uygulaması, URL tabanlı Yönlendirme ve SSL boşaltma gerçekleştirmek dengeleyici yükleyin. |
|[Web uygulaması güvenlik duvarı](../application-gateway/waf-overview.md) (WAF)|Web uygulamalarınızda açıklardan yararlanmaya ve güvenlik açıkları merkezi koruma sağlayan bir Application Gateway özelliğidir|
| [Azure Load Balancer](../load-balancer/load-balancer-overview.md)|TCP/UDP uygulama Ağ Yükü dengeleyici. |
| [Azure ExpressRoute](../expressroute/expressroute-introduction.md)| Adanmış bir WAN, şirket içi ağlar ve Azure sanal ağları arasında bağlantı. |
| [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md)| Yük Dengeleyici Genel DNS.|
| [Azure uygulama proxy'si](../active-directory/active-directory-application-proxy-get-started.md)| Bir web uygulaması için uzaktan erişimin güvenliğini sağlamak için kullanılan ön uç kimlik doğrulaması, şirket içinde barındırılan. |
|[Azure güvenlik duvarı](../firewall/overview.md)|Azure sanal ağ kaynaklarınıza koruyan yönetilen, bulut tabanlı bir ağ güvenlik hizmeti.|
|[Azure DDoS koruması](../virtual-network/ddos-protection-overview.md)|Uygulama tasarım en iyi yöntemleri ile birlikte, DDoS saldırılarına karşı koruma sağlar.|
|[Sanal ağ hizmet uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md)|Sanal ağ özel adres alanınızı ve Azure Hizmetleri, sanal ağınızın kimliğini doğrudan bağlantı üzerinden genişletir.|