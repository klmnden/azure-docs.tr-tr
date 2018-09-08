---
title: Data Lake Store güvenlik genel bakış | Microsoft Docs
description: Azure Data Lake Store daha güvenli bir büyük veri deposuna nasıl olduğunu anlama
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ebd5b2ac-c5cc-46d4-9cfd-1a1ee70024c2
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: nitinme
ms.openlocfilehash: 4ecc94f4ab5e9091db1705e99d4a5df6abbaf350
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44161093"
---
# <a name="security-in-azure-data-lake-store"></a>Azure Data Lake Store güvenlik
Çoğu kurum akıllı kararlar yapmalarına yardımcı olmak iş öngörüleri için büyük veri analizi avantajlarından sürüyor. Bir kuruluş, artan sayıda farklı kullanıcılar ile karmaşık ve denetimli bir ortam olabilir. Kritik iş verilerini doğru bireysel kullanıcılara verilen erişim düzeyini ile daha güvenli bir şekilde saklandığından emin olmak bir kuruluş için önemlidir. Azure Data Lake Store, bu güvenlik gereksinimlerini karşılamanıza yardımcı olmak üzere tasarlanmıştır. Bu makalede Data Lake Store güvenlik özellikleri hakkında bilgi dahil olmak üzere:

* Kimlik Doğrulaması
* Yetkilendirme
* Ağ yalıtımı
* Veri koruma
* Denetim

## <a name="authentication-and-identity-management"></a>Kimlik doğrulaması ve Kimlik Yönetimi
Kimlik doğrulaması, kullanıcı için Data Lake Store bağlanan herhangi bir hizmeti veya Data Lake Store ile etkileşim kurduğunda, bir kullanıcının kimliği doğrulanır işlemidir. Data Lake Store kimlik yönetimi ve kimlik doğrulaması için kullandığı [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md), kullanıcıların ve grupların yönetimini kolaylaştıran kapsamlı bir kimlik ve erişim yönetimi bulut çözümüdür.

Her Azure aboneliğinin Azure Active Directory örneği ile ilişkili olabilir. Yalnızca kullanıcı ve Azure Active Directory hizmetinizdeki tanımlanan hizmet kimlikleri Data Lake Store hesabınıza komut satırı araçları, Azure portalı kullanarak erişebilir veya istemci uygulamaları ile Azure Data Lake kullanarak kuruluşunuz oluşturur. Store SDK'sı. Bir merkezi erişim denetimi mekanizması Azure Active Directory kullanmanın başlıca avantajları şunlardır:

* Basitleştirilmiş kimlik yaşam döngüsü yönetimi. Bir kullanıcı veya hizmet (hizmet sorumlusu kimliği) kimliğini hızlı bir şekilde oluşturulabilir ve yalnızca silme veya dizinde hesabı devre dışı bırakarak hızlı bir şekilde iptal edildi.
* Çok faktörlü kimlik doğrulaması. [Çok faktörlü kimlik doğrulaması](../active-directory/authentication/multi-factor-authentication.md) kullanıcı oturum açma ve işlemleri için ek bir güvenlik katmanı sağlar.
* OAuth veya Openıd gibi standart bir açık protokol üzerinden herhangi bir istemciden kimlik doğrulaması.
* Federasyon ile Kurumsal Dizin Hizmetleri ve bulut kimlik sağlayıcıları.

## <a name="authorization-and-access-control"></a>Yetkilendirme ve erişim denetimi
Azure Active Directory, Azure Data Lake Store erişebilmesi için bir kullanıcı kimlik doğrulaması yaptıktan sonra Data Lake Store için izinleri erişimi yetkilendirme kontrol eder. Data Lake Store hesabı ve veri ilgili etkinlikler için yetkilendirme aşağıdaki şekilde ayırır:

* [Rol tabanlı erişim denetimi](../role-based-access-control/overview.md) (RBAC), hesap yönetimi için Azure tarafından sağlanan
* Veri deposundaki erişmek için POSIX ACL'si

### <a name="rbac-for-account-management"></a>Hesap yönetimi için RBAC
Dört temel rol, varsayılan Data Lake Store için tanımlanır. Rolleri, bir Data Lake Store hesabı Azure portalı, PowerShell cmdlet'leri ve REST API'leri aracılığıyla farklı işlemlerin izin verir. Sahibi ve katkıda bulunan rollerine hesapta çeşitli yönetim işlevleri gerçekleştirebilirsiniz. Okuyucu rolü yalnızca hesap yönetimi verileri görüntüleyen kullanıcılara atayabilirsiniz.

![RBAC rollerini](./media/data-lake-store-security-overview/rbac-roles.png "RBAC rolleri")

