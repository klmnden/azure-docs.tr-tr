---
title: "Data Lake Store'da güvenliğine genel bakış | Microsoft Docs"
description: "Azure Data Lake Store daha güvenli bir büyük veri deposu nasıl özellikleri anlama"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ebd5b2ac-c5cc-46d4-9cfd-1a1ee70024c2
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: d65341ae79a8894d054503e0b0807dee3e4cca8c
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="security-in-azure-data-lake-store"></a>Azure Data Lake Store'da güvenlik
Çoğu kurum, büyük veri analizi için akıllı kararlar almanıza yardımcı olmak iş öngörüleri avantajlarından sürüyor. Bir kuruluşun farklı kullanıcılar artan sayıda ile karmaşık ve düzenlenen bir ortam olabilir. Kritik iş verileri doğru bireysel kullanıcılara verilen erişim düzeyini ile daha güvenli bir şekilde saklandığından emin olmak bir kuruluş için önemlidir. Azure Data Lake Store, bu güvenlik gereksinimlerini karşılamak amacıyla tasarlanmıştır. Bu makalede, Data Lake Store, güvenlik özellikleri hakkında bilgi de dahil olmak üzere:

* Kimlik Doğrulaması
* Yetkilendirme
* Ağ yalıtımı
* Veri koruma
* Denetim

## <a name="authentication-and-identity-management"></a>Kimlik doğrulama ve Kimlik Yönetimi
Kimlik doğrulama kullanıcı Data Lake Store'a bağlanan herhangi bir hizmeti veya Data Lake Store ile etkileşim zaman, bir kullanıcının kimliği doğrulanır işlemidir. Kimlik Yönetimi ve kimlik doğrulaması için Data Lake Store kullanan [Azure Active Directory](../active-directory/active-directory-whatis.md), kullanıcılar ve gruplar yönetimini kolaylaştıran bir kapsamlı kimlik ve erişim yönetimi bulut çözümü.

Her Azure aboneliğinin Azure Active Directory örneği ile ilişkili olabilir. Yalnızca kullanıcılar ve Azure Active Directory hizmetinizi tanımlı hizmet kimlikleri, Data Lake Store hesabı Azure portal, komut satırı araçlarını kullanarak erişebilir veya Azure Data Lake kullanarak, kuruluşunuz istemci uygulamaları aracılığıyla oluşturur SDK deposu. Bir merkezi erişim denetimi mekanizması Azure Active Directory'yi kullanarak anahtar avantajları şunlardır:

* Basitleştirilmiş kimlik yaşam döngüsü yönetimi. Bir kullanıcı veya hizmet (bir hizmet asıl kimliği) kimliğini hızlı bir şekilde oluşturulur ve yalnızca silme veya hesabın dizinde devre dışı bırakarak hızlı bir şekilde iptal edildi.
* Çok öğeli kimlik doğrulama. [Çok faktörlü kimlik doğrulaması](../multi-factor-authentication/multi-factor-authentication.md) kullanıcı oturum açmaları ve işlemleri için ek bir güvenlik katmanı sağlar.
* OAuth veya Openıd gibi standart bir açık protokolü aracılığıyla herhangi bir istemciden kimlik doğrulaması.
* Kurumsal Dizin Hizmetleri ve bulut kimlik sağlayıcıları ile Federasyon.

## <a name="authorization-and-access-control"></a>Yetkilendirme ve erişim denetimi
Azure Data Lake Store erişebilmesi için Azure Active Directory kullanıcı kimlik doğrulaması yaptıktan sonra Data Lake Store izinlerini erişimi yetkilendirme kontrol eder. Data Lake Store hesabı ve veri ilgili etkinlikler için yetkilendirme aşağıdaki şekilde ayırır:

* [Rol tabanlı erişim denetimi](../active-directory/role-based-access-control-what-is.md) (RBAC) hesap yönetimi için Azure tarafından sağlanan
* POSIX deposundaki verileri erişmek için ACL

### <a name="rbac-for-account-management"></a>Hesap yönetimi için RBAC
Dört temel roller Data Lake Store için varsayılan olarak tanımlanır. Rolleri Azure portalı, PowerShell cmdlet'leri ve REST API'leri aracılığıyla Data Lake Store hesabındaki farklı işlemler izin verir. Sahibi ve katkıda bulunan rollerinin yönetim işlevleri, çeşitli hesabında gerçekleştirebilirsiniz. Yalnızca verilerle etkileşimli kullanıcılar okuyucu rolüne atayabilirsiniz.

![RBAC rolleri](./media/data-lake-store-security-overview/rbac-roles.png "RBAC rolleri")

