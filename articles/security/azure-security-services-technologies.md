---
title: Azure güvenlik hizmetleri ve teknolojileriyle | Microsoft Docs
description: Makale, Azure güvenlik hizmetleri ve teknolojileriyle seçkin bir listesini sağlar.
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: a5a7f60a-97e2-49b4-a8c5-7c010ff27ef8
ms.service: security
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2018
ms.author: barclayn
ms.openlocfilehash: e52cee2cb642de6e54270c597e6ed99f7162d0ed
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34641467"
---
# <a name="security-services-and-technologies-available-on-azure"></a>Güvenlik Hizmetleri ve teknolojileriyle Azure üzerinde kullanılabilir

Geçerli ve gelecekteki Azure müşterilerine bizim tartışmalarını biz genellikle "tüm sunmak için Azure sahip teknolojileri ve Hizmetleri güvenlikle ilgili listesini var mı?" sorulur

Bulut hizmeti sağlayıcısı seçenekleri değerlendirirken, bu bilgilere sahip yararlıdır. Bu nedenle başlamanıza yardımcı olmak için bu listeyi sağladık.

Zaman içinde bu liste değiştirin ve Azure gibi büyütün. Bu sayfayı bizim güvenlikle ilgili hizmetleri ve teknolojileri hakkında güncel kalmak için düzenli olarak kontrol ettiğinizden emin olun.

## <a name="general-azure-security"></a>Genel Azure güvenliği
|Hizmet|Açıklama|
|--------|--------|
|[Azure&nbsp;güvenlik&nbsp;Merkezi](../security-center/security-center-intro.md)| Karma bulut iş yüklerinde güvenlik yönetimi ve Gelişmiş tehdit koruması sağlayan bir bulut iş yükü koruması çözümü.|
|[Azure Anahtar Kasası.](../key-vault/key-vault-overview.md)| Güvenli parolaları deposu parolalar, bağlantı dizeleri ve diğer bilgileri çalışma, uygulamalarınızı tutmanız gerekir. |
|[Log Analytics](../log-analytics/log-analytics-overview.md)|Telemetri ve diğer verileri toplar ve uygulamalar ve kaynaklar için operasyonel Öngörüler teslim etmek için bir sorgu dili ve analizi altyapısı sağlayan izleme hizmeti. Güvenlik Merkezi gibi tek başına veya diğer hizmetler ile kullanılabilir. |
|[Azure geliştirme ve Test Laboratuvarları](../devtest-lab/devtest-lab-overview.md)|Geliştiriciler ve sınayıcılar hızla yardımcı olan bir hizmet oluşturma ortamları Azure'da atık en aza indirmenizi ve maliyetini denetleme.  |

<!---|[Azure&nbsp;Disk&nbsp;Encryption](azure-security-disk-encryption-overview.md)| THIS WILL GO TO THE NEW OVERVIEW TOPIC MEGHAN STEWART IS WRITING|--->