Hesap yönetimi için rolleri atanmış olsa da, bazı roller verilere erişim etkileyeceğini unutmayın. Dosya sisteminde bir kullanıcının gerçekleştirebileceği işlemleri erişimi denetlemek için ACL'leri kullanmanız gerekir. Aşağıdaki tabloda, yönetim haklarını ve varsayılan rolleri için veri erişim haklarını özetini gösterir.

| Roller | Yönetim hakları | Veri erişim hakları | Açıklama |
| --- | --- | --- | --- |
| Atanmış bir rol yok |None |ACL tarafından yönetilir |Kullanıcı, Data Lake Store göz atmak için Azure portal veya Azure PowerShell cmdlet'lerini kullanamazsınız. Kullanıcı, yalnızca komut satırı araçlarını kullanabilirsiniz. |
| Sahip |Tümü |Tümü |Bir süper kullanıcı sahibi rolüdür. Bu rol, her şeyi yönetebilir ve veri tam erişime sahip olur. |
| Okuyucu |Salt okunur |ACL tarafından yönetilir |Okuyucu rolü, hangi role atanmış kullanıcı gibi hesap yönetimi ile ilgili her şeyi görüntüleyebilir. Okuyucu rolü, değişiklik yapamazsınız. |
| Katılımcı |Ekle ve Kaldır rolleri dışında tümü |ACL tarafından yönetilir |Katkıda bulunan rolü dağıtımları oluşturma ve Uyarıları yönetme gibi bir hesap, bazı özelliklerini yönetebilir. Katkıda bulunan rolü ekleme veya rolleri kaldırın. |
| Kullanıcı Erişimi Yöneticisi |Rol Ekleme ve kaldırma |ACL tarafından yönetilir |Kullanıcı erişimi yöneticisi rolü hesaplarına kullanıcı erişimini yönetebilir. |