Hesap yönetimi için rolleri atanmış rağmen bazı roller verilere erişim etkileyeceğini unutmayın. Bir kullanıcının dosya sisteminde gerçekleştirebileceği işlemleri erişimi denetlemek için ACL kullanmanız gerekir. Aşağıdaki tabloda, yönetim haklarını ve varsayılan rolleri için veri erişim hakları özetini gösterir.

| Roller | Yönetim hakları | Veri erişim hakları | Açıklama |
| --- | --- | --- | --- |
| Atanmış bir role yok |Hiçbiri |ACL ile yönetilen |Kullanıcı, Azure portalında veya Azure PowerShell cmdlet'leri, Data Lake Store göz atmak için kullanamazsınız. Kullanıcı yalnızca komut satırı araçlarını kullanabilirsiniz. |
| Sahip |Tümü |Tümü |Süper kullanıcı sahibi rolüdür. Bu rolü her şeyi yönetebilir ve veri tam erişimi vardır. |
| Okuyucu |Salt okunur |ACL ile yönetilen |Okuyucu rolüne hangi role atanmış kullanıcı gibi hesap yönetimi ile ilgili her şeyi görüntüleyebilir. Okuyucu rolüne değişiklik yapamazsınız. |
| Katılımcı |Tüm ekleme ve kaldırma rolleri dışında |ACL ile yönetilen |Katkıda bulunan rolü dağıtımları ve oluşturma ve Uyarıları yönetme gibi bir hesap, bazı yönlerini yönetebilirsiniz. Katkıda bulunan rolü ekleme veya rollerini kaldırın. |
| Kullanıcı Erişimi Yöneticisi |Rol Ekleme ve kaldırma |ACL ile yönetilen |Kullanıcı erişimi yöneticisi rolü hesaplarına kullanıcı erişimini yönetebilirsiniz. |

