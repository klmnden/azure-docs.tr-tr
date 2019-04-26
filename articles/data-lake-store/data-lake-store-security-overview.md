---
title: Azure Data Lake depolama Gen1 Güvenliğe genel bakış | Microsoft Docs
description: Azure Data Lake depolama Gen1 daha güvenli bir büyük veri deposuna nasıl olduğunu anlama
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: ebd5b2ac-c5cc-46d4-9cfd-1a1ee70024c2
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: twooley
ms.openlocfilehash: 63e538ab43eaf4a34226b0084cf55334e2cc782b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60195325"
---
# <a name="security-in-azure-data-lake-storage-gen1"></a>Azure Data Lake depolama Gen1 güvenliği
Çoğu kurum akıllı kararlar yapmalarına yardımcı olmak iş öngörüleri için büyük veri analizi avantajlarından sürüyor. Bir kuruluş, artan sayıda farklı kullanıcılar ile karmaşık ve denetimli bir ortam olabilir. Kritik iş verilerini doğru bireysel kullanıcılara verilen erişim düzeyini ile daha güvenli bir şekilde saklandığından emin olmak bir kuruluş için önemlidir. Azure Data Lake depolama Gen1 bu güvenlik gereksinimlerini karşılamanıza yardımcı olmak üzere tasarlanmıştır. Bu makalede, dahil olmak üzere güvenlik özelliklerinin Data Lake depolama Gen1, hakkında bilgi edinin:

* Kimlik Doğrulaması
* Yetkilendirme
* Ağ yalıtımı
* Veri koruma
* Denetim

## <a name="authentication-and-identity-management"></a>Kimlik doğrulaması ve Kimlik Yönetimi
Kimlik doğrulaması, kullanıcı için Data Lake depolama Gen1 bağlanan herhangi bir hizmeti veya Data Lake depolama Gen1 ile etkileşim kurduğunda, bir kullanıcının kimliği doğrulanır işlemidir. Data Lake depolama Gen1 kimlik yönetimi ve kimlik doğrulaması için kullandığı [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md), kullanıcıların ve grupların yönetimini kolaylaştıran kapsamlı bir kimlik ve erişim yönetimi bulut çözümüdür.

Her Azure aboneliğinin Azure Active Directory örneği ile ilişkili olabilir. Yalnızca kullanıcı ve hizmet kimlikleri, Azure Active Directory hizmetinizdeki tanımlanan Data Lake depolama Gen1 hesabınıza komut satırı araçları, Azure portalı kullanarak erişebilir veya istemci uygulamaları veri Gölü kullanarak kuruluşunuzun oluşturur. Depolama Gen1 SDK. Bir merkezi erişim denetimi mekanizması Azure Active Directory kullanmanın başlıca avantajları şunlardır:

* Basitleştirilmiş kimlik yaşam döngüsü yönetimi. Bir kullanıcı veya hizmet (hizmet sorumlusu kimliği) kimliğini hızlı bir şekilde oluşturulabilir ve yalnızca silme veya dizinde hesabı devre dışı bırakarak hızlı bir şekilde iptal edildi.
* Çok öğeli kimlik doğrulama. [Çok faktörlü kimlik doğrulaması](../active-directory/authentication/multi-factor-authentication.md) kullanıcı oturum açma ve işlemleri için ek bir güvenlik katmanı sağlar.
* OAuth veya Openıd gibi standart bir açık protokol üzerinden herhangi bir istemciden kimlik doğrulaması.
* Federasyon ile Kurumsal Dizin Hizmetleri ve bulut kimlik sağlayıcıları.

## <a name="authorization-and-access-control"></a>Yetkilendirme ve erişim denetimi
Azure Active Directory, Data Lake depolama Gen1 erişebilmesi için bir kullanıcı kimlik doğrulaması yaptıktan sonra Data Lake depolama Gen1 izinlerini erişimi yetkilendirme kontrol eder. Data Lake depolama Gen1 yetkilendirme hesabı ve veri ilgili etkinlikler için aşağıdaki şekilde ayırır:

* [Rol tabanlı erişim denetimi](../role-based-access-control/overview.md) (RBAC), hesap yönetimi için Azure tarafından sağlanan
* Veri deposundaki erişmek için POSIX ACL'si

### <a name="rbac-for-account-management"></a>Hesap yönetimi için RBAC
Dört temel rol, varsayılan Data Lake depolama Gen1 için tanımlanır. Rolleri, bir Data Lake depolama Gen1 hesabı Azure portalı, PowerShell cmdlet'leri ve REST API'leri aracılığıyla farklı işlemlerin izin verir. Sahibi ve katkıda bulunan rollerine hesapta çeşitli yönetim işlevleri gerçekleştirebilirsiniz. Okuyucu rolü yalnızca hesap yönetimi verileri görüntüleyen kullanıcılara atayabilirsiniz.

![RBAC rollerini](./media/data-lake-store-security-overview/rbac-roles.png "RBAC rolleri")