## <a name="storage-security"></a>Depolama alanı güvenliği
|Hizmet|Açıklama|
|------|--------|
| [Azure&nbsp;depolama&nbsp;hizmet&nbsp;şifreleme](../storage/common/storage-service-encryption.md)|Verilerinizi Azure depolama alanında otomatik olarak şifreler bir güvenlik özelliği.   |
|[StorSimple karma depolama şifreli](../storsimple/storsimple-ova-overview.md)| Şirket içi cihazlar ve Azure bulut depolama arasında depolama görevleri yöneten bir tümleşik depolama çözümü.|
|[Azure istemci tarafı şifreleme](../storage/common/storage-client-side-encryption.md)| Azure Storage'a yüklemeden önce istemci uygulamaların içindeki verileri şifreler istemci tarafı şifreleme çözüm; Ayrıca indirilirken verilerin şifresini çözer. |
| [Azure Storage paylaşılan erişim imzası](../storage/common/storage-dotnet-shared-access-signature-part-1.md)|Paylaşılan erişim imzası temsilci depolama hesabınızdaki kaynaklara erişim sağlar.  |
|[Azure depolama hesabı anahtarları](../storage/common/storage-create-storage-account.md)| Depolama hesabı erişildiğinde kimlik doğrulaması için kullanılan Azure depolama için bir erişim denetimi yöntemi. |
|[Azure dosya paylaşımları SMB 3.0 şifrelemesi ile](../storage/files/storage-files-introduction.md)|Otomatik sağlayan bir ağ güvenlik teknoloji ağ şifrelemesi için sunucu ileti bloğu (SMB) dosya paylaşım protokolüdür. |
|[Azure depolama çözümlemeleri](https://docs.microsoft.com/en-us/rest/api/storageservices/Storage-Analytics)| Depolama hesabınızdaki veriler için bir oturum açma ve Ölçümleri oluşturma teknolojisi. |

<!------>

## <a name="database-security"></a>Veritabanı güvenliği
|Hizmet|Açıklama|
|------|--------|
| [Azure&nbsp;SQL&nbsp;güvenlik duvarı](../sql-database/sql-database-firewall-configure.md)|Veritabanı için çalışan ağ tabanlı saldırılara karşı koruyan bir ağ erişim denetimi özelliği. |
|[Azure&nbsp;SQL&nbsp;hücre&nbsp;düzey şifreleme](https://blogs.msdn.microsoft.com/sqlsecurity/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database/)| Ayrıntılı düzeyinde şifreleme sağlar veritabanı güvenlik teknolojisi.  |
| [Azure&nbsp;SQL&nbsp;bağlantı şifreleme](../sql-database/sql-database-control-access.md)|SQL Veritabanı güvenliği sağlamak için erişimi IP adresine göre bağlantıyı sınırlayan güvenlik duvarı kuralları, kullanıcıların kimliğini kanıtlamasını gerektiren kimlik doğrulama sistemleri ve kullanıcıları belirli eylemler ve verilerle sınırlayan yetkilendirme sistemleriyle denetler. |
| [Azure SQL her zaman şifreleme](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017)|Kredi kartı numaraları veya Ulusal tanımlama numaraları (örneğin, ABD sosyal güvenlik numarası), gibi Azure SQL Database veya SQL Server veritabanlarına depolanan hassas verileri korur.  |
| [Azure&nbsp;SQL&nbsp;saydam veri şifreleme](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql?view=azuresqldb-current)| Veritabanının tamamını depolama şifreler veritabanı güvenlik özelliği. |
| [Azure SQL veritabanı denetimi](../sql-database/sql-database-auditing.md)|Bir veritabanı denetimi veritabanı olaylarını izler özellik ve Azure depolama hesabınızdaki bunları Denetim günlüğüne yazar.  |


## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi
|Hizmet|Açıklama|
|------|--------|
| [Azure&nbsp;rol&nbsp;göre&nbsp;erişim denetimi](../active-directory/role-based-access-control-configure.md)|Bir erişim denetimi özelliği, kullanıcıların yalnızca kuruluştaki rollerine göre erişim için gerekli olan kaynaklara erişmesine izin vermek üzere tasarlanmıştır.  |
| [Azure Active Directory](../active-directory/active-directory-whatis.md)|Çok kiracılı, bulut tabanlı bir dizin ve Azure içinde birden çok kimlik yönetimi hizmetlerini destekleyen bir bulut tabanlı kimlik doğrulama deposu.  |
| [Azure Active Directory B2C](../active-directory-b2c/active-directory-b2c-overview.md)|Üzerinde nasıl denetim sağlayan bir kimlik yönetimi hizmeti müşteriler kaydolma, oturum açma ve Azure tabanlı uygulamalar kullanırken profillerini yönetin.   |
| [Azure Active Directory etki alanı Hizmetleri](../active-directory-domain-services/active-directory-ds-overview.md)| Active Directory etki alanı Hizmetleri bulut tabanlı ve yönetilen sürümü. |
| [Azure çok faktörlü kimlik doğrulaması](../active-directory/authentication/multi-factor-authentication.md)| Birkaç farklı form kimlik doğrulaması ve güvenli bilgilere erişim verilmeden önce doğrulama kullanan bir güvenlik sağlama. |

## <a name="backup-and-disaster-recovery"></a>Yedekleme ve olağanüstü durum kurtarma
|Hizmet|Açıklama|
|------|--------|
| [Azure&nbsp;yedekleme](../backup/backup-introduction-to-azure-backup.md)| Yedekleme ve Azure Bulutu verileri geri yüklemek için kullanılan bir Azure tabanlı hizmet. |
| [Azure&nbsp;Site&nbsp;kurtarma](../site-recovery/site-recovery-overview.md)|Fiziksel ve sanal makineleri (VM'ler) üzerinde bir hatadan sonra kurtarma hizmetleri sağlamak için ikincil bir konuma bir birincil siteden çalışan iş yüklerini çoğaltan bir çevrimiçi hizmet. |

## <a name="networking"></a>Ağ
|Hizmet|Açıklama|
|------|--------|
| [Ağ&nbsp;güvenlik&nbsp;grupları](../virtual-network/virtual-networks-nsg.md)| İzin ver veya Reddet kararları yapmak için 5-tanımlama grubu kullanarak ağ tabanlı erişim denetimi özelliği.  |
| [Azure VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md)| İzin vermek için bir VPN uç noktası Azure sanal ağlara erişimi şirketler arası olarak kullanılan bir ağ aygıtı.  |
| [Azure uygulama ağ geçidi](../application-gateway/application-gateway-introduction.md)|Gelişmiş web uygulaması URL'sine bağlı yönlendirmek ve SSL boşaltma gerçekleştirmek dengeleyici yükleyin. |
| [Azure Load Balancer](../load-balancer/load-balancer-overview.md)|TCP/UDP uygulama Ağ Yük Dengeleyici. |
| [Azure ExpressRoute](../expressroute/expressroute-introduction.md)| Ayrılmış WAN şirket içi ağlar ve Azure sanal ağlar arasında bağlantı kurun. |
| [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md)| Genel DNS yük dengeleyici.|
| [Azure uygulama proxy'si](../active-directory/active-directory-application-proxy-get-started.md)| Bir web uygulamaları için uzaktan erişimin güvenliğini sağlamak için kullanılan ön uç kimlik doğrulaması, şirket içi barındırılan. |