Yönergeler için [Data Lake Store hesaplarına kullanıcıları veya güvenlik gruplarına atamak](data-lake-store-secure-data.md#assign-users-or-security-groups-to-azure-data-lake-store-accounts).

### <a name="using-acls-for-operations-on-file-systems"></a>Dosya sistemi işlemleri için ACL'leri kullanarak
Data Lake Store olduğu hiyerarşik dosya sistemi gibi Hadoop dağıtılmış dosya sistemi (HDFS) ve destekler [POSIX ACL](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Okuma (r) denetimleri, yazma (w) ve yürütme (x) kaynaklara sahip rolü, sahipleri grup ve diğer kullanıcılar ve gruplar için izinleri. Data Lake Store ACL'ler Kök klasörde, klasörlerde ve tek tek dosyalarda etkinleştirilebilir. Data Lake Store bağlanımda ACL’lerin nasıl çalıştığı üzerine daha fazla bilgi için bkz. [Data Lake Store’da erişim denetimi](data-lake-store-access-control.md).

ACL'leri kullanarak birden çok kullanıcı için tanımladığınız öneririz [güvenlik grupları](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md). Kullanıcılar bir güvenlik grubuna ekleyin ve bir dosyanın veya klasörün ACL'lerini bu güvenlik grubuna atayın. 28 girişleri atanmış izinler için en fazla sınırlı olduğundan, atanmış izinler sağlamak istediğinizde bu kullanışlıdır. Daha iyi Azure Active Directory güvenlik gruplarını kullanarak Data Lake Store içinde depolanan verilerin güvenliğini sağlama hakkında daha fazla bilgi için bkz. [ACL olarak kullanıcıları veya güvenlik grubunu Azure Data Lake Store dosya sistemine atama](data-lake-store-secure-data.md#filepermissions).

![Liste erişim izinleri](./media/data-lake-store-security-overview/adl.acl.2.png "erişim izinleri Listele")

## <a name="network-isolation"></a>Ağ yalıtımı
Kullanım Data Lake, veri depolama ağ düzeyinde erişimi denetlemeye yardımcı olmak için Store. Güvenlik duvarları oluşturmak ve güvenilen istemcileriniz için bir IP adresi aralığı tanımlayın. Bir IP adresi aralığı ile yalnızca bir IP adresi tanımlı aralığın içinde olan istemciler için Data Lake Store bağlanabilirsiniz.

![Güvenlik Duvarı ayarları ve IP erişim](./media/data-lake-store-security-overview/firewall-ip-access.png "Güvenlik Duvarı ayarları ve IP adresi")

## <a name="data-protection"></a>Veri koruma
Azure Data Lake Store, yaşam döngüsü boyunca, verilerinizi korur. Data Lake Store, Taşınmakta olan veriler için ağ üzerinden veri güvenliğini sağlamak için sektör standardı Aktarım Katmanı Güvenliği (TLS 1.2) protokolünü kullanır.

![Data Lake Store şifrelemesi](./media/data-lake-store-security-overview/adls-encryption.png "Data Lake Store şifrelemesi")

Data Lake Store, hesapta depolanan veriler için şifreleme özelliği de sağlar. Verilerinizin şifrelenmesini tercih edebilir ya da şifrelemeyi kabul etmeyebilirsiniz. Şifreleme için kabul ederseniz, Data Lake Store içinde depolanan verileri kalıcı medyada depolanmadan önce şifrelenir. Böyle bir durumda, Data Lake Store otomatik olarak kaydetmeden önce verileri şifreler ve veriye erişen istemci için tamamen saydam olacak şekilde, almadan öncesinde verilerin şifresini çözer. İstemci tarafında veri şifreleme/şifre çözme için gereken kod değişikliği vardır.

Anahtar Yönetimi için Data Lake Store, Data Lake Store içinde depolanan tüm verilerin şifresini çözmek için gerekli olan, ana şifreleme anahtarlarının (Mek'ler) yönetimi için iki mod sağlar. Data Lake Store, Mek'leri yönetmek veya Azure anahtar kasası hesabınızı kullanarak Mek'ler sahipliğini tutmayı seçin ya da sağlayabilirsiniz. Anahtar Yönetimi modunda, bir Data Lake Store hesabı oluşturma sırasında belirtin. Şifreleme tabanlı yapılandırmanın nasıl sağlanacağı üzerine daha fazla bilgi edinmek için bkz. [Azure Portal'ı kullanarak Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md).

## <a name="activity-and-diagnostic-logs"></a>Etkinlik ve tanılama günlükleri
Etkinlik veya hesap yönetimi ile ilgili etkinlikler ve verilerle ilgili etkinlikler için mi aradığınız bağlı olarak tanılama günlükleri, kullanabilirsiniz.

* Hesap Yönetimi ile ilgili etkinlikleri, Azure Resource Manager API'lerini kullanın ve Azure portalda etkinlik günlükleri çıkarılır.
* Verilerle ilgili etkinlikler, WebHDFS REST API'lerini kullanma ve Azure portalında, tanılama günlükleri çıkarılır.

### <a name="activity-log"></a>Etkinlik günlüğü
Yönetmeliklerine uyum sağlamak için bir kuruluşun belirli olaylar halinde sorgulaması gerekiyorsa hesap yönetimi etkinlikleri, yeterli denetim izleri gerektirebilir. Data Lake Store yerleşik izleme ve tüm hesap yönetimi etkinliklerini kaydeder.

Hesap Yönetimi denetim kayıtları, görüntülemek ve günlüğe kaydetmek istediğiniz sütunları seçin. Ayrıca, Azure depolama etkinlik günlüklerini dışarı aktarabilirsiniz.

![Etkinlik günlüğü](./media/data-lake-store-security-overview/activity-logs.png "etkinlik günlüğü")

Etkinlik günlükleri ile çalışma hakkında daha fazla bilgi için bkz. [kaynaklara uygulanan eylemleri denetlemek için etkinlik günlüklerini görüntüleme](../azure-resource-manager/resource-group-audit.md).

### <a name="diagnostics-logs"></a>Tanılama günlükleri
Veri erişim denetim ve tanılama Azure portalında günlük kaydını etkinleştirmek ve bir Azure Blob Depolama hesabı, bir olay hub'ı veya Log Analytics, günlükleri gönderin.

![Tanılama günlükleri](./media/data-lake-store-security-overview/diagnostic-logs.png "tanılama günlükleri")

Azure Data Lake Store ile tanılama günlükleri ile çalışma hakkında daha fazla bilgi için bkz. [Data Lake Store tanılama günlüklerine erişme](data-lake-store-diagnostic-logs.md).

## <a name="summary"></a>Özet
Kurumsal müşteriler, güvenli ve kullanımı kolay bir veri analizi bulut platformunun isteğe bağlı. Azure Data Lake Store, kimlik yönetimi ve Azure Active Directory Tümleştirmesi, ACL tabanlı yetkilendirme, ağ yalıtımı, aktarım ve bekleme sırasında veri şifrelemesi ile kimlik doğrulaması aracılığıyla bu gereksinimleri karşılamasını yardımcı olmak için tasarlanmıştır ve denetim.

Data Lake Store yenilikleri görmek istiyorsanız, bize geri bildirim gönder [Data Lake Store UserVoice forumumuzu](https://feedback.azure.com/forums/327234-data-lake).

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Data Lake Store'a Genel Bakış](data-lake-store-overview.md)
* [Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md)
* [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)