Hesap yönetimi için rolleri atanmış olsa da, bazı roller verilere erişim etkileyeceğini unutmayın. Dosya sisteminde bir kullanıcının gerçekleştirebileceği işlemleri erişimi denetlemek için ACL'leri kullanmanız gerekir. Aşağıdaki tabloda, yönetim haklarını ve varsayılan rolleri için veri erişim haklarını özetini gösterir.

| Roller | Yönetim hakları | Veri erişim hakları | Açıklama |
| --- | --- | --- | --- |
| Atanmış bir rol yok |None |ACL tarafından yönetilir |Kullanıcı, Data Lake depolama Gen1 göz atmak için Azure portal veya Azure PowerShell cmdlet'lerini kullanamazsınız. Kullanıcı, yalnızca komut satırı araçlarını kullanabilirsiniz. |
| Sahip |Tümü |Tümü |Bir süper kullanıcı sahibi rolüdür. Bu rol, her şeyi yönetebilir ve veri tam erişime sahip olur. |
| Okuyucu |Salt okunur |ACL tarafından yönetilir |Okuyucu rolü, hangi role atanmış kullanıcı gibi hesap yönetimi ile ilgili her şeyi görüntüleyebilir. Okuyucu rolü, değişiklik yapamazsınız. |
| Katılımcı |Ekle ve Kaldır rolleri dışında tümü |ACL tarafından yönetilir |Katkıda bulunan rolü dağıtımları oluşturma ve Uyarıları yönetme gibi bir hesap, bazı özelliklerini yönetebilir. Katkıda bulunan rolü ekleme veya rolleri kaldırın. |
| Kullanıcı Erişimi Yöneticisi |Rol Ekleme ve kaldırma |ACL tarafından yönetilir |Kullanıcı erişimi yöneticisi rolü hesaplarına kullanıcı erişimini yönetebilir. |