Yönergeler için bkz: [Data Lake Store hesapları için kullanıcıların veya güvenlik gruplarının atamak](data-lake-store-secure-data.md#assign-users-or-security-groups-to-azure-data-lake-store-accounts).

### <a name="using-acls-for-operations-on-file-systems"></a>ACL'leri kullanarak dosya sistemi işlemleri
Data Lake Store olan hiyerarşik dosya sistemi gibi Hadoop dağıtılmış dosya sistemi (HDFS) ve onu destekleyen [POSIX ACL'leri](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Okuma (r) denetimleri, yazma (w) ve yürütme (x) kaynaklara sahip rolünü, sahipleri grup ve diğer kullanıcılar ve gruplar için izinleri. Data Lake Store'da, kök klasör, alt klasörler ve dosyaları tek tek ACL'ler etkinleştirilebilir. Data Lake Store bağlanımda ACL’lerin nasıl çalıştığı üzerine daha fazla bilgi için bkz. [Data Lake Store’da erişim denetimi](data-lake-store-access-control.md).

ACL'leri kullanarak birden çok kullanıcı için tanımladığınız öneririz [güvenlik grupları](../active-directory/active-directory-groups-create-azure-portal.md). Kullanıcılar bir güvenlik grubuna ekleyin ve bir dosya veya klasör için ACL'leri bu güvenlik grubuna atayın. En fazla dokuz girişleri özel erişim için ekleme ile sınırlı olduğundan, özel erişim sağlamak istediğinizde kullanışlıdır. Daha iyi Azure Active Directory güvenlik gruplarını kullanarak Data Lake Store'da depolanan verilerin güvenliğini sağlama hakkında daha fazla bilgi için bkz: [ACL'ler kullanıcılar veya güvenlik grubu için Azure Data Lake Store dosya sistemi atamak](data-lake-store-secure-data.md#filepermissions).

![Liste standart ve özel erişim](./media/data-lake-store-security-overview/adl.acl.2.png "listesinde standart ve özel erişim")

## <a name="network-isolation"></a>Ağ yalıtımı
Kullanım Data Lake veri deponuza ağ düzeyinde erişimi denetlemeye yardımcı olmak için deposu. Güvenlik duvarları kurmak ve güvenilir istemcileriniz için bir IP adresi aralığı tanımlayabilirsiniz. Bir IP adresi aralığı ile bir IP adresi tanımlı aralığın içinde olan istemciler için Data Lake Store bağlanabilir.

![Güvenlik Duvarı ayarları ve IP erişim](./media/data-lake-store-security-overview/firewall-ip-access.png "Güvenlik Duvarı ayarları ve IP adresi")

## <a name="data-protection"></a>Veri koruma
Azure Data Lake Store yaşam döngüsü boyunca verilerinizi korur. Aktarımdaki verileri için Data Lake Store, ağ üzerinden veri güvenliğini sağlamak için endüstri standardı Aktarım Katmanı Güvenliği (TLS) protokolünü kullanır.

![Data Lake Store'da şifreleme](./media/data-lake-store-security-overview/adls-encryption.png "Data Lake Store'da şifreleme")

Data Lake Store, hesapta depolanan veriler için şifreleme özelliği de sağlar. Verilerinizin şifrelenmesini tercih edebilir ya da şifrelemeyi kabul etmeyebilirsiniz. Şifreleme için kabul ederseniz, Data Lake Store içinde depolanan veriler kalıcı medyada depolamak önce şifrelenir. Böyle bir durumda, Data Lake Store otomatik olarak devam ettirmeden önce verileri şifreler ve verilere erişilirken istemciye tamamen saydam olacak şekilde alma, öncesinde verilerin şifresini çözer. İstemci tarafında veri şifreleme/şifre çözme için gerekli kod değişiklik yoktur.

Anahtar Yönetimi için Data Lake Store'da depolanan verilerin şifresini çözmek için gerekli olan ana şifreleme anahtarlarınızı (MEKs) yönetmek için Data Lake Store iki mod sağlar. Data Lake Store MEKs yönettiğiniz ya da Azure anahtar kasası hesabınızı kullanarak MEKs sahipliğini tutmayı seçin ya da izin verebilirsiniz. Bir Data Lake Store hesabı oluşturma sırasında sırasında anahtar yönetimi modunu belirtin. Şifreleme tabanlı yapılandırmanın nasıl sağlanacağı üzerine daha fazla bilgi edinmek için bkz. [Azure Portal'ı kullanarak Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md).

## <a name="auditing-and-diagnostic-logs"></a>Denetim ve tanılama günlükleri
Yönetim ilgili etkinlikleri veya ilgili verileri etkinlik günlükleri için mi aradığınız bağlı olarak, denetim veya tanılama günlüklerini kullanabilir.

* Yönetim ilgili etkinlikleri Azure portalı denetim günlüklerini çıkmış ve Azure Resource Manager API'leri kullanın.
* Veri ilgili etkinlikleri ve Azure portalı tanılama günlüklerini çıkmış WebHDFS REST API'lerini kullanın.

### <a name="auditing-logs"></a>Denetim günlükleri
Düzenlemelerle uyumlu olması, bir kuruluşun belirli olaylara derinliklerine gerekirse yeterli denetim izleri gerektirebilir. Data Lake Store yerleşik izleme ve denetim sahiptir ve tüm hesap yönetimi etkinlikleri günlüğe kaydeder.

Hesap Yönetimi denetim izleri görüntülemek ve günlüğe kaydetmek istediğiniz sütunları seçin. Ayrıca, Azure depolama birimine denetim günlüklerini dışa aktarabilirsiniz.

![Denetim günlükleri](./media/data-lake-store-security-overview/audit-logs.png "Denetim günlükleri")

### <a name="diagnostic-logs"></a>Tanılama günlükleri
Veri erişimi denetim izleri Azure portalında (tanılama ayarları) olarak ayarlayın ve günlüklerin depolandığı bir Azure Blob Depolama hesabı oluşturun.

![Tanılama günlüklerini](./media/data-lake-store-security-overview/diagnostic-logs.png "tanılama günlükleri")

Tanılama ayarlarını yapılandırdıktan sonra üzerinde günlüklerini görüntüleyebilirsiniz **tanılama günlüklerini** sekmesi.

Tanılama günlükleri Azure Data Lake Store ile çalışma hakkında daha fazla bilgi için bkz: [erişim Data Lake Store için tanılama günlükleri](data-lake-store-diagnostic-logs.md).

## <a name="summary"></a>Özet
Kurumsal müşteriler, güvenli ve kullanımı kolay bir veri analizi bulut platformu talep. Azure Data Lake Store (gelecekte gelen kimlik yönetimi ve Azure Active Directory ile tümleştirme, ACL tabanlı yetkilendirme, ağ yalıtımı, veri şifreleme Aktarımdaki hem de aracılığıyla kimlik doğrulaması aracılığıyla bu gereksinimleri rest adresi yardımcı olmak için tasarlanmıştır ) ve denetim.

Data Lake Store'da yeni özellikler görmek istiyorsanız, bize geri bildirim gönder [Data Lake deposu UserVoice Forumu](https://feedback.azure.com/forums/327234-data-lake).

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Data Lake Store'a Genel Bakış](data-lake-store-overview.md)
* [Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md)
* [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)