Yönergeler için [Data Lake depolama Gen1 hesaplarına kullanıcıları veya güvenlik gruplarına atamak](data-lake-store-secure-data.md#assign-users-or-security-groups-to-data-lake-storage-gen1-accounts).

### <a name="using-acls-for-operations-on-file-systems"></a>Dosya sistemi işlemleri için ACL'leri kullanarak
Data Lake depolama Gen1 olduğu hiyerarşik dosya sistemi gibi Hadoop dağıtılmış dosya sistemi (HDFS) ve destekler [POSIX ACL](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Okuma (r) denetimleri, yazma (w) ve yürütme (x) kaynaklara sahip rolü, sahipleri grup ve diğer kullanıcılar ve gruplar için izinleri. Data Lake depolama Gen1 ACL'ler Kök klasörde, alt klasörler ve dosyaları tek tek etkinleştirilebilir. ACL'ler Data Lake depolama Gen1 bağlamında nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Data Lake depolama Gen1'deki erişim denetimi](data-lake-store-access-control.md).

ACL'leri kullanarak birden çok kullanıcı için tanımladığınız öneririz [güvenlik grupları](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md). Kullanıcılar bir güvenlik grubuna ekleyin ve bir dosyanın veya klasörün ACL'lerini bu güvenlik grubuna atayın. 28 girişleri atanmış izinler için en fazla sınırlı olduğundan, atanmış izinler sağlamak istediğinizde bu kullanışlıdır. Daha iyi Azure Active Directory güvenlik gruplarını kullanarak Data Lake depolama Gen1 içinde depolanan verilerin güvenliğini sağlama hakkında daha fazla bilgi için bkz. [ACL olarak kullanıcıları veya güvenlik grubunu Data Lake depolama Gen1 dosya sistemine atama](data-lake-store-secure-data.md#filepermissions).

![Liste erişim izinleri](./media/data-lake-store-security-overview/adl.acl.2.png "erişim izinleri Listele")

## <a name="network-isolation"></a>Ağ yalıtımı
Data Lake depolama Gen1 veri deponuz ağ düzeyinde erişimi denetlemeye yardımcı olmak için kullanın. Güvenlik duvarları oluşturmak ve güvenilen istemcileriniz için bir IP adresi aralığı tanımlayın. Bir IP adresi aralığı ile yalnızca bir IP adresi tanımlı aralığın içinde olan istemciler için Data Lake depolama Gen1 bağlanabilirsiniz.

![Güvenlik Duvarı ayarları ve IP erişim](./media/data-lake-store-security-overview/firewall-ip-access.png "Güvenlik Duvarı ayarları ve IP adresi")

## <a name="data-protection"></a>Veri koruma
Data Lake depolama Gen1 yaşam döngüsü boyunca verilerinizi korur. Data Lake depolama Gen1, Taşınmakta olan veriler için ağ üzerinden veri güvenliğini sağlamak için sektör standardı Aktarım Katmanı Güvenliği (TLS 1.2) protokolünü kullanır.

![Data Lake depolama Gen1 şifreleme](./media/data-lake-store-security-overview/adls-encryption.png "Data Lake depolama Gen1 şifreleme")

Data Lake depolama Gen1 ayrıca hesapta depolanan veriler için şifreleme sağlar. Verilerinizin şifrelenmesini tercih edebilir ya da şifrelemeyi kabul etmeyebilirsiniz. Şifreleme için kabul ederseniz, Data Lake depolama Gen1 içinde depolanan verileri kalıcı medyada depolanmadan önce şifrelenir. Böyle bir durumda, Data Lake depolama Gen1 otomatik olarak kaydetmeden önce verileri şifreler ve veriye erişen istemci için tamamen saydam olacak şekilde, almadan öncesinde verilerin şifresini çözer. İstemci tarafında veri şifreleme/şifre çözme için gereken kod değişikliği vardır.

Anahtar Yönetimi için Data Lake depolama Gen1, Data Lake depolama Gen1 içinde depolanan tüm verilerin şifresini çözmek için gerekli olan, ana şifreleme anahtarlarının (Mek'ler) yönetimi için iki mod sağlar. Data Lake depolama Mek'leri yönettiğiniz veya Azure anahtar kasası hesabınızı kullanarak Mek'ler sahipliğini tutmayı seçmeniz Gen1 ya da sağlayabilirsiniz. Anahtar Yönetimi modunda, bir Data Lake depolama Gen1 hesabı oluşturulurken bir hata belirtin. Şifreleme tabanlı yapılandırma sağlama konusunda daha fazla bilgi için bkz. [Azure Data Lake depolama Gen1 ile çalışmaya başlama Azure portalını kullanarak](data-lake-store-get-started-portal.md).

## <a name="activity-and-diagnostic-logs"></a>Etkinlik ve tanılama günlükleri
Etkinlik veya hesap yönetimi ile ilgili etkinlikler ve verilerle ilgili etkinlikler için mi aradığınız bağlı olarak tanılama günlükleri, kullanabilirsiniz.

* Hesap Yönetimi ile ilgili etkinlikleri, Azure Resource Manager API'lerini kullanın ve Azure portalda etkinlik günlükleri çıkarılır.
* Verilerle ilgili etkinlikler, WebHDFS REST API'lerini kullanma ve Azure portalında, tanılama günlükleri çıkarılır.

### <a name="activity-log"></a>Etkinlik günlüğü
Yönetmeliklerine uyum sağlamak için bir kuruluşun belirli olaylar halinde sorgulaması gerekiyorsa hesap yönetimi etkinlikleri, yeterli denetim izleri gerektirebilir. Data Lake depolama Gen1 yerleşik izleme ve tüm hesap yönetimi etkinliklerini kaydeder.

Hesap Yönetimi denetim kayıtları, görüntülemek ve günlüğe kaydetmek istediğiniz sütunları seçin. Ayrıca, Azure depolama etkinlik günlüklerini dışarı aktarabilirsiniz.

![Etkinlik günlüğü](./media/data-lake-store-security-overview/activity-logs.png "etkinlik günlüğü")

Etkinlik günlükleri ile çalışma hakkında daha fazla bilgi için bkz. [kaynaklara uygulanan eylemleri denetlemek için etkinlik günlüklerini görüntüleme](../azure-resource-manager/resource-group-audit.md).

### <a name="diagnostics-logs"></a>Tanılama günlükleri
Veri erişim denetim ve tanılama Azure portalında günlük kaydını etkinleştirmek ve bir Azure Blob Depolama hesabına, bir olay hub'ı günlükleri gönderme veya Azure İzleyici günlüğe kaydeder.

![Tanılama günlükleri](./media/data-lake-store-security-overview/diagnostic-logs.png "tanılama günlükleri")

Data Lake depolama Gen1 ile tanılama günlükleri ile çalışma hakkında daha fazla bilgi için bkz. [Data Lake depolama Gen1 için tanılama günlüklerine erişme](data-lake-store-diagnostic-logs.md).

## <a name="summary"></a>Özet
Kurumsal müşteriler, güvenli ve kullanımı kolay bir veri analizi bulut platformunun isteğe bağlı. Data Lake depolama Gen1 kimlik yönetimi ve Azure Active Directory Tümleştirmesi, ACL tabanlı yetkilendirme, ağ yalıtımı, aktarım ve bekleme sırasında veri şifrelemesi ile kimlik doğrulaması aracılığıyla bu gereksinimleri karşılamasını yardımcı olmak için tasarlanmıştır ve denetim.

Data Lake depolama Gen1 yenilikleri görmek istiyorsanız, bize geri bildirim gönder [Data Lake depolama Gen1 UserVoice forumumuzu](https://feedback.azure.com/forums/327234-data-lake).

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Data Lake depolama Gen1 genel bakış](data-lake-store-overview.md)
* [Data Lake depolama Gen1 ile çalışmaya başlama](data-lake-store-get-started-portal.md)
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)

